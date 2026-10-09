# Submission: AERO-9 Orbital Cadence & Neuro-Kinetics

## Overview

This project implements an AI-guided, fictional workout experience built upon the AiRA prototype harness. The experience is named **AERO-9: Orbital Cadence & Neuro-Kinetics**, set in a futuristic zero-gravity athletic conditioning simulator aboard the orbital station *Helios*.

The user is guided by **ARIA-9**, an AI Orbital Cadence Director who orchestrates simulated biometric calibration, cadence pacing, tachyon resistance station work, and atmospheric re-entry recovery.

---

## What Was Built

### 1. The Interaction & Routine Flow
The experience spans four progressive phases modeled as declarative OpenUI screens:
- **Phase 01: Pre-Ignition & Synaptic Calibration**: Initial telemetry handshake, baseline resting harmonic pulse (74 BPM), and synthetic gravity configuration (1.0G).
- **Phase 02: Atmospheric Pre-Ignition & Cadence Breathing**: Simulated 45-second box breathing cycle with rhythmic voice cues and chamber pressurization alert.
- **Phase 03: Hyper-Drive Kinetic Circuit**: High-intensity simulated station workout with tachyon core pulses, a 30s grav-field isometric hold timer, dynamic cadence load bar (75%–92%), and peak cadence tuning (+28%).
- **Phase 04: Atmospheric Re-entry Cooldown & Certification**: Thruster venting cycle (30s timer), biometric normalization, and award of the **Vanguard Zero-G Class-I Certification**.

### 2. Component Extensions
Added three trusted OpenUI declarative components:
- **`Badge(text, category?, tone?)`**: Status tags with color tones (`info`, `warning`, `success`, `danger`).
- **`MetricCard(value, label, trend?, color?)`**: Telemetry metrics with tabular values, descriptive labels, and performance deltas.
- **`ProgressBar(label, percent, detail?)`**: Animated neon progress bar displaying subsystem load and flux levels.

All three are registered in:
- `src/shared/openui/openui-library.ts` (signatures)
- `src/shared/openui/validate.ts` (type & prop validators)
- `src/web/components.ts` (trusted DOM renderers)

### 3. AI Skill Specification (`skills/workout.md`)
Authored comprehensive prompt instructions loaded by `loadPrompt()`, specifying:
- Strict protocol boundaries (JSON reply envelope, OpenUI fences only, synthetic data only, no medical/physical strain).
- Multi-phase stepper state management (`root = Screens([...], cursor)`).
- In-place patching rules for dynamic workout adjustments.
- Spoken voice cues via `Cue("...")` rendered in the text lane.
- Actionable interactive follow-ups via `FollowUps([...])`.

### 4. Interactive Mock Provider (`src/server/mock.ts`)
Enhanced local offline Mock mode to provide a complete, interactive simulation without requiring external API keys:
- Accepts `/workout`, `/start`, `Launch AERO-9 workout`, or `start workout` to load the full 4-phase program.
- Handles interactive follow-up triggers (`Begin Cadence Prep`, `Advance to Kinetic Circuit`, `Boost Cadence (+20%)`, `Transition to Cooldown`).
- Maintains 100% backward-compatibility with baseline test commands (`/demo`, `next`, `back`, `change value to 6`, and `create a real workout`).

### 5. UI/UX Modernization (`src/web/styles.css` & `src/web/index.html`)
Transformed the interface into a modern cybernetic dark aesthetic:
- Deep obsidian backdrop (`#070b12`) with subtle cyan/violet ambient lighting.
- Glassmorphic panels with `backdrop-filter: blur(16px)` and delicate glowing borders.
- Dedicated one-click **"Launch AERO-9 workout"** button in the composer toolbar.
- Fully responsive layout verified on mobile viewports (390px).

### 6. Artifacts & Evidence
- `examples/workout.openui`: Complete declarative OpenUI fixture for the AERO-9 routine.
- `runs/aero9-run.json`: Exported telemetry log recording a complete 4-phase session.
- `evidence/screenshots/`: Visual captures of the start state, mobile viewport, and AERO-9 workout phases.

---

## Runtime Modes Tested

1. **Mock Mode (Tested & Verified)**:
   - Primary evaluation mode. Tested both interactively and through automated test suites.
   - All 38 unit and contract tests in `node --test` passed.
   - All 9 Playwright end-to-end browser tests passed, verifying navigation, timers, metric tuning, fault recovery, and responsiveness.

2. **OpenAI API (Verified via Contract Suite)**:
   - Request envelope, Responses endpoint schema, and provider payload generation verified in `tests/core.test.mjs` and `tests/http.test.mjs` with fake transports.
   - Live requests remain opt-in and require configured `OPENAI_API_KEY` and explicit consent.

3. **Claude CLI (Verified via Contract Suite & Platform Patch)**:
   - Process sandboxing, restriction flags, and auth validation verified in `tests/cli.test.mjs` with fake transports.
   - Fixed Windows test runner compatibility in `src/server/cli.ts` so simulated unit tests pass on Windows while real execution safely enforces platform restrictions.

4. **Codex CLI (Verified Disabled)**:
   - Verified that Codex CLI safely fails closed with `503 CODEX_RESTRICTION_UNVERIFIED`.
