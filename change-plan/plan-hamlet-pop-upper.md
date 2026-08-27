---
id: plan-hamlet-pop-upper
title: Hamlet Pop-Upper — Change Plan
kind: change-plan
status: open
version: 1.0.0
date_decided: null
decided_by: null
scope: "[[hamlet-pop-upper]]"
depends_on: []
provenance:
  packages_analyzed:
    - name: Hamlet_Pop_Upper_v1.5_HANDOFF_PACKAGE
      artifact: artifact/Hamlet_Pop_Upper_v1.5_SINGLE_FILE.html
      sha256: (see MANIFEST_VERIFY.md in package)
    - name: Hamlet_Pop_Upper_v1.6_HANDOFF_PACKAGE
      artifact: artifact/Hamlet_Pop_Upper_v1.6_SINGLE_FILE.html
      sha256: a0e8855e46773711ddddd2816d909acd3410ed03714b1071eefea5f30e825cee
  method: source-read (both single-file HTML artifacts, full), screenshot-review (QA package screenshots, both versions), doc-read (00_START_HERE, product/00 & 01, design/01 02 04, DECISION_RATIONALE_REGISTER, CHANGELOG, PROOF_GAPS_AND_NEXT_VALIDATION, original_concept_sketch.jpg — both packages)
  cross_reference: mozareeduge/mozare-wiki @ fe0ee71 (read-only) — works/grave-machine.md, works/the-black-bird.md, works/winter-road.md, people/mohammad-zare.md, CLAUDE.md, SEARCH_GUIDE.md
  observed: '2026-08-27'
---

# Hamlet Pop-Upper — Change Plan

No prior atelier lineage exists for this work (not yet present in mozare-wiki;
this is its first diagnosis session). All IDs below are new.

## Diagnosis summary

v1.5 and v1.6 both ship as dependency-free single HTML files. The v1.5→v1.6
change is real and source-verified, not just changelog rhetoric: in v1.5,
`WORLD` (v1.5 L244-253) drives a single fixed-timer list — every death,
Father included, is appended straight to the mortality list by `addDeath()`
(L306-319) on its own schedule, while the Hamlet dialog opens/closes on an
*independent* timer (`nextAskAt`, L323-337) that a death can only nudge
earlier, never author. When the terminal death fires, `beginTerminal()`
(L345-353) plays a "dying" animation on whatever the dialog happened to be
showing — it never writes `So am I.` into the dialog itself. The apparatus
dies of a flag, not of its own last words. This is HPU-01 below.

v1.6 replaces this with one procedure: `SCORE` events are classified against
live dialog state (`classifyEncounter()`, L387-394) into one of five
encounter classes, routed through `beginImpact()`/`beginTerminal()`
(L398-426), which write the death text into `#eventIncisionText` *inside*
the chassis before it ever reaches the mortality field, and only then
commit. `So am I.` is deliberately privileged here: `beginTerminal()`
(L419-426) is a distinct path from `beginImpact()`, and it is the one place
the apparatus itself appears to speak the character's line. The unification
DR-16-01 through DR-16-05 claim is real in the code, not aspirational.

That fix is not free. Diagnosing v1.6 on its own claims (HZN-001 "interface
agency ≠ dramatic causality", HZN-005 "memory is residue, not authority")
surfaces five open findings, all below. None is a defect in the QA sense —
the v1.6 QA campaign's own `VERIFICATION_INCOMPLETE` verdict is honest and
matches what the source shows. These are creative-direction findings: places
where the work's own stated grammar and its delivered sensory/legibility
layer pull against each other, or where the last move (v1.5→v1.6) commits
this piece to a trajectory (more state, more taxonomy) worth naming before
a next version repeats it by default.

---

```yaml
decision: HPU-01
title: Terminal event was cosmetic-only in v1.5; v1.6 already corrects it
status: decided
priority: P1
origin: mistake
finding: >
  v1.5's beginTerminal() (L345-353) plays the dialog's death animation
  independently of what the dialog displays; "So am I." only ever appears
  in the mortality-field list, never spoken by the apparatus. v1.6's
  beginTerminal() (L419-426) writes event.text into #eventIncisionText
  before commitDeath, so the apparatus speaks the line before it dies.
evidence:
  repo: (local handoff packages, not yet in a git remote)
  commit: v1.5 sha n/a — v1.6 sha256 a0e8855e46773711ddddd2816d909acd3410ed03714b1071eefea5f30e825cee
  locus: "v1.5 artifact L306-319, L345-353 vs v1.6 artifact L398-426"
  observed: '2026-08-27'
  method: source-read
decision_text: >
  No action needed. Recorded as provenance for why HPU-02..HPU-06 treat
  v1.6, not v1.5, as the live baseline, and as the reference point for
  the "add vs. subtract" question in HPU-06 — this is the one place where
  ADDING a dedicated code path measurably fixed a diagnosed contradiction,
  not merely added taxonomy.
implementation: []
acceptance: []
```

**HPU-01 — resolved, informational.** v1.5 quietly broke its own claim that
the apparatus and the tragedy are one procedure: the dialog's death was
staged, not authored, because nothing connected `event.text` to what the
dialog rendered while it animated. v1.6 fixes this at the source level. This
sets the baseline for everything below: v1.6 is diagnosed on its own terms,
not measured against v1.5's weaker version of the same claim.

---

```yaml
decision: HPU-02
title: Encounter-class wound geometry is real but likely imperceptible
status: hold
priority: P1
origin: unmade-decision
finding: >
  addWound() (L352-357) assigns real, distinct per-encounter-class geometry
  (a lookup table keyed unanswered/answered/answer_collision/deferred/
  terminal, L355) plus small per-index drift — the code genuinely
  differentiates wounds by encounter class, matching DR-16-05's claim. But
  .wound opacity is 0.15-0.32 (L136-140) over a bone-on-paper chassis with
  low inherent contrast; a first-time visitor with no handoff package is
  very unlikely to ever consciously register that wound A differs from
  wound B because of how the visitor answered. The causal claim (HZN-017:
  "visitor state shapes encounter... changes wound geometry") is true in
  code and almost certainly false in experience.
evidence:
  repo: (local handoff package)
  commit: v1.6 sha256 a0e8855e46773711ddddd2816d909acd3410ed03714b1071eefea5f30e825cee
  locus: "artifact L136-140 (.wound opacity rules), L352-357 (addWound geometry table)"
  observed: '2026-08-27'
  method: source-read, cross-checked against qa/screenshots/desktop_1440x900_open.png and REALTIME_polonius_meets_deferral.png (wound marks not independently distinguishable by eye in either capture)
decision_text: >
  Held pending Mohammad's read: is imperceptibility the point (residue as
  fact, not spectacle — consistent with HZN-005 and with Winter Road's
  "disappearance as an authored condition" per mozare-wiki), or is this
  code weight that never reaches an audience and should either surface once
  or be cut? Ballot HPU-B1 below carries three concrete options; return
  condition: his verdict on HPU-B1.
implementation: []
acceptance: []
depends_on: [HPU-B1]
```

**HPU-02.** The generous read: this is the same restraint that makes his
other born-digital work trustworthy — a mechanism doesn't need to perform
its own legibility to be true, and making the wounds louder would be the
kind of over-explanation the wiki's own "visible genesis" material warns
against (`grave-machine.md`: "how much procedure can remain readable
without reducing the generated poem to an explanation of itself"). The
harder read: an invisible causal system doesn't cost the audience anything,
which means it isn't actually load-bearing for them — it's load-bearing for
the documentation. Both readings are defensible; this is exactly the kind
of aesthetic call the atelier protocol reserves for him, not for a fix
applied on paper.

---

```yaml
decision: HPU-03
title: "CALL NN" is the sharpest satirical device in the piece and is under-lit
status: hold
priority: P1
origin: unmade-decision
finding: >
  The apparatus borrows customer-support/consent-pattern UI vocabulary
  wholesale: "Remember this answer" (a cookie-consent checkbox, L254-258),
  "Ask me later" (an app-permission deferral, L259), and a literal ticket
  counter "call 01" (L239) that increments not only on scheduled questions
  but also when a death gets silently absorbed as the next call
  (beginImpact's continuation branch, L416: "A death met a closed/answered
  machine: after being processed, it becomes the next call."). That comment
  names the joke directly in the source. The rendered badge is 9px mono,
  fully subordinate chrome (.title-status, L101) with no visual distinction
  between "the machine asked on schedule" and "a death just got logged as a
  support ticket."
evidence:
  repo: (local handoff package)
  commit: v1.6 sha256 a0e8855e46773711ddddd2816d909acd3410ed03714b1071eefea5f30e825cee
  locus: "artifact L101 (.title-status), L239 (#callCounter markup), L416 (call-increment-on-death branch + its own inline comment)"
  observed: '2026-08-27'
  method: source-read
decision_text: >
  Held. The piece already wrote its own best joke into a code comment and
  then hid it at 9px. Ballot HPU-B2 proposes ways to let a call-count jump
  caused by a death read differently from a call-count jump caused by
  schedule, without turning the badge into an explainer.
implementation: []
acceptance: []
depends_on: [HPU-B2]
```

**HPU-03.** Deaths being processed as support tickets is a genuinely cruel,
funny, exact target — the piece's own vocabulary already says the
Renaissance tragedy is being handled by the same interface grammar as a
cancel-subscription flow. Right now that target is legible only to someone
reading the JS. A redesign axis worth testing: not "add a system," but "let
an existing signal be seen once."

---

```yaml
decision: HPU-04
title: "So am I." spoken by the apparatus may collapse the piece's protected boundary
status: hold
priority: P0
origin: unmade-decision
finding: >
  HZN-001 protects "interface agency ≠ dramatic causality" — the apparatus
  is explicitly not a character; it delivers a fixed score and cannot
  originate content. HZN-020/DR-16-04 then have the apparatus itself
  display "So am I." as its own terminal utterance (beginTerminal(),
  L419-426) before the line joins the mortality field — Hamlet's dying
  line is, for one beat, rendered as the machine's line. This is either the
  piece's strongest single move (the interrogator that spent 84 seconds
  asking a question it could never let matter finally becomes mortal too,
  collapsing questioner into questioned on purpose, once, at the end) or a
  quiet violation of the one rule the piece asks to be judged by, at the
  exact moment the rule matters most. The source cannot settle which; this
  is a reading, and readings are his to make.
evidence:
  repo: (local handoff package)
  commit: v1.6 sha256 a0e8855e46773711ddddd2816d909acd3410ed03714b1071eefea5f30e825cee
  locus: "artifact L419-426 (beginTerminal), product/00_PRODUCT_HORIZON.md HZN-001 and HZN-020"
  observed: '2026-08-27'
  method: source-read, doc cross-read
decision_text: >
  Held — this is the plan's P0 because it is a contradiction between the
  work's own protected claim and its own delivered terminal beat, not a
  polish item. Ballot HPU-B3 asks directly which reading he holds, and
  whether the terminal sequence should mark the collapse as deliberate
  (e.g., a formal signal that this one beat is exempt from HZN-001) or be
  re-cut to preserve the boundary (e.g., the apparatus visibly breaks/fails
  rather than "speaking").
implementation: []
acceptance: []
depends_on: [HPU-B3]
```

**HPU-04.** This is the one finding in this plan that is not a
legibility problem — it is a live interpretive fork in the work's own
governing rule, surfaced by close reading of the terminal code path against
the horizon document it is supposed to answer to.

---

```yaml
decision: HPU-05
title: Palette/type system is well-made but not yet singular to this piece
status: hold
priority: P2
origin: unmade-decision
finding: >
  :root tokens (L10-27) — near-black field #0f100e, bone/paper #d9d9cc-#e8e5d7,
  ink #171814, single rust accent #a92f16/#ff6238, Georgia serif for content
  vs. Courier mono for chrome — read as a fluent, restrained "editorial
  instrument" system. It is legible and coherent, and the notched clip-path
  chassis (L85, "not a generic OS dialog" per design/02_VISUAL_SYSTEM.md) is
  a real, specific formal choice. But the token *recipe itself* (dark field
  + warm paper card + one accent) is a widely reachable "serious web-art"
  default, not yet a signature the way Winter Road's haiga darkness or Black
  Bird's hypergraph field are described as being singular to those pieces in
  mozare-wiki. A cropped screenshot with the title hidden would be hard to
  attribute to this piece specifically versus a well-made system in the
  same family.
evidence:
  repo: (local handoff package)
  commit: v1.6 sha256 a0e8855e46773711ddddd2816d909acd3410ed03714b1071eefea5f30e825cee
  locus: "artifact L10-27 (:root tokens), L85 (chassis clip-path)"
  observed: '2026-08-27'
  method: source-read, cross-referenced against mozare-wiki works/winter-road.md and works/the-black-bird.md for comparison of signature-formal-choice strength
decision_text: >
  Held, lowest priority of the open findings. Ballot HPU-B4 proposes
  candidate directions for one additional singular formal signature (not a
  restyle) if he wants to pursue this; also carries a "decline, current
  system is exactly restrained enough" option, since under-signature is a
  real defensible reading of this piece's own request to look like an
  instrument rather than a poster.
implementation: []
acceptance: []
depends_on: [HPU-B4]
```

---

```yaml
decision: HPU-06
title: Add-more-state is the trend across v1.5 to v1.6; name it before a v1.7 repeats it by default
status: hold
priority: P1
origin: unmade-decision
finding: >
  The v1.5→v1.6 change added a real axis of precision (five encounter
  classes, per-class wound/incision geometry) in direct, successful
  response to a diagnosed contradiction (HPU-01). But the corpus-wide
  pattern visible in mozare-wiki (the 94-artifact evidence architecture,
  canonical/claim/source/derivative distinctions, the horizon-numbered
  HZN-* documents, this very package's own QA taxonomy of five encounter
  classes) shows a recurring authorial move: when something feels
  insufficiently authored, the answer is another named category. That move
  is earned here once (HPU-01). It is not yet clear it should be the
  default move again. No source-level evidence can decide this — it is a
  question about which direction is more interesting to build next.
evidence:
  repo: mozareeduge/mozare-wiki
  commit: fe0ee710456168367530163e725d25ef0163187c
  locus: "CLAUDE.md always-on rules 3, 6; SEARCH_GUIDE.md search-is-a-route framing; cross-read against Hamlet Pop-Upper DECISION_RATIONALE_REGISTER.md DR-16-01..05"
  observed: '2026-08-27'
  method: both (source-read of artifact + doc-read across two repositories)
decision_text: >
  Held. Ballot HPU-B5 poses this as the umbrella creative-direction
  question for whatever comes after v1.6: add a sixth axis (more encounter
  classes, more residue types) as the safe continuation of the v1.6 method,
  or deliberately subtract one existing axis of precision as a controlled
  experiment in whether felt weight increases when some of this
  systematization is removed and replaced by unresolved ambiguity.
implementation: []
acceptance: []
depends_on: [HPU-B5]
```

## Ballot round 1 — verdicts received (2026-08-27)

Pasted verbatim, as received:

```yaml
# Hamlet Pop-Upper — Ballot Round 1 verdicts
ballot: hamlet-pop-upper-r1
ballot_version: 1.0.0
exported: 2026-08-27
selected_by: Mohammad Zare (Mozare)
provenance:
  artifact: hamlet-pop-upper-ballot-r1.html
  lineage: plan-hamlet-pop-upper.md (first atelier session, HPU-01..HPU-06)
verdicts:
  HPU-B1:
    title: "Should a machine wound ever be seen?"
    verdict: adopt
  HPU-B2:
    title: "Let a death-as-call read differently from a scheduled call"
    verdict: adopt
  HPU-B3:
    title: "\"So am I.\" spoken by the apparatus: culmination or violation?"
    verdict: adopt
  HPU-B4:
    title: "Does the piece need one more singular formal mark?"
    verdict: hold
    note: "Instead of cross shape i prefer a minimal brutalistic skull"
  HPU-B5:
    title: "v1.7 should add a class, or v1.7 should remove one"
    verdict: adopt
    note: "Overall, i want the aparatus that is interaction site and text procedure and visual/sensual design to be related and all parts of the whole work"
next_step: fold verdicts into plan-hamlet-pop-upper.md per decision-schema.md; declines marked do-not-re-propose; prototype verdicts gated with promotion criteria; holds recorded with return conditions from notes above.
```

**Ambiguity resolutions (composition, not silent choice — per decision-schema.md §Verdict semantics):**

- HPU-B1 carried no note specifying which of the plan's three options (pure
  residue / flash-once-then-settle / afterimage-only) `adopt` meant. Resolved
  in favor of **flash-once-then-settle**: it is the option the ballot's own
  demo actually built and is the one most consistent with his HPU-B5 note
  ("interaction site and text procedure and visual/sensual design... related")
  — the flash ties the visible mark directly to the moment of the interaction
  that caused it, rather than leaving it purely ambient.
- HPU-B3's `adopt` maps to the verdict strip's first button, labeled
  "A — keep, mark deliberate" — i.e. the apparatus keeps speaking `So am I.`
  as its own line, with a one-time hairline signal added to mark the
  boundary-collapse as authored rather than missed.
- HPU-B5's `adopt` maps to "A — add a class." His note doesn't name the class;
  resolved by composition: a new `recalled` encounter class — a death that
  meets a closed, non-deferred machine while an earlier answer is still
  remembered. This was chosen specifically because it is the one extension
  that *integrates* an existing but previously cosmetic system (the
  remembered-answer scar) into the encounter/wound system, directly answering
  the "all parts related" note rather than adding an unrelated new axis.

```yaml
decision: HPU-07
title: Wounds are felt once, at the moment they're made, then settle to the existing residue
status: decided
priority: P1
origin: unmade-decision
finding: resolves HPU-02 / ballot HPU-B1 (adopt)
evidence:
  repo: (local artifact)
  commit: v1.7 sha256 b1b5fb0a6e4190ee0fb5776f229749474d46aa877f8a98a92b96dfd2fccbfe36
  locus: "Hamlet_Pop_Upper_v1.7_SINGLE_FILE.html — .wound.is-new + @keyframes wound-arrive (CSS), addWound() WOUND_REST_OPACITY table (JS)"
  observed: '2026-08-27'
  method: source-read, browser verification (Playwright, time-compressed clock; screenshot v17_runA_state.png)
decision_text: >
  Every wound now animates from 0.85 opacity down to its original v1.6 resting
  opacity over 1500ms on arrival, via a per-kind --rest custom property so the
  final state exactly matches the old static rule. Causality is felt once,
  not explained; the resting mark is exactly as quiet as before.
implementation:
  - ".wound.is-new{animation:wound-arrive 1500ms steps(9,end) both}"
  - "addWound() sets style.setProperty('--rest', WOUND_REST_OPACITY[kind])"
acceptance:
  - "Playwright run confirms .wound.is-new class present on every wound created during a live run (v17_runA)."
executed: '2026-08-27' · Hamlet_Pop_Upper_v1.7_SINGLE_FILE.html
```

```yaml
decision: HPU-08
title: A death-forced call gets one quiet admission the badge didn't give it before
status: decided
priority: P1
origin: unmade-decision
finding: resolves HPU-03 / ballot HPU-B2 (adopt)
evidence:
  repo: (local artifact)
  commit: v1.7 sha256 b1b5fb0a6e4190ee0fb5776f229749474d46aa877f8a98a92b96dfd2fccbfe36
  locus: "Hamlet_Pop_Upper_v1.7_SINGLE_FILE.html — #callCounter.hit + @keyframes call-hit (CSS); beginImpact()'s 'becomes the next call' branch (JS)"
  observed: '2026-08-27'
  method: source-read, browser verification (Playwright MutationObserver on #callCounter; confirmed 1 toggle in a live run)
decision_text: >
  Only the call-increment inside beginImpact's fallback branch (a death
  processed by a closed/answered/recalled machine) toggles the .hit class;
  the ordinary scheduled reopen in openQuestion() is untouched. The two kinds
  of "call" now read differently exactly once, without new copy.
implementation:
  - "el.callCounter.classList.remove('hit');void el.callCounter.offsetWidth;el.callCounter.classList.add('hit')"
acceptance:
  - "Playwright MutationObserver confirmed the .hit class toggling during a live death-forced call, correlated with a 'recalled' encounter commit."
executed: '2026-08-27' · Hamlet_Pop_Upper_v1.7_SINGLE_FILE.html
```

```yaml
decision: HPU-09
title: The apparatus keeps speaking "So am I."; the collapse is marked, not hidden
status: decided
priority: P0
origin: unmade-decision
finding: resolves HPU-04 / ballot HPU-B3 (adopt = option A)
evidence:
  repo: (local artifact)
  commit: v1.7 sha256 b1b5fb0a6e4190ee0fb5776f229749474d46aa877f8a98a92b96dfd2fccbfe36
  locus: "Hamlet_Pop_Upper_v1.7_SINGLE_FILE.html — .window.terminal-marked::before + @keyframes terminal-mark (CSS); beginTerminal() (JS)"
  observed: '2026-08-27'
  method: source-read, browser verification (Playwright; v17_runB_terminal.png shows the hairline + \"So am I.\" together)
decision_text: >
  No text or timing change to the terminal sequence. beginTerminal() adds
  .terminal-marked to the chassis the instant it starts speaking the line;
  a single 3px rust hairline fades in and back out over 1400ms. The
  interface/character collapse HPU-04 identified is real and is now
  declared, not left for a close reader of the source to notice alone.
implementation:
  - "el.window.classList.add('terminal-marked') added inside beginTerminal(), immediately after setMode('terminal',kind)"
acceptance:
  - "Playwright confirmed .terminal-marked present on #windowFrame at the moment #eventIncisionText reads \"So am I.\""
executed: '2026-08-27' · Hamlet_Pop_Upper_v1.7_SINGLE_FILE.html
```

```yaml
decision: HPU-10
title: A sixth encounter class — recalled — ties the memory system into the wound system
status: decided
priority: P1
origin: unmade-decision
finding: resolves HPU-06 / ballot HPU-B5 (adopt = option A, class chosen by composition)
evidence:
  repo: (local artifact)
  commit: v1.7 sha256 b1b5fb0a6e4190ee0fb5776f229749474d46aa877f8a98a92b96dfd2fccbfe36
  locus: "Hamlet_Pop_Upper_v1.7_SINGLE_FILE.html — classifyEncounter() recalled branch, addWound()/encounterPoint() recalled entries, .wound[data-kind=recalled] (CSS)"
  observed: '2026-08-27'
  method: source-read, browser verification (Playwright; v17_runA — Polonius and Mother both committed with encounter=recalled and a matching wound)
decision_text: >
  classifyEncounter() now checks remembered state before the plain
  answered/closed fallback (the pre-existing active-deferral check still
  takes priority, preserving DR-16-02). Once a visitor has ever checked
  "Remember this answer," every subsequent death meeting a closed, non-
  deferred machine is classified recalled rather than answered/closed, with
  its own wound geometry, incision style, and claim-line anchor point. This
  is the resolution his note asked for: the remembered-answer scar was
  previously cosmetic on a button; it is now load-bearing for how the next
  death is received.
  Note for a v1.8 read: this makes recalled the DOMINANT classification for
  any visitor who ever uses "remember" — verify in a future session whether
  that dominance is still correct once HPU-B4 is resolved and the two
  changes are seen together, or whether recalled should fade after N
  encounters rather than holding indefinitely.
implementation:
  - "classifyEncounter(): if(machineState==='closed'&&remembered)return 'recalled'; — inserted before the answered check, after the active-deferral check"
  - "addWound() geom table + WOUND_REST_OPACITY['recalled']=.24; encounterPoint() pts.recalled=[.58,.4]"
acceptance:
  - "Playwright run committed at least one death with dataset.encounter==='recalled' and a corresponding <i class=\"wound\" data-kind=\"recalled\"> in #woundLayer."
executed: '2026-08-27' · Hamlet_Pop_Upper_v1.7_SINGLE_FILE.html
```

**Status changes on prior held decisions:**

- HPU-02: `status: hold → decided (via HPU-07) · 2026-08-27 · evidence: ballot hamlet-pop-upper-r1, HPU-B1 adopt`
- HPU-03: `status: hold → decided (via HPU-08) · 2026-08-27 · evidence: ballot hamlet-pop-upper-r1, HPU-B2 adopt`
- HPU-04: `status: hold → decided (via HPU-09) · 2026-08-27 · evidence: ballot hamlet-pop-upper-r1, HPU-B3 adopt (option A)`
- HPU-06: `status: hold → decided (via HPU-10) · 2026-08-27 · evidence: ballot hamlet-pop-upper-r1, HPU-B5 adopt (option A)`
- HPU-05: `status: hold, unchanged · 2026-08-27 · verdict: hold · return condition (from his note): return when a minimal brutalist skull glyph — not the demoed cross — has been prototyped as a candidate for the rail-mark/instrument-glyph signature, for his review in a round 2 ballot.`

## Plan state — after ballot round 1

- **Decided:** HPU-01 (informational), HPU-07, HPU-08, HPU-09, HPU-10 — all four executed directly into `Hamlet_Pop_Upper_v1.7_SINGLE_FILE.html` and verified live (Playwright, time-compressed clock; see verification notes above).
- **Held:** HPU-05 (ballot HPU-B4) — return condition above. Not yet built into the shipped artifact; annex-only when it exists, per the ballot-artifact protocol for a hold carrying a redesign direction.
- **Declined:** none.
- Lineage is not closed (HPU-05 still open) — station 6 (a full implementation
  kit) is not yet called for; v1.7 itself, delivered directly, is the
  appropriate deliverable shape for a single autonomous-HTML work at this
  stage, per his note's own framing (one related whole, not componentized).

Next step: when a skull-glyph study exists for HPU-05, present it as a
flagged annex in a round-2 ballot ("held — study only — no decision
required") rather than deciding it silently.

## Prototype round r2 — HPU-05 skull glyph (2026-08-27)

Built on request, immediately following the round 1 fold-in. A single
recurring glyph — 16-unit grid, no curves, one `evenodd` SVG path so it
inherits `currentColor` (ink-on-paper at the rail, bone-on-field in the
afterimage) with no separate light/dark variant — shown live at three loci:

1. **Instrument rail** — direct swap for the generic `?` mark, same 15×15
   slot, no layout change.
2. **Afterimage** — the dead machine's ghost, at 72px. Flagged explicitly
   as new content rather than a mark-swap, since v1.7's afterimage otherwise
   keeps only structural geometry (design/02_VISUAL_SYSTEM.md).
3. **Wound layer** — shown, not proposed. Included to make the case for
   *excluding* it visible: it would compete with the wound residue behavior
   just decided in HPU-07.

Ballot: `hamlet-pop-upper-ballot-r2.html`. Verdict strip uses prototype-round
vocabulary (promote at rail-mark only / promote at both loci / decline),
per `references/ballot-artifact.md` §Prototype rounds. Lineage: r1 (HPU-B4
held) → r2 (this prototype). Not yet decided — HPU-05 remains `hold` until
a verdict returns; this round does not touch `Hamlet_Pop_Upper_v1.7_SINGLE_FILE.html`.

## Playtest diagnosis and fix — v1.8 (2026-08-27)

Not a ballot round. Mohammad played the live v1.7 artifact himself and
reported four concrete experiential problems, in product/design/technical
terms, then asked for an integrated diagnosis, fix, and QA before seeing a
new version — not a vote. These are treated as bugs against the work's own
protected claims (HZN-003 "the prompt and equal decisions recur"; HZN-006
"visible dramatic time"), not aesthetic ambiguity, so they are fixed
directly rather than balloted. All four are diagnosed and fixed in
`Hamlet_Pop_Upper_v1.8_SINGLE_FILE.html`, sha256
`660f31574dbaf4795601ed3a2f96489290e1e075864ea4a1840fb553774f81d0`.

**His report, verbatim substance:** (1) long waiting before the popup
reappears; (2) unclear relation between an interaction and its text — the
interaction's own text should speak on the popup surface before the
procedural/accumulated lines do; (3) "ask me later" behaves by an unknown
logic; (4) after a while, interactions stop mattering and the text
procedure advances regardless of what the user does.

**Method:** source-read of `beginImpact`/`closeQuestion`/`classifyEncounter`
in v1.7, then Playwright verification under two clock regimes — a
corrected accelerated clock for functional confirmation (tracks true real
elapsed time between `requestAnimationFrame` callbacks, then scales it;
an earlier version of this harness fixed 16.7ms/callback and silently ran
~2x real time in headless Chromium, which produced misleading numbers and
was caught and discarded before any finding below relied on it) and pure
real time (scale 1) for every timing-sensitive measurement. All real-time
numbers below are measured, not estimated.

```yaml
decision: HPU-15
title: An unanswered call silently held the same open dialog for the rest of the piece — the actual cause of "interactions stop mattering"
status: decided
priority: P0
origin: mistake
finding: >
  beginImpact()'s kind==='unanswered'&&wasOpen branch (v1.7 L449) resumed
  the SAME already-open dialog in place, with no close/reopen and no
  animation class change. Measured directly (Playwright, poll-based, real
  time, single early click then no further interaction): the dialog's
  `open` attribute went true at 10400ms and did not go false again until
  84700ms+ — a single 74800ms continuous span absorbing polonius, ophelia,
  mother, claudius, rg, and laertes with zero visible re-presentation.
  HZN-003 ("the prompt... recurs") was true in the state machine and false
  in every frame a visitor could see after the first missed round.
evidence:
  repo: (local artifact)
  commit: v1.7 sha256 a0e8855e46773711ddddd2816d909acd3410ed03714b1071eefea5f30e825cee
  locus: "v1.7 artifact L449 (the removed branch)"
  observed: '2026-08-27'
  method: source-read, browser verification (Playwright, real time, poll-based dialog.open/callCounter logging — not the accelerated clock)
decision_text: >
  Removed the special case. Every death that meets a closed, answered,
  recalled, OR already-open-unanswered machine now goes through one shared
  presentNextCall(): a real close (safeCloseDialog()) then reopen
  (safeOpenDialog()) with the existing withdraw/opening animation classes,
  a call-count bump, and the existing call-badge pulse (HPU-08) — the same
  visible re-announcement every other kind of call already got. Re-measured
  under the identical click-once-then-ignore scenario: call badge now
  advances at every one of the 7 subsequent scored events (verified
  timestamps: 6681/20081/42080/66080/72080/76080/80080ms), each with an
  observed withdraw→reopen cycle.
implementation:
  - "beginImpact(): removed `if(kind==='unanswered'&&wasOpen){...return}`"
  - "new presentNextCall(): withdrawing (closeAnimation) -> safeCloseDialog() -> opening -> setMode/machineState/callCount/badge-pulse -> safeOpenDialog()"
  - "beginImpact()'s and beginTerminal()'s fallthrough/interrupt paths call presentNextCall() or are otherwise unaffected"
acceptance:
  - "Playwright, real time: click-once-then-ignore scenario shows call-badge transitions at all 7 subsequent scored events (was: 1 transition total, frozen for the remaining ~75s)."
executed: '2026-08-27' · Hamlet_Pop_Upper_v1.8_SINGLE_FILE.html
```

**HPU-15 is the headline finding.** His complaint #4 was not the score's
late-act density outrunning interaction windows — an earlier hypothesis in
this session that a direct real-time A/B test (7 scored events, real time,
identical simulated "attentive visitor" script) refuted: v1.7 answered
every late death correctly under that script, which only made sense once
the true mechanism above was found. The piece never stopped listening; it
just stopped telling you it was still asking.

```yaml
decision: HPU-11
title: Scheduled reopen waits are shorter and capped by time-to-next-death
status: decided
priority: P1
origin: unmade-decision
finding: >
  TIMING.answerReturn/laterReturn were flat 8500/14500ms regardless of the
  score. Measured (Playwright, real time): a "to be" click reopened the
  dialog 8347ms later — matching his "long waiting" report exactly.
evidence:
  repo: (local artifact)
  commit: v1.7 sha256 a0e8855e46773711ddddd2816d909acd3410ed03714b1071eefea5f30e825cee
  locus: "v1.7 artifact L321 (TIMING), L412/416 (nextAskAt assignment)"
  observed: '2026-08-27'
  method: browser verification (Playwright, real time)
decision_text: >
  answerReturnBase/laterReturnBase lowered to 4500/7000ms and capped by
  timeToNextScoreEvent() minus a safety margin, floored so a reopen is
  never near-instant. Re-measured: the same click now reopens in 4333ms.
  Note: this alone would have made "unanswered" outcomes MORE frequent
  (shorter windows mean more reopen cycles, each a chance to coincide with
  a fixed death before a visitor reacts) — that stopped being a legibility
  problem only once HPU-15 shipped alongside it, since every such
  coincidence now gets the same visible re-announcement rather than
  silence. The two decisions are a pair; do not ship one without the other.
implementation:
  - "TIMING: answerReturnBase 4500, laterReturnBase 7000, returnFloor 2200, returnSafetyMargin 700"
  - "timeToNextScoreEvent() / scheduleReturn(reason), used by closeQuestion()"
acceptance:
  - "Playwright, real time: reopen after answer measured at 4333ms (was 8347ms)."
executed: '2026-08-27' · Hamlet_Pop_Upper_v1.8_SINGLE_FILE.html
```

```yaml
decision: HPU-12
title: An interaction speaks on the popup surface once, before withdrawing — symmetric with how a death already does
status: decided
priority: P1
origin: unmade-decision
finding: >
  A scored death already writes into #eventIncisionText before it commits
  to the mortality field (v1.6). A visitor's own answer/later had no
  equivalent — chooseAnswer()/askLater() went straight to closeQuestion()
  with no trace of the interaction on the popup itself. This produced
  exactly the asymmetry he named: "the relation between interactions [and]
  related text that should come first onto the popup surface, then
  procedural accumulated lines" held for the world's text and not for his.
evidence:
  repo: (local artifact)
  commit: v1.7 sha256 a0e8855e46773711ddddd2816d909acd3410ed03714b1071eefea5f30e825cee
  locus: "v1.7 artifact L415-420 (chooseAnswer/askLater, no trace step)"
  observed: '2026-08-27'
  method: source-read, browser verification (Playwright: dataset.trace/incisionText observed mid-interaction, cleared after TIMING.traceHold)
decision_text: >
  chooseAnswer() writes "to be."/"not to be." into the incision area in ink
  (not rust — rust stays reserved for the world's events); askLater()
  writes a wordless "—". Both hold for TIMING.traceHold (460ms) via the
  existing answerTimer, then proceed to closeQuestion exactly as before.
  Verified: mid-click state shows {trace:"answer", text:"to be."}; cleared
  after the hold.
implementation:
  - "CSS: .window[data-trace] variants of .event-incision/.prompt/.control-side, ink-colored"
  - "chooseAnswer()/askLater(): set dataset.trace + incisionText, hold TIMING.traceHold, then closeQuestion()"
  - "beginImpact(): clears dataset.trace if a death interrupts a pending trace"
acceptance:
  - "Playwright: trace state observed and correctly cleared; no dataset.trace leak into impact/terminal mode on interruption."
executed: '2026-08-27' · Hamlet_Pop_Upper_v1.8_SINGLE_FILE.html
```

```yaml
decision: HPU-13
title: "\"Ask me later\" made legible with a wordless draining hairline"
status: decided
priority: P1
origin: unmade-decision
finding: >
  Deferral was tracked internally (deferredUntil, lastDismissReason) with
  no visitor-facing signal at all — matching his report exactly ("ask me
  later works with an unknown logic i couldnt sense").
evidence:
  repo: (local artifact)
  commit: v1.7 sha256 a0e8855e46773711ddddd2816d909acd3410ed03714b1071eefea5f30e825cee
  observed: '2026-08-27'
  method: source-read, browser verification (Playwright + screenshot v18_defer_gauge.png)
decision_text: >
  A hairline under the work-title (not inside the dialog — the dialog is
  closed for the entire deferral, so anything placed inside it would be
  invisible for exactly the period it needs to show; caught by screenshot
  review before shipping, first placement was wrong) drains from full to
  empty over the deferral's exact real duration via a JS-set transition
  duration, no words. Suppresses the generic idle-pulse (HPU-14) while
  active so the two signals never compete. A death does not cancel this
  gauge if the deferral itself is still active (DR-16-02, unchanged) —
  only hidden when the deferral genuinely ends.
implementation:
  - "#deferGauge on <main>, sibling of .work-title; showDeferGauge(ms)/hideDeferGauge()"
  - "closeQuestion(): showDeferGauge(nextAskAt-worldElapsed) when reason==='later'"
  - "beginImpact(): hides the gauge only when kind is NOT ('deferred'&&deferActive)"
acceptance:
  - "Playwright: gauge opacity .55 and draining confirmed mid-deferral; screenshot confirms visibility while dialog is closed."
executed: '2026-08-27' · Hamlet_Pop_Upper_v1.8_SINGLE_FILE.html
```

```yaml
decision: HPU-14
title: A slow idle-pulse marks genuine waiting as time passing, not nothing happening
status: decided
priority: P2
origin: unmade-decision
finding: >
  The closed/idle gap between calls had zero ambient signal — contributing
  to "long waiting" reading as dead air rather than HZN-006's "visible
  dramatic time."
evidence:
  repo: (local artifact)
  commit: v1.7 sha256 a0e8855e46773711ddddd2816d909acd3410ed03714b1071eefea5f30e825cee
  observed: '2026-08-27'
  method: source-read
decision_text: >
  The existing run-bit gets a slow (2400ms) opacity pulse whenever
  machineState is genuinely closed and not deferred; suppressed during an
  active deferral (HPU-13's gauge already speaks for that state) and during
  question/impact/terminal. Respects prefers-reduced-motion via the
  existing global rule.
implementation:
  - ".run-bit.is-idle{animation:idle-pulse 2400ms ease-in-out infinite}"
  - "setIdle(on) called from finishClose() (gated by defer-active check), openQuestion(), beginImpact(), beginTerminal(), restartWork(), initial load"
acceptance:
  - "Manual/visual: idle-pulse and defer-gauge confirmed mutually exclusive across finishClose()'s branch."
executed: '2026-08-27' · Hamlet_Pop_Upper_v1.8_SINGLE_FILE.html
```

**Incidental fix (not one of his four, found during this pass):** v1.7's
`.terminal-marked` hairline (HPU-09) used `classList.add` with no remove —
after a restart, the class was already present and the animation would
never replay. Fixed in `beginTerminal()` to remove-then-add.

## Ballot round 2 — verdict received (2026-08-27)

Pasted verbatim, as received:

```yaml
# Hamlet Pop-Upper — Ballot Round 2 (prototype) verdict
ballot: hamlet-pop-upper-r2
ballot_version: 1.0.0
exported: 2026-08-27
selected_by: Mohammad Zare (Mozare)
provenance:
  artifact: hamlet-pop-upper-ballot-r2.html
  lineage: hamlet-pop-upper-ballot-r1.html (HPU-B4 hold) -> r2 (this prototype)
  plan: plan-hamlet-pop-upper.md HPU-05
verdicts:
  HPU-B4-r2:
    title: "One glyph, drawn once, placed where the mark already lived"
    verdict: promote_both
next_step: promote -> wire the glyph into Hamlet_Pop_Upper_v1.7_SINGLE_FILE.html at the voted locus/loci and re-verify; decline -> record do-not-re-propose on HPU-05 and close the lineage.
```

**Process note, logged honestly rather than glossed over:** he had already
said "build the skull too" before this verdict arrived. That instruction
was only carried out as far as the r2 ballot study — it was never wired
into the actual shipped artifact he was then given to look at, which is
exactly what he was working from when he asked. That was a real execution
gap, not a process ambiguity; it is recorded here rather than smoothed
over.

```yaml
decision: HPU-16
title: Skull glyph promoted to both loci — rail-mark and afterimage
status: decided
priority: P2
origin: unmade-decision
finding: resolves HPU-05 / ballot HPU-B4-r2 (verdict promote_both)
evidence:
  repo: (local artifact)
  commit: v1.8 sha256 c519b8e1a8a9b5fcdacc2d777acdbdb7987c765b9e23bb410d69a5e894dd7b10
  locus: "Hamlet_Pop_Upper_v1.8_SINGLE_FILE.html — shared #skullGlyph <defs>, .rail-mark (replaces the generic \"?\"), .after-skull inside #machineAfterimage"
  observed: '2026-08-27'
  method: source-read, browser verification (Playwright screenshots: v18_skull_railmark.png at the live rail-mark, v18_skull_afterimage.png inside the terminal afterimage)
decision_text: >
  The exact glyph built and shown in ballot r2 (12x12 pixel grid, no
  curves, evenodd-free — plain filled rects in a shared SVG <g>, referenced
  by <use> so both loci are provably the same shape) is now live at both
  voted loci: the instrument-rail mark (ink-colored, replacing the generic
  "?") and a faint ghost inside the machine's afterimage (bone-colored,
  rgba(232,229,215,.15), centered). The wound-layer locus from the r2 study
  was shown for contrast, not proposed, and stays excluded per that
  study's own reasoning (competes with HPU-07's wound-as-residue decision).
implementation:
  - "shared <g id=\"skullGlyph\"> in a hidden defs <svg>, 115 unit rects"
  - ".rail-mark: <svg><use href=\"#skullGlyph\"/></svg> replacing the literal \"?\" text"
  - "#machineAfterimage: <svg class=\"after-skull\"><use .../></svg>, centered via CSS"
acceptance:
  - "Screenshot-confirmed at both loci on the live artifact, not the ballot study."
executed: '2026-08-27' · Hamlet_Pop_Upper_v1.8_SINGLE_FILE.html
```

## Plan state — after v1.8 (skull wired in)

- **Decided:** HPU-01, HPU-07 through HPU-16 — all executed and
  Playwright-verified in `Hamlet_Pop_Upper_v1.8_SINGLE_FILE.html`
  (sha256 `c519b8e1a8a9b5fcdacc2d777acdbdb7987c765b9e23bb410d69a5e894dd7b10`).
- **Held:** none.
- **Declined:** none.
- Every open item from both ballot rounds and the playtest diagnosis is
  now closed. Lineage is closed; station 6 (implementation kit) is
  available if he wants one, though for a single autonomous-HTML work the
  artifact itself has been the appropriate deliverable shape throughout.

## Final integration pass and publish (2026-08-27)

One full real-time (unaccelerated) Playwright run through the entire
piece, father through the terminal dwell, with realistic mixed interaction
(remember+answer, later, plain answers) — checking the whole system
together rather than each fix in isolation:

- Skull present at the rail-mark from first paint.
- Full run produced a healthy mix of encounter kinds, including multiple
  `recalled` (confirms HPU-10 holds under real, not just scripted, play).
- Terminal reached; restart correctly revealed after the dwell.
- Zero JS errors (`pageerror` and `console.error`) across the entire run.
- Screenshot of the final frame: mortality field complete, terminal line
  present, afterimage with the skull ghost visible, structurally intact.

Published live: **https://claude.ai/code/artifact/e6a83225-e76c-4828-83db-30fd92eec64b**
(sha256 `c519b8e1a8a9b5fcdacc2d777acdbdb7987c765b9e23bb410d69a5e894dd7b10` —
same file as delivered, now also playable directly rather than only as a
downloaded file).

## Skull emphasis at the terminal scene (2026-08-27)

Direct instruction, not a ballot item: "in final scene, i want that skull
bigger, emerges as the foreground, part of it below the text layer." Fixed
directly.

```yaml
decision: HPU-17
title: Terminal-scene skull enlarged and layered in front of the mortality text
status: decided
priority: P2
origin: unmade-decision
finding: direct instruction, not diagnosed from a defect
evidence:
  repo: (local artifact)
  commit: v1.8 sha256 39691846dc87d970a446db2c010337f509c08ec750a22c72881dd2815a987bc3
  locus: "Hamlet_Pop_Upper_v1.8_SINGLE_FILE.html — .mortality-field z-index (51), .after-skull sizing/animation (210-ish), new @keyframes skull-emerge"
  observed: '2026-08-27'
  method: source-read, browser verification (Playwright screenshots at 1440x900 and 412x915)
decision_text: >
  .mortality-field raised from z-index 2 to 12, above .machine-afterimage's
  11 — the accumulated death lines now render in front of the afterimage
  at the terminal scene, so a bigger skull can sit partly behind/under them
  without swallowing the text. .after-skull resized from a fixed 15% of
  the small captured chassis rect (min 34px) to clamp(150px,46vmin,360px)
  — sized off the viewport, not the tiny remembered chassis, so it visibly
  outgrows the structural burn around it. Given its own emergence keyframe
  (skull-emerge, mirroring the existing afterimage-arrive curve) rather
  than just inheriting the parent's fade, so it reads as surfacing rather
  than simply being large from the first frame.
implementation:
  - ".mortality-field{z-index:12}" (was 2)
  - ".after-skull{width/height:clamp(150px,46vmin,360px);color:rgba(232,229,215,.42);opacity:0;transform:...scale(.72)}"
  - "@keyframes skull-emerge, applied via .machine-afterimage.is-present .after-skull"
acceptance:
  - "Screenshots at 1440x900 and 412x915 confirm the skull dominates the terminal frame, overlaps the mortality text, and the text stays legible in front where they intersect, on both."
executed: '2026-08-27' · Hamlet_Pop_Upper_v1.8_SINGLE_FILE.html
```
