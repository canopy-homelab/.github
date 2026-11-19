# Canopy (`canopy-homelab`)

**Canopy** is a declarative, security-hardened NixOS homelab ecosystem providing shared platform infrastructure, identity, edge TLS routing, and an operational control plane (`appctl`) for independently deployed workloads.

---

## Ecosystem Repositories

| Repository | Role | Forge / Mirror |
| :--- | :--- | :--- |
| **[`canopy-homelab/.profile`](https://gitea.roadtotech.me/canopy-homelab/.profile)** | Organization profile, master roadmap, cross-repo planning, and architectural plans | [GitHub `.github`](https://github.com/canopy-homelab/.github) |
| **[`canopy-homelab/nixos-core`](https://gitea.roadtotech.me/canopy-homelab/nixos-core)** | Declarative NixOS host foundation, Core platform stack, `appctl` control plane daemon, and `gitops_dispatcher` | [GitHub `nixos-core`](https://github.com/canopy-homelab/nixos-core) |
| **[`canopy-homelab/ddns-monitor`](https://gitea.roadtotech.me/canopy-homelab/ddns-monitor)** | Lightweight WAN IP rotation monitor and Cloudflare DNS updater | [GitHub `ddns-monitor`](https://github.com/canopy-homelab/ddns-monitor) |
| **`<user>/canopy-workloads`** *(Workload Tier)* | Unified workload catalog (`~/Workloads/<app>`) containing `app.yaml`, `docker-compose.yml`, and `deployments.yaml` | — |

---

## Conceptual Architecture

| Canopy Layer | Responsibility |
| :--- | :--- |
| **Canopy Core** (`nixos-core`) | Host OS (NixOS), Traefik ingress, Authelia/LLDAP SSO, Stalwart mail, `socket-proxy` |
| **Canopy Control** (`appctl`) | Dual-transport daemon (`homelab-appctl.service`), 3-role RBAC, append-only audit guard, thin CLI |
| **Canopy GitOps** (`gitops_dispatcher`) | HMAC-SHA256 webhook ingress, Compose AST security validator, serialized deployment queue |
| **Canopy Workloads** (`~/Workloads`) | Independently managed containerized workloads with isolated host state (`~/var/lib/homelab/<app>/`) |

See [`ROADMAP.md`](../ROADMAP.md) for current milestones (P0–P4 completed; P4.5–P4.6 + P5–P7 upcoming).
