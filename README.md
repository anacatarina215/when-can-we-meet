# When Can We Meet? 📅

A simple group scheduling tool. Create an event, share the link, and everyone marks the days they're **busy**. The calendar highlights the days when **everyone is free**.

No accounts, no installs, no backend to maintain: it's a single HTML file that uses Firebase Realtime Database to sync in real time.

## Features

- **Private events**: create an event with a name and a password
- **Easy sharing**: invite people with a link, or with an event code + password
- **Real-time sync**: everyone in the event sees changes instantly
- **Easy to tell people apart**: each person gets their own shape and color (circle, square, triangle, diamond, star, cross, hexagon, inverted triangle) on the calendar
- **"Everyone free" highlighting**: days with no one busy are marked green, with a summary list of upcoming free days
- **3-month calendar view**: navigate month by month
- **Session memory**: returning visitors on the same browser are logged back into their event automatically
- **Responsive**: works on desktop and mobile

## How it works

1. The creator picks an event name, a password, and their own name.
2. The app generates a unique event code and a shareable link (`?event=<code>`).
3. Friends open the link (or use *Join with Code* on the home page), enter their name and the event password, and join.
4. Everyone clicks days to mark themselves busy. Click again to unmark.
5. Days where nobody is busy turn green.

## Tech stack

- Plain HTML, CSS and vanilla JavaScript (single file, no build step)
- [Firebase Realtime Database](https://firebase.google.com/docs/database) for storage and live sync
- Web Crypto API (SHA-256) for password hashing

## Setup

### 1. Create a Firebase project

1. Go to the [Firebase Console](https://console.firebase.google.com/) and create a project (the free plan is enough).
2. Add a **Web App** and copy the config values.
3. Create a **Realtime Database**.
4. Set the database rules (see the security notes below).

### 2. Add your config

Open `index.html` and fill in the `FIREBASE_CONFIG_INLINE` block:

```js
const FIREBASE_CONFIG_INLINE = {
  apiKey: "...",
  authDomain: "...",
  databaseURL: "...",
  projectId: "...",
  storageBucket: "...",
  messagingSenderId: "...",
  appId: "...",
  measurementId: "..."
};
```

### 3. Deploy

The app must be served over HTTP(S). It won't work when opened directly from disk (`file://`).

- **Netlify Drop**: drag the HTML file onto [app.netlify.com/drop](https://app.netlify.com/drop)
- **GitHub Pages**: push the file as `index.html`, then go to *Settings → Pages*

## Security notes

This project is designed for casual use among friends, not for sensitive data. Please be aware of the following:

- The event password is hashed in the browser and compared client-side. It prevents casual access but is **not** real server-side authentication.
- The database rules need to allow public read/write for the app to work, so anyone who knows an event's code could technically read its data directly from the database.
- The Firebase web config is visible to anyone who views the page source. This is normal for Firebase web apps, but you should restrict your API key to your site's domain in the Google Cloud Console.
- Don't store anything private here. Event data is just names and busy dates.

If you need stronger protection, consider adding Firebase Authentication and tightening the database rules.

## Possible improvements

- Server-side password verification (e.g. Firebase Auth or Cloud Functions)
- Event creator controls (remove participants, delete event)
- Time-of-day availability, not just full days
- Export or share the best dates
- Multi-language support

## License

MIT
