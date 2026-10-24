---
doc: checklist
status: approved
---

# Build Checklist

Build mode: fast

## Slices

- [ ] **1. Scaffolding, Vintage Theme, Splash Screen & Live Chat Interface**
  Becomes usable: A running single-page web app with the vintage brown & muted rose palette, an animated splash screen with Sugar's mascot and greeting, smooth transition into the chat workspace, track selection, and a multiline chatbox where student messages render live in the chat stream.
  Why now: Bootstraps the project, establishes the visual identity from the approved PRD/spec, and provides the complete user interface for all subsequent logic and engine hooks.
  PRD ref: `prd.md > Screens and Layout`, `prd.md > Look and Feel`, `prd.md > The Core Journey` (Steps 1 & 2)
  Spec ref: `spec.md > Components (SplashScreen, ChatWorkspace, InputController)`, `spec.md > Look and Feel`, `spec.md > File Structure`
  Build: Create `index.html`, `css/main.css`, `css/vintage-theme.css`, and `js/app.js`. Implement splash screen entry, track picker, multiline textarea with Ctrl+Enter support, and chat bubble rendering.
  Verify (mechanical): Start a local static server, load `index.html`, verify splash screen renders mascot and title, click start to reveal chat, type and submit a message, verify message bubble renders in chat stream.
  Learner check: Open `index.html` in your browser, check the vintage aesthetic and splash screen, click into the chat, submit a message, and confirm the layout feels welcoming.
  Commit: `Bootstrap app with splash screen and vintage chat interface`

- [ ] **2. Voice & Roast Engine with VOICE.md Guardrails**
  Becomes usable: Sugar responds conversationally to messages! Non-code input gets redirected with Sugar's café prompt, typing `hint` returns conceptual clues, `help` returns working code snippets, and repeated errors track toward a solution reveal on the 3rd attempt.
  Why now: The unique kernel (Sugar's older-sibling roast personality and strict `VOICE.md` rules) is built early and verified before adding complex database execution.
  PRD ref: `prd.md > Feedback & Roast Engine`, `prd.md > The Core Journey` (Step 4), `VOICE.md`
  Spec ref: `spec.md > Components (VoiceAndRoastEngine)`, `spec.md > Data Model`
  Build: Create `js/voice.js` implementing all dialogue rules from `VOICE.md`: query-roasting (teasing code mistakes, not the student), no emojis on errors, max one roast line followed by plain-English fix, `hint` clues, `help` snippets, and the 3rd attempt reveal. Wire into `js/app.js`.
  Verify (mechanical): Submit `asdf` to verify the confusion prompt; submit `hint` to verify conceptual clue; submit `help` to verify code snippet; trigger 3 error responses to verify the gentle reveal.
  Learner check: Type `hint`, `help`, and gibberish into the chat, and verify Sugar's personality is cheeky, helpful, and strictly follows `VOICE.md`.
  Commit: `Add voice and roast engine adhering to VOICE.md`

- [ ] **3. SQL Engine Integration & 3-Step Event Management Track**
  Becomes usable: Full end-to-end interactive SQL learning experience! `sql.js` initializes in browser memory. Step 1 (`CREATE TABLE events`), Step 2 (`INSERT INTO events`), and Step 3 (`SELECT`) validate authentic SQLite execution. Real SQL syntax errors trigger Sugar's witty roasts; correct queries advance steps; completing Step 3 displays the full working script and delivers the Concert Management System take-home challenge.
  Why now: Delivers the primary proof of concept and complete 60-second video demo finish line.
  PRD ref: `prd.md > Track 2: SQL — Event Management System`, `prd.md > The Core Journey` (Steps 3-5)
  Spec ref: `spec.md > Components (SQLEngine)`, `spec.md > External Services and Dependencies`
  Build: Create `js/sql-engine.js` integrating `sql.js` (WebAssembly SQLite). Implement step validation logic for `CREATE TABLE events`, `INSERT INTO events`, and `SELECT` queries. Connect to `voice.js` and `app.js`.
  Verify (mechanical): Submit `MAKE TABLE` and verify SQLite error is caught and roasted; submit valid `CREATE TABLE events` and verify step advances; submit valid `INSERT INTO events` and advance; submit valid `SELECT` and verify project completion with full code display and take-home challenge.
  Learner check: Play through the 3-step Event Management System in your browser, test a deliberate syntax error to see Sugar roast your query, then complete the project.
  Commit: `Implement sql.js engine and 3-step Event Management project`

- [ ] **4. Python Quiz App Track (Pyodide Layer) & Final Polish**
  Becomes usable: Switch between SQL and Python tracks seamlessly. The Python track runs the 3-step Quiz Application using in-browser WebAssembly execution (`Pyodide`), verifying lists, loops, and conditions with native Python error roasting and the Password Strength Checker challenge.
  Why now: Completes the second planned track after the core SQL engine is verified and stable.
  PRD ref: `prd.md > Track 1: Python — Quiz Application`, `prd.md > States and Boundaries`
  Spec ref: `spec.md > Components (PythonEngine)`, `spec.md > External Services and Dependencies`
  Build: Create `js/python-engine.js` integrating `Pyodide` with safe execution timeouts. Implement step validation for question lists, loops/inputs, and score calculations. Polish UI transitions, reset buttons, and mobile responsiveness.
  Verify (mechanical): Switch track to Python, submit invalid syntax, verify Python exception is roasted; submit valid Quiz App snippets across all 3 steps, verify completion and Password Strength Checker prompt.
  Learner check: Switch between SQL and Python tracks in the browser, try the Python quiz app steps, and confirm the entire app feels cohesive and ready for GitHub Pages.
  Commit: `Add Python Quiz App track and final experience polish`

## Hands-on Checkpoints

- [ ] Early usable behavior explored — Slice 2 (voice and roast personality live in chat)
- [ ] Final kick-the-tires exploration and feedback completed

## Final Review

- [ ] Final review complete — feedback resolved and learner confirms ready to ship

## Code Tour and App Map

- [ ] Learning activity complete — guided route, focused alternative, prior practice connected, or brief recap
- [ ] Optional edit and transfer reflection addressed — offered/declined/already covered/not applicable as appropriate
- [ ] `devpost/app-map.html` generated from finished code, checked, and shown, including a project-grounded practice to reuse

Activity and evidence: [to be recorded upon completion]
Route and stops: [to be recorded upon completion]
Edit outcome: [to be recorded upon completion]
Reflection: [to be recorded upon completion]
Activity mode: [to be recorded upon completion]

## Revisions
