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

