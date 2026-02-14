# Swasthya - Pilot Deployment Plan

**Version:** 1.0  
**Date:** February 15, 2026  
**Duration:** 6 weeks (Weeks 15-20)  
**Target:** 5 pilot hospitals, 1000+ patients

---

## Executive Summary

This document outlines the detailed plan for deploying Swasthya to 5 pilot hospitals across India. The pilot will validate the system's functionality, cost-effectiveness, and clinical impact before national rollout.

**Objectives:**
- Validate system functionality in real-world settings
- Achieve cost target of ₹23/patient/month
- Demonstrate clinical value (triage accuracy >90%)
- Collect user feedback for improvements
- Build case studies for national rollout

---

## Pilot Hospital Selection

### Selection Criteria

#### Must-Have Criteria
1. **Bed Capacity:** 50-500 beds
2. **Internet Connectivity:** Minimum 10 Mbps, backup connection
3. **ABDM Readiness:** Registered with ABDM or willing to register
4. **IT Infrastructure:** Basic computers, network infrastructure
5. **Management Support:** Hospital administration committed to pilot
6. **Staff Availability:** Willing to dedicate staff for training

#### Nice-to-Have Criteria
1. Existing EMR/HIS system (for integration testing)
2. IoT device infrastructure
3. Previous digital health initiatives
4. Research collaboration interest
5. Teaching hospital (for medical validation)

### Selected Pilot Hospitals

#### Hospital 1: Urban Tertiary Care (Delhi)
**Name:** AIIMS Delhi (Hypothetical Partnership)  
**Type:** Government tertiary care teaching hospital  
**Beds:** 500  
**Location:** New Delhi  
**Rationale:** 
- High patient volume for testing scalability
- Medical expertise for clinical validation
- Teaching hospital for training and research
- ABDM integration already in place

**Key Contacts:**
- Hospital Director: [Name]
- IT Head: [Name]
- Medical Superintendent: [Name]

#### Hospital 2: Urban Private (Mumbai)
**Name:** Lilavati Hospital (Hypothetical Partnership)  
**Type:** Private multi-specialty hospital  
**Beds:** 300  
**Location:** Mumbai, Maharashtra  
**Rationale:**
- Private sector validation
- Advanced IT infrastructure
- High-paying patients (cost validation)
- Strong management support

**Key Contacts:**
- CEO: [Name]
- CTO: [Name]
- Medical Director: [Name]

#### Hospital 3: Semi-Urban Government (Jaipur)
**Name:** SMS Hospital Jaipur (Hypothetical Partnership)  
**Type:** Government medical college hospital  
**Beds:** 400  
**Location:** Jaipur, Rajasthan  
**Rationale:**
- Semi-urban setting
- Mix of urban and rural patients
- Medical college for training
- State government partnership

**Key Contacts:**
- Principal: [Name]
- Medical Superintendent: [Name]
- IT Coordinator: [Name]

#### Hospital 4: Semi-Urban Private (Pune)
**Name:** Ruby Hall Clinic (Hypothetical Partnership)  
**Type:** Private multi-specialty hospital  
**Beds:** 250  
**Location:** Pune, Maharashtra  
**Rationale:**
- Tier-2 city validation
- Tech-savvy patient population
- Good IT infrastructure
- Innovation-friendly management

**Key Contacts:**
- Managing Director: [Name]
- IT Manager: [Name]
- Quality Head: [Name]

#### Hospital 5: Rural PHC (Uttar Pradesh)
**Name:** Community Health Center, Rae Bareli (Hypothetical Partnership)  
**Type:** Government primary health center  
**Beds:** 50  
**Location:** Rae Bareli, Uttar Pradesh  
**Rationale:**
- Rural setting validation
- Limited infrastructure (stress test)
- Offline capability testing
- Low-resource environment
- High social impact

**Key Contacts:**
- Medical Officer In-Charge: [Name]
- District Health Officer: [Name]
- State NHM Coordinator: [Name]

---

## Pre-Deployment Phase (Week 15)

### Week 15: Hospital Assessment & Preparation

#### Day 1-2: Site Visits
- [ ] Visit all 5 hospitals
- [ ] Assess IT infrastructure
- [ ] Check internet connectivity
- [ ] Evaluate physical space for equipment
- [ ] Meet key stakeholders
- [ ] Document current workflows
- [ ] Identify integration points with existing systems

#### Day 3-4: Requirements Gathering
- [ ] Conduct stakeholder interviews
  - [ ] Doctors (5-10 per hospital)
  - [ ] Nurses (10-15 per hospital)
  - [ ] Admin staff (3-5 per hospital)
  - [ ] IT staff (1-2 per hospital)
- [ ] Document hospital-specific requirements
- [ ] Identify customization needs
- [ ] Assess training requirements
- [ ] Document current pain points

#### Day 5: Agreement Finalization
- [ ] Finalize pilot agreements
- [ ] Sign data sharing agreements
- [ ] Sign confidentiality agreements
- [ ] Clarify roles and responsibilities
- [ ] Set success metrics
- [ ] Establish communication protocols
- [ ] Define escalation procedures

### Infrastructure Preparation

#### Hardware Requirements (Per Hospital)

**Urban/Semi-Urban Hospitals (4 hospitals):**
- Edge Gateway: 1x Raspberry Pi 4 (8GB RAM)
- Workstations: 5-10 computers (existing)
- Tablets: 5x Android tablets for nurses
- IoT Devices: 10x patient monitoring devices
- Network: Backup 4G router
- UPS: Backup power supply

**Rural PHC (1 hospital):**
- Edge Gateway: 1x Raspberry Pi 4 (8GB RAM)
- Workstations: 2 computers (existing)
- Tablets: 2x Android tablets
- IoT Devices: 5x patient monitoring devices
- Network: 4G router (primary + backup)
- UPS: Backup power supply
- Solar power backup (if needed)

#### Software Preparation
- [ ] Create hospital-specific configurations
- [ ] Set up hospital subdomains
- [ ] Configure hospital-specific templates
- [ ] Prepare user accounts
- [ ] Configure role-based access
- [ ] Prepare training environments

#### Procurement
- [ ] Order hardware (Week 14)
- [ ] Arrange logistics
- [ ] Coordinate with hospital IT teams
- [ ] Plan installation schedule
- [ ] Arrange backup equipment

---

## Deployment Phase (Weeks 16-17)

### Week 16: Infrastructure Deployment

#### Day 1-2: Hardware Installation (Hospital 1 & 2)
- [ ] Install edge gateways
- [ ] Configure network connectivity
- [ ] Install IoT devices
- [ ] Set up tablets
- [ ] Test connectivity
- [ ] Configure backup systems

#### Day 3-4: Hardware Installation (Hospital 3 & 4)
- [ ] Install edge gateways
- [ ] Configure network connectivity
- [ ] Install IoT devices
- [ ] Set up tablets
- [ ] Test connectivity
- [ ] Configure backup systems

#### Day 5: Hardware Installation (Hospital 5 - Rural PHC)
- [ ] Install edge gateway
- [ ] Configure 4G connectivity
- [ ] Install IoT devices
- [ ] Set up tablets
- [ ] Test offline capabilities
- [ ] Configure solar backup (if applicable)

### Week 16: Software Configuration

#### Hospital-Specific Configuration
- [ ] Configure hospital profiles
- [ ] Set up departments
- [ ] Create user accounts
  - [ ] Doctors
  - [ ] Nurses
  - [ ] Admin staff
  - [ ] IT staff
- [ ] Configure role permissions
- [ ] Set up notification preferences
- [ ] Configure alert thresholds
- [ ] Customize templates

#### Integration Setup
- [ ] Integrate with existing EMR (if applicable)
- [ ] Set up data migration scripts
- [ ] Configure ABDM integration
- [ ] Test Health ID creation
- [ ] Test consent management
- [ ] Validate FHIR data exchange

#### Testing
- [ ] Conduct smoke tests
- [ ] Test all user workflows
- [ ] Test offline capabilities
- [ ] Test IoT device connectivity
- [ ] Test alert mechanisms
- [ ] Conduct security testing
- [ ] Performance testing

### Week 17: Training & Go-Live

#### Training Schedule

**Day 1: Admin & IT Staff Training (All Hospitals)**
- Session 1 (9 AM - 12 PM): System Overview
  - Architecture and components
  - User roles and permissions
  - Security and compliance
  - Troubleshooting basics
- Session 2 (2 PM - 5 PM): Hands-on Training
  - User management
  - Configuration management
  - Monitoring and alerts
  - Backup and recovery

**Day 2: Doctor Training (Hospital 1 & 2)**
- Session 1 (9 AM - 12 PM): Provider Dashboard
  - Login and navigation
  - Patient search
  - Health record viewing
  - Consent management
- Session 2 (2 PM - 5 PM): Clinical Features
  - Clinical notes
  - Prescription generation
  - Discharge planning
  - AI triage interpretation

**Day 3: Doctor Training (Hospital 3, 4 & 5)**
- Same as Day 2

**Day 4: Nurse Training (All Hospitals)**
- Session 1 (9 AM - 12 PM): Patient Monitoring
  - IoT device usage
  - Vital signs monitoring
  - Alert management
  - Patient registration
- Session 2 (2 PM - 5 PM): Hands-on Practice
  - Device setup
  - Data entry
  - Alert response
  - Troubleshooting

**Day 5: Patient Onboarding Training**
- Session 1 (9 AM - 12 PM): Patient App
  - Health ID creation
  - App navigation
  - Health record access
  - Consent management
- Session 2 (2 PM - 5 PM): Patient Support
  - Helpdesk training
  - Common issues
  - Patient communication
  - Feedback collection

#### Go-Live Preparation
- [ ] Final system checks
- [ ] Verify all accounts
- [ ] Test all workflows
- [ ] Prepare support team
- [ ] Set up helpdesk
- [ ] Prepare escalation contacts
- [ ] Create quick reference guides
- [ ] Set up monitoring dashboards

#### Go-Live (Day 5 Evening)
- [ ] Enable production access
- [ ] Monitor system closely
- [ ] Support team on standby
- [ ] Address immediate issues
- [ ] Collect initial feedback
- [ ] Document lessons learned

---

## Operation Phase (Weeks 18-20)

### Week 18: Stabilization

#### Daily Activities
- [ ] Morning standup with hospital IT teams
- [ ] Monitor system performance
- [ ] Track user adoption
- [ ] Address bugs and issues
- [ ] Provide on-site support
- [ ] Collect user feedback
- [ ] Evening status report

#### Key Metrics to Monitor
- System uptime
- API response times
- Error rates
- User login counts
- Patient registrations
- Health records created
- IoT device connectivity
- Alert response times

#### Support Structure
- **On-Site Support:** 1 engineer per hospital (Week 18)
- **Remote Support:** 24/7 helpdesk
- **Escalation:** Technical team on-call
- **Response Times:**
  - Critical issues: <30 minutes
  - High priority: <2 hours
  - Medium priority: <8 hours
  - Low priority: <24 hours

### Week 19: Optimization

#### Performance Optimization
- [ ] Analyze performance bottlenecks
- [ ] Optimize slow queries
- [ ] Improve API response times
- [ ] Optimize mobile app performance
- [ ] Reduce edge gateway latency

#### User Experience Improvements
- [ ] Address usability issues
- [ ] Improve UI based on feedback
- [ ] Add requested features (quick wins)
- [ ] Improve error messages
- [ ] Enhance mobile app UX

#### Training Reinforcement
- [ ] Conduct refresher training sessions
- [ ] Create video tutorials
- [ ] Update user guides
- [ ] Address knowledge gaps
- [ ] Certify power users

### Week 20: Evaluation

#### Data Collection

**Quantitative Metrics:**
- [ ] System uptime percentage
- [ ] API response times (p50, p95, p99)
- [ ] Error rates
- [ ] User adoption rates
- [ ] Patient registrations
- [ ] Health records created
- [ ] IoT messages processed
- [ ] Cost per patient
- [ ] Triage accuracy
- [ ] Documentation time savings

**Qualitative Feedback:**
- [ ] Conduct user interviews (20-30 users per hospital)
- [ ] Distribute satisfaction surveys
- [ ] Collect success stories
- [ ] Document pain points
- [ ] Gather feature requests

#### Success Metrics Evaluation

| Metric | Target | Hospital 1 | Hospital 2 | Hospital 3 | Hospital 4 | Hospital 5 | Overall |
|--------|--------|------------|------------|------------|------------|------------|---------|
| System Uptime | >99.5% | | | | | | |
| API Response Time | <500ms | | | | | | |
| User Adoption | >80% | | | | | | |
| Patient Registrations | 1000+ | | | | | | |
| Cost per Patient | ₹23/month | | | | | | |
| Triage Accuracy | >90% | | | | | | |
| User Satisfaction | >4.0/5 | | | | | | |

#### Pilot Report
- [ ] Executive summary
- [ ] Methodology
- [ ] Results and findings
- [ ] Success stories
- [ ] Challenges and solutions
- [ ] Lessons learned
- [ ] Recommendations
- [ ] Next steps

---

## Risk Management

### Technical Risks

#### Risk 1: Internet Connectivity Issues (Rural PHC)
**Probability:** High  
**Impact:** High  
**Mitigation:**
- Implement robust offline mode
- Use 4G backup connection
- Buffer data locally on edge gateway
- Sync when connectivity restored
- Monitor connectivity proactively

#### Risk 2: IoT Device Failures
**Probability:** Medium  
**Impact:** Medium  
**Mitigation:**
- Keep spare devices
- Implement device health monitoring
- Train staff on troubleshooting
- Provide 24/7 technical support
- Have vendor support on standby

#### Risk 3: Integration Issues with Existing EMR
**Probability:** Medium  
**Impact:** Medium  
**Mitigation:**
- Thorough pre-deployment testing
- Fallback to manual data entry
- Phased integration approach
- Vendor coordination
- Dedicated integration engineer

#### Risk 4: Performance Issues Under Load
**Probability:** Low  
**Impact:** High  
**Mitigation:**
- Conduct load testing before deployment
- Monitor performance continuously
- Auto-scaling configured
- Performance optimization team ready
- Fallback to reduced functionality

### Operational Risks

#### Risk 1: User Resistance
**Probability:** Medium  
**Impact:** High  
**Mitigation:**
- Comprehensive training program
- Change management strategy
- Identify and empower champions
- Collect and address feedback quickly
- Demonstrate value early

#### Risk 2: Data Migration Issues
**Probability:** Medium  
**Impact:** Medium  
**Mitigation:**
- Thorough data validation
- Phased migration approach
- Backup all data
- Rollback plan ready
- Data quality checks

#### Risk 3: Staff Turnover During Pilot
**Probability:** Low  
**Impact:** Medium  
**Mitigation:**
- Document all processes
- Record training sessions
- Create quick reference guides
- Provide ongoing training
- Build redundancy in knowledge

#### Risk 4: Budget Overruns
**Probability:** Low  
**Impact:** Medium  
**Mitigation:**
- Detailed budget planning
- Cost monitoring
- Contingency budget (10%)
- Regular cost reviews
- Optimize continuously

---

## Budget

### Hardware Costs

| Item | Quantity | Unit Cost (₹) | Total Cost (₹) |
|------|----------|---------------|----------------|
| Raspberry Pi 4 (8GB) | 5 | 8,000 | 40,000 |
| Android Tablets | 24 | 15,000 | 3,60,000 |
| IoT Devices | 45 | 10,000 | 4,50,000 |
| 4G Routers | 5 | 5,000 | 25,000 |
| UPS Systems | 5 | 10,000 | 50,000 |
| Networking Equipment | 5 | 20,000 | 1,00,000 |
| **Subtotal** | | | **10,25,000** |

### Software & Cloud Costs (6 weeks)

| Item | Monthly Cost (₹) | 6 Weeks Cost (₹) |
|------|------------------|------------------|
| AWS Infrastructure | 50,000 | 75,000 |
| ABDM Integration | 10,000 | 15,000 |
| Blockchain Network | 20,000 | 30,000 |
| Monitoring Tools | 5,000 | 7,500 |
| **Subtotal** | | **1,27,500** |

### Personnel Costs (6 weeks)

| Role | Number | Weekly Cost (₹) | 6 Weeks Cost (₹) |
|------|--------|-----------------|------------------|
| Project Manager | 1 | 50,000 | 3,00,000 |
| Technical Lead | 1 | 50,000 | 3,00,000 |
| Engineers (On-site) | 5 | 40,000 | 12,00,000 |
| Support Staff | 3 | 25,000 | 4,50,000 |
| Medical Expert | 1 | 40,000 | 2,40,000 |
| Training Specialists | 2 | 30,000 | 3,60,000 |
| **Subtotal** | | | **28,50,000** |

### Travel & Logistics

| Item | Cost (₹) |
|------|----------|
| Travel (Team) | 2,00,000 |
| Accommodation | 3,00,000 |
| Local Transport | 50,000 |
| Equipment Shipping | 50,000 |
| **Subtotal** | **6,00,000** |

### Training & Materials

| Item | Cost (₹) |
|------|----------|
| Training Materials | 50,000 |
| User Guides (Printed) | 30,000 |
| Video Production | 1,00,000 |
| Venue & Catering | 1,00,000 |
| **Subtotal** | **2,80,000** |

### Contingency & Miscellaneous

| Item | Cost (₹) |
|------|----------|
| Contingency (10%) | 4,88,250 |
| Miscellaneous | 50,000 |
| **Subtotal** | **5,38,250** |

### Total Pilot Budget

| Category | Cost (₹) |
|----------|----------|
| Hardware | 10,25,000 |
| Software & Cloud | 1,27,500 |
| Personnel | 28,50,000 |
| Travel & Logistics | 6,00,000 |
| Training & Materials | 2,80,000 |
| Contingency & Misc | 5,38,250 |
| **Total** | **54,20,750** |

---

## Success Criteria

### Technical Success Criteria
- ✅ System uptime >99.5%
- ✅ API response time <500ms (p95)
- ✅ Zero critical security incidents
- ✅ All core features functional
- ✅ IoT devices connected and streaming data
- ✅ Offline mode working in rural PHC

### Clinical Success Criteria
- ✅ Triage accuracy >90%
- ✅ Documentation time reduced by >50%
- ✅ Medical errors reduced by >30%
- ✅ Emergency response time reduced by >20%
- ✅ Validated by medical experts

### User Adoption Criteria
- ✅ >80% of doctors using system daily
- ✅ >80% of nurses using system daily
- ✅ >500 patients registered per hospital
- ✅ User satisfaction >4.0/5
- ✅ <5% user churn

### Cost Criteria
- ✅ Cost per patient ≤₹23/month
- ✅ Infrastructure costs within budget
- ✅ No cost overruns >10%
- ✅ ROI positive within 6 months

### Business Criteria
- ✅ 5 hospitals successfully deployed
- ✅ 1000+ patients with digital health wallets
- ✅ 3+ success stories documented
- ✅ Pilot report completed
- ✅ Hospitals willing to continue post-pilot

---

## Post-Pilot Activities

### Immediate (Week 21)
- [ ] Conduct pilot review meeting
- [ ] Finalize pilot report
- [ ] Share results with stakeholders
- [ ] Plan improvements based on feedback
- [ ] Decide on continuation with pilot hospitals

### Short-term (Weeks 22-24)
- [ ] Implement critical improvements
- [ ] Prepare for regional expansion
- [ ] Create case studies
- [ ] Present at conferences
- [ ] Seek additional funding

### Long-term (Months 7-12)
- [ ] Scale to 50 hospitals
- [ ] Achieve national readiness
- [ ] Open-source SDK release
- [ ] National launch

---

## Communication Plan

### Internal Communication

**Daily:**
- Morning standup (9 AM)
- Evening status report (6 PM)
- Slack updates

**Weekly:**
- Team meeting (Monday 10 AM)
- Hospital check-ins (Wednesday)
- Executive update (Friday)

**Bi-weekly:**
- Sprint review
- Sprint retrospective
- Stakeholder update

### External Communication

**Weekly:**
- Hospital leadership update
- User feedback summary
- Issue resolution report

**Bi-weekly:**
- Pilot steering committee meeting
- Progress report to funders

**Monthly:**
- Public blog post
- Social media updates
- Newsletter

---

## Appendices

### Appendix A: Training Materials
- User guides
- Video tutorials
- Quick reference cards
- FAQ documents

### Appendix B: Support Documentation
- Troubleshooting guides
- Runbooks
- Escalation procedures
- Contact lists

### Appendix C: Forms & Templates
- User feedback form
- Incident report template
- Daily status report template
- Weekly progress report template

### Appendix D: Hospital Profiles
- Detailed profiles of each pilot hospital
- Contact information
- Infrastructure details
- Customization requirements

---

**Document Control**

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2026-02-15 | Swasthya Team | Initial pilot deployment plan |

**Approval**

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Project Manager | | | |
| Technical Lead | | | |
| Medical Director | | | |

---

**End of Pilot Deployment Plan**
