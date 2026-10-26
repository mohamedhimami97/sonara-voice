# Sonara — AI Voice Studio

An original, dependency-free Node 22+ SaaS starter with a responsive voice studio, real ElevenLabs TTS integration, private audio library, authentication, and a server-side character allowance.

## Run

```bash
node --version # Node >=22
npm start
```

Open http://localhost:3000. Set environment variables from `.env.example` before starting. `ELEVENLABS_API_KEY` must be set for real voices and generation. Never commit `.env` or `data/`.

## Architecture

- `server.js`: HTTP API, SQLite schema initialization/migrations baseline, scrypt password hashing, opaque HttpOnly session tokens, authenticated voice proxy, provider validation, server-side TTS, private audio storage, usage accounting.
- `public/`: responsive vanilla JavaScript UI with studio, voice catalog, audio history, account, pricing, documentation, legal overview.
- `data/`: runtime SQLite database and private MP3 files (not public assets).
- `tests/`: smoke tests.

## Important limitations

This is a functional development starter, **not yet a production-ready paid SaaS**. Provider keys and real provider access are required to exercise live TTS. No placeholder audio is represented as AI output. Pricing is illustrative and Stripe is not connected. No PostgreSQL, multi-instance storage, email verification, password reset, paid subscriptions, streaming, advanced audio tools, or API keys for customers. Audio deletion currently removes corresponding usage records, so the allowance is replenished on deletion; replace this with immutable billing records before enabling payments. Usage is character-based and may differ from actual provider billing. Rate limits are in-memory and require a shared distributed store for scaled deployment. SQLite and filesystem storage require persistent disk, single-instance deployment, backup policy, and encryption controls. No real customer legal policies are included.

## Security and deployment

Use HTTPS and `NODE_ENV=production`, set `APP_ORIGIN` to your exact public HTTPS origin, store `ELEVENLABS_API_KEY` as a hosting secret, mount `DATA_DIR` on a private persistent volume, and configure backups. The API enforces per-user audio access, CSRF origin check for browser writes, secure session cookies in production, input validation, and generation rate limiting. Provider API failures are surfaced to the user; generation timeouts are 30 seconds. `GET /health` is the health check.

For Render, create a **web service** from a Git repository containing this project (not a static site), set start command `npm start`, Node 22+, persistent disk, environment secrets, and health path `/health`. Deployment requires a repository and an authorized target workspace; this package has not been deployed.

## Roadmap

1. Move to managed PostgreSQL, private S3-compatible storage, durable credit ledger with reservation/refund/idempotency, distributed rate limiting, and background jobs.
2. Add email verification, password reset, user projects, favorites, and profile editing.
3. Provider capability discovery for models and language/accent filters; voice preview source policies.
4. Stripe checkout, webhooks, subscriptions, and plan enforcement.
5. Consent verification and moderation before any voice cloning; transcription, dubbing, and public API later.
6. Automated end-to-end tests against an isolated provider test account and production observability.
