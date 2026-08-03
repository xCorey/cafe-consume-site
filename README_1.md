# Café Consume — website

A static site (`index.html` + `reviews.json`), plus a small GitHub Action that
keeps the Google and Yelp review sections up to date automatically.

## Deploy the site (pick one, all free)

### GitHub Pages
1. Create a new GitHub repo (e.g. `cafe-consume-site`) and upload everything in
   this folder, **keeping the folder structure** — `index.html`, `reviews.json`,
   `scripts/`, and `.github/workflows/` all need to be there.
2. Repo → **Settings** → **Pages** → set source to the `main` branch, `/root`.
3. Live at `https://<username>.github.io/cafe-consume-site/`.

GitHub Pages is the recommended option here specifically because the review
sync below runs as a GitHub Action in the same repo — everything stays in one
place.

### Netlify / Cloudflare Pages
Same idea — drag-and-drop or connect the repo. The review sync workflow only
runs on GitHub itself, so if you host elsewhere you'd still keep the repo on
GitHub for the Action to run, and point Netlify/Cloudflare at that repo to
redeploy whenever it changes.

## Keeping reviews live and refreshing weekly

A static site has no server, so nothing can poll Google or Yelp on its own.
Instead, `.github/workflows/update-reviews.yml` runs **on GitHub's servers**
every Monday, pulls the current rating and latest reviews from both
platforms, and commits the result to `reviews.json`. The page fetches that
file on every visit and renders it — so visitors always see last Monday's
snapshot, refreshed automatically, with zero ongoing maintenance from you.

To turn this on:

1. **Get a Google Places API key**
   - Go to [console.cloud.google.com](https://console.cloud.google.com/), create
     a project, enable **"Places API (New)"**, and create an API key under
     **APIs & Services → Credentials**.
   - Google's free monthly credit comfortably covers one place, checked once a
     week.

2. **Find your Google Place ID**
   - Use Google's [Place ID Finder](https://developers.google.com/maps/documentation/places/web-service/place-id)
     tool, search "Consume Vintage, 3452 S Western Ave, Chicago" (or Café
     Consume), and copy the Place ID it gives you.

3. **Get a Yelp Fusion API key**
   - Sign up at [yelp.com/developers](https://www.yelp.com/developers) and
     create an app to get an API key. Free tier is fine for a weekly check.

4. **Confirm the Yelp business ID**
   - It's the last part of the Yelp URL — for this business it looks like
     `café-consume-chicago-3` (from `yelp.com/biz/café-consume-chicago-3`).
     Double-check it's correct before adding it as a secret.

5. **Add all four as repo secrets**
   - Repo → **Settings** → **Secrets and variables** → **Actions** → **New
     repository secret**, and add:
     - `GOOGLE_PLACES_API_KEY`
     - `GOOGLE_PLACE_ID`
     - `YELP_API_KEY`
     - `YELP_BUSINESS_ID`

6. **Run it once manually**
   - Repo → **Actions** tab → **Update reviews** workflow → **Run workflow**.
     If it succeeds, `reviews.json` will update and the site will show the
     "Reviews last synced [date]" line instead of the fallback snapshot.

After that, it runs itself every Monday. If you ever want a different day or
time, edit the `cron` line in `.github/workflows/update-reviews.yml`.

**Note on Yelp:** their public API only returns up to 3 review excerpts per
business (that's a Yelp limitation, not this site's) — Google returns up to 5.

**If you skip this setup entirely:** the site still works fine. It falls back
to the saved snapshot baked into the page and just won't say "synced" — no
errors, nothing broken.

## Keeping it alive long-term

- The site itself is plain HTML/CSS/JS with no framework, so there's nothing
  to patch or that can break from a dependency update.
- The review sync is optional — if you stop maintaining the API keys, the
  workflow will just quietly fail and the site keeps showing the last good
  snapshot.
- Keep a backup copy of the whole folder somewhere durable (a second repo,
  cloud drive).
- Buying a domain (~$10–15/year) and pointing it at your host is optional —
  ask if you'd like the DNS steps for a specific registrar.
