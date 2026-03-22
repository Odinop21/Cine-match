<div align="center">

```
 ██████╗██╗███╗   ██╗███████╗███╗   ███╗ █████╗ ████████╗ ██████╗██╗  ██╗
██╔════╝██║████╗  ██║██╔════╝████╗ ████║██╔══██╗╚══██╔══╝██╔════╝██║  ██║
██║     ██║██╔██╗ ██║█████╗  ██╔████╔██║███████║   ██║   ██║     ███████║
██║     ██║██║╚██╗██║██╔══╝  ██║╚██╔╝██║██╔══██║   ██║   ██║     ██╔══██║
╚██████╗██║██║ ╚████║███████╗██║ ╚═╝ ██║██║  ██║   ██║   ╚██████╗██║  ██║
 ╚═════╝╚═╝╚═╝  ╚═══╝╚══════╝╚═╝     ╚═╝╚═╝  ╚═╝   ╚═╝    ╚═════╝╚═╝  ╚═╝
```

**Your personal AI-powered movie recommender.**
UNDER DEVELOPMENT!!!!
Search movies · Get smart recommendations · Find where to stream · Track what you watch.

[![React](https://img.shields.io/badge/React-18-61DAFB?style=flat-square&logo=react)](https://react.dev)
[![Node.js](https://img.shields.io/badge/Node.js-20-339933?style=flat-square&logo=nodedotjs)](https://nodejs.org)
[![Python](https://img.shields.io/badge/Python-3.11-3776AB?style=flat-square&logo=python)](https://python.org)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-4169E1?style=flat-square&logo=postgresql)](https://postgresql.org)
[![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?style=flat-square&logo=docker)](https://docker.com)
[![License](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)](LICENSE)

[Features](#-features) · [Tech Stack](#-tech-stack) · [Getting Started](#-getting-started) · [Architecture](#-architecture) · [API Reference](#-api-reference) · [Roadmap](#-development-roadmap)

</div>

---

## 🎬 What is CineMatch?

CineMatch is a full-stack movie recommendation platform that learns your taste and suggests what to watch next. Tell it what you've liked, and it uses a machine learning model trained on movie descriptions, genres, and themes to find films you'll genuinely enjoy — not just what's trending.

Built as a learning project from the ground up over 5 phases, CineMatch covers the full spectrum of modern web development: a React frontend, a Node.js/Express API, a PostgreSQL database, a Python-powered ML recommendation microservice, Redis caching, JWT authentication, and Docker containerisation.

---

## ✨ Features

**Movie Discovery** — Search across thousands of movies powered by the TMDB API. Every result includes full details: description, cast photos, director, crew, rating, release year, and trivia.

**AI Recommendations** — Mark movies as watched or liked and CineMatch builds a taste profile. The recommendation engine uses TF-IDF vectorisation and cosine similarity to find movies that match your preferences by genre, theme, and director style. The more you interact, the better it gets.

**Streaming Availability** — See exactly where a movie is streaming right now (Netflix, Prime Video, Disney+, Apple TV+, and more). If it's not available on any platform, the app handles that gracefully and links you to JustWatch for alternatives.

**Smart Search Autocomplete** — A Trie data structure powers instant prefix-based suggestions as you type, with no API call needed on every keystroke.

**Personalised Watch History** — Your watched list and liked movies are saved to your account. Recommendations exclude what you've already seen and explain why each film was suggested ("Because you liked Oppenheimer...").

**Stats Dashboard** — See your personal stats: total watched, total liked, and your recommendation match accuracy.

---

## 🛠 Tech Stack

| Layer          | Technology              | Purpose                                |
| -------------- | ----------------------- | -------------------------------------- |
| Frontend       | React 18 + Vite         | UI components, routing, state          |
| Backend        | Node.js + Express       | REST API, auth middleware, TMDB proxy  |
| ML Service     | Python 3 + Flask        | Recommendation engine microservice     |
| Database       | PostgreSQL + Prisma     | Users, movies, watch history           |
| Cache          | Redis                   | Streaming data cache (24hr TTL)        |
| Auth           | JWT + bcrypt            | Login sessions, password security      |
| ML Libraries   | scikit-learn, pandas    | TF-IDF + cosine similarity             |
| DevOps         | Docker + Docker Compose | One-command local setup                |
| Movie Data     | TMDB API                | Movie details, cast, posters, trailers |
| Streaming Data | Watchmode API           | Platform availability by region        |

---

## 🚀 Getting Started

### Prerequisites

Make sure you have the following installed before starting:

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) — the only hard requirement to run everything
- A free [TMDB API key](https://www.themoviedb.org/settings/api)
- A free [Watchmode API key](https://api.watchmode.com/)

### 1. Clone the repository

```bash
git clone https://github.com/yourusername/cinematch.git
cd cinematch
```

### 2. Set up environment variables

Copy the example env file and fill in your API keys:

```bash
cp .env.example .env
```

Then open `.env` and add your keys:

```env
# API Keys
TMDB_API_KEY=your_tmdb_api_key_here
WATCHMODE_API_KEY=your_watchmode_api_key_here

# Auth
JWT_SECRET=pick_any_long_random_string_here

# Database (pre-filled for Docker)
DATABASE_URL=postgresql://cinematch:password@db:5432/cinematch

# Redis (pre-filled for Docker)
REDIS_URL=redis://redis:6379
```

### 3. Start the entire application

```bash
docker-compose up --build
```

That's it. Docker starts PostgreSQL, runs database migrations automatically, starts the Node backend, starts the Python ML service, and serves the React frontend. Everything is wired together.

| Service                    | URL                   |
| -------------------------- | --------------------- |
| React Frontend             | http://localhost:5173 |
| Node.js API                | http://localhost:3001 |
| Python ML Service          | http://localhost:5001 |
| Prisma Studio (DB browser) | `npx prisma studio`   |

### 4. Running without Docker (local dev)

If you prefer running services individually during development:

```bash
# Terminal 1 — Backend
cd backend
npm install
npm run dev

# Terminal 2 — Frontend
cd frontend
npm install
npm run dev

# Terminal 3 — ML Service
cd ml-service
python -m venv venv
source venv/bin/activate   # Windows: venv\Scripts\activate
pip install -r requirements.txt
python app.py
```

---

## 🏗 Architecture

CineMatch is split into three independent services that communicate over HTTP. This mirrors a real-world microservice architecture and separates concerns cleanly.

```
┌─────────────────────────────────────────────────────────────┐
│                        Browser                               │
│                  React App (port 5173)                       │
└────────────────────────┬────────────────────────────────────┘
                         │ HTTP requests
                         ▼
┌─────────────────────────────────────────────────────────────┐
│               Node.js / Express API (port 3001)              │
│                                                              │
│  • Proxies TMDB requests (hides API key)                    │
│  • Handles auth (JWT + bcrypt)                              │
│  • Reads/writes PostgreSQL via Prisma                       │
│  • Reads/writes Redis cache                                 │
│  • Calls Python ML service for recommendations              │
└──────┬──────────────────┬───────────────────────────────────┘
       │                  │
       ▼                  ▼
┌────────────┐   ┌────────────────────────────────────────────┐
│ PostgreSQL │   │       Python / Flask ML (port 5001)         │
│  (port     │   │                                             │
│   5432)    │   │  • TF-IDF vectorisation of movie text      │
│            │   │  • Cosine similarity scoring               │
│  Users     │   │  • Returns ranked recommendation IDs      │
│  Movies    │   └────────────────────────────────────────────┘
│  History   │
└────────────┘
       ▲
┌──────┴─────┐
│   Redis    │
│  (port     │
│   6379)    │
│            │
│  Streaming │
│  cache     │
└────────────┘
```

### Why a separate Python service?

Python has the best machine learning ecosystem (scikit-learn, pandas, numpy). Node.js has the best web API ecosystem. Rather than compromise one for the other, the two services communicate over HTTP — Node handles all the web work, Python handles all the math. This is a standard pattern in production ML systems.

---

## 📁 Project Structure

```
cinematch/
│
├── frontend/                   # React application
│   ├── src/
│   │   ├── components/         # Reusable UI components
│   │   │   ├── MovieCard.jsx
│   │   │   ├── SearchBar.jsx
│   │   │   ├── DetailPanel.jsx
│   │   │   ├── StreamingBadge.jsx
│   │   │   ├── CastRow.jsx
│   │   │   ├── RecommendStrip.jsx
│   │   │   └── StatsBar.jsx
│   │   ├── pages/
│   │   │   ├── Home.jsx
│   │   │   └── MoviePage.jsx
│   │   ├── hooks/              # Custom React hooks
│   │   │   ├── useMovie.js
│   │   │   └── useSearch.js
│   │   ├── utils/
│   │   │   ├── Trie.js         # DSA: autocomplete search
│   │   │   ├── LRUCache.js     # DSA: in-memory cache
│   │   │   └── MinHeap.js      # DSA: top-K recommendations
│   │   └── App.jsx
│   └── Dockerfile
│
├── backend/                    # Node.js / Express API
│   ├── routes/
│   │   ├── movies.js
│   │   ├── auth.js
│   │   └── history.js
│   ├── middleware/
│   │   └── authenticate.js     # JWT verification
│   ├── prisma/
│   │   └── schema.prisma       # Database schema
│   ├── utils/
│   │   └── redis.js
│   ├── server.js
│   └── Dockerfile
│
├── ml-service/                 # Python recommendation engine
│   ├── recommender.py          # TF-IDF + cosine similarity logic
│   ├── app.py                  # Flask API
│   ├── requirements.txt
│   └── Dockerfile
│
├── docker-compose.yml
├── .env.example
└── README.md
```

---

## 📡 API Reference

### Movies

```
GET  /api/movies/search?q={query}     Search movies by title
GET  /api/movies/:id                  Full movie details + cast + crew
GET  /api/movies/:id/streaming        Streaming platform availability
```

### Authentication

```
POST /api/auth/register               Create a new account
POST /api/auth/login                  Log in, receive JWT token
```

### Watch History (Protected — requires JWT)

```
POST   /api/history                   Mark a movie as watched
PATCH  /api/history/:movieId          Update liked status
GET    /api/history                   Get your watch history
DELETE /api/history/:movieId          Remove from history
```

### Recommendations (Protected — requires JWT)

```
GET  /api/recommendations             Get personalised recommendations
                                      Returns popular movies if no history yet
```

### Example: Calling a protected route

```javascript
// All protected routes require this header
fetch("/api/recommendations", {
  headers: {
    Authorization: `Bearer ${yourJWTToken}`,
  },
});
```

---

## 🧠 How the Recommendation Engine Works

When you like a movie, CineMatch stores that in your watch history. When you request recommendations, here's what happens behind the scenes:

**Step 1 — Build a content profile.** Each movie's genres and description are combined into a single text string and converted into a numerical vector using TF-IDF (Term Frequency–Inverse Document Frequency). Words that are rare and specific (like "heist" or "dystopian") get weighted higher than common words.

**Step 2 — Find your taste vector.** The system looks at your last 10 liked movies and averages their TF-IDF vectors into a single "taste profile" that represents what you enjoy.

**Step 3 — Score everything.** Cosine similarity measures the angle between your taste vector and every movie in the database. A score of 1 means identical, 0 means nothing in common. Movies you've already watched are excluded.

**Step 4 — Return the top 10.** A min-heap data structure efficiently extracts the top 10 highest-scoring candidates in O(n log K) time without sorting the entire list.

```python
# The core logic, simplified
taste_vector = average(tfidf_vectors_of_liked_movies)
scores = cosine_similarity(taste_vector, all_movie_vectors)
recommendations = get_top_k(scores, k=10, exclude=watched_ids)
```

---

## 💾 Database Schema

```prisma
model User {
  id           Int            @id @default(autoincrement())
  email        String         @unique
  passwordHash String
  name         String?
  createdAt    DateTime       @default(now())
  watchHistory WatchHistory[]
}

model Movie {
  id           Int            @id        // TMDB movie ID
  title        String
  posterPath   String?
  overview     String?
  genres       String[]
  rating       Float?
  cachedAt     DateTime       @default(now())
  watchHistory WatchHistory[]
}

model WatchHistory {
  id        Int      @id @default(autoincrement())
  userId    Int
  movieId   Int
  liked     Boolean  @default(false)
  watchedAt DateTime @default(now())
  user      User     @relation(fields: [userId], references: [id])
  movie     Movie    @relation(fields: [movieId], references: [id])

  @@unique([userId, movieId])
}
```

---

## 🧩 DSA Implementations

One of the goals of this project was to implement core data structures from scratch and apply them to real features, not just solve them on LeetCode.

**Trie (`/frontend/src/utils/Trie.js`)** powers the search autocomplete. When movies are loaded, their titles are inserted into a Trie character by character. On each keystroke, the Trie traverses to the end of the typed prefix and collects all matching movie IDs in O(L) time, where L is the length of the prefix — far faster than scanning all titles linearly.

**MinHeap (`/frontend/src/utils/MinHeap.js`)** powers the top-K recommendation ranking. When the ML model returns 100 scored candidates, the heap maintains only the top 10 as it processes each candidate. This runs in O(n log K) time rather than O(n log n) for a full sort, which matters when n is large.

**LRU Cache (`/frontend/src/utils/LRUCache.js`)** powers the in-memory streaming data layer. It combines a HashMap for O(1) lookups with a doubly linked list for O(1) insertion and eviction. The least recently accessed entry is evicted when the cache reaches capacity — this is the classic LeetCode problem #146, implemented in production.

---

## 🔒 Security

- Passwords are hashed with bcrypt (10 salt rounds) before storage. Plain-text passwords are never written to the database.
- JWTs expire after 7 days and are verified on every protected request using a secret stored in environment variables.
- The TMDB and Watchmode API keys are never exposed to the frontend. All third-party API calls go through the Express backend.
- `helmet.js` sets secure HTTP headers on every response.
- Rate limiting is applied to the search endpoint (30 requests/minute per IP) to prevent abuse.

---

## 🧪 Running Tests

```bash
# Backend tests
cd backend
npm test

# Run with coverage report
npm run test:coverage
```

Tests cover the core API routes (`/api/movies/search`, `/api/movies/:id`, `/api/recommendations`), authentication flows, and all three DSA implementations (Trie, MinHeap, LRU Cache).

---

## 📈 Development Roadmap

This project was built in 5 phases. Each phase added a complete layer of functionality.

**Phase 0 — Foundation** covered the basics: how the internet works, HTML and CSS, JavaScript fundamentals (variables, array methods, async/await), and Git. Nothing was written in the project yet — this phase was about building the mental models needed to understand everything that came after.

**Phase 1 — Movie Data Display** was the first real code. A React frontend with a search bar, movie grid, and detail panel. An Express backend that proxies TMDB requests and hides the API key. A PostgreSQL database with Prisma for storing users and caching movie data. The Trie autocomplete was also built here.

**Phase 2 — AI Recommendations** added user authentication (registration, login, JWT), watch history tracking, and the Python ML microservice. The recommendation engine uses TF-IDF + cosine similarity and is called from the Node backend whenever a logged-in user requests suggestions. The MinHeap ranking was implemented here.

**Phase 3 — Streaming Availability** integrated the Watchmode API to show where each movie is streaming. Redis caching was added so streaming lookups are instant after the first fetch. All three edge cases are handled gracefully: available, rent/buy only, and completely unavailable. The LRU Cache was built alongside this.

**Phase 4 — Production Hardening** added tests (Jest + Supertest), performance optimisation (lazy loading, code splitting, React.memo), security headers (Helmet, rate limiting), and Docker containerisation. The app was deployed with the frontend on Vercel and the backend on Render.

**Phase 5 — Mobile** (in progress) is porting the web app to React Native using Expo. Approximately 70% of the component logic transfers directly. The main differences are navigation (React Navigation instead of React Router) and UI primitives (View/Text instead of div/p).

---

## 🌍 Deployment

The production app is deployed across three services:

| Service           | Platform | URL                                |
| ----------------- | -------- | ---------------------------------- |
| React Frontend    | Vercel   | https://cinematch.vercel.app       |
| Node.js Backend   | Render   | https://cinematch-api.onrender.com |
| Python ML Service | Render   | https://cinematch-ml.onrender.com  |

Environment variables are configured in each platform's dashboard. No secrets are stored in the repository.

---

## 🤝 Contributing

This is a personal learning project, but contributions and suggestions are welcome.

```bash
# Fork the repo, then:
git checkout -b feature/your-feature-name
git commit -m "feat: describe what you added"
git push origin feature/your-feature-name
# Open a pull request
```

Please follow the commit message convention used throughout: `feat:`, `fix:`, `refactor:`, `docs:`, `chore:`.

---

## 📄 License

MIT License — see [LICENSE](LICENSE) for details. Free to use, modify, and build upon.

---

## 🙏 Acknowledgements

- [TMDB](https://www.themoviedb.org/) for the movie data API (free and incredibly rich)
- [Watchmode](https://api.watchmode.com/) for streaming availability data
- [Prisma](https://www.prisma.io/) for making PostgreSQL feel approachable
- [scikit-learn](https://scikit-learn.org/) for TF-IDF and cosine similarity
- Every Stack Overflow answer that unblocked a 2am debugging session

---

<div align="center">
  <p>Built from scratch as a learning project · Phases 0 → 5</p>
  <p>
    <a href="https://github.com/yourusername/cinematch/commits/main">Commit History</a> ·
    <a href="https://github.com/yourusername/cinematch/issues">Issues</a> ·
    <a href="https://cinematch.vercel.app">Live Demo</a>
  </p>
</div>
