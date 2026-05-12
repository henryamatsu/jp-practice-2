# AI-Assisted Development Guidelines

## Core Philosophy

This project follows a **Smallest Demonstrable Feature (SDF)** approach to AI-assisted software development.

The goal is to move quickly **without sacrificing understanding, control, or correctness**.

Instead of generating large, complex systems in a single step, development proceeds in **small, atomic increments** that can be immediately demonstrated and verified through real behavior.

---

## Principles

### 1. Smallest Demonstrable Feature (SDF)

Every change must be:

- The **smallest possible unit of functionality**
- **Directly demonstrable through behavior** (UI, logs, interaction, etc.)
- **Meaningfully integrated** into the system

Avoid:

- Changes that are too large to review effectively
- Changes that cannot be observed or interacted with
- Purely theoretical or unused code

Good SDF examples:

- Render a video stream to the screen
- Capture microphone input and log activity
- Add a working mute button that toggles audio state

Bad SDF examples:

- Defining unused types or interfaces
- Creating full systems without demonstration points
- Implementing multiple features at once

---

### 2. Demonstrate After Every Step

After each SDF:

- Run the code
- Observe actual behavior (UI, logs, output, interaction)
- Confirm it works as expected before continuing

Do not rely on assumptions or unverified code.

---

### 3. Build Only What Is Needed (Just-In-Time Design)

Only implement functionality when it is actively being worked on.

Do NOT:

- Pre-build schemas, APIs, or systems for future features
- Speculatively design parts of the system

Instead:

- Build each piece when it becomes necessary
- Let the design evolve based on real usage

Example:

If building "Projects":

- Create the `Project` schema only when working on Projects
- Do NOT create `User`, `Team`, or relationships yet

---

### 4. Maintain Full Understanding

All generated code should be:

- Small enough to read and understand fully
- Easy to reason about
- Easy to modify or discard

If a change feels too large to mentally simulate, it is too big.

---

### 5. Prefer Iteration Over Completeness

Do not aim for "finished systems."

Instead:

- Build partial functionality
- Demonstrate it
- Extend it incrementally

---

## How to Break Features into SDFs

When given a larger feature, decompose it into **independent, demonstrable steps**.

### Step 1: Identify the End Goal

Define the user-visible outcome.

Example:

> "User can join a video call"

---

### Step 2: Break into Functional Layers

Split into high-level capabilities:

- Video capture
- Video rendering
- Audio capture
- Audio playback
- Controls (mute, camera toggle)

---

### Step 3: Convert Each Layer into SDFs

Each SDF must:

- Produce a visible or observable result
- Not depend on unfinished systems

Example breakdown:

#### Video

1. Access webcam stream
2. Render webcam stream to a video element
3. Handle permission errors visibly

#### Audio

4. Access microphone input
5. Demonstrate audio input (e.g., log levels or simple playback)

#### Controls

6. Add mute toggle (local state only, visibly changes state)
7. Connect mute toggle to audio track

---

### Step 4: Enforce Sequential Execution

Only work on **one SDF at a time**.

Rules:

- Do not combine steps
- Do not skip ahead
- Do not scaffold future steps

---

### Step 5: Demonstrate Before Continuing

After each SDF:

- Confirm it works through actual behavior
- Fix issues immediately
- Only then proceed

---

## AI Usage Guidelines

When using AI tools:

### Always Request:

- One SDF at a time
- Minimal code required to make it work
- A clear way to demonstrate the result
- No speculative abstractions
- No future-facing implementations

### Avoid Asking For:

- Full features or systems
- Boilerplate for unrelated parts
- Premature architecture decisions

---

## Definition of Done (Per SDF)

A feature is complete when:

- It runs without errors
- It produces observable behavior
- It can be demonstrated (UI, logs, or interaction)
- The code is understandable in isolation

---

## Anti-Patterns to Avoid

- "Build the entire feature"
- "Set up all schemas upfront"
- "Generate the full architecture"
- Large unreviewable diffs
- Undemonstrated code accumulation

---

## Summary

This workflow prioritizes:

- Speed through **clarity**, not automation
- Progress through **demonstration**, not assumption
- Simplicity through **incremental design**
