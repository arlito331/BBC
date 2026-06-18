# Bay Bay Capital — Factoring

A single-file web app for the factoring operation. Two parts:

- **New operation** — a step-by-step wizard that collects the deal details, applies the 60% cash cap, and generates the five legal documents (in Spanish) for printing or saving as PDF.
- **Dashboard** — live exposure and performance. **Stores numbers only** — no client names or IDs are ever saved here.

The whole app is one file: `index.html`. No build step, no server required.

---

## Users

| Username | Password      | Can do                                  |
|----------|---------------|------------------------------------------|
| `1`      | `davinci88`   | Everything, **including delete** (master) |
| `v`      | `sandysandy33`| Everything **except delete**             |

To change these, see "Changing passwords" below.

---

## Run it locally

Just open `index.html` in any browser (double-click it). That's it.

---

## Put it live on GitHub Pages

1. Create a new repository on GitHub (a **private** repo is recommended — see "Security" below).
2. Upload `index.html` and this `README.md` to the repo (drag-and-drop in the GitHub web UI works fine).
3. In the repo: **Settings → Pages**.
4. Under **Build and deployment → Source**, choose **Deploy from a branch**.
5. Branch: `main`, folder: `/ (root)`. Save.
6. Wait ~1 minute. GitHub shows the live URL (e.g. `https://yourname.github.io/your-repo/`).

The app entry point must be named `index.html` (it is) so GitHub Pages serves it automatically.

---

## Important: how data is saved

The app saves the dashboard numbers in the **browser's local storage on the device you're using**.

- This means the dashboard data **does not sync between two different computers or phones.** If you open it on your laptop and your assistant opens it on hers, each sees their own separate dashboard.
- It persists fine on a single device across sessions.

If you need **one shared live dashboard** that both of you see from anywhere, that requires a small cloud database (e.g. Firebase or Supabase). The app is built so this can be added later without redoing the rest. Ask and it can be wired in.

(Note: inside the Claude preview the app uses Claude's shared storage instead, which is why it appeared to sync there. On a normal web host it uses the browser's local storage, as described above.)

---

## Auto-email (optional, not yet active)

When a new operation is created, the app can automatically email a **non-sensitive summary** (numbers only) to `aa@mlggroup.com` and `vm@mlggroup.com`. The documents themselves are generated in the app for download.

This uses [EmailJS](https://www.emailjs.com) (a browser-based email relay — a plain web page can't send email on its own). To turn it on:

1. Create a free EmailJS account and an email service + template.
2. Open `index.html`, find the `EMAIL_CONFIG` block (search for `EMAIL_CONFIG`).
3. Paste your three values:
   ```js
   const EMAIL_CONFIG = {
     serviceId:  'YOUR_SERVICE_ID',
     templateId: 'YOUR_TEMPLATE_ID',
     publicKey:  'YOUR_PUBLIC_KEY',
     to: 'aa@mlggroup.com, vm@mlggroup.com'
   };
   ```
4. The EmailJS template should accept the variables `to_email`, `subject`, and `message`.

Until those keys are filled in, the app simply skips emailing and still works (you download the documents).

---

## Touch ID sign-in (master, on your Mac)

The master user can unlock with the Mac's fingerprint instead of typing the password.

How to set it up (do this once, on the live HTTPS site):
1. Open the live GitHub Pages URL on your Mac (Touch ID needs HTTPS — it won't work by double-clicking the local file).
2. Sign in once with `1` / `davinci88`.
3. Click **Enable Touch ID** in the top bar and approve the fingerprint prompt.
4. From then on, the login screen shows **Unlock with Touch ID** — tap it and use your fingerprint.

Notes:
- It's tied to that specific Mac and that specific site URL. Setting it up on one Mac doesn't enable it on another.
- The password always still works as a backup.
- It registers as the master user.

---

## Note on the signer

The app/brand is **Bay Bay Capital**, but for now the generated legal documents name **Ariel Ashouri Levy** (cédula 8-774-2065) as the party owed and the signer, acting in his own name while the company is being formalized.

---

## Changing passwords

Passwords are checked with a small hash in the file (search for `AUTH_USERS`). To change a password, you need to regenerate the hash for the new `username:password` pair. Ask and a new hash can be generated for you, or replace the whole `AUTH_USERS` block.

---

## Security — please read

This is an internal two-person tool, and the login is a **front-door latch, not a vault**:

- The password check happens inside the file. Someone who downloads the file could, with effort, recover the password from it.
- If you enable EmailJS, the public key sits in the file too; anyone with the file could send mail through that key.
- The dashboard itself stores **no client names or cédulas** — only numbers — so even if the file leaked, there is nothing sensitive stored in it. Personal data only ever flows through the documents and the optional email.

Recommendations:
- Use a **private** GitHub repo if you only need code backup/versioning.
- If you publish via GitHub Pages, the page is publicly reachable by URL. Keep the URL private and treat the login as casual protection only.
- For anything stronger (real accounts, true shared data, access control), use a hosted backend.

---

Build v11.
