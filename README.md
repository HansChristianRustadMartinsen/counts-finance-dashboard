# Counts — Accounts accounted for when it counts.

**Counts** is a small, local-first financial dashboard developed using generative AI as part of an introductory AI assignment. Its aim is to make spending categories correctable, explainable and reusable — including for households with several accounts.

The dashboard (v0.7) opens with **“How ya goin' count”** and uses fictional example transactions by default. v0.7 retains the distinct category colors and balanced demo transactions, places sorted category legends to the right of both donuts (below on narrow phones), and lays out donut slices counterclockwise starting with the largest at 12 o’clock. The time-series panel also offers bar and smoothed line views with period-dependent aggregation. A top-right **EN / NO** selector changes the interface language instantly, remembering the preference in the same browser. The name, slogan and dashboard greeting intentionally stay in English.


## What changed in v0.7

- **Transaction filtering:** select a spending type first (Essential / Non-essential / Uncategorized / Internal transfers), then optionally narrow to an individual subcategory. The two filters combine.
- **Flexible classification:** a category such as Subscriptions can be designated Essential or Non-essential for each individual transaction or merchant rule. Existing v0.6 data remains compatible; category defaults still apply where no override exists.
- **Account identification:** Norwegian statements that identify the source of outgoing transactions and destination of incoming payments can yield a reliable repeated account identifier. Counts remembers a *fingerprint* after the first confirmed import and suggests the same account for subsequent statements, even with generic filenames. Manual account selection is always available. An optional account-number field under **Accounts → Edit** enables users to associate an account when necessary; the full number is not kept in the app's persistent account settings. Identification never proves whether an account is checking, savings, household or mortgage, so its role must be confirmed initially. Matching uses a local, non-cryptographic fingerprint; it is an identifier, not a security guarantee.
- **Fictional demo:** 12 months, January–December 2026. Mortgage installments and common charges are fixed; electricity is higher during winter and groceries vary by less than 50% between the least and most expensive months. Leisure and retail purchases vary substantially.
- **Chart axes:** spending over time now displays amounts in NOK (`kr`) on both bar and line charts.
- **Navigation:** My share is renamed **Me** (Norwegian: **Meg**).

**Safety before updating:** export a JSON backup from your current Counts version before opening a new HTML version. Your existing browser data should continue to load, and the new fictional demo replaces previous example transactions only if you explicitly select *Reset to example data*. Never upload real banking CSVs or backups to GitHub.

## Running it

- **Open `Counts.html`** directly in a browser, or open `index.html`.
- For a local web server, open this folder in a terminal and run `python -m http.server 8000`, then visit `http://localhost:8000`.
- To host on GitHub Pages, upload `index.html` to the root of a public repository and enable **Settings → Pages → Deploy from a branch → main / (root)**. `index.html` contains the CSS and JavaScript already — no build system or API keys required.
- The application is a single-page browser prototype, not a native banking app, and has no automatic bank connection.

> **Important:** Browser data is tied to the website origin. Data stored when opening `Counts.html` locally will not automatically appear on a future GitHub Pages URL. Export a JSON backup and restore it in the new location if needed.

## Features

| Feature | How it works |
|---|---|
| Language selection | Switch between English and Norwegian Bokmål inside the same application. The UI language is stored separately from transaction data and merchant rules; custom user-entered names and bank records are not translated. |
| Multiple bank accounts | Add as many accounts as needed and label them Checking/current, Household, Savings, Mortgage/loan, Credit card, Investment, or Other. Account **type** is independent of **personal/shared ownership**. |
| Three views | **Household** shows all imported accounts at their full amount; **Me** weights each account by its configured percentage; **By account** displays one account at a time. |
| Time filters | Three linked selectors: **Year → Quarter → Month**, defaulting to the current month and year, plus an All years setting for all-time reports. The time-series chart shows the trailing **4 months**, **4 quarters** or **4 years** based on the chosen reporting interval; All years aggregates all recorded years. Missing periods display as zero. |
| Charts | Two descending-order expense breakdown donuts (largest sector starts at 12 o’clock, goes counterclockwise) and a spending trend chart with Bar/Line buttons. The line is smoothed for readability but plotted from actual period totals. |
| Expense categories | Essential, Non-essential and Uncategorized groups, plus more detailed categories including Groceries, Electricity, Mortgage, Transport and Shopping. |
| Category correction | Edit a transaction, then optionally remember the merchant. Any keyword or phrase (such as `KIWI`, `MENY`, or `Steam`) can match multiple transactions. A more specific rule such as `KIWI 482 Majorstuen` takes precedence over `KIWI`. Spending type and detailed category are selected separately. |
| Transaction details | Click a transaction row to expand the detail panel. Time, reference and post-transaction balance display **when those data fields are available** in the bank CSV. |
| Transfer matching | Conservatively matches opposite-signed transfers between accounts, using matching source/destination account identifiers (when exported by the bank) or clear transfer wording. Manual overrides remain available. |
| CSV bank exports | Import **multiple files at once**, with account suggestions based on filenames and per-file correction. Recognizes SpareBank 1 SMN `Inn` and `Ut` automatically, and handles semicolon, comma or tab delimiters. Advanced manual mapping stays available. |
| Data storage | Local browser storage and downloadable JSON backup/restore. No accounts, transactions or rules are transmitted to a server by this application. |

### Which financial files work?

The prototype accepts **CSV** transaction files with parseable dates and amounts. It's designed to support different banks through a manual mapping step, but **we cannot guarantee every bank's export format**. The importer does not directly accept PDFs, Excel workbooks, bank APIs, live feeds or multiple currencies. Files over 12 MB need to be broken into shorter periods. The app can **create new account profiles during the import**, rather than requiring accounts to be set up first. You may select up to 30 files per import batch.

For SpareBank 1 SMN, export each account as a separate CSV, select all the files in one browser file picker, review the **suggested account assignments**, then choose **Import all files**. For the filenames `Trekkonto.csv`, `Brukskonto.csv`, and `Felleskonto.csv`, Counts suggests Mortgage, Personal checking, and Shared household accounts respectively. These are filename guesses and can be changed before import.

### A word about categorization and transfers

- Imported **positive amounts** are generally treated as incoming funds, unless categorized as refunds/offsets. Negative amounts are generally treated as spending unless classified as transfers.
- Transfers are excluded **only** when marked by the user or conservatively paired across different imported accounts using opposing amounts within three days **and matching exported source/destination account identifiers**, or clear transfer-related wording. Ambiguous transfers still need review. To protect privacy, source/destination identifiers are reduced to non-cryptographic matching fingerprints in the saved transaction records; they are not displayed. These fingerprints should not be treated as secure encryption or anonymization.
- A transfer to savings is not spending when both sides have been imported and matched. If the destination account is missing, you may need to mark the outgoing transaction as a transfer manually.
- The **Me** view uses the configurable percentage for each account (e.g., shared household account = 50%, personal current account = 100%). It's an allocation estimate, not formal joint-accounting or tax advice.
- Mortgage installments are shown as cash outflow; principal and interest are not separated automatically.
- Duplicate detection uses an account-specific transaction fingerprint derived from date, description, amount and occurrence; unusual repeated identical transactions across partially overlapping exports may still need review.

## Testing

With Node.js 18+ installed:

```bash
node --test tests.mjs tests_language.mjs tests_batch_import.mjs
```

The included tests check translations and language switching in addition to the finance calculations. The finance tests check merchant-rule matching across multiple merchants, more-specific rule priority, Norwegian amount parsing, dates and transaction times, optional CSV metadata, month/quarter/year filters, transfer pairing, personal-share calculations individual category overrides, chronological trend aggregation and counterclockwise donut segment order.

Additional graphical browser testing used fictional bank CSVs and covered account creation, CSV importing, category updates, expandable details and mobile viewport rendering. **The multi-file importer was tested locally against three user-supplied SpareBank 1 SMN CSV exports**, covering 369 rows, with positive and negative amount mapping, account suggestions and duplicate re-imports. Real bank data was used only for testing and is **not included in the project files**. Other banks may still need column adjustments.

## Privacy

This prototype runs entirely in the browser: imported financial data is not sent to a backend or AI service. Browser local storage is **not encrypted**, and neither are downloaded JSON backups. Anyone with access to your browser profile or backup files may be able to view the stored data. Use fictional data for public demos and GitHub, and avoid publishing account numbers, statements, credentials or other private information.

If testing with real bank statements, do so in a private local copy and export a backup before changing browsers or site addresses.

## What's not included in the assignment MVP

- Live bank connectivity, PSD2 consent/authentication, passwords or API keys.
- Secure hosted accounts, encrypted cloud backups, multi-user sign-in or mobile app-store packaging.
- Trained AI models, automated merchant enrichment or complex transaction-line-item analysis.

## Project structure

- `index.html` — complete self-contained app, intended for GitHub Pages.
- `Counts.html` — standalone copy of the app for direct opening.
- `tests.mjs`, `tests_language.mjs`, `tests_batch_import.mjs` — Node.js regression tests (all use fictional sample data).
- `DEMO_GUIDE.md` — suggested demonstration and submission checklist.
- `ASSIGNMENT_DESCRIPTION.md` — short description for Canvas submission.
- `sample_personal.csv`, `sample_shared.csv`, and the three `Demo_*.csv` statements — **fictional** sample data for safely showing CSV import, account matching and transfer reconciliation.

## How generative AI was used

The human project owner defined the problem, bank-account perspectives, category rules, visual hierarchy, branding, account type flexibility and design refinements in plain English. ChatGPT generated the first implementation, revised the code iteratively from feedback, identified and fixed a responsive-layout issue, generated automated unit tests and exercised a functional browser workflow with synthetic transaction statements. The project reflects an AI-assisted iterative development process, rather than a hand-coded project requiring prior web development experience.

## v0.4 interface refinement

- Changed both donut diagrams to a consistent legend at the right of the wheel (stacked below at narrow phone widths). The largest slice starts at 12 o’clock and runs counterclockwise.
- Spending over time is no longer limited to monthly data regardless of selection. Month → trailing 4 monthly buckets; Quarter → trailing 4 quarters; Year → trailing 4 years; All years → one bucket per recorded year. Zero-transaction periods stay visible.
- Added an accessible **Bar / Line** switch. The line uses smooth interpolation for visual trends; dot tooltips show the exact underlying totals. The chosen mode persists as you navigate views in the open browser tab.

## Counts v0.5 — language support

- Switch between **EN** and **NO** from the top-right of any screen. Both languages are included in the same HTML file, and the preference is stored locally under `counts_ui_language`.
- The **counts.** name, **Accounts accounted for when it counts.** tagline, and **How ya goin' count** greeting intentionally remain in English. Navigation, dates, charts, categories, account settings, transaction editor, importer and notifications are localized to Norwegian Bokmål.
- Interface translation does **not** change saved category IDs, merchant-matching rules, imported statement content or financial calculations.
- Browser smoke checks included switching each page and both edit dialogues to Norwegian and back, with no JavaScript errors or horizontal overflow on a 390px-wide viewport. The browser test environment restricted direct access to real file/HTTP origins, so persisted settings were also checked with a simulated localStorage object.

## Counts v0.6 — real-bank import correction

- Corrected automatic mapping for **SpareBank 1 SMN** columns `Inn` (money in) and `Ut` (money out). Previously the app could import only positive amounts, leaving spending charts empty.
- CSV picker accepts multiple files simultaneously; each preview card shows a suggested destination account, parsed counts and incoming/outgoing amounts. Advanced mapping remains optional.
- Account names are suggested from file names, but can be overridden. Unknown account names can be created during the same import, including savings or other supported account types.
- CSV source/destination account columns can be used to reconcile transfers when text alone is ambiguous. Existing duplicate transactions can be enriched with additional bank metadata on re-import without overwriting category corrections.
- Confirmed 369 rows across three actual SpareBank 1 SMN exports in local browser testing. **No private bank statements or screenshots were packaged or published.**

### If you already imported files with a previous version

If the earlier import went into the correct account, simply import those same CSVs again in v0.6; duplicate entries are skipped and missing fields may be filled. If an earlier file was assigned to the **wrong account**, export a JSON backup, then clear the transactions in **Import & backup** and re-import with correct account assignments. Clearing the transactions does not delete your merchant categorization rules.
