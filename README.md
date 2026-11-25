# Talent Vote Board

Anonymous audience voting for song, creativity, and performance scores with Firebase Firestore shared storage.

## Firebase setup

1. In Firebase Console, enable **Authentication → Sign-in method → Anonymous**.
2. Create a Firestore database.
3. Publish the contents of `firestore.rules` as your Firestore rules.
4. Create document `settings/voting` with a boolean field `open: true`.
5. Publish the app from this branch (or merge this branch into the GitHub Pages publishing branch).

The app imports Firebase's browser SDK from Google's CDN, so `npm install firebase` is not needed for this GitHub Pages version.

## Voting

- Visitors are authenticated anonymously by Firebase.
- A vote is saved at `votes/{anonymous UID}_{normalized performer name}`.
- The form stays available after submission. A voter can score multiple performers, but the same normalized performer name can only be submitted once per anonymous UID: Firestore allows only document creation and denies edits to an existing vote.
- Votes cannot be read while voting is open. To reveal results, set `settings/voting.open` to `false` in the Firebase console.
- The one-vote-per-performer limit is per anonymous account. Anonymous voting is not a robust identity check: changing browser/device or clearing site data may create another anonymous account.

After changing Firestore rules, click **Publish** in Firebase Console. The existing votes collection remains in place.
