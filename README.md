# GlucoTrack v4

A personal blood glucose tracking Progressive Web App (PWA) for Type 1 diabetics. Runs entirely in the browser, installs to your iPhone or Android home screen, and stores all data locally on your device.

> ⚠️ **Medical Disclaimer:** GlucoTrack is a personal logging and analysis tool — not a medical device. Never adjust insulin doses based solely on app suggestions. Always consult your endocrinologist or diabetes care team before making changes to your treatment plan.

---

## Installing to Your Phone

### iPhone (Safari only)
1. Open your GitHub Pages URL in **Safari**
2. Tap the **Share** button (⬆️ box with arrow)
3. Tap **Add to Home Screen**
4. Tap **Add**

The app installs like a native app — no browser bar, full screen, works offline.

### Android (Chrome)
1. Open your GitHub Pages URL in **Chrome**
2. Tap the **⋮ menu**
3. Tap **Add to Home Screen**

### Updating the App
After you push a new version to GitHub Pages, the app updates silently in the background. To get the update immediately, swipe the app away in your app switcher and reopen it.

> **Your data is always safe during updates.** All readings are stored in `localStorage` separately from the app code.

---

## Core Features

### Logging a Reading
Tap the blue **Log Reading** button at the bottom of the screen.

- Enter your glucose value in **mmol/L**
- Select context (Before Meal, After Meal, Before Bed, Waking Up, etc.)
- Enter **Carbs (g)** — the app automatically suggests a bolus insulin dose based on your carb ratio
- **Insulin section:**
  - 💉 **Bolus (short-acting)** — your pre-meal or correction dose
  - 🌙 **Basal (long-acting)** — auto-fills with your configured daily dose (e.g. Lantus 20u). Fill both if you're injecting both at the same time, or just the one that applies.
- Tap **Save Reading**

### Logging a Meal
Open the drawer menu → **Log Meal**, or use the full-screen camera scanner (📷 Scan Meal in the top bar).

- **Manual entry** — add food items with carb amounts; tap **🤖 AI Carb Lookup** to auto-estimate carbs for all items at once
- **Photo scan** — take a photo of your meal and AI analyses carb content automatically

### Importing from FreeStyle Libre
Settings → Data & Backup → **📲 Import from FreeStyle Libre (CSV)**

1. In the FreeStyle LibreLink app: Menu → My Account → Export Data
2. Choose your date range and export
3. Save the CSV to your phone
4. In GlucoTrack, tap the import button and select the file

All readings merge with your existing data. Duplicate readings are skipped automatically. Supports all LibreLink CSV date formats including South African (DD-MM-YYYY).

---

## Insulin Tracking

GlucoTrack tracks both types of insulin separately as a Type 1 diabetic uses them differently.

### Bolus (Short-Acting) Insulin
Logged alongside each reading or meal. Used for:
- Carb coverage: `Carbs ÷ Carb Ratio = Bolus units`
- Correction: `(Current BG − Target) ÷ Correction Factor = Correction units`

Configure your ratios in **Settings → Bolus Insulin**.

### Basal (Long-Acting) Insulin
Logged daily via the **Log Reading** modal — the basal field auto-fills with your configured dose.

Configure in **Settings → Basal Insulin**:
- **Insulin Name** — e.g. Lantus, Toujeo, Tresiba
- **Daily Dose** — in units
- **Injection Time** — default 18:00; app fires a reminder at this time daily

Basal insulin is stored separately from bolus insulin and is **excluded from the Glucose Forecast's insulin-on-board (IOB) calculations** — basal provides a flat background effect and does not cause the short-term glucose swings that bolus does.

---

## Basal Insulin Analysis (Forecast Screen)

Because long-acting insulin cannot be meaningfully "predicted" in a short-term glucose forecast, GlucoTrack instead provides three evidence-based observational tools to help you and your doctor understand whether your basal dose is working correctly.

### 1. 🌙 Basal Adequacy
**What it does:** Analyses your fasting readings (logged as "Before Bed" or "Waking Up") over the past 14 days and calculates what percentage are in range, above target, or below target.

**Verdicts:**
- ✅ **Well Controlled** — 70%+ of fasting readings in range. Basal dose appears appropriate.
- ⚠️ **Possibly Too Low** — 40%+ of fasting readings above target. Basal may need increasing.
- 🚨 **Possibly Too High** — 20%+ of fasting readings below target. Basal may be too high — review urgently.
- ⚠️ **Variable** — Inconsistent fasting readings. Could indicate missed doses, injection site issues, or illness.

**Requires:** At least 3 fasting readings (Before Bed or Waking Up context) over 14 days.

> This is an observational indicator only. Never adjust your basal dose without your endocrinologist's guidance.

### 2. 🌜 Overnight Trend
**What it does:** Pairs each "Before Bed" reading with the next "Waking Up" reading (within 12 hours) to calculate the average overnight glucose change across 7 days. Shows a table of the last 5 nights.

**Verdicts:**
- ✅ **Stable Overnight** — Average change within ±1.0 mmol/L. Basal is working well overnight.
- ⚠️ **Rising Overnight** — Average rise >1.0 mmol/L. May indicate insufficient basal, dawn phenomenon, or evening snacking.
- 🚨 **Falling Overnight** — Average drop >1.0 mmol/L. Basal may be too high — risk of overnight hypoglycaemia.

**Requires:** Both Before Bed and Waking Up readings on multiple days over 7 days.

> Persistent overnight rising or falling patterns should be discussed with your diabetes care team before any dose changes.

### 3. 💉 Basal Injection Streak
**What it does:** Tracks how many consecutive days you have logged your basal insulin injection. Missing a basal dose as a Type 1 diabetic has serious consequences (diabetic ketoacidosis risk), so consistency matters.

**Features:**
- Current streak with emoji milestone indicators (3 days ✅, 7 days ⭐, 14 days 🔥, 30 days 🏆)
- Longest streak ever recorded
- Total days logged
- 14-day visual grid showing which days were logged (green = logged, grey = missed)
- Reminder if today's injection has not yet been logged

**Requires:** Basal logged via the Log Reading modal (🌙 Basal field).

---

## Glucose Forecast

The short-term glucose forecast (1–3 hours) uses:
- Your current glucose reading and recent trend
- Active carbohydrates from meals logged in the past 2 hours
- **Bolus insulin on board (IOB)** from doses in the past 4 hours

Basal insulin is intentionally excluded from the forecast calculation because its action profile (20–24 hours, flat) is already reflected in your baseline glucose trend.

The forecast screen also shows:
- Active Factors (meals and bolus insulin currently affecting your glucose)
- Personalised Recommendations (predicted lows, correction suggestions)

---

## Alarms

Configure in the **Alarms** screen (drawer menu).

| Alarm | Default threshold | Cooldown |
|---|---|---|
| Urgent Low | < 3.0 mmol/L | 5 minutes |
| Low | < 3.9 mmol/L | 15 minutes |
| High | > 10.0 mmol/L | 15 minutes |
| Rising Fast | > 2 mmol/h | 15 minutes |
| Falling Fast | > 2 mmol/h drop | 15 minutes |

**When alarms trigger:**
- Every time you log a reading manually
- Every 30 seconds while the app is open (checks if the latest reading is recent and in alarm state)
- Basal reminder fires once daily at your configured injection time

**Notification Settings:**
- 🔊 Sound — synthesised tones via Web Audio API (no audio files needed)
- 📳 Vibration — distinct vibration patterns per alarm type
- 🌙 Overnight Alarms — can be silenced between 22:00–07:00

Use the **🔔 Test Alarm** button on the Alarms screen to verify sound and vibration are working on your device.

> **iOS note:** Alarms only trigger while the app is open on screen. iOS does not allow web apps to run in the background or send push notifications without a native app wrapper.

---

## A1c & Trends

Estimated HbA1c calculated from your average glucose using the formula:
`A1c ≈ (avg mmol/L × 1.5927 + 1.628)`

Also shows:
- Weekly A1c trend chart
- Time In Range breakdown (Low / Target / High / Very High) over 7 days
- 7-day stats (average, lowest, highest)

---

## Data & Privacy

- All data is stored **locally on your device** in `localStorage`
- Nothing is sent to any server (except AI meal analysis, which sends meal photos to the Anthropic API)
- No account required, no ads, no tracking

### Backup & Restore
Settings → Data & Backup:
- **💾 Backup All Data (JSON)** — saves a full backup including all readings and settings to your Files app
- **📥 Restore from Backup** — restores from a previously saved JSON backup (replaces current data)
- **📤 Export Readings (CSV)** — exports all readings as a CSV spreadsheet for sharing with your healthcare team
- **📲 Import from FreeStyle Libre (CSV)** — imports readings from a LibreLink CSV export

---

## Settings Reference

| Setting | Description |
|---|---|
| Low Below | Glucose below this triggers Low alarm (default 3.9 mmol/L) |
| High Above | Glucose above this triggers High alarm (default 10.0 mmol/L) |
| Carb Ratio | Grams of carbohydrate covered by 1 unit of bolus insulin (default 10) |
| Correction Factor | How much 1 unit of bolus insulin drops your BG in mmol/L (default 2.2) |
| Basal Insulin Name | Your long-acting insulin brand name (e.g. Lantus) |
| Basal Daily Dose | Your prescribed daily basal dose in units |
| Basal Injection Time | Daily reminder time (default 18:00) |

---

## Technical Notes

- Single HTML file — no build process, no dependencies, no server required
- PWA with service worker for offline support
- AI meal analysis powered by Claude (Anthropic API) — requires internet connection
- AI food carb lookup powered by Claude — requires internet connection
- All charts drawn on HTML5 Canvas
- Compatible with Safari (iOS), Chrome (Android), and desktop browsers
