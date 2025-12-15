# Cold Chain Risk Analysis Using EPCIS Traceability Data

## Overview
Cold-chain integrity is traditionally monitored using temperature sensors and IoT devices. However, operational failures in cold chains are also strongly driven by **process complexity**—including excessive handling, complex routing, and extended dwell times.  

This project demonstrates how **EPCIS 2.0 traceability event data** can be transformed into actionable process-level risk indicators, even in the absence of explicit temperature measurements.

Using a synthetic but standards-compliant food supply chain dataset, the analysis quantifies cold-chain process complexity and predicts high-risk shipments based on early-stage traceability signals.

---

## Problem Statement
Temperature excursions and quality degradation in cold chains often occur due to long dwell times and repeated handling across complex distribution networks. These risks are typically detected **after delivery**, when corrective action is no longer possible.

The key problem addressed in this project is:

**How can EPCIS event data be used to quantify and predict cold-chain process risk driven by excessive time and operational complexity?**

---

## Business Motivation
From an operations perspective:
- A small subset of shipments often accounts for a disproportionate share of delays and risk.
- Manual monitoring of all shipments is inefficient and costly.
- Early identification of high-risk chains enables **risk-based monitoring**, targeted interventions, and network simplification.

This project reframes cold-chain monitoring from reactive temperature alerts to **predictive, process-driven risk management**.

---

## Data Description
The analysis uses the **EPCIS Synthetic Food Supply Chain Dataset** published on Kaggle, encoded in EPCIS 2.0 JSON format :contentReference[oaicite:0]{index=0}.

Key characteristics:
- ~5,650 EPCIS events
- 305 unique item / batch chains (epcClass-level)
- Event types include ObjectEvent, AggregationEvent, TransformationEvent, and AssociationEvent
- Each event records *what happened, when, where, and under which business context*

The dataset represents item-level traceability rather than pallet-level aggregation.

---

## Methodology
The analysis follows a structured analytics pipeline:

1. **Event Flattening**  
   EPCIS JSON events are transformed into a tabular structure by expanding quantity lists and extracting core attributes such as event time, business step, and location.

2. **Chain-Level Feature Engineering**  
   Events are aggregated by item/batch to derive:
   - Total chain duration (hours)
   - Number of handling events
   - Number of unique locations visited
   - Number and type of business steps (shipping, receiving, etc.)

3. **Risk Definition**  
   In the absence of temperature data, **long dwell time** is used as a proxy for elevated cold-chain risk.  
   Shipments in the **top 25% of chain duration** are labelled as high-risk.

4. **Statistical Analysis**  
   Hypothesis tests (t-test and ANOVA) are used to evaluate whether long dwell times are associated with specific origin locations.

5. **Predictive Modelling**  
   Supervised learning models are trained to predict high-risk chains:
   - Logistic Regression
   - Random Forest (class-balanced)

Model performance is evaluated using accuracy, recall, precision, and ROC–AUC.

---

## Key Results
- Cold-chain process metrics exhibit **heavy right-tailed distributions**:
  - A minority of shipments experience extremely long dwell times (up to ~664 days).
  - These same shipments also show high handling intensity and network complexity.
- Long dwell times are **systemic**, not attributable to specific origin facilities.
- Predictive performance:
  - Logistic Regression: ~97% accuracy, ROC–AUC ≈ 0.99
  - Random Forest: ~92% accuracy, ROC–AUC ≈ 0.96
- The strongest predictors of risk are:
  - Number of events
  - Number of locations
  - Shipping and receiving frequency

---

## Operational Interpretation
The findings indicate that **process complexity is a primary driver of cold-chain risk**.

In a real deployment, this approach could be used to:
- Flag high-risk shipments early for enhanced monitoring
- Prioritise temperature logging and telematics resources
- Identify and redesign overly complex routing structures
- Support continuous improvement in cold-chain network design

This complements traditional sensor-based monitoring by predicting *where risk is likely to occur*, not just detecting it after the fact.

---

## Limitations
- The dataset is synthetic and does not include explicit temperature or quality measurements.
- Risk is inferred indirectly through time and complexity metrics.
- Results should be interpreted as a proof-of-concept for real-world EPCIS-based analytics.

---

## Future Work
Potential extensions include:
- Integrating real temperature sensor data with EPCIS events
- Cost-sensitive threshold optimisation for operational alerts
- Survival analysis for time-to-delay modelling
- Deployment as a real-time dashboard for planners and operations teams

---

## Repository Structure
notebooks/ → Data preparation, analysis, and modelling
data/ → Dataset description or external data references
results/ → Figures and visual outputs

---

## Author
Vansh Singh

