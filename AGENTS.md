# Canopy (`canopy-homelab`) — Operations & Planning Guidelines

This repository (`canopy-homelab/.profile`, mirrored publicly to `canopy-homelab/.github`) is the **unified workstation operations hub** and architectural planning center for the **Canopy** ecosystem.

---

## 1. Workspace Topology

- **Hub Root (`/home/<user>/Projects/active/canopy`)**: Owns ecosystem roadmap ([`ROADMAP.md`](ROADMAP.md)), cross-repo plans (`docs/plans/`), checklist ([`todo.txt`](todo.txt)), isolated worktrees (`.worktrees/`), and private incident/scratch notes (`.internal/`).
- **Nested Repositories (`repos/`, gitignored)**:
  - `repos/nixos-core` (`canopy-homelab/nixos-core`): NixOS host foundation, Core platform stack, `appctl` daemon/CLI, and `gitops_dispatcher` — deployed to `/home/operator/Core` on `server`.
  - `repos/kiskaadee` (`<user>/canopy-workloads`): Unified Tier 2 workload catalog — deployed to `/home/operator/Sites` on `server`.
  - `repos/ddns-monitor` (`canopy-homelab/ddns-monitor`): WAN IP rotation monitor & Cloudflare DDNS daemon.
- **Delegation**: When operating inside `repos/<subrepo>`, follow **both** these root operational guardrails and `<subrepo>/AGENTS.md`.
- **Public Mirror Boundary**: Because this repository mirrors publicly to GitHub (`canopy-homelab/.github`), **never** commit personal domains, LAN IPs, emails, or secrets here. Use SSH aliases (`server-local`, `server-remote`), `operator`, and `<domain>`; keep host-specific notes in gitignored `.internal/`.

---

## 2. Remote Interaction & Server Integrity Guardrails

### A. Local-First & Workspace Isolation
- **Never edit code or configuration files directly on the server over SSH** (`sed`, `cat <<EOF`, `nano`, `vim`). All durable changes originate in local Git repositories on a dedicated branch.
- Diagnostic investigation that is read-only does not require a branch. If repository state must change during troubleshooting, branch off `main` (`investigate/<topic>`, `fix/<topic>`, `feat/<topic>`).

### B. Read-Only SSH Diagnostics (Permitted)
Use `ssh server-local` (LAN) or `ssh server-remote` (WAN) — or `operator@server-local` — for non-destructive inspection:
- **Logs**: `journalctl -u <service> -n 50 --no-pager`, `docker logs --tail 50 <container>`, `appctl logs <app>`
- **Status**: `systemctl status <service> --no-pager`, `appctl list --core`, `appctl status`, `appctl info <app>`, `docker ps -a`, `docker inspect <container>`
- **Network/DNS checks**: `docker exec <container> <nslookup|curl|getent ...>`

### C. Production Execution Boundaries (Explicit User Approval Required)
To protect live infrastructure, agents **must not autonomously**:
1. **Execute production deployments** or pull changes onto the live server without explicit user authorization.
2. **Run `nixos-rebuild switch`** on the server (risks disrupting network, DNS, SOPS, or boot state).
3. **Restart or stop live production services** (`appctl up/down/restart`, `systemctl restart`, `docker restart`).
4. **Run destructive Docker/state commands** (`docker system prune -a --volumes`, deleting `~/var/lib/homelab/*` data).
5. **Mutate `/etc/` or host system files** outside declarative NixOS modules.

---

## 3. Production Deployment & Handoff Policy

When a change is validated locally and ready for the live server:
1. Present a structured **Deployment Handoff** containing:
   - **Exact User Action Required**: Exact commands for the target tier (e.g. `sudo nixos-rebuild switch --flake path:/home/operator/Core#server --impure` for Core, or `appctl update/restart <app>` for workloads).
   - **Expected Production Changes**: Services, units, or containers that will recreate or reload.
   - **Expected Post-Deployment Evidence**: Health checks, socket status, HTTP codes, or log lines confirming success.
   - **Verification Commands**: Read-only checks to run after execution.
   - **Rollback Procedure**: Exact steps to revert (`nixos-rebuild switch --rollback`, restoring backed-up state, etc.).
2. Wait for user execution (or explicit consent to execute) before proceeding to post-deployment verification.

---

## 4. CI/CD & Gitea Actions (`act_runner`) Invariants

- **Pre-Baked Images over In-Job Installers**: Declare `container: { image: <image> }` (e.g. `nixery.dev/shell/coreutils/git/nix/nodejs:latest`) rather than installing tools dynamically. Ensure the image includes `/bin/sleep` required by `act_runner` init.
- **Git Safe Directory**: Configure `git config --global --add safe.directory "$GITHUB_WORKSPACE"` in workflow steps when container UIDs differ.
- **No GitHub-Only API Assumptions**: Avoid third-party actions relying on `${{ github.server_url }}` resolving to `github.com`.

---

## 5. Global Skill Delegation

Operational workflows defer to the workstation's global skills (never duplicate or contradict them here):
- **Troubleshooting / RCA**: `/diagnose`
- **Git Hygiene, Branches & Commits**: `git-manager` (incremental Conventional Commits on working branches in `delivery` mode; never commit directly to `main` without explicit approval)
- **Planning & Decomposition**: `/gitea-plan` and `/implement-slice`
- **Code Review & Runtime QA**: `/code-review` and `/verify-slice`
- **Documentation**: `/document` (session insights) and `/reconcile-docs` (canonical docs)

---

## 6. Organization Planning Sync Contract

- **Authoritative Roadmap & Checklist**: [`ROADMAP.md`](ROADMAP.md), [`todo.txt`](todo.txt), and `docs/plans/` in `canopy-homelab/.profile`.
- **Organization Kanban Board**: **Canopy Platform** (`https://gitea.<domain>/canopy-homelab/-/projects/7`, Project ID: `7`).
- **Canonical 5-Column Pipeline (Deep Signal)**:
  - `Backlog` -> `column_id: 30` (`#475569`, default)
  - `Ready` -> `column_id: 31` (`#1d76db`)
  - `Work in Progress` -> `column_id: 32` (`#d97706`)
  - `Review` -> `column_id: 33` (`#6d28d9`)
  - `Completed` -> `column_id: 34` (`#15803d`)
- **Board Policy**: Child issues in `canopy-homelab/nixos-core` and `canopy-homelab/ddns-monitor` attach directly to Org Project `7`.
- **Mirror Policy**: Gitea (`canopy-homelab/.profile`) is primary; GitHub (`canopy-homelab/.github`) is an automated `sync_on_commit` push mirror.
- **Re-sync Trigger**: Re-run `/gitea-plan` before decomposing the next rolling milestone (P5/P6/P7) or upon scope shifts / Readiness Gates.
