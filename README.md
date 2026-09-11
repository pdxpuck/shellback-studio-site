# shellback-studio-site

Static landing site for **Shellback Studio** (`shellbackstudio.xyz`), hosted on
GitHub Pages.

**Status: v1 built per team brief (tagline "Crafting what's missing.", webfont
pair, umbrella scope) — pending user browser review → GitHub repo + Pages +
Porkbun DNS.**

> **History:** the increment-0 draft (inline-styled `index.html` +
> `8bells/privacy/index.html`) is preserved in git history.

> **Portable by design:** plain static files — vanilla HTML/CSS, no
> frameworks, no build step, no JavaScript. The only external assets are the
> Google Fonts CSS + woff2 files; system-fallback font stacks are declared, so
> the site still reads correctly with fonts blocked or offline. GitHub Pages
> hosts the site now; it can be moved to any static host later by copying this
> directory and pointing DNS at the new host.

## Design language

The 8 Bells instrument palette, shared by both pages:

| Token | Value | Use |
| --- | --- | --- |
| Field navy | `#081727` | Page background |
| Panel navy | `#0D2137` | Raised cards |
| Brass | `#C9A227` | Wordmark, headings, links |
| Brass hairline | `rgba(201, 162, 39, 0.45)` | Rules and card borders |
| Cream ink | `#F2F4F6` | Body text |
| Dial cream | `#EFE6D0` | Accents, card titles |

Fonts (Google Fonts CDN, with system fallbacks): **Playfair Display**
(wordmark, tagline, headings) and **Source Sans 3** (body). Mobile-first;
single column; hairline brass rules are the only ornament. The favicon is an
inline SVG data URI (brass bell on a navy circle). The privacy page is
written to read well on phones down to 360dp wide.

## Contents

| File | Purpose |
| --- | --- |
| `index.html` | Brand front door — wordmark, tagline, philosophy, "Things we've made", contact |
| `8bells/privacy/index.html` | Hosted privacy policy for 8 Bells (Play Store requirement) |
| `style.css` | Shared stylesheet — palette tokens, type, cards, chrome |

## Deployment runbook

Work top to bottom. Every step is a checkbox.

- [ ] **1. Create the GitHub repo**
  In a browser: github.com → **New repository** → name `shellback-studio-site`,
  visibility **Public**. Do not initialize with a README or .gitignore — this
  directory already contains the files.

- [ ] **2. Push this directory to the repo**
  From inside this directory (`C:\Users\pdxpu\dev\shellback-studio-site`):

  ```powershell
  git init
  git branch -M main
  git remote add origin <REPO_URL>   # e.g. https://github.com/pdxpuck/shellback-studio-site.git
  git push -u origin main
  ```

- [ ] **3. Enable GitHub Pages**
  Repo → **Settings** → **Pages** → Source: **Deploy from a branch** → branch
  `main`, folder `/ (root)` → **Save**.

- [ ] **4. Set the custom domain**
  Settings → **Pages** → **Custom domain**: `shellbackstudio.xyz` (apex) →
  **Save**. The `www` variant is covered too: once the `www` CNAME in step 5
  exists, `www.shellbackstudio.xyz` routes to the same Pages site (GitHub
  redirects it to the apex).

- [ ] **5. Add the Porkbun DNS records**
  At Porkbun (domain `shellbackstudio.xyz`), create exactly these records:

  | Type  | Host | Value                  |
  | ----- | ---- | ---------------------- |
  | A     | `@`  | `185.199.108.153`      |
  | A     | `@`  | `185.199.109.153`      |
  | A     | `@`  | `185.199.110.153`      |
  | A     | `@`  | `185.199.111.153`      |
  | CNAME | `www`| `pdxpuck.github.io.`   |

  (These are the canonical GitHub Pages addresses. If Porkbun pre-created
  parking or forwarding records on `@` or `www`, remove them so only these
  remain.)

- [ ] **6. Enforce HTTPS**
  After DNS propagates: repo → Settings → Pages → tick **Enforce HTTPS**.
  If the checkbox is greyed out, the TLS certificate is still issuing — wait a
  few minutes and reload the page.

- [ ] **7. Verification gates**
  - [ ] `https://shellbackstudio.xyz` loads the landing page.
  - [ ] `https://shellbackstudio.xyz/8bells/privacy/` loads the privacy
        policy. **REQUIRED** — this URL is on the critical path: the 8 Bells
        Play Store closed test cannot publish until it is live.
