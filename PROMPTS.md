# Prompts

How Serene was built: two idea lists merged and ranked, one master build prompt for the AI coding agent, one add-on, and a batched fix-prompt template.

## 1. Merged and ranked features

We merged Riya's and Aryan's ideas, dropped duplicates (keeping the more rage-inducing version), cut one idea that was too close to the organisers' banned example, and ranked the rest by rage impact first and build risk second.

| # | Feature | Idea by | Rage (1–5) | Build risk |
|---|---|---|---|---|
| 1 | Runaway Sign in, Username box and Save link | Both | 5 | Low |
| 2 | Fake save: 99% → 12% → "Saved ✅", then a field reverts | Both | 5 | Low |
| 3 | Name typed on a reshuffling on-screen keyboard | Riya | 5 | Low |
| 4 | Phone number slider | Riya | 5 | Low |
| 5 | Birthday from 01/01/0001 with "+1 day" only | Riya | 5 | Low |
| 6 | Typo gremlin (every 4th keystroke) | Aryan | 5 | Med |
| 7 | Password rules, one at a time | Riya | 5 | Med |
| 8 | Huge "Discard" that really discards | Riya | 4 | Low |
| 9 | Rage detector → forced breathing | Aryan | 4 | Low |
| 10 | "I do NOT accept" terms that keep growing | Riya | 4 | Low |
| 11 | Every username "taken" | Aryan | 4 | Low |
| 12 | Toggle betrayal (light mode, phone goes public) | Both | 4 | Low |
| 13 | Inverted brightness slider | Riya | 4 | Low |
| 14 | Cookie wall with sticky "legitimate interest" | Riya | 4 | Med |
| 15 | Split email, typed backwards, 50 endings | Riya | 4 | Med |
| 16 | AI bio: hallucinated, autocorrected, shrinking limit | Both | 4 | Med |
| 17 | Cards shuffle every 8s | Riya | 4 | Med |
| 18 | CAPTCHA where every square is a token | Riya | 3 | Low |
| 19 | Avatar: decoy button, 90°/89° rotation | Both | 3 | Low |
| 20 | Lying labels | Aryan | 3 | Low |
| 21 | Sera, the "absolutely right" AI assistant | Both | 3 | Low |
| 22 | Hold-to-raise volume that decays | Riya | 3 | Low |
| 23 | Pick exactly 3 interests; "Select all" clears | Riya | 3 | Low |
| 24 | Paste blocked | Aryan | 2 | Low |
| 25 | Confirmshaming newsletter | Riya | 2 | Low |
| 26 | Chicken soundscape (add-on) | Aryan | 5 | Med |

## 2. Master build prompt

Each feature is one line with an exact behaviour and a built-in escape hatch, tagged with its rank and source so later fix prompts can refer to it by ID.

```text
ROLE
You are a senior front-end engineer and dark-pattern UX satirist. Build "Serene — the calmest profile page on the internet": a premium "My Profile" settings page where every interaction is deliberate, funny ragebait. It must look beautiful and polished; the rage comes from the contrast. Every feature must stay technically completable: infuriating, never broken.

TECH
- Use the environment's default stack; if none, one index.html with inline CSS and vanilla JS (Google Fonts only, no other libraries).
- Save profile data to localStorage. Zero console errors. Responsive down to 375px; the demo runs on a laptop with a mouse.
- One page only: the sign-in step is a full-screen overlay on this same page. It appears once per page load and never comes back. Nothing anywhere logs the user out.
- Every feature below is its own function with an on/off flag in one CHAOS config object. Loading the page with ?calm in the URL turns all chaos off (for testing). Keep each [#ID · source] tag as a code comment.
- All overlays, toasts and runaway elements live in a top-level layer directly inside <body>. Never put filter, transform or backdrop-filter on <body> or page wrappers (it breaks position: fixed).

ON PAGE LOAD
- [#14 · Riya] Cookie wall over everything: giant glowing "Accept all". A tiny grey "Manage preferences" link opens 10 joke vendors, each with "Consent" and "Legitimate interest" toggles, all pre-ON. Each "Legitimate interest" toggle flips itself back ON once, 1s after being turned off. "Save preferences" closes the wall.
- Then the "Verify it's you" overlay: a premium sign-in card with Email (uses #6), Password (any value works; "Forgot password?" says "Have you tried remembering it? 🌿"), the terms (#10), the CAPTCHA (#18) and a Sign in button (a #1 runaway element). Sign in works only when email and password are filled, the CAPTCHA is passed and the terms are accepted. Then the overlay fades out with "Welcome back! You're absolutely right to be here 🌿".
- [#10 · Riya] Terms: a scrollable terms box and a PRE-CHECKED checkbox "I do NOT accept the Terms and Conditions" that must be unchecked to continue. The checkbox stays disabled until the box is scrolled to the bottom. Each time you near the bottom, more terms are appended (max 3 times).
- [#18 · Riya] CAPTCHA "Select all squares containing a token": a 3×3 grid where all 9 squares show a token 🪙. It passes only when all 9 are selected.

PROFILE FIELDS (glass cards in a responsive grid; header: Serene logo, tagline, and "3 others are editing this profile" with a randomly changing number)
- [#19 · Both] Avatar: a big glowing "Download image" button does nothing except toast "Done! 🌿". The real upload is a tiny "change photo" link. Each click on the photo rotates it 90°. A "Fix rotation" button rotates it 89° (its 3rd click finally resets it to 0°).
- [#3 · Riya] Name: the box holds the real text "Your name", styled like a placeholder, which doesn't clear on focus. Physical typing is blocked ("For your security, use the secure keyboard 🔒"). Input only via an on-screen A–Z + Space + ⌫ keyboard that reshuffles its keys after every press.
- [#11 · Aryan] Username: a #1 runaway element. 1s after you stop typing, it always says "Taken 😔" and offers 3 suggestions built from your text, like "<text>_official_real_notfake_9921". Clicking a suggestion is the only way to set it.
- [#15 · Riya] Email: split into a username box @ a domain box + a TLD dropdown. The username box types backwards (each new character is inserted at the start). The dropdown has 50 TLDs in random order with .com near the bottom.
- [#4 · Riya] Phone: a slider from 0 to 9,999,999,999 showing the formatted number, −1/+1 buttons that accelerate while held, and an "Is this your number?" button that generates a random number.
- [#5 · Riya] Date of birth: starts at 01/01/0001. The only control is a "+1 day" button. Holding it ticks every 50ms, doubling speed every 0.5s up to 5,000 days per tick; releasing resets the speed. Going past today wraps back to 01/01/0001.
- [#16 · Both] Bio: pre-filled by "AI" with a hallucinated bio ("47-year-old Norwegian dentist, 3× national champion in competitive sneezing."). When a word is finished, AI Autocorrect swaps it (student→stunt man, coding→cooking, football→foot ball, love→loaf, engineer→engineered). The character limit starts at 150 and drops by 1 every second while you type, never below 40. Text past the limit is cut off.
- [#7 · Riya] Change password: rules appear one at a time, each only after the previous one passes: 8+ characters → a number → an uppercase letter → digits sum to 25 → a month name → a Roman numeral (uppercase I, V, X, L, C, D or M) → the word "titan" → 🥚 → total length is a prime number (count with [...value].length). Show ✅/❌ for each visible rule.
- [#23 · Riya] Interests: 12 checkboxes; exactly 3 must be picked (Save refuses otherwise). A "Select all" button unchecks everything.
- [#12 · Both] Toggles: "Light mode" turns on (filter: invert(1) hue-rotate(180deg) on <html>), then flips back off after 2s asking "Are you sure?"; choosing Yes keeps it. The first time "Email notifications" is turned off, "Show phone number publicly" turns itself on.
- [#22 · Riya] Volume: hold a button to raise it 1% every 50ms. On release it drops a random 5–20%. It decays 1% per second.
- [#13 · Riya] Brightness: an inverted slider (right = darker) that dims the whole page live, slider included, using a full-screen black overlay with pointer-events: none. Maximum overlay opacity is 0.85.
- [#25 · Riya] Newsletter: pre-ticked "Send me 9 emails a day". The opt-out link reads "No thanks, I enjoy missing out."

SAVING
- [#8 · Riya] A huge glowing "Discard changes" button really discards all unsaved changes without confirmation ("Discarded! You're absolutely right, it wasn't good anyway 🌿"). The real Save is a tiny text link and a #1 runaway element.
- [#2 · Both] Save shows a progress bar that crawls to 99%, drops to 12%, then completes, followed by "Saved ✅". 10 seconds after the first save, ONE random saved field silently reverts (once only) and Sera says "We didn't change anything. Are you feeling okay? 🌿". Beside Save: "Only 2 saves left today", which decreases per save but never actually blocks.

ALWAYS-ON CHAOS
- [#1 · Both] Runaway elements (Sign in button, Username box, Save link): first lift the element into the top-level layer (glass cards' backdrop-filter would trap position: fixed). When the cursor comes within 100px, the element jumps to the mirrored opposite side of the screen (clamped fully on-screen, 250ms transition), leaving a fading ghost copy where it was. After 5 escapes (Save: 3) it stays put and says "Fine. Click me. I don't care."
- [#6 · Aryan] Typo gremlin on the sign-in Email, Username and Bio: in each field, every 4th printable keystroke types an adjacent QWERTY letter instead, and every 10th keystroke also deletes one random earlier character. It never affects Backspace, Delete, arrow keys or password fields. Insert characters manually at the caret and keep the caret position correct.
- [#9 · Aryan] Rage detector: 3+ clicks within 1 second shows a full-screen "Rage click detected 😌 Let's breathe." with a 10-second breathing circle. Any mouse movement restarts the countdown. It triggers at most once per 60 seconds.
- [#21 · Both] Sera, the AI assistant: after sign-in, a bubble pops up every 20s with an overly polite, never-wrong line ("You're absolutely right! I've fixed it 🌿", although it hasn't). Its close button says "Send to bottom" and it returns 20s later. Every toast on the page uses Sera's polite, gaslighting voice.
- [#17 · Riya] Card shuffle: after sign-in, every 8s the profile cards swap grid positions using CSS order only. Never re-create DOM nodes, so focus and typing survive.
- [#20 · Aryan] Lying labels: clicking a label focuses the wrong field (Username label → Bio, Bio label → Email, Email label → Username).
- [#24 · Aryan] Paste is blocked in every field: "Pasting is cheating. Type it like a real human 🌿".

DESIGN SYSTEM
- Dark premium theme: background #0B0B12, surface #151522, primary #7C5CFF, accent #22D3EE, text #F5F5FA.
- Space Grotesk for headings, Inter for body (Google Fonts).
- Glass cards with backdrop blur and 16px radius, glowing hovers, smooth 200–300ms transitions.
- It must look like a real premium SaaS settings page: generous spacing, an aligned grid, polished toasts and modals.

PROCESS
1. Reply first with a 5-line plan.
2. Build everything in one pass.
3. Self-check every feature: confirm it works AND that a user can still complete it. Fix anything broken before you finish.
4. End with a ✅/❌ checklist of all 25 features by #ID.
```

## 3. Add-on prompt

```text
- [#26 · Aryan] Chicken soundscape: a photo of a chicken on a tree bounces around the screen like a DVD logo, above everything else. Every time it touches an edge, the audio multiplies (one more screaming-chicken voice, up to 36) and amplifies (the master volume rises through a limiter), so the longer the user stays, the louder it gets. Sound starts on the first click because browsers block autoplay. Every 15 hits another chicken joins (max 4); a corner hit adds a bonus; clicking a chicken counts as a hit. "Pause soundscape" pauses for 3 seconds and comes back louder. "Use my own sound" lets the user upload an audio file that becomes the multiplied voice.
```

## 4. Fix-prompt template

```text
FIX ROUND — keep everything that already works exactly as it is.

Broken (what happens → what should happen):
- [#__] … → …
- [#__] … → …
- [#__] … → …

Fix all of these in one pass. Don't rewrite, restyle or remove anything that works. Keep zero console errors and keep every feature completable.
If any of these still won't work, remove features completely in this order until the rest is stable: #25, #24, #23, #22, #20, then #17, then #15 (replace it with a normal email field).
Then re-run the self-check and output the ✅/❌ checklist by #ID again.
```
