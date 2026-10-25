# Counts v0.7 demonstration guide

This demonstration must use fictional data. Do not include actual bank files, personal names, full account identifiers or transaction history in the recording, source repository or public hosting.

1. Start Counts with the fictional demo data; switch the language with EN / NO. Show the three reporting views: **Household**, **Me**, **By account**.
2. Show expense breakdowns. Change from October to a quarter or full year, then switch **Bar** / **Line**. Both charts display NOK on the y-axis.
3. Open **Transactions** and filter by **Essential**, then by a specific subcategory; switch to **Non-essential** and demonstrate the difference.
4. Edit an example transaction. Set **Essential → Subscriptions** (or **Non-essential → Healthcare**) and save. Confirm that totals update.
5. Open **Categories** and demonstrate a rule such as `KIWI` and a more specific rule such as `KIWI 482` to explain the override behavior.
6. Open **Import & backup** and show importing multiple *fictional* CSV statements together. Confirm the proposed account assignment. If a statement has a recognizable repeated account identifier, subsequent imports can be matched automatically.
7. Summarize testing: 12 fictional months, bank-format import validation, duplicate prevention, transfer matching and category overrides. Mention that all financial information stays in the browser. Avoid claiming perfect account identification or bank integration.

## For the Canvas submission

- GitHub repository with `index.html`, README, sample CSVs (fictional only), and tests.
- A working demonstration link or YouTube video according to assignment instructions.
- A brief description of the app and how generative AI was used to plan, code, debug, test and iterate.
