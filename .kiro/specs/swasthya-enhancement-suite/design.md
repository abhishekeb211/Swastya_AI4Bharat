# Design Document: Swasthya Enhancement Suite

## Overview

This design document describes the architecture and implementation approach for five major enhancements to the Swasthya healthcare intelligence network. The enhancements expand capabilities while maintaining cost efficiency (₹23/patient/month), ABDM compliance, and open-source principles.

### Design Goals

- Maintain existing ₹23/patient/month cost target (54% savings from original ₹50 target)
- Preserve 99.9% uptime and <200ms response time for critical operations
- Support 22 Indian languages across all new features
- Enable offline functionality for rural areas with poor connectivity
- Ensure medical accuracy with 90%+ expert validation concordance
- Detect and eliminate algorithmic bias
- Provide explainable AI for all medical decisions
- Maintain ABDM, DISHA, and FHIR R4 compliance

### Enhancement Areas

1. **Open-Source SDK & Community**: Publish medical algorithms, documentation, and community platform
2. **Cost Transparency Dashboard**: Real-time cost tracking and optimization recommendations
3. **Rural Accessibility**: Offline PWA, SMS alerts, voice interface, USSD integration
4. **Validation & Quality**: Pilot deployment framework, expert validation, outcome tracking
5. **AI Ethics & Governance**: Bias detection, explainable AI, human-in-the-loop, audit framework

### Implementation Phases

The implementation is organized into four phases to enable incremental delivery and validation:

**Phase 1: Foundation & SDK (Months 1-2)**
- Open-source SDK publication with core algorithms
- Developer community platform setup
- Basic cost tracking infrastructure
- Requirements: 1, 2, 20, 23

**Phase 2: Cost Transparency & Validation (Months 2-3)**
- Cost transparency dashboard with real-time tracking
- Pilot deployment framework
- Medical expert validation workflow
- Algorithm accuracy comparison
- Requirements: 3, 5, 6, 7, 21, 22, 24

**Phase 3: Rural Accessibility (Months 3-4)**
- Offline-first PWA with sync capabilities
- SMS alert system
- Multi-language voice interface
- USSD integration for feature phones
- Low-bandwidth API optimization
- Requirements: 4, 15, 16, 17, 18, 19

**Phase 4: AI Ethics & Governance (Months 4-5)**
- Bias detection system
- Explainable AI engine
- Human-in-the-loop workflows
- Audit framework and ethics committee integration
- Patient outcome tracking
- Continuous quality monitoring
- Requirements: 8, 9, 10, 11, 12, 13, 14, 25

## Architecture

### High-Level Architecture

The enhancements integrate with existing Swasthya architecture while adding new components:

```
┌─────────────────────────────────────────────────────────────────┐
│                    New Enhancement Layer                         │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐         │
│  │ SDK Package  │  │ Cost         │  │ Offline PWA  │         │
│  │ Registry     │  │ Dashboard    │  │ Service      │         │
│  └──────────────┘  └──────────────┘  └──────────────┘         │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐         │
│  │ Validation   │  │ Bias         │  │ XAI          │         │
│  │ Platform     │  │ Detector     │  │ Engine       │         │
│  └──────────────┘  └──────────────┘  └──────────────┘         │
└────────────────────────────┬────────────────────────────────────┘
                             │
┌────────────────────────────▼────────────────────────────────────┐
│              Existing Swasthya Core System                       │
│  (User Service, Health Records, AI Agents, IoT, ABDM)          │
└─────────────────────────────────────────────────────────────────┘
```

### Technology Stack Additions

- **SDK**: Python (pip), JavaScript (npm), Java (Maven)
- **Cost Dashboard**: React + D3.js for visualization, AWS Cost Explorer API
- **Offline PWA**: Service Workers, IndexedDB, Background Sync API
- **Voice Interface**: AWS Polly (TTS), AWS Transcribe (STT), Bhashini API
- **SMS/USSD**: AWS SNS, Twilio, telecom operator APIs
- **Bias Detection**: Fairlearn, AIF360, custom metrics
- **XAI**: SHAP, LIME, attention visualization
- **Validation Platform**: Custom React dashboard with PostgreSQL backend



## Components and Interfaces

### 1. Open-Source SDK

**Purpose**: Provide developers with reusable medical algorithms and tools

**Package Structure**:
```
swasthya-sdk/
├── core/
│   ├── triage/          # Rule-based triage algorithms
│   ├── scheduling/      # OR-Tools optimization
│   ├── forecasting/     # Prophet time series
│   └── nlp/             # BioBERT integration
├── utils/
│   ├── fhir/            # FHIR R4 helpers
│   ├── validation/      # Data validators
│   └── generators/      # Synthetic data
├── examples/
│   ├── python/
│   ├── javascript/
│   └── java/
├── docs/
│   ├── api/
│   ├── tutorials/
│   └── workshops/
└── tests/
```

**Key Interfaces**:

```python
# Python SDK Interface
from swasthya_sdk import TriageEngine, Scheduler, Forecaster

# Triage
triage = TriageEngine()
result = triage.assess(
    symptoms=["fever", "cough"],
    vital_signs={"temperature": 101.5, "heart_rate": 95},
    history=["diabetes"]
)
# Returns: {"priority": "URGENT", "confidence": 0.92, "reasoning": "..."}

# Scheduling
scheduler = Scheduler()
schedule = scheduler.optimize(
    staff=[...],
    constraints={...},
    preferences={...}
)

# Forecasting
forecaster = Forecaster(model="prophet")
forecast = forecaster.predict(
    historical_data=[...],
    periods=30
)
```

```javascript
// JavaScript SDK Interface
import { TriageEngine, Scheduler, Forecaster } from '@swasthya/sdk';

const triage = new TriageEngine();
const result = await triage.assess({
  symptoms: ['fever', 'cough'],
  vitalSigns: { temperature: 101.5, heartRate: 95 },
  history: ['diabetes']
});
```

**Distribution**:
- Python: PyPI (pip install swasthya-sdk)
- JavaScript: npm (npm install @swasthya/sdk)
- Java: Maven Central
- GitHub: https://github.com/swasthya/sdk (Apache 2.0)

**Documentation**:
- API reference with Sphinx (Python), JSDoc (JavaScript), Javadoc (Java)
- Interactive tutorials with Jupyter notebooks
- Workshop materials with hands-on exercises
- Video tutorials in English and Hindi

### 2. Cost Transparency Dashboard

**Purpose**: Real-time cost monitoring and optimization

**Architecture**:
```
┌─────────────────────────────────────────────────────────────┐
│                    Cost Dashboard UI                         │
│  (React + D3.js + Material-UI)                              │
└────────────────────────┬────────────────────────────────────┘
                         │
┌────────────────────────▼────────────────────────────────────┐
│                Cost Analytics Service                        │
│  - Real-time cost aggregation                               │
│  - Optimization recommendations                             │
│  - Report generation                                        │
└────────────────────────┬────────────────────────────────────┘
                         │
        ┌────────────────┼────────────────┐
        │                │                │
┌───────▼──────┐  ┌─────▼─────┐  ┌──────▼──────┐
│ AWS Cost     │  │ CloudWatch│  │ Custom      │
│ Explorer API │  │ Metrics   │  │ Cost DB     │
└──────────────┘  └───────────┘  └─────────────┘
```

**Key Features**:

1. **Real-Time Cost Tracking**:
   - Per-patient cost breakdown
   - Service-wise allocation (compute, storage, AI, network)
   - Department and cohort segmentation
   - Hourly, daily, weekly, monthly views

2. **Cost Comparison**:
   - Swasthya vs traditional EMR systems
   - Target (₹23) vs actual costs
   - Hospital-to-hospital benchmarking

3. **Optimization Recommendations**:
   - Right-sizing compute resources
   - Storage lifecycle policies
   - Reserved instance opportunities
   - Unused resource identification

4. **Export & Reporting**:
   - CSV, PDF, Excel formats
   - Scheduled email reports
   - Custom date ranges
   - Executive summaries

**Data Model**:
```json
{
  "costRecord": {
    "timestamp": "2026-02-15T10:30:00Z",
    "hospitalId": "string",
    "patientId": "string",
    "costBreakdown": {
      "compute": 8.5,
      "storage": 4.2,
      "aiInference": 3.1,
      "dataTransfer": 2.8,
      "monitoring": 1.5,
      "other": 2.9
    },
    "totalCost": 23.0,
    "currency": "INR",
    "services": [
      {
        "name": "Lambda-TriageAgent",
        "cost": 2.1,
        "invocations": 150
      }
    ]
  }
}
```

**API Endpoints**:
```
GET  /api/v1/costs/realtime?hospitalId={id}&period={period}
GET  /api/v1/costs/breakdown?patientId={id}&startDate={date}&endDate={date}
GET  /api/v1/costs/comparison?hospitalIds={ids}
GET  /api/v1/costs/recommendations?hospitalId={id}
POST /api/v1/costs/export
```

### 3. Rural Accessibility Components

#### 3.1 Offline-First PWA

**Architecture**:
```
┌─────────────────────────────────────────────────────────────┐
│                    PWA Frontend                              │
│  (React + Service Worker + IndexedDB)                       │
└────────────────────────┬────────────────────────────────────┘
                         │
                    Online/Offline?
                         │
        ┌────────────────┴────────────────┐
        │ Online                           │ Offline
┌───────▼──────┐                  ┌───────▼──────┐
│ Cloud APIs   │                  │ Local Cache  │
│ (Real-time)  │                  │ (IndexedDB)  │
└──────────────┘                  └──────┬───────┘
                                         │
                                  ┌──────▼───────┐
                                  │ Background   │
                                  │ Sync Queue   │
                                  └──────────────┘
```

**Service Worker Strategy**:
```javascript
// Cache-first for static assets
self.addEventListener('fetch', (event) => {
  if (event.request.url.includes('/static/')) {
    event.respondWith(
      caches.match(event.request)
        .then(response => response || fetch(event.request))
    );
  }
});

// Network-first with offline fallback for API calls
if (event.request.url.includes('/api/')) {
  event.respondWith(
    fetch(event.request)
      .catch(() => caches.match(event.request))
  );
}

// Background sync for offline actions
self.addEventListener('sync', (event) => {
  if (event.tag === 'sync-health-records') {
    event.waitUntil(syncHealthRecords());
  }
});
```

**Offline Capabilities**:
- View cached health records
- Record vital signs
- Schedule appointments (queued)
- Access medical reference content
- View medication lists
- Register new patients (queued)

**Storage Management**:
- Maximum 100MB local storage
- Intelligent cache eviction (LRU)
- Prioritize recent and critical data
- Compress data with gzip

**Sync Strategy**:
- Automatic sync when online
- Conflict resolution (server wins for clinical data)
- Retry failed syncs with exponential backoff
- User notification of sync status

#### 3.2 SMS Alert System

**Architecture**:
```
┌─────────────────────────────────────────────────────────────┐
│              Alert Generation Service                        │
│  (Appointment reminders, lab results, emergencies)          │
└────────────────────────┬────────────────────────────────────┘
                         │
┌────────────────────────▼────────────────────────────────────┐
│              SMS Gateway Service                             │
│  - Template management                                       │
│  - Language selection                                        │
│  - Delivery tracking                                         │
└────────────────────────┬────────────────────────────────────┘
                         │
        ┌────────────────┴────────────────┐
        │                                  │
┌───────▼──────┐                  ┌───────▼──────┐
│ AWS SNS      │                  │ Twilio       │
│ (Primary)    │                  │ (Backup)     │
└──────────────┘                  └──────────────┘
```

**SMS Templates**:
```
Appointment Reminder (Hindi):
"नमस्ते {name}, आपकी अपॉइंटमेंट {date} को {time} बजे है। 
डॉ. {doctor} के साथ। कॉल करें: {phone}"

Lab Result (English):
"Hello {name}, your {test_name} result is ready. 
View at swasthya.health or call {phone}"

Emergency Alert (Tamil):
"அவசரம்! {patient_name} க்கு உடனடி மருத்துவ கவனிப்பு தேவை। 
அழைக்கவும்: {emergency_number}"
```

**Features**:
- Unicode support for 22 Indian languages
- 160-character limit with smart truncation
- Delivery status tracking
- Retry logic (3 attempts)
- Opt-in/opt-out management
- Cost tracking (₹2/patient/month budget)

#### 3.3 Voice Interface

**Architecture**:
```
┌─────────────────────────────────────────────────────────────┐
│                Voice Interface Layer                         │
└────────────────────────┬────────────────────────────────────┘
                         │
        ┌────────────────┴────────────────┐
        │                                  │
┌───────▼──────┐                  ┌───────▼──────┐
│ Speech-to-   │                  │ Text-to-     │
│ Text (STT)   │                  │ Speech (TTS) │
│              │                  │              │
│ AWS Transcribe│                 │ AWS Polly    │
│ Bhashini API │                  │ Bhashini API │
└──────┬───────┘                  └───────▲──────┘
       │                                  │
       └──────────────┬───────────────────┘
                      │
              ┌───────▼──────┐
              │ NLU Engine   │
              │ (Intent      │
              │ Recognition) │
              └──────────────┘
```

**Supported Commands**:
- "Book appointment with Dr. Sharma"
- "Show my health records"
- "What are my medications?"
- "Check my blood pressure reading"
- "Call emergency services"

**Language Support**:
- 22 Indian languages
- Code-mixing (Hinglish, Tanglish)
- Accent adaptation
- Noise cancellation

**Performance**:
- STT latency: <2 seconds
- TTS latency: <1 second
- End-to-end interaction: <5 seconds
- 90% transcription accuracy

#### 3.4 USSD Integration

**Menu Structure**:
```
*123# - Swasthya Main Menu
├── 1. Book Appointment
│   ├── 1. Select Hospital
│   ├── 2. Select Doctor
│   ├── 3. Select Date
│   └── 4. Confirm
├── 2. View Health Records
│   ├── 1. Recent Visits
│   ├── 2. Medications
│   └── 3. Lab Results
├── 3. Emergency Services
│   ├── 1. Call Ambulance
│   └── 2. Nearest Hospital
└── 4. Language / भाषा
    ├── 1. English
    ├── 2. हिंदी
    └── 3. More...
```

**Technical Implementation**:
- USSD gateway integration with telecom operators
- Session management (180-second timeout)
- State machine for menu navigation
- Database queries optimized for <3 second response
- Zero cost for patients (operator partnership)

### 4. Validation & Quality Platform

**Architecture**:
```
┌─────────────────────────────────────────────────────────────┐
│              Validation Dashboard (React)                    │
│  - Expert review interface                                   │
│  - Accuracy metrics                                          │
│  - Outcome tracking                                          │
└────────────────────────┬────────────────────────────────────┘
                         │
┌────────────────────────▼────────────────────────────────────┐
│           Validation Service (Python/FastAPI)                │
│  - Expert validation workflow                                │
│  - Accuracy calculation                                      │
│  - Outcome analysis                                          │
│  - Report generation                                         │
└────────────────────────┬────────────────────────────────────┘
                         │
        ┌────────────────┼────────────────┐
        │                │                │
┌───────▼──────┐  ┌─────▼─────┐  ┌──────▼──────┐
│ Validation   │  │ AI        │  │ Outcome     │
│ Database     │  │ Predictions│  │ Database    │
│ (PostgreSQL) │  │ Log       │  │ (PostgreSQL)│
└──────────────┘  └───────────┘  └─────────────┘
```

**Expert Validation Workflow**:
1. AI makes prediction with confidence score
2. If confidence < 70% or critical case, route to expert
3. Expert reviews prediction with full context
4. Expert approves, rejects, or modifies
5. System records validation result
6. Feedback incorporated into model retraining

**Accuracy Metrics**:
- Precision, Recall, F1-Score
- AUC-ROC, AUC-PR
- Sensitivity, Specificity
- Concordance rate with experts
- Segmented by demographics, conditions, hospitals

**Outcome Tracking**:
```json
{
  "outcomeRecord": {
    "predictionId": "string",
    "patientId": "string",
    "algorithmType": "triage|scheduling|diagnosis",
    "prediction": {...},
    "actualOutcome": {
      "readmitted": false,
      "complications": [],
      "recoveryTime": 7,
      "patientSatisfaction": 4.5
    },
    "timestamp": "2026-02-15T10:30:00Z"
  }
}
```

### 5. AI Ethics & Governance Components

#### 5.1 Bias Detection System

**Architecture**:
```
┌─────────────────────────────────────────────────────────────┐
│              Bias Detection Engine                           │
│  (Fairlearn + AIF360 + Custom Metrics)                      │
└────────────────────────┬────────────────────────────────────┘
                         │
        ┌────────────────┼────────────────┐
        │                │                │
┌───────▼──────┐  ┌─────▼─────┐  ┌──────▼──────┐
│ Demographic  │  │ Prediction│  │ Fairness    │
│ Analysis     │  │ Analysis  │  │ Metrics     │
└──────────────┘  └───────────┘  └─────────────┘
```

**Fairness Metrics**:
- **Demographic Parity**: P(Ŷ=1|A=0) = P(Ŷ=1|A=1)
- **Equalized Odds**: P(Ŷ=1|Y=y,A=0) = P(Ŷ=1|Y=y,A=1)
- **Equal Opportunity**: P(Ŷ=1|Y=1,A=0) = P(Ŷ=1|Y=1,A=1)
- **Calibration**: P(Y=1|Ŷ=p,A=0) = P(Y=1|Ŷ=p,A=1)

**Protected Attributes**:
- Gender
- Age groups
- Socioeconomic status
- Geographic region (urban/rural)
- Language
- Religion (where applicable)

**Bias Detection Process**:
1. Weekly automated analysis of all AI predictions
2. Calculate fairness metrics across protected attributes
3. Statistical significance testing (p < 0.05)
4. Generate bias report with visualizations
5. Alert ethics committee if bias detected
6. Recommend mitigation strategies

**Mitigation Strategies**:
- Data rebalancing
- Reweighting samples
- Adversarial debiasing
- Post-processing adjustments
- Algorithm replacement if severe bias

#### 5.2 Explainable AI (XAI) Engine

**Architecture**:
```
┌─────────────────────────────────────────────────────────────┐
│                  XAI Engine                                  │
└────────────────────────┬────────────────────────────────────┘
                         │
        ┌────────────────┼────────────────┐
        │                │                │
┌───────▼──────┐  ┌─────▼─────┐  ┌──────▼──────┐
│ SHAP         │  │ LIME      │  │ Attention   │
│ Explainer    │  │ Explainer │  │ Visualization│
└──────────────┘  └───────────┘  └─────────────┘
```

**Explanation Types**:

1. **Feature Importance** (SHAP):
```python
# Top 5 factors influencing triage decision
{
  "factors": [
    {"name": "Temperature", "importance": 0.35, "value": "101.5°F"},
    {"name": "Age", "importance": 0.22, "value": "65 years"},
    {"name": "Respiratory Rate", "importance": 0.18, "value": "24/min"},
    {"name": "Diabetes History", "importance": 0.15, "value": "Yes"},
    {"name": "Blood Pressure", "importance": 0.10, "value": "140/90"}
  ]
}
```

2. **Natural Language Explanation**:
```
"This patient was triaged as URGENT because:
1. High fever (101.5°F) indicates possible infection
2. Age 65+ increases risk for complications
3. Elevated respiratory rate suggests respiratory distress
4. Diabetes history increases infection severity risk
5. Blood pressure is slightly elevated

Recommendation: See doctor within 30 minutes"
```

3. **Visual Explanation**:
- Decision tree visualization
- Attention heatmaps for image analysis
- SHAP waterfall plots
- Counterfactual examples

**Multi-Language Support**:
- Explanations generated in 22 Indian languages
- Medical terminology localization
- Cultural context adaptation

#### 5.3 Human-in-the-Loop (HITL) System

**Workflow**:
```
AI Prediction → Confidence Check → Critical Case Check
                                          │
                                    Yes   │   No
                                          │
                                  ┌───────▼──────┐
                                  │ Route to     │
                                  │ Human Expert │
                                  └───────┬──────┘
                                          │
                                  ┌───────▼──────┐
                                  │ Expert       │
                                  │ Review       │
                                  └───────┬──────┘
                                          │
                        ┌─────────────────┼─────────────────┐
                        │                 │                 │
                   Approve           Modify            Reject
                        │                 │                 │
                        └─────────────────┴─────────────────┘
                                          │
                                  ┌───────▼──────┐
                                  │ Execute      │
                                  │ Decision     │
                                  └──────────────┘
```

**Critical Case Criteria**:
- Life-threatening conditions
- High-risk procedures
- AI confidence < 70%
- Patient request for human review
- Regulatory requirement

**Expert Review Interface**:
- Complete patient context
- AI recommendation with explanation
- Supporting evidence (labs, imaging, history)
- Override capability with mandatory documentation
- Escalation to senior expert if needed

**Performance Targets**:
- Review turnaround: <10 minutes for critical cases
- Expert availability: 24/7 coverage
- Escalation time: <15 minutes

#### 5.4 Audit Framework

**Audit Types**:

1. **Automated Monthly Audits**:
   - Algorithm performance metrics
   - Bias detection results
   - Security compliance checks
   - Data privacy compliance
   - ABDM/DISHA compliance

2. **Quarterly External Audits**:
   - Independent auditor review
   - Compliance certification
   - Risk assessment
   - Remediation verification

3. **Annual Comprehensive Audits**:
   - Full system review
   - Ethics committee evaluation
   - Regulatory compliance
   - Strategic recommendations

**Audit Report Structure**:
```json
{
  "auditReport": {
    "id": "string",
    "type": "automated|external|comprehensive",
    "date": "2026-02-15",
    "scope": ["algorithm_performance", "bias", "security", "compliance"],
    "findings": [
      {
        "id": "F001",
        "severity": "HIGH|MEDIUM|LOW",
        "category": "bias|security|compliance|performance",
        "description": "...",
        "evidence": "...",
        "recommendation": "...",
        "dueDate": "2026-03-15"
      }
    ],
    "overallScore": 95,
    "status": "PASS|FAIL|CONDITIONAL"
  }
}
```

**Remediation Tracking**:
- Automated ticket creation
- Priority assignment
- Owner assignment
- Progress tracking
- Verification testing
- Closure approval



## Data Models

### SDK Package Metadata

```json
{
  "package": {
    "name": "swasthya-sdk",
    "version": "1.0.0",
    "license": "Apache-2.0",
    "repository": "https://github.com/swasthya/sdk",
    "languages": ["python", "javascript", "java"],
    "algorithms": [
      {
        "name": "triage",
        "type": "rule-based",
        "accuracy": 0.93,
        "responseTime": "50ms"
      },
      {
        "name": "scheduling",
        "type": "optimization",
        "library": "OR-Tools",
        "efficiency": "45% improvement"
      },
      {
        "name": "forecasting",
        "type": "time-series",
        "library": "Prophet",
        "accuracy": 0.85
      }
    ],
    "dependencies": {
      "python": ["prophet", "ortools", "transformers"],
      "javascript": ["@google-cloud/aiplatform"],
      "java": ["com.google.ortools"]
    }
  }
}
```

### Cost Record Schema

```sql
CREATE TABLE cost_records (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    timestamp TIMESTAMP NOT NULL,
    hospital_id VARCHAR(100) NOT NULL,
    patient_id VARCHAR(100),
    service_name VARCHAR(255) NOT NULL,
    cost_inr DECIMAL(10, 2) NOT NULL,
    cost_category VARCHAR(50) NOT NULL, -- compute, storage, ai, network, monitoring
    resource_id VARCHAR(255),
    usage_quantity DECIMAL(15, 4),
    usage_unit VARCHAR(50),
    metadata JSONB,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_cost_records_timestamp ON cost_records(timestamp);
CREATE INDEX idx_cost_records_hospital ON cost_records(hospital_id);
CREATE INDEX idx_cost_records_patient ON cost_records(patient_id);
CREATE INDEX idx_cost_records_category ON cost_records(cost_category);
```

### Offline Sync Queue Schema

```javascript
// IndexedDB Schema for PWA
const dbSchema = {
  name: 'SwasthyaOffline',
  version: 1,
  stores: {
    healthRecords: {
      keyPath: 'id',
      indexes: [
        { name: 'patientId', keyPath: 'patientId' },
        { name: 'timestamp', keyPath: 'timestamp' }
      ]
    },
    syncQueue: {
      keyPath: 'id',
      autoIncrement: true,
      indexes: [
        { name: 'action', keyPath: 'action' },
        { name: 'status', keyPath: 'status' },
        { name: 'timestamp', keyPath: 'timestamp' }
      ]
    },
    medicalReference: {
      keyPath: 'id',
      indexes: [
        { name: 'category', keyPath: 'category' },
        { name: 'language', keyPath: 'language' }
      ]
    }
  }
};

// Sync Queue Item
{
  "id": 1,
  "action": "CREATE_APPOINTMENT|UPDATE_RECORD|REGISTER_PATIENT",
  "payload": {...},
  "status": "PENDING|IN_PROGRESS|COMPLETED|FAILED",
  "retryCount": 0,
  "timestamp": "2026-02-15T10:30:00Z",
  "error": null
}
```

### Validation Record Schema

```sql
CREATE TABLE validation_records (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    prediction_id UUID NOT NULL,
    algorithm_type VARCHAR(50) NOT NULL,
    algorithm_version VARCHAR(20) NOT NULL,
    patient_id VARCHAR(100) NOT NULL,
    prediction JSONB NOT NULL,
    expert_id VARCHAR(100),
    expert_decision VARCHAR(20), -- APPROVE, REJECT, MODIFY
    expert_feedback TEXT,
    validation_timestamp TIMESTAMP,
    concordance BOOLEAN,
    confidence_score DECIMAL(3, 2),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_validation_algorithm ON validation_records(algorithm_type);
CREATE INDEX idx_validation_expert ON validation_records(expert_id);
CREATE INDEX idx_validation_concordance ON validation_records(concordance);
```

### Bias Detection Record Schema

```sql
CREATE TABLE bias_detection_records (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    analysis_date DATE NOT NULL,
    algorithm_type VARCHAR(50) NOT NULL,
    protected_attribute VARCHAR(50) NOT NULL, -- gender, age_group, region, etc.
    group_a VARCHAR(100) NOT NULL,
    group_b VARCHAR(100) NOT NULL,
    metric_name VARCHAR(50) NOT NULL, -- demographic_parity, equalized_odds, etc.
    metric_value DECIMAL(5, 4) NOT NULL,
    threshold DECIMAL(5, 4) NOT NULL,
    bias_detected BOOLEAN NOT NULL,
    statistical_significance DECIMAL(5, 4),
    sample_size_a INTEGER,
    sample_size_b INTEGER,
    mitigation_recommended TEXT,
    status VARCHAR(20), -- DETECTED, INVESTIGATING, MITIGATED, RESOLVED
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_bias_date ON bias_detection_records(analysis_date);
CREATE INDEX idx_bias_algorithm ON bias_detection_records(algorithm_type);
CREATE INDEX idx_bias_detected ON bias_detection_records(bias_detected);
```

### Outcome Tracking Schema

```sql
CREATE TABLE patient_outcomes (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    patient_id VARCHAR(100) NOT NULL,
    prediction_id UUID,
    encounter_id VARCHAR(100) NOT NULL,
    algorithm_type VARCHAR(50),
    ai_assisted BOOLEAN DEFAULT FALSE,
    admission_date DATE NOT NULL,
    discharge_date DATE,
    length_of_stay INTEGER,
    readmitted_30_days BOOLEAN,
    complications JSONB,
    recovery_score INTEGER, -- 1-10
    patient_satisfaction DECIMAL(2, 1), -- 1.0-5.0
    mortality BOOLEAN,
    cost_inr DECIMAL(10, 2),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_outcomes_patient ON patient_outcomes(patient_id);
CREATE INDEX idx_outcomes_prediction ON patient_outcomes(prediction_id);
CREATE INDEX idx_outcomes_algorithm ON patient_outcomes(algorithm_type);
CREATE INDEX idx_outcomes_ai_assisted ON patient_outcomes(ai_assisted);
```

### Audit Record Schema

```sql
CREATE TABLE audit_records (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    audit_type VARCHAR(50) NOT NULL, -- automated, external, comprehensive
    audit_date DATE NOT NULL,
    scope JSONB NOT NULL, -- array of audit areas
    findings JSONB NOT NULL, -- array of finding objects
    overall_score INTEGER,
    status VARCHAR(20) NOT NULL, -- PASS, FAIL, CONDITIONAL
    auditor_name VARCHAR(255),
    auditor_organization VARCHAR(255),
    report_url VARCHAR(500),
    next_audit_date DATE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE audit_findings (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    audit_id UUID REFERENCES audit_records(id),
    finding_code VARCHAR(20) NOT NULL,
    severity VARCHAR(10) NOT NULL, -- HIGH, MEDIUM, LOW
    category VARCHAR(50) NOT NULL,
    description TEXT NOT NULL,
    evidence TEXT,
    recommendation TEXT,
    due_date DATE,
    assigned_to VARCHAR(100),
    status VARCHAR(20) DEFAULT 'OPEN', -- OPEN, IN_PROGRESS, RESOLVED, CLOSED
    resolution_notes TEXT,
    resolved_at TIMESTAMP,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_audit_type ON audit_records(audit_type);
CREATE INDEX idx_audit_date ON audit_records(audit_date);
CREATE INDEX idx_findings_audit ON audit_findings(audit_id);
CREATE INDEX idx_findings_status ON audit_findings(status);
```

### SMS Alert Schema

```sql
CREATE TABLE sms_alerts (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    patient_id VARCHAR(100) NOT NULL,
    phone_number VARCHAR(15) NOT NULL,
    alert_type VARCHAR(50) NOT NULL, -- appointment, lab_result, emergency, medication
    language VARCHAR(10) NOT NULL,
    message_text TEXT NOT NULL,
    status VARCHAR(20) NOT NULL, -- PENDING, SENT, DELIVERED, FAILED
    provider VARCHAR(50), -- AWS_SNS, TWILIO
    provider_message_id VARCHAR(255),
    sent_at TIMESTAMP,
    delivered_at TIMESTAMP,
    retry_count INTEGER DEFAULT 0,
    cost_inr DECIMAL(5, 2),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_sms_patient ON sms_alerts(patient_id);
CREATE INDEX idx_sms_status ON sms_alerts(status);
CREATE INDEX idx_sms_sent_at ON sms_alerts(sent_at);
```

### USSD Session Schema

```sql
CREATE TABLE ussd_sessions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    session_id VARCHAR(100) UNIQUE NOT NULL,
    phone_number VARCHAR(15) NOT NULL,
    patient_id VARCHAR(100),
    language VARCHAR(10) DEFAULT 'en',
    current_menu VARCHAR(50) NOT NULL,
    menu_history JSONB,
    session_data JSONB,
    status VARCHAR(20) NOT NULL, -- ACTIVE, COMPLETED, TIMEOUT, ERROR
    started_at TIMESTAMP NOT NULL,
    last_activity_at TIMESTAMP NOT NULL,
    ended_at TIMESTAMP,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_ussd_session_id ON ussd_sessions(session_id);
CREATE INDEX idx_ussd_phone ON ussd_sessions(phone_number);
CREATE INDEX idx_ussd_status ON ussd_sessions(status);
```

### Deployment Tracking Schema

```sql
CREATE TABLE pilot_deployments (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    hospital_id VARCHAR(100) UNIQUE NOT NULL,
    hospital_name VARCHAR(255) NOT NULL,
    location VARCHAR(255) NOT NULL,
    deployment_phase VARCHAR(50) NOT NULL, -- PLANNING, SETUP, TESTING, GO_LIVE, PRODUCTION
    start_date DATE NOT NULL,
    go_live_date DATE,
    completion_date DATE,
    deployment_lead VARCHAR(255),
    infrastructure_ready BOOLEAN DEFAULT FALSE,
    integration_complete BOOLEAN DEFAULT FALSE,
    training_complete BOOLEAN DEFAULT FALSE,
    validation_complete BOOLEAN DEFAULT FALSE,
    issues_count INTEGER DEFAULT 0,
    user_adoption_rate DECIMAL(5, 2),
    status VARCHAR(20) NOT NULL, -- ON_TRACK, AT_RISK, DELAYED, COMPLETED
    notes TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_deployment_hospital ON pilot_deployments(hospital_id);
CREATE INDEX idx_deployment_phase ON pilot_deployments(deployment_phase);
CREATE INDEX idx_deployment_status ON pilot_deployments(status);
```



## Correctness Properties

A property is a characteristic or behavior that should hold true across all valid executions of a system—essentially, a formal statement about what the system should do. Properties serve as the bridge between human-readable specifications and machine-verifiable correctness guarantees.

### Phase 1: SDK & Foundation Properties

**Property 1: SDK Package Completeness**
*For any* SDK package distribution (Python, JavaScript, Java), the package SHALL include all required components: source code, API documentation, examples, tests, and contribution guidelines.
**Validates: Requirements 1.2, 1.3, 1.4, 1.5**

**Property 2: Algorithm Output Format Consistency**
*For any* algorithm execution in the SDK, the output SHALL be valid FHIR R4 format that can be parsed without errors.
**Validates: Requirements 2.5**

**Property 3: SDK Backward Compatibility**
*For any* major version release, all public APIs from the previous major version SHALL remain functional or provide clear deprecation warnings.
**Validates: Requirements 1.7**

**Property 4: Test Coverage Threshold**
*For any* SDK release, automated tests SHALL achieve minimum 80% code coverage across all modules.
**Validates: Requirements 1.8**

**Property 5: Algorithm Performance Benchmarks**
*For any* algorithm in the SDK, execution SHALL complete within documented performance thresholds (e.g., triage <50ms, scheduling <5s).
**Validates: Requirements 2.6**

**Property 6: Synthetic Data Generation Validity**
*For any* generated synthetic patient data, the data SHALL conform to FHIR R4 schema and pass validation rules.
**Validates: Requirements 2.8**

### Phase 2: Cost & Validation Properties

**Property 7: Cost Calculation Accuracy**
*For any* time period, the sum of individual service costs SHALL equal the total cost displayed in the dashboard within 0.01 INR tolerance.
**Validates: Requirements 3.1, 3.2**

**Property 8: Cost Target Compliance**
*For any* hospital deployment, the average per-patient monthly cost SHALL not exceed ₹23 when measured over a 30-day rolling window.
**Validates: Requirements 3.8, 21.8**

**Property 9: Cost Export Data Integrity**
*For any* exported cost report (CSV/PDF), the data SHALL match the dashboard display exactly for the same time period and filters.
**Validates: Requirements 3.5**

**Property 10: Optimization Recommendation Validity**
*For any* cost optimization recommendation, implementing the recommendation SHALL result in cost reduction within 10% of the estimated savings.
**Validates: Requirements 21.1, 21.7**

**Property 11: Validation Concordance Rate**
*For any* set of AI predictions reviewed by medical experts, the concordance rate SHALL be at least 90%.
**Validates: Requirements 6.7**

**Property 12: Accuracy Metric Calculation**
*For any* algorithm, calculated accuracy metrics (precision, recall, F1, AUC-ROC) SHALL match standard statistical formulas within 0.001 tolerance.
**Validates: Requirements 7.1**

**Property 13: Validation Completeness**
*For any* pilot hospital deployment, validation SHALL achieve 90% completion before production go-live.
**Validates: Requirements 22.8**

**Property 14: Deployment Timeline**
*For any* single pilot hospital deployment, completion SHALL occur within 30 days from start date.
**Validates: Requirements 5.8**

### Phase 3: Rural Accessibility Properties

**Property 15: Offline Data Sync Consistency**
*For any* data modified offline in the PWA, when synced online, the data SHALL match the offline version or resolve conflicts according to defined rules (server wins for clinical data).
**Validates: Requirements 17.4**

**Property 16: API Response Size Limit**
*For any* standard API operation, the response payload SHALL be smaller than 50KB after compression.
**Validates: Requirements 16.1, 4.5**

**Property 17: Data Compression Ratio**
*For any* API response, gzip compression SHALL achieve minimum 70% size reduction.
**Validates: Requirements 16.2, 4.9**

**Property 18: PWA Storage Limit**
*For any* PWA installation, total local storage usage SHALL not exceed 100MB.
**Validates: Requirements 17.7**

**Property 19: SMS Message Length**
*For any* SMS alert, the message SHALL not exceed 160 characters while maintaining required information.
**Validates: Requirements 18.3**

**Property 20: SMS Cost Budget**
*For any* patient, SMS costs SHALL not exceed ₹2 per month averaged over a 30-day period.
**Validates: Requirements 18.7**

**Property 21: Voice Transcription Accuracy**
*For any* voice input in supported languages, speech-to-text transcription SHALL achieve minimum 90% word accuracy.
**Validates: Requirements 15.2**

**Property 22: Voice Response Latency**
*For any* standard voice query, the complete interaction (STT + processing + TTS) SHALL complete within 5 seconds.
**Validates: Requirements 15.8**

**Property 23: USSD Response Time**
*For any* USSD menu selection, the system SHALL respond within 3 seconds.
**Validates: Requirements 19.3**

**Property 24: USSD Session Timeout**
*For any* USSD session, the session SHALL remain active for 180 seconds before automatic timeout.
**Validates: Requirements 19.6**

### Phase 4: AI Ethics & Governance Properties

**Property 25: Bias Detection Statistical Significance**
*For any* detected bias, the statistical significance SHALL be p < 0.05 before generating an alert.
**Validates: Requirements 9.2**

**Property 26: Fairness Metric Calculation**
*For any* algorithm and protected attribute pair, fairness metrics (demographic parity, equalized odds, equal opportunity) SHALL be calculated according to standard definitions.
**Validates: Requirements 9.3**

**Property 27: Bias Detection Frequency**
*For any* AI algorithm in production, bias detection analysis SHALL run automatically on a weekly basis.
**Validates: Requirements 9.5**

**Property 28: XAI Explanation Generation Time**
*For any* AI prediction, the explainable AI module SHALL generate an explanation within 2 seconds.
**Validates: Requirements 10.6**

**Property 29: XAI Top Factors Completeness**
*For any* AI explanation, the system SHALL provide exactly 5 top influencing factors with relative importance scores that sum to 1.0.
**Validates: Requirements 10.2**

**Property 30: HITL Critical Case Routing**
*For any* AI prediction with confidence below 70% or identified as life-threatening, the system SHALL route to human expert review.
**Validates: Requirements 11.2**

**Property 31: HITL Review Turnaround Time**
*For any* critical case routed to human review, the review SHALL be completed within 10 minutes on average.
**Validates: Requirements 11.8**

**Property 32: HITL Escalation Time**
*For any* critical case where the initial reviewer is unavailable, escalation to senior provider SHALL occur within 15 minutes.
**Validates: Requirements 11.6**

**Property 33: Audit Finding Closure Rate**
*For any* audit cycle, 100% of findings SHALL be closed (resolved or mitigated) within 90 days.
**Validates: Requirements 12.8**

**Property 34: Audit History Retention**
*For any* audit record, the system SHALL maintain the record for minimum 7 years.
**Validates: Requirements 12.5**

**Property 35: Ethics Committee Review Timeline**
*For any* ethics committee recommendation, implementation SHALL occur within 30 days or provide documented justification for delays.
**Validates: Requirements 13.6**

**Property 36: System Uptime**
*For any* 30-day measurement period, system uptime SHALL be at least 99.9%.
**Validates: Requirements 14.8**

**Property 37: Critical Operation Response Time**
*For any* critical operation (triage, emergency alerts, vital signs recording), response time SHALL be less than 200ms at the 95th percentile.
**Validates: Requirements 14.8**

**Property 38: Outcome Data Linkage**
*For any* AI-assisted clinical decision, the system SHALL link the decision to subsequent patient outcomes for tracking.
**Validates: Requirements 8.2**

**Property 39: Outcome Privacy Preservation**
*For any* outcome report or research data export, patient data SHALL be de-identified according to HIPAA Safe Harbor or Expert Determination methods.
**Validates: Requirements 8.5**

**Property 40: Compliance Audit Score**
*For any* regulatory audit (ABDM, DISHA, FHIR), the system SHALL achieve 100% compliance score.
**Validates: Requirements 25.8**

### Cross-Phase Properties

**Property 41: Multi-Language Support Completeness**
*For any* user-facing feature (UI, voice, SMS, USSD), the system SHALL support all 22 specified Indian languages.
**Validates: Requirements 4.4, 10.3, 15.1, 18.4, 19.4**

**Property 42: FHIR R4 Compliance**
*For any* health data exchange, the data format SHALL validate against FHIR R4 schema without errors.
**Validates: Requirements 2.5, 8.8, 25.3**

**Property 43: Data Encryption at Rest**
*For any* stored patient data (cloud or local), the data SHALL be encrypted using AES-256 or stronger.
**Validates: Requirements 17.3, 25.2**

**Property 44: API Authentication**
*For any* API request, the system SHALL require valid authentication tokens and reject unauthenticated requests.
**Validates: Requirements 25.2**

**Property 45: Deployment Parallelization**
*For any* multi-hospital deployment, failures in one hospital SHALL not affect deployments in other hospitals.
**Validates: Requirements 24.3**


## Error Handling

### SDK Error Handling

**Algorithm Execution Errors**:
- Invalid input data: Return structured error with field-level validation messages
- Missing required parameters: Raise `MissingParameterError` with parameter name
- Algorithm timeout: Raise `TimeoutError` after configured threshold
- Resource unavailable: Raise `ResourceUnavailableError` with retry guidance

**Data Validation Errors**:
- FHIR validation failure: Return detailed validation report with line numbers and error descriptions
- Schema mismatch: Raise `SchemaValidationError` with expected vs actual schema
- Data type errors: Raise `TypeMismatchError` with expected and received types

**Error Response Format**:
```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Input validation failed",
    "details": [
      {
        "field": "vitalSigns.temperature",
        "error": "Value must be between 95 and 110",
        "received": "150"
      }
    ],
    "timestamp": "2026-02-15T10:30:00Z",
    "requestId": "req_abc123"
  }
}
```

### Cost Dashboard Error Handling

**Data Collection Errors**:
- AWS Cost Explorer API failure: Retry with exponential backoff (3 attempts), fallback to cached data
- CloudWatch metrics unavailable: Display warning banner, use last known values
- Database connection failure: Queue requests, retry on reconnection

**Calculation Errors**:
- Division by zero in per-patient cost: Handle gracefully, display "N/A" for zero-patient periods
- Missing cost data: Interpolate from adjacent time periods, mark as estimated
- Currency conversion errors: Use last known exchange rate, display warning

**Export Errors**:
- PDF generation failure: Retry once, offer CSV alternative
- Large dataset timeout: Implement pagination, limit to 10,000 records per export
- File system errors: Return error message, log for investigation

### Rural Accessibility Error Handling

**Offline PWA Errors**:
- Storage quota exceeded: Implement LRU eviction, notify user
- Sync conflict: Apply server-wins strategy for clinical data, notify user of changes
- IndexedDB corruption: Clear corrupted data, re-sync from server
- Service worker registration failure: Fall back to online-only mode

**SMS Errors**:
- Delivery failure: Retry up to 3 times with 5-minute intervals
- Invalid phone number: Validate before sending, log error
- Provider API timeout: Switch to backup provider (Twilio)
- Rate limit exceeded: Queue messages, send when limit resets

**Voice Interface Errors**:
- Speech recognition failure: Ask user to repeat, offer text input alternative
- TTS synthesis error: Fall back to text display
- Unsupported language: Notify user, offer supported language options
- Network timeout: Cache common responses for offline playback

**USSD Errors**:
- Session timeout: Notify user, allow restart from main menu
- Invalid menu selection: Display error, re-show current menu
- Backend API failure: Display error message, provide support phone number
- Telecom operator issues: Log error, retry on next user input

### Validation Platform Error Handling

**Expert Review Errors**:
- Expert unavailable: Escalate to backup expert within 15 minutes
- Review timeout: Send reminder notification, escalate if no response in 30 minutes
- Conflicting expert opinions: Route to senior expert for final decision

**Accuracy Calculation Errors**:
- Insufficient data: Display warning, require minimum 100 samples for statistical validity
- Missing ground truth: Mark as "pending validation", exclude from accuracy metrics
- Statistical anomalies: Flag for manual review, investigate data quality

**Outcome Tracking Errors**:
- Missing outcome data: Send reminder to data entry staff, mark as incomplete
- Data linkage failure: Use fuzzy matching on patient identifiers, manual review if needed
- Delayed outcome reporting: Accept late data, recalculate metrics, note in reports

### AI Ethics & Governance Error Handling

**Bias Detection Errors**:
- Insufficient sample size: Require minimum 1,000 samples per group, display warning if below
- Statistical test failure: Log error, use alternative test method
- Protected attribute missing: Skip bias analysis for that attribute, log warning

**XAI Errors**:
- Explanation generation timeout: Return simplified explanation, log for investigation
- SHAP calculation failure: Fall back to LIME, then to feature importance
- Translation error: Display explanation in English, log translation failure

**HITL Errors**:
- All experts unavailable: Display urgent alert to administrators, queue for next available expert
- Audit trail logging failure: Block decision execution until logging succeeds
- Override without documentation: Reject override, require mandatory reasoning

**Audit Errors**:
- Audit script failure: Send alert to audit team, retry with manual intervention
- Report generation timeout: Generate partial report, mark as incomplete
- Finding tracking system down: Queue findings, sync when system recovers

### General Error Handling Principles

1. **Graceful Degradation**: System continues operating with reduced functionality rather than complete failure
2. **User-Friendly Messages**: Error messages in plain language, avoid technical jargon
3. **Logging**: All errors logged with context (user ID, timestamp, request ID, stack trace)
4. **Monitoring**: Critical errors trigger alerts to on-call engineers
5. **Retry Logic**: Transient failures retried with exponential backoff
6. **Fallback Strategies**: Alternative approaches when primary method fails
7. **Data Integrity**: Never compromise data integrity; fail safely
8. **User Notification**: Users informed of errors affecting their workflow
9. **Recovery Guidance**: Error messages include next steps or support contact
10. **Compliance**: Error handling maintains ABDM, DISHA, and FHIR compliance


## Testing Strategy

### Dual Testing Approach

The testing strategy employs both unit testing and property-based testing as complementary approaches:

- **Unit Tests**: Verify specific examples, edge cases, error conditions, and integration points
- **Property Tests**: Verify universal properties across all inputs through randomization
- **Together**: Provide comprehensive coverage where unit tests catch concrete bugs and property tests verify general correctness

### Property-Based Testing Configuration

**Library Selection by Language**:
- Python: Hypothesis
- JavaScript/TypeScript: fast-check
- Java: jqwik

**Test Configuration**:
- Minimum 100 iterations per property test (due to randomization)
- Each property test references its design document property
- Tag format: `Feature: swasthya-enhancement-suite, Property {number}: {property_text}`
- Each correctness property implemented by a SINGLE property-based test

### Phase 1: SDK & Foundation Testing

**Unit Tests**:
- SDK package installation across Python, JavaScript, Java
- API documentation completeness checks
- Example code execution verification
- Contribution workflow validation
- Algorithm-specific edge cases (empty inputs, boundary values)
- FHIR format validation with invalid schemas
- Error handling for missing parameters

**Property Tests**:
- Property 1: SDK package completeness across all distributions
- Property 2: Algorithm output FHIR R4 format validity
- Property 3: Backward compatibility for all public APIs
- Property 4: Test coverage threshold maintenance
- Property 5: Algorithm performance benchmarks
- Property 6: Synthetic data generation validity

**Integration Tests**:
- End-to-end algorithm execution (input → processing → FHIR output)
- Multi-language SDK interoperability
- GitHub CI/CD pipeline validation
- Package registry publication workflow

**Performance Tests**:
- Triage algorithm: <50ms for 95th percentile
- Scheduling optimization: <5s for typical hospital size
- Forecasting: <10s for 30-day prediction
- Load testing: 1000 concurrent SDK users

### Phase 2: Cost & Validation Testing

**Unit Tests**:
- Cost calculation for specific service combinations
- Dashboard UI component rendering
- Export format validation (CSV, PDF structure)
- Optimization recommendation generation for known scenarios
- Expert validation workflow state transitions
- Accuracy metric calculation with known datasets
- Deployment checklist item completion

**Property Tests**:
- Property 7: Cost calculation accuracy
- Property 8: Cost target compliance
- Property 9: Cost export data integrity
- Property 10: Optimization recommendation validity
- Property 11: Validation concordance rate
- Property 12: Accuracy metric calculation
- Property 13: Validation completeness
- Property 14: Deployment timeline

**Integration Tests**:
- AWS Cost Explorer API integration
- CloudWatch metrics collection
- PostgreSQL database operations
- Real-time dashboard updates via WebSocket
- Expert review workflow end-to-end
- Multi-hospital deployment orchestration

**Performance Tests**:
- Dashboard load time: <2s for initial render
- Real-time cost updates: <500ms latency
- Export generation: <10s for 1-year data
- Concurrent expert reviews: 50 simultaneous users

### Phase 3: Rural Accessibility Testing

**Unit Tests**:
- Service worker registration and caching
- IndexedDB CRUD operations
- Sync queue management
- SMS template rendering in all 22 languages
- Voice command parsing for specific phrases
- USSD menu navigation paths
- API response compression verification

**Property Tests**:
- Property 15: Offline data sync consistency
- Property 16: API response size limit
- Property 17: Data compression ratio
- Property 18: PWA storage limit
- Property 19: SMS message length
- Property 20: SMS cost budget
- Property 21: Voice transcription accuracy
- Property 22: Voice response latency
- Property 23: USSD response time
- Property 24: USSD session timeout

**Integration Tests**:
- PWA offline-to-online sync workflow
- SMS delivery via AWS SNS and Twilio
- Voice interface with AWS Polly/Transcribe
- USSD session with telecom operator APIs
- Multi-language support across all channels

**Performance Tests**:
- PWA load time on 2G network: <10s
- Offline operation responsiveness: <100ms
- SMS delivery time: <30s
- Voice interaction latency: <5s end-to-end
- USSD response time: <3s per interaction

**Accessibility Tests**:
- Screen reader compatibility for PWA
- Voice interface accuracy across accents
- SMS readability on basic phones
- USSD usability on feature phones

### Phase 4: AI Ethics & Governance Testing

**Unit Tests**:
- Bias detection for known biased datasets
- Fairness metric calculation with edge cases
- XAI explanation generation for specific predictions
- SHAP/LIME calculation verification
- HITL routing logic for critical cases
- Audit report generation with sample findings
- Ethics committee workflow state transitions

**Property Tests**:
- Property 25: Bias detection statistical significance
- Property 26: Fairness metric calculation
- Property 27: Bias detection frequency
- Property 28: XAI explanation generation time
- Property 29: XAI top factors completeness
- Property 30: HITL critical case routing
- Property 31: HITL review turnaround time
- Property 32: HITL escalation time
- Property 33: Audit finding closure rate
- Property 34: Audit history retention
- Property 35: Ethics committee review timeline
- Property 36: System uptime
- Property 37: Critical operation response time
- Property 38: Outcome data linkage
- Property 39: Outcome privacy preservation
- Property 40: Compliance audit score

**Integration Tests**:
- End-to-end bias detection and mitigation workflow
- XAI explanation generation for all algorithm types
- HITL workflow with expert escalation
- Audit cycle from detection to remediation
- Ethics committee review and approval process
- Outcome tracking from prediction to result

**Performance Tests**:
- Bias detection analysis: <5 minutes for 10,000 predictions
- XAI explanation generation: <2s per prediction
- HITL routing latency: <1s
- Audit report generation: <30s
- Outcome analysis query: <5s for 1-year data

### Cross-Phase Testing

**Security Tests**:
- Authentication and authorization for all APIs
- Data encryption at rest and in transit
- SQL injection and XSS vulnerability scanning
- Penetration testing for all public endpoints
- DISHA compliance validation

**Compliance Tests**:
- ABDM certification requirements
- FHIR R4 schema validation
- DISHA data protection compliance
- Audit trail completeness
- Regulatory reporting accuracy

**Disaster Recovery Tests**:
- Database backup and restore
- Service failover and recovery
- Data center outage simulation
- Rollback procedures for failed deployments

**User Acceptance Tests**:
- Pilot hospital deployment validation
- Developer SDK usability testing
- Rural healthcare worker PWA testing
- Medical expert validation interface testing
- Ethics committee portal testing

### Test Automation

**CI/CD Pipeline**:
- Automated test execution on every commit
- Code coverage reporting (80% minimum)
- Performance regression detection
- Security vulnerability scanning
- Deployment smoke tests

**Test Data Management**:
- Synthetic patient data generation
- Test data anonymization
- Test environment provisioning
- Data cleanup after test execution

**Monitoring and Alerting**:
- Test failure notifications
- Performance degradation alerts
- Coverage drop warnings
- Flaky test identification

### Test Metrics

**Coverage Targets**:
- Unit test coverage: 80% minimum
- Integration test coverage: 70% minimum
- Property test coverage: 100% of correctness properties
- End-to-end test coverage: All critical user journeys

**Quality Gates**:
- All tests must pass before merge
- No critical or high-severity bugs
- Performance benchmarks met
- Security scans clean
- Compliance checks passed

**Test Execution Time**:
- Unit tests: <5 minutes
- Integration tests: <15 minutes
- Property tests: <30 minutes
- Full test suite: <1 hour
