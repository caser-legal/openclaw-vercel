# Deploy your own OpenClaw on Vercel

This repository is a Next.js app. Deploy it as a new project in your own Vercel account. It does not contain a Vercel project id, team slug, token, or private hostname. Put those only in your Vercel project settings or a local env file that stays uncommitted.

## What you need

- Node.js 20 or newer and npm 10 or newer
- A Vercel account
- An Upstash Redis database (Vercel Marketplace, or any Upstash-compatible REST endpoint)

## Deploy

1. Fork this repository or push a clone to a Git host you control.
2. In Vercel, create a new project and import that repository. Use the Next.js framework preset. Do not attach this app to someone else's project.
3. Copy keys from `.env.example`. The example file has empty values on purpose. Set real values in the Vercel project (or in a gitignored `.env.local` for local work).
4. For channel state, queue data, and sandbox metadata to survive cold starts, set:
   - `UPSTASH_REDIS_REST_URL`
   - `UPSTASH_REDIS_REST_TOKEN`
5. Auth defaults to `admin-secret`. If `ADMIN_SECRET` is unset, the app generates a secret and stores it in Upstash. Set `ADMIN_SECRET` yourself when you want a password you already know. That value also authenticates `/api/cron/watchdog` unless you set `CRON_SECRET`.
6. Deploy. Open the URL Vercel gives your project and sign in to the admin UI.
7. Start the sandbox from the admin panel, or open `/gateway`.
8. Run launch verification before you connect Slack, Telegram, or Discord. Enter channel credentials in the admin UI. They are stored in Redis, not in this repository.

## Local development

```sh
npm install
cp .env.example .env.local
npm run dev
```

Fill `.env.local` on your machine. Do not commit it.

## Optional settings

`.env.example` documents the rest:

- `VERCEL_AUTH_MODE=sign-in-with-vercel` plus your own OAuth client id, client secret, and `SESSION_SECRET`
- `OPENCLAW_PACKAGE_SPEC`, for example `openclaw@1.2.3`, instead of floating `openclaw@latest`
- `OPENCLAW_INSTANCE_ID` when several deployments share one Redis database
- `OPENCLAW_SANDBOX_VCPUS` and `OPENCLAW_SANDBOX_SLEEP_AFTER_MS`
- `VERCEL_AUTOMATION_BYPASS_SECRET` if Deployment Protection would block webhooks
- `NEXT_PUBLIC_APP_URL` or `NEXT_PUBLIC_BASE_DOMAIN` when the public origin should not be the default Vercel hostname
- `NEXT_PUBLIC_SANDBOX_SCOPE` and `NEXT_PUBLIC_SANDBOX_PROJECT` as labels for your team and project on the admin terminal tab

AI Gateway auth on Vercel uses OIDC. `AI_GATEWAY_API_KEY` is only a fallback when OIDC is unavailable.

## License

MIT. See LICENSE. Copyright remains with the copyright holder named there.
