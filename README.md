# FaciliMap

**Campus Facility Finder & Issue Reporter for FCSIT, Universiti Malaya**  
Built by **Spirit Systems (OCC 3)** — WIA2006 Systems Analysis and Design.

FaciliMap helps students and staff quickly locate essential campus facilities around FCSIT (Motorcycle Parking, Water Dispenser, and Toilet), inspect real-time verification timestamps, and submit facility issue reports with photo evidence.

---

## Repository Structure

```text
facilimap/
├── frontend/     # Interactive Leaflet.js map UI, search/filter, detail modal, report & admin views
├── backend/      # REST API server (Node.js/Express or Python/Flask) + upload & auth middleware
├── database/     # SQLite 5-table schema, constraints/indexes, and seed data scripts
├── docs/         # Master Data Dictionary, 4-screen wireframes, PDPA & QA documentation
├── .gitignore
└── README.md
```

---

## Port Configuration

| Service | Default Port | Local URL | Notes |
|---|---|---|---|
| **Frontend** | `5173` (or `5500` Live Server) | `http://localhost:5173` | Static HTML/CSS/JS + Leaflet.js + OSM tiles |
| **Backend API** | `3000` (Express) / `5000` (Flask) | `http://localhost:3000/api` | Configured via `PORT` in `backend/.env` |
| **Database** | File-based (SQLite) | `database/facilimap.db` | Local SQLite file (gitignored; generated via schema + seed) |

> Make sure `CORS_ORIGIN` in `backend/.env` matches your frontend origin (e.g., `http://localhost:5173` or `http://127.0.0.1:5500`), and point frontend `API_BASE_URL` to the active backend port.

---

## Local Setup Steps

### 1. Clone the Repository
```bash
git clone https://github.com/YJ0310/facilimap.git
cd facilimap
git checkout dev
```

### 2. Backend Setup (`/backend`)
1. Copy the environment template:
   ```bash
   cd backend
   cp .env.example .env
   ```
2. Update `.env` with your local `PORT`, `DB_PATH=../database/facilimap.db`, `ADMIN_SECRET`, and `CORS_ORIGIN`.
3. Install dependencies and start the server:
   - **If Node.js / Express:**
     ```bash
     npm install
     npm run dev
     ```
   - **If Python / Flask:**
     ```bash
     python3 -m venv .venv
     source .venv/bin/activate
     pip install -r requirements.txt
     python app.py
     ```

### 3. Database Setup (`/database`)
Run the schema and seed scripts (once created in `T05`–`T13`) to initialize `database/facilimap.db` with the 5 core tables (`USER`, `CATEGORY`, `FACILITY`, `ISSUE_REPORT`, `ADMIN_REVIEW_LOG`) and FCSIT seed facilities.

### 4. Frontend Setup (`/frontend`)
1. Open `/frontend` using VS Code **Live Server** (port `5500`) or serve it locally:
   ```bash
   cd frontend
   npx serve -l 5173
   ```
2. Open `http://localhost:5173` in your browser.

---

## Git Workflow & Feature-Branch PR Rules

### Branch Strategy
- **`main`**: Production-ready, stable release branch. **Protected** — direct pushes are blocked; merges require a Pull Request from `dev` with review.
- **`dev`**: Active Sprint integration branch. **Protected** — direct pushes are blocked; all feature work merges here first via PR.
- **`feature/<scope>`** / **`fix/<scope>`**: Short-lived branches created from `dev` for individual tasks (e.g., `feature/map-search`, `feature/db-schema`, `feature/issue-report`, `fix/mobile-layout`).

### Pull Request Rules
1. **Always branch off `dev`:**
   ```bash
   git checkout dev
   git pull origin dev
   git checkout -b feature/<your-task>
   ```
2. **Keep commits focused and descriptive:**
   - Prefix commits with task ID when applicable, e.g. `feat(T05): add SQLite 5-table schema` or `fix(T18): debounce live keyword search`.
3. **Never commit secrets or local DB binaries:**
   - Verify `.env` and `*.db` / `*.sqlite` files are not staged (`git status`).
4. **Open a Pull Request into `dev`:**
   - Link the Sprint Tracker Task ID (`T01`–`T50`) in the PR description.
   - Ensure local app runs cleanly with no console/API errors before requesting review.
   - At least **1 teammate / PM review** is required before merging into `dev` or `main`.
5. **Delete merged feature branches** after the PR is merged into `dev`.

---

## Team — Spirit Systems (OCC 3)

| Member | Code | Role |
|---|---|---|
| **Sek Yin Jia** | `SY` | PM / Repo & Integration |
| **Ke Yifan** | `KY` | Data, Dictionary & QA |
| **Li Kaichen** | `LK` | UI/UX & Frontend Leaflet |
| **Lin Junxiao** | `LJ` | Backend API & SQLite DB |
