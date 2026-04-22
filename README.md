# Exercise Logger - WIP

A personal workout tracking app built to log training sessions, track progression week over week, and see how far you've come across a full program.

Self-hosted on a VPS. No subscriptions, no third-party cloud, no nonsense.

---

## What it does

- **Training programs** with days (e.g. Pass A / Pass B / Pass C), grouped into sections and exercises
- **Session logging** — each time you complete a workout, you log the actual sets, reps, and weight you used
- **Carry-forward** — when you start a session, it prefills from your previous session for the same day. You adjust up or down based on how you feel
- **Program progression** — when a program block is complete, archive it and clone it. The new program starts where the last one ended (your final weights become the new baselines)
- **History** — every session is saved as a snapshot, so you can always look back and compare where you started vs where you ended up

---

## Tech stack

| Layer | Choice |
|-------|--------|
| Frontend | React + Vite + TypeScript |
| Styling | Plain CSS with CSS custom properties |
| Backend | Node.js + Express + TypeScript |
| ORM | Prisma |
| Database | PostgreSQL |
| Auth | JWT + bcrypt |
| Deployment | Self-hosted VPS |

---

## AI disclosure

This project is built openly — including how it was made.

**The backend** (Express, Prisma schema, routes, business logic) is written by me. I'm using this project to properly learn backend development in TypeScript, so that side is intentionally hand-written.

**The frontend** (React components, UI, styling) is generated with [Claude](https://claude.ai) by Anthropic. I provide the design system, component specs, and the API contracts — Claude writes the implementation. This lets me focus my learning time on the backend while still ending up with a well-designed UI.

The schema design, architecture decisions, and overall direction are collaborative — discussed and reasoned through with Claude as a design partner before anything gets built.

---

## Project structure

```
träningsapp/
├── client/               # React frontend (Vite + TypeScript)
│   └── src/
│       ├── components/
│       ├── pages/
│       ├── hooks/
│       ├── services/
│       └── types/
├── server/               # Express backend (TypeScript)
│   ├── src/
│   │   ├── controllers/
│   │   ├── services/
│   │   ├── routes/
│   │   ├── middleware/
│   │   └── utils/
│   └── prisma/
│       └── schema.prisma
└── .env                  # Not committed
```

---

## Database schema (overview)

The schema separates **the plan** from **the log**.

**Plan side** — the program template:
- `Program` → a training block ("Styrka & Form")
- `ProgramDay` → a named training day ("Pass A")
- `Section` → a group of exercises ("Superset 1", "Zone 2")
- `ProgramExercise` → a single exercise with target sets, reps, and starting weight

**Log side** — what actually happened:
- `Session` → one occurrence of completing a day
- `SessionExercise` → links a session to a template exercise
- `SessionSet` → the actual data: reps, weight, duration, or distance per set

When a program block is finished, it's archived and cloned. Each exercise's `targetWeight` in the new program is set from the final session's actual weight — your endpoint becomes the next starting point.

---


## Status

Currently in active development. Following a phased build plan:

- [x] Schema design + architecture
- [ ] Phase 1 — Server scaffold + DB
- [ ] Phase 2 — Auth
- [ ] Phase 3 — Program CRUD
- [ ] Phase 4 — Session logging
- [ ] Phase 5 — React app + login
- [ ] Phase 6 — Dashboard + program view
- [ ] Phase 7 — Session logger UI
- [ ] Phase 8 — VPS deployment

---

## Self-hosting

The app is designed to be self-hosted on any Linux VPS. Deployment notes will be added as part of Phase 8.

---

## License

MIT — do whatever you want with it.
