# Field-Trip-only-the-fields-you-want-
Move over Mrs Frizzle — this tiny tool takes your wild Okta field trip and turns it into a tidy class roll. Click once, and boom: a clean CSV with first_name, last_name, employee_id, onboard_time, and email. No jq. No copy-paste. Just a smooth ride from messy JSON to import-ready magic. 🚌✨

README — CleanSheet Machine

CleanSheet Machine is a zero-install bookmarklet that exports a clean, import-ready CSV from the Okta Admin UI. It auto-paginates, stays same-origin (no CORS issues), and maps:

first_name ← profile.firstName

last_name ← profile.lastName

employee_id ← id

onboard_time ← created

email ← profile.email

What you get

A downloaded file named import_CSV_template.csv with all users (or only ACTIVE users, if you use the second bookmarklet below).

Setup

In Chrome, open the Bookmarks Bar (View → Always Show Bookmarks Bar).

Create a new bookmark (right-click bar → Add page…).

Name it CleanSheet Machine.

Paste the code below into the URL field.

Save.

Bookmarklet — All Users → CSV

Paste this entire line into the bookmark URL:

javascript:(()=>{let u=new URL('/api/v1/users?limit=200',location.origin),esc=v=>'"'+String(v??'').replace(/"/g,'""')+'"',rows=['first_name,last_name,employee_id,onboard_time,email'];(async()=>{try{while(u){const r=await fetch(u.toString(),{credentials:'include'});if(!r.ok)throw new Error('HTTP '+r.status);const d=await r.json();for(const x of d){rows.push([x?.profile?.firstName,x?.profile?.lastName,x?.id,x?.created,x?.profile?.email].map(esc).join(','))}const L=r.headers.get('link'),m=L&&/<([^>]+)>\s*;\s*rel="next"/i.exec(L);if(m&&m[1]){const n=new URL(m[1],location.origin);n.protocol=location.protocol;n.host=location.host;u=n}else u=null}const b=new Blob([rows.join('\n')],{type:'text/csv'}),a=document.createElement('a');a.href=URL.createObjectURL(b);a.download='import_CSV_template.csv';document.body.appendChild(a);a.click();a.remove()}catch(e){alert('Export failed: '+e.message)}})();})();

Optional Bookmarklet — ACTIVE Users Only
javascript:(()=>{let u=new URL('/api/v1/users?limit=200&filter=status%20eq%20%22ACTIVE%22',location.origin),esc=v=>'"'+String(v??'').replace(/"/g,'""')+'"',rows=['first_name,last_name,employee_id,onboard_time,email'];(async()=>{try{while(u){const r=await fetch(u.toString(),{credentials:'include'});if(!r.ok)throw new Error('HTTP '+r.status);const d=await r.json();for(const x of d){rows.push([x?.profile?.firstName,x?.profile?.lastName,x?.id,x?.created,x?.profile?.email].map(esc).join(','))}const L=r.headers.get('link'),m=L&&/<([^>]+)>\s*;\s*rel="next"/i.exec(L);if(m&&m[1]){const n=new URL(m[1],location.origin);n.protocol=location.protocol;n.host=location.host;u=n}else u=null}const b=new Blob([rows.join('\n')],{type:'text/csv'}),a=document.createElement('a');a.href=URL.createObjectURL(b);a.download='import_CSV_template.csv';document.body.appendChild(a);a.click();a.remove()}catch(e){alert('Export failed: '+e.message)}})();})();

How to use

Log into your Okta Admin site (e.g., boom-admin.okta.com).

Navigate to any admin page (staying on the admin domain).

Click the CleanSheet Machine bookmarklet.

Your CSV downloads automatically.

Customizing columns

Edit the header array: ['first_name,last_name,employee_id,onboard_time,email']

Update the corresponding mapping inside the loop to include or remove fields:

[x?.profile?.firstName, x?.profile?.lastName, x?.id, x?.created, x?.profile?.email]

Troubleshooting

Nothing downloads? Ensure you’re on the Okta Admin domain where /api/v1/users is same-origin.

CORS error? You’re likely on a different host; run the bookmarklet on the admin domain.

Need fewer users? Change the query to include filters (e.g., status eq "ACTIVE").

Encoding/quotes in names? The bookmarklet escapes quotes for CSV safety.

Security

Runs in your browser using your existing Okta session.

No tokens are copied, nothing is sent to third parties.

Only reads /api/v1/users and saves a local CSV.

Move over Mrs Frizzle — your Okta field trip just got way easier. 🚌📒
