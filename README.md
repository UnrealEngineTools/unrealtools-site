# Unreal Tools — landing page

The hub at **unrealtools.com** — an entry point into Conduit and future tools for
Unreal Engine developers.

A single self-contained static page (`index.html`), no build step. Deployed via
Cloudflare Pages.

## Local preview

```bash
python -m http.server 8080   # then visit http://localhost:8080
```

Independent project, not affiliated with or endorsed by Epic Games or Unreal Engine.

## Deploy / update

Hosted on **Cloudflare Pages** (project `unrealtools-site`, domains `unrealtools.com` + `www`)
via wrangler **direct upload** — pushing this repo does NOT auto-deploy. Run wrangler from a
non-repo dir so its `.wrangler` cache never lands in the repo:

```bash
D=$(mktemp -d) && cp index.html "$D"/
cd "$D" && npx wrangler pages deploy . --project-name unrealtools-site --branch main --commit-dirty=true
```

### Custom domain note (apex)

Cloudflare auto-creates DNS for **subdomains** added to a Pages project, but NOT for the
**apex**. For `unrealtools.com` add the records by hand in the Cloudflare dashboard, then the
cert provisions in a few minutes:

| Type | Name | Target | Proxy |
|---|---|---|---|
| CNAME | `@` | `unrealtools-site.pages.dev` | Proxied |
| CNAME | `www` | `unrealtools-site.pages.dev` | Proxied |
