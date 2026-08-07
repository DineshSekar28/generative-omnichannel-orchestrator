# Generative Omnichannel AI Orchestrator

**Production-Grade Enterprise Architecture** — an interactive, click-to-explore reference architecture for a multi-channel generative AI platform spanning IVR, live agent assist, and design-time workflow authoring.

🔗 **Live demo:** https://dineshsekar28.github.io/generative-omnichannel-orchestrator/

---

## What this is

This repo hosts a self-contained, static architecture portal — no build step, no backend, no dependencies. It's built around one central artifact: an **interactive flow map** that lets you trace how a request actually moves through the system, filter by architectural plane, and drill into the real diagram for any individual service.

It documents a platform of:

- **8 architectural planes**
- **22 microservices**
- **3 shared data stores** (MongoDB, Neo4j, PostgreSQL + pgvector)
- **3 end-to-end flows**: Command Studio (design-time), IVR (telephony), Agent Assist (live agent copilot)

## Why an interactive map instead of a static diagram

A single static diagram can show *what exists*. It can't show *how a request actually flows*, which services are shared across multiple channels, or let you go from "here's the system" to "here's exactly what this one service does" without leaving the page. This portal does all three:

- **Trace a flow** — click any box once to highlight everything that specific request path touches, end to end. Shared services (like the orchestration layer or the ML engine, which serve both IVR and Agent Assist) light up for every flow that uses them.
- **Browse by plane** — click any of the 8 plane tiles to highlight every service in that plane and open its plane-level architecture overview.
- **Inspect any service** — double-click (or use the ⤢ button on) any box to open that service's own dedicated diagram.

## Repo structure

```
generative-omnichannel-orchestrator/
├── index.html                          # Live landing page — architecture portal
├── master-architecture-interactive.html # The interactive flow map itself
├── legacy-index.html                   # Previous landing page (embedded the flow map via iframe)
├── README.md
└── diagrams/
    ├── planes/                         # 8 plane-level overview diagrams
    │   ├── plane-01-design-time-control.png
    │   ├── plane-02-realtime-streaming.png
    │   ├── plane-03-orchestration-gateways.png
    │   ├── plane-04-mcp-providers.png
    │   ├── plane-05-stateful-intelligence.png
    │   ├── plane-06-observability-governance.png
    │   ├── plane-07-evaluation-mlops.png
    │   └── plane-08-maintenance-lifecycle.png
    └── services/                       # Per-service diagrams, one folder per plane
        ├── plane-01/  (2 services)
        ├── plane-02/  (3 services)
        ├── plane-03/  (2 services)
        ├── plane-04/  (3 services)
        ├── plane-05/  (2 services)
        ├── plane-06/  (3 services)
        ├── plane-07/  (3 services)
        └── plane-08/  (2 services)
```

`index.html` is the live architecture portal, deployed at the repo root alongside the `diagrams/` folder exactly as structured above. All image references are relative paths, so nothing needs reconfiguring beyond keeping that folder layout intact. `master-architecture-interactive.html` remains available standalone as the interactive flow map; `legacy-index.html` is the previous landing page that embedded it via iframe.

## The 8 planes

| Plane | Focus |
|---|---|
| Design-Time Control | Workflow & agent template authoring (Command Studio, KYC Data Studio) |
| Streaming & Frontend | Real-time agent copilot UI and WebSocket streaming |
| Orchestration & Gateways | Request routing, decision mapping, and GenAI workflow execution |
| MCP Providers | Model Context Protocol tool execution and context retrieval |
| Stateful Intelligence | Conversational AI engine and model inference serving |
| Observability & Governance | Monitoring, drift detection, and PII redaction |
| Evaluation & MLOps | Automated evaluation, synthetic data generation, telemetry pipelines |
| Maintenance & Lifecycle | Graph hygiene, background sync, and CI/CD quality gates |

## Design notes

- **No internal service identifiers are shown in the UI.** Every service displays a human-readable name and description; raw `svc-*` codebase identifiers exist only in the underlying data model, not on screen.
- **Every node has both a name and a description** — nothing is a bare, unexplained label.
- **Box background color indicates plane membership** at a glance; the left-edge color bar indicates flow membership. The two are independent and can be read together.
- **Edge routing avoids known collisions** where reasonably possible — same-column edges that would otherwise cut through an intervening box are rerouted through the empty space between columns. Some multi-column edges may still visually cross a box; this was a deliberate trade-off after an earlier, more aggressive routing system made the diagram harder to read overall.

## Updating a diagram

Diagram images are matched by exact filename. To update or add one:

1. Generate/export the image as a `.png`
2. Save it to the matching path under `diagrams/services/plane-0X/` or `diagrams/planes/`, using the **exact filename** already referenced in `master-architecture-interactive.html` (paths are case-sensitive)
3. Commit and push — no code changes needed if the filename matches what's already wired up

If you're adding a service that doesn't exist yet in the map, you'll need to add its entry to the `nodes` array in `master-architecture-interactive.html` (position, plane, flow membership, pseudo name, description, and diagram path).

## Deployment

This is a static site — any static host works. For GitHub Pages specifically:

```bash
git add .
git commit -m "Update architecture portal"
git push origin main
```

Then confirm the deployment under the repo's **Settings → Pages** tab, or the **Actions** tab if using a Pages workflow.

## Tech stack

Vanilla HTML, CSS, and JavaScript. No frameworks, no build tooling, no package manager. Everything renders directly in the browser.

---

*Last updated: August 2026*