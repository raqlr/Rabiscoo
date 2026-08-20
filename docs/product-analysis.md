Product analysis — Rabiscoo
(Working draft) — English

1. Product overview

Rabiscoo (branded in the UI as “Desenha Comigo”) is a small single-page web application that runs entirely in the browser. It is an educational drawing game that presents short challenge cards to be read by an adult while a child draws in a canvas area. The game uses a three-level progression (Concreto, Ação, Abstrato), timers per level, a simple star-based reward system, and on-screen guidance for parents.

Evidence:

Title and branding in index.html (index.html:1-10).
Levels definition and cards are defined inlined in index.html (LEVELS constant) (index.html:1520-1608).

2. Target users

Primary users:

Parents / caregivers who read the prompts and evaluate the child's drawing (adult zone). Evidence: on-screen label "Adulto" and instructional onboarding copy (index.html:1228-1238, index.html:1208-1218).
Secondary users:

Young children who interact via touch to draw on the canvas (child zone). Evidence: labeled "Criança — Desenhe aqui!" toolbar and touch event handlers in the JS (index.html:1298-1308, index.html:1620-1636).

3. Main user journey

Onboarding shows three cards explaining how to play and parental guidance. Evidence: onboarding stack and obNext() (index.html:1200-1236, index.html:2068-2088).
Adult reads a card displayed in the Adult Zone. Evidence: #card-text and cardMeta elements (index.html:1234-1242, index.html:1256-1266).
Child draws on the canvas within a timed window (level-specific duration). Evidence: canvas area and startTimer() using LEVELS[currentLevelIdx].time (index.html:1268-1298, index.html:1678-1688).
When time expires the canvas locks and the adult evaluates ("Acertou!" or "Tentar novamente"). Evidence: lockCanvas() / handleCorrect() / handleRetry() (index.html:1640-1656, index.html:1796-1816).
Correct answers earn stars; 5 stars unlock next level; game ends after 3 levels with a final victory screen. Evidence: star logic, levelStars, answeredCount, showLevelUp() and showFinalVictory() (index.html:1560-1576, index.html:1872-1898).

4. Existing features

Onboarding with parent guidance and three cards (index.html:1200-1236).
Two-zone UI: Adult Zone (prompts, timer, evaluation) and Child Zone (canvas, toolbar) (index.html:1218-1246, index.html:1268-1310).
Canvas drawing with touch and mouse support, pencil/eraser, color and size selection, clear canvas (index.html:1600-1670, index.html:1708-1736).
Timer per level with visual bar and warnings at 10s/5s (index.html:1698-1722, index.html:1710-1728).
Level progression system with 3 levels and per-level card sets defined in code (index.html:1520-1608).
Retry flow for missed cards and basic deck shuffling (index.html:1748-1764, index.html:2026-2038).
Celebration overlays: level-up, final victory, confetti, star animations (index.html:1320-1356, index.html:1848-1888).
Simple scoring: one star per correct card; 5 stars to level up (index.html:1560-1576).
Project title and site link in README.md (README.md:1-3).

5. Strengths

Extremely simple, focused experience appropriate for parent–child interaction; minimal UI friction for touch devices. Evidence: compact two-column layout, large buttons, touch handlers (index.html:8-16, index.html:1608-1624).
All logic in a single file: easy to reason about and modify for prototypes. Evidence: full app implemented inline in index.html (index.html:1-30, index.html:1600-1620).
Strong onboarding and caregiver guidance text encouraging supportive evaluation rather than strict correctness. Evidence: onboarding cards content (index.html:1216-1248).

6. Problems or friction points

Accessibility: no apparent keyboard navigation, ARIA attributes, or high-contrast options. Evidence: raw HTML elements and no ARIA roles in index.html (scan).
Internationalization / language lock: UI text is Portuguese (lang="pt-BR"), limiting reach. Evidence: html lang="pt-BR" and Portuguese strings throughout (index.html:1-4, index.html:1208-1218).
Data persistence: progress and scores are only in-memory; refresh loses state. Evidence: no use of localStorage or backend calls in JS (scan).
Testability & maintainability: monolithic single-file implementation can grow hard to maintain. Evidence: CSS, HTML, and JS all inline in index.html (scan).
Evaluation subjectivity: adult acts as the grader, which is by design, but there is no guidance on scoring criteria beyond binary correct/try again. Evidence: handleCorrect() only increments stars with no rubrics (index.html:1796-1816).
Mobile viewport: layout assumes 2-column; although responsive CSS exists, smaller screens may have cramped adult controls. Evidence: two-column #app with fixed #adult-zone width 260px (index.html:34-42).

7. Opportunities for improvement

Persistence: add optional localStorage to keep level and star progress between sessions (low effort, high value). Rationale: keeps families motivated across sessions.
i18n: extract UI strings and add simple language switcher (medium effort). Rationale: extend to non‑Portuguese families.
Accessibility enhancements: keyboard controls for adult actions, ARIA labels on buttons and canvas, color-contrast checks (high importance).
Grading guidance: provide optional evaluation hints for adults (e.g., "Recognized shape", "Attempted details") to reduce subjectivity (medium effort).
Modularize code: split JS into modules and move CSS to separate file for maintainability (longer-term refactor).
Analytics opt‑in: simple opt‑in telemetry (events: level completed, time spent) to measure engagement (requires privacy considerations).

8. Recommended priorities (value / effort)

Add localStorage persistence for currentLevelIdx, totalStars, and deck (High value / Low effort).
Fix basic accessibility gaps (ARIA, focus states, keyboard flow) (High value / Medium effort).
Add simple evaluation guidance for adults (Low/Medium effort depending on UX) (Medium value / Medium effort).
Extract UI strings for i18n (Medium value / Medium effort).
Split code into separate files and add basic build tooling only when planning larger feature work (Long term).

9. Open questions

Is offline-first operation and local persistence acceptable? Any privacy constraints on storing progress?
Who is the intended first market / language? Keep Portuguese-only or plan bilingual launch?
Are there future plans for accounts, sharing drawings, or remote teacher reviews?
Should the adult evaluation include structured rubrics or remain a simple binary flow?
Summary & recommended first action

Rabiscoo is a tightly focused parent–child drawing game implemented as a single-page app. Its core strengths are simplicity and a UX designed for parental guidance. The top friction is lack of persistence and accessibility gaps. I recommend tackling localStorage persistence first (store current level, earned stars, and optionally recent drawings). It's small to implement, immediately increases retention and perceived progress, and preserves the playful progression that already exists.

Notes: all factual claims cite index.html (app implementation) and README.md (project name).

--- END OF FILE