# Canopy Workloads (`~/Workloads`) Progressive Migration Plan

This plan governs the incremental migration of Tier 2 workloads from 13 individual wrapper repositories in `/home/operator/Sites/` into the unified `canopy-workloads` monorepo at `/home/operator/Workloads/`, with mutable runtime state isolated under `/home/operator/var/lib/homelab/<app>/`.

---

## Migration Principles & Guardrails

1. **Vertical-Slice Progression**: Migrate one small, cohesive slice at a time with explicit user consent before each live cutover step.
2. **Source-Clone / Image Purity**:
   - **Class B (Image-Only)**: Express via official/registry Docker images in `docker-compose.yml` + `app.yaml`.
   - **Class A (Source-Backed)**: Express via remote Git `build:` context or CI-published OCI images **without copying upstream source code into `canopy-workloads`**. If an upstream source repo is not yet ready for standalone build/clone, **explicitly defer** that workload's migration.
3. **Hybrid Coexistence**: Core tooling (`provenance.py`, `appctl_router.py`) checks `<workload>/.git` first and falls back to `~/Workloads/.git` subpath status so migrated and deferred workloads operate side-by-side safely.
4. **Catastrophic-Error Mitigation**:
   - **Gitea**: Verified SHA256-checksummed backup at `/home/operator/var/lib/homelab/backups/` + state already isolated at `/home/operator/var/lib/homelab/gitea` before any monorepo consolidation.
   - **Per-Slice Rollback**: Keep legacy `Sites/<app>` intact until the migrated `Workloads/<app>` container passes health, HTTP/TLS routing, and `appctl` checks.

---

## Progressive Slices

| Slice | Target | Repo / Scope | Risk & State Strategy |
| :--- | :--- | :--- | :--- |
| **Slice 0 (Prereq)** | **Issue #33**: `~/Sites` $\to$ `~/Workloads` rename, hybrid subpath provenance, monorepo GitOps, remote `build:` AST check | `canopy-homelab/nixos-core` | Foundation PR; verify `./scripts/test` before touching live host |
| **Slice 1 (Pilot)** | `excalidraw`, `mermaid` | `canopy-workloads` | **Class B (Official Images)** — stateless; proves `appctl up` + Traefik routing from `~/Workloads` |
| **Slice 2 (Stateless / Disposable)** | `landing`, `pgsql`, `mongodb`, `ollama`, `minecraft` | `canopy-workloads` | **Class B** — static assets / disposable state; isolate any retained data to `~/var/lib/homelab/<app>` |
| **Slice 3 (Stateful Class B)** | `snappymail`, `jellyfin` | `canopy-workloads` | Isolate `snappymail/data` to `~/var/lib/homelab/snappymail`; address Jellyfin `/dev/dri` policy |
| **Slice 4 (Crown Jewel)** | `gitea` | `canopy-workloads` | **High Care**: State already isolated in `~/var/lib/homelab/gitea`; verify `act_runner` socket policy & SSH `:2223` |
| **Slice 5 (Source-Backed Class A)** | `houston`, `bdinvite`, `armonia` | `canopy-workloads` | Audit each upstream repo for clean `Dockerfile` + remote `build:` / OCI readiness; **defer** unready repos |
