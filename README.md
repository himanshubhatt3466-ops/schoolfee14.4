# ICON PUBLIC SCHOOL — School + Bus Fee Manager (GitHub Pages)

A no-Node.js, static web application starter for a single institution. It runs in a modern browser and stores records locally in IndexedDB.

## Features included
- ICON Public School logo and fixed school branding on login, sidebar and printed fee receipts
- Admin login (first-use credentials: `admin` / `admin123`)
- School student records
- Class, section, session and transport allocation
- Bus / route / driver / stops register
- Fee obligations with due dates and concessions
- Payment collection, receipt numbers and printable receipts
- Paid / partial / unpaid status for configured obligations
- Custom overdue fine configured in Settings
- Dashboard, collection chart, reports and search
- JSON full backup/restore
- CSV student import and student/payment export
- Optional Google Apps Script receiver for Google Sheets backup log + Drive JSON snapshots

## GitHub Pages setup
1. Create a new GitHub repository, e.g. `school-bus-fee-manager`.
2. Upload all files and folders from this ZIP to the repository root.
3. Open **Settings → Pages**.
4. Under **Build and deployment**, choose **Deploy from a branch**.
5. Select branch `main` and folder `/(root)`, then Save.
6. Wait for the Pages URL to appear. Open it in Chrome or Edge.
7. Sign in using `admin` / `admin123`, then immediately change the password in **Settings**.

No Node.js, build command, package manager or server is required.

## Important storage / 100,000 student note
- IndexedDB is used rather than localStorage. It is designed for larger browser datasets.
- Data belongs to that browser profile on that computer. It is not automatically shared with other computers or browsers.
- Browser storage can be cleared or evicted. Download JSON backups regularly.
- This project uses limited table rendering (first 1,500 students / 2,000 payment rows in views) to avoid freezing the browser. It does not mean the database is limited to those numbers.
- 100,000-record performance must be load-tested on the target PC with realistic fee/payment volumes before production use.
- GitHub Pages is static hosting. For multiple computers sharing live data, secure user accounts, central backup and concurrent access, add a real backend/database. Do not expose database credentials or privileged API keys in frontend JavaScript.

## Fee workflow
1. Add students in **Students**. Enter the student class, section, session and optional bus details.
2. For bus users, tick **Uses school bus** and fill route/stop.
3. Add fee obligations from School Fees or Bus Management. Enter amount, due date and discount.
4. Use **Collect fee** or Collect beside an obligation. Payment is linked to the obligation.
5. The ledger computes Paid / Partial / Unpaid and balance. The configured custom late fine is added to an overdue unpaid balance.
6. Use Payments & Receipts to reprint a receipt.

A fee obligation is a fee that has been formally assigned to a student. The application cannot calculate genuine pending dues until those obligations are entered/imported.

## Google Sheets + Google Drive backup setup (optional)
This project includes `google-apps-script/Code.gs` as a starter receiver:
1. Create a Google Sheet and a Drive folder for backups.
2. Open `script.google.com`, create a project and paste `Code.gs`.
3. Set `SPREADSHEET_ID` and `BACKUP_FOLDER_ID` in the script.
4. Deploy → New deployment → Web app. Choose **Execute as: Me** and an access setting appropriate to your account and organization's policy.
5. Copy the Web App URL and paste it into **Backup & Import → Google Sheets / Drive connector**.
6. Click **Send backup to Sheets** and verify that a JSON file appears in Drive and a row appears in the Backup Log sheet.

Security warning: the sample receiver is a simple starter endpoint. Depending on deployment access, it may allow anyone with the URL to submit data. Do not use it for sensitive live student data until you add authentication, access controls, validation, retention policy and appropriate privacy safeguards. Do not put secrets in the GitHub repository. Test with dummy records first.

## Known limitations / production checklist
This is a functional starter, not a certified production ERP:
- Browser-only login is a convenience gate, not secure server-side authentication. Anyone with browser developer tools can inspect or modify local data.
- No multi-user sync or role system; the requested admin-only mode is implemented.
- No automatic recurring fee generation. Add fee obligations for each billing period.
- No automated fine waiver, receipt cancellation, audit trail, bank reconciliation or statutory accounting.
- CSV import currently imports student records. Use JSON backup/restore for all record types.
- For production: add backend authentication, server-side validation, encrypted backups, audit logs, restore testing, and formal UAT.

## Files
- `index.html` — app shell and forms
- `icon-public-school-logo.png` — supplied ICON Public School logo used in the app and receipts
- `style.css` — responsive light theme
- `app.js` — IndexedDB, fee logic, UI, receipts and data transfer
- `google-apps-script/Code.gs` — optional Google backup receiver
