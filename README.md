# Smart Dynamic Pricing for Urban Parking Spaces

---

## Introduction

Parking in urban areas is a limited and highly sought-after resource, often impacted by static pricing mechanisms that lead to inefficiencies—either in the form of underused spots or excessive demand.

This project explores and implements **dynamic pricing algorithms** using real-time data to improve efficiency. Two pricing models were developed:

- **Model 1:** Linear Occupancy-Based Pricing (Baseline)  
- **Model 2:** Multi-Factor Dynamic Demand-Based Pricing

Both approaches utilize **numpy** and **pandas**, with parameter tuning applied for optimal pricing control.

---

## Dataset Overview

- Collected from **14 parking lots** across a span of **73 days**
- **18 data points per day** (half-hour intervals from 8:00 AM to 4:30 PM)
- Key features include:
  - Occupancy  
  - Total Capacity  
  - Queue Length  
  - Traffic Conditions  
  - Special Day Indicator  
  - Vehicle Type  

---

## Model 1: Linear Pricing Based on Occupancy

### Pricing Logic:

The model updates price according to shifts in occupancy levels:

NewPrice_t = PreviousPrice_t + α × (Occupancy_t - PrevOccupancy_t) / (Capacity - PrevOccupancy_t)

yaml
Copy
Edit

- **α (alpha):** Sensitivity factor governing price change based on occupancy

---

### Key Assumptions:

- A fixed base price of \$10 at the start of each day  
- Price changes linearly with occupancy differences  
- External factors are not considered in this model  

---

### Parameter Tuning:

Performed a **grid search** on alpha to minimize:

- **Mean of daily standard deviation** in price fluctuations

---

## Model 2: Multi-Factor Dynamic Pricing

### Demand Estimation:

A weighted combination of various demand drivers:

Demand = α × (Occupancy / Capacity) + β × QueueLength − γ × TrafficCondition + δ × IsSpecialDay + ε × VehicleTypeWeight

csharp
Copy
Edit

Final price is determined as:

NewPrice_t = BasePrice × (1 + λ × NormalizedDemand)

yaml
Copy
Edit

- **λ** (lambda): Scaling factor (0.6 used)  
- Price bounded within **0.5× to 2×** the base price

---

### Assumptions:

- Demand drivers influence pricing linearly  
- Demand is normalized on a per-day basis  
- Prices are constrained to avoid extreme fluctuations  
- Categorical variables mapped as:
  - Traffic: `{Low: 0.3, Medium: 0.6, High: 1.0}`  
  - Vehicle Types: `{Car: 1, Bike: 0.7, Truck: 1.3}`

---

### Optimization Strategy:

Used **grid search** to tune parameters (α, β, γ, δ, ε) to minimize:

- **Average day-wise price fluctuation (standard deviation)**

---

## Pricing Behavior Insights:

- **High Occupancy / Queue Length:** Increases demand → leads to higher prices  
- **High Traffic Levels:** Suppresses demand (parking becomes harder to access)  
- **Special Days:** Demand and prices rise  
- **Vehicle Type:** Adjusts demand based on category weight  

Pricing stability is achieved through:

- Demand normalization by day  
- Pricing caps  
- Objective-driven parameter tuning  

---

## Model Evaluation

Following project evaluation norms:

- No fixed quantitative metric  
- Models assessed qualitatively based on:
  - Smoothness of price variation  
  - Logical response to real-time demand  
  - Clarity and transparency of the pricing formula  

---

## Summary & Conclusion

- **Model 1** provides a simple, easy-to-understand baseline  
- **Model 2** incorporates more nuanced variables, resulting in more accurate and realistic price adjustments  
- Parameter optimization ensures balanced and steady price changes  

---

## Possible Enhancements:

- Add revenue/profit-focused optimization  
- Introduce dynamic constraints for responsiveness  
- Explore non-linear or ML-based demand prediction for adaptive tuning  

---

## Project Deliverables:

- Fully annotated Google Colab notebook (with Bokeh visualizations)  
- This structured report covering:
  - Pricing logic and equations  
  - Model assumptions  
  - Price response behavior  
  - Visual and analytical insights  
  - Evaluation commentary  

---
