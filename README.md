# shellback-studio-site

Static landing site for **Shellback Studio** (`shellbackstudio.xyz`), hosted on
GitHub Pages.

**Status: DEPLOYED 2026-09-11 — live at https://shellbackstudio.xyz (apex;
`www` redirects to apex), GitHub Pages from this repo. All verification gates
passed over HTTPS on deploy day. Enforce HTTPS ticked; domain ownership
verified via Google Search Console (DNS TXT) and Play Console same day.**

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

- [x] **1. Create the GitHub repo**
  In a browser: github.com → **New repository** → name `shellback-studio-site`,
  visibility **Public**. Do not initialize with a README or .gitignore — this
  directory already contains the files.

- [x] **2. Push this directory to the repo**
  From inside this directory (`C:\Users\pdxpu\dev\shellback-studio-site`):

  ```powershell
  git init
  git branch -M main
  git remote add origin <REPO_URL>   # e.g. https://github.com/pdxpuck/shellback-studio-site.git
  git push -u origin main
  ```

- [x] **3. Enable GitHub Pages**
  Repo → **Settings** → **Pages** → Source: **Deploy from a branch** → branch
  `main`, folder `/ (root)` → **Save**.

- [x] **4. Set the custom domain**
  Settings → **Pages** → **Custom domain**: `shellbackstudio.xyz` (apex) →
  **Save**. The `www` variant is covered too: once the `www` CNAME in step 5
  exists, `www.shellbackstudio.xyz` routes to the same Pages site (GitHub
  redirects it to the apex). GitHub auto-committed a CNAME file to this repo
  when the domain was saved (commit "Create CNAME"); pull before pushing so
  local and remote don't diverge.

- [x] **5. Add the Porkbun DNS records**
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

  (2026-09-11 field notes: Porkbun's current UI no longer displays a blank host
  as `@` — type `@` or the bare domain; both normalize to the apex. Apex parking
  is an ALIAS record to `pixie.porkbun.com` that MUST be deleted before the A
  records are accepted — an ALIAS and A records cannot coexist on the same host.
  There is no "URL Forward" tab in the current UI. Pre-existing mail-provider
  records (Purelymail CNAMEs, MX, TXT) coexist untouched.)

- [x] **6. Enforce HTTPS**
  After DNS propagates: repo → Settings → Pages → tick **Enforce HTTPS**.
  If the checkbox is greyed out, the TLS certificate is still issuing — wait a
  few minutes and reload the page. (Cert confirmed live 2026-09-11 — https
  fetches to apex and www both served valid TLS before the tick; the
  settings-page "provisioning" text lags reality.) (ticked 2026-09-11)

- [x] **7. Verification gates**
  - [x] `https://shellbackstudio.xyz` loads the landing page.
        (verified 2026-09-11)
  - [x] `https://shellbackstudio.xyz/8bells/privacy/` loads the privacy
        policy. **REQUIRED** — this URL is on the critical path: the 8 Bells
        Play Store closed test cannot publish until it is live.
        (verified 2026-09-11)
