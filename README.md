# Carebook

A responsive, private health-records site for GitHub Pages, backed by Supabase. After signing in, the owner can list, search, add, edit, and delete records in the `medications`, `lab_results`, and `visits` tables. Visitors who are not signed in see only the login screen and cannot read any data.

## One-time Supabase setup

1. **Create your login:** Supabase dashboard → **Authentication → Users → Add user → Create new user**. Enter your email and a strong password, and tick **Auto Confirm User**.
2. **Block other sign-ups:** **Authentication → Sign In / Providers** → turn off **Allow new users to sign up**.
3. **Create everything else:** **SQL Editor → New query**, paste the whole of `supabase-new-patient.sql`, then click **Run**. It creates the tables, the access rules, and the private `reports` storage bucket for PDFs and images. It adds no records.
4. **Connect the site:** put the project URL and the publishable key from **Project Settings → API** into `supabase-config.js`.
5. **Allow password resets:** **Authentication → URL Configuration** → set **Site URL** to the published site address, and add the same address under **Redirect URLs**.

Each row has a `user_id`, and the access rules only let the signed-in owner of a row read or change it. New records are stamped with your user automatically.

## How visit notes pull in Rx and Test

A visit note shows an **Rx** section built from medications and a **Test** section built from lab results. A record belongs to a visit when it shares the visit's key: the date plus the last three letters of the provider's name, lower-cased. A visit on `2026-10-13` with provider `Dr. Rahim Sharif` has the key `2026-10-13-rif`, and it collects every medication whose `start_date` and `prescriber` produce that key, and every lab result whose `test_date` and `provider` produce it. The key is shown on each section so mismatches are easy to spot.

## Supabase keys

`supabase-config.js` holds the project URL and publishable key. These are meant to be public; the access rules protect the data. Never put the database password or the secret (`service_role`) key in this site.

## Run and deploy

Because the app loads JavaScript modules, preview it over HTTP rather than opening `index.html` directly. Any local static HTTP server will work.

To publish with GitHub Pages:

1. Push the site files (`index.html`, `app.js`, `styles.css`, `supabase-config.js`, `README.md`) to a GitHub repository. Keep the CSV files out of the repository: the repo is public even though the site's data is not.
2. Open the repository's **Settings → Pages**.
3. Choose **Deploy from a branch**, select the branch and the root folder, then save.
4. Open the Pages URL after deployment completes and sign in.
5. In Supabase → **Authentication → URL Configuration**, set **Site URL** to your Pages URL.
