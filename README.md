<div align="center">

#  LOGIVISION

**Intelligent Warehouse Monitoring System**

*Real-time box tracking · Computer Vision · MLOps · 100% On-Premise*

[![Python](https://img.shields.io/badge/python-3.11-blue.svg)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.110+-009688.svg)](https://fastapi.tiangolo.com/)
[![React](https://img.shields.io/badge/React-18-61dafb.svg)](https://react.dev/)
[![YOLOv8](https://img.shields.io/badge/YOLOv8-Ultralytics-yellow.svg)](https://github.com/ultralytics/ultralytics)
[![Docker](https://img.shields.io/badge/Docker-Compose-2496ED.svg)](https://docs.docker.com/compose/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Status](https://img.shields.io/badge/status-MVP%20in%20progress-orange.svg)]()

</div>

---

##  Table des Matières

- [À propos](#-à-propos)
- [Fonctionnalités](#-fonctionnalités)
- [Architecture](#-architecture)
- [Stack technique](#-stack-technique)
- [Démarrage rapide](#-démarrage-rapide)
- [Structure du projet](#-structure-du-projet)
- [Workflow MLOps](#-workflow-mlops)
- [Développement](#-développement)
- [Tests](#-tests)
- [Déploiement](#-déploiement)
- [Documentation](#-documentation)
- [Équipe](#-équipe)
- [Licence](#-licence)

---

##  À propos

**LOGIVISION** est un système de surveillance intelligent pour entrepôts logistiques et usines, basé sur la Computer Vision. Il automatise le suivi des boîtes et articles, détecte les anomalies en temps réel, et fournit un dashboard centralisé aux gestionnaires.

### Contexte

Projet académique réalisé par une équipe de 5 étudiants ingénieurs, déployable on-premise sur du matériel CPU standard, sans dépendance cloud. Adapté aux entreprises de logistique et manufacturing au Maroc et à l'international.

### Objectifs

- Automatiser le comptage et le tracking des boîtes/palettes
-  Détecter les anomalies (zones vides, mauvais produits, mouvements suspects)
-  Offrir un dashboard temps réel (latence < 5s)
-  Garantir la souveraineté des données (100% on-premise)
-  Démontrer un pipeline MLOps complet (training → deploy → monitoring)

---

## ✨ Fonctionnalités

| Module | Description | Priorité |
|---|---|---|
|  **Tracking Boxes** | Détection YOLOv8 + tracking ByteTrack + lecture QR/barcodes | P1 |
|  **Entrées/Sorties** | Stats journalières, comparatifs, exports CSV/PDF | P2 |
|  **Gestion Zones** | Vue par zone/étagère, heatmap d'occupation, capacités | P3 |
|  **Anomalies** | 7 types d'alertes (critique/haute/moyenne/basse) | P4 |
| **Catégories** | Répartition produits, tendances multi-mois | P5 |
|  **Rapports** | Génération auto quotidienne/hebdo/mensuelle | P6 |
|  **Administration** | Gestion utilisateurs, caméras, notifications | P7 |

---

##  Architecture

Architecture en **5 couches** avec pattern **Kappa** (stream processing unique).

```
┌─────────────────────────────────────────────────────────────┐
│ 01 CAPTURE          │ 2 caméras smartphone (RTSP) +         │
│                     │ MediaMTX (replay vidéo pour démo)     │
├─────────────────────────────────────────────────────────────┤
│ 02 TRAITEMENT VIDÉO │ OpenCV → YOLOv8n (OpenVINO) → pyzbar  │
│                     │ → ByteTrack                           │
├─────────────────────────────────────────────────────────────┤
│ 03 BACKEND & API    │ FastAPI REST + WebSocket              │
│                     │ Redis Streams + PostgreSQL + MLflow   │
├─────────────────────────────────────────────────────────────┤
│ 04 FRONTEND         │ React + TypeScript + TailwindCSS      │
│                     │ Recharts + WebSocket client           │
├─────────────────────────────────────────────────────────────┤
│ 05 MONITORING/MLOPS │ Prometheus + Grafana + Evidently AI   │
│                     │ MLflow tracking + GitHub Actions      │
└─────────────────────────────────────────────────────────────┘
```

> 📖 Architecture complète : voir [docs/architecture/](docs/architecture/) et [cahier de charges](docs/cahier_de_charges.pdf)

---

##  Stack technique

### Computer Vision & ML
- **[YOLOv8n](https://github.com/ultralytics/ultralytics)** — Détection d'objets (nano, CPU-optimized)
- **[OpenVINO](https://docs.openvino.ai/)** — Accélération inférence Intel CPU (×2-3)
- **[ByteTrack](https://github.com/ifzhang/ByteTrack)** — Tracking multi-objets
- **[pyzbar](https://github.com/NaturalHistoryMuseum/pyzbar)** — Lecture barcodes/QR
- **[Roboflow](https://roboflow.com/)** — Annotation & augmentation datasets

### Backend
- **[FastAPI](https://fastapi.tiangolo.com/)** 0.110+ — API REST async
- **[PostgreSQL](https://www.postgresql.org/)** 16 — Base de données
- **[Redis](https://redis.io/)** 7 — Streams (message queue) + cache
- **[Celery](https://docs.celeryq.dev/)** — Workers asynchrones
- **[SQLAlchemy](https://www.sqlalchemy.org/) + [Alembic](https://alembic.sqlalchemy.org/)** — ORM & migrations

### Frontend
- **[React](https://react.dev/)** 18 + **[TypeScript](https://www.typescriptlang.org/)** 5
- **[Vite](https://vitejs.dev/)** — Build tool
- **[TailwindCSS](https://tailwindcss.com/)** 3 — Styling
- **[Recharts](https://recharts.org/)** — Graphiques
- **[TanStack Query](https://tanstack.com/query)** — Gestion état serveur

### MLOps & Infrastructure
- **[MLflow](https://mlflow.org/)** — Experiment tracking + Model Registry
- **[DVC](https://dvc.org/)** — Dataset versioning
- **[MinIO](https://min.io/)** — Stockage artefacts S3-compatible
- **[Evidently AI](https://www.evidentlyai.com/)** — Data/Model drift detection
- **[Prometheus](https://prometheus.io/)** + **[Grafana](https://grafana.com/)** — Monitoring
- **[Docker Compose](https://docs.docker.com/compose/)** — Orchestration
- **[Nginx](https://nginx.org/)** — Reverse proxy + HTTPS
- **[MediaMTX](https://github.com/bluenviron/mediamtx)** — Serveur RTSP (démo)

### DevOps
- **GitHub Actions** — CI/CD
- **Jira** — Gestion projet (Scrum)
- **[pytest](https://pytest.org/)** + **[Vitest](https://vitest.dev/)** — Tests

---

##  Démarrage rapide

### Prérequis

- **Docker** 24+ et **Docker Compose** v2
- **Python** 3.11+ (pour dev local)
- **Node.js** 20+ (pour dev local)
- **Git** 2.40+
- **Smartphone** avec app [DroidCam](https://www.dev47apps.com/) ou [iVCam](https://www.e2esoft.com/ivcam/)

### Installation

```bash
# 1. Cloner le repo
git clone https://github.com/VOTRE_ORG/logivision.git
cd logivision

# 2. Copier et configurer les variables d'environnement
cp .env.example .env
#  Éditer .env avec vos vraies valeurs (IPs caméras, mots de passe...)

# 3. Démarrer toute la stack
docker-compose up -d

# 4. Attendre ~30s que tous les services démarrent, puis vérifier
docker-compose ps

# 5. Appliquer les migrations de base de données
docker-compose exec api alembic upgrade head

# 6. (Optionnel) Seed de données de test
docker-compose exec api python scripts/seed_db.py
```

### Accès aux services

| Service | URL | Credentials par défaut |
|---|---|---|
| 🖥️ **Dashboard** | http://localhost:3000 | admin@logivision.local / admin |
| 🔌 **API docs (Swagger)** | http://localhost:8000/docs | — |
| 📊 **Grafana** | http://localhost:3001 | admin / admin |
| 🔬 **MLflow** | http://localhost:5000 | — |
| 📦 **MinIO Console** | http://localhost:9001 | minioadmin / minioadmin |
| 🎥 **MediaMTX** | rtsp://localhost:8554 | — |

### Démarrer le pipeline vidéo

```bash
# Option A : caméras smartphone
# 1. Lancer DroidCam/iVCam sur 2 smartphones
# 2. Récupérer les IPs RTSP affichées dans les apps
# 3. Mettre à jour .env avec CAMERA_1_RTSP et CAMERA_2_RTSP
# 4. Redémarrer le video-engine
docker-compose restart video-engine

# Option B : MediaMTX avec vidéos pré-enregistrées (plan B démo)
# 1. Déposer demo_video_1.mp4 et demo_video_2.mp4 dans infra/mediamtx/
# 2. Démarrer MediaMTX
docker-compose up -d mediamtx
# 3. Dans .env :
#    CAMERA_1_RTSP=rtsp://mediamtx:8554/camera1
#    CAMERA_2_RTSP=rtsp://mediamtx:8554/camera2
docker-compose restart video-engine
```

---

## 📁 Structure du projet

```
logivision/
├── video-engine/        # Pipeline Computer Vision (YOLOv8 + OpenVINO)
├── api/                 # Backend FastAPI + WebSocket
├── frontend/            # Dashboard React + TypeScript
├── ml/                  # Notebooks training + scripts
├── datasets/            # Datasets versionnés avec DVC
├── infra/               # Docker, Nginx, Prometheus, Grafana, MediaMTX
├── docs/                # Documentation technique + cahier de charges
├── scripts/             # Scripts utilitaires
├── tests/               # Tests intégration & E2E
├── docker-compose.yml   # Stack principale
├── CLAUDE.md            # Instructions pour Claude Code
└── README.md            # Ce fichier
```

> Détail complet de l'architecture : [docs/architecture/structure.md](docs/architecture/structure.md)

---

##  Workflow MLOps

Pipeline en **4 phases** (Data → Training → Deployment → Monitoring) avec boucle de rétroaction.

### Training hybride : Colab Pro + CPU local

| Étape | Environnement | Outils |
|---|---|---|
| **Annotation** | Cloud (Roboflow Free) | Roboflow |
| **Training initial** | Google Colab Pro (GPU T4) | Ultralytics YOLOv8, MLflow |
| **Inférence production** | CPU local | OpenVINO IR |
| **Re-training incrémental** | CPU local | Ultralytics + dataset enrichi |
| **Versioning** | Local | DVC + MLflow + MinIO |

### Lancer un training

```bash
# Ouvrir le notebook dans Colab Pro
# ml/notebooks/02_training_yolov8.ipynb

# Ou en local (si GPU disponible)
cd ml
python scripts/train.py --config configs/yolov8_config.yaml
```

### Déployer un nouveau modèle

```bash
# 1. Exporter depuis MLflow vers OpenVINO
python ml/scripts/export_openvino.py --run-id <mlflow_run_id>

# 2. Placer dans video-engine/models/
cp best_openvino_model/* video-engine/models/

# 3. Redémarrer le video-engine (Blue-Green)
docker-compose up -d --no-deps --build video-engine
```

> 📖 Guide complet MLOps : [docs/mlops/](docs/mlops/)

---

##  Développement

### Mode développement (hot reload)

```bash
# Lance avec les volumes montés + hot reload
docker-compose -f docker-compose.yml -f docker-compose.dev.yml up
```

### Développement local (sans Docker)

```bash
# Backend API
cd api
python -m venv venv
source venv/bin/activate  # ou venv\Scripts\activate sur Windows
pip install -r requirements.txt
uvicorn app.main:app --reload

# Frontend
cd frontend
npm install
npm run dev

# Video Engine
cd video-engine
pip install -r requirements.txt
python -m app.main
```

### Commandes Makefile

```bash
make help               # Liste toutes les commandes
make up                 # docker-compose up -d
make down               # docker-compose down
make logs               # Suit les logs
make test               # Lance tous les tests
make lint               # Vérifie le code (ruff + eslint)
make format             # Formate le code (black + prettier)
make shell-api          # Shell dans le container API
make shell-db           # psql dans la DB
make migrate            # Applique migrations Alembic
make reset-db           #  Reset complet de la DB
```

### Conventions de code

- **Python** : [Black](https://black.readthedocs.io/) + [Ruff](https://docs.astral.sh/ruff/) — line length 100
- **TypeScript** : [Prettier](https://prettier.io/) + [ESLint](https://eslint.org/) — strict mode
- **Commits** : [Conventional Commits](https://www.conventionalcommits.org/)
  - `feat:` nouvelle fonctionnalité
  - `fix:` correction de bug
  - `docs:` documentation
  - `refactor:` refactoring sans changement fonctionnel
  - `test:` ajout/modif de tests
  - `chore:` tâches diverses (deps, config...)
- **Branches** : `feature/TICKET-ID-description` (ex: `feature/LOG-42-add-zone-heatmap`)

### Workflow Git

```bash
# Créer une branche depuis develop
git checkout develop
git pull
git checkout -b feature/LOG-12-camera-capture

# Commits fréquents
git add .
git commit -m "feat(video-engine): add RTSP capture module"

# Push et ouvrir une PR
git push -u origin feature/LOG-12-camera-capture
# Puis ouvrir la PR sur GitHub vers develop
```

---

## 🧪 Tests

```bash
# Tous les tests
make test

# Backend uniquement
cd api && pytest tests/ -v --cov=app

# Frontend uniquement
cd frontend && npm test

# Tests E2E
cd tests/e2e && npm run test:e2e
```

### Critères de qualité

-  Couverture de code > 70% (backend)
-  Linting passant (ruff + eslint)
-  Type checking (mypy + tsc)
-  Tests E2E critiques (login, détection, anomalie)

---

##  Déploiement

### MVP (environnement local école)

```bash
docker-compose -f docker-compose.yml -f docker-compose.prod.yml up -d
```

### Variables à durcir en production

- `API_SECRET_KEY` : générer avec `openssl rand -hex 32`
- `POSTGRES_PASSWORD` : mot de passe fort, 20+ caractères
- `REDIS_PASSWORD` : activer l'auth Redis
- **HTTPS** : générer un certificat Let's Encrypt ou auto-signé
- **Firewall** : ouvrir uniquement 443/80

### CI/CD GitHub Actions

Les workflows sont dans [`.github/workflows/`](.github/workflows/) :

- `ci.yml` — lint + tests à chaque PR
- `docker-build.yml` — build des images Docker
- `deploy.yml` — déploiement (manuel trigger pour MVP)

---

## 📊 Performance cible

| Métrique | Cible MVP | Critique |
|---|---|---|
| Latence détection (caméra → dashboard) | < 5 s | < 10 s |
| Précision détection (mAP50) | > 92 % | > 85 % |
| Précision lecture codes | > 98 % | > 95 % |
| Inférence CPU par frame (OpenVINO) | < 300 ms | < 500 ms |
| Uptime | > 99 % | > 97 % |
| Temps chargement dashboard | < 2 s | < 4 s |

---

##  Documentation

-  [Cahier de charges complet](docs/cahier_de_charges.pdf)
- [Architecture détaillée](docs/architecture/)
-  [Documentation API (Swagger)](http://localhost:8000/docs)
-  [Guide MLOps](docs/mlops/)
-  [Guide contributeurs](CONTRIBUTING.md)
-  [Changelog](CHANGELOG.md)

---



## 📄 Licence

Ce projet est sous licence **MIT** — voir [LICENSE](LICENSE) pour plus de détails.

**Note importante** : YOLOv8 (Ultralytics) est sous licence AGPL-3.0. Pour un usage commercial, une licence commerciale Ultralytics est requise. Ce projet est réalisé dans un cadre académique non-commercial.

---

##  Remerciements

- [Ultralytics](https://github.com/ultralytics/ultralytics) pour YOLOv8
- [Roboflow](https://roboflow.com/) pour l'annotation gratuite
- [Intel OpenVINO](https://docs.openvino.ai/) pour l'accélération CPU
- L'écosystème open source en général

---

<div align="center">

**LOGIVISION** · 2026 · Projet Académique

[Report Bug](https://github.com/VOTRE_ORG/logivision/issues) · [Request Feature](https://github.com/VOTRE_ORG/logivision/issues) · [Cahier de Charges](docs/cahier_de_charges.pdf)

</div>