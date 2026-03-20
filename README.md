# EquiPay

---

## Executive Summary
Gig economy platforms often fail to account for external variables that impede worker efficiency. This document outlines a system designed to bridge the gap between effort and earnings by utilizing **Machine Learning (ML)** and a **Root Mean Square (RMS)** compensation model to adjust weekly payouts based on real-world delivery friction.

---

## Problem Statement

Gig workers, particularly delivery personnel, operate in environments where their efficiency is decoupled from their effort due to external stressors.

**Key challenges include:**

- **Adverse Weather**: Rainfall, extreme heat, or cold that slows transit  
- **Heavy Traffic**: Unforeseen congestion and road closures  
- **Curfew Restrictions**: Legal or safety-related hour limitations  
- **Fraudulent Behavior**: False complaints, order rejections, and "no-show" scenarios  

**Objective:**  
To design an AI-driven system that ensures fair weekly payouts by compensating for inefficiencies caused by these external factors while excluding personal factors like health or vehicle maintenance.

---

## Mathematical Model

### Base Metrics

Let the daily salary be \( S_d \), where \( d \in \{1,2,\dots,7\} \)

The standard weekly salary is:

$$
S_w = \sum_{d=1}^{7} S_d
$$

---

### Inefficiency Calculation

Total inefficiency \( \eta \) is:

$$
\eta = \eta_{bw} + \eta_{tr} + \eta_{cr}
$$

We introduce a scaling factor \( \alpha \), generated via ML models, to adjust weights dynamically.

---

### RMS Compensation Function

$$
RMS = \sqrt{\frac{1}{n} \sum_{i=1}^{n} (\alpha \cdot \eta_i)^2}
$$

---

### Final Adjusted Salary

$$
S_f = S_w + RMS
$$

---

## System Architecture
![System Architecture](Architecture.jpeg)
![flowchart image](https://postimg.cc/yWML8nR5)
### Technology Stack

| Layer      | Technology                          | Key Features |
|-----------|------------------------------------|-------------|
| Frontend  | MERN (React.js)                    | Real-time dashboards |
| Backend   | Node.js / Express                  | High concurrency APIs |
| ML Engine | Python (Scikit-learn / TensorFlow) | Analysis + scaling |
| Database  | MongoDB & PostgreSQL               | Logs + financial data |

---

### Module Responsibilities

- **Inefficiency Analyst**  
  Integrates with APIs to calculate \( \eta_{bw} \) and \( \eta_{tr} \)

- **Fraud Guard**  
  Uses anomaly detection for complaints and fraud

- **Payout Engine**  
  Executes RMS formula at the end of billing cycle

---

## Data Schema Design

### Core Entities

- Worker  
- Daily Earnings  
- Inefficiency Factors  
- Claims  

---

### Workers Table

| Column | Type | Description |
|-------|------|------------|
| worker_id | SERIAL (PK) | Unique ID |
| name | VARCHAR | Full name |
| primary_zone_polygon | TEXT | Area data |
| avg_hourly_earnings | FLOAT | Benchmark |

---

### Daily Earnings Table

| Column | Type | Description |
|-------|------|------------|
| earning_id | SERIAL (PK) | Entry ID |
| worker_id | INT (FK) | Reference |
| base_salary | FLOAT | Original |
| adjusted_salary | FLOAT | Final |

---

### Inefficiency Factors Table

| Column | Type | Description |
|-------|------|------------|
| factor_id | SERIAL (PK) | Record ID |
| eta_bw | FLOAT | Weather score |
| eta_tr | FLOAT | Traffic score |
| alpha | FLOAT | ML scaling factor |

---

## Key Advantages & Conclusion

### Advantages

1. **Equity**: Removes randomness in earnings  
2. **Retention**: Improves worker satisfaction  
3. **Accuracy**: RMS avoids bias from extreme values  
4. **Security**: Fraud detection prevents misuse  

---

### Conclusion

This system represents a shift from **"Fixed Payouts"** to **"Fair Payouts"**. By leveraging statistical modeling and machine learning, platforms can protect workers from uncontrollable external variables and build a more sustainable gig economy.
