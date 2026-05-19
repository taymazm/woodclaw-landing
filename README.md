# WoodClaw landing page

Static single-page site for [woodclaw.io](https://woodclaw.io). Hosted on
GitHub Pages with a custom domain.

## Files

- `index.html` — the entire page (inline CSS, no build step, no JS).
- `CNAME` — tells GitHub Pages that the custom domain is `woodclaw.io`.
- `README.md` — this file.

## Local preview

Open `index.html` in any browser:

```bash
open index.html
```

Or serve it locally so relative paths work like in production:

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

## Deploy to GitHub Pages

1. Create a new GitHub repo named `woodclaw-landing` (public).
2. Push this directory:

   ```bash
   cd /Users/murattaymaz/Documents/woodclaw-landing
   git init
   git add .
   git commit -m "Initial landing page"
   git branch -M main
   git remote add origin https://github.com/<your-username>/woodclaw-landing.git
   git push -u origin main
   ```

3. In the GitHub repo: **Settings → Pages**:
   - Source: **Deploy from a branch**
   - Branch: `main` / `/ (root)` → Save
4. Wait ~1 min. Confirm it's live at
   `https://<your-username>.github.io/woodclaw-landing/`.

## Connect the custom domain (woodclaw.io)

The `CNAME` file in this repo already tells GitHub Pages your domain.
You need to point DNS at GitHub:

1. Log in to your domain registrar (where you registered `woodclaw.io`).
2. Add these DNS records on `woodclaw.io`:

   **A records (apex domain → GitHub Pages):**
   ```
   A  @  185.199.108.153
   A  @  185.199.109.153
   A  @  185.199.110.153
   A  @  185.199.111.153
   ```

   **CNAME (www subdomain):**
   ```
   CNAME  www  <your-username>.github.io
   ```

3. Back in GitHub **Settings → Pages**:
   - Custom domain: `woodclaw.io` → Save
   - Wait for DNS check to pass (a few minutes to a few hours).
   - Tick **Enforce HTTPS** once GitHub issues the cert.

## After deploying

Update two things in `index.html`:

- Replace `https://huggingface.co/spaces/your-username/woodclaw` with the
  real Hugging Face Spaces URL once the Streamlit app is deployed.
- (Optional) Set up `hello@woodclaw.io` email forwarding at your registrar
  if you want the `mailto:` to work.
