# Café Consume — website

A single-file static site (`index.html`, no build step, no dependencies except Google Fonts). This is the easiest kind of site to keep alive for years with zero maintenance.

## Deploy it (pick one, all free)

### GitHub Pages
1. Create a new GitHub repo (e.g. `cafe-consume-site`).
2. Upload `index.html` to the repo root.
3. Repo → **Settings** → **Pages** → set source to the `main` branch, `/root`.
4. Your site goes live at `https://<username>.github.io/cafe-consume-site/`.

### Netlify
1. Go to netlify.com → **Add new site** → **Deploy manually**.
2. Drag the folder containing `index.html` into the upload box.
3. It's live immediately at a `*.netlify.app` URL.

### Cloudflare Pages
1. Go to the Cloudflare dashboard → **Workers & Pages** → **Create** → **Pages** → **Upload assets**.
2. Upload `index.html`.
3. It's live at a `*.pages.dev` URL.

## Point a real domain at it (optional, ~$10–15/year)

1. Buy a domain (e.g. `cafeconsume.com`) from any registrar — Cloudflare Registrar, Namecheap, Porkbun are reasonable options.
2. In your host's settings (GitHub Pages / Netlify / Cloudflare Pages all support this), add the custom domain and follow their DNS instructions (usually a CNAME record pointing at the host).
3. Renew the domain yearly — that's the only recurring task.

## Keeping it alive long-term

- It's plain HTML/CSS with no server or database, so there's nothing to patch or that can break from a dependency update.
- Keep a copy of `index.html` somewhere durable (a second repo, a cloud drive) as backup.
- If you ever want a copy that survives even if you stop paying for anything, you can pin it to Arweave (permanent, pay-once storage) — ask if you'd like help with that.
