## Objective

<!-- What does this project or change accomplish?
     One or two sentences. Match the projects table description in README.md. -->

## Why this design

<!-- Tradeoff rationale — why this approach over the alternatives?
     Examples:
     - Why DMA circular buffer over interrupt-per-sample for ADC?
     - Why Q1.15 over Q1.31 for the FIR coefficients?
     - Why polling over EXTI for this button debounce?
     - Why this clock frequency / prescaler combination?
     This is the field that separates a design decision from an implementation detail. -->

## Registers touched

<!-- List every peripheral register written or read, with the bit fields
     and values. If no new registers were touched, write "None — no new peripherals". -->

| Register | Address | Bits | Value | Purpose |
|---|---|---|---|---|
| | | | | |

## Test performed

<!-- How was correct operation verified? -->

- [ ] GDB register inspection — values match expected (document in docs/design.md)
- [ ] Visual / functional verification — describe what was observed
- [ ] Build size recorded in docs/performance.md
- [ ] Timing measured (if applicable) — method and result in docs/performance.md
- [ ] docs/design.md updated — includes tradeoff rationale from "Why this design"
- [ ] docs/performance.md updated
- [ ] CHANGELOG.md updated with release entry
- [ ] Commit messages describe register-level decisions, not just file changes
- [ ] Semver tag created with annotated message

## Notes

<!-- Anything unexpected encountered. Wrong hypotheses, GDB revelations,
     errata hit. This feeds private_notes.md — copy anything worth keeping. -->
