# Serene

**The calmest profile page on the internet.**

Serene is a premium-looking "My Profile" settings page where every single interaction is deliberate ragebait. It looks like a polished SaaS product; the rage comes from the contrast. Everything is still technically completable: infuriating, never broken.

Built for **Attack on Token** (CESAC, VIT Pune) · Round 2: *Token Treachery*
Team: **Aryan Patil & Riya**

> ⚠️ **Turn your volume down before opening.** A chicken bounces around the screen, and every time it touches an edge the screaming multiplies and gets louder.

---

## Run it

No install, no build step, no dependencies.

1. Download `index.html`.
2. Open it in Chrome, Edge, Firefox or Safari.
3. Click anywhere once. Browsers block sound until you interact, so that first click is when the chickens start.

Use a laptop or desktop with a mouse; the runaway buttons need a cursor.

**Testing switches**

| Switch | What it does |
|---|---|
| `index.html?calm` (or `#calm` on a hosted link) | Turns all the chaos off so you can check the layout |
| **Reset demo data** (footer link) | Clears the saved profile so the next person starts fresh |

---

## Features

26 ragebaits, ranked by rage impact first and build risk second.

| # | Feature | What happens | Idea by |
|---|---|---|---|
| 1 | Runaway elements | **Sign in**, the **Username** box and **Save** jump to the opposite side of the screen when your cursor gets within 100px. They give up after 5 escapes (Save after 3): "Fine. Click me. I don't care." | Both |
| 2 | Fake save | The progress bar crawls to 99%, drops to 12%, then finishes. Ten seconds after your first save, one field quietly reverts and the assistant says "We didn't change anything. Are you feeling okay?" | Both |
| 3 | Secure keyboard | Your name can only be typed on an on-screen keyboard that reshuffles after every key. "Your name" is real text that won't clear. | Riya |
| 4 | Phone slider | Choose your number on a slider from 0 to 9,999,999,999. The ±1 buttons speed up when held. | Riya |
| 5 | Birthday | Starts at 01/01/0001 with a single "+1 day" button. Hold to speed up; go past today and it wraps back to year 1. | Riya |
| 6 | Typo gremlin | In the email, username and bio fields, every 4th keystroke types a neighbouring key and every 10th deletes a random earlier letter. | Aryan |
| 7 | Password rules | Nine rules appear one at a time: 8+ characters, a number, an uppercase letter, digits that add up to 25, a month, a Roman numeral, the word "titan", an egg 🥚, and a prime-number length. | Riya |
| 8 | Discard trap | The huge glowing button is **Discard changes**, and it really discards everything. The real Save is a tiny link. | Riya |
| 9 | Rage detector | Three fast clicks open a 10-second breathing screen that restarts whenever you move the mouse. | Aryan |
| 10 | Terms | "I do NOT accept the Terms and Conditions" is pre-ticked and locked until you scroll to the bottom, and new terms appear every time you get there. | Riya |
| 11 | Username | Every username is "Taken 😔". The only way out is an absurd suggestion like `you_official_real_notfake_9921`. | Aryan |
| 12 | Toggle betrayal | Light mode switches itself back after 2 seconds and asks "Are you sure?". Turning off email notifications quietly makes your phone number public. | Both |
| 13 | Brightness | The slider is inverted and dims the whole page, itself included. | Riya |
| 14 | Cookie wall | "Accept all" is huge; rejecting is hidden. Each "Legitimate interest" toggle switches itself back on. | Riya |
| 15 | Email | Split into name, domain and ending. The name types backwards and `.com` hides near the bottom of 50 endings. | Riya |
| 16 | AI bio | Pre-written by "AI" (a Norwegian dentist). AI Autocorrect swaps words (love → loaf, coding → cooking, I → Aye), and the character limit shrinks every second you type. | Both |
| 17 | Card shuffle | The settings cards swap places every 8 seconds. | Riya |
| 18 | CAPTCHA | "Select all squares containing a token." Every square is a token. | Riya |
| 19 | Avatar | "Download image" does nothing. Clicking the photo rotates it 90°; "Fix rotation" rotates it 89°. | Both |
| 20 | Lying labels | Clicking a field's label puts your cursor in a different field. | Aryan |
| 21 | Sera | An AI assistant who is always "absolutely right" pops up every 20 seconds. Her close button says "Send to bottom". | Both |
| 22 | Volume | Hold to raise it; it drops when you let go and slowly decays. | Riya |
| 23 | Interests | Pick exactly 3. "Select all" unticks everything. | Riya |
| 24 | No paste | Pasting is blocked in every field. | Aryan |
| 25 | Newsletter | Pre-ticked. The opt-out reads "No thanks, I enjoy missing out." | Riya |
| 26 | Chicken soundscape | A chicken on a tree bounces around like a DVD logo. Every wall hit adds another screaming chicken and raises the volume, a new chicken joins every 15 hits, and "Pause soundscape" comes back louder. "Use my own sound" swaps in any audio file. | Aryan |

---

## How to beat it

1. **Cookie wall:** Accept all, or fight the toggles in Manage preferences.
2. **Verify it's you:** any email and password; scroll the terms to the very bottom (three times) and untick the box; select all 9 CAPTCHA squares and press Verify; chase Sign in until it gives up.
3. **Name:** press ⌫ until "Your name" is gone, then type on the secure keyboard.
4. **Username:** type anything, then click one of the suggestions.
5. **Email:** type your name backwards, add a domain, and find `.com`.
6. **Interests:** exactly 3.
7. **Password:** leave it empty, or use `MaytitanX🥚997`, which passes all nine rules.
8. **Save:** chase the tiny save link until it gives up, then wait out the progress bar.

---

## How it's built

- One self-contained `index.html` (about 110 KB): HTML, CSS and vanilla JavaScript, no frameworks.
- The chicken photo is embedded as base64, so the page works offline.
- The screaming is synthesized live with the Web Audio API (sawtooth oscillators, formant filters and noise through a limiter). No audio files are needed, and the volume ramp is capped by the limiter.
- Every ragebait is its own function behind a flag in one `CHAOS` config object, which is how `?calm` switches them all off.
- The profile is saved to `localStorage`.
- Fonts: Space Grotesk and Inter from Google Fonts.

## How we prompted it

We built Serene by guiding an AI coding agent with one structured master prompt. Each ragebait is written as a one-line, testable constraint with a built-in escape hatch, so nothing ends up broken. The prompt is split into ROLE, TECH, ON PAGE LOAD, PROFILE FIELDS, SAVING, ALWAYS-ON CHAOS, DESIGN SYSTEM and PROCESS, and the agent self-checks every feature before finishing. The full prompt, the ranking and the fix-prompt template are in [`PROMPTS.md`](PROMPTS.md).
