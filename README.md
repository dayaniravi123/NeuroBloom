# 🌱 NeuroBloom

**A calm, low-stimulation companion for concussion recovery and mental wellness.**
**Fully on-device — no APIs, no third-party services, no dependencies, no data leaves the browser.**

Concussion (mild traumatic brain injury) recovery is uniquely hard: it hits the
body *and* the mind, it demands patient pacing that's easy to get wrong, and the
people going through it are often light- and screen-sensitive — so most apps
literally hurt to use. NeuroBloom is built for them. It tracks symptoms and mood,
paces a safe graduated return to activity, and offers a recovery companion whose
machine learning runs **entirely in the browser** and is **safety-checked before
it ever answers**.

The whole interface is intentionally dim, quiet, and low-motion — the design *is*
the accessibility feature.

---

## ✨ What it does

- **Daily check-in** — a SCAT-style symptom inventory (physical, cognitive,
  emotional, sleep) scored 0–6, plus a mood rating, sleep hours, and notes.
  A **red-flag safety screen** runs first and pushes users to emergency care.
- **Recovery bloom** — a plant that grows through your recovery stages, turning an
  abstract, discouraging process into visible progress.
- **Graduated recovery plan** — the consensus **Return-to-Play** and
  **Return-to-Learn** step protocols, with contact/full-load stages **locked
  behind clinician clearance** (human-in-the-loop).
- **Trends + on-device forecast** — hand-drawn charts (no libraries) of symptom
  burden, mood and sleep, plus a **linear-regression recovery forecast computed
  locally** on your own check-ins.
- **Rest & reset** — box-breathing, a cognitive-rest timer, and 5-4-3-2-1
  grounding for symptom spikes and injury-related anxiety.
- **Recovery companion** — an on-device **machine-learning** guide (Naive Bayes),
  with a deterministic safety layer that screens every message for crisis and
  danger signs *before* the model runs.
- **Local-first & private** — all data lives in the browser. Nothing is uploaded.
  Export or erase anytime.

---

## 🤖 The AI/ML, and why it's fully on-device

NeuroBloom uses **two small machine-learning models that train and run in the
browser** — there is no external API and nothing you type or track is ever sent
anywhere.

1. **Companion intent classifier — multinomial Naive Bayes.**
   A labeled dataset of example questions is embedded in the app. On load, the
   model is trained (word likelihoods with Laplace smoothing, class priors), and
   each question is tokenized and classified into a recovery topic (rest, screens,
   headache, exercise, sleep, mood, dizziness, when-to-see-a-doctor). Low-confidence
   messages fall back to a general reply. It reports a softmax confidence so the app
   knows when it's unsure.

2. **Recovery forecaster — least-squares linear regression.**
   Runs over your recent symptom-burden history to fit a trend, draw a dashed
   projection on the chart, and estimate — in plain language, with caveats — whether
   you're improving, holding steady, or flaring, and roughly how many more check-ins
   until a "symptom-light" range.

This is a deliberate, honest take on **Responsible AI**: real ML, but private by
construction. Health data never leaves the device, so there's no data-sharing risk
to manage in the first place.

---

## 🩺 Responsible AI, concretely

1. **Safety before intelligence.** Every companion message hits a deterministic,
   auditable classifier *first*. Self-harm language → compassion + 988, no model
   output. Concussion danger-sign language → urgent in-person care, no model output.
   The probabilistic model never gets the chance to mishandle a crisis.
2. **Constrained scope.** The model only routes to vetted, non-diagnostic
   educational answers — it can't free-form or hallucinate medical instructions.
3. **Honest about uncertainty.** Low classifier confidence → a general reply that
   asks for more detail. Forecasts are labeled as trends, not promises.
4. **Human-in-the-loop.** Contact and full-load recovery stages can't be reached
   without confirming clinician clearance.
5. **Privacy by default.** All health data is stored only on-device.
6. **Clear limits.** "Educational only, not a diagnosis" is stated on every
   relevant surface.

---

## 🏆 How it maps to the tracks

| Track | How NeuroBloom competes |
|---|---|
| **Mental Health (core)** | Mood tracking, anxiety/low-mood support, grounding + breathing tools, crisis detection with 988 escalation. |
| **Physical Health (core)** | Symptom tracking, sleep logging, graduated physical return-to-activity protocol. |
| **Best Tech for Concussion Recovery** | SCAT-style symptoms, red-flag screening, Return-to-Play *and* Return-to-Learn staging, clinician-gated progression, on-device recovery forecast. |
| **Best Use of AI/ML & Responsible AI** | Two ML models trained and run **in the browser** (Naive Bayes classifier + linear-regression forecaster), a deterministic safety net that runs *before* the model, and privacy by construction — no API, no data leaves the device. |
| **Best Design** | A brief-driven, low-stimulation "healing garden at dusk" palette; a living bloom instead of a generic dashboard number; respects `prefers-reduced-motion`; keyboard-focusable; light/dark themes. |
| **Best Use of Render** | Deploys as a free static site *or* via `render.yaml` Blueprint with a zero-dependency Node server. |
| **Best Innovation & Creativity** | Reframes recovery as *growth*, and turns the users' own light sensitivity into the core design principle. |
| **Girls Who Code Future Innovator** | Solves a real, under-served problem (youth sports concussions especially affect girls' soccer & basketball) with an accessible, empathetic, privacy-first product. |

---

## 🚀 Run it

### Locally
Any static server works, because there's no build step and no backend logic. Two easy ways:

```bash
# Option 1: the included zero-dependency Node server
npm start                    # -> http://localhost:3000

# Option 2: no Node at all — just open the file
#   open public/index.html in your browser
```
(There are no packages to install — `npm install` does nothing.)

### Deploy to Render

**Option A — Static Site (simplest, free):**
Render → **New +** → **Static Site** → pick the repo → set **Publish Directory** to `public`.

**Option B — Web Service (Blueprint):**
Push to GitHub → Render → **New +** → **Blueprint** → pick the repo. `render.yaml`
runs the included Node server. No environment variables, no keys.

---

## 🧱 Tech stack

- **Frontend:** framework-free HTML/CSS/JS in a single self-contained file.
  Charts, the recovery bloom, the Naive Bayes classifier and the regression
  forecaster are all hand-written — **no libraries, no build step**.
- **Server:** optional, Node.js **built-ins only** (no Express, no npm deps).
- **Storage:** browser `localStorage` (local-first).
- **Deploy:** Render (static site or `render.yaml`).

No tracking, no analytics, no accounts, no external requests.

---

## 🎬 90-second demo script for judges

1. **Settings** → add a name + injury date. Show the greeting and bloom personalize.
2. **Check-in** → tick a red-flag box → the emergency banner fires instantly
   (safety before anything else). Untick, then rate symptoms + mood → save.
3. **Today** → readiness ring, streak, and the bloom updates.
4. **Plan** → advance a stage; reach the clinician-gated contact stage to show the lock.
5. **Trends** → the hand-drawn chart plus the on-device regression forecast.
6. **Companion** → ask a normal question (the Naive Bayes model routes it), then
   type "my headache is getting worse" → watch the safety layer escalate to care.
7. **Reset** → start box breathing for a calm close.

*Mention: everything just shown ran on-device — no servers, no APIs, no data sent anywhere.*

---

## 🛣️ Roadmap

- Clinician share-link (read-only recovery summary export).
- Richer on-device personalization from a user's own symptom history.
- Reminder scheduling and caregiver mode.
- Localization and a true native low-brightness mode.

---

## ⚠️ Disclaimer

NeuroBloom is an educational and self-tracking tool, **not a medical device and
not a diagnosis**. It does not replace evaluation, treatment, or clearance by a
qualified clinician. In an emergency, contact local emergency services. If you are
in crisis in the US, call or text **988**.

MIT licensed.
