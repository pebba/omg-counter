# Oh My Gosh Counter

A public page with one shared "Oh my gosh" counter. Everyone sees the same count live, and the page shows who pressed it last and when.

Each press waits 4 seconds on your screen before it is sent, so "undo last press" can take it back. Once sent, it stays: the rules never allow lowering the count.

- `index.html`: the whole page, hosted on GitHub Pages
- `firestore.rules`: Firestore security rules (reads open, writes can only add 1 to 10 per request)
- `firebase.json`: lets `firebase deploy --only firestore:rules` find the rules

Backend: Cloud Firestore on the free Spark plan. The Firebase web config inside `index.html` is public by design; the rules protect the data.

## Reset the counter

Firebase console → Firestore → `counters` → delete the `omg` document. It is recreated at 1 on the next press.
