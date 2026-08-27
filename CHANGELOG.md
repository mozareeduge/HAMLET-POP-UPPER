# Changelog

## v1.8 — Playtest fixes, timing, and the skull

### Fixed
- The root cause of "interactions stop mattering": an unanswered call used
  to resume the same still-open dialog silently, forever — one missed
  round and the popup never visibly closed or reopened again for the rest
  of the piece (measured: 75-79s of one continuous open span). Every call
  now gets a real close-and-reopen with the same badge pulse, whether it
  was answered, ignored, or deferred.
- Reopen waits shortened and made score-aware: capped by how long is
  actually left before the next fixed death, so the back half of the score
  keeps a completable interaction window. Measured: reopen after an answer
  went from 8347ms to 4332ms.
- Terminal hairline mark (v1.7) only ever played once — a restart never
  replayed it. Fixed.

### Added
- An answer or "later" now speaks once on the popup surface — "to be." /
  "not to be." / "—" — before withdrawing, the same shape a scored death's
  incision already took.
- "Ask me later" gets a wordless hairline gauge (under the work-title, not
  inside the dialog — the dialog is closed for the whole deferral) that
  drains over its exact real duration.
- A slow idle-pulse on the run-bit marks genuine waiting as time passing.
- A sixth encounter class, `recalled`: a death meeting a closed, non-
  deferred machine while an earlier answer is still remembered is now its
  own encounter, with its own wound and incision geometry — the memory
  system stops being cosmetic and starts shaping how the next death lands.
- A recurring skull glyph (12×12 pixels, no curves, one shared shape by
  reference) at the instrument-rail mark and, enlarged, inside the
  terminal afterimage — sized off the viewport rather than the small
  captured chassis, so it outgrows the structural burn around it. The
  mortality field renders in front of it where they overlap.

### Preserved
- Everything from v1.6/v1.7: one dramaturgical score, equal `TO BE / NOT
  TO BE` authority, no branching tragedy, persistent mortality field,
  visible dramatic time, autonomous single HTML.

Full evidence and rationale for every decision above: `change-plan/plan-hamlet-pop-upper.md`.

## v1.7 — Ballot round 1

- Wounds are felt once on arrival, then settle to the same low-contrast
  residue as before.
- A death-forced call and a scheduled call share one badge; only the
  forced one gets a single quiet pulse.
- The apparatus keeps speaking "So am I." as its own line; a hairline
  marks that collapse as deliberate.

## v1.6 — Integrated dramaturgical score

- Replaced the practical two-engine relation with
  `INITIAL_DEATH + SCORE → HAMLET_MACHINE encounter → mortality commit`.
- Added explicit machine modes (`question`, `impact`, `terminal`) and
  encounter classes (`unanswered`, `answered`, `answer_collision`,
  `deferred`, `closed`, `terminal`).
- Persistent machine wounds and claim-lines now encode the state an
  inevitable event met.

## v1.5 and earlier

Practical two-engine relation: a fixed death timeline and a separately
scheduled questioning popup, only loosely coupled. Superseded by v1.6.
