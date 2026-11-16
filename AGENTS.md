# Canopy (`canopy-homelab`) — Organization & Planning Guidelines

This repository (`canopy-homelab/.profile`, mirrored to `canopy-homelab/.github`) is the planning and architectural hub for the **Canopy** organization.

---

## Planning & Gitea Synchronization Contract

All cross-repository delivery across `canopy-homelab/nixos-core`, `canopy-homelab/ddns-monitor`, and `canopy-homelab/.profile` is coordinated via the organization-level Kanban board.

- **Authoritative Roadmap & Checklist**: [`ROADMAP.md`](ROADMAP.md) and [`todo.txt`](todo.txt) in `canopy-homelab/.profile` (with `docs/plans/` holding detailed milestone plans).
- **Organization Kanban Board**: **Canopy Platform** (`https://gitea.roadtotech.me/canopy-homelab/-/projects/7`, Project ID: `7`).
- **Canonical 5-Column Pipeline (Deep Signal)**:
  - `Backlog` -> `column_id: 30` (`#475569`, default)
  - `Ready` -> `column_id: 31` (`#1d76db`)
  - `Work in Progress` -> `column_id: 32` (`#d97706`)
  - `Review` -> `column_id: 33` (`#6d28d9`)
  - `Completed` -> `column_id: 34` (`#15803d`)
- **Board Policy**: Child issues in `canopy-homelab/nixos-core` and `canopy-homelab/ddns-monitor` attach directly to Org Project `7` (`gt issue create ... -p 7` or `gt issue edit <N> -R <owner/repo> --add-project 7`).
- **Mirror Policy**: Gitea (`gitea.roadtotech.me`) is the primary forge; GitHub (`github.com/canopy-homelab/.github`) is an automated `sync_on_commit` push mirror.
- **Re-sync Trigger**: Re-run `/gitea-plan` before decomposing the next rolling milestone (P5/P6/P7) or upon scope shifts / Readiness Gates.
