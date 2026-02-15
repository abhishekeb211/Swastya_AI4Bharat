# Swasthya - Deployment Guide

**Version:** 1.0  
**Date:** February 15, 2026  
**Target:** AWS Cloud Deployment

---

## Prerequisites

### AWS Account Setup
- AWS Account with admin access
- AWS CLI installed and configured
- Terraform or AWS CDK installed
- Docker installed locally

### Required AWS Services
- Amazon ECS (Fargate)
- Amazon RDS (PostgreSQL)
- AWS HealthLake
- Amazon S3
- Amazon CloudFront
- AWS IoT Core
- Amazon ElastiCache (Redis)
- AWS Cognito
- AWS KMS
- Amazon CloudWatch

---

## Quick Start

### 1. Clone Repository
```bash
git clone https://github.com/swasthya/swasthya-platform
cd swasthya-platform
```

### 2. Configure Environment
```bash
cp .env.example .env
# Edit .env with your AWS credentials and configuration
```

### 3. Deploy Infrastructure
```bash
cd infrastructure
terraform init
terraform plan
terraform apply
```

### 4. Deploy Services
```bash
# Build and push Docker images
./scripts/build-and-push.sh

# Deploy to ECS
./scripts/deploy.sh production
```

### 5. Verify Deployment
```bash
# Check service health
curl https://api.swasthya.health/health

# Run smoke tests
npm run test:smoke
```

---

## Detailed Deployment Steps

### Phase 1: Infrastructure Setup

#### 1.1 VPC Configuration
```bash
terraform apply -target=module.vpc
```

#### 1.2 Database Setup
```bash
terraform apply -target=module.rds
# Run migrations
npm run migrate:prod
```

#### 1.3 ECS Cluster
```bash
terraform apply -target=module.ecs
```

### Phase 2: Service Deployment

#### 2.1 User Service
```bash
cd services/user-service
docker build -t swasthya/user-service .
docker push swasthya/user-service
aws ecs update-service --cluster swasthya-prod --service user-service --force-new-deployment
```

#### 2.2 Health Record Service
```bash
cd services/health-record-service
docker build -t swasthya/health-record-service .
docker push swasthya/health-record-service
aws ecs update-service --cluster swasthya-prod --service health-record-service --force-new-deployment
```

#### 2.3 AI Agent Services
```bash
cd services/ai-agents
docker build -t swasthya/ai-agents .
docker push swasthya/ai-agents
aws ecs update-service --cluster swasthya-prod --service ai-agents --force-new-deployment
```

### Phase 3: Frontend Deployment

#### 3.1 Web Dashboard
```bash
cd frontend/web-dashboard
npm run build
aws s3 sync build/ s3://swasthya-web-dashboard
aws cloudfront create-invalidation --distribution-id XXXXX --paths "/*"
```

#### 3.2 Mobile App
```bash
cd mobile/patient-app
# iOS
cd ios && pod install
xcodebuild -workspace Swasthya.xcworkspace -scheme Swasthya -configuration Release
# Upload to App Store Connect

# Android
cd android
./gradlew assembleRelease
# Upload to Play Console
```

---

## Configuration

### Environment Variables

```bash
# AWS Configuration
AWS_REGION=ap-south-1
AWS_ACCOUNT_ID=123456789012

# Database
DB_HOST=swasthya-prod.xxxxx.ap-south-1.rds.amazonaws.com
DB_PORT=5432
DB_NAME=swasthya
DB_USER=swasthya_admin
DB_PASSWORD=<secret>

# Redis
REDIS_HOST=swasthya-prod.xxxxx.cache.amazonaws.com
REDIS_PORT=6379

# ABDM
ABDM_CLIENT_ID=<client_id>
ABDM_CLIENT_SECRET=<secret>
ABDM_BASE_URL=https://sandbox.abdm.gov.in

# JWT
JWT_SECRET=<secret>
JWT_EXPIRY=3600

# AWS Services
HEALTHLAKE_DATASTORE_ID=<datastore_id>
IOT_ENDPOINT=<iot_endpoint>
S3_BUCKET=swasthya-medical-records
```

---

## Monitoring

### CloudWatch Dashboards
- System Overview: https://console.aws.amazon.com/cloudwatch/dashboards/swasthya-overview
- API Performance: https://console.aws.amazon.com/cloudwatch/dashboards/swasthya-api
- IoT Metrics: https://console.aws.amazon.com/cloudwatch/dashboards/swasthya-iot

### Alarms
- High error rate (>1%)
- High latency (>1s p95)
- Low availability (<99.5%)
- High cost (>budget)

---

## Backup & Recovery

### Automated Backups
- RDS: Daily automated backups (35-day retention)
- S3: Versioning enabled, cross-region replication
- DynamoDB: Point-in-time recovery enabled

### Disaster Recovery
- RTO: 1 hour
- RPO: 0 (zero data loss)
- DR Region: ap-south-2 (Hyderabad)

---

## Security

### Encryption
- At rest: AWS KMS (AES-256)
- In transit: TLS 1.3
- Database: Encrypted with KMS

### Access Control
- IAM roles for services
- Cognito for user authentication
- RBAC for authorization

### Compliance
- DISHA compliant
- ABDM certified
- ISO 27001 aligned

---

## Troubleshooting

### Common Issues

#### Service Not Starting
```bash
# Check ECS task logs
aws logs tail /ecs/swasthya/user-service --follow

# Check service events
aws ecs describe-services --cluster swasthya-prod --services user-service
```

#### Database Connection Issues
```bash
# Test database connectivity
psql -h $DB_HOST -U $DB_USER -d $DB_NAME

# Check security groups
aws ec2 describe-security-groups --group-ids sg-xxxxx
```

#### High Latency
```bash
# Check CloudWatch metrics
aws cloudwatch get-metric-statistics --namespace AWS/ECS --metric-name CPUUtilization

# Check X-Ray traces
aws xray get-trace-summaries --start-time $(date -u -d '1 hour ago' +%s) --end-time $(date +%s)
```

---

## Scaling

### Auto-Scaling Configuration
```hcl
resource "aws_appautoscaling_target" "ecs_target" {
  max_capacity       = 100
  min_capacity       = 3
  resource_id        = "service/swasthya-prod/user-service"
  scalable_dimension = "ecs:service:DesiredCount"
  service_namespace  = "ecs"
}

resource "aws_appautoscaling_policy" "ecs_policy" {
  name               = "cpu-autoscaling"
  policy_type        = "TargetTrackingScaling"
  resource_id        = aws_appautoscaling_target.ecs_target.resource_id
  scalable_dimension = aws_appautoscaling_target.ecs_target.scalable_dimension
  service_namespace  = aws_appautoscaling_target.ecs_target.service_namespace

  target_tracking_scaling_policy_configuration {
    target_value       = 70.0
    predefined_metric_specification {
      predefined_metric_type = "ECSServiceAverageCPUUtilization"
    }
  }
}
```

---

## Cost Optimization

### Reserved Instances
- RDS: 1-year reserved instance (40% savings)
- ElastiCache: 1-year reserved instance (40% savings)

### Spot Instances
- Batch processing: Use Spot instances (70% savings)

### S3 Lifecycle Policies
- Move to IA after 30 days
- Move to Glacier after 90 days

---

## Support

### Documentation
- Architecture: ARCHITECTURE.md
- API Reference: API_REFERENCE.md
- Runbooks: docs/runbooks/

### Contact
- Technical Support: support@swasthya.health
- Emergency: +91-XXXX-XXXXXX (24/7)
- Slack: swasthya-support.slack.com

---

**End of Deployment Guide**
