# Premium Fragrance MNR — Android setup (free-tier deployment)

This package contains:
- `index.html` — installable-style web app frontend, Supabase login, shared sales/stock/expenses and CSV exports.
- `supabase_setup.sql` — cloud database schema and access rules.
- `manifest.webmanifest` + `icon.svg` — app name/icon metadata.
- `SETUP-ANDROID.md` — this guide.

## Before starting
You will need a Google account, a Supabase account, and a GitHub account. Free tiers are suitable to start, but provider limits and terms can change. This is a PWA-style website, not a Play Store APK. Hosting and cloud records require internet access.

## Part A — Create the cloud database (Supabase)
1. On your Android phone, open Chrome and go to https://supabase.com. Sign up and create a project named `premium-fragrance-mnr`. Choose a strong database password and keep it private. Wait until the project is ready.
2. In the project dashboard, open **SQL Editor** → **New query**.
3. Open `supabase_setup.sql` from this package (you can view it in a text editor) and copy all SQL into the editor. Run it.
4. In the SQL Editor, create a new query containing the final `insert into public.allowed_members...` example from the bottom of the SQL file, replacing the two example placeholders with the exact email addresses Jeewan and Rijan will use to sign in. Run it. Do not use the same login for both partners.
5. In Supabase, open **Project Settings** → **API** (or **Data API / API Keys**, depending on dashboard version). Copy the **Project URL** and the **publishable key** (or legacy `anon` public key). Never copy a `service_role` or secret key into the website.
6. Optional recommended account setting: under **Authentication** settings, leave email confirmation enabled for better security. Both partners should confirm their email. If sign-up confirmation is enabled, the first sign-up may return to the login screen until the email is confirmed.

## Part B — Connect the app to Supabase
1. Open `index.html` in a text editor that can edit HTML (for example, an Android code editor). Find these lines near the bottom:
   `const SUPABASE_URL="PASTE_SUPABASE_PROJECT_URL_HERE";`
   `const SUPABASE_ANON_KEY="PASTE_SUPABASE_PUBLISHABLE_OR_ANON_KEY_HERE";`
2. Replace the first placeholder with the exact Project URL and the second with the publishable/anon key. Keep the quotation marks. Do not put your database password or `service_role` key here.
3. Save the file.

## Part C — Publish the website free with GitHub Pages
1. In Chrome, open https://github.com and create/sign into an account.
2. Create a new repository named `premium-fragrance-mnr`. For the easiest setup, you may set it **Public** (the frontend contains only the public/anon key; database access is protected by Supabase Row Level Security). Do not upload any secret/service-role keys.
3. Use **Add file** → **Upload files** and upload `index.html`, `manifest.webmanifest`, and `icon.svg` from this package. Commit the files.
4. Open repository **Settings** → **Pages**. Under Build and deployment, choose **Deploy from a branch**, select `main` and `/(root)`, then Save.
5. Wait a few minutes. GitHub Pages will show a website URL, usually `https://YOUR-USERNAME.github.io/premium-fragrance-mnr/`. Open that exact URL on both phones.
6. If you edit the app later, upload/commit the updated files and allow Pages to redeploy.

## Part D — Create each partner's login
1. On the live website, Jeewan enters his own email and a strong password and taps **Create account**.
2. Rijan does the same using his own approved email address. Confirm emails if asked.
3. Each partner signs in with their own account. Only the exact emails added to `allowed_members` can read or edit the shared business data. Other signed-in users cannot read or change sales, stock or expenses because the database policies check membership.
4. Test with one sample sale and one stock entry. Confirm the same records appear on both phones before adding real business history.

## Part E — Install as a home-screen app on Android
1. Open the deployed HTTPS website URL in Chrome.
2. Tap Chrome's three-dot menu (⋮).
3. Choose **Install app** if offered, or **Add to Home screen** → **Install/Add**.
4. Rename it `Premium Fragrance MNR` if prompted. Repeat on Rijan's phone.
5. The app icon opens the shared website. A network connection is required for login and cloud updates.

## Important data and security notes
- The Supabase publishable/anon key is intended to be used in a browser; **Row Level Security (RLS)** is what restricts access. Never publish a `service_role` key, database password, or partner passwords.
- Keep the allowed-members list limited to the two partners. Anyone whose email is added there has equal read/write/delete access to all business records.
- Sales profit is automatically computed by the database as sold price minus invested cost. Dashboard profit subtracts all recorded expenses; for accurate accounting, avoid double-counting delivery or other expenses in both invested cost and expenses.
- The current stock payment status is for the whole purchase entry, and the partner split is an equal calculation; it does not yet track a separate paid amount for each partner.
- Records sync between devices when online. Export CSVs periodically. A free service may pause or have usage limits; check the providers' current terms.
- If you don't have a computer, GitHub's mobile website can upload files through the browser, but editing code on a phone can be awkward. If needed, use an Android text/code editor. Do not paste database secrets into public chat or a public repository.
