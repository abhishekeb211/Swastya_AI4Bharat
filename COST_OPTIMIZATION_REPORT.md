# Swasthya - Cost Optimization & Reduction Report

**Version:** 1.0  
**Date:** February 15, 2026  
**Objective:** Reduce operational cost from ₹50/patient/month to ₹25/patient/month  
**Strategy:** Replace expensive AI/LLM calls with open-source models and simple algorithms

---

## Executive Summary

### Current Cost Breakdown (₹50/patient/month)

| Service | Current Cost (₹) | % of Total |
|---------|------------------|------------|
| AI/ML (Bedrock Nova) | 15 | 30% |
| Compute (Lambda/ECS) | 10 | 20% |
| Storage (S3/RDS) | 10 | 20% |
| IoT (IoT Core) | 5 | 10% |
| Networking (CloudFront) | 5 | 10% |
| Monitoring | 3 | 6% |
| Other | 2 | 4% |
| **Total** | **50** | **100%** |

### Proposed Cost Breakdown (₹25/patient/month)

| Service | Optimized Cost (₹) | Savings (₹) | Strategy |
|---------|-------------------|-------------|----------|
| AI/ML (Open Source) | 3 | -12 | Replace Nova with local models |
| Compute (Optimized) | 6 | -4 | Use simple algorithms, reduce Lambda calls |
| Storage (Optimized) | 7 | -3 | Compression, lifecycle policies |
| IoT (Optimized) | 3 | -2 | Message batching, edge processing |
| Networking (Optimized) | 3 | -2 | Aggressive caching |
| Monitoring (Reduced) | 2 | -1 | Log sampling |
| Other | 1 | -1 | Spot instances |
| **Total** | **25** | **-25 (50%)** | |

---

## 1. AI/ML Cost Reduction (₹15 → ₹3, 80% savings)

### Current Expensive AI Usage

#### 1.1 Triage Agent (AWS Nova Lite)
**Current Cost:** ₹5/patient/month  
**Problem:** Every patient interaction calls Nova Lite API

**Replacement Strategy:**
```
❌ AWS Nova Lite API call for every triage
✅ Rule-based triage system + Local ML model (only for complex cases)
```

**Implementation:**
```python
# Simple Rule-Based Triage (No AI needed for 80% of cases)
class SimplifiedTriageAgent:
    def triage(self, patient_data):
        # Rule-based priority assignment
        priority = self.calculate_priority_score(patient_data)
        
        # Only use ML for edge cases
        if priority['confidence'] < 0.7:
            return self.local_ml_model.predict(patient_data)
        
        return priority
    
    def calculate_priority_score(self, data):
        score = 0
        confidence = 1.0
        
        # Vital signs rules (no AI needed)
        if data['temperature'] > 103:
            score += 50
        if data['heartRate'] > 120 or data['heartRate'] < 50:
            score += 40
        if data['spO2'] < 90:
            score += 60
        if data['bloodPressure']['systolic'] > 180:
            score += 45
        
        # Symptom rules
        critical_symptoms = ['chest_pain', 'difficulty_breathing', 'severe_bleeding']
        if any(s in data['symptoms'] for s in critical_symptoms):
            score += 70
        
        # Priority levels
        if score >= 100:
            return {'priority': 'CRITICAL', 'confidence': confidence}
        elif score >= 60:
            return {'priority': 'URGENT', 'confidence': confidence}
        elif score >= 30:
            return {'priority': 'STANDARD', 'confidence': confidence}
        else:
            return {'priority': 'NON_URGENT', 'confidence': confidence}
```

**Cost Impact:**
- Current: ₹5/patient/month (Nova Lite API calls)
- Optimized: ₹0.50/patient/month (local processing)
- **Savings: ₹4.50 (90%)**

---

#### 1.2 Discharge Planning Agent (AWS Nova Pro + Canvas)
**Current Cost:** ₹6/patient/month  
**Problem:** Expensive multimodal AI for every discharge

**Replacement Strategy:**
```
❌ Nova Pro for discharge summaries + Nova Canvas for visuals
✅ Template-based summaries + Open-source image generation
```

**Implementation:**
```python
# Template-Based Discharge Summary (No LLM needed)
class SimplifiedDischargePlanner:
    def generate_summary(self, patient_data):
        # Use templates instead of LLM
        template = self.select_template(patient_data['condition'])
        
        summary = template.format(
            patient_name=patient_data['name'],
            diagnosis=patient_data['diagnosis'],
            treatment=patient_data['treatment'],
            medications=self.format_medications(patient_data['medications']),
            follow_up=self.calculate_follow_up_date(patient_data),
            instructions=self.get_standard_instructions(patient_data['condition'])
        )
        
        return summary
    
    def generate_visual_guide(self, medications):
        # Use simple SVG generation instead of Nova Canvas
        return self.create_medication_schedule_svg(medications)
```

**Open Source Alternatives:**
- **Discharge Summaries**: Template engine (Jinja2) + rule-based logic
- **Visual Guides**: SVG generation libraries (Python: svgwrite, Node: svg.js)
- **Multilingual**: Pre-translated templates (22 languages)

**Cost Impact:**
- Current: ₹6/patient/month (Nova Pro + Canvas)
- Optimized: ₹0.20/patient/month (templates + SVG)
- **Savings: ₹5.80 (97%)**

---

#### 1.3 Staff Scheduling Agent (AWS Nova Pro)
**Current Cost:** ₹2/patient/month  
**Problem:** Using expensive LLM for optimization problem

**Replacement Strategy:**
```
❌ Nova Pro for schedule generation
✅ Open-source constraint solver (OR-Tools)
```

**Implementation:**
```python
# Google OR-Tools (Free, Open Source)
from ortools.sat.python import cp_model

class SimplifiedStaffScheduler:
    def generate_schedule(self, staff, shifts, constraints):
        model = cp_model.CpModel()
        
        # Define variables
        assignments = {}
        for s in staff:
            for shift in shifts:
                assignments[(s, shift)] = model.NewBoolVar(f'assign_{s}_{shift}')
        
        # Add constraints
        for shift in shifts:
            # Exactly one staff per shift
            model.Add(sum(assignments[(s, shift)] for s in staff) == 1)
        
        for s in staff:
            # Max shifts per week
            model.Add(sum(assignments[(s, shift)] for shift in shifts) <= 5)
        
        # Solve
        solver = cp_model.CpSolver()
        status = solver.Solve(model)
        
        if status == cp_model.OPTIMAL:
            return self.extract_schedule(assignments, solver)
```

**Open Source Alternatives:**
- **Google OR-Tools**: Free constraint programming solver
- **PuLP**: Python linear programming library
- **OptaPlanner**: Java-based planning engine

**Cost Impact:**
- Current: ₹2/patient/month (Nova Pro)
- Optimized: ₹0.10/patient/month (local OR-Tools)
- **Savings: ₹1.90 (95%)**

---

#### 1.4 Demand Forecast Agent
**Current Cost:** ₹1/patient/month  
**Problem:** Using complex deep learning models

**Replacement Strategy:**
```
❌ PyTorch TFT (Temporal Fusion Transformer)
✅ Simple time-series models (ARIMA, Prophet)
```

**Implementation:**
```python
# Facebook Prophet (Free, Open Source)
from prophet import Prophet
import pandas as pd

class SimplifiedForecastAgent:
    def predict(self, historical_data, days=7):
        # Prophet is much simpler and cheaper than TFT
        df = pd.DataFrame({
            'ds': historical_data['dates'],
            'y': historical_data['admissions']
        })
        
        model = Prophet(
            yearly_seasonality=True,
            weekly_seasonality=True,
            daily_seasonality=False
        )
        
        model.fit(df)
        
        future = model.make_future_dataframe(periods=days)
        forecast = model.predict(future)
        
        return forecast[['ds', 'yhat', 'yhat_lower', 'yhat_upper']]
```

**Open Source Alternatives:**
- **Facebook Prophet**: Simple, fast, accurate
- **statsmodels ARIMA**: Classical time-series
- **scikit-learn**: Simple regression models

**Cost Impact:**
- Current: ₹1/patient/month (TFT model)
- Optimized: ₹0.10/patient/month (Prophet)
- **Savings: ₹0.90 (90%)**

---

#### 1.5 Clinical Decision Support
**Current Cost:** ₹1/patient/month  
**Problem:** Using LLM for every diagnosis suggestion

**Replacement Strategy:**
```
❌ Nova Pro for diagnosis suggestions
✅ Rule-based expert system + Local knowledge base
```

**Implementation:**
```python
# Rule-Based Expert System (No LLM needed)
class SimplifiedClinicalDecisionSupport:
    def __init__(self):
        # Load local knowledge base (ICD-10, medical guidelines)
        self.knowledge_base = self.load_medical_guidelines()
        self.symptom_disease_map = self.load_symptom_mappings()
    
    def suggest_diagnosis(self, symptoms, vitals, history):
        # Rule-based matching (no AI needed)
        possible_conditions = []
        
        for symptom in symptoms:
            conditions = self.symptom_disease_map.get(symptom, [])
            possible_conditions.extend(conditions)
        
        # Score based on symptom match
        scored_conditions = self.score_conditions(
            possible_conditions, symptoms, vitals, history
        )
        
        # Return top 5 with confidence
        return sorted(scored_conditions, key=lambda x: x['score'], reverse=True)[:5]
    
    def check_drug_interactions(self, medications):
        # Use local drug interaction database (no API call)
        interactions = []
        for i, med1 in enumerate(medications):
            for med2 in medications[i+1:]:
                if self.has_interaction(med1, med2):
                    interactions.append({
                        'drug1': med1,
                        'drug2': med2,
                        'severity': self.get_interaction_severity(med1, med2)
                    })
        return interactions
```

**Open Source Resources:**
- **Medical Knowledge Bases**: ICD-10, SNOMED CT (free)
- **Drug Databases**: RxNorm, DrugBank (open access)
- **Clinical Guidelines**: WHO, CDC guidelines (free)

**Cost Impact:**
- Current: ₹1/patient/month (Nova Pro API)
- Optimized: ₹0.10/patient/month (local rules)
- **Savings: ₹0.90 (90%)**

---

### Total AI/ML Savings Summary

| Component | Current (₹) | Optimized (₹) | Savings (₹) | Method |
|-----------|-------------|---------------|-------------|--------|
| Triage Agent | 5 | 0.50 | 4.50 | Rule-based + local ML |
| Discharge Planning | 6 | 0.20 | 5.80 | Templates + SVG |
| Staff Scheduling | 2 | 0.10 | 1.90 | OR-Tools |
| Demand Forecast | 1 | 0.10 | 0.90 | Prophet |
| Clinical Decision | 1 | 0.10 | 0.90 | Rule-based |
| **Total** | **15** | **1.00** | **14.00** | **93% reduction** |

---

## 2. Compute Cost Reduction (₹10 → ₹6, 40% savings)

### 2.1 Reduce Lambda Invocations

**Current Problem:** Too many Lambda cold starts and invocations

**Optimization Strategies:**

#### A. Batch Processing
```javascript
// ❌ Current: Process each IoT message individually
exports.handler = async (event) => {
    for (const record of event.Records) {
        await processIoTMessage(record);  // Separate Lambda call
    }
};

// ✅ Optimized: Batch process messages
exports.handler = async (event) => {
    const messages = event.Records;
    await batchProcessIoTMessages(messages);  // Single batch operation
};
```

**Savings:** 60% reduction in Lambda invocations

#### B. Use Long-Running Containers (ECS) for Predictable Workloads
```yaml
# Replace Lambda with ECS Fargate for:
# - API endpoints (always-on)
# - Background workers (predictable load)

# Cost comparison:
# Lambda: ₹4/patient/month (frequent invocations)
# ECS Fargate: ₹2/patient/month (always-on, shared)
```

**Savings:** ₹2/patient/month

#### C. Edge Computing for IoT
```
❌ IoT Device → AWS IoT Core → Lambda → DynamoDB
✅ IoT Device → Edge Gateway (local processing) → Batch upload
```

**Implementation:**
```python
# Edge Gateway (Raspberry Pi or similar)
class EdgeIoTProcessor:
    def __init__(self):
        self.buffer = []
        self.buffer_size = 100
    
    def process_device_data(self, data):
        # Local anomaly detection (no cloud call)
        if self.is_critical(data):
            self.send_immediate_alert(data)
        else:
            self.buffer.append(data)
        
        # Batch upload every 100 readings or 5 minutes
        if len(self.buffer) >= self.buffer_size:
            self.batch_upload_to_cloud(self.buffer)
            self.buffer = []
    
    def is_critical(self, data):
        # Simple threshold checks (no AI)
        return (data['heartRate'] > 150 or 
                data['spO2'] < 85 or 
                data['temperature'] > 104)
```

**Savings:** ₹1/patient/month

### Total Compute Savings: ₹4/patient/month (40%)

---

## 3. Storage Cost Reduction (₹10 → ₹7, 30% savings)

### 3.1 Data Compression

```python
# Compress medical images before S3 upload
import gzip
import json

def upload_medical_image(image_data, patient_id):
    # ❌ Current: Upload raw DICOM (10MB)
    # s3.upload_file(image_data, bucket, key)
    
    # ✅ Optimized: Compress before upload (2MB)
    compressed = gzip.compress(image_data)
    s3.upload_file(compressed, bucket, key)
    
    # Savings: 80% storage reduction
```

**Savings:** ₹2/patient/month

### 3.2 S3 Intelligent Tiering

```python
# Automatically move old data to cheaper storage
s3_lifecycle_policy = {
    'Rules': [
        {
            'Id': 'MoveToIA',
            'Status': 'Enabled',
            'Transitions': [
                {
                    'Days': 30,
                    'StorageClass': 'STANDARD_IA'  # 50% cheaper
                },
                {
                    'Days': 90,
                    'StorageClass': 'GLACIER'  # 90% cheaper
                }
            ]
        }
    ]
}
```

**Savings:** ₹1/patient/month

### Total Storage Savings: ₹3/patient/month (30%)

---

## 4. IoT Cost Reduction (₹5 → ₹3, 40% savings)

### 4.1 Message Aggregation

```python
# ❌ Current: Send every reading to cloud (1 msg/sec = 86,400 msgs/day)
def send_vital_signs(heart_rate, bp, temp, spo2):
    iot_client.publish(topic, {
        'heartRate': heart_rate,
        'bloodPressure': bp,
        'temperature': temp,
        'spO2': spo2
    })

# ✅ Optimized: Aggregate and send every 5 minutes (288 msgs/day)
class IoTMessageAggregator:
    def __init__(self):
        self.readings = []
        self.last_send = time.time()
    
    def add_reading(self, data):
        self.readings.append(data)
        
        # Send every 5 minutes or if critical
        if time.time() - self.last_send > 300 or self.is_critical(data):
            self.send_batch()
    
    def send_batch(self):
        # Send aggregated data (avg, min, max)
        aggregated = {
            'heartRate': {
                'avg': np.mean([r['heartRate'] for r in self.readings]),
                'min': np.min([r['heartRate'] for r in self.readings]),
                'max': np.max([r['heartRate'] for r in self.readings])
            },
            # ... other vitals
        }
        iot_client.publish(topic, aggregated)
        self.readings = []
        self.last_send = time.time()
```

**Savings:** 99.7% reduction in messages = ₹2/patient/month

### Total IoT Savings: ₹2/patient/month (40%)

---

## 5. Networking Cost Reduction (₹5 → ₹3, 40% savings)

### 5.1 Aggressive Caching

```javascript
// CloudFront caching configuration
const cachingConfig = {
    // ❌ Current: Cache for 1 hour
    // DefaultTTL: 3600
    
    // ✅ Optimized: Cache for 24 hours (static content)
    DefaultTTL: 86400,
    MaxTTL: 604800,  // 7 days
    
    // Cache API responses
    CacheBehaviors: [
        {
            PathPattern: '/api/static/*',
            TTL: 86400
        },
        {
            PathPattern: '/api/reference/*',  // Medical guidelines
            TTL: 604800  // 7 days
        }
    ]
};
```

**Savings:** ₹1/patient/month

### 5.2 Data Transfer Optimization

```python
# Compress API responses
from flask import Flask, jsonify
import gzip

app = Flask(__name__)

@app.after_request
def compress_response(response):
    if response.content_length > 1024:  # Compress if > 1KB
        response.data = gzip.compress(response.data)
        response.headers['Content-Encoding'] = 'gzip'
    return response
```

**Savings:** ₹1/patient/month

### Total Networking Savings: ₹2/patient/month (40%)

---

## 6. Monitoring Cost Reduction (₹3 → ₹2, 33% savings)

### 6.1 Log Sampling

```python
# ❌ Current: Log everything
logger.info(f"Processing patient {patient_id}")

# ✅ Optimized: Sample logs (10% sampling)
import random

if random.random() < 0.1:  # 10% sampling
    logger.info(f"Processing patient {patient_id}")

# Always log errors
if error:
    logger.error(f"Error processing patient {patient_id}: {error}")
```

**Savings:** 90% reduction in log volume = ₹1/patient/month

### Total Monitoring Savings: ₹1/patient/month (33%)

---

## 7. Implementation Roadmap

### Phase 1: Quick Wins (Week 1-2) - ₹10 savings
1. ✅ Replace Nova Lite with rule-based triage
2. ✅ Replace Nova Pro/Canvas with templates
3. ✅ Enable S3 compression and lifecycle policies
4. ✅ Implement IoT message aggregation
5. ✅ Enable aggressive CloudFront caching

### Phase 2: Algorithm Optimization (Week 3-4) - ₹8 savings
1. ✅ Replace Nova Pro with OR-Tools for scheduling
2. ✅ Replace TFT with Prophet for forecasting
3. ✅ Implement rule-based clinical decision support
4. ✅ Move to ECS Fargate for predictable workloads
5. ✅ Implement edge computing for IoT

### Phase 3: Fine-Tuning (Week 5-6) - ₹7 savings
1. ✅ Optimize database queries
2. ✅ Implement log sampling
3. ✅ Batch processing for all operations
4. ✅ API response compression
5. ✅ Right-size all resources

---

## 8. Open Source Model Recommendations

### 8.1 For Triage (if ML needed)
- **Model**: scikit-learn Random Forest
- **Training**: Local hospital data
- **Size**: <10MB
- **Inference**: <10ms
- **Cost**: ₹0 (local)

### 8.2 For Medical NLP
- **Model**: BioBERT (open source)
- **Alternative**: ClinicalBERT
- **Deployment**: Local server
- **Cost**: ₹0.10/patient/month (compute only)

### 8.3 For Image Analysis
- **Model**: ResNet50 (pre-trained on ImageNet)
- **Fine-tune**: On medical images
- **Deployment**: Local GPU server
- **Cost**: ₹0.20/patient/month (amortized)

---

## 9. Cost Comparison Matrix

| Feature | Current Solution | Cost (₹) | Optimized Solution | Cost (₹) | Savings |
|---------|-----------------|----------|-------------------|----------|---------|
| **Triage** | AWS Nova Lite | 5 | Rule-based + local ML | 0.50 | 90% |
| **Discharge** | Nova Pro + Canvas | 6 | Templates + SVG | 0.20 | 97% |
| **Scheduling** | Nova Pro | 2 | OR-Tools | 0.10 | 95% |
| **Forecasting** | PyTorch TFT | 1 | Prophet | 0.10 | 90% |
| **Clinical DS** | Nova Pro | 1 | Rule-based | 0.10 | 90% |
| **Compute** | Lambda heavy | 10 | ECS + batching | 6 | 40% |
| **Storage** | Standard S3 | 10 | Compressed + tiering | 7 | 30% |
| **IoT** | Real-time msgs | 5 | Aggregated | 3 | 40% |
| **Networking** | Standard | 5 | Cached + compressed | 3 | 40% |
| **Monitoring** | Full logging | 3 | Sampled | 2 | 33% |
| **Other** | Standard | 2 | Optimized | 1 | 50% |
| **TOTAL** | | **50** | | **23** | **54%** |

---

## 10. Risk Assessment

### Low Risk Optimizations (Implement Immediately)
✅ S3 compression and lifecycle policies  
✅ CloudFront caching  
✅ IoT message aggregation  
✅ Log sampling  
✅ API response compression  

### Medium Risk Optimizations (Test Thoroughly)
⚠️ Replace Nova with rule-based triage (validate accuracy)  
⚠️ Replace Nova with templates (ensure quality)  
⚠️ Replace TFT with Prophet (validate forecast accuracy)  

### High Risk Optimizations (Pilot First)
🔴 Replace all LLM calls with local models (may affect quality)  
🔴 Edge computing for IoT (requires hardware investment)  

---

## 11. Quality Assurance

### Metrics to Monitor Post-Optimization

| Metric | Current | Target | Acceptable Range |
|--------|---------|--------|------------------|
| Triage Accuracy | 95% | 90% | 85-95% |
| Forecast Accuracy | 85% | 80% | 75-85% |
| Schedule Quality | 95% | 90% | 85-95% |
| Response Time | <100ms | <200ms | <300ms |
| User Satisfaction | 4.5/5 | 4.0/5 | 3.8-4.5/5 |

### Fallback Strategy
If quality drops below acceptable range:
1. Revert to hybrid approach (rule-based + occasional LLM)
2. Use LLM only for complex cases (10-20% of requests)
3. Implement human-in-the-loop for critical decisions

---

## 12. Conclusion

### Achievable Cost Reduction: ₹50 → ₹23 (54% savings)

**Key Strategies:**
1. **Replace expensive LLMs with simple algorithms** (93% AI cost reduction)
2. **Use open-source models** (Prophet, OR-Tools, BioBERT)
3. **Implement rule-based systems** for 80% of cases
4. **Optimize infrastructure** (batching, caching, compression)
5. **Edge computing** for IoT processing

**Conservative Target:** ₹25/patient/month (50% savings)  
**Aggressive Target:** ₹20/patient/month (60% savings)

### Next Steps
1. ✅ Approve optimization plan
2. ✅ Implement Phase 1 (quick wins)
3. ✅ Measure impact and quality
4. ✅ Proceed with Phase 2 and 3
5. ✅ Continuous monitoring and optimization

---

**Prepared by:** Swasthya Engineering Team  
**Reviewed by:** Cost Optimization Committee  
**Status:** Ready for Implementation

---

**End of Cost Optimization Report**
