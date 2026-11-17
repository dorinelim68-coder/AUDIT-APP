# Audit Testing Tracker

A single-page audit testing app for financial documents. It reads Excel or CSV files in the browser, runs automated audit tests, ranks the issues it finds by risk and priority, and links each one to SSA and SFRS(I) paragraphs and to the exact cell in the file.

## Features

- **Issue tracker**: risk category, risk level, amount, score (0–100) and priority (P1–P3), with status and notes.
- **Location of each issue**: file › sheet › row › cell. Click it to open the spreadsheet view with the cell highlighted.
- **SSA and SFRS(I) references**: each issue cites the paragraph and highlights the relevant line in the built-in reference library.
- **File uploader**: `.xlsx`, `.xlsm`, `.xls` and `.csv`. Files are parsed locally with SheetJS; nothing is uploaded to a server.
- **15 automated tests**: balance and footing checks, formula errors, duplicates, round sums, weekend and Singapore public holiday postings, cut-off, entries above performance materiality, missing descriptions and approvals, year-on-year fluctuations, abnormal balances, suspense accounts, related parties, going concern indicators and Benford's law.

Data is saved in the browser's `localStorage`, so it stays on one device and one browser. Use **Copy issues as CSV** to move findings into working papers.

## Project structure

```
index.html    The whole app (HTML, CSS and JavaScript)
vercel.json   Vercel settings: clean URLs and security headers
README.md     This file
```

There is no build step and no dependencies to install. SheetJS and Google Fonts load from public CDNs at runtime.

## Deploy to Vercel

1. Sign in at [vercel.com](https://vercel.com) and choose **Add New… → Project**.
2. Import the `AUDIT-APP` GitHub repository.
3. Leave the settings as they are:
   - Framework Preset: **Other**
   - Build Command: *(empty)*
   - Output Directory: *(empty, the repository root)*
   - Install Command: *(empty)*
4. Select **Deploy**. Every push to `main` redeploys automatically.

## Run locally

Open `index.html` in a browser, or serve the folder:

```bash
npx serve .
```

## Standards note

Paragraph references for SSA 240 and SSA 570 follow the versions in force for periods beginning before 15 December 2026. SSA 240 (Revised) and SSA 570 (Revised 2024) apply after that date and renumber their paragraphs. SFRS(I) 18 replaces SFRS(I) 1-1 for annual periods beginning on or after 1 January 2027. The summaries in the app are paraphrased; check the full standards from ISCA and the Accounting Standards Committee before relying on them.
