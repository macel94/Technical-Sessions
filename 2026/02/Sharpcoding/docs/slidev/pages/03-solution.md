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

# Business value: estimated monthly hours saved

### For 150 app registrations

- Manual model (monitoring + coordination + updates): about 35 min per app per month
- Automated model (exceptions + approvals + review): about 8 min per app per month
- Estimated saving: about 27 min per app per month

### Monthly impact

- 150 x 27 min = 4,050 min per month
- About 67.5 hours saved every month
- At 200 apps, about 90 hours saved per month

::right::

# Why this matters across teams

- Fewer cross-team status pings and handoffs
- Less waiting between Wuerth teams and client teams
- Faster renewals with a shared system of record (Jira + SharePoint)
- Time shifts from firefighting to hardening and onboarding

<div class="mt-3 text-xs opacity-75">
Estimate based on... ai

I just made it up, since sometimes it was 4 days of actual work per month, sometimes just 2 hours

Please don't tell everyone that I pulled these numbers out of thin air 🙈
</div>