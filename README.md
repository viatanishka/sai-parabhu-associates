# Sai Prabhu Associates — website

Public site: `index.html` · Admin panel: `admin.html` (open it as `yoursite/admin`)

```
index.html            the public website (city fly-through, services, projects, credentials, contact)
admin.html            the admin panel (login, enquiries, projects, credentials, client quotes)
assets/config.js      Supabase connection (public URL + publishable key only)
assets/logo.png       logo
supabase/setup.sql    database setup, run once in Supabase
netlify.toml          hosting settings
robots.txt            keeps the admin page out of Google
```

---

## Setup (do these in order)

### 1. Set up the database
1. Open **supabase.com/dashboard** → your project.
2. Left menu → **SQL Editor** → **New query**.
3. Open `supabase/setup.sql`, copy everything, paste it in.
4. Find the line marked `>>> ADMIN EMAIL <<<` and replace `CHANGE-ME@example.com` with the email that will log in to the admin panel.
5. Click **Run**. You should see "Success. No rows returned".

### 2. Create the admin login
1. Left menu → **Authentication** → **Users** → **Add user** → **Create new user**.
2. Use **the same email** as in step 1.4, choose a strong password, tick **Auto Confirm User**.
3. Click **Create user**.

### 3. Stop strangers from creating accounts
1. **Authentication** → **Sign In / Providers** (may be called **Providers** or **Settings**).
2. Turn **off** "Allow new users to sign up". Save.

(Even if someone did sign up, they could not edit anything, because only emails in the admin list can. This is a second lock.)

### 4. Put the code on GitHub
1. Open your repository on github.com.
2. Click **Add file** → **Upload files**.
3. Drag in **everything inside** this folder (index.html, admin.html, assets, supabase, netlify.toml, robots.txt, README.md). Keep the folders as they are.
4. Click **Commit changes**.

### 5. Put the site online with Netlify
1. **app.netlify.com** → **Add new site** (or **Add new project**) → **Import an existing project** → **GitHub**.
2. Choose the repository.
3. Build command: leave **empty**. Publish directory: **.** (a single dot).
4. Click **Deploy**. In a minute you get an address like `something.netlify.app`.
5. Optional: **Site configuration** → **Change site name** → `saiprabhuassociates` → the address becomes `saiprabhuassociates.netlify.app`.

### 6. Tell Supabase the site's address (needed for "Forgot password")
1. Supabase → **Authentication** → **URL Configuration**.
2. **Site URL**: your Netlify address, e.g. `https://saiprabhuassociates.netlify.app`
3. **Redirect URLs** → add `https://saiprabhuassociates.netlify.app/admin.html`
4. Save.

### 7. Test
1. Open the site, fill the contact form, press **Send on WhatsApp**.
2. Open `/admin`, sign in, check the enquiry is in the **Enquiries** tab.
3. Add one project with a photo and check it appears on the site.

---

## Everyday use (for the admin)

- **Enquiries**: every message from the contact form lands here, even if the WhatsApp message was never sent. Mark them *contacted* or *closed* to keep the inbox tidy.
- **Projects**: add a photo, name, type, place and year. Use ↑ ↓ to choose the order. *Hide* removes it from the site without deleting it.
- **Credentials**: upload degree / licence scans. A "for verification only" watermark is printed into the image itself before it is uploaded. Cover ID numbers, signatures and home addresses first.
- **Client quotes**: real words from real clients, with their permission. The Clients section appears on the site only when at least one quote exists.

## Security notes

- `assets/config.js` holds only the **publishable** key, which is designed to be public.
- **Never** put the `service_role` / secret key or the database password in any file here.
- What the public can and cannot do is enforced by the database itself (see `setup.sql`): visitors can read visible content and send enquiries; only emails in the `admins` table can change anything or read enquiries.
- To add a second admin later: SQL Editor → `insert into public.admins (email) values ('new@email.com');` and create that user in Authentication → Users.

## Changing text on the website

Text such as headings and service descriptions lives in `index.html`. Edit it on GitHub (open the file → pencil icon → edit → **Commit changes**); Netlify publishes the change automatically within a minute.
