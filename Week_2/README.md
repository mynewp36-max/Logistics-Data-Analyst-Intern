# Logistics Data Analyst Internship — Week 2
## Data Cleaning, Feature Engineering and Analytical Architecture

An enterprise-grade, reproducible data engineering and analytics pipeline transforming raw, messy transactional logistics records into a fully governed, feature-engineered, dual-sheet Excel analytical deliverable.

---

## 1. Project Overview

Week 2 of the **Logistics Data Analyst Internship** represents the core data engineering, forensic hygiene, and architecture phase of the multi-week program. The primary objective was to transition from the exploratory observations established in Week 1 to building a verified, production-ready logistics dataset and analytical asset.

Starting from an immutable raw transactional log of **184,223 records across 53 heterogeneous attributes**, the dataset suffered from severe real-world data debt: 3,704 exact duplicate rows, 180,519 completely missing product descriptions (100% missingness), demographic omissions, corrupted non-ASCII character encodings, multilingual administrative geography, redundant legacy identifiers, and unstandardized numerical scales.

Through a rigorous **two-notebook architecture**, the project executed:
* **Systematic Ingestion & Primary Key Audit**: Verified Latin-1 encoding and locked `Order Item Id` as the unique transactional grain.
* **Controlled Deduplication & Base Locking**: Eliminated exactly 3,704 duplicate records and locked a clean baseline of **180,519 records** with zero row deletion permitted thereafter.
* **Deterministic Missing Value Remediation**: Recovered 100% of product descriptions via relational joining with a reference catalog (`product_descriptions.csv`) and reconstructed customer demographics using internal relational IDs.
* **Multi-Tier Geographic & Text Standardization**: Harmonized international cities, states, and 164 countries into canonical English names; removed diacritics and normalized whitespace.
* **Column Governance**: Dropped 9 redundant, zero-variance, and PII columns while locking 44 canonical operational attributes.
* **Outlier Domain Retention & StandardScaler Normalization**: Justified commercial extremes via domain reality and generated 11 continuous Z-score features ($\mu = 0.0000, \sigma = 1.0000$).
* **Domain Feature Engineering**: Engineered 15 behavioral indicators spanning transit duration, delivery delays, severity tiers, order seasonality, commercial deficits, and shipping corridors.
* **Dual-Sheet Relational Excel Architecture**: Partitioned data into `Logistic_Master_Business_Data` (59 columns) and `Logistic_ML_Analytics_Ready` (16 columns), linked by `Order Item Id` with zero orphan records.
* **Automated Post-Export Audit**: Verified file serialization on disk (**85.5 MB**) and confirmed a formal **PASS** status.

---

## 2. Week 2 Objectives

Based on the official internship curriculum and technical roadmap (`roadmap.txt`), Week 2 executed the following core requirements:

1. **Establish Data Cleaning Scope & Integrity Principles**: Define non-destructive cleaning standards, preserve raw data immutability, and enforce evidence-based transformations.
2. **Execute Ingestion & Encoding Verification**: Safely load the source dataset under ISO-8859-1 (Latin-1) encoding and identify the true dataset grain.
3. **Audit & Eliminate Duplicate Records**: Perform full-row and primary-key duplicate forensics; purge verified redundancy and establish a clean base lock.
4. **Remediate Missing Values Deterministically**: Reconstruct missing text and categorical attributes using relational catalog lookups and internal entity links without synthetic imputations.
5. **Standardize Geographic & Categorical Attributes**: Cleanse orthographic anomalies, character encoding corruption, and language variations across cities, states, and countries.
6. **Enforce Column Governance**: Audit and eliminate redundant identifier duplicates, system artifacts, and sensitive PII columns.
7. **Profile Outliers & Apply Standardization**: Review numerical distributions using IQR methodology, justify valid commercial operational extremes, and normalize continuous metrics via `StandardScaler`.
8. **Engineer Domain-Specific Logistics Features**: Formulate derived features for transit durations, delivery delay variances, temporal calendar rhythm, commercial loss indicators, and freight routing lanes.
9. **Implement Dual-Sheet Workbook Architecture**: Architect an enterprise Excel deliverable separating operational commercial analysis from normalized machine learning features.
10. **Execute Independent Post-Export Audit**: Validate physical file integrity, sheet counts, row/column dimensions, and relational key parity on disk.

---

## 3. Project Workflow & Roadmap

The end-to-end analytical workflow strictly follows the sequence established in `roadmap.txt`:

```text
┌────────────────────────────────────────────────────────────────────────┐
│                        RAW DATASET INGESTION                           │
│     Logistic_Data_Analyst_Intern_Raw.csv (184,223 rows × 53 columns)       │
└───────────────────────────────────┬────────────────────────────────────┘
                                    │
                                    ▼
┌────────────────────────────────────────────────────────────────────────┐
│             PHASE 1: DATA CLEANING & QUALITY PREPROCESSING             │
│   (Notebook 1: Data_Cleaning_and_Preprocessing.ipynb | Steps 1 – 16)   │
├────────────────────────────────────────────────────────────────────────┤
│ • Step 1–5:  Ingestion, Latin-1 Encoding Audit & Primary Key Grain     │
│ • Step 6–8:  Controlled Deduplication (-3,704 Rows) & Clean Base Lock │
│ • Step 9–10: Missingness Remediation (+180,519 Product Descriptions)   │
│ • Step 11–12: Geographic & Text Hygiene; Dropped 9 Redundant/PII Cols  │
│ • Step 13–15: Outlier Domain Retention & StandardScaler (11 Z-Scores)  │
│ • Step 16:   Consolidated Cleaning Verification & Intermediate Lock    │
└───────────────────────────────────┬────────────────────────────────────┘
                                    │
                        [Cleaned Baseline Handoff]
                        180,519 rows × 55 columns
                                    │
                                    ▼
┌────────────────────────────────────────────────────────────────────────┐
│        PHASE 2: FEATURE ENGINEERING & WORKBOOK ARCHITECTURE            │
│ (Notebook 2: Feature_Engineering_and_Workbook_Architecture.ipynb)      │
├────────────────────────────────────────────────────────────────────────┤
│ • Step 17:   Operational Feature Engineering (+15 Domain Features)     │
│ • Step 17.9: Feature Engineering Validation & Feature-Set Lock         │
│              Final Dataset Dimensions: 180,519 rows × 70 columns       │
│ • Step 18.1–18.3: Architecture Planning & 9-Domain Narrative Ordering  │
│ • Step 18.4: Controlled Dual-Sheet Allocation & Relational Key Parity  │
│ • Step 18.5: OpenPyXL Workbook Serialization, Autofilters & Panes      │
│ • Step 18.6: Independent Post-Export Disk Verification (85.5 MB)       │
└───────────────────────────────────┬────────────────────────────────────┘
                                    │
                                    ▼
┌────────────────────────────────────────────────────────────────────────┐
│              FINAL ENTERPRISE EXCEL DELIVERABLE (STATUS: PASS)         │
│    Logistic_Data_Analyst_Final_Cleaned_Workbook_.xlsx (85.5 MB)    │
│    ├── Sheet 1: Logistic_Master_Business_Data (180,519 × 59)          │
│    └── Sheet 2: Logistic_ML_Analytics_Ready   (180,519 × 16)          │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 4. Dataset Information

### 4.1 Primary Raw Dataset: `Logistic_Data_Analyst_Intern_Raw.csv`
* **File Size**: 100,065,221 bytes (~100.1 MB)
* **Dimensions**: 184,223 rows × 53 columns
* **Source Context**: Global retail and logistics supply chain transactional log (sourced from Kaggle's DataCo Global Supply Chain dataset package).
* **Primary Key & Grain**: `Order Item Id`. Every individual record represents a single line-item within an order transaction.
* **Character Encoding**: ISO-8859-1 (Latin-1). Contains non-ASCII international characters in customer names, cities, and product descriptions.

### 4.2 Reference Description Dataset: `product_descriptions.csv`
* **File Size**: 36,015 bytes (~36.0 KB)
* **Dimensions**: 1,180 records × 2 columns (`Product Card Id`, `Product Description`)
* **Role**: Official auxiliary catalog dataset provided as part of the original Kaggle package. It serves as the single source of truth for deterministic recovery of the 180,519 missing product descriptions in the raw transactional file.

---

## 5. Raw Dataset vs. Final Processed Dataset

| Technical Dimension | Raw Ingestion Dataset | Cleaned Intermediate Dataset | Final Feature-Engineered Dataset | Final Professional Workbook |
| :--- | :--- | :--- | :--- | :--- |
| **Primary Filename** | `Logistic_Data_Analyst_Intern_Raw.csv` | `Logistic_Data_Analyst_Cleaned.csv` | `Logistic_Data_Analyst_Feature_Engineered_Week2.csv` | `Logistic_Data_Analyst_Final_Cleaned_Workbook_.xlsx` |
| **Storage Size** | 100.1 MB | 149.5 MB | 173.6 MB | 85.5 MB |
| **Total Record Count** | 184,223 rows | 180,519 rows | 180,519 rows | 180,519 rows (in both sheets) |
| **Total Attribute Count**| 53 columns | 55 columns | 70 columns | 75 allocated (59 Sheet 1, 16 Sheet 2) |
| **Duplicate Records** | 3,704 exact full rows | 0 duplicates | 0 duplicates | 0 duplicates |
| **Primary Key Uniqueness**| 97.9894% (3,704 clashes) | 100.0000% (180,519 unique) | 100.0000% (180,519 unique) | 100.0000% (180,519 unique) |
| **Product Descriptions** | 180,519 missing (100.0%) | 0 missing (100% catalog recovered) | 0 missing (100% catalog recovered) | 0 missing (100% catalog recovered) |
| **Customer Demographics**| 8 missing names, 3 zipcodes | 0 missing (deterministically filled)| 0 missing | 0 missing |
| **Geographic Hygiene** | Corrupted encodings & accents | 164 ISO English Countries | 164 ISO English Countries | 164 ISO English Countries |
| **Redundant / PII Columns**| 9 redundant columns present | 9 columns permanently dropped | 9 columns permanently dropped | Excluded |
| **Derived Features** | 0 engineered features | 11 StandardScaler Z-scores | 11 Scaled + 15 Domain Features | Separated across Sheets 1 & 2 |
| **Relational Integrity** | Unstructured flat file | Validated intermediate baseline | Fully locked analytical baseline | Dual-sheet linked via `Order Item Id` |
| **Data Integrity Status** | Severely Compromised | Clean Base Lock Verified | Feature Engineering Locked | **AUDIT PASSED (Zero Data Debt)** |

---

## 6. Two-Notebook Architecture

To maintain clear separation of concerns, reproducibility, and computational efficiency, the Week 2 implementation was structured into two specialized Jupyter notebooks:

### 6.1 Notebook 1 — Data Cleaning and Preprocessing
* **File**: `Logistic_Data_Analyst_Week2_Data_Cleaning_and_Preprocessing.ipynb`
* **Size / Cell Count**: 18.7 MB | 458 executable cells
* **Scope**: Steps 1 to 16 of `roadmap.txt`.
* **Core Responsibilities**:
  * Raw dataset safe loading and encoding verification.
  * Primary key uniqueness testing and row-level grain verification.
  * Duplicate identification, forensic analysis, and controlled removal.
  * Clean base data lock enforcement (Step 8).
  * Missing value treatment (relational catalog description recovery, demographic reconstruction).
  * Multi-tier geographic standardization (city, state, country, region).
  * Text normalization (whitespace trimming, case harmonization).
  * Column governance (dropping 9 redundant and PII attributes).
  * Numerical outlier analysis via IQR with domain justification.
  * Data type casting and datetime conversion.
  * Z-score standardization via `StandardScaler` across 11 continuous features.
  * Consolidated Step 16 integrity verification and export of `Logistic_Data_Analyst_Cleaned.csv`.

### 6.2 Notebook 2 — Feature Engineering and Workbook Architecture
* **File**: `Logistic_Data_Analyst_Week2_Feature_Engineering_and_Workbook_Architecture.ipynb`
* **Size / Cell Count**: 1.1 MB | 47 executable cells
* **Scope**: Steps 17 to 18.6 of `roadmap.txt`.
* **Core Responsibilities**:
  * Intermediate cleaned dataset ingestion and schema verification.
  * Formulation and mathematical implementation of 15 logistics domain features.
  * Feature integrity auditing (0 nulls, 0 infinities, logical boundary reconciliation).
  * Feature-set baseline locking at 180,519 rows × 70 columns (Step 17.9).
  * Architectural design of dual-sheet enterprise workbook.
  * Implementation of the 9-domain professional column ordering framework.
  * Controlled data allocation between Master Business Data and ML Analytics Ready sheets.
  * Workbook generation and serialization using `openpyxl` with autofilters and freeze panes.
  * Independent post-export disk verification audit (Step 18.6).

### 6.3 Architectural Rationale for Separation
1. **Separation of Governance and Enrichment**: Cleaning fixes historical data debt; feature engineering enriches valid data with forward-looking intelligence. Decoupling them ensures pipeline failures in enrichment never invalidate baseline cleaning.
2. **Reproducible Intermediate Checkpoint**: The intermediate export (`Logistic_Data_Analyst_Cleaned.csv`) serves as an immutable foundation for downstream analytics, business intelligence dashboards, and modeling.
3. **Memory Optimization**: Splitting execution avoids kernel exhaustion and browser latency when processing 180,000+ records across 70+ dimensions.

---

## 7. Data Cleaning and Preprocessing Deep-Dive

### 7.1 Duplicate Record Elimination (Steps 6–8)
* **Audit Finding**: Out of 184,223 raw records, exactly **3,704 rows** were identical across all 53 dimensions, causing identical `Order Item Id` values to appear multiple times.
* **Controlled Removal**: Purged 3,704 exact duplicates while preserving exactly 1 valid record per primary key.
* **Verification**: Post-deduplication rows = **180,519**. Unique `Order Item Id` values = **180,519** (100.0000% uniqueness). Zero row deletion permitted thereafter.

### 7.2 Missing Value Reconstruction (Steps 9–10)
* **Product Descriptions**: In the raw dataset, `Product Description` was 100% missing (180,519 nulls). Using `product_descriptions.csv`, descriptions were recovered through an inner relational join on `Product Card Id`. All 180,519 records received valid catalog descriptions with 0 remaining nulls.
* **Customer Demographics**: Identified 8 missing values in `Customer Lname` and 3 missing values in `Customer Zipcode`. These were deterministically reconstructed using historical customer profiles grouped by `Customer Id`.
* **Structural Cross-Border Nulls**: Audited 155,679 nulls (86.24%) in `Order Zipcode`. These were verified as legitimate international postal nulls (orders shipped outside the US/PR postal zone do not use 5-digit US ZIP codes) and preserved without synthetic distortion.

### 7.3 Geographic Standardization (Step 11)
* **City Standardization**: Resolved orthographic corruptions, accented characters, and encoding artifacts (e.g., `Goiânia` → `Goiania`, `Bra?ov` → `Brasov`, `Kahramanmara?` → `Kahramanmaras`).
* **State Standardization**: Harmonized non-English and accented provinces to English standard conventions (e.g., `Oregón` → `Oregon`, `Gy?r` → `Gyor`).
* **Country Standardization**: Mapped all international variations into standardized English country names across **164 unique countries** matching ISO conventions.

### 7.4 Global Text Cleaning & Column Governance (Step 12)
* **Whitespace & Casing**: Applied regex trimming to eliminate leading, trailing, and multi-space whitespace (1,543 text anomalies remediated) and harmonized categorical casing.
* **Column Redundancy Governance**: Dropped exactly 9 non-analytical, redundant, or PII columns:
  1. `Customer Password` (Critical Security / PII violation)
  2. `Product Image` (Dead URL / zero operational variance)
  3. `Product Status` (Zero variance constant = 0)
  4. `Sales per customer` (Exact arithmetic duplicate of `Order Item Total`)
  5. `Order Item Cardprod Id` (Exact duplicate of `Product Card Id`)
  6. `Order Customer Id` (Exact duplicate of `Customer Id`)
  7. `Product Category Id` (Exact duplicate of `Category Id`)
  8. `Customer Fname` (Redundant unstandardized name field)
  9. `Customer Lname` (Redundant unstandardized name field)
* **Canonical Schema**: Locked 44 base operational columns.

### 7.5 Outlier Analysis & Numerical Standardization (Steps 13–15)
* **Outlier Profiling**: Audited 9 continuous numerical variables using Interquartile Range ($IQR = Q3 - Q1$). All extreme operational records (e.g., negative profit commercial losses down to -\$4,200, bulk order quantities, high-value electronics) were intentionally retained to preserve commercial reality.
* **Data Type Casting**: Parsed `order date (DateOrders)` and `shipping date (DateOrders)` into standard `datetime64[ns]` timestamps.
* **StandardScaler Normalization**: Applied Z-score standardization ($z = \frac{x - \mu}{\sigma}$) to 11 continuous features, creating 11 scaled analytical columns with verified $\mu = 0.0000$ and $\sigma = 1.0000$.

---

## 8. Feature Engineering

Within Notebook 2 (Step 17), **15 domain-specific logistics features** were engineered to bridge raw transactional logs with strategic supply chain intelligence:

| # | Feature Name | Category | Formula / Logic | Logistics Purpose & Domain Value |
| :-: | :--- | :--- | :--- | :--- |
| **1** | `Delivery_Time_Hours` | Transit Metric | `Days for shipping (real) × 24.0` | Converts fulfillment transit duration from coarse days into hourly precision for SLA tracking. |
| **2** | `Promised_Time_Hours` | Transit Metric | `Days for shipment (scheduled) × 24.0` | Quantifies contractual carrier SLA commitment in operational hours. |
| **3** | `Delay_Hours` | Delay Metric | `Delivery_Time_Hours - Promised_Time_Hours` | Mathematical delivery variance. Positive = late; Zero = on-time; Negative = early fulfillment. |
| **4** | `Delay_Flag` | Delay Indicator | `1 if Delay_Hours > 0 else 0` | Binary classification indicator for shipment delays (empirical baseline: 54.77% delayed). |
| **5** | `Performance_Category` | Delay Tiers | `'Late' if Delay_Hours > 0, 'On-Time' if == 0, 'Early' if < 0` | Multi-class operational fulfillment categorization for carrier benchmarking. |
| **6** | `Delay_Severity_Level` | Delay Tiers | Tiers: `None/Early`, `Minor (1–24h)`, `Moderate (25–48h)`, `Severe (>48h)` | Granular triage metric identifying critical carrier delivery bottlenecks requiring escalation. |
| **7** | `Order_Year` | Temporal Calendar | `order_date.dt.year` (2015 – 2018) | Annual temporal aggregation anchor for multi-year trend and cohort analysis. |
| **8** | `Order_Month` | Temporal Calendar | `order_date.dt.month` (1 – 12) | Monthly seasonality index for inventory planning and demand forecasting. |
| **9** | `Order_Month_Name` | Temporal Calendar | `order_date.dt.month_name()` (January – December) | Human-readable calendar month for executive reporting and BI visualizations. |
| **10** | `Order_Day_of_Week` | Temporal Calendar | `order_date.dt.day_name()` (Monday – Sunday) | Weekly operational rhythm indicator identifying warehouse throughput surges. |
| **11** | `Is_Weekend_Order` | Temporal Calendar | `1 if day in ['Saturday', 'Sunday'] else 0` | Flags weekend purchasing patterns to evaluate off-peak order processing latency. |
| **12** | `Is_Commercial_Loss` | Commercial Loss | `1 if Benefit per order < 0 else 0` | Flags unprofitable orders (18.71% incidence) for pricing strategy and margin protection. |
| **13** | `Logistics_Corridor_ID` | Network Routing | `Market + ' -> ' + Order Region` | Macro-level regional supply chain lane identifier (e.g., `Europe -> Western Europe`). |
| **14** | `Shipping_Lane_ID` | Network Routing | `Customer State + ' -> ' + Order Country` | Micro-level fulfillment route identifier connecting dispatch origin to destination market. |
| **15** | `Is_Cross_Border` | Freight Regulation | `1 if Customer Country != Order Country else 0` | Identifies international cross-border trade (13.84% incidence) requiring customs clearance. |

---

## 9. Feature Engineering Validation & Feature Set Lock

In Step 17.9, a consolidated mathematical and schema audit was executed across all 15 engineered features:

* **Completeness Verification**: Verified that all 15 features were populated across **100% of the 180,519 records** with **exactly 0 missing values** and **0 infinite values**.
* **Logical Boundary Reconciliation**:
  * Confirmed that `Delay_Flag == 1` strictly corresponds to instances where `Delay_Hours > 0` (exactly 98,877 records; 54.77%).
  * Reconciled `Performance_Category` sums: Delayed (98,877), Early (63,889), On Time (17,753) = exactly 180,519 records.
  * Verified `Is_Commercial_Loss` matches `Benefit per order < 0` (33,779 records; 18.71%).
* **Schema Evolution**:
  $$\text{Cleaned Base (55 Columns)} + \text{Engineered Features (15 Columns)} = \mathbf{70\text{ Total Columns}}$$
* **Feature Set Baseline Lock**: The 70-column dataset was locked and serialized to `Logistic_Data_Analyst_Feature_Engineered_Week2.csv` (173.6 MB).

---

## 10. Professional Excel Workbook Architecture

The final deliverable, `Logistic_Data_Analyst_Final_Cleaned_Workbook_.xlsx` (85.5 MB), implements a **dual-sheet relational architecture** designed to serve two distinct enterprise user groups:

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│              Logistic_Data_Analyst_Final_Cleaned_Workbook_.xlsx             │
├───────────────────────────────────────┬─────────────────────────────────────────┤
│    Sheet 1: Master Business Data      │     Sheet 2: ML Analytics Ready         │
│      180,519 rows × 59 columns        │       180,519 rows × 16 columns         │
├───────────────────────────────────────┼─────────────────────────────────────────┤
│ • Unscaled operational values ($ USD) │ • 10 Standardized Z-scores (μ=0, σ=1)   │
│ • Full customer & order geography     │ • Continuous fulfillment & sales scales │
│ • Complete product & catalog info     │ • Algorithmic target features           │
│ • All 15 engineered domain indicators │ • Pre-processed for clustering/modeling │
│ • 11 scaled features EXCLUDED         │ • Order Zipcode_scaled EXCLUDED         │
├───────────────────────────────────────┴─────────────────────────────────────────┤
│             Relational Join Key: Order Item Id (100% 1-to-1 Parity)            │
└─────────────────────────────────────────────────────────────────────────────────┘
```

### 10.1 Sheet 1: `Logistic_Master_Business_Data` (59 Columns)
* **Audience**: Supply chain managers, financial controllers, and commercial operations analysts.
* **Design Philosophy**: Contains exclusively unscaled, human-interpretable metrics: original currency values ($ USD), calendar dates, and physical durations (hours and days).
* **Column Allocation**:
  * 44 Canonical base columns + 15 Engineered business features = **59 columns**.
  * **All 11 scaled Z-score features are deliberately excluded** to prevent commercial misinterpretation.
* **Professional 9-Domain Narrative Ordering**:
  1. *Relational Identifiers* (Cols 1–3): `Order Item Id`, `Order Id`, `Customer Id`
  2. *Customer Demographics* (Cols 4–11): Names, Segment, City, State, Country, Zipcode, Email
  3. *Destination Network Geography* (Cols 12–17): Order City, State, Country, Region, Zipcode, Market
  4. *Product Catalog Details* (Cols 18–24): Product Card ID, Name, Description, Category, Department
  5. *Commercial Ledger & Financials* (Cols 25–32): Price, Quantity, Discount, Total, Sales, Benefit, Profit Ratio
  6. *Fulfillment & Logistics Operations* (Cols 33–39): Type, Delivery Status, Late Risk, Shipping Mode, Real/Scheduled Days, Order Status
  7. *Temporal Timestamps* (Cols 40–41): Order Date, Shipping Date
  8. *Engineered Logistics Intelligence* (Cols 42–56): Transit hours, delays, categories, calendar indicators, loss flag, corridor/lane IDs, cross-border indicator
  9. *Spatial Coordinates* (Cols 57–59): Customer Street, Latitude, Longitude

### 10.2 Sheet 2: `Logistic_ML_Analytics_Ready` (16 Columns)
* **Audience**: Data scientists, machine learning engineers, and advanced predictive modelers.
* **Design Philosophy**: Contains mathematically standardized continuous variables and key operational targets, optimized for immediate algorithmic ingestion (clustering, regression, neural networks).
* **Column Allocation**:
  * 1 Primary Relational Key: `Order Item Id`
  * 10 Standardized Continuous Z-Scores: `Days for shipping (real) scaled`, `Days for shipment (scheduled) scaled`, `Benefit per order scaled`, `Order Item Discount scaled`, `Order Item Discount Rate scaled`, `Order Item Product Price scaled`, `Order Item Profit Ratio scaled`, `Order Item Quantity scaled`, `Sales scaled`, `Order Item Total scaled`
  * 5 Key Operational Modeling Targets: `Delivery Time Hours`, `Promised Time Hours`, `Delay Hours`, `Delay Flag`, `Late delivery risk`
  * *Note*: `Order Zipcode_scaled` was excluded because postal codes are discrete nominal categories rather than continuous analytical measurements.

---

## 11. Post-Export Workbook Integrity Verification

In Step 18.6, an automated, independent audit was executed on the serialized Excel workbook file using Python's `openpyxl` library:

| Verification Parameter | Required Architecture Specification | Physical File Audit Finding | Audit Status |
| :--- | :--- | :--- | :---: |
| **Physical File Existence** | File present on storage disk | Located at target workspace path | **PASSED** |
| **File Size Validation** | Expected ~80 – 90 MB enterprise workbook | **85,541,052 bytes (~85.54 MB)** | **PASSED** |
| **Sheet Count & Names** | Exactly 2 sheets: Master Business + ML Analytics | `['Logistic_Master_Business_Data', 'Logistic_ML_Analytics_Ready']` | **PASSED** |
| **Sheet 1 Dimensions** | Exactly 180,519 rows × 59 columns | **180,519 rows × 59 columns** | **PASSED** |
| **Sheet 2 Dimensions** | Exactly 180,519 rows × 16 columns | **180,519 rows × 16 columns** | **PASSED** |
| **Relational Key Presence** | `Order Item Id` present in Column 1 of both sheets | Verified as primary column in both sheets | **PASSED** |
| **Relational Key Uniqueness** | Exactly 180,519 unique keys (0 duplicates) | **180,519 unique keys (100.0000%)** | **PASSED** |
| **Missing Key Values** | Exactly 0 nulls in relational key | **0 nulls in both sheets** | **PASSED** |
| **Cross-Sheet Key Parity** | 100% 1-to-1 alignment; zero orphan records | `df_master['Order Item Id'].equals(df_ml['Order Item Id']) == True` | **PASSED** |
| **Workbook Formatting** | Freeze panes on row 2, autofilters enabled | Freeze panes and header filters configured | **PASSED** |
| **Final Compliance Sign-Off** | Full conformance with Week 2 requirements | **ZERO DATA DEBT / 100% COMPLIANCE** | **PASSED** |

---

## 12. Technologies and Tools Used

| Technology / Tool | Purpose in Week 2 Implementation |
| :--- | :--- |
| **Python 3** | Primary programming language used for all ingestion, data transformation, validation, and serialization. |
| **Jupyter Notebook** | Interactive development environment utilized for reproducible two-notebook workflow execution. |
| **Google Colab** | Cloud execution runtime supporting high-memory notebook computations. |
| **Pandas (`pd`)** | Core data manipulation library utilized for DataFrame ingestion, merging, datetime parsing, indexing, and aggregation. |
| **NumPy (`np`)** | High-performance numerical computing library utilized for vectorization, array manipulation, and condition indexing. |
| **Scikit-learn (`sklearn`)** | Machine learning preprocessing library deploying `StandardScaler` for continuous feature Z-score standardization. |
| **OpenPyXL** | Enterprise Excel library utilized for workbook generation, dual-sheet creation, styling, autofilters, and disk verification. |
| **Regular Expressions (`re`)** | Text processing engine used for whitespace auditing, string hygiene, and character standardization. |
| **Plotly Graph Objects** | Interactive visualization engine utilized to build the standalone Week 2 technical roadmap figure. |
| **Microsoft Excel** | Enterprise spreadsheet software validating workbook usability, freeze panes, multi-sheet navigation, and formula integrity. |

---

## 13. Python Libraries & Dependencies

The complete Python environment utilized across both notebooks includes:

```python
# Data Analysis and Core Computation
import pandas as pd
import numpy as np

# Machine Learning Preprocessing
from sklearn.preprocessing import StandardScaler

# Excel Serialization and Formatting
import openpyxl
from openpyxl import load_workbook
from openpyxl.utils import get_column_letter

# String Hygiene, System & File Utilities
import re
import os
from pathlib import Path

# Interactive Reporting & Visualizations
import plotly.graph_objects as go
from IPython.display import display
```

---

## 14. Key Data Quality Improvements

| Data Quality Dimension | Initial State (Raw Dataset) | Final State (Cleaned & Delivered) | Quantitative Impact |
| :--- | :--- | :--- | :--- |
| **Record Duplication** | 3,704 exact duplicate rows | Exactly 0 duplicate rows | **-3,704 redundant records purged** |
| **Primary Key Grain** | 3,704 primary key collisions | Exactly 180,519 unique `Order Item Id` keys | **100.0000% primary key uniqueness locked** |
| **Product Descriptions** | 180,519 nulls (100% missing) | 180,519 catalog descriptions populated | **100.0% catalog recovery (0 nulls remaining)** |
| **Customer Demographics** | 8 missing names, 3 missing zipcodes | 0 missing demographic values | **100% profile resolution via Customer Id** |
| **Geographic Standardization**| Accents, question marks, non-ISO names | Clean ISO English names across 164 countries | **100% administrative boundary normalization** |
| **Text Consistency** | Inconsistent casing, trailing spaces | Trimmed, unified title casing | **1,543 text whitespace defects corrected** |
| **Schema Governance** | 9 redundant/PII columns present | 9 non-analytical/PII columns eliminated | **Clean canonical 44-attribute base schema** |
| **Numerical Distribution** | Raw disparate scales | 11 continuous features Z-score standardized | **$\mu = 0.0000, \sigma = 1.0000$ machine learning readiness** |
| **Behavioral Features** | Zero derived logistics indicators | 15 domain-specific logistics features | **15 actionable operational metrics engineered** |
| **Workbook Architecture** | Single flat CSV file | Enterprise dual-sheet Excel deliverable | **Separation of commercial & ML data (PASS)** |

---

## 15. Final Deliverables

The Week 2 internship project generated the following verified deliverable files:

1. **`Logistic_Data_Analyst_Cleaned.csv`** (149.5 MB) — Intermediate cleaned dataset containing 180,519 rows × 55 columns (44 base + 11 scaled).
2. **`Logistic_Data_Analyst_Final_Cleaned_Workbook_.xlsx`** (85.5 MB) — Enterprise Excel workbook featuring `Logistic_Master_Business_Data` (59 cols) and `Logistic_ML_Analytics_Ready` (16 cols).
3. **`Logistic_Data_Analyst_Week2_Data_Cleaning_and_Preprocessing.ipynb`** (18.7 MB) — Executed Jupyter notebook for Phase 1 data cleaning, missingness remediation, and standardization.
4. **`Logistic_Data_Analyst_Week2_Feature_Engineering_and_Workbook_Architecture.ipynb`** (1.1 MB) — Executed Jupyter notebook for Phase 2 feature engineering, workbook allocation, and export.
5. **`Data Cleaning, Feature Engineering and Analytical Architecture in Logistics_Week2_Report.docx`** (2.8 MB) — Comprehensive 46-page formal internship report complete with executive summaries, visual figures, and forensic script outputs.
6. **`Data Cleaning, Feature Engineering and Analytical Architecture in Logistics_Week2_Report.pdf`** (1.81 MB) — Comprehensive 46-page formal internship report complete with executive summaries, visual figures, and forensic script outputs.
7. **`README.md`** — Comprehensive GitHub-quality project documentation.

---

## 16. Project File Structure

```text
Week_2/
├── Raw Dataset/
│      ├── Logistic_Data_Analyst_Intern_Raw.csv
│      └── product_descriptions.csv
├── Cleaned Dataset/
│      └── Logistic_Data_Analyst_Cleaned.csvx
├── Final Cleaned Workbook Dataset/
│       └── Logistic_Data_Analyst_Final_Cleaned_Workbook_.xlsx
├── Python Notebooks/
│   ├── Logistic_Data_Analyst_Week2_Data_Cleaning_and_Preprocessing.ipynb
│   └── Logistic_Data_Analyst_Week2_Feature_Engineering_and_Workbook_Architecture.ipynb
└── Week 2 Final Report/
│    └── Data Cleaning, Feature Engineering and Analytical Architecture in Logistics_Week2_Report.docx
    └── Data Cleaning, Feature Engineering and Analytical Architecture in Logistics_Week2_Report.pdf
└── README.md                                                     # Complete Project Technical Documentation
```

---

## 17. Key Results Summary

* **Raw Input Volume**: 184,223 rows × 53 columns (100.1 MB)
* **Exact Duplicate Purge**: 3,704 records permanently removed
* **Locked Population Baseline**: **180,519 records** (100.0000% primary key uniqueness)
* **Recovered Descriptions**: **180,519 catalog records** (0 missing descriptions remaining)
* **Approved Column Drops**: 9 redundant/PII columns eliminated
* **Canonical Schema**: 44 base attributes locked
* **StandardScaler Features**: 11 continuous features standardized ($\mu = 0.0000, \sigma = 1.0000$)
* **Approved Engineered Features**: **15 domain-specific operational indicators**
* **Total Engineered Dimensions**: **70 columns** across 180,519 rows
* **Final Workbook Physical Size**: **85,541,052 bytes (~85.5 MB)**
* **Workbook Architecture**:
  * `Logistic_Master_Business_Data`: 180,519 rows × 59 columns
  * `Logistic_ML_Analytics_Ready`: 180,519 rows × 16 columns
* **Relational Key Alignment**: `Order Item Id` (180,519 matching keys; 0 orphans; 100% 1-to-1 match)
* **Post-Export Verification Status**: **PASS (Zero Data Debt)**

---

## 18. Practical Skills Demonstrated

* **Advanced Data Cleaning**: Forensic auditing, row-level grain verification, exact deduplication, non-destructive data locking.
* **Deterministic Missing Value Recovery**: Multi-table relational joins, catalog lookups, entity-based demographic reconstruction.
* **Text & Geographic Standardization**: Regular expression whitespace normalization, diacritic stripping, ISO geographic harmonization across 164 countries.
* **Feature Engineering & Transformation**: Domain mathematical formulation, temporal date decomposition, binary indicator creation, routing lane concatenation.
* **Machine Learning Preprocessing**: Statistical outlier analysis via IQR, feature scaling using Scikit-learn `StandardScaler`.
* **Enterprise Excel Architecture**: Separation of commercial and analytical concerns, 9-domain column ordering, `openpyxl` programmatic serialization, freeze panes, autofilters.
* **Data Quality Governance & Auditing**: Post-export file verification, relational key parity checking, zero-data-debt sign-off.
* **Technical Communication**: Technical reporting, two-notebook pipeline engineering, interactive Plotly visualization design.

---

## 19. Final Project Status

Week 2 of the Logistics Data Analyst Internship has been **successfully completed with full verification across all roadmap checkpoints**. 

The dataset transitioned from a raw, unstandardized transactional CSV file containing over 184,000 messy records into a **governed, feature-rich, dual-sheet enterprise Excel deliverable** and an intermediate analytics-ready CSV baseline. Every stage of the transformation has been mathematically audited, verified for zero data leakage, and documented in production-grade code. The resulting assets stand fully prepared for Week 3 advanced predictive modeling, carrier delay classification, and executive dashboard deployment.
