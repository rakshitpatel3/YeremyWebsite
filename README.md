# Youvngprcuts — Astro + Vercel Setup Guide

Complete steps from zero to live on youvngprcuts.com

---

## STEP 1 — Install Node.js

Astro requires Node.js. Check if you have it:

```bash
node -v
```

If you get a version number (v18 or higher) you're good. If not:

1. Go to https://nodejs.org
2. Download the **LTS** version
3. Install it, then re-run `node -v` to confirm

---

## STEP 2 — Set up the project locally

1. Open Terminal (Mac: Cmd+Space → type Terminal)
2. Navigate to where you want the project:

```bash
cd ~/Desktop
```

3. Clone your existing GitHub repo (replace with your actual repo URL):

```bash
git clone https://github.com/YOUR_USERNAME/YEREMYWEBSITE.git
cd YEREMYWEBSITE
```

4. Copy all the files from the `astro-project` folder you downloaded into this repo folder, replacing what's there. Your structure should look like:

```
YEREMYWEBSITE/
├── public/
│   ├── Favi/          ← copy your existing Favi folder here
│   ├── Gallery/       ← copy your existing Gallery folder here
│   ├── CNAME
│   └── _headers
├── src/
│   ├── components/
│   ├── layouts/
│   ├── pages/
│   └── styles/
├── astro.config.mjs
└── package.json
```

5. Install dependencies:

```bash
npm install
```

---

## STEP 3 — Run it locally

```bash
npm run dev
```

Open your browser to **http://localhost:4321**

You should see the site running. Any changes you make to `.astro` files will hot-reload instantly.

---

## STEP 4 — Build for production (test before deploying)

```bash
npm run build
npm run preview
```

This builds the final static files into a `dist/` folder and lets you preview exactly what Vercel will serve.

---

## STEP 5 — Push to GitHub

```bash
git add .
git commit -m "migrate to astro"
git push origin main
```

---

## STEP 6 — Deploy on Vercel

1. Go to **https://vercel.com**
2. Click **Sign Up** → choose **Continue with GitHub**
3. Authorize Vercel to access your repos
4. Click **Add New Project**
5. Find your `YEREMYWEBSITE` repo and click **Import**
6. Vercel auto-detects Astro — you don't need to change any settings
7. Click **Deploy**

Vercel builds your site and gives you a live URL like:
`yeremywebsite.vercel.app`

Every future `git push` to main will auto-deploy. Done.

---

## STEP 7 — Connect your custom domain

1. In Vercel dashboard → click your project → **Settings → Domains**
2. Type `youvngprcuts.com` → click **Add**
3. Also add `www.youvngprcuts.com` → click **Add**
4. Vercel will show you DNS records to add. You'll get something like:

```
Type: A
Name: @
Value: 76.76.21.21

Type: CNAME
Name: www
Value: cname.vercel-dns.com
```

5. Log into **Hostinger** → Domains → `youvngprcuts.com` → **DNS / Nameservers**
6. Add the records Vercel gave you (delete any old A or CNAME records pointing to GitHub first)
7. Wait 5–30 minutes for DNS to propagate
8. Visit `youvngprcuts.com` — it should now load from Vercel

---

## STEP 8 — Verify security headers are active

Once live on Vercel, test your headers at:
**https://securityheaders.com/?q=youvngprcuts.com**

You should get an A or A+ rating. The `public/_headers` file handles this automatically on Vercel.

---

## Adding new gallery images

1. Drop the image file into `public/Gallery/`
2. Open `src/components/Gallery.astro`
3. Add a new entry to the `images` array at the top:

```js
{ src: '/Gallery/your-new-image.jpg', alt: 'Description', cls: 'gi-2' },
```

4. `git push` — Vercel deploys automatically

---

## Changing hours

Open `src/components/Hero.astro` and find `SHOP_HOURS` near the bottom in the `<script>` block:

```js
const SHOP_HOURS = {
  0: [11,16], // Sun  — 11am to 4pm
  1: [10,19], // Mon  — 10am to 7pm
  2: null,    // Tue  — closed
  3: [10,19], // Wed
  4: [10,19], // Thu
  5: [10,19], // Fri
  6: [10,19], // Sat
};
```

Edit the numbers (24-hour format) and push. The open/closed dot and pill update automatically.

---

## File reference

| File | What it does |
|------|-------------|
| `src/layouts/BaseLayout.astro` | HTML shell — `<head>`, fonts, meta tags — used by every page |
| `src/pages/index.astro` | Homepage — imports all section components |
| `src/pages/about.astro` | Full story page |
| `src/components/Nav.astro` | Navigation bar (write once, used everywhere) |
| `src/components/Hero.astro` | Hero section + live hours logic |
| `src/components/About.astro` | About teaser section |
| `src/components/Gallery.astro` | Gallery grid — edit image list here |
| `src/components/Reviews.astro` | Reviews — edit quotes here |
| `src/components/Location.astro` | Hours + directions |
| `src/components/Contact.astro` | SMS form + socials |
| `src/components/Footer.astro` | Footer |
| `src/styles/global.css` | All shared CSS variables and base styles |
| `public/_headers` | Real HTTP security headers served by Vercel |
| `public/Gallery/` | Your cut photos |
| `public/Favi/` | Favicon files |
| `astro.config.mjs` | Astro configuration |