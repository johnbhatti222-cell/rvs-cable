# Rajeev Video Cable website

Files in this folder:

| File | What it is |
|---|---|
| `index.html` | The whole website, including the chatbot and the admin page (`/#admin`) |
| `logo.png` | Browser tab icon |
| `Code.gs` | Google Apps Script that saves requests into your Google Sheet and stores the admin edits |

Requests go into the Google Sheet **"Rajeev Video Cable – Website Requests"** in your Drive, in four tabs:
**Plan Selections**, **New Connections**, **Complaints**, **Billing & Renewals**.
Each request gets a reference number (PL…, NC…, CP…, BR…) and a **Status** column (New / Called / In progress / Done / Cancelled) for your team.

---

## Step 1: Connect the Google Sheet (about 5 minutes, one time)

1. Open the sheet **Rajeev Video Cable – Website Requests** in Google Drive.
2. Click **Extensions → Apps Script**.
3. Delete everything in `Code.gs`, paste in the contents of this folder's `Code.gs`, and click **Save**.
4. In the function dropdown at the top choose **setup**, click **Run**, and allow the permissions Google asks for.
   This creates the four tabs with headings, plus a hidden "Website Content" tab.
5. Click **Deploy → New deployment**. Click the gear icon, choose **Web app**, and set:
   - **Execute as:** Me
   - **Who has access:** Anyone
6. Click **Deploy**, then copy the **Web app URL** (it starts with `https://script.google.com/macros/s/` and ends with `/exec`).
7. Open `index.html` in a text editor and search for `PASTE_YOUR_WEB_APP_URL_HERE`. It is on this line:

   ```html
   <script>window.RVC_CONFIG={apiUrl:"PASTE_YOUR_WEB_APP_URL_HERE"};</script>
   ```

   Replace `PASTE_YOUR_WEB_APP_URL_HERE` with your Web app URL and save.

> If you change `Code.gs` later, use **Deploy → Manage deployments → Edit → New version** so the URL stays the same.

## Step 2: Put the website on GitHub Pages

1. Sign in to GitHub and create a new **public** repository, for example `rajeev-video-cable`.
2. Click **Add file → Upload files**, upload `index.html` and `logo.png`, then **Commit changes**.
3. Go to **Settings → Pages**. Under **Build and deployment** choose **Deploy from a branch**, branch **main**, folder **/ (root)**, and click **Save**.
4. After a minute the site is live at `https://<your-github-username>.github.io/rajeev-video-cable/`.

## Step 3: Use your GoDaddy domain

1. In GitHub **Settings → Pages → Custom domain**, type your domain, e.g. `www.yourdomain.com`, and click **Save**.
2. In GoDaddy open **My Products → your domain → DNS** and add these records:

   | Type | Name | Value |
   |---|---|---|
   | CNAME | `www` | `<your-github-username>.github.io` |
   | A | `@` | `185.199.108.153` |
   | A | `@` | `185.199.109.153` |
   | A | `@` | `185.199.110.153` |
   | A | `@` | `185.199.111.153` |

   Delete any existing `A @` record that points to GoDaddy's parking page, and any old `CNAME www` record.
3. DNS can take up to a few hours. When GitHub shows the domain as verified, tick **Enforce HTTPS**.

## Step 4: Edit the website

- Open `https://www.yourdomain.com/#admin` (or click **Admin login** in the footer).
- Default login: **admin / admin123**. Change it straight away under **Login & security**.
- Edit any section, use **Preview** to check, then **Publish**. Visitors see the change the next time they open or refresh the site.
- **Open requests sheet** in the admin bar jumps to your Google Sheet.

Forgot the admin password? In Apps Script run the function **resetAdminLogin**. The login goes back to admin / admin123.

## Notes

- After 5 wrong passwords, admin login is locked for 15 minutes.
- The same mobile number can send one request of each type per minute. This stops double taps and spam.
- Customer requests only reach the sheet from the hosted website, not from the claude.ai preview.
