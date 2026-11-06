# 8WTC notes and member activity schema

Notes taken from https://www.8wtc.com/ on 6 November 2026, plus the event schema that Transformation Live uses to show (and simulate) what members do.

## What 8WTC is

- **8 Week Transformation Challenge**: a judged fitness challenge run as fixed 8-week rounds with set start and end dates. Challenge 33 is listed as "opens soon" with a waitlist.
- **Goals on the site:** lose fat, build strength, build consistency.
- **Scale:** 31 challenges completed, 10,000+ transformations, 4.9/5 average rating, over £360,000 awarded.
- **Base:** UK (prices in GBP, UK App Store). The waitlist phone dropdown lists about 45 countries across Europe, the Middle East, Asia, Africa, the Americas and Oceania.

## The member journey

| Stage | What the member does |
|---|---|
| Sign up | Chooses **home or gym** training, sets **experience level**, enters **starting measurements** and **starting photos** |
| Every day | Logs **meals** (recipe library filtered to their targets, or **barcode scan**), **weigh-in**, **steps**, **water**, **workouts** |
| Training | Sessions of **45–60 minutes**, home or gym (example plan: "Gym · Advanced · 4× a week"), with **streaks** and a weekly target |
| Every week | **Weekly check-in** that adjusts **calories, macros** and the week's focus |
| Week 8 | **Final photos and measurements** and a final check-in, inside the challenge window |
| After | **Maintain mode** keeps weigh-ins, meals and sessions going |

**Health sync:** Apple Health, Samsung Health, Garmin, Fitbit and WHOOP.

**Community:** leaderboards (including an overall step total across all 8 weeks), cohort WhatsApp groups and a private community.

**Prizes per challenge (£18,000 total):**
- £5,000 for the top male transformation and £5,000 for the top female transformation.
- 2 × £500 weekly giveaways for 8 weeks.
- Judged on change from each entrant's own starting point.

**Pricing:**
- £149.99 per challenge, with a money-back guarantee if the photos show no change.
- Membership at £24.99 a month or £299.88 a year.
- Unlimited at £299.99 a year, which includes 2 challenge entries.

**Partner programme:** 8WTC pays commission on referred members. This is the route for affiliate links on the stream.

## Activity schema

One message per member action. The scene accepts these over WebSocket, a JSON endpoint, or `window.pushUpload(event)`.

```json
{
  "id": "evt_8812",
  "type": "workout",
  "user": { "id": "u_331", "name": "Priya", "country": "GB" },
  "title": "Pull day · 50 min",
  "category": "cable"
}
```

| `type` | Extra fields | What the character does in the scene |
|---|---|---|
| `meal` | `title` (dish name) | Fridge → chopping board → stove (pan, wok or pot depending on the dish) → plates up → eats at a table → sometimes washes up |
| `mealprep` | `title` (e.g. "Turkey chilli x4") | Batch cooks, boxes up containers at the counter, stores them in the fridge |
| `workout` | `title`, optional `category`: `treadmill`, `bike`, `bench`, `squat`, `cable`, `dumbbell`, `mat` | Goes to that station and does 3 sets with rests (cardio as intervals, yoga as a flow) |
| `weighin` | `kg`, optional `delta` (e.g. -0.4) | Steps on the scales; reading and change pop up |
| `water` | `title` (e.g. "2.1 L water") | Refills a bottle at the water station and drinks |
| `steps` | `steps` | Walks laps of the lobby |
| `checkin` | `title` (e.g. "Week 3 check-in") | Uses the check-in kiosk ("macros updated") |
| `photo` | `title` | Front, side and back poses at the photo wall with a ring light |
| `signup` | none | Walks in as a new member: kiosk → scales → starting photos |

`user.country` is a two-letter code shown on the name tag. `user.id` keeps a member's look the same every time.

## How the 100,000-member simulation works

- Members are spread across 34 countries, weighted towards the UK, Ireland and the Gulf (UAE, Saudi Arabia, Qatar, Kuwait, Bahrain, Oman), then the USA, Canada, Australia, New Zealand, South Africa, Europe and Asia.
- Each member is generated from their number, so they always keep the same name, country, home/gym setup and starting weight.
- Activity follows each country's **local time**:
  - Breakfast, lunch and dinner peaks.
  - Morning and evening workout peaks.
  - Morning weigh-ins and evening step syncs.
  - Sunday check-ins and Sunday meal prep.
- That produces a few thousand uploads a minute worldwide. The HUD and world panel show the global totals, and a "director" picks a balanced sample to act out in the scene.
- The simulation is always labelled **DEMO / simulated preview** on screen. Switch it off once real events are connected, and the badge changes to **LIVE**.
