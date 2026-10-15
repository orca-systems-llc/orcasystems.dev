# orcasystems.dev

Public company site for **Orca Systems LLC**. Static HTML, no build step.

Apple Developer Program needs a working website on the company domain, not a registrar placeholder. After this is live, use:

| App Store Connect / enrollment | URL |
| --- | --- |
| Organization website | https://orcasystems.dev |
| Support URL | https://orcasystems.dev/support |
| Privacy Policy URL | https://orcasystems.dev/privacy |

## Deploy: GitHub Pages

This is the easiest option. The repo is already on GitHub. There is no Docker, Railway, or Node server.

**The repository must be public.** GitHub Free for organizations only serves Pages from public repos. A company homepage has nothing private in it; make `orca-systems-llc/orcasystems.dev` public.

1. Push `main`.
2. GitHub → the repo → **Settings** → **Pages**.
3. Source: **Deploy from a branch**. Branch: `main`, folder: `/ (root)`.
4. Custom domain: `orcasystems.dev`. Save. Check **Enforce HTTPS** after DNS is green (often 10–60 minutes).

The `CNAME` file in this repo already names `orcasystems.dev`. `.nojekyll` keeps GitHub from running Jekyll on the files.

### DNS at the registrar

Apex (`orcasystems.dev`) — four `A` records, and optionally four `AAAA`:

```
A     @    185.199.108.153
A     @    185.199.109.153
A     @    185.199.110.153
A     @    185.199.111.153
AAAA  @    2606:50c0:8000::153
AAAA  @    2606:50c0:8001::153
AAAA  @    2606:50c0:8002::153
AAAA  @    2606:50c0:8003::153
```

`www` — one `CNAME`:

```
CNAME  www  orca-systems-llc.github.io
```

Remove any parking/`A` record the registrar added for `@`, or HTTPS will not issue.

If the domain is already on Cloudflare, an `ALIAS`/`CNAME` flattened to `orca-systems-llc.github.io` also works. Keep extra apex records off that hostname so GitHub can mint a certificate.

### Email

Apple’s support URL has to lead to a real contact. Create mailboxes (or forwards) for:

- `admin@orcasystems.dev`

Cloudflare Email Routing is the usual zero-inbox way: MX on the domain, forward that mailbox to the address you actually read.

## Local preview

```sh
python3 -m http.server 8765
```

Open http://127.0.0.1:8765

## Why not Railway or Cloudflare Workers

Smallfry’s product site uses a tiny Node server on Railway because it has health checks and gzip. This company site is files on disk. GitHub Pages is fewer moving parts, free, and HTTPS is automatic once DNS is right. Switch later if you need a server; you will not need one for Apple.
