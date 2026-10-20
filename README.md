# Daily Cup — Coffee Shop Manager (PWA starter)

A mobile-friendly installable web app for daily sales, item-wise sales, purchases, recurring/one-off expenses, records, CSV export, and JSON backup. The interface works in local mode without an account. To share data between the owner and staff phones, configure Firebase as described below.

## Included
- Sales with optional item/quantity/unit price lines and cash/UPI split
- Purchases and operating expenses with editable categories
- Today dashboard and current-month cash-based surplus
- Date/type filters, CSV export, JSON backup/restore
- Import of the existing wide-layout CSV ledger (`DATE, 3/1/2026, ...`)
- Installable PWA and basic app-shell caching
- Firebase email/password sign-in and Firestore sync (optional)

## Run locally
Because ES modules and the service worker need an HTTP origin, don't open `index.html` directly. From this folder run:

```bash
python -m http.server 8080
```

Then open `http://localhost:8080`. Local mode stores data in that browser only. It does not sync to other devices.

## Enable shared free cloud sync (Firebase)
1. Create a project at https://console.firebase.google.com/.
2. Add a Web app in Project settings and copy its config values into `firebase-config.js`.
3. In Authentication → Sign-in method, enable **Email/Password**.
4. In Firestore Database, create a database. Open Rules and publish the contents of `firestore.rules`.
5. Host this folder over HTTPS. Firebase Hosting has a no-cost Spark plan; in the Firebase console, set up Hosting and follow the CLI instructions. Alternatively use another static host.
6. Open the hosted app, create the owner account, then create/sign in staff accounts on their devices. This starter uses one shared shop collection per Firebase project; all signed-in accounts can read/write all shop records.

### Important security note
This is a starter app. Anyone who can create an account in this Firebase project can access the shop's shared records under the included rules. Restrict account creation/invites before using it with real business data; for production, add owner-controlled staff invitations and role-based Firestore rules. Do not put private service-account credentials in this frontend. Firebase web config is intended to be public; Firestore security rules protect data.

## Import your existing CSV
In the app choose **Import existing ledger CSV** and select your existing CSV export. The importer recognizes the current wide layout with a first column named `DATE`, date columns across the top, an `INCOME` section with `Online`/`Offline` rows, and amount cells by date. It classifies obvious recurring bills/salary/rent/utilities as expenses and other top-section items as purchases. Please review imported rows: the original ledger's category placement may not distinguish every purchase from every operating expense perfectly. Keep your original file as the authoritative backup until the totals are reconciled.

## Accounting notes and limitations
- Monthly cash surplus = sales − stock purchases − other expenses. This is a cash-flow estimate, not formal profit; inventory purchased but not yet consumed can distort it.
- Item-wise sale lines are captured, but they do not automatically calculate the sales total unless quantities and unit prices are filled in.
- Recurring expenses are recorded as entries; automatic monthly bill generation, reminders, stock levels, receivables, and detailed role permissions are not yet implemented.
- Back up regularly using **Backup all data** and keep a copy outside the phone.
