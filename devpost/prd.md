---
doc: prd
status: approved
---

# Sugar — Product Requirements

A cute but savage conversational coding tutor that guides students through practical Python and SQL projects step-by-step, playfully roasting mistakes to help them break out of tutorial hell.
Source: `scope.md > The Unique Kernel` and `scope.md > The Core Loop`.

## The Core Journey
1. **Welcome & Loading Screen:** The user lands on Sugar and is greeted by an animated loading/splash screen with a playful mascot introducing Sugar's personality.
2. **Track Selection:** The user arrives at the main view and chooses one of two curated, practical tracks:
   - **Python Track:** Build a 3-step **Quiz Application** (Lists, Loops, Conditions, Score Calculation).
   - **SQL Track:** Build a 3-step **Event Management System** (Tables, Insertions, Filtering Queries).
3. **Interactive Step-by-Step Build:**
   - Sugar introduces Step 1 with a bite-sized, plain-English explanation of the concept and assigns the task.
   - The student types code directly into the chatbox and submits it.
4. **Evaluation & The Savage Feedback Loop:**
   - **Incorrect Code:** Sugar fires back with a witty, friendly roast and nudges them toward the right logic.
   - **Correct Code:** Sugar celebrates the win, confirms the snippet, and advances to the next step.
   - **Keyword `hint`:** Sugar provides a conceptual clue without giving away the code.
   - **Keyword `help`:** Sugar provides a working code snippet example to unblock the student.
   - **Gibberish / Empty:** Sugar redirects the mistake in character: *"That input looks like a recipe for confusion. Let's stick to the code so we can finish this project."*
5. **Project Completion & Take-Home Challenge:**
   - After completing all 3 steps, Sugar displays the full assembled working project code and crowns the student a graduate.
   - Sugar presents an uninspected, independent take-home project challenge:
     - Python: **Password Strength Checker**
     - SQL: **Concert Management System**

## Screens and Layout
A single-surface, highly responsive web application with two distinct phases:
- **Surface 1: Welcome / Splash Screen:**
  - Cute animated mascot and welcome greeting setting Sugar's "cute but savage" tone.
  - Smooth automated or tap-to-continue transition into the workspace.
- **Surface 2: Main Workspace & Chat Interface:**
  - **Header:** Sugar branding, current status badge, and quick track switcher (Python vs. SQL).
  - **Chat Stream:** Chronological message list displaying Sugar's explanations, roasts, hints, and user responses with clear styling distinction.
  - **Input Bar:** Multiline-capable code input box, "Submit" button, and quick-action chips for `hint` and `help`.

## Look and Feel
- **Color Palette (Vintage Brown & Muted Rose):** Inspired by the vintage palette reference (`https://pin.it/1uut2kuAF`):
  - Primary Background: Soft warm cream / almond latte (`#F7F3EE`)
  - Surface Cards: Clean ivory / warm white (`#FFFFFF`) with subtle warm borders (`#E8DFD5`)
  - Accent Muted Rose / Dusty Pink: Soft vintage pink (`#D88A8A` / `#F0C2C2`)
  - Brand Browns: Rich mocha (`#54382B`) and warm chocolate (`#3A231A`) for strong, legible typography
  - Feedback Accents: Savage roast badge (`#C84B4B`), Success sage (`#5E8A75`)
- **Typography:** Warm, rounded, modern typography (e.g., Plus Jakarta Sans / Inter) paired with a clean monospace font for code blocks.
- **Vibe:** "Normal cute"—retro-chic, cozy, warm, and inviting without feeling childish or neon.

## Features and Behavior

### Track 1: Python — Quiz Application (Guided)
- **Step 1: Data Setup:** Create a list of question dictionaries with choices and correct answers.
- **Step 2: Loop & User Input:** Loop through questions and capture user input.
- **Step 3: Condition & Score:** Compare user answers against correct values, increment score, and print final score.
- **Success Criteria:** All 3 steps submitted correctly; full Python code snippet rendered.

### Track 2: SQL — Event Management System (Guided)
- **Step 1: Schema Definition:** `CREATE TABLE events` with ID, name, date, and venue.
- **Step 2: Data Population:** `INSERT INTO events` sample event records.
- **Step 3: Querying & Filtering:** `SELECT` upcoming events filtered by date/venue.
- **Success Criteria:** All 3 steps submitted correctly; full SQL script rendered.

### Feedback & Roast Engine
- Evaluates student input against expected patterns for the current step.
- Contextual roasts for common slip-ups (e.g. missing colons, wrong keywords, syntax slips).
- Instant recognition of special triggers:
  - `hint` -> Conceptual explanation (at most one emoji).
  - `help` -> Code snippet reference.
  - Non-code gibberish -> *"That input looks like a recipe for confusion. Let's stick to the code so we can finish this project."*
  - Strict compliance with `VOICE.md`: Roasts tease the query/code, never the student; max one roast line followed by the fix; no emojis on errors; gentle reveal after 3 tries.

## States and Boundaries
- **Splash / Loading State:** Displays mascot, friendly intro, and smooth entry.
- **Track Selection State:** Clean choice between Python Quiz App and SQL Event System.
- **Active Step State:** Shows current step (1 of 3, 2 of 3, 3 of 3) with prompt.
- **Roast / Error State:** Shows Sugar's humorous rebuke with hint and retry option.
- **Completion State:** Full code view + delivery of the take-home challenge (Password Strength Checker / Concert Management System).
- **Reset State:** User can switch tracks or restart from Step 1 at any time.

## Product Decisions
- **Curated Projects vs. Dynamic Topics:** Pre-selected Quiz App and Event System to guarantee immediate, bug-free, high-value learning without prompt hallucinations.
- **Direct Chat Input:** Code typed directly into chatbox for zero setup friction.
- **Dual Support Keywords:** Explicit `hint` (clue) and `help` (snippet) to keep students from getting stuck.
- **Uninspected Take-Home:** Gives students a post-hackathon goal without bloating the POC with grading infrastructure.

## What We're Building
- Complete single-page web app with the splash screen and chat workspace.
- Working Python Quiz App track (3 steps).
- Working SQL Event Management track (3 steps).
- Sugar's roasting engine with custom replies, hints, snippets, and gibberish detection.
- Complete vintage brown & muted rose design system.

## Deferred From the POC
- In-browser code runner / execution engine (Pyodide / SQLite WASM).
- Automated inspection / grading of the take-home challenges.
- User accounts and persistent progress tracking across browser refreshes.

## Non-Goals
- Building an IDE with multi-file management or terminal tabs.
- Adding arbitrary uncurated language tracks.
- Softening Sugar's personality into a generic polite corporate assistant.

## Open Questions
- None blocking spec approval; all core behaviors, states, and visuals are resolved.
