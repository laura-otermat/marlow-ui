# Marlow Web

A React and TypeScript web client for [Marlow](https://github.com/laura-otermat/marlow), an AI household memory assistant.

Household members sign in, save things worth remembering, ask questions about them, and get checklists and summaries built from those memories. All data access goes through the Marlow API, which enforces household membership with PostgreSQL row-level security.

> This is a public snapshot of the Marlow web client. Active development happens in a private repository.

<!-- Add a screenshot here, e.g. ![Marlow Web](docs/screenshot.png) -->

## How it fits together

```
Browser (Marlow Web)
  │  signs in with Supabase Auth
  │  sends the user's access token as a bearer token
  ▼
Marlow API (FastAPI)  ──►  OpenAI (structured outputs)
  │  runs each request with the caller's token
  ▼
Supabase PostgreSQL (row-level security)
```

## Design decisions

- **The browser never holds privileged keys.** The client only uses the Supabase anon key. Inviting a household owner goes through the API, so the service-role key stays server-side.
- **No user creation in the browser.** Users are invited and set their own password through a secure Supabase invitation.
- **Membership is server-controlled.** Users can only act in households they already belong to. Membership is provisioned by a trusted server or admin flow, never by typing a household ID into the browser.
- **The server decides authorship.** The API derives who created a memory from the authenticated user, not from anything the client sends.

## Built with

React, TypeScript, Vite, Supabase Auth, Oxlint

## Running locally

You need a running [Marlow API](https://github.com/laura-otermat/marlow) and a Supabase project.

1. Copy `.env.example` to `.env.local` and fill in:

   ```
   VITE_MARLOW_API_URL=
   VITE_SUPABASE_URL=
   VITE_SUPABASE_ANON_KEY=
   ```

2. In Supabase Authentication settings, set the Site URL to `http://localhost:5173` and add `http://localhost:5173/**` as a redirect URL.

3. Install and start:

   ```
   npm install
   npm run dev -- --host localhost
   ```

   Restart Vite after editing `.env.local`.

## Checks

```
npm run build
npm run lint
```
