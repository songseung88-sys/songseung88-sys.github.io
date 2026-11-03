# no-safe-tobacco.com — Firebase Hosting deployment plan

Source of truth: `safe-tobacco/index.html` (unchanged public v0.6).

## Existing preview
- https://songseung88-sys.github.io/safe-tobacco/
- Hosted by GitHub Pages, separate from the intended Firebase custom domain.

## Firebase production setup (pending authenticated access)
1. Create a **dedicated Firebase project** for this website. Do not reuse the unrelated '취재를 종합하면' hosting target.
2. Enable standard **Firebase Hosting** (not Firebase App Hosting; the page is static).
3. Deploy this directory using `safe-tobacco/firebase.json`; hosting `public: "."` makes the `safe-tobacco/index.html` the site's root. Confirm the generated `*.web.app` preview before DNS cutover.
4. Create a limited-scope GitHub Actions Firebase service account, store its JSON as encrypted repository secret `FIREBASE_SERVICE_ACCOUNT_NO_SAFE_TOBACCO`. Never commit credentials.
5. Run `.github/workflows/deploy-no-safe-tobacco-firebase.yml` with the new Firebase project ID. This workflow is manual and scoped to `safe-tobacco/`, so changes to other websites cannot deploy by mistake.

## Domain ownership and DNS (pending domain purchase)
1. Purchase `no-safe-tobacco.com` at your chosen registrar.
2. In Firebase Hosting for the dedicated project, click **Add custom domain** and enter `no-safe-tobacco.com`.
3. Copy the exact verification TXT / DNS A or other records shown by the Firebase wizard to the registrar DNS manager. Do not make up verification tokens or use records from another site. Keep TXT verification records in DNS.
4. Add `www.no-safe-tobacco.com` in Firebase and redirect it to the apex domain or configure it to serve the same website.
5. Wait for Firebase to show **Connected** and a valid managed TLS certificate. Remove conflicting old A/AAAA/CNAME records. When using Cloudflare DNS, use DNS-only while setting up a new Firebase domain to avoid proxy/SSL validation issues.
6. Verify both HTTPS addresses, redirection, product search, mobile full-table display, and downloads.

Official instructions:
- https://firebase.google.com/docs/hosting/custom-domain
- https://firebase.google.com/docs/hosting/github-integration

## Current blockers
- Firebase project administration/deployment access is not connected in this chat.
- Domain ownership and DNS access are not yet available.
- The GitHub secret and Firebase project ID do not exist yet for this new site.

The application's user-facing wording and product measurements were not modified by these deployment preparations.
