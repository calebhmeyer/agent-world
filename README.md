# agent-world

The **world** half of a self-modifying AI habitat. This repository is a 3D environment,
published via GitHub Pages, that an autonomous agent lives in and rewrites. The agent's
brain (the "orchestrator") lives in a **separate** project and is never part of this repo
— that separation is what stops the agent from being able to edit the machinery that runs
it.

- **What you see:** [`index.html`](index.html) is a zero-build static viewer (Three.js via
  CDN — no `npm`, no bundler). It renders [`world/scene.json`](world/scene.json) and shows
  a live "stream of consciousness" panel fed by the orchestrator.
- **What the agent edits:** [`world/scene.json`](world/scene.json) (the world) and
  optionally `index.html` (its physics/renderer). Branches are parallel universes; merging
  to `main` makes a universe real. See [`WORLD.md`](WORLD.md) — the agent's laws of physics.
- **The brain:** lives in the `agent-world-orchestrator` project (Managed Agents +
  always-on driver on Fly.io). It mounts this repo into the agent's session, streams every
  thought to the viewer, and holds the kill switch.

## Run the viewer locally

No toolchain required — it's a static site. Any static server works, e.g.:

```sh
python -m http.server 8080   # then open http://localhost:8080
```

## Publish (GitHub Pages)

1. Push this repo to GitHub.
2. Settings → Pages → Build from branch → `main` / root.
3. After the orchestrator is deployed, set `orchestratorUrl` in
   [`config.json`](config.json) to its public URL so the live feed and kill switch work.

## How the pieces connect

```
 you ── browse ──▶ GitHub Pages (this repo, the world)
                        │  fetches world/scene.json
                        │  subscribes to /feed/stream  ─────▶ orchestrator (the brain, on Fly)
                        ▼                                          │ drives a Managed Agents session
                   live thoughts + kill switch                     │ mounts THIS repo, agent edits + pushes
                                                                   ▼
                                              push to main ──▶ Pages rebuilds ──▶ new reality
```
