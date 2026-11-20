# CodeMe V1.1 — Android-only setup with free AI

## What this version includes
- Installable mobile PWA; no computer or Android Studio required.
- HTML/CSS/JS editor, live preview, local saving and HTML export.
- Cloudflare Workers AI backend template using the free hosted AI option where available.
- No paid model-provider API key is needed. Cloudflare free usage limits, model availability and account eligibility can change.

## Part 1 — Publish CodeMe from your Android phone
1. Download and extract this ZIP using your Android Files app.
2. In Chrome, sign in at https://github.com/ and create a public repository named `codeme`.
3. Upload the ZIP CONTENTS into the repository root (not the ZIP file). Keep `icons/` and `cloudflare-worker/` folders. `index.html` must be at the root.
4. Commit the files.
5. Open repository Settings → Pages → Build and deployment. Choose Deploy from a branch, select `main` and `/(root)`, then Save.
6. Wait for GitHub Pages to publish. Your site address will look like `https://YOURNAME.github.io/codeme/`.
7. Open that address in Chrome. Use ⋮ → Install app or Add to Home screen.

## Part 2 — Enable free hosted AI (Cloudflare Workers AI)
You can do this in Chrome on Android. If the dashboard layout is cramped, enable Chrome's Desktop site.

1. Create/sign in to a Cloudflare account at https://dash.cloudflare.com/.
2. Open Workers & Pages and create a Worker. Name it `codeme-ai`.
3. Open the Worker editor. Replace its starter code with the full contents of `cloudflare-worker/worker.js` from this ZIP, then Deploy.
4. In the Worker, open Settings → Bindings (the exact dashboard wording may vary) → Add binding → Workers AI. Set the variable/binding name to exactly `AI`, then save and deploy.
5. In Settings → Variables and Secrets, add a Secret named `CODEME_ACCESS_TOKEN`. Make a long random password (at least 32 characters), save it privately, and never put it in GitHub.
6. Add a plain-text variable named `ALLOWED_ORIGIN` with your GitHub Pages origin only, without the path. Example: if the site is `https://YOURNAME.github.io/codeme/`, set `ALLOWED_ORIGIN` to `https://YOURNAME.github.io`. Save and deploy.
7. Open your deployed CodeMe site. Tap ⚙. Paste the Worker URL ending in `/generate` and the same access token you created. Tap Save settings.
8. Enter a short website request and tap Generate.

The Worker uses the model ID `@cf/meta/llama-3.1-8b-instruct`. If Cloudflare no longer offers this model in your account, choose an available Workers AI instruct/chat model in the Cloudflare model catalog and change the `MODEL` constant at the top of `worker.js`.

## Important: free-tier and security limits
- "Free" means subject to Cloudflare's current free allocation, rate limits, model availability and account terms. It is not unlimited; check the Workers AI dashboard for current usage and billing details before relying on it.
- The access token is stored in this browser on your phone and is visible to anyone who can inspect that browser's local storage or source requests. It helps keep casual strangers away but is NOT strong authentication for a public app.
- The Worker checks the request origin, but CORS is not authentication. Do not publish your Worker URL/token or share the token. For a public multi-user launch, add real user authentication, server-side rate limiting and per-user quotas before inviting users.
- Never put Cloudflare account credentials or provider secrets in the website code or GitHub.
- The AI may generate incorrect or unsafe code. Review generated HTML/CSS/JS before publishing.
- GitHub Pages hosts static files only. CodeMe V1.1 does not compile APKs, run React/npm, provide a terminal, or run server-side code.
- Local projects are saved in Chrome storage on this phone. Export backups regularly.
