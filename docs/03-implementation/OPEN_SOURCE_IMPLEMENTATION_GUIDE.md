# Swasthya - Open Source Implementation Guide

**Version:** 1.0  
**Date:** February 15, 2026  
**Purpose:** Guide for implementing cost-optimized open-source components

---

## Overview

This guide provides detailed implementation instructions for all open-source components used in Swasthya to achieve 93% AI cost reduction. Each component replaces expensive cloud AI services with free, open-source alternatives.

**Cost Impact:** ₹15/patient/month → ₹1/patient/month (93% savings)

---

## Table of Contents

1. [Rule-Based Triage Agent](#1-rule-based-triage-agent)
2. [Template-Based Discharge Planning](#2-template-based-discharge-planning)
3. [Staff Scheduling with OR-Tools](#3-staff-scheduling-with-or-tools)
4. [Demand Forecasting with Prophet](#4-demand-forecasting-with-prophet)
5. [Clinical Decision Support](#5-clinical-decision-support)
6. [Medical NLP with BioBERT](#6-medical-nlp-with-biobert)
7. [Edge Computing for IoT](#7-edge-computing-for-iot)

---

## 1. Rule-Based Triage Agent

### Overview
Replaces AWS Nova Lite with a rule-based system for 80% of cases, using ML only for complex cases.

**Cost Savings:** ₹5/patient/month → ₹0.50/patient/month (90% savings)

### Architecture

```
Patient Data → Rule-Based Scorer → Confidence Check
                                         ↓
                                   High Confidence? → Return Priority
                                         ↓
                                   Low Confidence → ML Model → Return Priority
```

### Implementation

See detailed implementation in `/docs/open-source/` directory:
- `triage-agent.md` - Complete triage implementation
- `discharge-planning.md` - Template-based discharge system
- `staff-scheduling.md` - OR-Tools scheduling
- `demand-forecasting.md` - Prophet forecasting
- `clinical-decision-support.md` - Rule-based CDS
- `medical-nlp.md` - BioBERT integration
- `edge-computing.md` - IoT edge processing

### Quick Start

```bash
# Install Python dependencies
pip install scikit-learn numpy pandas

# Clone repository
git clone https://github.com/swasthya/open-source-agents
cd open-source-agents

# Run triage agent
python agents/triage/rule_based_triage.py
```

### Key Files
- `agents/triage/rule_based_triage.py` - Main triage logic
- `agents/triage/ml_fallback.py` - ML model for complex cases
- `agents/triage/config.yaml` - Threshold configurations
- `tests/test_triage.py` - Unit tests
