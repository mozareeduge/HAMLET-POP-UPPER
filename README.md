# Hamlet Pop-Upper

*Hamlet Pop-Upper* is a browser-native miniature adaptation by Mohammad Zare (Mozare). Hamlet's question returns through a popup apparatus that can retain a selection or defer another call while a fixed score of deaths advances.

**Live work:** https://hamlet-popupper.theblackbirdfield.com/

## Dramaturgical structure

"Father is dead." establishes the opening condition. Seven timed events then follow in a fixed order: Polonius, Ophelia, Mother, Claudius, Rosencrantz and Guildenstern, Laertes, and Hamlet.

The dialog repeatedly presents `TO BE` and `NOT TO BE`. Selecting **Remember this answer** retains the chosen value for later dialog openings. Selecting **Ask me later** schedules another opening. These actions change the state in which the next timed event meets the apparatus; they leave the score's wording, order, and timing intact.

Before a death enters the visible list, the program classifies the encounter:

- `unanswered` — the question was open without a selected answer;
- `answer_collision` — the death arrived while an answer was being registered;
- `deferred` — the death arrived during an active "Ask me later" interval;
- `recalled` — the dialog was closed while a remembered answer remained active;
- `answered` — the dialog was closed after an unremembered answer;
- `closed` — the dialog was closed without one of the more specific conditions;
- `terminal` — Hamlet's final event.

The class changes three visible relations: the incision shown when the event reaches the dialog, the position and density of the thin line retained on the dialog surface, and the connection drawn from the corresponding death line back to the apparatus. Visitor choices therefore alter each encounter and its trace while the tragedy keeps its fixed sequence.

## Implementation

The work is contained in one dependency-free `index.html` file. It requires no build step, runtime package, external asset, or network service.

The question uses an in-page HTML `<dialog>` opened with `showModal()`. While it is open, the browser places it in the top layer and makes the rest of the document inert.

Selecting **Remember this answer** writes `to_be` or `not_to_be` to `localStorage` under `hamlet-pop-upper:answer`, allowing later dialog openings and later sessions on the same origin to retrieve it. When persistent storage is unavailable, an in-memory value carries the selection through the current session; clearing **Remember this answer** removes the saved value. Answer data stays within browser storage.

The score uses accumulated visible-page time. `worldElapsed` advances while `document.hidden` is false, so switching away from the tab pauses score time. The stored answer and the death timer remain separate inputs to `classifyEncounter()`, which derives the encounter class used by the trace system.

Sound consists of synthesized cues created through the Web Audio API after visitor input. The artifact also includes keyboard-operable controls, live regions, reduced-motion handling, forced-colors handling, and responsive layouts.

The work's own artist statement is reachable two ways, because the dialog's `showModal()` makes the rest of the document inert while it's open — a corner control outside the dialog would be unreachable for most of the piece's runtime. A "Statement" mark in the dialog's own instrument rail swaps its content area to the statement text whenever the dialog is open (reachable in the majority state); a matching "Statement" disclosure at the top-right corner covers the windows when the dialog is closed. Opening one auto-closes the other rather than risking the two ever being visibly open at once, and Hamlet's own terminal line is guaranteed never to land hidden behind the statement pane, even if a visitor is mid-read when it arrives.

## Run locally

Serve the repository root through a static server so the page has a stable origin:

```bash
python -m http.server 8000
```

Then open `http://localhost:8000/`.

## Repository map

- `index.html` — released work
- `CHANGELOG.md` — version history
- `change-plan/plan-hamlet-pop-upper.md` — design decisions, implementation evidence, and verification history
- `archive/` — retained earlier versions
- `docs/HAMLET_POP_UPPER_STATEMENT.md` — public work statement
- `docs/HAMLET_POP_UPPER_RESEARCH_NOTE.md` — artistic-research context and sources

## Research lineage

In *Hamlet*, the question enters another dramatic moment each time the work calls it back. In the interface, another dialog opening brings earlier selections and deferrals into the present machine state. Joining these operations allows a past answer to return as material inside a later death event. The [research note](docs/HAMLET_POP_UPPER_RESEARCH_NOTE.md) documents the popup sources, browser-warning research, claim boundaries, and the mechanisms through which that research enters the artifact.

## Current edition

- Version: `v1.8`
- Form: autonomous single-file HTML
- Language: English
- Duration: approximately ninety seconds to the terminal state
- Author: Mohammad Zare (Mozare)
- Year: 2026

## Citation

> Zare, Mohammad (Mozare). *Hamlet Pop-Upper*. Browser-native miniature adaptation, version 1.8, 2026.
