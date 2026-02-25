---
layout: two-cols
class: text-sm
transition: slide-left
---

# Who observes the observer?

Every function, workflow, and integration reports to **Application Insights** — so nothing fails silently.

### Structured logging

- All functions use `ILogger<T>` wired to Application Insights via the SDK
- Correlation IDs propagate through Event Grid → Function chains
- Tracing... just works?

### Automatic alerting

- Simple detection: exceptions > 0 = email with critical alert to team...
- ...30 days to fix though

::right::

<div class="flex items-center justify-center h-full">
  <img src="../images/vibe-coders.png" class="max-h-[30vh] object-contain rounded-lg" />
</div>

---
layout: two-cols
transition: slide-left
---

# Takeaways

- ✅ **Zero downtime**: proactive secret rotation
- 🔒 **Security-first operations**: no secret sharing out-of-band
- ⚡ **Serverless integration**: low ops overhead, scalable by default
- 🧾 **Auditable by design**: event payloads and backups for replay
- 🧘 **Less toil**: fewer late-night surprises from expiry-driven outages

::right::

<div class="flex items-center justify-center h-full">
  <img src="../images/me-smart.png" class="max-h-[30vh] object-contain rounded-lg" />
</div>

---
layout: two-cols
---

# Future improvements

- Add self-service dashboards for teams and clients with explainable status
- Auto-generate owner-facing summaries and action checklists per app, possibly using AI (clustering)
- Expand policy-as-code checks before deployment and before rotation
- Improve app reg provisioning and deprovisioning workflows
- Self-service secret rotation trigger for teams and clients, with explainable status and audit trails
- Self-service app registration creation or onboarding for existing apps
- More tests???

::right::

<div class="flex items-center justify-center h-full">
  <img src="../images/test-in-prod-memedrivendotdev.png" alt="I don't always test my code, but when I do, I do it in production" class="max-h-[30vh] object-contain rounded-lg" />
</div>