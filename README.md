# Oh My Gosh Counter

A public page with two shared counters ("Oh my gosh" and "Absolutely"). Everyone sees the same counts live, and each counter shows who pressed it last and when.

- `index.html`: the whole page, hosted on GitHub Pages
- `firestore.rules`: Firestore security rules (reads open, writes can only add 1 to 10 per request)
- `firebase.json`: lets `firebase deploy --only firestore:rules` find the rules

Backend: Cloud Firestore on the free Spark plan. The Firebase web config inside `index.html` is public by design; the rules protect the data.

## Reset a counter

Firebase console → Firestore → `counters` → delete the `omg` or `abs` document. It is recreated at 1 on the next press.
