---
layout: default
transition: slide-left
---

# Agenda

- The incident pattern and why it repeats
- Design goals: security, auditability, zero-downtime
- Event-driven architecture on Azure
- Business impact: estimated hours saved per month
- Deep dive: Event Grid trigger and rotation
- Operations: SharePoint sync and backups
- IaC + delivery: Bicep, GitHub Actions, Dependabot
- Copilot support for implementation and documentation
- Future improvements: AI-assisted operations
- Takeaways and practical next steps

---
layout: two-cols
transition: slide-left
---

# Why this story
- Secrets expire on weekends and holidays too
- Manual rotation = security gaps and audit pain
- **Downtime** is the most expensive kind of learning

::right::

# What changed
- We gained trust (aka permissions)
- Event Grid + Functions replace manual scripts
- Jira + SharePoint become the system of record
- Backups and replay for forensic confidence
- A reusable blueprint for secret hygiene

---
layout: two-cols
transition: slide-left
---

# The real cost of downtime

## Human impact
- On-call stress and weekend firefighting
- Context switching derails planned work
- Manual effort repeated every single month
- Shared secrets erode hygiene and team trust

<div class="flex items-center justify-center">
  <img src="../images/fix-prod-fast.png" class="max-h-[30vh] object-contain rounded-lg" />
</div>

::right::

<br/>
<br/>

## Business impact

We manage **150 app registrations**.  
One failing secret on a critical integration:

| | |
|---|---|
| Client annual revenue | €1,000,000,000 |
| Working hours/day | 12 |
| **Cost per working hour** | **~€250,000** |

> Even a short outage on the wrong app  
> can cost **hundreds of thousands of euros**.
