# Canopy (`canopy-homelab`)

**Canopy** is a declarative, security-hardened NixOS homelab ecosystem providing shared platform infrastructure, identity, edge TLS routing, and an operational control plane (`appctl`) for independently deployed workloads.

---

## Repository Hub Contents

- [`profile/README.md`](profile/README.md): Public organization profile rendered on Gitea and GitHub.
- [`ROADMAP.md`](ROADMAP.md): Authoritative Canopy ecosystem roadmap (Milestones P0–P7).
- [`docs/plans/`](docs/plans/): Detailed milestone readiness, migration, and decoupling plans.
- [`AGENTS.md`](AGENTS.md): Organization guidelines and Gitea Planning Sync Contract.
- [`todo.txt`](todo.txt): Active ecosystem task checklist.

## Organization Repositories

- [`canopy-homelab/nixos-core`](https://gitea.roadtotech.me/canopy-homelab/nixos-core) — NixOS host foundation, Core stack, `appctl` control plane, GitOps dispatcher.
- [`canopy-homelab/ddns-monitor`](https://gitea.roadtotech.me/canopy-homelab/ddns-monitor) — WAN IP rotation monitor & Cloudflare DDNS daemon.
- `kiskaadee/canopy-workloads` — Personal Tier 2 workload catalog (`~/Sites`).
