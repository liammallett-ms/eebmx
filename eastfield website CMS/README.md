# Eastfield Eagles Website + CMS

A free, fast, dark-themed website for Maroondah BMX Club with a built-in admin panel so committee members can update content without touching code.

---

## What's in this folder

```
maroondah-bmx-cms/
├── index.html              ← The website itself
├── admin/
│   ├── index.html          ← The CMS admin panel (visit /admin/ to edit)
│   └── config.yml          ← Defines what editors can change
├── content/
│   ├── site.json           ← Page title, logo, intro video
│   ├── hero.json           ← Top banner copy
│   ├── ticker.json         ← Scrolling slogans
│   ├── about.json          ← About section + stats
│   ├── events.json         ← Weekly events
│   ├── gallery.json        ← Photo gallery
│   ├── join.json           ← Join CTA copy
│   └── footer.json         ← Address, contact, socials
└── uploads/                ← Logos, photos, videos go here
    ├── eagles-logo.png
    └── eagles-intro.mp4
```

When an editor saves a change in the CMS, they're really just updating one of those JSON files. The website reads them on every page load and renders the latest content.

---

## Recommended deployment: Cloudflare Pages

Free forever, fast (CDN in Australia), no usage limits that matter for a club site.

### One-time setup (≈30 minutes)

**1. Create a free GitHub account** at [github.com](https://github.com) if you don't have one.

**2. Create a new repository:**
- Click "New repository"
- Name it `maroondah-bmx-website` (or whatever you like)
- Set it to **Public** (required for the free CMS auth method below)
- Click "Create repository"
- Upload everything in this folder using the "uploading an existing file" link, then commit.

**3. Update the CMS config to point at your repo:**
Open `admin/config.yml` and change the line:
```yaml
repo: YOUR_GITHUB_USERNAME/YOUR_REPO_NAME
```
to your actual GitHub username and repo name. Also update the two `YOUR-SITE-DOMAIN.com` placeholders to your real domain (or your `*.pages.dev` URL — see below).

**4. Connect Cloudflare Pages:**
- Sign up for free at [pages.cloudflare.com](https://pages.cloudflare.com)
- Click "Create application" → "Connect to Git" → authorise GitHub
- Pick your `maroondah-bmx-website` repo
- Build settings: leave everything blank — there's no build step. Just set the output directory to `/` (the root)
- Click "Save and Deploy"

In about 60 seconds, your site is live at `https://maroondah-bmx-website.pages.dev` (or whatever name you chose).

**5. Set up authentication for the CMS:**

Decap CMS needs a way to let editors log in to GitHub. The easiest free option is **GitHub OAuth via a small auth helper**. The official Decap docs walk through three options:

- [Cloudflare-hosted auth helper](https://decapcms.org/docs/external-oauth-clients/) (free, recommended for Cloudflare Pages)
- [Netlify Identity](https://decapcms.org/docs/netlify-identity/) (free if you also host on Netlify instead)
- [Direct GitHub OAuth](https://decapcms.org/docs/github-backend/) (slightly more setup)

The Cloudflare option takes about 10 minutes — follow [this guide](https://decapcms.org/docs/external-oauth-clients/#cloudflare-workers).

> **If you'd prefer Netlify hosting instead** (also free, similar speed), it's even simpler — Netlify has built-in Identity that just works. Sign up at [netlify.com](https://netlify.com), connect the same GitHub repo, and follow [their CMS guide](https://decapcms.org/docs/netlify-identity/). Either host works perfectly.

**6. Point your domain at it:**
In Cloudflare Pages → "Custom domains", add `maroondahbmxclub.com.au`. You'll need to update DNS at wherever the domain is currently registered. Cloudflare gives you the exact records to change.

---

## How editors use the CMS

Once it's deployed, anyone with access goes to:

```
https://maroondahbmxclub.com.au/admin/
```

They click "Login with GitHub", authorise, and land on a clean editor that looks like this:

```
🦅 Eagles Website
├── 🏠 Site Basics — logo, intro video, page title
├── 🚀 Hero — top of homepage
├── 📜 Scrolling Ticker
├── ℹ️ About Section
├── 📅 What's On — Events
├── 🖼️ Photo Gallery
├── 🎯 Join CTA
└── 🦅 Footer
```

They click a section, fill in the friendly form, hit "Publish". Site updates in about a minute.

### Tips for editors
- **Bold text:** wrap a phrase in `**double asterisks**` and it'll render bold on the site.
- **Internal links:** to link to a section on the same page, use `#about`, `#whats-on`, `#gallery`, or `#join`.
- **Photos:** the CMS resizes nothing — upload reasonably-sized images (under 2MB). Use a free tool like [tinypng.com](https://tinypng.com) to compress before uploading if photos are large.
- **Mistakes:** every save creates a Git commit. Mistakes can be rolled back from the GitHub repo's commit history.

---

## Adding committee members as editors

To give another committee member edit access:

1. They create a free GitHub account.
2. In your GitHub repo, go to "Settings" → "Collaborators" → "Add people" → enter their GitHub username.
3. Once they accept the invite, they can log in to `/admin/` with their own GitHub account and edit just like you.

No payment, no extra setup. As many editors as you want.

---

## Local preview (optional)

If you want to test changes before deploying, you can run the site locally. From inside this folder:

```bash
# Python (comes pre-installed on Mac/Linux)
python3 -m http.server 8000

# OR Node
npx serve .
```

Then visit `http://localhost:8000` in your browser. The CMS admin panel won't work locally (it needs GitHub auth), but the site itself will render fine.

---

## Costs

- **Cloudflare Pages:** free, no limits that matter
- **GitHub repo:** free for public repos
- **Decap CMS:** free, open source
- **Domain:** whatever your domain renewal already costs

Total ongoing cost: **$0**.

---

## Future ideas

- Add a "Sponsors" section managed the same way (just add a new file in `content/` and a new entry in `admin/config.yml`).
- Add a blog/news collection — Decap supports a "folder" collection where each post is its own file.
- Hook up Cloudflare Web Analytics (free) for traffic stats.

If anything breaks or you want help adding a section, save the error message and we can sort it out.
