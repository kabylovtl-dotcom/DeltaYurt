# DeltaYurt — Live Physics Classroom

![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)
![TypeScript](https://img.shields.io/badge/TypeScript-strict-blue)
![Netlify](https://img.shields.io/badge/deployed-Netlify-00C7B7)

**Live:** https://deltayurt.netlify.app

DeltaYurt is a real-time virtual classroom platform built to replace costly physical lab equipment with interactive physics simulations. It was created to address a gap in STEM accessibility at schools across Kyrgyzstan, where lab infrastructure is limited or unavailable.

The platform supports live teacher-student sessions, in-browser physics simulations, and multilingual delivery in **English, Russian, and Kyrgyz**.

---

## Why It Exists

After observing that students in our school were consistently outperforming in humanities but disengaging from STEM, I started researching the root cause. The answer was partly infrastructural — no working oscilloscopes, no optics equipment, no way to run real experiments. DeltaYurt was built to close that gap digitally.

A 200-student pilot was conducted at Jusup Balasagyn High School. D7 retention increased by 43% compared to the previous digital tool in use.

---

## Tech Stack

**Frontend:** React · TypeScript · Vite · Tailwind CSS · shadcn/ui · Zustand · Framer Motion · Recharts

**Backend:** Node.js · Express · TypeScript

**Realtime:** Socket.IO

**i18n:** Custom JSON locale system (EN / RU / KY)

**Infra:** Netlify (frontend) · Bun

---

## Features

- **Live classroom sessions** — teacher broadcasts, students join via class code
- **Physics simulations** — pendulum, wave interference, electromagnetism (more in progress)
- **Real-time interaction** — chat, raise hand, simulation control queue
- **Multilingual UI** — full EN/RU/KY support with runtime language switching
- **Student profiles** — XP, achievements, experiment history, leaderboard
- **Lesson calendar** — scheduling with deadline tracking
- **Dark / light / system theme**

---

## Project Structure

```
src/
  components/
    classroom/     # live session UI
    teacher/       # dashboard, class management
    student/       # student-side views
    ui/            # shared components
  store/           # Zustand state
  types/           # shared TypeScript types
  pages/           # route-level components
  i18n.ts          # i18n config

server/
  index.ts         # Express + Socket.IO entrypoint
  seed.ts          # dev seed data

public/
  locales/
    en/ ru/ ky/    # translation JSON files
```

---

## Getting Started

```bash
# Requirements: Node >= 20

npm install
cd server && npm install && cd ..

# Terminal 1 — backend
cd server && npm run dev    # http://localhost:3005

# Terminal 2 — frontend
npm run dev                 # http://localhost:8081
```

### Environment

```ini
# server/.env
PORT=3005
NODE_ENV=development
```

### Dev accounts (local only)

```
Teacher:  teacher@deltayurt.test  /  password123
Student:  student1@deltayurt.test /  password123
Class code: DY-TEST1
```

---

## Adding a Language

```bash
cp -r public/locales/en public/locales/<lang>
# translate the JSON files
```

Then register the locale in `src/i18n.ts` and add the option to `LanguageSwitcher.tsx`.

---

## Socket.IO Events (key)

| Event | Direction | Description |
|---|---|---|
| `register_user` | client→server | auth on connect |
| `join_class` | client→server | student joins session |
| `teacher_start_lesson` | client→server | opens live room |
| `teacher_present_simulation` | client→server | pushes sim to students |
| `chat_message` | both | classroom chat |
| `raise_hand` | client→server | student queue |
| `grade_submission` | server→client | feedback delivery |

---

## Roadmap

- [ ] Vitest unit tests + Playwright e2e
- [ ] PWA support (offline simulation cache)
- [ ] Sentry error tracking
- [ ] Additional simulations: optics, thermodynamics
- [ ] Teacher analytics dashboard

---

## License

MIT © 2025 kabylovtl
