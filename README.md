# Oh My Gosh Counter

A page for a trusted group with one shared "Oh my gosh" counter. Everyone sees the count live, plus a history of who pressed and when.

- Each press waits 4 seconds before it is sent, so "undo last press" can take it back.
- After that, fix mistakes in the History panel: rename an entry, or delete it (behind a confirm step), which lowers the total by that entry's size.

Files:

- `index.html`: the whole page, hosted on GitHub Pages
- `firestore.rules`: Firestore security rules (the counter plus its `presses` history; anything else is closed)
- `firebase.json` / `.firebaserc`: let `firebase deploy --only firestore:rules` find the rules and the project

Backend: Cloud Firestore on the free Spark plan. The Firebase web config inside `index.html` is public by design; the rules decide what can be written. They trust whoever has the page, so keep the URL within the group.

The rules only let the count move together with a history entry: a press raises it by exactly that entry's size, and deleting an entry lowers it by exactly that size. The counter's `last` field names the entry being added or deleted so the rules can check this. Nobody can set the count directly from the browser.

## Lock down the web API key

Google Cloud console → APIs & Services → Credentials → the "Browser key (auto created by Firebase)" for `omg-counter-c8eb2`:

1. Application restrictions → Websites: add your GitHub Pages URL (e.g. `https://<user>.github.io/*`). Add `http://localhost/*` too if you test locally.
2. API restrictions → Restrict key: keep only the APIs Firebase uses here: Cloud Firestore API, Firebase Installations API, and Firebase App Check API.

## App Check

App Check makes Firestore accept requests only from your real page, not from scripts that copied the config.

1. Create a reCAPTCHA v3 key at https://www.google.com/recaptcha/admin with your GitHub Pages domain.
2. Firebase console → App Check → Apps: register the web app with reCAPTCHA v3 and paste the **secret** key there.
3. Put the **site** key in `APP_CHECK_SITE_KEY` in `index.html` and deploy the page.
4. Watch App Check → APIs → Cloud Firestore for a day or so. Once nearly all requests are verified, click **Enforce**.

Leave `APP_CHECK_SITE_KEY` empty to turn App Check off. Don't enforce until the page is sending tokens, or every press will be rejected.

## Reset the counter

Firebase console → Firestore → `counters` → `omg`: edit `count`, and delete entries under `presses` if you want the history cleared too. The console bypasses the rules, so this still works.
