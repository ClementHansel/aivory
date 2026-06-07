# Aivory — AI-Powered Business Transformation Platform

Aivory helps organizations assess AI readiness, design implementation blueprints, and deploy AI systems with confidence. From diagnostic to deployment — everything in one platform.

## Platform Architecture

Aivory is built as a microservices architecture. Each service is independently deployable with its own Docker container, allowing isolated maintenance and scaling.

```
┌─────────────────────────────────────────────────────────────────────┐
│                        REVERSE PROXY (Traefik/Nginx)                 │
└──────────┬───────────────────────────────────────────┬──────────────┘
           │                                           │
┌──────────▼──────────┐                    ┌───────────▼──────────────┐
│   Frontend Layer    │                    │     Backend Layer         │
│                     │                    │                           │
│ • avry-website:9000 │                    │ • avry-backend:8081       │
│ • avry-user-dash:9001│                   │ • avry-diagnostics:8082   │
│ • avry-admin-dash:9002│                  │ • avry-blueprint:8083     │
└─────────────────────┘                    │ • avry-roadmaps:8084      │
                                           │ • avry-payments:8085      │
                                           │ • avry-console:8086       │
                                           │ • avry-workflows:8087     │
                                           │ • avry-blog:8088          │
                                           │ • avry-careers:8089       │
                                           │ • avry-zeroclaw:8090      │
                                           └───────────────────────────┘
                                                       │
                                           ┌───────────▼──────────────┐
                                           │   Infrastructure Layer    │
                                           │                           │
                                           │ • Supabase (Auth + DB)    │
                                           │ • avry-n8n (Automation)   │
                                           │ • avry-vps (Orchestration)│
                                           │ • avry-vps-monitoring     │
                                           └───────────────────────────┘
```

## Repositories

### Frontend

| Repository | Description | Port |
|-----------|-------------|------|
| [avry-website](https://github.com/ClementHansel/avry-website) | Marketing website & homepage (Next.js 16) | 9000 |
| [avry-user-dashboard](https://github.com/ClementHansel/avry-user-dashboard) | User dashboard — diagnostic, blueprint, workflows | 9001 |
| [avry-admin-dashboards](https://github.com/ClementHansel/avry-admin-dashboards) | Admin panel — user management, analytics | 9002 |

### Backend Services

| Repository | Description | Port |
|-----------|-------------|------|
| [avry-backend](https://github.com/ClementHansel/avry-backend) | Core API — auth, user management, orchestration (FastAPI) | 8081 |
| [avry-diagnostics](https://github.com/ClementHansel/avry-diagnostics) | AI Readiness Diagnostic engine | 8082 |
| [avry-blueprint](https://github.com/ClementHansel/avry-blueprint) | AI System Blueprint generator | 8083 |
| [avry-roadmaps](https://github.com/ClementHansel/avry-roadmaps) | Implementation Roadmap service | 8084 |
| [avry-payments](https://github.com/ClementHansel/avry-payments) | Payment processing (Midtrans integration) | 8085 |
| [avry-console](https://github.com/ClementHansel/avry-console) | AI Console — conversational AI assistant | 8086 |
| [avry-workflows](https://github.com/ClementHansel/avry-workflows) | Workflow Builder engine | 8087 |
| [avry-blog](https://github.com/ClementHansel/avry-blog) | Blog CMS service | 8088 |
| [avry-careers](https://github.com/ClementHansel/avry-careers) | Careers/job listings service | 8089 |
| [avry-zeroclaw](https://github.com/ClementHansel/avry-zeroclaw) | AI Agent deployment engine | 8090 |

### Infrastructure

| Repository | Description |
|-----------|-------------|
| [avry-vps](https://github.com/ClementHansel/avry-vps) | VPS orchestration, Nginx config, deployment scripts |
| [avry-vps-monitoring](https://github.com/ClementHansel/avry-vps-monitoring) | Health monitoring, metrics, alerting |
| [avry-n8n](https://github.com/ClementHansel/avry-n8n) | n8n workflow automation (self-hosted) |
| [avry-shared-libs](https://github.com/ClementHansel/avry-shared-libs) | Shared utilities, types, and SDK packages |

## Quick Start (Full Stack Local Dev)

### Prerequisites

- Docker & Docker Compose
- Node.js 18+ (for frontend development)
- Python 3.11+ (for backend services)
- Supabase account (or local Supabase instance)

### 1. Clone All Repos

```bash
mkdir aivory && cd aivory

# Frontend
git clone https://github.com/ClementHansel/avry-website.git
git clone https://github.com/ClementHansel/avry-user-dashboard.git
git clone https://github.com/ClementHansel/avry-admin-dashboards.git

# Backend
git clone https://github.com/ClementHansel/avry-backend.git
git clone https://github.com/ClementHansel/avry-diagnostics.git
git clone https://github.com/ClementHansel/avry-blueprint.git
git clone https://github.com/ClementHansel/avry-roadmaps.git
git clone https://github.com/ClementHansel/avry-payments.git
git clone https://github.com/ClementHansel/avry-console.git
git clone https://github.com/ClementHansel/avry-workflows.git
git clone https://github.com/ClementHansel/avry-blog.git
git clone https://github.com/ClementHansel/avry-careers.git
git clone https://github.com/ClementHansel/avry-zeroclaw.git

# Infrastructure
git clone https://github.com/ClementHansel/avry-vps.git
git clone https://github.com/ClementHansel/avry-vps-monitoring.git
git clone https://github.com/ClementHansel/avry-n8n.git
git clone https://github.com/ClementHansel/avry-shared-libs.git
```

### 2. Configure Environment

Each service has its own `.env.example`. Copy to `.env` (or `.env.local` for Next.js apps) and fill in your values.

Key shared variables:
```env
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_SERVICE_KEY=your-service-key
SUPABASE_ANON_KEY=your-anon-key
MIDTRANS_SERVER_KEY=your-midtrans-server-key
MIDTRANS_CLIENT_KEY=your-midtrans-client-key
OPENAI_API_KEY=your-openai-key
```

### 3. Run with Docker Compose

Each service has its own `docker-compose.yml`. To run everything:

```bash
# Start core backend
cd avry-backend && docker compose up -d && cd ..

# Start microservices
for svc in avry-diagnostics avry-blueprint avry-roadmaps avry-payments avry-console avry-workflows avry-blog avry-careers; do
  cd $svc && docker compose up -d && cd ..
done

# Start frontends
cd avry-website && docker compose up -d && cd ..
cd avry-user-dashboard && docker compose up -d && cd ..
cd avry-admin-dashboards && docker compose up -d && cd ..
```

### 4. Access

| Service | URL |
|---------|-----|
| Website | http://localhost:9000 |
| User Dashboard | http://localhost:9001 |
| Admin Dashboard | http://localhost:9002 |
| Backend API | http://localhost:8081 |
| API Docs (Swagger) | http://localhost:8081/docs |

## VPS Deployment

Each service is deployed as an independent Docker container on the VPS. See [avry-vps](https://github.com/ClementHansel/avry-vps) for:
- Nginx reverse proxy configuration
- SSL/TLS (Let's Encrypt)
- Docker network setup
- Deployment scripts

## Technology Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Next.js 16, React 19, Tailwind CSS v4, TypeScript |
| Backend | FastAPI (Python 3.11+), SQLAlchemy |
| Database | Supabase (PostgreSQL) |
| Auth | Supabase Auth (JWT) |
| Payments | Midtrans Snap SDK |
| AI/LLM | OpenAI GPT-4, Claude |
| Automation | n8n (self-hosted) |
| Deployment | Docker, Traefik/Nginx |
| Monitoring | Custom health checks, metrics |

## Domain Architecture

| Domain | Service |
|--------|---------|
| aivory.id | avry-website (marketing) |
| dashboard.aivory.id | avry-user-dashboard |
| admin.aivory.id | avry-admin-dashboards |
| api.aivory.id | avry-backend + microservices |

## Contributing

1. Each repo has its own development workflow
2. Branch from `main`, PR back to `main`
3. All services must pass their own tests before merge
4. Docker builds must succeed (`docker build .`)

## License

Proprietary — Aivory © 2026. All rights reserved.
