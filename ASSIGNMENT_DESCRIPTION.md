# Counts — short description for Canvas

**Counts** is a bilingual (English/Norwegian) personal and household financial dashboard built with generative AI. It addresses a common problem in existing banking apps: automatically assigned spending categories are often wrong or incomplete. Users can import CSV statements from multiple bank accounts, compare combined spending with their personal share, and view spending by year, quarter or month. Expenses are grouped into Essential, Non-essential and Uncategorized, with detailed color-coded categories and graphs. Users can correct individual transactions and save merchant-wide rules, such as categorizing all transactions containing “KIWI” as groceries. The app also identifies potential transfers between imported accounts to reduce double-counting. It includes expandable transaction details and local JSON backup/restore.

**Technology:** self-contained HTML, CSS and JavaScript, using browser local storage. No live bank connection or cloud services. Demonstrated with fictional data.

**How AI was used:** brainstorming, defining the MVP, generating the code, improving the UI from user feedback, adding import and classification logic, and creating/tests debugging the working prototype.

**Final design iteration (v0.4):** Set both expense breakdown wheels to start with their largest slice at 12 o’clock and proceed counterclockwise, reverted both to consistent category legends, and added a bar/smoothed-line trend view. Monthly selection shows the previous four months, quarter selection shows the previous four quarters, and year selection shows the previous four years, with zero-filled missing periods. Automated tests and interactive browser checks were repeated after these revisions.

**Final localization iteration (v0.5):** Added an in-app EN/NO switch with a locally saved language preference. Core branding and the playful greeting “How ya goin' count” remain in English; the rest of the interface is translated into Norwegian Bokmål. Bilingual testing covered all five screens, the edit dialogs, responsive views and return switching.

**Real-data validation iteration (v0.7):** Testing with three real SpareBank 1 SMN transaction exports revealed a genuine bug: the initial CSV importer did not recognize the bank’s `Inn` and `Ut` column headings, so outgoing expenses were not included. The importer was corrected, expanded to multi-file selection with suggested account assignments, and improved to reconcile transfers using source and destination account identifiers. The real statements were not included in the source code, repository or demonstration materials.


Version 0.7 improvements: flexible expense type overrides independent of subcategory; separate high-level and subcategory transaction filters; repeatable account identification using a locally stored fingerprint for supported bank export formats; one full year of fictional demo data with seasonal and irregular spending; and NOK axis labels on spending charts. The three perspectives are Household, Me, and By account. The app does not make live bank connections.
