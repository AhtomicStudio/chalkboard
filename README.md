# Chalktalk 🖤

*our little chalkboard*

A cozy chalkboard for any two people. Each pair gets their own private board: one half each — draw with chalk, leave typed notes, erase with satisfying dust clouds, and pick the board color together.

Opening the site with no link shows a welcome page with a **make your chalkboard** button. Every new board gets its own private link (e.g. `#honey-teapot-k3x9p`) — whoever you send it to becomes the second person. The welcome page also lists boards you've joined on this device, and the footer has *start another board*, so one person can share different boards with different people.

## Try it right now (local mode)

Just double-click `index.html`. Everything works — drawing, notes, eraser, backgrounds — but it only saves on your own device. Perfect for playing with the feel before wiring up sync.

## Connect the two of you (Firebase, free, ~5 minutes)

1. Go to [console.firebase.google.com](https://console.firebase.google.com) and sign in with a Google account.
2. **Create a project** — name it anything (e.g. `chalkboard`). You can turn off Analytics.
3. In the left menu: **Build → Realtime Database → Create Database**. Pick the location closest to you, and choose **Start in test mode**.
4. Click the gear icon → **Project settings** → under *Your apps*, click the **`</>` (Web)** icon. Register the app (any nickname, no hosting needed). Firebase shows you a `firebaseConfig` object.
5. Open `index.html` in a text editor, find `const FIREBASE_CONFIG = null;` near the top of the script, and replace it:

   ```js
   const FIREBASE_CONFIG = {
     apiKey: "AIza...",
     authDomain: "chalkboard-xxxxx.firebaseapp.com",
     databaseURL: "https://chalkboard-xxxxx-default-rtdb.firebaseio.com",
     projectId: "chalkboard-xxxxx",
   };
   ```

   Make sure `databaseURL` is included — copy it from the Realtime Database page if it's missing from the snippet.

> Test mode rules expire after 30 days. When they do, go to Realtime Database → Rules and set:
> ```json
> { "rules": { "rooms": { ".read": true, ".write": true } } }
> ```
> Fine for a private gift — the room link itself is the secret.

## Put it online so your aunt can open a link

Easiest option — **Netlify Drop** (no account setup pain):

1. Go to [app.netlify.com/drop](https://app.netlify.com/drop).
2. Drag the `Chalkboard` folder onto the page.
3. You get a URL like `https://something.netlify.app`.

Open it, claim your side, then hit **✉ invite** — it copies the link with your room ID (e.g. `#honey-teapot-42`). Send that to your aunt. When she opens it, she picks her half and you're connected.

(Firebase Hosting or GitHub Pages work just as well if you prefer.)

## How it works

- **Rooms** — the part after `#` in the URL. Different room ID = a fresh board. The link is the key: it's random enough that nobody stumbles into your board, but anyone you give it to can see it (only the two owners can draw). One Firebase project serves all boards.
- **Sides** — first visit asks your name and which half is yours. Your browser remembers you; only you can draw, erase, or move things on your half.
- **Tools** — chalk sticks (5 colors), eraser (with chalk clouds), `Aa` typed notes (drag to move, double-click to edit, × to delete), doodle stamps (♥ ★ ✿ ☀ — one-tap chalk doodles, sized by the text slider), chalk width + text size sliders, board color swatches, undo (Ctrl+Z), and a two-tap "wipe my side".
- **Chalk sounds** — a soft chalk scratch while writing and a felt swipe while erasing, generated in the browser (no audio files). Toggle with the 🔊 button in the tray.
- **Keepsake** — the 🖼 button saves the whole board as a polaroid-style PNG (wooden frame, cream border, title + date caption). Perfect for keeping the handmade ones.
- **"They left you something"** — come back after your person added anything and their half glows softly with a note like "✨ auntie left you something".
- **Presence** — the dot next to a name glows green when that person has the board open (Firebase mode).
