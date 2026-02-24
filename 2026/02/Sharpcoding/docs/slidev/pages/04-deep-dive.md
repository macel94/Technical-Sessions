---
layout: default
class: text-sm
transition: slide-left
---

# Deep Dive: Event Grid → Function (CloudEvents)

What the trigger does (by design):

- Logs the CloudEvent envelope for traceability
- Persists the raw payload to Blob Storage (`event-grid-payloads`) for replay/audit
- Renames the blob with the secret name for fast filtering
- Validates `SecretNewVersionCreated` and ignores non-AppId secret names
- Runs the shared workflow and **rethrows on failure** to enable Event Grid retries

```csharp
[Function(nameof(OnSecretUpdatedTrigger))]
public async Task Run([EventGridTrigger] CloudEvent cloudEvent)
{
	var containerClient = _blobServiceClient.GetBlobContainerClient("event-grid-payloads");
	await containerClient.CreateIfNotExistsAsync();
	var blobClient = containerClient.GetBlobClient($"{cloudEvent.Id}.json");
	await blobClient.UploadAsync(new BinaryData(JsonSerializer.Serialize(cloudEvent)), overwrite: true);
}
```

---
layout: default
class: text-sm
---

# Deep Dive: Proactive rotation (timer)

The rotator is intentionally conservative:

- Daily schedule (6 AM) and a next-month expiry threshold
- Skips apps with a newer credential (avoid double-rotation)
- Validates Key Vault references and URI structure before writing
- Creates a 6-month credential, writes to Key Vault, then relies on Event Grid

```csharp
[Function(nameof(SecretRotatorFunction))]
public async Task Run([TimerTrigger("0 0 6 * * *")] TimerInfo myTimer)
{
	var records = (await _sharePointService.GetAllRenewalRecordsAsync())
		.OrderBy(r => r.NextExpiryDateTime)
		.ToList();
	var thresholdDate = DateTime.UtcNow.AddMonths(1).AddDays(1).Date;
	var expiringRecords = records.Where(r =>
		r.NextExpiryDateTime != null &&
		r.NextExpiryDateTime <= thresholdDate &&
		r.NextExpiryDateTime > DateTime.UtcNow.AddMonths(-1)).ToList();
}
```

---
layout: default
class: text-sm
---

# Deep Dive: Automatic validation (what used to be manual)

The twice-daily sync validates each SharePoint row and repairs metadata:

- Normalize Key Vault references and flag invalid formats
- Ensure the App ID still exists in Entra ID
- Verify owners in Jira and Entra; enforce account ownership patterns
- Confirm Key Vault secret exists and assign Secrets User when needed
- Validate Jira ticket URL and creation window vs. expiry dates
