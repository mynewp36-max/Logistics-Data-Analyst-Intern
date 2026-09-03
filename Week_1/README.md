# Week 1 — Strategic Planning and Project Foundation

[![Python 3.10+](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)
[![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-1.2%2B-orange.svg)](https://scikit-learn.org/)
[![PuLP Optimization](https://img.shields.io/badge/PuLP-Linear%20Programming-green.svg)](https://coin-or.github.io/pulp/)
[![Plotly Visualization](https://img.shields.io/badge/Plotly-Express%20%26%20Graph%20Objects-purple.svg)](https://plotly.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

## Project Overview

The **Intelligent Logistics Performance Analytics and Delivery Optimization System** is an end-to-end data analytics, machine learning, and operations research framework designed for complex, multi-modal freight and supply chain networks. Operating across high-volume distribution territories and regional line-haul corridors, commercial logistics networks encounter persistent transit stochasticity, carrier delivery delays, volatile spot-market freight rates, and strict customer Service Level Agreements (SLAs).

This project establishes a comprehensive decision-support system integrating:
- **Logistics Performance Monitoring & Diagnostic Auditing**: Continuous tracking of network-wide freight velocity, cross-dock dwell times, and carrier reliability.
- **Hierarchical KPI Architecture**: Multi-tiered performance measurement aligning strategic executive objectives with operational dispatch choices.
- **Predictive Machine Learning**: Supervised regression for continuous elapsed transit time forecasting and classification for pre-dispatch delay risk detection.
- **Unsupervised Route Segmentation**: K-Means clustering to profile line-haul corridors into actionable operational risk archetypes.
- **Prescriptive Operations Research**: Mixed-Integer Linear Programming (MILP) to minimize total network transportation expenditures while guaranteeing fleet capacity and SLA delivery windows.
- **Decision Intelligence & Interactive Dashboards**: Production-grade visual interfaces powered by Plotly for dispatcher intervention and executive performance oversight.

---

## Week 1 Objective

Week 1 serves as the foundational **Strategic Planning, Research, and Architectural Design** phase of the internship lifecycle. Prior to programmatic implementation and large-scale model training, robust analytical planning is required to scope business requirements, translate operational bottlenecks into formal mathematical and statistical problems, establish data governance protocols, and blueprint the analytics pipeline.

### Core Week 1 Objectives
1. Formulate a comprehensive operational scenario and formal problem statement based on enterprise logistics challenges.
2. Establish a multi-tiered Key Performance Indicator (KPI) framework linking executive financial performance to ground-level dispatch metrics.
3. Conduct feasibility research into public logistics datasets and enterprise Transportation Management System (TMS) data schemas.
4. Architect the data science methodology across supervised regression, binary classification, unsupervised clustering, and prescriptive linear programming.
5. Design an end-to-end analytics lifecycle and modular Python architecture to govern implementation in subsequent project phases.

---

## Problem Statement

Modern commercial freight operations function under narrow operating margins, rigid delivery appointments, and volatile transportation expenditures. Across our modeled enterprise business scenario—comprising **5 operational territories** (Northeast, Southeast, Midwest, Southwest, Pacific West), **12 central cross-dock distribution hubs**, and **4 transportation modes** (Express Air, Full Truckload [FTL], Less-Than-Truckload [LTL], and Intermodal Rail)—operational leadership faces three primary structural bottlenecks:

1. **Transit Duration Stochasticity & Delivery Delays**: Highway transit duration is heavily affected by mountain elevation passes, urban toll bridge congestion, weather fronts, and terminal dwell times. Static mileage-based scheduling tables fail to account for non-linear transit variability, resulting in suboptimal arrival windows and SLA chargebacks.
2. **Suboptimal Modal Selection & Rising Line-Haul Costs**: Dispatchers frequently resort to emergency spot-market carrier bookings and expedited air upgrades to salvage compromised delivery commitments, inflating transportation costs per shipment.
3. **Absence of Pre-Dispatch Decision Intelligence**: Operational teams lack predictive visibility prior to departure. When delay risks are recognized only after vehicles are in transit, remedial rerouting options are severely restricted and prohibitively expensive.

```
┌─────────────────────────┐     ┌──────────────────────────┐     ┌─────────────────────────┐
│ High Demand Volatility  │ ──► │ Static Transit Tables    │ ──► │ Pre-Dispatch Delays     │
│ & Terminal Choke Points │     │ & Manual Dispatching     │     │ & Margin Compression    │
└─────────────────────────┘     └──────────────────────────┘     └─────────────────────────┘
                                              │
                                              ▼
                                ┌──────────────────────────┐
                                │ Reactive Spot Upgrades   │
                                │ & Contract SLA Penalties │
                                └──────────────────────────┘
```

> **Methodological Note**: All baseline values, percentage thresholds, and financial expenditure benchmarks referenced during Week 1 represent strategic planning assumptions formulated to construct a realistic operational business case. Empirical parameters will be computed during data ingestion and exploratory data analysis in Week 2.

---

## Key Performance Indicators (KPIs)

To evaluate logistics efficiency without creating conflicting operational incentives (e.g., maximizing speed at unsustainable expenditure, or minimizing cost at the expense of customer service), Week 1 establishes a balanced, multi-dimensional KPI framework structured into Strategic, Tactical, and Operational tiers.

| KPI Name | Category | Tier | Measurement Formula | Target Benchmark | Decision Impact |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **On-Time Delivery Rate (OTD%)** | Customer Service / Reliability | Strategic | `(Shipments Delivered On or Before SLA / Total Delivered Shipments) × 100` | $\ge 95.0\%$ | Network delivery contract compliance & customer retention |
| **Total Transportation Cost per Shipment (TCPS)** | Financial Performance | Strategic | `Total Direct Line-Haul Transportation Cost / Total Shipments Completed` | $\le \$450.00$ | Unit operational margin and carrier contract profitability |
| **Average Delay Time (ADT)** | Operational Velocity | Tactical | `Total Delayed Hours Across Delayed Shipments / Total Delayed Shipments` | $\le 1.75\text{ hrs}$ | Hub cross-dock dwell management and scheduling buffer design |
| **Vehicle Capacity Utilization Rate (VCUR)** | Fleet Efficiency | Tactical | `(Total Consignment Weight or Volume / Total Rated Vehicle Capacity) × 100` | $\ge 85.0\%$ | Avoidance of partial-load inefficiencies and modal over-allocation |
| **Pre-Dispatch Delay Detection Rate** | Predictive Capability | Operational | `True Positive Pre-Dispatch Delay Alerts / Total Actual Delayed Shipments` | $\ge 85.0\%$ | Timely dispatch intervention, driver re-assignment, and carrier upgrades |
| **Route Cost Friction Ratio (RCFR)** | Corridor Efficiency | Operational | `Actual Incurred Transit Cost / Minimum Theoretical Benchmark Direct Cost` | $\le 1.15$ | Identification of circuitous routing, toll friction, and congestion lanes |

---

## Analytics Approach

The project bridges business challenges and data science methods using an **Analytics Maturity Framework** that progresses through five complementary stages:

```
[Descriptive] ────► [Diagnostic] ────► [Predictive] ────► [Prescriptive] ────► [Decision Intel]
  What happened?      Why did it happen?  What will happen?   What should we do?  How to automate?
```

### 1. Descriptive Analytics (Visibility & Baselines)
- Audits historical shipping manifests to calculate baseline OTD%, average velocity (km/h), modal cost splits, and regional throughput.
- Generates univariate distributions and cross-dock volume profiles.

### 2. Diagnostic Analytics (Root-Cause Discovery)
- Conducts multivariate correlation analysis between environmental covariates (precipitation, highway terrain elevation, terminal dwell) and transit delays.
- Isolates chronic choke points and attributes carrier-specific delay variances.

### 3. Predictive Analytics (Machine Learning)
- **Supervised Regression**: Evaluates OLS, Ridge/Lasso, Random Forest Regressor, and LightGBM to forecast continuous elapsed shipment duration (`Delivery_Time_Hours`).
- **Binary Classification**: Evaluates Logistic Regression, Random Forest Classifier, and XGBoost to predict pre-dispatch SLA breach probability (`Delay_Flag ∈ {0, 1}`) using calibrated probability thresholds.

### 4. Prescriptive Analytics (Mathematical Optimization)
- Formulates a **Mixed-Integer Linear Programming (MILP)** optimization model using PuLP and the CBC Simplex solver.
- Minimizes total transportation network cost across origin supply nodes, destination demand markets, and transportation modes subject to capacity constraints, demand fulfillment, and SLA maximum duration bounds.

### 5. Decision Intelligence (Operational Decision Support)
- Translates predictive risks and optimization outputs into real-time operational artifacts: dispatcher exception alerts, automated mode recommendations, and executive dashboards.

---

## Data Strategy

The data foundation follows a strict, reproducible **8-stage data lifecycle** engineered to prevent data leakage and guarantee data hygiene:

```text
Data Ingestion (TMS/WMS/GPS)
  │
  ▼
Data Cleaning & Quality Audit (Schema Validation, MD5 Deduplication, IQR Fencing)
  │
  ▼
Data Preparation & Staging (Median Imputation, Temporal Parsing ISO-8601)
  │
  ▼
Exploratory Data Analysis (Interactive Plotly Profiling, Modal Benchmark Matrices)
  │
  ▼
Feature Engineering (Haversine Distance, Terrain Friction, Cyclical Sine/Cosine Features)
  │
  ▼
Predictive & Unsupervised Modeling (Time-Series Train/Validation Split, Scikit-Learn Pipelines)
  │
  ▼
Mathematical Cost Optimization (PuLP Fleet & Modal Assignment Solvers)
  │
  ▼
Decision Support & Visualization (Plotly Dashboards & Automated Exception Portals)
```

### Public Data Resources & Feasibility Research
During Week 1, public logistics data repositories were researched to evaluate dataset schemas and modeling feasibility:
- **Freight Analysis Framework (FAF5)**: Multi-modal commodity flow volumes and national freight density matrices.
- **Bureau of Transportation Statistics (BTS)**: Freight transportation services index, carrier on-time performance, and border crossing durations.
- **NYC TLC Trip Record Data**: High-frequency urban trip duration, GPS spatial breadcrumbs, and weather congestion modeling benchmarks.
- **UK Department for Transport Freight Statistics**: Line-haul domestic haulage statistics, average payload weights, and vehicle fill factors.
- **Open Logistics Datasets (Kaggle / Mendeley Data)**: Supply chain shipment pricing, GPS breadcrumb tracks, and delivery status logs.

---

## Planned Technology Stack

The project utilizes a pure Python-based enterprise data science and operations research stack:

| Component | Technology | Purpose & Scope |
| :--- | :--- | :--- |
| **Language** | `Python 3.10+` | Core programming language for scripts, models, and analytics |
| **Data Manipulation** | `Pandas`, `NumPy` | High-performance vector operations, schema governance, dataset aggregation |
| **Machine Learning** | `Scikit-Learn` | Regression pipelines, classification estimators, cross-validation, metrics |
| **Gradient Boosting** | `LightGBM`, `XGBoost` | State-of-the-art tree ensemble modeling for non-linear freight relationships |
| **Mathematical Optimization** | `PuLP`, `SciPy Optimize` | Mixed-integer linear programming (MILP), CBC Simplex cost optimization solver |
| **Data Visualization** | `Plotly Express`, `Plotly Graph Objects` | Interactive, executive-grade exploratory charts and performance scorecards |
| **Development Environment** | `Jupyter Notebook`, `VS Code` | Reproducible exploratory notebooks and structured modular Python development |
| **Version Control** | `Git`, `GitHub` | Source code management, release branching, documentation hosting |

---

## Week 1 Deliverables

The strategic planning deliverables completed during Week 1 include:

- [x] **Formal Problem Statement**: Formulated business context across 5 geographic zones, 12 distribution hubs, and 4 shipping modes.
- [x] **Project Objectives & Analytical Questions**: Established core research questions spanning baseline reliability, transit stochasticity, and cost trade-offs.
- [x] **Stakeholder Framework**: Structured a RACIS decision-maker matrix across Executive, Operations, Logistics Engineering, and Finance.
- [x] **KPI Measurement Framework**: Designed a balanced scorecard encompassing OTD%, ADT, TCPS, VCUR, and SLA Compliance.
- [x] **Data Schema Specification**: Defined a 20+ feature logistics data dictionary covering geospatial, operational, temporal, and financial attributes.
- [x] **Public Data Feasibility Evaluation**: Evaluated 7 national and international transportation data custodians.
- [x] **Data Quality & Governance Matrix**: Established validation rules for completeness, accuracy, validity, timeliness, and consistency.
- [x] **Predictive Modeling Strategy**: Designed supervised regression and classification pipelines with leakage prevention protocols.
- [x] **Unsupervised Clustering Strategy**: Defined K-Means route risk profiling across transit velocity, terrain, and delay metrics.
- [x] **Prescriptive Optimization Formulation**: Mathematically formulated the cost minimization objective function and operational linear constraints.
- [x] **Modular Python Architecture**: Blueprinted 8 Python implementation modules (Modules A through H) utilizing Plotly, Scikit-Learn, and PuLP.
- [x] **Formal Submission Report**: Authored the complete 44-page technical planning deliverable (`Week_1_Intelligent_Logistics_Performance_Analytics_Report.docx`).

---

## Project Roadmap

The multi-week internship implementation follows a phased progression from conceptual architecture to production deployment:

```text
┌────────────────────────────────────────────────────────────────────────┐
│  WEEK 1: Strategic Planning and Project Foundation (COMPLETED)          │
│  - Problem formulation, KPI framework, data governance & ML design     │
└────────────────────────────────────────────────────────────────────────┘
                                   │
                                   ▼
┌────────────────────────────────────────────────────────────────────────┐
│  WEEK 2: Data Collection, Cleaning and Exploratory Analysis (EDA)       │
│  - TMS data ingestion, cleaning pipeline, interactive Plotly profiling │
└────────────────────────────────────────────────────────────────────────┘
                                   │
                                   ▼
┌────────────────────────────────────────────────────────────────────────┐
│  WEEK 3: Predictive Modeling and Machine Learning                      │
│  - Feature engineering, transit regression, pre-dispatch delay classifier│
└────────────────────────────────────────────────────────────────────────┘
                                   │
                                   ▼
┌────────────────────────────────────────────────────────────────────────┐
│  WEEK 4: Optimization and Advanced Analytics                           │
│  - PuLP linear cost solver, route clustering, prescriptive dispatch    │
└────────────────────────────────────────────────────────────────────────┘
                                   │
                                   ▼
┌────────────────────────────────────────────────────────────────────────┐
│  WEEK 5: Dashboard Development & Business Intelligence                 │
│  - Interactive Plotly Dash scorecards, dispatcher exception portals    │
└────────────────────────────────────────────────────────────────────────┘
                                   │
                                   ▼
┌────────────────────────────────────────────────────────────────────────┐
│  WEEK 6: Final Project Completion and Executive Delivery               │
│  - Full pipeline integration, executive presentation & final sign-off │
└────────────────────────────────────────────────────────────────────────┘
```

---

## Repository Structure

```text
Logistics-Data-Analyst-Intern/
│
├── Week_1/
│   ├── README.md                                                      
│   └── Strategic Planning and Data Exploration in Logistics_Week1_Report.docx
│   └── Strategic Planning and Data Exploration in Logistics_Week1_Report.pdf


## Author & Academic Acknowledgements
- **Author**: Logistics Data Analytics Intern (Yuwa Remote Internship Program)
- **Project Track**: Logistics Data Analyst Internship — Advanced Analytics & Decision Science
- **Target Organization**: Enterprise Multi-Modal Freight & Line-Haul Distribution Network
- **Academic Period**: August 2026 (Academic Year 2026–2027)
