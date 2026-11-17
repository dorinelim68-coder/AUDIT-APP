# Audit Testing Tracker

A single-page audit testing app for financial documents. It reads Excel or CSV files in the browser, runs automated audit tests, ranks the issues it finds by risk and priority, and links each one to SSA and SFRS(I) paragraphs and to the exact cell in the file.

## Features

- **Issue tracker**: risk category, risk level, amount, score (0–100) and priority (P1–P3), with status and notes.
- **Location of each issue**: file › sheet › row › cell. Click it to open the spreadsheet view with the cell highlighted.
- **SSA and SFRS(I) references**: each issue cites the paragraph and highlights the relevant line in the built-in reference library.
- **File uploader**: `.xlsx`, `.xlsm`, `.xls` and `.csv`. Files are parsed locally with SheetJS; nothing is uploaded to a server.
- **15 automated tests**: balance and footing checks, formula errors, duplicates, round sums, weekend and Singapore public holiday postings, cut-off, entries above performance materiality, missing descriptions and approvals, year-on-year fluctuations, abnormal balances, suspense accounts, related parties, going concern indicators and Benford's law.

- **Sign-in and user management**: users sign in with a username and password managed by Supabase Auth. Admins add users with an initial password, reset passwords, change roles and deactivate accounts from the **Users** tab. Everyone can change their own password.
- **Shared data in Supabase**: engagement settings, uploaded files, issues, statuses and notes are stored in Supabase, so the whole team works on the same engagement. Each issue records who added it and who last updated it.

Use **Copy issues as CSV** to move findings into working papers. If you used the earlier browser-only version, sign in on the same browser and select **Import to Supabase** to move that data across.

## Supabase backend

Project: `https://aurobpkxmppxwzsgnjtz.supabase.co`. The app uses the publishable key, which is safe to ship in the browser; row level security controls access.

| Table | Holds |
|---|---|
| `auth.users` | Supabase's built-in accounts and hashed passwords. Usernames sign in as `username@auditapp.local`. |
| `public.profiles` | Username, full name, role (`admin`, `reviewer`, `auditor`) and active flag. Created automatically for each new auth user. |
| `public.engagements` | Entity, year end, materiality, performance and trivial percentages, enabled tests, issue counter. |
| `public.audit_files` | Uploaded workbooks: sheet data, header row and column mapping, uploader. |
| `public.issues` | Every issue with code (`AT-001`…), risk, priority, status, notes, location, SSA and SFRS(I) references, creator and last editor. |

Admin actions run through database functions `admin_create_user`, `admin_set_password` and `admin_update_user`, which reject anyone who is not an active admin. Deactivating a user bans them in Supabase Auth and blocks their data access.

### Starting users

| Username | Role | Initial password |
|---|---|---|
| `priya` | Admin | `password` |
| `dan` | Reviewer | `password` |
| `marcus` | Auditor | `password` |

Change these passwords after the first sign-in.

## Project structure

```
index.html    The whole app (HTML, CSS and JavaScript)
vercel.json   Vercel settings: clean URLs and security headers
README.md     This file
```

There is no build step and no dependencies to install. SheetJS, supabase-js and Google Fonts load from public CDNs at runtime.

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
