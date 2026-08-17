# Healthspring EDS Demo — EMA Setup Runbook

This project is a **ready-to-connect target** for the Experience Modernization Agent (EMA).
The local scaffold is done. The remaining steps are **account-gated** (they need your GitHub /
Adobe / da.live logins), so run them yourself in order.

**Target identity**
- GitHub owner: `nkettler-adobe`
- Repo: `healthspring-demo`
- Content source: Document Authoring — `https://content.da.live/nkettler-adobe/healthspring-demo/`
- Preview URL (EMA needs this): `https://main--healthspring-demo--nkettler-adobe.aem.page/`
- Source site EMA will import: `https://www.healthspring.com/`

---

## 1. Create the GitHub repo (installs AEM Code Sync automatically)

Use the GitHub **template** flow — it creates the repo *and* triggers AEM Code Sync in one step
(a plain `git push` does **not** install Code Sync).

1. Go to https://github.com/adobe/aem-boilerplate → **Use this template** → **Create a new repository**.
2. Owner: `nkettler-adobe` · Repository name: `healthspring-demo` · Public.
3. Create the repository.

## 2. Push this local scaffold over the fresh repo

This folder already has an initial commit on `main`. Point it at your new repo and push (this
overwrites the template default with the DA `fstab.yaml` + Healthspring branding):

```bash
cd /Users/nkettler/Documents/AEM/Claude/healthspring-demo
git remote add origin https://github.com/nkettler-adobe/healthspring-demo.git
git push -u --force origin main
```

## 3. Install the two required GitHub Apps on the repo

- **AEM Code Sync** — https://github.com/apps/aem-code-sync (already installed if step 1 used the template; confirm it lists `healthspring-demo`).
- **AEM Code Connector** — install and grant access to `healthspring-demo`. This is what lets the EMA console inspect the repo.

## 4. Set up the Document Authoring (da.live) content source

1. Open https://da.live and sign in with your Adobe ID.
2. Create the org/site so it resolves at `https://da.live/#/nkettler-adobe/healthspring-demo`
   (matches the `fstab.yaml` mountpoint in this repo).
3. You don't need to author any pages — EMA writes the imported Healthspring content here.

## 5. Verify the preview URL is live

After Code Sync runs, this should resolve (a 404 body is fine — it means routing works, content is just empty):

```bash
curl -I https://main--healthspring-demo--nkettler-adobe.aem.page/
```

## 6. Connect the project in EMA and import Healthspring

1. Go to **https://aemcoder.adobe.io** and sign in with your Adobe ID.
2. Click **Switch site** to exit demo mode.
3. Authorize **AEM Code Connector** with GitHub when prompted.
4. Paste the Preview URL: `https://main--healthspring-demo--nkettler-adobe.aem.page/`
5. Click **Checkout to workspace**.
6. Prompt EMA to build the demo, e.g.:
   - `Catalog the templates and blocks on https://www.healthspring.com/`
   - `Migrate the page https://www.healthspring.com/` (imports content into DA)
   - `Import the design/styles from https://www.healthspring.com/`
7. Preview, then submit the change via the GitHub workflow for review.

---

## Local development (optional, to preview during the demo)

```bash
cd /Users/nkettler/Documents/AEM/Claude/healthspring-demo
npm install
aem up      # opens http://localhost:3000, proxying DA content via fstab.yaml
```

## What was pre-configured in this scaffold

- `fstab.yaml` → DA mountpoint for `nkettler-adobe/healthspring-demo`.
- `styles/styles.css` → link colors set to a Healthspring purple placeholder (EMA refines this on design import).
- `package.json` → renamed to `healthspring-demo`.
- Clean lint, initial git commit on `main`.
