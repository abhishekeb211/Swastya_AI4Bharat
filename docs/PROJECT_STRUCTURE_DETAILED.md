# Swasthya - Detailed Project Structure

```
swasthya/
├── .github/
│   ├── workflows/
│   │   ├── backend-ci.yml
│   │   ├── frontend-ci.yml
│   │   ├── mobile-ci.yml
│   │   └── deploy.yml
│   ├── ISSUE_TEMPLATE/
│   └── PULL_REQUEST_TEMPLATE.md
│
├── docs/
│   ├── architecture/
│   │   ├── system-architecture.md
│   │   ├── data-flow.md
│   │   └── security-architecture.md
│   ├── api/
│   │   ├── openapi.yaml
│   │   └── postman-collection.json
│   ├── guides/
│   │   ├── developer-guide.md
│   │   ├── user-guide.md
│   │   └── admin-guide.md
│   ├── runbooks/
│   │   ├── incident-response.md
│   │   ├── deployment.md
│   │   └── troubleshooting.md
│   └── open-source/
│       ├── triage-agent.md
│       ├── discharge-planning.md
│       ├── staff-scheduling.md
│       ├── demand-forecasting.md
│       └── clinical-decision-support.md
│
├── infrastructure/
│   ├── terraform/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── outputs.tf
│   │   ├── modules/
│   │   │   ├── vpc/
│   │   │   ├── ecs/
│   │   │   ├── rds/
│   │   │   ├── s3/
│   │   │   └── iot/
│   │   └── environments/
│   │       ├── dev/
│   │       ├── staging/
│   │       └── prod/
│   ├── docker/
│   │   ├── Dockerfile.user-service
│   │   ├── Dockerfile.health-record-service
│   │   ├── Dockerfile.ai-agents
│   │   └── docker-compose.yml
│   └── kubernetes/ (optional)
│
├── services/
│   ├── user-service/
│   │   ├── src/
│   │   │   ├── controllers/
│   │   │   ├── models/
│   │   │   ├── routes/
│   │   │   ├── middleware/
│   │   │   ├── utils/
│   │   │   └── index.js
│   │   ├── tests/
│   │   ├── package.json
│   │   ├── Dockerfile
│   │   └── README.md
│   │
│   ├── health-record-service/
│   │   ├── src/
│   │   │   ├── api/
│   │   │   ├── models/
│   │   │   ├── services/
│   │   │   ├── utils/
│   │   │   └── main.py
│   │   ├── tests/
│   │   ├── requirements.txt
│   │   ├── Dockerfile
│   │   └── README.md
│   │
│   ├── abdm-gateway/
│   │   ├── src/
│   │   ├── tests/
│   │   ├── package.json
│   │   └── README.md
│   │
│   ├── ai-agents/
│   │   ├── agents/
│   │   │   ├── triage/
│   │   │   │   ├── rule_based_triage.py
│   │   │   │   ├── ml_fallback.py
│   │   │   │   └── config.yaml
│   │   │   ├── discharge/
│   │   │   │   ├── template_engine.py
│   │   │   │   ├── svg_generator.py
│   │   │   │   └── templates/
│   │   │   ├── scheduling/
│   │   │   │   ├── or_tools_scheduler.py
│   │   │   │   └── constraints.py
│   │   │   ├── forecasting/
│   │   │   │   ├── prophet_forecaster.py
│   │   │   │   └── anomaly_detector.py
│   │   │   └── clinical_decision/
│   │   │       ├── rule_based_cds.py
│   │   │       ├── knowledge_base/
│   │   │       └── drug_interactions.py
│   │   ├── tests/
│   │   ├── requirements.txt
│   │   └── README.md
│   │
│   ├── iot-service/
│   │   ├── src/
│   │   ├── edge/
│   │   │   ├── gateway.py
│   │   │   ├── anomaly_detector.py
│   │   │   └── message_aggregator.py
│   │   ├── tests/
│   │   └── README.md
│   │
│   └── analytics-service/
│       ├── src/
│       ├── tests/
│       └── README.md
│
├── frontend/
│   ├── web-dashboard/
│   │   ├── src/
│   │   │   ├── components/
│   │   │   ├── pages/
│   │   │   ├── services/
│   │   │   ├── store/
│   │   │   ├── utils/
│   │   │   ├── App.tsx
│   │   │   └── index.tsx
│   │   ├── public/
│   │   ├── tests/
│   │   ├── package.json
│   │   ├── tsconfig.json
│   │   └── README.md
│   │
│   └── admin-console/
│       ├── src/
│       ├── tests/
│       ├── package.json
│       └── README.md
│
├── mobile/
│   ├── patient-app/
│   │   ├── src/
│   │   │   ├── screens/
│   │   │   ├── components/
│   │   │   ├── navigation/
│   │   │   ├── services/
│   │   │   ├── store/
│   │   │   ├── utils/
│   │   │   └── App.tsx
│   │   ├── android/
│   │   ├── ios/
│   │   ├── tests/
│   │   ├── package.json
│   │   └── README.md
│   │
│   └── provider-app/ (future)
│
├── shared/
│   ├── types/
│   │   ├── user.ts
│   │   ├── health-record.ts
│   │   └── fhir.ts
│   ├── constants/
│   ├── utils/
│   └── validators/
│
├── scripts/
│   ├── build-and-push.sh
│   ├── deploy.sh
│   ├── migrate.sh
│   ├── seed-data.sh
│   └── backup.sh
│
├── tests/
│   ├── integration/
│   ├── e2e/
│   ├── load/
│   └── security/
│
├── .kiro/
│   ├── specs/
│   │   └── swasthya-enhancement-suite/
│   │       ├── requirements.md
│   │       ├── design.md
│   │       └── tasks.md
│   └── steering/
│
├── .env.example
├── .gitignore
├── README.md
├── REQUIREMENTS.md
├── DESIGN.md
├── ARCHITECTURE.md
├── IMPLEMENTATION_PLAN.md
├── TODO.md
├── PILOT_DEPLOYMENT_PLAN.md
├── OPEN_SOURCE_IMPLEMENTATION_GUIDE.md
├── API_REFERENCE.md
├── DEPLOYMENT_GUIDE.md
├── COST_OPTIMIZATION_REPORT.md
├── AI4BHARAT_EVALUATION.md
├── FINAL_EVALUATION_SUMMARY.md
├── LICENSE
└── CONTRIBUTING.md
```

## Key Directories Explained

### `/services`
Microservices backend - each service is independently deployable

### `/frontend`
Web applications built with React + TypeScript

### `/mobile`
React Native mobile applications

### `/infrastructure`
Infrastructure as Code (Terraform) and Docker configurations

### `/docs`
Comprehensive documentation including architecture, APIs, and guides

### `/scripts`
Automation scripts for deployment, migration, and maintenance

### `/tests`
Integration, E2E, load, and security tests

### `/.kiro`
Kiro.ai spec-driven development files

---

**End of Project Structure**
