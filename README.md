# Atlas — Unlimited Whiteboards

A visual thinking workspace built with **React, Vite, React Flow, Firebase Authentication, and Firestore**.

## Features

- Unlimited boards (subject to Firebase service quotas)
- Folder organization, starred boards, recently updated view and full board-name search
- Infinite pannable/zoomable canvas; sticky notes, headings, concepts, and tasks
- Connect cards with edges, edit by double-clicking, drag and reposition
- Duplicate, rename, move, star, delete boards
- Export and import boards as JSON
- Autosave to Firestore with per-user access rules
- Google sign-in or anonymous sign-in
- Dark, responsive dashboard; light canvas

## Setup

1. Run `npm install`.
2. In Firebase Console, create a project and register a Web app.
3. Enable **Authentication > Sign-in method > Google** and optionally **Anonymous**.
4. Create a **Cloud Firestore** database, then deploy the included security rules with `firebase deploy --only firestore:rules` (or paste `firestore.rules` into the Firestore Rules editor and publish).
5. Copy `.env.example` to `.env` and fill the **VITE_FIREBASE_*** values from Firebase Web app settings. Never commit your .env or service account keys.
6. Run `npm run dev`.

## Firebase Hosting

Install Firebase CLI if needed, run `firebase login` and `firebase use --add` to connect your project. Run `npm run build && firebase deploy --only hosting` to deploy. For other static hosts, build with `npm run build` and serve `dist` with SPA fallback.

## Storage architecture

`users/{uid}/boards/{boardId}` and `users/{uid}/folders/{folderId}`. User ownership is enforced in `firestore.rules`. Boards store node/edge arrays in one document for easy autosaving. For very large boards or simultaneous collaborative editing, partition canvas elements into per-node documents and add presence/conflict resolution. **Firestore documents have a 1 MiB size limit**; the current implementation does not support truly unbounded content inside any individual board.

## Notes

Anonymous guest boards are tied to the anonymous Firebase account and are not portable unless you export them. For persistent access across devices, use Google sign-in. This is an individual-workspace MVP, not a multi-user realtime collaboration system. Your Firebase project credentials and deployment are not included.
