# Artha Group Inc. — Deployment Guide
## Hosting: Cloudflare Pages + GoDaddy Domain

---

## STEP 1 — Set Up Contact Form (Formspree)

1. Go to https://formspree.io and create a free account
2. Click "New Form" → name it "Artha Contact"
3. Copy your Form ID (looks like: `xabcdefg`)
4. Open `index.html` and replace:
   ```
   action="https://formspree.io/f/YOUR_FORM_ID"
   ```
   with your actual form ID, e.g.:
   ```
   action="https://formspree.io/f/xabcdefg"
   ```

---

## STEP 2 — Push to GitHub

1. Install Git if needed: https://git-scm.com/
2. Create a free GitHub account: https://github.com
3. Create a new PRIVATE repository named `artha-website`
4. Run these commands in your project folder:

```bash
cd C:\Users\sangi\artha-website
git init
git add .
git commit -m "Initial website build"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/artha-website.git
git push -u origin main
```

---

## STEP 3 — Deploy on Cloudflare Pages

1. Go to https://dash.cloudflare.com and sign up (free)
2. In the sidebar: **Workers & Pages → Pages → Create a project**
3. Click **"Connect to Git"** → authorize GitHub
4. Select your `artha-website` repository
5. Configure the build:
   - **Framework preset**: None
   - **Build command**: *(leave empty)*
   - **Build output directory**: `/` (root)
6. Click **"Save and Deploy"**
7. Cloudflare will give you a URL like: `artha-website.pages.dev`

---

## STEP 4 — Connect Your GoDaddy Domain

### Option A: Change Nameservers (Recommended — Full Cloudflare)
This gives you Cloudflare's full CDN, DDoS protection, and SSL.

1. In Cloudflare dashboard: **Add a Site** → enter `arthagroupinc.com` → pick Free plan
2. Cloudflare will scan existing DNS records
3. Click **Continue** → copy the two Cloudflare nameservers shown (e.g., `ada.ns.cloudflare.com`)
4. Log into **GoDaddy → My Domains → arthagroupinc.com → Manage DNS**
5. Scroll to **Nameservers → Change → Custom**
6. Replace with the two Cloudflare nameservers
7. Save — DNS propagation takes 5–30 minutes
8. Back in Cloudflare: **Workers & Pages → your project → Custom Domains**
9. Add `arthagroupinc.com` and `www.arthagroupinc.com`

### Option B: Keep GoDaddy DNS (CNAME only)
If you want to keep GoDaddy as DNS manager:

1. In GoDaddy DNS, add a CNAME record:
   - **Name**: `@` or `www`
   - **Value**: `artha-website.pages.dev`
   - **TTL**: 1 hour
2. In Cloudflare Pages → your project → **Custom Domains** → add your domain

---

## STEP 5 — SSL / HTTPS (Automatic)

Cloudflare provides **free SSL** automatically. No setup needed.
Your site will be available at `https://arthagroupinc.com` within minutes.

---

## MAINTAINING THE SITE

### Making Updates
```bash
# Edit files in C:\Users\sangi\artha-website\
# Then push to GitHub — Cloudflare auto-deploys in ~30 seconds:

git add .
git commit -m "Update services section"
git push
```

### Update Text Content
- **Company info**: Edit `index.html` — search for placeholder text
- **Phone/email**: Replace `+1 (123) 456-7890` and `info@arthagroupinc.com`
- **Address**: Find `United States` in the contact section
- **Stats**: Edit the numbers in the `.stats-strip` section
- **Services**: Modify the `.service-card` blocks

### Add a Logo Image
1. Place your logo file in `C:\Users\sangi\artha-website\` (e.g., `logo.png`)
2. In `index.html`, replace the `.logo` anchor with:
   ```html
   <a href="#home" class="logo">
     <img src="logo.png" alt="Artha Group Inc." height="40" />
   </a>
   ```

### Add a Real Background Image to Hero
1. Add your image as `hero-bg.jpg` in the project root
2. In `css/styles.css`, find `.hero {` and add:
   ```css
   background-image: url('../hero-bg.jpg');
   background-size: cover;
   background-position: center;
   ```

---

## COSTS SUMMARY

| Service              | Cost         |
|---------------------|--------------|
| Cloudflare Pages    | **FREE**     |
| Cloudflare CDN/SSL  | **FREE**     |
| Formspree (contact) | **FREE** (50 submissions/mo) |
| GoDaddy Domain      | ~$15–20/year (already yours) |

**Total hosting cost: $0/month**

---

## ALTERNATIVES COMPARISON

| Platform         | Free Tier | Auto-Deploy | CDN | SSL | Custom Domain |
|-----------------|-----------|-------------|-----|-----|---------------|
| **Cloudflare Pages** | ✅ 500 builds/mo | ✅ | ✅ Global | ✅ | ✅ |
| Netlify          | ✅ 100GB/mo | ✅ | ✅ | ✅ | ✅ |
| Vercel           | ✅ 100GB/mo | ✅ | ✅ | ✅ | ✅ |
| GitHub Pages     | ✅ 1GB | Manual | ❌ | ✅ | ✅ |
| GoDaddy Hosting  | ❌ ~$6/mo | ❌ | Limited | ✅ | ✅ |
