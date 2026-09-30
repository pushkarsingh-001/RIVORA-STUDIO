# RIVORA STUDIO

A collaborative team workspace built with Next.js, Socket.IO, MongoDB, and Redis.

## Start locally

Requirements: Node.js 20.9 or later, npm, and Docker Desktop.

1. Copy `.env.example` to `.env` and set `JWT_SECRET` to a long random value. Use the same value in the realtime server environment.
2. Start the data services with `docker compose up -d`.
3. Install dependencies with `npm install`.
4. Start the web app and realtime server with `npm run dev`.
5. Open `http://localhost:3000`.

The UI works without a login in local demo mode. To connect realtime events, the browser must have a signed access token in `localStorage` under `orbit:access-token`. Tokens must include `sub` (the user ID) and `name` claims; `initials` and `color` are optional. The token issuer and login flow are intentionally left to the application's identity provider. Realtime workspace access is checked against MongoDB membership on every room join; viewers cannot send messages or edit board notes.

## Services

- Next.js serves the collaborative workspace UI.
- The Socket.IO service authenticates access tokens, checks workspace membership and roles, broadcasts chat and presence events, and syncs board notes.
- MongoDB stores workspaces, memberships, messages, and the shared board note. Create a `Workspace` record with a unique `slug` and member entries before joining its room.
- Redis is optional for a single realtime process and enables cross-instance Socket.IO broadcasts when `REDIS_URL` is configured.

`npm run dev:web` starts only the UI. `npm run dev:realtime` starts the realtime service. Copy `.env.example` to configure the environment. The demo's workspace slugs are `studio-north`, `fieldnotes`, and `common-ground`.

## Current demo boundary

Chat and board updates sync through the realtime service when configured. Tasks, file metadata, invites, and workspace switching currently use in-browser demo state; connect these to authenticated API routes and a file storage provider before using them for persistent team data. Never treat the UI's role selector as authorization: roles must come from trusted workspace membership data on the server.

Keep local environment secrets out of version control.
