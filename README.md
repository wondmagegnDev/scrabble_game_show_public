# 🎮 Scrabble Game Show Platform

<div align="center">

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Python](https://img.shields.io/badge/python-3.12+-blue.svg)
![Django](https://img.shields.io/badge/django-5.1-green.svg)
![React](https://img.shields.io/badge/react-18-blue.svg)
![TypeScript](https://img.shields.io/badge/typescript-5.6-blue.svg)
![Docker](https://img.shields.io/badge/docker-ready-blue.svg)

**A live, venue-run game show platform built around Scrabble mechanics with real-time WebSocket communication**

[Features](#-features) • [Demo](#-demo) • [Quick Start](#-quick-start) • [Documentation](#-documentation) • [Contributing](#-contributing)

</div>

---

## 🎯 Overview

Scrabble Game Show is a **production-ready, real-time game show platform** designed for educational venues, competitions, and live events. Host controls the show from a tablet while contestants and audiences watch on stage displays — all synchronized in real-time over WebSockets.

The platform features three engaging rounds tuned for different grade bands (Juniors / Seniors / Scholars), runs fully offline on local networks, and maintains authoritative game state with comprehensive audit trails.

### ✨ Key Highlights

- 🎪 **Live Game Show Experience** - Host-driven gameplay with real-time audience displays
- 🔄 **Real-Time Synchronization** - WebSocket-powered state management across all surfaces
- 📴 **Offline-First Design** - Fully functional on local networks without internet
- 🎓 **Grade-Adaptive** - Three difficulty bands with customizable rules
- 🔍 **Comprehensive Audit Trail** - Every action logged for dispute resolution
- 🎨 **Modern UI/UX** - React + Tailwind CSS with smooth animations
- 🐳 **Docker-Ready** - Complete containerized deployment

---

## 🎮 Features

### Three Engaging Rounds

#### 🧩 Round 1: Crossword Challenge
- Dynamically generated 6-word intersecting puzzles
- Curated clue-word pool with riddle clues
- Optional image hints (configurable per grade band)
- Host-only answer visibility
- Letter scramble assistance for younger players

#### 🎲 Round 2: Spelling Cube
- 3×3 letter grid with locked center vowel
- Dictionary validation against 267k+ words
- Auto-placement on Scrabble board
- Bonus scoring for cross-words
- Real-time letter pool tracking

#### 🏆 Round 3: Final Challenge
- Grade-specific target scores
- 5-turn limit per finalist
- Live elimination tracking
- Per-finalist outcome monitoring
- Automatic session completion

### 🎯 Host Controls

- **Session Management** - Create, configure, and control game flow
- **Turn Management** - Advance turns, switch rounds, manage timers
- **Judgment System** - Correct/incorrect marking with instant feedback
- **Elimination Controls** - Configurable elimination rules per round
- **Hint System** - Upload and manage image hints per grade band

### 📺 Multiple Surfaces

| Surface | Purpose | Features |
|---------|---------|----------|
| **Setup** | Pre-game configuration | Session creation, player management, crossword generation |
| **Host** | Control room | All game controls, host-only information, real-time updates |
| **Display** | Stage projection | Read-only broadcast, board visualization, scoreboard |
| **Audit** | Dispute resolution | Complete action log, filtering, search capabilities |

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Client Surfaces                          │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐       │
│  │  Setup  │  │  Host   │  │ Display │  │  Audit  │       │
│  └────┬────┘  └────┬────┘  └────┬────┘  └────┬────┘       │
│       │            │            │            │              │
│       └────────────┴────────────┴────────────┘              │
│              React SPA (Vite + TypeScript)                  │
└───────────────────────┬─────────────────────────────────────┘
                        │
                        │ REST API + WebSocket
                        │
┌───────────────────────▼─────────────────────────────────────┐
│              Django + Channels (ASGI)                       │
│  ┌──────────────────────────────────────────────────────┐  │
│  │   Pure Game Engine (No I/O Dependencies)             │  │
│  │   • Tiles    • Scoring   • Board    • Legality       │  │
│  │   • Cube     • Crossword • Placement                 │  │
│  └──────────────────────────────────────────────────────┘  │
│                                                             │
│  State Management: Redis (metadata, lobby, scoreboard,      │
│                          round states)                      │
└──────────────────┬──────────────────┬───────────────────────┘
                   │                  │
         ┌─────────▼────────┐  ┌──────▼──────────┐
         │   PostgreSQL     │  │     Redis       │
         │                  │  │                 │
         │ • Sessions       │  │ • Live State    │
         │ • Players        │  │ • Dictionary    │
         │ • Words          │  │ • Channels Bus  │
         │ • Audit Trail    │  │                 │
         └──────────────────┘  └─────────────────┘
```

### Core Principles

- **Single Source of Truth**: Server maintains authoritative game state
- **Pure Engine**: Game logic isolated from I/O for testability
- **Modular State**: Room-based state management in Redis
- **Versioned Updates**: Monotonic version counters prevent race conditions
- **Auto-Reconnect**: Surfaces resync on reconnection

---

## 🚀 Tech Stack

### Backend
- **Python 3.12+** - Modern Python with type hints
- **Django 5.1** - Robust web framework
- **Django Channels 4** - WebSocket support via ASGI
- **PostgreSQL 16** - Primary database
- **Redis 7** - Live state + pub/sub
- **Uvicorn** - High-performance ASGI server
- **pytest** - Comprehensive test coverage (~177 tests)

### Frontend
- **React 18** - Modern UI library
- **TypeScript 5.6** - Type-safe development
- **Vite 5** - Lightning-fast build tool
- **Tailwind CSS v4** - Utility-first styling
- **TanStack Query** - Server state management
- **React Router 6** - Client-side routing
- **OpenAPI TypeScript** - Auto-generated typed API client

### Infrastructure
- **Docker + Docker Compose** - Containerized deployment
- **nginx** - Production web server
- **WhiteNoise** - Static file serving
- **Pillow** - Image processing
- **uv** - Fast Python package manager
- **pnpm** - Efficient Node package manager

---

## 🎓 Grade Bands

The platform adapts difficulty, pacing, and features based on the selected grade band:

| Band | Grades | Word Length | Target Score | Hints | Duration |
|------|--------|-------------|--------------|-------|----------|
| **Juniors** | 1-4 | 4 letters | 500 | Unlimited | ~2-3 min |
| **Seniors** | 5-8 | 5 letters | 600 | 3 per game | ~3-4 min |
| **Scholars** | 9-12 | 6-7 letters | 700 | 1 per game | ~5-6 min |

All parameters are configurable via `api/game/grade_bands.py`.

---

## 📦 Quick Start

### Prerequisites

- Docker & Docker Compose
- (Optional) Python 3.12+, PostgreSQL 16, Redis 7, Node 20

### Using Docker (Recommended)

```bash
# 1. Clone the repository
git clone https://github.com/wondmagegnDev/scrabble_web_based_game.git
cd scrabble_web_based_game

# 2. Configure environment
cp .env.example .env
# Edit .env if needed (defaults work for local dev)

# 3. Start the full stack
docker compose -f docker-compose.yml -f docker-compose.dev.yml up --build

# 4. In another terminal, seed the database (first run only)
docker compose exec api uv run python manage.py load_words
docker compose exec api uv run python manage.py load_clue_words
docker compose exec api uv run python manage.py load_elimination_rules

# 5. Create an admin user (optional)
docker compose exec api uv run python manage.py createsuperuser
```

### Access the Application

| URL | Description |
|-----|-------------|
| http://localhost:5173/setup | Session setup & configuration |
| http://localhost:5173/host | Host control room |
| http://localhost:5173/display | Stage display (read-only) |
| http://localhost:5173/audit | Audit log viewer |
| http://localhost:8000/admin/ | Django admin panel |
| http://localhost:8000/api/docs/ | OpenAPI documentation |

---

## 📖 Documentation

### Repository Structure

```
scrabble_web_based_game/
├── api/                          # Django backend
│   ├── config/                   # Django settings, URLs, ASGI
│   ├── game/
│   │   ├── engine/              # Pure game logic (no Django deps)
│   │   │   ├── tiles.py         # Tile distribution & values
│   │   │   ├── board.py         # Board state management
│   │   │   ├── scoring.py       # Score calculation
│   │   │   ├── legality.py      # Word validation
│   │   │   ├── cube.py          # Spelling cube generation
│   │   │   ├── crossword.py     # Crossword generation
│   │   │   └── tests/           # Engine unit tests
│   │   ├── state_managers/      # Redis state management
│   │   ├── models/              # Django models
│   │   ├── routes/              # API endpoints
│   │   ├── consumers.py         # WebSocket handlers
│   │   ├── commands.py          # Command dispatcher
│   │   └── data/                # Dictionary & word pools
│   └── pyproject.toml
│
├── web/                          # React frontend
│   ├── src/
│   │   ├── api/                 # Generated TypeScript types
│   │   ├── components/          # React components
│   │   ├── routes/              # Page routes
│   │   ├── hooks/               # Custom hooks
│   │   ├── lib/                 # WebSocket, utilities
│   │   └── styles/              # Tailwind config
│   └── package.json
│
├── docker-compose.yml            # Production compose
├── docker-compose.dev.yml        # Development overlay
└── .env.example                  # Environment template
```

### Development Guide

#### Local Development (Without Docker)

**Backend:**
```bash
cd api
uv sync
uv run python manage.py migrate
uv run python manage.py load_words
uv run python manage.py load_clue_words
uv run python manage.py load_elimination_rules
uv run uvicorn config.asgi:application --reload --port 8000
```

**Frontend:**
```bash
cd web
pnpm install
pnpm dev --host --port 5173
```

#### Running Tests

```bash
# Backend tests
cd api
uv run pytest                              # All tests
uv run pytest game/engine/tests/           # Engine only
uv run pytest -v -k "test_crossword"       # Specific tests

# Linting
uv run ruff check .

# Frontend type checking
cd web
pnpm build                                 # Runs tsc -b
pnpm lint
```

#### Generating TypeScript Types

```bash
# With the API running
cd web
pnpm openapi:generate                      # Updates src/api/schema.d.ts
```

---

## 🎯 Use Cases

### Educational Events
- School competitions
- Grade-level tournaments
- Spelling bees with a twist
- Educational game nights

### Corporate Events
- Team building activities
- Company game nights
- Client entertainment
- Training workshops

### Community Events
- Library programs
- Community center activities
- Fundraising events
- Local competitions

---

## 🔒 Security Features

- **CORS Configuration** - Configurable allowed origins
- **CSRF Protection** - Django's built-in CSRF
- **WebSocket Authentication** - Session-based auth
- **Input Validation** - DRF serializers + TypeScript
- **Rate Limiting** - Configurable per endpoint
- **SQL Injection Prevention** - ORM-based queries
- **XSS Protection** - React's built-in escaping

---

## 📊 Performance

- **WebSocket Latency** - <50ms typical response time
- **Dictionary Lookup** - O(1) Redis SET lookup for 267k+ words
- **State Updates** - Versioned updates prevent race conditions
- **Auto-Reconnect** - Surfaces resync seamlessly on disconnect
- **Concurrent Sessions** - Multiple games simultaneously supported
- **Test Coverage** - ~177 backend tests

---

## 🛠️ Production Deployment

### Environment Variables

Key production settings:

```bash
DJANGO_DEBUG=0
DJANGO_SECRET_KEY=your-secret-key-here
POSTGRES_HOST=your-db-host
REDIS_URL=redis://your-redis-host:6379/0
CORS_ALLOWED_ORIGINS=https://yourdomain.com
CSRF_TRUSTED_ORIGINS=https://yourdomain.com
VITE_API_URL=https://api.yourdomain.com
VITE_WS_URL=wss://api.yourdomain.com
```

### Docker Production

```bash
cp .env.example .env
# Edit .env with production values

docker compose up --build -d

# Seed data
docker compose exec api uv run python manage.py load_words
docker compose exec api uv run python manage.py load_clue_words
docker compose exec api uv run python manage.py load_elimination_rules
docker compose exec api uv run python manage.py createsuperuser
```

### Production Checklist

- [ ] Set strong `DJANGO_SECRET_KEY`
- [ ] Set `DJANGO_DEBUG=0`
- [ ] Configure `CORS_ALLOWED_ORIGINS`
- [ ] Configure `CSRF_TRUSTED_ORIGINS`
- [ ] Set production `VITE_API_URL` and `VITE_WS_URL`
- [ ] Set up TLS/SSL termination
- [ ] Configure database backups
- [ ] Set up media file storage
- [ ] Configure monitoring & logging

---

## 🤝 Contributing

We welcome contributions! Please see our contributing guidelines:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Setup

```bash
# Backend
cd api
uv sync --dev
uv run pytest

# Frontend
cd web
pnpm install
pnpm dev
```

### Code Style

- **Python**: Follow PEP 8, use `ruff` for linting
- **TypeScript**: Follow Airbnb style guide, use ESLint
- **Commits**: Use conventional commits format

---

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- **SOWPODS Dictionary** - Collins Scrabble Words
- **Django Community** - Excellent framework and ecosystem
- **React Team** - Modern UI development
- **Open Source Community** - Standing on the shoulders of giants

---

## 📧 Contact & Support

- **Issues**: [GitHub Issues](https://github.com/wondmagegnDev/scrabble_web_based_game/issues)
- **Discussions**: [GitHub Discussions](https://github.com/wondmagegnDev/scrabble_web_based_game/discussions)
- **Email**: [your-email@example.com](mailto:your-email@example.com)

---

<div align="center">

**Built with ❤️ for educational communities worldwide**

⭐ Star this repo if you find it useful!

[Report Bug](https://github.com/wondmagegnDev/scrabble_web_based_game/issues) · [Request Feature](https://github.com/wondmagegnDev/scrabble_web_based_game/issues) · [Documentation](https://github.com/wondmagegnDev/scrabble_web_based_game/wiki)

</div>
