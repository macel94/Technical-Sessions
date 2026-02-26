---
layout: two-cols
transition: slide-left
---

# The incident pattern we inherited

## What actually breaks
- Critical apps go down with no warning
- Logs show: "client secret expired"
- Escalations, manual firefighting, long MTTR
- This stems from good intentions: "secrets should expire every 6 months" 
- **Security debt**: secrets shared via chat or email, no audit trail

::right::

```mermaid {scale: 0.6}
graph TD
  A[App Down] -->|Panic| B(Manual ticket)
  B --> C{Wait for the right team}
  C -->|Manual generation| D[Secret shared via chat/email]
  D -->|Risk + no audit| E[Restore service]
```

---
layout: default
---

# The legacy detection approach

- Someone runs scripts "every now and then" to discover what is expiring...
-   ❌ Reactive, slow, and brittle
-   ❌ No context or audit trail for operations
-   ❌❌❌❌ I'm the one manually doing it, each month, through manually created tickets ❌❌❌❌

<div class="flex items-center justify-center">
  <img src="../images/programmer-move.png" class="max-h-[30vh] object-contain rounded-lg" />
</div>