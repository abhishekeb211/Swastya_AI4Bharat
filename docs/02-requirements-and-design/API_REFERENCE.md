# Swasthya - API Reference

**Version:** 1.0  
**Base URL:** `https://api.swasthya.health/v1`  
**Authentication:** Bearer Token (JWT)

---

## Authentication

### POST /auth/register
Register a new user

**Request:**
```json
{
  "phone": "+919876543210",
  "name": "John Doe",
  "dateOfBirth": "1990-01-01",
  "gender": "male"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "userId": "uuid",
    "phone": "+919876543210",
    "otpSent": true
  }
}
```

### POST /auth/verify-otp
Verify OTP and complete registration

### POST /auth/login
Login with phone and password

### POST /auth/refresh
Refresh access token

---

## Health Records

### POST /records
Create a new health record

### GET /records/{id}
Get health record by ID

### GET /records/patient/{patientId}
Get all records for a patient

### POST /records/share
Share record with provider

### POST /records/consent
Manage consent for data sharing

---

## AI Agents

### POST /ai/triage
Perform patient triage

**Request:**
```json
{
  "patientId": "uuid",
  "symptoms": ["fever", "cough"],
  "vitalSigns": {
    "temperature": 101.5,
    "heartRate": 95,
    "spO2": 96
  }
}
```

**Response:**
```json
{
  "priority": "URGENT",
  "confidence": 0.92,
  "reasoning": "High fever with respiratory symptoms",
  "estimatedWaitTime": 15
}
```

### POST /ai/discharge-summary
Generate discharge summary

### POST /ai/schedule
Generate staff schedule

### GET /ai/forecast
Get demand forecast

---

## ABDM Integration

### POST /abdm/health-id/create
Create ABDM Health ID

### POST /abdm/health-id/link
Link existing Health ID

### GET /abdm/providers/search
Search healthcare providers

### GET /abdm/facilities/search
Search health facilities

---

## IoT Monitoring

### POST /iot/devices/register
Register IoT device

### GET /iot/devices/{deviceId}/data
Get device data

### POST /iot/alerts/configure
Configure alert thresholds

---

For complete API documentation, see: https://api.swasthya.health/docs
