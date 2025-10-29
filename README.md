# [Meme League](https://memeleague.app)

A lightweight web app to run meme contests with friends. Organizers create a contest, invite players via short links, collect meme submissions, run anonymous voting (1–10, no self-votes), and display final rankings for both memes and players. Built as an MVP to be usable by a small community and easy to deploy on a small VPS.

## Frontend
- React (JavaScript) + Vite
- React Router for routing
- shadcn/ui for consistent, modern UI
- Mobile‑first, simple components focused on the three contest phases: Participation, Voting, Results
- Environment-driven backend URL via `import.meta.env.VITE_POCKETBASE_URL` (dev/prod)

## Backend
- PocketBase (embedded DB, REST API, auth, file storage)
- Collections for users, contests, players, memes, votes
- Business rules enforced with PocketBase hooks (e.g., phase transitions, voting constraints, limits)
- File uploads for memes (PNG/JPEG/GIF), max size enforced server-side and guarded on the client

## Deployment
- VPS with Caddy as the reverse proxy + automatic HTTPS
  - Static frontend served from `/var/www/dist`
  - PocketBase service under `/opt/memeleague/backend` managed by systemd
  - Logs: PocketBase (`/var/log/pocketbase`), Caddy (`/var/log/caddy`)
  - Correct client IP forwarding via `X-Forwarded-For` and PocketBase proxy header settings
- CI/CD via GitHub Actions
  - SSH deploy key and sudo permissions for `rsync`/`systemctl`
  - Environment secret `VITE_POCKETBASE_URL` to point the frontend to the deployed backend

## Why this project was valuable
- Rapid prototyping with PocketBase: modeling data, file storage, and writing hooks for guardrails (limits, unique votes, phase validation)
- Turning user flows into phased UI with clear state transitions and constraints
- Building a consistent, responsive UI quickly using shadcn/ui components
- Clean routing patterns, including short-link auth that redirects to the full player route
- Practical DevOps on a small VPS: systemd services, logs, firewall, Caddy reverse proxy + TLS
- Thoughtful environment management with Vite’s `import.meta.env` and minimal, focused configuration

