# robhutt.com

The standalone marketing site for Rob Hutt: a single static HTML page with
inline CSS/JS and Google Fonts (Fraunces + Spline Sans Mono). No framework, no
bundler, no build step.

This folder is self-contained and intentionally separate from the full-stack
app in the rest of this repository.

## Layout

```
robhutt-com/
  index.html          the page (the ported rob-hutt.html)
  netlify.toml        deploy config for Netlify
  assets/
    video/            drop reel-01.mp4 ... reel-04.mp4 here
    img/              favicon.png and og.png go here
  README.md
```

## Run locally

No build step. Serve the folder with any static server, for example:

```bash
cd robhutt-com
npx serve .
```

Then open the printed URL (typically http://localhost:3000). Any static
server works (`python3 -m http.server`, the VS Code Live Server extension,
etc.). Opening `index.html` directly via `file://` also renders, but a local
server is closer to production and avoids any path quirks.

## Things Rob needs to fill in

These are left as clearly-marked placeholders in `index.html`; search for
`TODO`:

1. **Booking link** - replace `REPLACE_WITH_YOUR_CALENDAR_LINK` in the
   `Book a conversation` button.
2. **Email** - replace `REPLACE_WITH_YOUR_EMAIL` in the `mailto:` and the
   visible link text `your@email.com`.
3. **Videos** - drop `reel-01.mp4` through `reel-04.mp4` into
   `assets/video/`, then uncomment the matching `<source>` line inside each
   `<video>` element. The slots stay in their placeholder state until a
   `<source>` is present, so there is nothing else to wire up.
4. **Editorial notes** - the `[bracketed]` reminders in the copy
   (`[Name your headline awards + Time Spent.]`, `[Link the book.]`) are
   intentional. Edit or remove them when the real details are ready.
5. **Favicon / social image** (optional) - add `assets/img/favicon.png` and
   `assets/img/og.png`, then uncomment the favicon `<link>` in `<head>`.

## Deploy

Chosen host: **Netlify**. Reason: it is a zero-config static host with
push-to-deploy from Git and free custom-domain support, and it can publish
from a subdirectory, which lets this static site live alongside the existing
full-stack app without colliding with that app's own Render deployment.

### One-time setup

1. In Netlify, create a new site from this Git repository.
2. Set **Base directory** to `robhutt-com`.
3. Leave the build command empty and the publish directory as `.`
   (already declared in `netlify.toml`).
4. Deploy. Every push to the connected branch redeploys automatically.

(Any other static host works too: point the publish/root directory at
`robhutt-com` and you are done.)

## Custom domain (robhutt.com) - DNS steps for Rob

DNS is **not** changed here; do this yourself at the registrar.

1. In Netlify: **Site settings -> Domain management -> Add custom domain** and
   enter `robhutt.com`.
2. At the domain registrar, point the domain at Netlify. Either:
   - delegate to Netlify DNS by setting the registrar's nameservers to the
     four `*.nsone.net` nameservers Netlify shows you, **or**
   - keep your current DNS and add records: an `A` record for the apex
     `robhutt.com` to Netlify's load-balancer IP (`75.2.60.5`), and a
     `CNAME` for `www` to your `*.netlify.app` site name.
3. Back in Netlify, enable HTTPS (Let's Encrypt certificate provisions
   automatically once DNS resolves).

Allow up to 24-48 hours for DNS propagation.
