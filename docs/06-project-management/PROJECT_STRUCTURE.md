# Swasthya - Complete Project Structure Implementation Plan

## Current Status
Repository contains documentation only. Need to implement full stack application.

## Implementation Plan

### Phase 1: Backend Core (Priority: Critical)
1. **Database Layer**
   - PostgreSQL schema (users, patients, hospitals, appointments, records)
   - Migration scripts
   - Seed data with Indian context

2. **Authentication & Authorization**
   - JWT-based auth with Aadhaar integration
   - Role-based middleware (patient, hospital, superadmin)
   - Session management

3. **Core API Routes**
   - `/api/auth` - Authentication endpoints
   - `/api/dashboard` - Role-specific dashboards
   - `/api/users` - User management
   - `/api/patients` - Patient records (FHIR-compliant)

### Phase 2: AI Agent Integration (Priority: High)
1. **Forecast Agent** (Port 8001)
   - Demand prediction using time-series
   - CSV upload for training
   - Model persistence

2. **Triage Agent** (Port 8005)
   - Patient acuity assessment
   - ER queue management
   - Priority scoring

3. **Staff Scheduling Agent** (Port 8002)
   - Shift optimization
   - Constraint satisfaction
   - Schedule export

4. **ER/OR Scheduling Agent** (Port 8003)
   - Emergency room management
   - Operating room scheduling
   - Resource allocation

5. **Discharge Planning Agent** (Port 8004)
   - Readiness assessment
   - Discharge summary generation
   - Follow-up scheduling

6. **Orchestrator Agent**
   - Multi-agent coordination
   - Workflow management
   - Conflict resolution

### Phase 3: Frontend Application (Priority: High)
1. **Core Components**
   - Authentication (Login, Register)
   - Dashboard layouts (Patient, Hospital, Admin)
   - Navigation & routing

2. **Patient Portal**
   - Health records view
   - Appointment booking
   - Telemedicine interface

3. **Hospital Dashboard**
   - Multi-agent overview
   - Forecast visualization
   - Triage queue
   - Staff schedule
   - ER/OR management
   - Discharge planning

4. **Admin Console**
   - Hospital management
   - User administration
   - System analytics

### Phase 4: ABDM Integration (Priority: High)
1. **Health ID Integration**
   - Aadhaar authentication
   - Health ID creation/linking
   - PHR address management

2. **FHIR Compliance**
   - FHIR R4 resource mapping
   - Data exchange APIs
   - Consent management

3. **Registry Integration**
   - HPR (Healthcare Provider Registry)
   - HFR (Health Facility Registry)
   - UHI (Unified Health Interface)

### Phase 5: IoT & Real-time Features (Priority: Medium)
1. **IoT Device Integration**
   - Device registration
   - Real-time data streaming
   - Alert system

2. **WebSocket Implementation**
   - Real-time notifications
   - Live queue updates
   - Emergency alerts

### Phase 6: Advanced Features (Priority: Medium)
1. **Federated Learning**
   - Multi-hospital model training
   - Privacy-preserving aggregation
   - Model versioning

2. **Analytics & Reporting**
   - Disease surveillance
   - Performance metrics
   - Custom reports

3. **Telemedicine**
   - Video consultation
   - Chat interface
   - Prescription generation

### Phase 7: Testing & Quality (Priority: Critical)
1. **Automated Testing**
   - Unit tests (Jest, Mocha)
   - Integration tests
   - E2E tests (Playwright)
   - Auto-fix system

2. **Security Testing**
   - Penetration testing
   - DISHA compliance validation
   - Security audit

### Phase 8: Deployment & DevOps (Priority: High)
1. **Containerization**
   - Docker images
   - Docker Compose setup
   - Kubernetes manifests

2. **CI/CD Pipeline**
   - GitHub Actions
   - Automated testing
   - Deployment automation

3. **Monitoring**
   - Application monitoring
   - Error tracking
   - Performance monitoring

## Technology Stack Confirmation

### Frontend
- React 18 + TypeScript
- Vite (build tool)
- TailwindCSS + shadcn/ui
- React Query (server state)
- React Router (routing)
- Axios (HTTP client)
- Recharts (data visualization)

### Backend
- Node.js + Express
- PostgreSQL (primary database)
- Redis (caching)
- bcryptjs (password hashing)
- jsonwebtoken (JWT auth)
- express-validator (input validation)

### AI/ML (Adapting to existing models)
- Python FastAPI for agent services
- AWS Bedrock SDK (Nova models)
- TensorFlow/PyTorch (local models)
- Scikit-learn (traditional ML)

### Infrastructure
- Docker + Docker Compose
- Nginx (reverse proxy)
- AWS services (when deployed)

## File Structure to Create

```
swasthya/
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   │   ├── ui/              # shadcn/ui components
│   │   │   ├── layout/          # Layout components
│   │   │   ├── auth/            # Auth components
│   │   │   ├── patient/         # Patient-specific
│   │   │   ├── hospital/        # Hospital-specific
│   │   │   └── admin/           # Admin-specific
│   │   ├── contexts/
│   │   │   ├── AuthContext.tsx
│   │   │   └── ThemeContext.tsx
│   │   ├── hooks/
│   │   │   ├── useAuth.ts
│   │   │   ├── useForecast.ts
│   │   │   ├── useTriage.ts
│   │   │   └── ...
│   │   ├── lib/
│   │   │   ├── api/
│   │   │   ├── utils/
│   │   │   └── constants/
│   │   ├── pages/
│   │   │   ├── auth/
│   │   │   ├── patient/
│   │   │   ├── hospital/
│   │   │   └── admin/
│   │   ├── App.tsx
│   │   └── main.tsx
│   ├── public/
│   ├── index.html
│   ├── vite.config.ts
│   ├── tailwind.config.js
│   ├── tsconfig.json
│   ├── package.json
│   └── Dockerfile
│
├── backend/
│   ├── config/
│   │   ├── database.js
│   │   └── env.js
│   ├── database/
│   │   ├── schema.sql
│   │   ├── migrations/
│   │   └── seeds/
│   ├── middleware/
│   │   ├── auth.js
│   │   ├── rbac.js
│   │   ├── validation.js
│   │   └── errorHandler.js
│   ├── routes/
│   │   ├── auth.js
│   │   ├── dashboard.js
│   │   ├── patients.js
│   │   ├── forecast.js
│   │   ├── triage.js
│   │   ├── scheduling.js
│   │   ├── eror.js
│   │   └── discharge.js
│   ├── services/
│   │   ├── authService.js
│   │   ├── patientService.js
│   │   ├── abdmService.js
│   │   └── ...
│   ├── utils/
│   │   ├── logger.js
│   │   ├── validators.js
│   │   └── helpers.js
│   ├── tests/
│   ├── server.js
│   ├── server-mock.js
│   ├── package.json
│   └── Dockerfile
│
├── agents/
│   ├── orchestrator/
│   │   ├── app.py
│   │   ├── requirements.txt
│   │   └── Dockerfile
│   ├── forecast/
│   │   ├── app.py
│   │   ├── model.py
│   │   ├── requirements.txt
│   │   └── Dockerfile
│   ├── triage/
│   │   ├── app.py
│   │   ├── requirements.txt
│   │   └── Dockerfile
│   ├── staff-scheduling/
│   │   ├── app.py
│   │   ├── requirements.txt
│   │   └── Dockerfile
│   ├── eror-scheduling/
│   │   ├── app.py
│   │   ├── requirements.txt
│   │   └── Dockerfile
│   ├── discharge/
│   │   ├── app.py
│   │   ├── requirements.txt
│   │   └── Dockerfile
│   └── federated-learning/
│       ├── server1/
│       ├── server2/
│       └── aggregator/
│
├── tests/
│   ├── test-runner.js
│   ├── auto-fix-engine.js
│   ├── integration-test-suite.js
│   └── ...
│
├── scripts/
│   ├── setup.sh
│   ├── init-db.sh
│   └── generate-data.py
│
├── docker-compose.yml
├── docker-compose.override.yml
├── .env.example
├── .gitignore
└── package.json
```

## Next Steps

1. **Immediate**: Create backend core with database schema
2. **Day 1**: Implement authentication and basic API routes
3. **Day 2**: Create frontend shell with routing
4. **Day 3**: Implement AI agent stubs
5. **Week 1**: Complete core features
6. **Week 2**: ABDM integration
7. **Week 3**: Testing and refinement
8. **Week 4**: Deployment preparation

## Notes

- All AI models will use AWS Bedrock (Nova) as primary, with local fallbacks
- FHIR compliance will be built into data models from start
- DISHA compliance will be enforced at every layer
- Multilingual support (22 Indian languages) will use i18n from start
- Cost optimization will be considered in every architectural decision

---

**Status**: Ready to begin implementation  
**Priority**: Backend Core → Frontend Shell → AI Agents → Integration  
**Timeline**: 4 weeks to MVP
