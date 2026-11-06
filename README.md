# Transformation Live

A live, streamable scene for the **8 Week Transformation** challenge. Every character in the gym and kitchen is a real challenge member, and they act out what they upload in the app:

- **Meal** – they cook at the stove, carry the plate to a table and eat it
- **Meal prep** – they cook, then chop and box up containers at the prep counter
- **Workout** – they go to the matching station and do the exercise: treadmill, spin bike, bench press, squat rack, lat pulldown, dumbbells (curls or shoulder press), or a mat (push-ups, crunches, yoga)

The overlay shows a live upload ticker, member/meal/workout counters and a sign-up box with your link and promo code. It renders at up to 4K (3840×2160).

## Try it

Open `index.html` in a browser. It runs on demo uploads until your app is connected. The panel under the scene lets you send test uploads, edit the sign-up text, change render quality and turn demo mode off.

With GitHub Pages turned on (Settings → Pages → Deploy from branch → `main`), the scene is live at `https://<your-username>.github.io/<repo-name>/`.

## Connect your app

1. When a member posts, your backend sends one JSON message per upload to a realtime channel (Supabase Realtime, Firebase, Pusher or your own WebSocket):

   ```json
   {
     "id": "evt_8812",
     "user": { "id": "u_331", "name": "Priya" },
     "type": "workout",
     "title": "Back squats 5x5",
     "category": "squat"
   }
   ```

   - `type`: `meal`, `mealprep` or `workout`
   - `category` (optional): `treadmill`, `bike`, `bench`, `squat`, `cable`, `dumbbell`, `mat`. If it's left out, the station is picked from the title.
   - Each member keeps the same look every time, based on `user.id`.

2. Put your channel address in the `FEED` block at the top of the script in `index.html`:

   ```js
   const FEED = {
     websocketUrl: "wss://your-relay.example.com/uploads",
     pollUrl: "",          // or a JSON endpoint that returns an array of events
     pollEveryMs: 4000
   };
   ```

   Any script on the page can also call `window.pushUpload({...})`.

## Stream it

1. In OBS, add a **Browser Source** pointing at the hosted page, sized 3840×2160 (or 1920×1080).
2. Stream to YouTube and Kick from OBS. For a 24/7 stream, run OBS on a cloud machine with a GPU.
3. If the machine struggles, set Render quality to 1440p or 1080p in the panel.

## Before going live

Only show members who have opted in, and use first names or handles.
