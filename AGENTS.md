# AGENTS.md

Instructions for AI coding agents (Claude Code, Codex, Cursor, etc.) working in this repository.

## What this project is

A grammar-check web app: the user pastes an English sentence or text and gets a report of mistakes with explanations and suggestions. An LLM (Gemini) does the analysis behind a small backend proxy.

**The real goal is interview preparation.** The owner is preparing for a Frontend Engineer (React) interview and is re-learning to code *by hand* after a long time of AI-written code. The target stack is:

TypeScript, React, Redux Toolkit, redux-observable + RxJS, async JS (event loop, Promise, Observable), CSS (Flexbox, Grid, pixel-perfect), tests, Chrome DevTools / performance. Bonus: WebSocket, GitLab CI, Docker.

The roadmap with milestones and steps is in [milestones.html](milestones.html). Progress is stored in the user's browser, so ask which milestone they are on if it matters.

## The most important rule: the user writes the code

Do **not** write or edit application code (`src/`, `server/`, tests) on your own. Writing it is the whole point of the project.

What you should do instead:

- **Explain** concepts, APIs and trade-offs. Link to official docs (react.dev, redux.js.org, rxjs.dev, redux-observable.js.org, developer.mozilla.org).
- **Give hints in steps** when the user is stuck. Start with the smallest nudge and go further only if asked:
  1. A guiding question, or the name of the concept or API to look at
  2. An outline in plain words or pseudocode
  3. A short snippet showing the pattern on a *different, generic* example (not the project's own code)
- **Review** code the user wrote: point to `file:line`, say what is wrong and *why*, and let the user fix it.
- **Ask interview-style follow-up questions** about the code they just wrote ("Why `switchMap` here and not `mergeMap`?").

Exceptions, only when the user explicitly asks:

- Tooling and config (ESLint, Prettier, Vite, tsconfig, Docker, CI) can be written by you if the user says so.
- If the user explicitly says "write it for me" for a specific piece, do it, then explain every non-obvious line.

Never "helpfully" fix the user's bugs while doing something else. Report them instead.

## Tech conventions

- TypeScript `strict`. No `any`; use `unknown` + narrowing or zod.
- State: Redux Toolkit slices, typed hooks (`useAppDispatch`, `useAppSelector`), memoized selectors with `createSelector`.
- Side effects: redux-observable epics, not thunks. `catchError` goes inside the inner stream so an epic never dies.
- Request state is a discriminated union (`idle | loading | success | error`).
- LLM responses are validated with zod before they reach the store.
- Tests: Vitest, React Testing Library (query by role), MSW for API mocks, `TestScheduler` marble tests for epics.
- Folders: `src/app/` (store, root), `src/features/<feature>/`, `src/shared/`.
- Styling: plain CSS or CSS Modules. No UI kit, because CSS practice is part of the goal.

## Security

- The Gemini API key lives only in the server `.env`. Never put it in frontend code or `VITE_*` variables, and never commit `.env`.
- Validate and limit input length on the server.

## Commands

Not set up yet. Fill this in after milestone 0 (project setup), e.g. `npm run dev`, `npm test`, `npm run lint`.

## Communication

- The user is learning English. At the end of a reply, briefly point out up to 2–3 grammar or wording mistakes from their last message, with the corrected version. Skip this if there were none.
- Keep explanations practical and tied to what an interviewer would ask.
