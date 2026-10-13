# Debt Freedom — GitHub Pages + Google Sheets

A single-page debt dashboard. GitHub Pages hosts `index.html`; a Google Apps Script attached to your spreadsheet reads and writes the data. There are no separate user accounts: both people use the same URL and shared password.

## Files
- `index.html` — upload this file to the root of your GitHub Pages repository.
- `Code.gs` — paste this into the Apps Script project attached to your Google Sheet.

## 1. Create the Google Sheet
1. On your iPhone, open https://sheets.google.com and create a blank spreadsheet named `Debt Freedom Data`.
2. From the spreadsheet, choose **Extensions → Apps Script**. If the mobile interface hides Extensions, use Safari's **Request Desktop Website** or do this step on a computer once.
3. In the Apps Script editor, replace the starter contents of `Code.gs` with the included `Code.gs`, then save.
4. In Apps Script, open **Project Settings** (gear icon), find **Script Properties**, and add:
   - Property: `APP_PASSWORD`
   - Value: a strong, unique shared password (at least 16 characters is recommended; do not reuse an important password).
5. Run no function manually; the web app will create `Accounts` and `Payments` tabs automatically the first time it connects. Approve Google's authorization prompts if shown.

## 2. Deploy the script as a web app
1. In Apps Script tap **Deploy → New deployment**.
2. Select the gear/type selector and choose **Web app**.
3. Set **Execute as** to **Me**.
4. Set **Who has access** to **Anyone** so the static GitHub Pages site can call it without a Google sign-in. This means the endpoint is internet-reachable; it checks the shared password on each request. Use a long unique password and don't store full card/account numbers in the sheet. The shared-password approach is simpler than Google sign-in but is not as strong as per-user authentication.
5. Tap **Deploy**, authorize if prompted, and copy the URL ending in `/exec`. Use the deployed URL, not the `/dev` test URL.

## 3. Publish the website with GitHub Pages
1. Create a repository on GitHub. Public or private repositories can be used for the website; note that a published GitHub Pages site is public to anyone with its URL and should not contain secrets.
2. Upload only `index.html` to the repository root.
3. Open **Settings → Pages**. Under build/deployment choose **Deploy from a branch**, select `main` and `/(root)`, then save.
4. Open the published Pages URL in Safari.
5. In the **Connect your Google Sheet** section, paste the Apps Script `/exec` URL and enter the same `APP_PASSWORD` you set in Script Properties. Tap **Connect and load data**.
6. Add your accounts and start logging payments. Repeat the URL/password setup on your wife's phone. Both will see the same spreadsheet data.

## 4. Backups and notes
- Use **Export JSON backup** periodically. **Import backup** replaces the Accounts and Payments tab contents with the backup.
- The app stores the URL and shared password in that browser's local storage so it can reconnect. Do not use this on a shared/public device.
- Payments and interest/fees are logged as transactions; current balances are updated on the Accounts tab. A manual balance correction can be done directly in the sheet, but it is best to also add a note in the payment history if you change a balance manually.
- Payoff dates are estimates using a simplified blended APR and a fixed monthly budget. Actual interest calculations, minimum payments, fees, and issuer posting dates can differ.
- The chart is a trend visualization based on recorded transaction history, not a bank feed.
- If you edit `Code.gs` after deployment, open **Deploy → Manage deployments → Edit**, choose a new version, and deploy the update.

## Troubleshooting
- **Incorrect shared password:** confirm the `APP_PASSWORD` Script Property matches the password typed into the website.
- **Connection timeout / could not reach Apps Script:** confirm the URL ends in `/exec`, and that the deployment is active and set to Anyone.
- **Google authorization:** the script runs as the spreadsheet owner, so that owner must authorize it.
- **Script changes not appearing:** update the existing deployment to a new version.
