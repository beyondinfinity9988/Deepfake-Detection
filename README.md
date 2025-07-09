# Deepfake-Detection

## MVP Architecture Overview

### ✅ Core Features in MVP

1. Upload image or video
2. Run DeepFake detection
3. Show results (score + visual explanation)
4. Shareable verification report (basic link)
5. Auth (optional for saved history)

---

## 🧱 Tech Stack Overview

### Frontend (Next.js + ShadCN)

- **Next.js (App Router)** – React SSR
- **Tailwind CSS + ShadCN UI** – Design system
- **TypeScript** – Type safety
- **UploadThing** or **Uppy** – Upload UI

### Backend (Python + FastAPI)

- **FastAPI** – Fast and async-friendly REST API
- **Celery** – Task queue for heavy video/image processing
- **Redis** – Message broker for Celery
- **PostgreSQL** – Result storage
- **S3 (or Supabase Storage)** – Media storage

### AI (Model Inference)

- Pre-trained models:
    - FaceForensics++, Deepware, DeepFakeDetectionChallenge (DFDC)
- Frameworks:
    - PyTorch/TensorFlow
- Hosted inference:
    - Replicate.com or Banana.dev for GPU-inference offload

### Infra

- **Vercel** – Frontend hosting
- **Render / Railway / Fly.io** – Backend + worker
- **GitHub Actions** – CI/CD
- **Docker** – Containerize inference pipeline

---

## 🧭 MVP App Structure (Mono-repo)

```
deepfake-pirate/
├── apps/
│   ├── web/                 # Next.js frontend
│   └── api/                 # FastAPI backend
├── packages/
│   ├── ui/                  # ShadCN UI components
│   └── utils/               # Shared TS + Python utils
├── workers/                # Celery task workers
│   └── detector.py         # Detection job logic
├── model/                  # Pretrained model loaders
│   └── inference.py        # DeepFake detection logic
├── infra/                  # Docker, Compose, CI, Nginx
├── .env                    # Shared secrets
└── README.md

```

---

## 📦 Detailed Folder Breakdown

### `apps/web/` (Next.js + Tailwind)

- `app/`
    - `page.tsx` → Homepage (upload UI)
    - `result/[id]/page.tsx` → Result display
- `components/`
    - `UploadForm.tsx` → Drag & drop upload
    - `ResultCard.tsx` → Score + interpretation
    - `Navbar.tsx`, `Footer.tsx`
- `lib/api.ts` → API call abstraction
- `hooks/useUpload.ts` → File upload hook
- `styles/` → Tailwind config

### `apps/api/` (FastAPI)

- `main.py` → App entrypoint
- `routes/upload.py` → Handle upload + schedule job
- `routes/result.py` → Fetch results
- `schemas/` → Pydantic models
- `services/` → Business logic
- `config.py` → Env management

### `workers/`

- `detector.py` → Receives job, runs model, returns results
- `queue.py` → Redis queue init
- `tasks.py` → Celery tasks

### `model/`

- `inference.py` → Load model, run prediction
- `utils.py` → Face alignment, preprocessing

---

## 🌐 Flow Diagram

1. **Frontend**
    - User uploads image/video
    - Calls `/upload` API → gets job ID
    - Polls `/result/:id` every few seconds
2. **Backend (FastAPI)**
    - Accepts file
    - Uploads to S3
    - Pushes job to Celery
3. **Worker (Celery)**
    - Pulls from Redis
    - Runs detection via model
    - Stores result in Postgres
    - (Optional) Generate visualization (e.g., heatmap)
4. **Frontend**
    - Fetch result and show

---

## ⚙️ Tech Setup Tasks

### Backend

- [ ]  Set up `FastAPI` app with `/upload`, `/result/:id`
- [ ]  Add Celery + Redis setup
- [ ]  Connect to PostgreSQL
- [ ]  Configure S3/Cloudinary for media
- [ ]  Integrate pre-trained model

### Frontend

- [ ]  Create landing page with pirate branding
- [ ]  Add upload drag-drop + status bar
- [ ]  Show result with “authenticity score”
- [ ]  Link to public report page (`/result/:id`)

### DevOps

- [ ]  Create Docker Compose with:
    - FastAPI + Celery
    - Redis + Postgres
- [ ]  GitHub Actions CI/CD
- [ ]  Deploy: Vercel (web), Render/Fly.io (API)

---

## 🧪 Detection Strategy (v1)

- Start with **image-only detection** using:
    - FaceForensics++ or Deepware
- Show score: **Real vs Fake (0–100%)**
- Bonus: Output heatmap or face landmarks

Later, add:

- **Video** support (by frame sampling)
- **Voice spoof** detection (optional)

---

## 🔐 Auth (Optional MVP)

- Use Clerk/Auth.js for:
    - Google/Email login
    - View scan history
    - Share private/public reports

---

Next:

- Scaffold this structure into a GitHub-ready repo?
- Help build the detection pipeline first.?
- Design the UI components or brand page?
