# Cozely — legal + marketing pages (GitHub Pages)

This folder holds the public **Privacy Policy**, **Terms of Use**, and **Support** pages
(plus the marketing `index.html` and a few `story-*.html` pages). The app links straight to
privacy/terms/support via `CozelyLegal.legalBase` in
`Relationship App/Backend/CozelyLegal.swift`, which is set to the live, verified host:

```
https://bswxyz.github.io/cozely-legal
```

The pages are hosted free on **GitHub Pages** from the PUBLIC `cozely-legal` repo (under the
`bswxyz` account), served directly at `bswxyz.github.io/cozely-legal/` — **no custom domain.**

> **Why not `tdle.win`?** The domain is registered (Cloudflare) and the Clerk auth subdomain
> `clerk.tdle.win` works, but the **apex (`tdle.win`) was never given a DNS record**, so it
> doesn't resolve in a browser. Rather than wait on DNS + cert provisioning, the legal/support
> URLs were pointed at the already-live github.io host. **Decision: ship on github.io.**
>
> **The `CNAME` file is intentionally disabled** (renamed `CNAME.disabled`). A live `CNAME`
> would tell GitHub Pages to switch to the `tdle.win` custom domain — and because the apex has
> no DNS record, that would 301-redirect these URLs to a domain that doesn't resolve and break
> the App Store links. Leave it renamed unless you complete the branded-domain setup below.

---

## Put them online — repo upload (~5 minutes, free, no terminal)

1. Go to **https://github.com/new** (sign in as `bswxyz`).
2. **Repository name:** type exactly `cozely-legal`.
3. Set visibility to **Public**. *(Required — App Review and users open these without logging in.)*
4. Click **Create repository**.
5. On the new empty repo page, click **uploading an existing file**.
6. In Finder, open this `legal/` folder. Select **all** the files —
   `index.html`, `privacy.html`, `terms.html`, `support.html`, `stories.html`,
   `story-busy.html`, `story-questions.html`, `story-stayin.html` — plus the `shots/` folder,
   then drag them into the browser's upload area.
   **Do NOT upload `CNAME.disabled`** (and never re-add a file literally named `CNAME` unless
   you finish the branded-domain steps below). `shots/` holds the marketing images;
   `assets/` holds the shipping Cozely mark and Apple touch icon, so publish it too.
7. Wait for the upload to finish, then click **Commit changes**.
8. Open the repo's **Settings** tab → left sidebar → **Pages**.
9. Under **Build and deployment → Source**, choose **Deploy from a branch**, **Branch** `main`,
   folder **`/ (root)`**, then **Save**. Leave **Custom domain** blank.

Confirm it's live (each loads over HTTPS — verified 200):
- https://bswxyz.github.io/cozely-legal/privacy.html
- https://bswxyz.github.io/cozely-legal/terms.html
- https://bswxyz.github.io/cozely-legal/support.html

These exact URLs are wired into the app (`CozelyLegal.swift`) and are what you paste into
App Store Connect (Privacy Policy URL + Support URL).

---

## REQUIRED before submit — make `support@tdle.win` receive mail (Cloudflare Email Routing)

All three legal pages, the Terms, the DMCA-agent filing, and the in-app "Report a concern"
link list **`support@tdle.win`**. The `tdle.win` zone currently has **no MX records**, so that
address bounces. This is independent of the web hosting above (MX ≠ the apex web record), so
you can keep github.io for the web AND make the branded email work:

1. Cloudflare dashboard → select **tdle.win** → **Email** → **Email Routing** → **Get started**.
2. Cloudflare offers to add the required **MX + TXT (SPF)** records automatically — accept.
3. Under **Routing rules**, add a custom address: **`support@tdle.win` → forward to** your
   Gmail. Cloudflare emails that Gmail a one-click verification — confirm it.
4. Send a test message to `support@tdle.win` and confirm it lands in your Gmail.

Free, ~2 minutes, doesn't touch the apex web record or `clerk.tdle.win`. Not tested by App
Review at upload, but required for privacy/DMCA requests to reach you.

---

## OPTIONAL (later) — branded `tdle.win` web domain

Only if you want `https://tdle.win/...` to serve the pages instead of github.io. Not required
to ship. Order matters — do DNS first or you'll break the live URLs:

1. **Cloudflare DNS** → add a `CNAME` record: **Name** `@` (apex), **Target** `bswxyz.github.io`,
   **Proxy status: DNS only (grey cloud)** (so GitHub can issue its own cert — proxied/orange
   loops with Pages HTTPS). Wait until `tdle.win` resolves.
2. In the cozely-legal repo, rename `CNAME.disabled` back to `CNAME` (contents `tdle.win`) and
   push, then GitHub **Pages → Custom domain** = `tdle.win`, **Save**, and tick **Enforce HTTPS**
   once the cert is issued (minutes to a few hours).
3. No app rebuild needed: GitHub 301-redirects the github.io links to `tdle.win`, and the App
   Store Connect URL fields are metadata you can update to `tdle.win` anytime.

Leave these alone throughout: `clerk.tdle.win` / `accounts.tdle.win` (Clerk production) and the
MX records from Email Routing above.

## Updating a page later
- Edit the file on GitHub (pencil icon) or re-upload it; Pages redeploys in ~1 minute.
  No app rebuild is needed unless you change `legalBase` itself (it ships compiled in).
