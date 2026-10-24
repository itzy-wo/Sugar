---
doc: spec
status: approved
---

# Sugar — Technical Specification

## How This Works, In Plain Language
Sugar is a static single-page web app built with vanilla HTML, CSS, and JavaScript that runs entirely inside the user's web browser. It has no backend server or cloud database. 

Instead of brittle regex string-matching, Sugar uses real client-side execution engines compiled to WebAssembly:
1. **For SQL:** It runs `sql.js` (an in-memory SQLite database in WebAssembly) directly in the browser. When the student types a SQL command, `sql.js` executes it against a live database. If the table name is wrong or syntax breaks, SQLite's native error is captured and translated into one of Sugar's playful roast lines (following `VOICE.md`).
2. **For Python (Sequenced Layer):** It uses `Pyodide` (Python compiled to WebAssembly). The student's code runs in a sandboxed Python runtime in browser memory, and Sugar verifies the actual output and behavior. Python syntax and name errors map directly to Sugar's roasts.

To handle the ~10MB WebAssembly load seamlessly, Sugar uses a cute animated splash screen that preloads the runtime in the background while greeting the user. All state (current step, attempt counter, chat history) lives in plain JavaScript variables in memory. The site publishes directly to **GitHub Pages** with zero setup.

## The Core Journey Through the System
1. **Page Load & Runtime Initialization:** The browser loads `index.html`. The splash screen activates, displaying Sugar's mascot and greeting while initializing `sql.js` asynchronously in the background.
   - *PRD ref:* `prd.md > The Core Journey (Step 1: Welcome & Loading Screen)`.
2. **Track Selection:** The user enters the main chat interface. They select the **SQL Event Management** track (sequenced first) or **Python Quiz App** track.
   - *PRD ref:* `prd.md > The Core Journey (Step 2: Track Selection)`.
3. **Step Prompt & Input:** Sugar renders Step 1 in the chat stream. The user enters code in the multiline input and hits Send.
   - *PRD ref:* `prd.md > The Core Journey (Step 3: Interactive Step-by-Step Build)`.
4. **Execution & Evaluation:**
   - The input is inspected: if it's `hint`, `help`, or empty/gibberish, the helper/guard handler fires immediately.
   - Otherwise, the code is passed to the runtime evaluator (`sql.js` or `Pyodide`).
   - If execution throws an error: the error type and message are passed to the roast engine in `voice.js`, which generates one short roast line teasing the mistake followed by the fix.
   - If 3 consecutive failures occur: Sugar drops the jokes and reveals the solution with encouraging guidance.
   - If execution succeeds and meets the step acceptance criteria: Sugar celebrates and advances the state machine from Step 1 → 2 → 3.
   - *PRD ref:* `prd.md > The Core Journey (Step 4: Evaluation & The Savage Feedback Loop)`.
5. **Project Completion:** At Step 3 completion, Sugar prints the full assembled project code in the chat, marks the project complete, and delivers the uninspected take-home challenge.
   - *PRD ref:* `prd.md > The Core Journey (Step 5: Project Completion & Take-Home Challenge)`.

## Stack
- **Frontend Core:** Vanilla HTML5, CSS3, and ES6+ JavaScript.
  - *Rationale:* Zero build tools, zero npm dependency rot, instant local testing, and clean hosting on GitHub Pages.
- **SQL Engine:** `sql.js` (WebAssembly SQLite build).
  - *Docs:* [https://sql.js.org/](https://sql.js.org/)
  - *Rationale:* Executes authentic SQL in memory in the browser. Catches real database syntax and table errors.
- **Python Engine:** `Pyodide` v0.26+ (WebAssembly Python runtime).
  - *Docs:* [https://pyodide.org/](https://pyodide.org/)
  - *Rationale:* Real Python evaluation testing functional behavior rather than text strings. (Sequenced after SQL).
- **Styling & Icons:** Vanilla CSS Custom Properties based on the Vintage Brown & Rose palette (`#F7F3EE` background, `#54382B` mocha text, `#D88A8A` vintage rose, `#E8DFD5` borders). Native SVG mascot and icons.
- **Hosting:** GitHub Pages (static publishing from `main` branch root).

## Where It Runs and How Someone Tries It
- **Runtime:** Any modern evergreen desktop or mobile browser (Chrome, Safari, Firefox, Edge) supporting WebAssembly.
- **Local Run / Dev:**
  - Double-click `index.html` or run a local static server:
    ```bash
    npx serve .
    # or python -m http.server 8000
    ```
  - Open `http://localhost:8000` (or `http://localhost:3000`).
- **Demo Recording Flow:**
  - Open the browser to the loaded page.
  - Select SQL Event System track.
  - Submit an intentional mistake (e.g., misspelled table or missing keyword) to show Sugar's roast and fix.
  - Submit the correct query to advance.
  - Demonstrate `hint` and `help` commands.
  - Complete the 3 steps to show full project output and the take-home challenge.
- **Live Deployment:** GitHub Pages enabled under repository `Settings > Pages > Branch: main / root`.

## Look and Feel
- **Color Tokens:**
  - `--bg-page`: `#F7F3EE` (warm almond cream)
  - `--bg-card`: `#FFFFFF` (ivory white)
  - `--border`: `#E8DFD5` (warm latte line)
  - `--text-main`: `#3A231A` (dark chocolate roast)
  - `--text-muted`: `#7A6258` (warm sepia)
  - `--accent-rose`: `#D88A8A` (vintage dusty pink)
  - `--accent-rose-soft`: `#F9EBEA` (subtle blush bubble)
  - `--accent-mocha`: `#54382B` (rich cocoa)
  - `--accent-sage`: `#5E8A75` (celebration success green)
- **Typography:** System sans-serif stack (`-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif`) with monospace font for code blocks (`SFMono-Regular, Menlo, Monaco, Consolas, monospace`).
- **Tone & Copy:** Governed strictly by `VOICE.md`. Warm older-sibling vibe, small café/sweets comparisons, maximum 1 roast line, no emojis on error messages.

## Components

### Component 1: SplashScreen (`SplashScreen.js`)
- *Role:* Renders the mascot animation, presents the warm "welcome hehe :0" intro, and coordinates background preloading of `sql.js` (and Pyodide). Automatically unlocks the "Start Learning" entry once the WASM runtime is warm.
- *PRD ref:* Implements `prd.md > Screens and Layout (Surface 1)` and `prd.md > The Core Journey (Step 1)`.

### Component 2: ChatWorkspace (`ChatWorkspace.js`)
- *Role:* Manages the main chat stream layout: renders message bubbles (Sugar prompt, student submission, roast, hint, celebration, and final code block), handles autoscroll, and hosts the track selector badge.
- *PRD ref:* Implements `prd.md > Screens and Layout (Surface 2)` and `prd.md > Features and Behavior`.

### Component 3: InputController (`InputController.js`)
- *Role:* Controls the multiline code textarea, submit button, keyboard shortcuts (`Ctrl+Enter` / `Cmd+Enter`), and quick-action chips for `hint` and `help`.
- *PRD ref:* Implements `prd.md > Screens and Layout (Input Bar)`.

### Component 4: SQLEngine (`SQLEngine.js`)
- *Role:* Wraps `sql.js`. Maintains the in-memory SQLite database instance. Executes student SQL commands for the 3 steps of the Event Management System:
  - Step 1: Validates table creation (`events` table with required columns).
  - Step 2: Validates data insertion (`INSERT INTO events`).
  - Step 3: Validates query results (`SELECT` filtered by criteria).
- *PRD ref:* Implements `prd.md > Track 2: SQL — Event Management System (Guided)`.

### Component 5: PythonEngine (`PythonEngine.js` - Phase 2 Layer)
- *Role:* Wraps `Pyodide`. Evaluates the Quiz Application steps by executing code in Python's namespace and verifying output behavior.
- *PRD ref:* Implements `prd.md > Track 1: Python — Quiz Application (Guided)`.

### Component 6: VoiceAndRoastEngine (`VoiceAndRoastEngine.js`)
- *Role:* The heart of Sugar's personality. Inspects runtime errors, syntax issues, input keywords, and attempt counts. Returns vetted dialogue following all rules in `VOICE.md`:
  - Roasts the query/code, never the person.
  - Delivers conceptual clues for `hint`.
  - Delivers working snippets for `help`.
  - Redirects gibberish/empty input with Sugar's café-themed prompt.
  - Drops roasts and reveals the solution on attempt 3.
- *PRD ref:* Implements `prd.md > Feedback & Roast Engine` and `VOICE.md`.

## Data Model
All application state is maintained client-side in an in-memory state object:
```javascript
const SugarState = {
  currentTrack: 'sql',      // 'sql' | 'python'
  currentStep: 1,           // 1, 2, or 3
  stepAttempts: 0,          // resets to 0 on step advancement
  isWasmReady: false,       // true when sql.js is initialized
  chatHistory: [            // array of message objects
    { sender: 'sugar' | 'user', type: 'text' | 'code' | 'roast' | 'success', text: '...' }
  ],
  completedCode: []         // accumulated working code snippets from completed steps
};
```
- *Persistence:* In-memory. Refreshing the browser resets the session, which guarantees clean, reproducible 60-second video demo recordings.

## File Structure
```
sugar/
├── index.html              # Main HTML markup: splash screen and chat workspace
├── css/
│   ├── main.css            # Base styles, typography, reset, layout
│   └── vintage-theme.css   # Color palette variables, bubbles, cards, animations
├── js/
│   ├── app.js              # Application entry point, state coordination
│   ├── voice.js            # Sugar's dialogue, roasts, hints, and VOICE.md rules
│   ├── sql-engine.js       # sql.js wrapper and Event System step validation
│   └── python-engine.js    # Pyodide wrapper for Quiz App (Phase 2 layer)
├── devpost/                # Hackathon planning artifacts
│   ├── learner-profile.md
│   ├── scope.md
│   ├── prd.md
│   └── spec.md
├── VOICE.md                # Canonical voice & personality guide
└── .gitignore              # Ignores local credentials and profile
```

## External Services and Dependencies
- **`sql.js` (v1.11.0):** Loaded via CDN (`https://cdnjs.cloudflare.com/ajax/libs/sql.js/1.11.0/sql-wasm.js` and `sql-wasm.wasm`). Zero cost, public domain / MIT license.
- **`Pyodide` (v0.26.2):** Loaded via CDN (`https://cdn.jsdelivr.net/pyodide/v0.26.2/full/pyodide.js`). Zero cost, MPL 2.0 license.
- **External APIs / Databases / Keys:** **NONE.** No API keys, no server costs, no rate limits.

## Important Failure Modes
1. **WASM CDN Loading Failure / Offline:**
   - *Fallback:* If CDN WebAssembly fails to load, Sugar's splash screen shows a friendly retry button and gracefully switches to offline simulated evaluation mode so the demo never hangs.
2. **Infinite Loop in User Code (Python):**
   - *Fallback:* Code execution wrapped in a safe timeout guard. If execution takes > 3 seconds, Sugar flags the loop: "That loop is spinning forever. Check your stopping condition."
3. **Repeated Errors (Frustration Barrier):**
   - *Fallback:* Tracked via `SugarState.stepAttempts`. On attempt 3, Sugar automatically triggers the reveal flow from `VOICE.md` to guarantee the learner never gets trapped.

## What Was Simplified and Why
- **In-Memory SQLite (`sql.js`) instead of PostgreSQL / Backend Server:** Eliminates server hosting, connection errors, and database credential management while preserving 100% authentic SQL execution in the browser.
- **Sequenced Track Delivery (SQL First, Python Second):** Focuses the build on completing an airtight SQL Event System with full roasts, hints, and reveals first, protecting against hackathon deadline crunch.
- **Client-Side State instead of Persistent User Accounts:** Avoids database migrations and authentication setup, allowing any reviewer or judge to open the link and start playing immediately.

## Decisions and Open Issues
- **Learner Decision — Stack:** Vanilla HTML/CSS/JS without build tools or framework bloat.
- **Learner Decision — Execution:** Real WebAssembly execution (`sql.js` + `Pyodide`) instead of brittle regex, pairing native error messages with Sugar's roasts.
- **Learner Decision — Sequencing:** Build SQL Event Management track completely first. Layer Python Quiz App once SQL is verified.
- **Learner Decision — Hosting:** GitHub Pages directly from repository `main` branch.
- **One Useful Unknown Resolved:** Addressed the 10MB WASM load time by utilizing Sugar's splash screen as a warm background preloader, ensuring the chat interface responds instantly without awkward lag.
- **Open Questions:** None blocking build.
