# Swasthya - Architecture Document

**Version:** 1.0  
**Date:** February 15, 2026  
**Status:** Draft  
**Project:** Swasthya - India's Decentralized Health Intelligence Network

---

## 1. Executive Summary

This document provides a comprehensive architectural overview of Swasthya, detailing the system's structure, components, and design decisions. The architecture is designed to support 1.4 billion users with 99.9% availability, <100ms response times for critical operations, and full DISHA/ABDM compliance.

### Key Architectural Principles
- **Scalability**: Horizontal scaling to support India's population
- **Resilience**: Multi-AZ, multi-region deployment
- **Security**: Zero-trust architecture with end-to-end encryption
- **Interoperability**: FHIR/ABDM standards compliance
- **Cost-Efficiency**: Serverless-first, pay-per-use model
- **Privacy**: Federated learning, blockchain consent

---

## 2. Architectural Views

### 2.1 Logical Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                         User Interface Layer                         │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐             │
│  │ Patient      │  │ Provider     │  │ Admin        │             │
│  │ Mobile/Web   │  │ Dashboard    │  │ Console      │             │
│  └──────────────┘  └──────────────┘  └──────────────┘             │
└────────────────────────────┬────────────────────────────────────────┘
                             │
┌────────────────────────────▼────────────────────────────────────────┐
│                      API Gateway & Security                          │
│  Authentication │ Authorization │ Rate Limiting │ DDoS Protection   │
└────────────────────────────┬────────────────────────────────────────┘
                             │
┌────────────────────────────▼────────────────────────────────────────┐
│                      Business Logic Layer                            │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐             │
│  │ User         │  │ Health       │  │ AI Agent     │             │
│  │ Management   │  │ Records      │  │ System       │             │
│  └──────────────┘  └──────────────┘  └──────────────┘             │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐             │
│  │ ABDM         │  │ IoT          │  │ Analytics    │             │
│  │ Integration  │  │ Management   │  │ Engine       │             │
│  └──────────────┘  └──────────────┘  └──────────────┘             │
└────────────────────────────┬────────────────────────────────────────┘
                             │
┌────────────────────────────▼────────────────────────────────────────┐
│                      Data Access Layer                               │
│  Repository Pattern │ CQRS │ Event Sourcing │ Caching               │
└────────────────────────────┬────────────────────────────────────────┘
                             │
┌────────────────────────────▼────────────────────────────────────────┐
│                      Data Storage Layer                              │
│  FHIR Store │ Relational DB │ NoSQL │ Object Storage │ Blockchain   │
└─────────────────────────────────────────────────────────────────────┘
```

### 2.2 Physical Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                         Global Layer                                 │
│  ┌──────────────────────────────────────────────────────────────┐  │
│  │ Amazon CloudFront (CDN)                                       │  │
│  │ - Static content caching                                      │  │
│  │ - Edge locations across India                                 │  │
│  │ - DDoS protection (AWS Shield)                                │  │
│  └──────────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────┘
                                  │
        ┌─────────────────────────┴─────────────────────────┐
        │                                                     │
┌───────▼──────────────────────┐         ┌──────────────────▼─────────┐
│   Region: ap-south-1 (Mumbai)│         │ Region: ap-south-2 (Hyd)   │
│                              │         │                             │
│  ┌────────────────────────┐ │         │  ┌────────────────────────┐│
│  │ Availability Zone 1a   │ │         │  │ Availability Zone 2a   ││
│  │ - ECS Services         │ │         │  │ - ECS Services         ││
│  │ - Lambda Functions     │ │         │  │ - Lambda Functions     ││
│  │ - RDS Primary          │ │         │  │ - RDS Read Replica     ││
│  └────────────────────────┘ │         │  └────────────────────────┘│
│                              │         │                             │
│  ┌────────────────────────┐ │         │  ┌────────────────────────┐│
│  │ Availability Zone 1b   │ │         │  │ Availability Zone 2b   ││
│  │ - ECS Services         │ │         │  │ - ECS Services         ││
│  │ - Lambda Functions     │ │         │  │ - Lambda Functions     ││
│  │ - RDS Standby          │ │         │  │ - RDS Read Replica     ││
│  └────────────────────────┘ │         │  └────────────────────────┘│
│                              │         │                             │
│  ┌────────────────────────┐ │         │  ┌────────────────────────┐│
│  │ Shared Services        │ │         │  │ Shared Services        ││
│  │ - S3 (Primary)         │ │         │  │ - S3 (Replica)         ││
│  │ - DynamoDB (Global)    │ │         │  │ - DynamoDB (Global)    ││
│  │ - ElastiCache          │ │         │  │ - ElastiCache          ││
│  └────────────────────────┘ │         │  └────────────────────────┘│
└──────────────────────────────┘         └────────────────────────────┘
```

