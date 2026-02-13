# SignLearn
![Next.js](https://img.shields.io/badge/Next.js-000000?style=for-the-badge&logo=next.js&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)
![MediaPipe](https://img.shields.io/badge/MediaPipe-Google-blue?style=for-the-badge)

**An AI-powered platform for learning American Sign Language with real-time webcam feedback.**

## About

SignLearn is a Duolingo-style web application that teaches American Sign Language through interactive lessons and live practice sessions. Users watch video demonstrations of ASL signs, then practice in front of their webcam while a computer vision model provides instant feedback on accuracy.

The core idea: Make ASL learning accessible, gamified, and effective by combining structured lessons with real-time AI-powered sign recognition.

## Technical Implementation

### Real-Time Sign Recognition

Built a gesture recognition pipeline using MediaPipe Hands for landmark detection and a custom classifier for ASL letter/sign identification:

* **Hand Tracking:** MediaPipe extracts 21 3D keypoints per hand at 30+ FPS
* **Feature Extraction:** Normalize landmarks relative to wrist position for scale/position invariance
* **Classification:** Custom model trained on ASL alphabet dataset, served via FastAPI
* **Feedback Loop:** WebSocket connection enables sub-200ms round-trip for live feedback

### Full-Stack Architecture

| Layer | Technology | Purpose |
|-------|------------|---------|
| Frontend | Next.js 14, TypeScript, Tailwind | SSR, webcam UI, real-time feedback display |
| Auth | NextAuth.js | Google OAuth + credentials |
| Database | PostgreSQL + Prisma ORM | Users, progress, lessons, signs |
| ML API | Python FastAPI | Sign recognition inference |
| Storage | AWS S3 | Lesson videos, assets |
| Deployment | Vercel + Railway | Frontend + ML service |
| CI/CD | GitHub Actions | Automated testing and deployment |

### Gamification System

* **XP Points:** Earn points for completing lessons and practice sessions
* **Streaks:** Track consecutive days of practice
* **Progress Tracking:** Visual dashboard showing signs learned and accuracy over time
* **Spaced Repetition:** Resurface signs you've struggled with

## Tech Stack

| Category | Technologies |
|----------|--------------|
| Frontend | Next.js, React, TypeScript, Tailwind CSS |
| Backend | Node.js, FastAPI, Prisma |
| Database | PostgreSQL (Neon) |
| ML/CV | MediaPipe, TensorFlow/PyTorch |
| Infrastructure | Docker, AWS S3, Vercel, Railway |
| DevOps | GitHub Actions, ESLint, Prettier |

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     Next.js Frontend                        │
│            Webcam Capture │ Lesson UI │ Dashboard           │
└─────────────────────────────┬───────────────────────────────┘
                              │ REST / WebSocket
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    API Layer (Next.js)                      │
│                 NextAuth │ Prisma │ Routes                  │
└──────────────┬──────────────────────────────┬───────────────┘
               │                              │
               ▼                              ▼
┌──────────────────────────┐    ┌─────────────────────────────┐
│       PostgreSQL         │    │      FastAPI ML Service     │
│   Users, Progress, etc.  │    │   MediaPipe + Classifier    │
└──────────────────────────┘    └─────────────────────────────┘
```

## Project Structure

```
signlearn/
├── apps/
│   ├── web/                    # Next.js frontend
│   │   ├── app/
│   │   │   ├── (auth)/         # Login, signup
│   │   │   ├── (dashboard)/    # Main app
│   │   │   ├── lessons/        # Lesson pages
│   │   │   ├── practice/       # Webcam practice
│   │   │   └── api/            # API routes
│   │   ├── components/
│   │   ├── lib/
│   │   └── prisma/
│   │
│   └── ml-api/                 # Python FastAPI
│       ├── app/
│       │   ├── main.py
│       │   └── model.py
│       ├── models/
│       └── Dockerfile
│
├── .github/workflows/
├── docker-compose.yml
└── README.md
```

## How to Run

```bash
# Clone repository
git clone https://github.com/kevinlukaixing/signlearn.git
cd signlearn

# Install frontend dependencies
cd apps/web
npm install

# Set up environment variables
cp .env.example .env.local
# Add your database URL, OAuth credentials, etc.

# Run database migrations
npx prisma migrate dev

# Start development server
npm run dev
```

### ML Service

```bash
cd apps/ml-api

# Create virtual environment
python -m venv venv
source venv/bin/activate  # or `venv\Scripts\activate` on Windows

# Install dependencies
pip install -r requirements.txt

# Run FastAPI server
uvicorn app.main:app --reload
```

## Roadmap

- [x] Project setup and architecture
- [ ] Authentication system
- [ ] Lesson content and video player
- [ ] Webcam capture component
- [ ] ML model integration
- [ ] Real-time feedback UI
- [ ] Progress tracking and XP system
- [ ] Leaderboards
- [ ] Mobile responsiveness

## License

MIT License — see [LICENSE](LICENSE) for details.
