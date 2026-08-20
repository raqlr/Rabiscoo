# UX Analysis — Rabiscoo

This report evaluates Rabiscoo's user experience from the perspectives of caregivers (adults) and children. It draws on the app implementation and the Product Manager's analysis.

References used: `index.html` and `docs/product-analysis.md` ([index.html](index.html#L1-L10), [docs/product-analysis.md](docs/product-analysis.md)).

## 1. UX overview

Rabiscoo (UI: "Desenha Comigo") is a focused parent–child drawing game where an adult reads a prompt and a child draws within a timed window. The experience is intentionally minimal: three levels of increasing abstraction, a simple star system, and a hand-off evaluation by the adult. (See level definitions in `index.html`.)

## 2. Adult / caregiver experience — Observations

- The UI explicitly labels an `Adult Zone` with the current prompt, timer, and evaluation buttons, making the adult role explicit ([index.html](index.html#L1218-L1246)).
- Controls ("Acertou!", "Tentar novamente", restart) are prominent and use clear language for Portuguese speakers ([index.html](index.html#L1248-L1260)).
- The onboarding cards provide guidance about supportive evaluation and suggested phrases to use with the child ([index.html](index.html#L1200-L1236)).
- There is no structured guidance for scoring or evaluating partial success — the adult's role is binary (correct / retry) which can produce inconsistency between caregivers. See `handleCorrect()` logic in `index.html` and product analysis notes ([index.html](index.html#L1796-L1816), [docs/product-analysis.md](docs/product-analysis.md)).

## 3. Child experience — Observations

- The `Child Zone` is visually labeled and includes a large canvas and a simple toolbar (pencil, eraser, sizes, colors). Touch/mouse input is supported and placeholders invite interaction ([index.html](index.html#L1268-L1310), [index.html](index.html#L1600-L1670)).
- The drawing experience supports basic tools and clear affordances for clearing and starting to draw. There is a time-up overlay that communicates when drawing time ends ([index.html](index.html#L1288-L1304), [index.html](index.html#L1316-L1336)).
- There is limited in-experience guidance for children about what to draw beyond the prompt text (no visual examples, no difficulty adjustments per age). The prompts are short text strings in Portuguese ([index.html](index.html#L1520-L1560)).

## 4. Onboarding — Observations

- Onboarding is immediate and concise: three cards explain the three steps, encourage supportive tone, and suggest phrases for adults to use ([index.html](index.html#L1200-L1236)).
- The flow dismisses after the last card; there is a review modal accessible later. Onboarding content is caregiver-facing — not designed to teach children directly.

## 5. Core gameplay flow — Observations

- Flow is clear: adult reads, child draws within a timer, adult evaluates. Feedback is a celebration overlay with stars/confetti for correct answers ([index.html](index.html#L1728-L1760), [index.html](index.html#L1848-L1888)).
- Progression is linear and transparent: 5 correct answers = level up; 3 levels total. The game resets or allows replay after final victory ([index.html](index.html#L1520-L1608), [index.html](index.html#L1872-L1898)).

## 6. Strengths (UX)

- Role clarity: adult vs child roles are clearly separated and labeled.
- Low interaction cost: few buttons and large touch targets reduce friction for parents and children.
- Positive reinforcement: confetti and star animations provide celebratory feedback.
- Strong onboarding tone: encourages supportive caregiving, aligning with educational goals.

## 7. UX problems and friction — Observations

- Subjective scoring: adult-only binary scoring can cause inconsistent outcomes and may make adults feel like judges rather than facilitators ([index.html](index.html#L1796-L1816)).
- Lack of persistence: progress is lost on refresh which reduces sense of continuity and replay motivation ([docs/product-analysis.md](docs/product-analysis.md)).
- Sparse guidance for children: prompts are text-only; pre-literate children or younger ages may struggle without visual cues or examples.
- Accessibility gaps: no keyboard navigation hints, no visible focus states, no ARIA attributes, and color choices not explicitly checked for contrast ([docs/product-analysis.md](docs/product-analysis.md)).
- Adult controls fixed-width may crowd small screens; the two-column layout can feel compressed on narrow mobile devices ([index.html](index.html#L34-L42)).

## 8. Opportunities & recommendations (UX)

Observations are followed by concrete recommendations grouped by priority.

High priority (impact / low-to-medium effort)
- Add a lightweight evaluation rubric or micro-prompts for adults: provide 2–3 checkboxes (e.g., "Recognizable", "Effort/Details", "Creative") that map to 0–3 micro-stars. This keeps adult judgement structured and educational rather than purely binary.
- Persist progress locally (e.g., `localStorage`) so families return to their level and stars. This increases retention and perceived competence — small UX change with immediate benefit.
- Improve accessibility: add ARIA labels, ensure focus styles, and make evaluation buttons keyboard-accessible. Test color contrast and provide a high-contrast toggle.

Medium priority
- Add optional visual examples or icons alongside prompts for younger children (age 3–5). Keep examples optional to avoid giving answers away to older children.
- Make onboarding optionally child-facing: a very short animated demo or an audio read-aloud of the first prompt to help pre-literate children understand.

Lower priority / longer term
- Provide an activity history or session summary so adults can see progress over sessions and share milestones with caregivers.
- Allow difficulty/age selection to adapt prompt complexity and time limits.

## 9. Recommended priorities (UX)

1. Structured adult evaluation (rubric) — reduces subjectivity and aligns adult actions with learning goals. (High impact / Low effort to prototype UI)
2. Local persistence of progress — increases retention and replay motivation. (High impact / Low effort)
3. Accessibility fixes (focus, ARIA, keyboard interactions) — ensures inclusivity and legal accessibility requirements. (High impact / Medium effort)

## 10. Questions for user testing

- Do parents understand what qualifies as "Acertou"? Run a 5-minute moderated test where a parent uses the app to evaluate 3 drawings and explain their reasoning.
- Can children (ages 3–6) follow a prompt without examples? Test with an unprompted draw session and observe confusion points.
- Does persistence change session frequency? A small A/B test with and without `localStorage` will indicate retention lift.

---

## Final recommendation: top 3 UX improvements

1. Structured adult evaluation (rubric)
- Why: Reduces inter-rater variability, frames the adult as a facilitator, and enhances learning signals. Small UI additions (3 checkboxes + mapping to stars) should be easy to prototype.

2. Local progress persistence
- Why: Keeps families motivated across sessions, preserves sense of progress, and increases replayability with minimal technical/UX risk.

3. Accessibility improvements
- Why: Ensures the app is usable by keyboard users and those who rely on assistive tech; fixes improve clarity for all users and address an important inclusion requirement.

Each of these changes is aligned with the app's pedagogical intent and can be prototyped quickly without major redesign.

---

File generated by UX review. For implementation details refer to `index.html` and `docs/product-analysis.md`.
