# ERIDAN Website

Static marketing website for ERIDAN. No build step — plain HTML, CSS and a small
client-side runtime. Just serve the folder.

## Structure

```
index.html                     # Live homepage (copy of "ERIDAN Website v2.dc.html")
support.js                     # Design-canvas runtime that renders the page
image-slot.js                  # <image-slot> web component
assets/                        # Images used by the site
ERIDAN Website v2.dc.html      # v2 source (current)
ERIDAN Website.dc.html         # v1 source
ERIDAN Website v2 (bundle-src).dc.html
ERIDAN Website.html            # Fully self-contained bundle (all assets inline)
screenshots/                   # Reference screenshots (not served)
```

`index.html` loads `support.js`, `image-slot.js` and images from `assets/`,
plus Google Fonts. Everything runs client-side.

## Run locally

Any static file server works:

```bash
python3 -m http.server 8080
# then open http://localhost:8080
```

## Deploy

### Option A — Static host (recommended)
Upload the whole folder (or push this repo) to any static host:
GitHub Pages, Netlify, Vercel, Cloudflare Pages, or an Nginx/Apache docroot.
The host serves `index.html` automatically.

**GitHub Pages:** Settings → Pages → Source: `main` branch, `/ (root)`.

### Option B — Nginx on your own server
```nginx
server {
    listen 80;
    server_name your-domain.com;
    root /var/www/eridanweb;   # this folder
    index index.html;
    location / { try_files $uri $uri/ =404; }
}
```

Then copy the files:
```bash
rsync -av --exclude='.git' ./ user@server:/var/www/eridanweb/
```

### Option C — Zero-dependency fallback
If a host struggles with the external assets/runtime, serve
`ERIDAN Website.html` (everything inlined) as `index.html` instead.
