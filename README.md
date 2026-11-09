# Talent Vote Board

Anonymous audience voting for song, creativity, and performance scores, with a shared Firebase Firestore backend.

## Firebase setup (one-time)

The web app is configured for Firebase project `thanking-voting`. Firebase's web API key is intended to be included in client-side web code; access is controlled by Firebase Authentication and Firestore Security Rules, not by hiding that key.

1. In Firebase Console, open **Authentication → Sign-in method** and enable **Anonymous**.
2. Open **Firestore Database** and create a database.
3. In Firestore **Rules**, publish the contents of `firestore.rules` from this repository.
4. In Firestore **Data**, create collection `settings`, document `voting`, with boolean field `open` set to `true`.
5. Publish the site from the branch containing this `index.html` (or merge the app branch into the branch configured for GitHub Pages).

The page imports Firebase's browser SDK from Google's CDN, so `npm install firebase` is not needed for this GitHub Pages static site.

## Voting and closing

- Visitors are signed in anonymously by Firebase Authentication.
- A vote is saved at `votes/{anonymous-auth-uid}`. Firestore rules permit only one immutable vote per anonymous Firebase account while voting is open.
- Names and scores are unreadable to clients while voting is open. They become visible after closing.
- To close voting, use Firestore Console → `settings` → `voting` and change `open` to `false`. The app will detect the change and reveal the shared leaderboard. No vote-closing password is used; clients cannot change the setting.
- Anonymity means voters do not need to provide a name or email. The one-vote limit is per anonymous Firebase account/browser, not a robust identity check: clearing site data or using another browser/device may create another anonymous account.

Do not make the vote collection readable while voting is open or allow clients to write to `settings/voting`. The provided rules enforce both protections.
