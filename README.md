# README — Okta Clean CSV (DevTools Snippets)

## What it is

Two Chrome DevTools **Snippets** that export a clean, import-ready CSV from the Okta Admin UI. Columns:

* `first_name` ← `profile.firstName`
* `last_name`  ← `profile.lastName`
* `employee_id` ← `id`
* `onboard_time` ← `created`
* `email` ← `profile.email`

## Why this works

* Runs in your logged-in Admin tab → same-origin requests (no CORS).
* Uses `/api/v1/users` and follows the `Link: rel="next"` header to paginate.
* No extensions, no external tools, no jq.

## Install & Run

1. Open **Okta Admin** (`…-admin.okta.com`) and log in.
2. Press **F12** → **Sources** → **Snippets**.
3. Create snippet **ALL USERS → CSV** and paste the “All Users” code.
4. (Optional) Create snippet **ONLY ACTIVE → CSV** with the second code.
5. Right-click a snippet → **Run** (or open the snippet and click ▶).
6. A file named `import_CSV_template.csv` downloads automatically.

## Customizing

* Change the header row:
  `const rows = ['first_name,last_name,employee_id,onboard_time,email'];`
* Update the mapping inside the loop to add/remove fields, e.g.:

  ```js
  rows.push([
    u?.profile?.firstName,
    u?.profile?.lastName,
    u?.id,
    u?.created,
    u?.profile?.email,
    u?.profile?.department   // ← example extra column
  ].map(esc).join(','));
  ```

## Troubleshooting

* **No download**: make sure you’re on the **Admin** domain (same origin as `/api/v1/users`).
* **CORS error**: you’re likely on a different host; run the snippet on `…-admin.okta.com`.
* **Quote/commas in names**: handled by CSV escaping (`"` doubled).
* **Too many users**: pagination is automatic; just let it finish.

## Security

* Uses your existing session; no tokens are exposed or stored.
* Only reads from `/api/v1/users` and creates a local CSV blob in your browser.

Move over Mrs Frizzle — one click to go from wild JSON to a neat class roll.
