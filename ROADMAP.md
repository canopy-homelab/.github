# 🗺️ Canopy Ecosystem Engineering Roadmap

This document is the authoritative **Canopy** (`canopy-homelab`) ecosystem roadmap coordinating [`canopy-homelab/nixos-core`](https://gitea.roadtotech.me/canopy-homelab/nixos-core), [`canopy-homelab/ddns-monitor`](https://gitea.roadtotech.me/canopy-homelab/ddns-monitor), and workload catalogs (`~/Workloads`).

---

## 🎯 Architectural Invariants

1. **Git is the Authoritative Source of Deployment State**: All workload configurations are versioned in Git (`~/Workloads`).
2. **One Logical API Across Dual Transports (`DEC-01`)**: `homelab-appctl.service` exposes `POST /v1/operations` + `GET /v1/health` identically over `/run/homelab/appctl.sock` and `127.0.0.1:9001`.
3. **Host Access Does Not Confer Mutation Authority (`DEC-02`)**: Mutations require an explicit Bearer token; tokenless UDS falls back strictly to `homelab-viewer`.
4. **Dedicated Service Identity & Fail-Closed Audit Guard (`DEC-03`)**: `homelab-appctl` runs isolated with mode `0600` append-only `/var/log/homelab/appctl/audit.jsonl`.
5. **Unified Execution, Pre-Flight & Pure-Python Environment (`DEC-04`, `DEC-05`)**: Pydantic v2 validation, Compose AST security checks, and inert pure-Python `.env` assembly.
6. **External AI Boundary**: AI agents act externally via client-side MCP tools (P6); the appliance runs zero autonomous agent loops.
7. **Clean State Root**: Mutable runtime data resides strictly under `~/var/lib/homelab/<app>/`, isolated from Git repositories.

---

## 📊 Milestone Overview

| Milestone | Title | Repo Scope | Status | Summary |
| :--- | :--- | :--- | :---: | :--- |
| **P0** | Baseline Consolidation | `nixos-core` | ✅ **Completed** | Reconciled repo baseline, ADRs, v2 metadata scaffolding. |
| **P1** | Mail Topology Realignment | `nixos-core` | ✅ **Completed** | Re-homed Stalwart (`smtp.`) and isolated SnappyMail. |
| **P2** | GitOps Hardening | `nixos-core` | ✅ **Completed** | `deployments.yaml` registry, Compose AST validator, systemd sandboxing. |
| **P3** | Manifest Contract | `nixos-core` | ✅ **Completed** | Fail-closed Pydantic v2 `core_manifest.py`, two-axis provenance engine. |
| **P4** | Control Plane Daemon & Host Cutover | `nixos-core` | ✅ **Completed** | `homelab-appctl.service`, 3-role RBAC, audit guard, dual-account `operator` cutover, Gitea state isolation. |
| **P4.5** | **Workloads Contract & Agent Readiness** | `nixos-core`, `.profile` | 🎯 **Active Horizon** | Decouple `deployments.yaml` into `~/Workloads/deployments.yaml`, provision read-only `agents` SSH user, and author `.agents/skills/new-deployment`. |
| **P4.6** | **Upstream CI/CD & Image Automation** | `nixos-core`, `.profile` | ⏳ **Planned** | Upstream-triggered CI/CD support and image update automation for workloads. |
| **P5** | **Identity, RBAC, `SecretProvider` & DDNS** | `nixos-core`, `ddns-monitor` | ⏳ **Planned** | LLDAP directory RBAC, Authelia ForwardAuth headers, user-scoped tokens, runtime `secretKeyRef` projection, and `ddns-monitor` cutover (`dynu-monitor` retirement). |
| **P6** | **Canopy MCP Gateway** | `nixos-core` | ⏳ **Planned** | Standardized JSON-RPC MCP tools wrapping `appctl` with client-side human confirmation gates. |
| **P7** | **Operations UI & Portal** | `nixos-core` | ⏳ **Planned** | Lightweight Operations UI client and Homepage live status integration. |

---

## 📁 Migrated Historical Plans
- [`docs/plans/p4-readiness-plan.md`](docs/plans/p4-readiness-plan.md) *(Completed — Milestone P4)*
- [`docs/plans/p4-refactor-and-decoupling-plan.md`](docs/plans/p4-refactor-and-decoupling-plan.md) *(Completed — Operator Cutover)*
