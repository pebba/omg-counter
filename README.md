# Oh My Gosh Counter

A page for a trusted group with one shared "Oh my gosh" counter. Everyone sees the count live, plus a history of who pressed and when.

- Each press waits 4 seconds before it is sent, so "undo last press" can take it back.
- After that, fix mistakes in the History panel: rename an entry, or delete it (behind a confirm step), which lowers the total by that entry's size.

Files:

- `index.html`: the whole page, hosted on GitHub Pages
- `firestore.rules`: Firestore security rules (the counter plus its `presses` history; anything else is closed)
- `firebase.json` / `.firebaserc`: let `firebase deploy --only firestore:rules` find the rules and the project

Backend: Cloud Firestore on the free Spark plan. The Firebase web config inside `index.html` is public by design; the rules decide what can be written. They trust whoever has the page, so keep the URL within the group.

## Reset the counter

Firebase console → Firestore → `counters` → `omg`: edit `count`, and delete entries under `presses` if you want the history cleared too.
