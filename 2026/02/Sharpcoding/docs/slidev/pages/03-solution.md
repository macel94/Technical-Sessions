---
layout: default
transition: slide-left
---

# The target design

Turn incidents into a **proactive, event-driven** workflow.

<div class="grid grid-cols-2 gap-20">

<div>

### Key components
- **Entra ID + Key Vault**: system of record for secrets
- **Event Grid system topics**: CloudEvents on secret changes
- **Azure Functions**: serverless workflow + integrations
- **Jira + SharePoint**: ticketing and tracking
- **Blob Storage**: immutable event replay and backups

**"I'm wicked smart, and with AI I'll be done in 5 minutes"**
</div>
<div>

```mermaid {scale: 0.5}
graph TD
    KV[Key Vault] -->|CloudEvent: SecretNewVersionCreated| EG[Event Grid]
    EG -->|Trigger| Func[Azure Function]
    Func -->|Create/update| Jira[Jira ticket]
    Func -->|Update record| SP[SharePoint]
    Func -->|Backup payload| Blob[Blob Storage]
```

</div>
</div>

---
layout: two-cols

---

# More problems
- What about resiliency
- What about division of responsibilities and least privilege?
- What about monitoring, alerting, and auditing?
- What about backups and replay for recovery and compliance?
- What about...?

::right::

<div class="flex items-center justify-center h-full">
  <img src="../images/send-help.png" class="max-h-[50vh] object-contain rounded-lg" />
</div>

---
layout: center

---

# Full architecture: close the loop

```mermaid {scale: 0.5}
graph TD
    subgraph "Azure"
        EntraID[Entra ID]
        KV[Key Vault]
        EG[Event Grid - system topic]
        Blob[Blob Storage - event replay + audit]

        subgraph "SecretRenewalFunctions"
            Rotate[SecretRotatorFunction - daily]
            Trigger[OnSecretUpdatedTrigger - Event Grid]
            Sync[SharePointSyncFunction - twice daily]
            Backup[BackupFunction - twice daily]
        end
    end

    subgraph "External Systems"
        Jira[Jira]
        SP[SharePoint]
    end

    Rotate -->|Create credential| EntraID
    Rotate -->|Write secret version| KV
    KV -->|CloudEvent: SecretNewVersionCreated| EG
    EG -->|Trigger| Trigger
    Trigger -->|Persist payload| Blob
    Trigger -->|Run workflow| Jira
    Trigger -->|Update record| SP
    Backup -->|Backup data| Blob
    Sync -->|Reconcile records| SP
```

---
layout: two-cols

---

# Business value

| Model | Time/app/month |
|---|---|
| Manual | ~35 min |
| Automated | ~8 min |

**150 apps → ~67.5 hours saved/month**

::right::

# Why it matters

- Less cross-team coordination and handoffs
- Shared system of record (Jira + SharePoint)
- Time shifts from firefighting to development

<div class="mt-3 text-xs opacity-75">
(numbers are based on VIBES — trust the trend, not the math 🙈)
</div>