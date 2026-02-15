# Swasthya - System Design Document

**Version:** 1.0  
**Date:** February 15, 2026  
**Status:** Draft  
**Project:** Swasthya - India's Decentralized Health Intelligence Network

---

## 1. Introduction

### 1.1 Purpose
This document describes the system design and architecture for Swasthya, detailing how the system meets the requirements specified in REQUIREMENTS.md.

### 1.2 Scope
This design covers:
- System architecture and components
- Data models and schemas
- API specifications
- Security architecture
- Deployment architecture
- Technology stack decisions

### 1.3 Design Goals
- **Scalability**: Support 1.4B users
- **Performance**: <100ms response for critical operations
- **Reliability**: 99.9% uptime
- **Security**: DISHA-compliant, end-to-end encryption
- **Cost-efficiency**: ₹50/patient/month
- **Interoperability**: ABDM/FHIR compliant

---

## 2. System Architecture

### 2.1 High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        Presentation Layer                        │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐         │
│  │ Patient App  │  │ Provider Web │  │  Admin Panel │         │
│  │ (Mobile/Web) │  │  Dashboard   │  │              │         │
│  └──────────────┘  └──────────────┘  └──────────────┘         │
└────────────────────────────┬────────────────────────────────────┘
                             │
┌────────────────────────────▼────────────────────────────────────┐
│                      API Gateway Layer                           │
│  Amazon CloudFront + API Gateway + AWS WAF                      │
│  (Authentication, Rate Limiting, DDoS Protection)               │
└────────────────────────────┬────────────────────────────────────┘
                             │
┌────────────────────────────▼────────────────────────────────────┐
│                    Application Layer (Microservices)             │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐         │
│  │ User Service │  │ Health Record│  │ AI Agent     │         │
│  │              │  │ Service      │  │ Orchestrator │         │
│  └──────────────┘  └──────────────┘  └──────────────┘         │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐         │
│  │ ABDM Gateway │  │ IoT Service  │  │ Analytics    │         │
│  │              │  │              │  │ Service      │         │
│  └──────────────┘  └──────────────┘  └──────────────┘         │
└────────────────────────────┬────────────────────────────────────┘
                             │
┌────────────────────────────▼────────────────────────────────────┐
│                      AI/ML Layer                                 │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐         │
│  │ Amazon       │  │ Amazon       │  │ Amazon       │         │
│  │ Bedrock      │  │ SageMaker    │  │ Comprehend   │         │
│  │ (Nova Models)│  │ (Training)   │  │ Medical      │         │
│  └──────────────┘  └──────────────┘  └──────────────┘         │
└────────────────────────────┬────────────────────────────────────┘
                             │
┌────────────────────────────▼────────────────────────────────────┐
│                      Data Layer                                  │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐         │
│  │ AWS          │  │ Amazon RDS   │  │ Amazon       │         │
│  │ HealthLake   │  │ (PostgreSQL) │  │ DynamoDB     │         │
│  │ (FHIR)       │  │              │  │ (NoSQL)      │         │
│  └──────────────┘  └──────────────┘  └──────────────┘         │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐         │
│  │ Amazon S3    │  │ Blockchain   │  │ Redis Cache  │         │
│  │ (Storage)    │  │ (Hyperledger)│  │              │         │
│  └──────────────┘  └──────────────┘  └──────────────┘         │
└─────────────────────────────────────────────────────────────────┘
```

### 2.2 Architectural Patterns

#### 2.2.1 Microservices Architecture
- Independent, loosely coupled services
- Each service owns its data
- Communication via REST APIs and message queues
- Independent scaling and deployment

#### 2.2.2 Event-Driven Architecture
- Asynchronous communication using Amazon EventBridge
- Event sourcing for audit trails
- CQRS pattern for read/write separation
- Real-time event processing with Kinesis

#### 2.2.3 Multi-Agent System
- Autonomous AI agents with specific responsibilities
- Agent communication via message passing
- Supervisor agent for coordination
- Conflict resolution mechanisms

---

## 3. Component Design


### 3.1 User Service

**Responsibility**: User authentication, authorization, profile management

**Technology Stack**:
- Runtime: Node.js with Express
- Database: Amazon RDS (PostgreSQL)
- Cache: Redis (ElastiCache)
- Authentication: AWS Cognito + Aadhaar

**APIs**:
```
POST   /api/v1/users/register
POST   /api/v1/users/login
GET    /api/v1/users/profile
PUT    /api/v1/users/profile
POST   /api/v1/users/link-health-id
DELETE /api/v1/users/logout
```

**Data Model**:
```json
{
  "userId": "uuid",
  "aadhaarHash": "string",
  "healthId": "string",
  "name": "string",
  "email": "string",
  "phone": "string",
  "dateOfBirth": "date",
  "gender": "enum",
  "language": "string",
  "createdAt": "timestamp",
  "updatedAt": "timestamp"
}
```

### 3.2 Health Record Service

**Responsibility**: FHIR-compliant health record management

**Technology Stack**:
- Runtime: Python with FastAPI
- Database: AWS HealthLake (FHIR)
- Blockchain: Hyperledger Fabric
- Storage: Amazon S3 (medical images)

**APIs**:
```
POST   /api/v1/records
GET    /api/v1/records/{recordId}
GET    /api/v1/records/patient/{patientId}
PUT    /api/v1/records/{recordId}
POST   /api/v1/records/share
POST   /api/v1/records/consent
```

**FHIR Resources**:
- Patient
- Observation
- Condition
- MedicationRequest
- DiagnosticReport
- Immunization
- AllergyIntolerance

### 3.3 AI Agent Orchestrator

**Responsibility**: Coordinate multi-agent system

**Technology Stack**:
- Runtime: Python
- AI Framework: LangChain
- Models: AWS Bedrock (Nova Pro)
- Message Queue: Amazon SQS

**Agent Architecture**:
```python
class AgentOrchestrator:
    def __init__(self):
        self.agents = {
            'triage': TriageAgent(),
            'scheduling': SchedulingAgent(),
            'discharge': DischargePlanningAgent(),
            'forecast': DemandForecastAgent(),
            'er_or': ERORSchedulingAgent()
        }
        self.supervisor = SupervisorAgent()
    
    async def process_request(self, request):
        # Route to appropriate agent
        agent = self.select_agent(request)
        result = await agent.execute(request)
        
        # Supervisor validation
        validated = await self.supervisor.validate(result)
        return validated
```

### 3.4 Triage Agent

**Responsibility**: Real-time patient triage and prioritization

**Technology Stack**:
- Model: AWS Nova Lite
- Deployment: AWS Lambda
- Response Time: <100ms

**Input**:
```json
{
  "patientId": "string",
  "symptoms": ["string"],
  "vitalSigns": {
    "heartRate": "number",
    "bloodPressure": "string",
    "temperature": "number",
    "spO2": "number"
  },
  "medicalHistory": ["string"],
  "images": ["base64"]
}
```

**Output**:
```json
{
  "priority": "CRITICAL|URGENT|STANDARD|NON_URGENT",
  "confidence": "number",
  "reasoning": "string",
  "recommendedActions": ["string"],
  "estimatedWaitTime": "number"
}
```

### 3.5 IoT Service

**Responsibility**: Real-time patient monitoring via IoT devices

**Technology Stack**:
- IoT Platform: AWS IoT Core
- Stream Processing: Amazon Kinesis
- Analytics: AWS IoT Analytics
- Alerts: Amazon SNS

**Device Protocol**: MQTT over TLS

**Data Flow**:
```
IoT Device → AWS IoT Core → Kinesis Stream → Lambda → DynamoDB
                                           ↓
                                    Anomaly Detection → SNS → Alert
```

### 3.6 ABDM Gateway Service

**Responsibility**: Integration with ABDM infrastructure

**Technology Stack**:
- Runtime: Node.js
- APIs: ABDM REST APIs
- Cache: Redis

**Integrations**:
- Health ID Service
- HPR (Healthcare Provider Registry)
- HFR (Health Facility Registry)
- UHI (Unified Health Interface)

**API Mapping**:
```
Swasthya API → ABDM Gateway → ABDM Sandbox/Production
```

---

## 4. Data Design

### 4.1 Database Schema

#### Users Table (PostgreSQL)
```sql
CREATE TABLE users (
    user_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    aadhaar_hash VARCHAR(256) UNIQUE NOT NULL,
    health_id VARCHAR(100) UNIQUE,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) UNIQUE,
    phone VARCHAR(15) UNIQUE NOT NULL,
    date_of_birth DATE NOT NULL,
    gender VARCHAR(20),
    language VARCHAR(10) DEFAULT 'en',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    is_active BOOLEAN DEFAULT TRUE
);

CREATE INDEX idx_users_health_id ON users(health_id);
CREATE INDEX idx_users_phone ON users(phone);
```

#### Health Records (AWS HealthLake - FHIR)
Stored as FHIR R4 resources in AWS HealthLake.

#### IoT Data (DynamoDB)
```json
{
  "deviceId": "string (partition key)",
  "timestamp": "number (sort key)",
  "patientId": "string",
  "measurements": {
    "heartRate": "number",
    "bloodPressure": "string",
    "temperature": "number",
    "spO2": "number",
    "glucose": "number"
  },
  "location": {
    "lat": "number",
    "lon": "number"
  },
  "ttl": "number"
}
```

#### Consent Records (Blockchain)
```json
{
  "consentId": "string",
  "patientId": "string",
  "providerId": "string",
  "purpose": "string",
  "dataTypes": ["string"],
  "grantedAt": "timestamp",
  "expiresAt": "timestamp",
  "revokedAt": "timestamp",
  "status": "ACTIVE|EXPIRED|REVOKED",
  "blockchainTxId": "string"
}
```

### 4.2 Data Flow Diagrams

#### Patient Registration Flow
```
Patient → Mobile App → API Gateway → User Service → Cognito
                                                   ↓
                                            ABDM Gateway → ABDM API
                                                   ↓
                                            Health ID Created
                                                   ↓
                                            Blockchain → Record Consent
```

#### Health Record Access Flow
```
Provider → Dashboard → API Gateway → Auth Check → Consent Check
                                                        ↓
                                                  AWS HealthLake
                                                        ↓
                                                  Return FHIR Data
                                                        ↓
                                                  Log Access (Blockchain)
```

---

## 5. API Design

### 5.1 RESTful API Standards

**Base URL**: `https://api.swasthya.health/v1`

**Authentication**: Bearer token (JWT)

**Request Headers**:
```
Authorization: Bearer <token>
Content-Type: application/json
Accept-Language: en|hi|ta|te|...
X-Request-ID: <uuid>
```

**Response Format**:
```json
{
  "success": true,
  "data": {},
  "error": null,
  "metadata": {
    "requestId": "string",
    "timestamp": "string",
    "version": "string"
  }
}
```

**Error Response**:
```json
{
  "success": false,
  "data": null,
  "error": {
    "code": "ERROR_CODE",
    "message": "Human readable message",
    "details": {}
  },
  "metadata": {
    "requestId": "string",
    "timestamp": "string"
  }
}
```

### 5.2 Key API Endpoints

#### Health Records API
```
POST   /api/v1/records
GET    /api/v1/records/{id}
PUT    /api/v1/records/{id}
DELETE /api/v1/records/{id}
GET    /api/v1/records/patient/{patientId}
POST   /api/v1/records/share
GET    /api/v1/records/shared-with-me
```

#### AI Triage API
```
POST   /api/v1/ai/triage
{
  "patientId": "string",
  "symptoms": ["fever", "cough"],
  "vitalSigns": {
    "temperature": 101.5,
    "heartRate": 95
  }
}

Response:
{
  "priority": "URGENT",
  "confidence": 0.92,
  "reasoning": "High fever with respiratory symptoms",
  "estimatedWaitTime": 15
}
```

#### IoT Monitoring API
```
POST   /api/v1/iot/devices/register
GET    /api/v1/iot/devices/{deviceId}/data
POST   /api/v1/iot/alerts/configure
GET    /api/v1/iot/alerts/history
```

#### ABDM Integration API
```
POST   /api/v1/abdm/health-id/create
POST   /api/v1/abdm/health-id/link
GET    /api/v1/abdm/providers/search
GET    /api/v1/abdm/facilities/search
POST   /api/v1/abdm/consent/request
```

---

## 6. Security Design

### 6.1 Authentication Architecture

```
User → Login Request → API Gateway → AWS Cognito
                                          ↓
                                    Aadhaar Verification
                                          ↓
                                    Generate JWT Token
                                          ↓
                                    Return Token + Refresh Token
```

**JWT Token Structure**:
```json
{
  "sub": "user-id",
  "healthId": "username@abdm",
  "role": "PATIENT|PROVIDER|ADMIN",
  "permissions": ["read:records", "write:records"],
  "iat": 1234567890,
  "exp": 1234571490
}
```

### 6.2 Authorization Model

**Role-Based Access Control (RBAC)**:

| Role | Permissions |
|------|-------------|
| Patient | Read own records, Grant consent, Book appointments |
| Provider | Read patient records (with consent), Write clinical notes, Prescribe |
| Admin | Manage users, View analytics, Configure system |
| Emergency | Read all records (audit logged) |

### 6.3 Data Encryption

**At Rest**:
- AWS KMS for encryption keys
- AES-256 encryption
- Separate keys per tenant
- Automatic key rotation

**In Transit**:
- TLS 1.3 for all communications
- Certificate pinning for mobile apps
- Perfect forward secrecy

**Application Level**:
- Field-level encryption for sensitive data (Aadhaar, medical records)
- Tokenization for payment data

### 6.4 Compliance Architecture

**DISHA Compliance**:
```
┌─────────────────────────────────────────┐
│         Compliance Layer                │
│  ┌────────────────────────────────┐    │
│  │ Consent Management             │    │
│  │ (Blockchain-based)             │    │
│  └────────────────────────────────┘    │
│  ┌────────────────────────────────┐    │
│  │ Audit Logging                  │    │
│  │ (CloudTrail + Blockchain)      │    │
│  └────────────────────────────────┘    │
│  ┌────────────────────────────────┐    │
│  │ Data Anonymization             │    │
│  │ (for analytics)                │    │
│  └────────────────────────────────┘    │
│  ┌────────────────────────────────┐    │
│  │ Access Control                 │    │
│  │ (IAM + Custom RBAC)            │    │
│  └────────────────────────────────┘    │
└─────────────────────────────────────────┘
```

---

## 7. AI/ML Design

### 7.1 Model Architecture

#### Triage Agent (Nova Lite)
```python
class TriageAgent:
    def __init__(self):
        self.model = BedrockClient(model_id="amazon.nova-lite-v1:0")
        self.prompt_template = """
        Analyze the following patient data and provide triage priority:
        
        Symptoms: {symptoms}
        Vital Signs: {vital_signs}
        Medical History: {history}
        
        Provide:
        1. Priority level (CRITICAL/URGENT/STANDARD/NON_URGENT)
        2. Confidence score (0-1)
        3. Reasoning
        4. Recommended actions
        """
    
    async def triage(self, patient_data):
        prompt = self.prompt_template.format(**patient_data)
        response = await self.model.invoke(prompt)
        return self.parse_response(response)
```

#### Discharge Planning Agent (Nova Pro)
```python
class DischargePlanningAgent:
    def __init__(self):
        self.model = BedrockClient(model_id="amazon.nova-pro-v1:0")
        self.canvas = BedrockClient(model_id="amazon.nova-canvas-v1:0")
    
    async def generate_discharge_summary(self, patient_id):
        # Fetch patient data
        records = await self.fetch_records(patient_id)
        
        # Generate text summary
        summary = await self.model.invoke(
            f"Generate discharge summary for: {records}"
        )
        
        # Generate visual medication guide
        visual_guide = await self.canvas.invoke(
            f"Create medication schedule infographic: {summary.medications}"
        )
        
        return {
            "summary": summary,
            "visual_guide": visual_guide
        }
```

### 7.2 Federated Learning Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                   Central Aggregator                         │
│                   (AWS SageMaker)                            │
│                                                              │
│  Global Model ← Aggregate Updates ← Hospital Models         │
└────────────┬────────────────────────────────────────────────┘
             │
    ┌────────┴────────┬────────────────┬────────────────┐
    │                 │                │                │
┌───▼────┐      ┌────▼───┐      ┌────▼───┐      ┌────▼───┐
│Hospital│      │Hospital│      │Hospital│      │Hospital│
│   1    │      │   2    │      │   3    │      │   N    │
│        │      │        │      │        │      │        │
│ Local  │      │ Local  │      │ Local  │      │ Local  │
│Training│      │Training│      │Training│      │Training│
│        │      │        │      │        │      │        │
│ Send   │      │ Send   │      │ Send   │      │ Send   │
│Updates │      │Updates │      │Updates │      │Updates │
│Only    │      │Only    │      │Only    │      │Only    │
└────────┘      └────────┘      └────────┘      └────────┘
```

**Benefits**:
- Patient data never leaves hospital
- DISHA compliant
- Improved model accuracy with diverse data
- Privacy-preserving

---

## 8. Deployment Architecture

### 8.1 AWS Infrastructure

**Regions**: ap-south-1 (Mumbai), ap-south-2 (Hyderabad)

**Availability Zones**: Multi-AZ deployment for high availability

**VPC Architecture**:
```
┌─────────────────────────────────────────────────────────────┐
│                         VPC                                  │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐  │
│  │              Public Subnet (AZ-1)                     │  │
│  │  ┌──────────────┐  ┌──────────────┐                 │  │
│  │  │ NAT Gateway  │  │ Load Balancer│                 │  │
│  │  └──────────────┘  └──────────────┘                 │  │
│  └──────────────────────────────────────────────────────┘  │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐  │
│  │            Private Subnet (AZ-1)                      │  │
│  │  ┌──────────────┐  ┌──────────────┐                 │  │
│  │  │ ECS Services │  │ Lambda       │                 │  │
│  │  └──────────────┘  └──────────────┘                 │  │
│  └──────────────────────────────────────────────────────┘  │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐  │
│  │            Data Subnet (AZ-1)                         │  │
│  │  ┌──────────────┐  ┌──────────────┐                 │  │
│  │  │ RDS          │  │ ElastiCache  │                 │  │
│  │  └──────────────┘  └──────────────┘                 │  │
│  └──────────────────────────────────────────────────────┘  │
│                                                              │
│  [Similar structure for AZ-2]                               │
└─────────────────────────────────────────────────────────────┘
```

### 8.2 Container Orchestration

**Amazon ECS with Fargate**:
- Serverless container management
- Auto-scaling based on CPU/memory
- Blue-green deployments
- Service discovery via AWS Cloud Map

**Service Configuration**:
```yaml
service:
  name: health-record-service
  cluster: swasthya-prod
  taskDefinition: health-record-service:latest
  desiredCount: 3
  launchType: FARGATE
  networkConfiguration:
    subnets:
      - subnet-private-1
      - subnet-private-2
    securityGroups:
      - sg-app-services
  loadBalancers:
    - targetGroupArn: arn:aws:elasticloadbalancing:...
      containerName: health-record-service
      containerPort: 8080
  autoScaling:
    minCapacity: 3
    maxCapacity: 100
    targetCPUUtilization: 70
```

### 8.3 CI/CD Pipeline

```
GitHub → GitHub Actions → Build → Test → Security Scan
                                              ↓
                                        Push to ECR
                                              ↓
                                        Deploy to Dev
                                              ↓
                                        Integration Tests
                                              ↓
                                        Deploy to Staging
                                              ↓
                                        Manual Approval
                                              ↓
                                        Blue-Green Deploy to Prod
                                              ↓
                                        Health Checks
                                              ↓
                                        Switch Traffic
```

---

## 9. Monitoring and Observability

### 9.1 Monitoring Stack

**Metrics**: Amazon CloudWatch
- Application metrics (latency, error rate, throughput)
- Infrastructure metrics (CPU, memory, disk)
- Custom business metrics (triage count, alert rate)

**Logs**: CloudWatch Logs + AWS OpenSearch
- Centralized log aggregation
- Log retention: 90 days
- Real-time log analysis

**Tracing**: AWS X-Ray
- Distributed tracing across microservices
- Performance bottleneck identification
- Service map visualization

**Alerting**: Amazon SNS + PagerDuty
- Critical alerts: <5 min response
- Warning alerts: <30 min response
- Info alerts: Daily digest

### 9.2 Key Metrics

| Metric | Target | Alert Threshold |
|--------|--------|-----------------|
| API Latency (p99) | <500ms | >1000ms |
| Triage Response Time | <100ms | >200ms |
| Error Rate | <0.1% | >1% |
| Availability | 99.9% | <99.5% |
| IoT Message Latency | <1s | >5s |
| Database Connection Pool | <80% | >90% |

---

## 10. Disaster Recovery

### 10.1 Backup Strategy

**RDS Backups**:
- Automated daily backups
- Retention: 35 days
- Cross-region replication to DR region

**S3 Backups**:
- Versioning enabled
- Cross-region replication
- Lifecycle policies for cost optimization

**Blockchain Backups**:
- Distributed ledger (inherently backed up)
- Snapshot exports to S3 daily

### 10.2 Recovery Procedures

**RTO (Recovery Time Objective)**: 1 hour  
**RPO (Recovery Point Objective)**: 0 (zero data loss)

**Disaster Scenarios**:

1. **Single AZ Failure**: Automatic failover to other AZ (<30 seconds)
2. **Region Failure**: Manual failover to DR region (<1 hour)
3. **Data Corruption**: Point-in-time recovery from backups
4. **Security Breach**: Isolate affected systems, restore from clean backups

---

## 11. Cost Optimization

### 11.1 Cost Breakdown (per patient/month)

| Service | Cost (₹) | Optimization Strategy |
|---------|----------|----------------------|
| Compute (Lambda/ECS) | 10 | Serverless, auto-scaling |
| Storage (S3/RDS) | 10 | Lifecycle policies, compression |
| AI/ML (Bedrock) | 5 | Batch processing, caching |
| IoT (IoT Core) | 5 | Message aggregation |
| Networking (CloudFront) | 5 | CDN caching |
| Database (RDS/DynamoDB) | 10 | Reserved instances, on-demand |
| Monitoring | 3 | Log sampling, metric aggregation |
| Other | 2 | Spot instances for batch jobs |
| **Total** | **50** | |

### 11.2 Cost Optimization Techniques

1. **Serverless First**: Use Lambda for variable workloads
2. **Reserved Instances**: 1-year commitment for predictable workloads (40% savings)
3. **Spot Instances**: For ML training (70% savings)
4. **S3 Intelligent Tiering**: Automatic cost optimization
5. **CloudFront Caching**: Reduce origin requests by 80%
6. **DynamoDB On-Demand**: Pay per request for IoT data
7. **Data Compression**: Reduce storage and transfer costs
8. **Right-Sizing**: Continuous monitoring and adjustment

---

## 12. Technology Stack Summary

### 12.1 Frontend
- **Mobile**: React Native (iOS/Android)
- **Web**: React + TypeScript
- **State Management**: Redux Toolkit
- **UI Framework**: Material-UI
- **PWA**: Workbox for offline support

### 12.2 Backend
- **API Gateway**: Amazon API Gateway
- **Microservices**: Node.js (Express), Python (FastAPI)
- **Container Runtime**: Docker
- **Orchestration**: Amazon ECS with Fargate
- **Serverless**: AWS Lambda

### 12.3 Data
- **FHIR Store**: AWS HealthLake
- **Relational DB**: Amazon RDS (PostgreSQL)
- **NoSQL DB**: Amazon DynamoDB
- **Cache**: Amazon ElastiCache (Redis)
- **Object Storage**: Amazon S3
- **Blockchain**: Hyperledger Fabric

### 12.4 AI/ML
- **LLM Platform**: Amazon Bedrock
- **Models**: Nova Pro, Nova Lite, Nova Micro, Nova Canvas, Nova Reel
- **ML Training**: Amazon SageMaker
- **Medical NLP**: Amazon Comprehend Medical
- **OCR**: Amazon Textract

### 12.5 IoT
- **IoT Platform**: AWS IoT Core
- **Protocol**: MQTT over TLS
- **Stream Processing**: Amazon Kinesis
- **Analytics**: AWS IoT Analytics

### 12.6 Security
- **Authentication**: AWS Cognito
- **Encryption**: AWS KMS
- **Secrets**: AWS Secrets Manager
- **WAF**: AWS WAF
- **DDoS Protection**: AWS Shield

### 12.7 DevOps
- **CI/CD**: GitHub Actions
- **Container Registry**: Amazon ECR
- **IaC**: AWS CDK (TypeScript)
- **Monitoring**: CloudWatch, X-Ray
- **Logging**: CloudWatch Logs, OpenSearch

---

## 13. Appendices

### Appendix A: Sequence Diagrams
Detailed interaction flows for key use cases.

### Appendix B: Class Diagrams
Object-oriented design for core components.

### Appendix C: Database ER Diagrams
Entity-relationship models for relational data.

### Appendix D: API Specifications
Complete OpenAPI 3.0 specifications.

---

**Document Control**

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2026-02-15 | Swasthya Team | Initial design |

**Approval**

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Technical Architect | | | |
| Security Architect | | | |
| DevOps Lead | | | |

---

**End of Design Document**
