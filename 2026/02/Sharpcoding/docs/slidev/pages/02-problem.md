---
layout: default
transition: slide-left
---

# The incident pattern we inherited

<div class="grid grid-cols-2 gap-20">

<div>

### What actually breaks
- Critical apps go down with no warning
- Logs show: "client secret expired"
- Escalations, manual firefighting, long MTTR
- **Security debt**: secrets shared out-of-band, weak audit trail

</div>

<div>

```mermaid {scale: 0.6}
graph TD
  A[App Down] -->|Panic| B(Manual ticket)
  B --> C{Wait for the right team}
  C -->|Manual generation| D[Secret shared out-of-band]
  D -->|Risk + no audit| E[Restore service]
```

</div>
</div>

---
layout: default
class: text-sm
---

# The legacy detection approach

- Someone runs scripts "every now and then" to discover what is expiring...
-   ❌ Reactive, slow, and brittle
-   ❌ No context or audit trail for operations
-   ❌❌❌❌ I'm the one manually doing it, each month, through manually created tickets ❌❌❌❌

<div class="flex items-center justify-center">
  <img src="../images/programmer-move.png" class="max-h-[30vh] object-contain rounded-lg" />
</div>