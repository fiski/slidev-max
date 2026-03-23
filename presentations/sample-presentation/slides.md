---
title: Q3 Product Roadmap Review
info: |
  A cross-functional overview of Q3 objectives, delivery milestones,
  infrastructure decisions, and team retrospective.
class: text-center
highlighter: shiki
lineNumbers: false
drawings:
  persist: false
transition: slide-left
mdc: true
---

# Q3 Product Roadmap Review

### Engineering · Design · Product

<div class="text-gray-400 mt-4 text-sm">
  Presented by the Platform Team — September 2025
</div>

<!--
Welcome everyone. Today we'll walk through what we set out to do in Q3,
what we delivered, and what's coming up in Q4.
-->

---

# What We Set Out to Do

Key objectives entering Q3:

<v-clicks>

- Migrate the data pipeline to the new event-streaming architecture
- Reduce p95 API latency below 200 ms across all regions
- Ship the redesigned onboarding flow to 100% of new users
- Establish a quarterly review cadence across all squads
- Deprecate three legacy endpoints before the hard cutoff date

</v-clicks>

<!--
These five objectives were agreed upon in the Q2 retro and signed off by all team leads.
We'll measure success against each one.
-->

---

<div class="w-full h-64 bg-gray-800 rounded-lg flex items-center justify-center text-gray-500 text-sm mt-4">
  [ Roadmap overview — add image to public/assets/roadmap-overview.png ]
</div>

<!--
This is an overview of the full Q3 roadmap as it looked on day 1.
Each swim lane represents a squad.
-->

---
layout: two-cols
---

<template v-slot:default>

## Architecture Before

<div class="w-full h-48 bg-gray-800 rounded-lg flex items-center justify-center text-gray-500 text-sm">
  [ Architecture diagram — add image to public/assets/arch-before.png ]
</div>

</template>
<template v-slot:right>

## What Changed

- Monolithic job runner replaced with distributed workers
- Shared PostgreSQL cluster split per domain
- All internal traffic moved to gRPC
- Secrets management migrated to Vault

</template>

<!--
The left shows the legacy monolith. The four bullet points on the right are
the concrete changes we shipped in the first six weeks.
-->

---
layout: two-cols
---

<template v-slot:default>

## Team Allocation

We restructured squads at the start of Q3 to better align with product areas.

- **Platform** — 4 engineers, 1 EM
- **Growth** — 3 engineers, 1 designer
- **Core API** — 5 engineers, 1 EM
- **Data** — 2 engineers, 1 data scientist

</template>
<template v-slot:right>

<div class="w-full h-48 bg-gray-800 rounded-lg flex items-center justify-center text-gray-500 text-sm">
  [ Team structure diagram — add image to public/assets/team-structure.png ]
</div>

</template>

<!--
Headcount stayed the same but squad boundaries shifted significantly.
The Growth squad gained a dedicated designer for the first time.
-->

---
layout: two-cols
---

<template v-slot:default>

## Shipped on Time

<v-clicks>

- Event streaming backbone
- Onboarding redesign (v1)
- Auth service consolidation
- Dashboard performance pass

</v-clicks>

</template>
<template v-slot:right>

## Slipped to Q4

<v-clicks>

- Self-serve billing portal
- Multi-region failover
- SDK v3 public release
- Admin audit log export

</v-clicks>

</template>

<!--
Four out of eight major initiatives shipped on time. The slips were
mostly caused by cross-team dependency delays, not scope creep.
-->

---

# Q3 Outcomes at a Glance

<div class="grid grid-cols-2 gap-6 mt-6">

<div class="p-6 rounded-lg bg-blue-900/30 border-l-4 border-blue-500">
  <div class="text-2xl font-bold text-blue-300">↓ 38%</div>
  <div class="text-sm text-gray-300 mt-1">Reduction in p95 API latency</div>
</div>

<div class="p-6 rounded-lg bg-green-900/30 border-l-4 border-green-500">
  <div class="text-2xl font-bold text-green-300">12 / 15</div>
  <div class="text-sm text-gray-300 mt-1">Planned features delivered</div>
</div>

<div class="p-6 rounded-lg bg-yellow-900/30 border-l-4 border-yellow-500">
  <div class="text-2xl font-bold text-yellow-300">99.94%</div>
  <div class="text-sm text-gray-300 mt-1">Uptime across all services</div>
</div>

<div class="p-6 rounded-lg bg-purple-900/30 border-l-4 border-purple-500">
  <div class="text-2xl font-bold text-purple-300">+22 NPS</div>
  <div class="text-sm text-gray-300 mt-1">Developer satisfaction score</div>
</div>

</div>

<!--
Latency target was 200 ms — we hit 185 ms p95. The NPS jump of 22 points
was the most surprising result; the new onboarding flow drove most of that.
-->

---

# Retrospective Notes

Per-squad retrospective collected after the quarter closed. Available in the exported PDF for async review.

- **Platform:** appreciated the clearer RFC process; wants shorter on-call rotations
- **Growth:** onboarding iteration cycle too slow; needs a dedicated staging environment
- **Core API:** strong delivery quarter; blocked occasionally by cross-team dependencies
- **Data:** undersized for scope; Q4 headcount request submitted and approved

<!--
This slide is intentionally not presented during the live session.
It exists for stakeholders reviewing the deck asynchronously.
-->

---
layout: default
---

# System Interaction Model

How requests flow through the updated stack:

<div class="grid grid-cols-2 gap-8 mt-4">

<div>

**Request path**

```mermaid
sequenceDiagram
    Client->>API Gateway: HTTPS request
    API Gateway->>Auth Service: Validate token
    Auth Service-->>API Gateway: 200 OK
    API Gateway->>Core API: Forward request
    Core API->>Event Bus: Publish event
    Core API-->>Client: Response
```

</div>

<div>

**Deployment pipeline**

```mermaid
graph TD
    A[Git push] --> B[CI: lint + test]
    B --> C{All checks pass?}
    C -->|Yes| D[Build Docker image]
    C -->|No| E[Fail + notify]
    D --> F[Push to registry]
    F --> G[Deploy to staging]
    G --> H[Smoke tests]
    H --> I[Deploy to production]
```

</div>

</div>

<!--
Left diagram shows the happy-path request flow. Right diagram is the
deployment pipeline — smoke tests gate every production deploy.
-->

---

# Q4 Focus Areas

<div class="grid grid-cols-3 gap-6 mt-6">

<div>

### Reliability

- Multi-region active-active
- Chaos engineering programme
- Automated rollback triggers

</div>

<div>

### Product

- Self-serve billing portal
- SDK v3 GA release
- Admin audit log export
- In-app notification centre

</div>

<div>

### Developer Experience

- Unified local dev setup
- Internal API documentation portal
- Dependency upgrade automation

</div>

</div>

<!--
Reliability is the top priority given the multi-region slip from Q3.
DX investments are small but high-impact for team velocity.
-->

---
class: text-center
---

# Thank You

Questions and discussion welcome.

<div class="mt-8 text-gray-400 text-sm">
  Slides available at <code>github.com/your-org/slidev-max</code>
</div>

<!--
Leave 10 minutes for questions. The full deck including retrospective
notes will be shared in the team channel after this session.
-->
