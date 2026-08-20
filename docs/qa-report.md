# QA Report — Structured Adult Evaluation (Rubric)

Scope: test the optional structured adult evaluation feature added to `index.html` (three checkboxes + submit) and verify it coexists with existing `Acertou!` and `Tentar novamente` flows. Tests are observational and static where runtime execution is not possible.

Test environment limitations
- This environment does not provide a real browser or DOM runtime to execute interactive behavior. Tests marked NOT TESTED indicate actions that require a browser. Static inspections were performed on `index.html` to verify DOM ids, functions, and code paths.

Summary of static inspection
- Confirmed the rubric markup exists with IDs: `rubric-recognizable`, `rubric-effort`, `rubric-creative`, and submit button `rubricSubmit`.
- Confirmed JS function `submitRubricEvaluation()` exists and maps `total>0` => `handleCorrect()` and `total==0` => `handleRetry()`.
- Confirmed `handleCorrect()` still increments `levelCorrect` and runs celebration logic.
- Confirmed `loadNextCard()` clears rubric checkboxes and re-enables controls.
- No JS syntax errors reported by static error tool.

Detailed test matrix

1) Existing "Acertou!" behavior still works
- Expected: Pressing `Acertou!` triggers the existing correct flow: stops timer, awards 1 star, shows celebration, and increments counters.
- Actual: NOT TESTED (requires browser). Static inspection: `handleCorrect()` function present and increments `levelCorrect`; UI button `btnCorrect` still calls `handleCorrect(event)`. No code paths were removed. (STATIC CHECK: PASS)
- Result: NOT TESTED

2) Existing "Tentar novamente" behavior still works
- Expected: Pressing `Tentar novamente` stops timer, unlocks canvas, pushes current card to `missed`, clears canvas, and loads next card.
- Actual: NOT TESTED. Static inspection: `handleRetry()` exists and implements this logic; button still wired. (STATIC CHECK: PASS)
- Result: NOT TESTED

3) Submitting the rubric with 0 criteria selected
- Expected: Treated as retry (same behavior as `Tentar novamente`). No star awarded.
- Actual: NOT TESTED. Static inspection: `submitRubricEvaluation()` sums checkboxes and calls `handleRetry()` when total == 0. (STATIC CHECK: PASS)
- Result: NOT TESTED

4) Submitting with 1 criterion selected
- Expected: Conservative mapping — any selection should be treated as correct and award one star. Celebration appears.
- Actual: NOT TESTED. Static inspection: `submitRubricEvaluation()` calls `handleCorrect()` when total > 0. (STATIC CHECK: PASS)
- Result: NOT TESTED

5) Submitting with 2 criteria selected
- Expected: Same as above (conservative mapping): treated as correct; one star awarded.
- Actual: NOT TESTED. Static inspection: same mapping as test 4. (STATIC CHECK: PASS)
- Result: NOT TESTED

6) Submitting with all 3 criteria selected
- Expected: Treated as correct; one star awarded.
- Actual: NOT TESTED. Static inspection: same mapping. (STATIC CHECK: PASS)
- Result: NOT TESTED

7) Only one star is awarded per evaluation
- Expected: Each evaluation increments `levelCorrect` by 1 (one star). Partial credit not implemented.
- Actual: NOT TESTED. Static inspection: `handleCorrect()` increments `levelCorrect++` and `submitRubricEvaluation()` maps any positive selection to a single call to `handleCorrect()`. (STATIC CHECK: PASS)
- Result: NOT TESTED

8) Duplicate submissions cannot award multiple times
- Expected: Rapid repeated clicks should not award multiple stars; controls are disabled on submit to avoid duplicates.
- Actual: NOT TESTED. Static inspection: `submitRubricEvaluation()` disables `rubricSubmit` and `btnCorrect` before calling `handleCorrect()`. `handleCorrect()` also disables `rubricSubmit` and `btnCorrect` at start. `loadNextCard()` re-enables controls. This pattern reduces duplicate submissions but cannot be fully validated here (race conditions possible in a real DOM). (STATIC CHECK: PROVISIONAL PASS)
- Result: NOT TESTED

9) Controls are correctly disabled and re-enabled for the next card
- Expected: On submit, controls disabled; when the next card loads, checkboxes cleared and controls re-enabled.
- Actual: NOT TESTED. Static inspection: `loadNextCard()` sets `btnCorrect.disabled = false`, `rubricSubmit.disabled = false`, and clears checkbox states. (STATIC CHECK: PASS)
- Result: NOT TESTED

10) Level progression still behaves correctly
- Expected: Five correct evaluations (one star each) lead to level-up; final victory at completion of all levels.
- Actual: NOT TESTED. Static inspection: existing progression logic untouched (`levelCorrect`, `answeredCount`, `showLevelUp()`, `showFinalVictory()` remain). `handleCorrect()` increments the same counters. (STATIC CHECK: PASS)
- Result: NOT TESTED

11) The timer and evaluation flow still work together
- Expected: Submitting evaluation stops timer and triggers appropriate flow; timer expiring locks canvas and requires adult evaluation.
- Actual: NOT TESTED. Static inspection: `handleCorrect()` and `handleRetry()` clear interval and interact with canvas locking; `startTimer()` and `lockCanvas()` unchanged. (STATIC CHECK: PASS)
- Result: NOT TESTED

12) The rubric works with keyboard navigation
- Expected: Tab navigation reaches checkboxes and submit button; Enter/Space activates them. Fieldset/legend and labels present.
- Actual: PARTIALLY TESTED (static). Static inspection: markup uses native `<input type="checkbox">` with `label for` associations and a `button` with `aria-label`. These are keyboard-focusable by default. (STATIC CHECK: PASS)
- Result: NOT TESTED (interactive keyboard behavior requires browser)

13) The new controls have appropriate accessibility attributes
- Expected: Controls use semantic markup and have necessary ARIA attributes.
- Actual: STATIC PASS. Observed `fieldset`/`legend`, `label for`, and `aria-label` on submit button. No role attributes explicitly needed for native elements. (STATIC CHECK: PASS)
- Result: NOT TESTED (screen reader verification needed)

14) The layout remains usable on a narrow/mobile viewport
- Expected: Rubric wraps or remains usable without overlapping critical controls.
- Actual: NOT TESTED. Static inspection: CSS includes a small `@media (max-width: 480px)` rule to adjust rubric gap and wrapping; visual verification required. (STATIC CHECK: PROVISIONAL PASS)
- Result: NOT TESTED

15) Look for JavaScript errors or other regressions
- Expected: No syntax errors or immediate runtime exceptions introduced by the change.
- Actual: STATIC PASS. The repository static error check reported no syntax errors in `index.html`. No obvious undefined references found. (STATIC CHECK: PASS)
- Result: NOT TESTED

Unusual user behavior tests (rapid clicks, toggles)
- Rapid clicks on evaluation button / select/deselect quickly / submit and immediately try another evaluation / switch between rubric and `Acertou!` / keyboard-only interaction / complete several cards: ALL marked NOT TESTED (require interactive DOM). Static inspection indicates defenses (disabling buttons) but cannot guarantee absence of race conditions.

Confirmed bugs
- None confirmed by static inspection. All tests that require execution are marked NOT TESTED with explanations.

Potential issues (observations only)
- Race condition risk: disabling buttons helps, but in some browsers rapid event queuing could still call handler multiple times before disabled state applies. Severity: Medium (could cause extra star award). Reproducible via interactive stress test.
- UI density: On very narrow screens, the adult zone with added rubric could feel cramped; the CSS attempts to wrap but visual QA recommended. Severity: Low.

Recommendations for follow-up testing (local)
1. Run the app locally in a browser and execute all interactive tests listed above. Focus on duplicate-submission edge cases by rapidly clicking/tapping.
2. Test with keyboard-only navigation and with a screen reader to validate accessibility semantics.
3. On mobile device (narrow viewport), verify the rubric layout and tappable area sizes.

Overall verdict

PASS WITH ISSUES
