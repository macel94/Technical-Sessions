---
layout: default
class: text-sm
transition: slide-left
---

# Infrastructure and delivery

<div class="grid grid-cols-2 gap-6">

<div>

### IaC with Bicep

- Event Grid **system topic** sourced from Key Vault
- CloudEvents schema, retry policy, filtered event types
- Subscription-scope `main.subscription.bicep` for per-client rollouts
- Event types include `SecretNewVersionCreated`, `SecretNearExpiry`, `SecretExpired`
- Delivery tuned for reliability: 3 attempts, 24h TTL, 1 event per batch

```bicep {1-12}
param includedEventTypes array = [
	'Microsoft.KeyVault.SecretNewVersionCreated'
	'Microsoft.KeyVault.SecretNearExpiry'
	'Microsoft.KeyVault.SecretExpired'
]

properties: {
	eventDeliverySchema: 'CloudEventSchemaV1_0'
	retryPolicy: {
		maxDeliveryAttempts: 3
		eventTimeToLiveInMinutes: 1440
	}
}
```

</div>

<div>

### CI/CD (GitHub Actions)

- Restore → build → test → publish → zip
- Deploy via `Azure/functions-action`
- Onboarding workflow generates parameter files and PRs per client
- What-if and deploy stages for Event Grid wiring

```yaml {1-10}
- name: Build SecretRenewalFunctions
	run: dotnet build --configuration Release --no-restore

- name: Test
	run: dotnet test --configuration Release --no-build --logger "trx;LogFileName=test_results.trx"

- name: Publish SecretRenewalFunctions
	run: dotnet publish src/SecretRenewalFunctions/SecretRenewalFunctions.csproj --configuration Release --no-build
```

</div>

</div>

---
layout: two-cols
---

# GitHub as force multiplier

### Delivery and platform workflows

- GitHub Actions standardizes build, test, publish, and deployment gates
- Onboarding workflows generate repeatable infra changes per client
- Pull requests become a shared operating model across teams

### Dependency hygiene

- Dependabot keeps NuGet and action versions current
- Security and maintenance updates are visible, reviewable, and trackable

::right::

# GitHub Copilot impact

- Speeds up implementation drafts and refactors
- Helps produce clearer docs and runbooks while coding
- Accelerates this Slidev presentation creation and iteration
- Frees engineering time for architecture and reliability decisions
