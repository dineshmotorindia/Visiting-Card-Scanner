# Gopali & Co Card Register: Firebase + APK setup

Total time about 45 minutes. Everything below is free.

## Part 1. Firebase (the online server)

1. Open https://console.firebase.google.com and sign in with a Google account the company will keep.
2. **Create a project** (name it e.g. `gopali-cards`). Turn Google Analytics OFF. Click Create.
3. On the project home, click the **web icon  </>**. Nickname: `card-register`. Do NOT tick "Firebase Hosting". Click Register app.
4. You will see a block called `firebaseConfig` with apiKey, authDomain, projectId, storageBucket, messagingSenderId, appId. Keep this page open.
5. Open `index.html` in a text editor (or the GitHub web editor). Near the top of the script find:

       const FIREBASE_CONFIG = { apiKey: "", authDomain: "", ... };

   Paste your values between the quotes. Leave `AI_KEY` empty unless you use the AI reader.
6. Left menu: **Build > Authentication > Get started > Sign-in method > Email/Password > Enable > Save**.
7. Still in Authentication: **Settings > Authorized domains > Add domain**. Add your GitHub address without https, for example `dineshmotorindia.github.io`. (Without this, sign-in fails on your live site.)
8. Left menu: **Build > Firestore Database > Create database**. Location: `asia-south1 (Mumbai)`. Choose **Production mode**. Enable.
9. Firestore > **Rules** tab. Delete everything, paste the contents of `firestore.rules`, click **Publish**.

## Part 2. Put the files on GitHub

Upload these files to the ROOT of your repository (replace old ones):
`index.html`, `manifest.json`, `sw.js`, `icon-192.png`, `icon-512.png`, `.nojekyll`

Settings > Pages > Deploy from a branch > `main` > `/ (root)`. Wait for the green tick under Actions.
Open your link on a phone. You should see the sign-in screen.

## Part 3. Make yourself the admin

1. On the live site, tap **New here? Create an account** and make YOUR account first.
2. Firebase > Firestore Database > Data > collection `users` > your document.
3. Change the field `role` from `staff` to `admin`. Sign out and in again in the app.
4. You now see: the Activity log, and the "show everyone's cards" switch in View All Cards.

Everyone else makes their own account the same way (role stays `staff`) and sees only their own cards.
To stop strangers from creating accounts later: Authentication > Settings > User actions, untick "Enable create (sign-up)", then add workers yourself under Authentication > Users > Add user.

## Part 4. Make the APK (PWABuilder, free)

1. Go to https://www.pwabuilder.com. Paste your live link (e.g. `https://dineshmotorindia.github.io/yourrepo/`). Click **Start**.
2. It should find the manifest and service worker. Click **Package for stores > Android**.
3. Fill in:
   - Package ID: `com.gopali.cardregister`
   - App name: `Gopali & Co Card Register`, Version: `1`
   - Signing key: **Create new**.
4. Click **Generate** and download the zip. KEEP the zip, the signing key file and its password in a safe place. You need the same key for every future update.
5. In the zip, the `.apk` file is the one to install. Send it to staff phones (WhatsApp, Drive or cable). On the phone: open it, allow "Install unknown apps" when asked.
6. First launch asks for camera permission. Tap Allow.

Updating later: the app opens your website, so when you update `index.html` on GitHub, every phone gets the new version automatically. You do NOT need a new APK for normal changes.

Top address bar: a Play-Store-style app hides the browser address bar only when the website proves it belongs to the app. That needs the file `.well-known/assetlinks.json` (it is inside the PWABuilder zip) at the ROOT of your domain. GitHub only allows that if your repository is named exactly `dineshmotorindia.github.io` (so the site lives at https://dineshmotorindia.github.io/). If your site lives under a repo name, the app still works fully, it just shows a thin address bar at the top.

## Free limits (Firebase Spark plan)

- Storage 1 GB. Each card stores its image(s), roughly 60 to 150 KB. That is about 6,000 to 10,000 cards in total for the whole team.
- 50,000 document reads and 20,000 writes per day. Opening View All Cards reads every card you can see, so one worker with 1,000 cards uses 1,000 reads per fresh open. The app caches cards on the phone, so repeat opens cost much less.
- If you grow past this, the fix is to move card images to Firebase Storage and keep only text in Firestore. Ask me and I will do it.

## Optional safety steps

- Google Cloud Console > APIs & Services > Credentials > your Browser key > Application restrictions > Websites. Add `https://dineshmotorindia.github.io/*`. This stops others from using your key from other sites.
- Firebase > Firestore > Data > Export is not on the free plan. Back up by opening View All Cards as admin and keeping a note of important contacts, or ask me to add an Excel/CSV download.

## Test checklist

1. Create account, close the app completely, reopen: lands on the landing page, no sign-in.
2. Scan a card, adjust corners, save. Check it appears under View All Cards.
3. Sign in with the same email on a second phone: the card is there.
4. Make a second account: it does NOT see the first account's cards.
5. As admin: Activity log shows logins, saves and label PDFs.
6. Select 2 cards, Make labels, Download PDF, test print on plain paper.
