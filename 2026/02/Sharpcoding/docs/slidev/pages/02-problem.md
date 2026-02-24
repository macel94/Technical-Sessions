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

Someone runs scripts "every now and then" to discover what is expiring...

<div class="text-xs max-h-56 overflow-auto">

TODO insert image meme here

</div>

<div class="mt-4 text-red-500 font-bold">
  ❌ Reactive, slow, and brittle
</div>
