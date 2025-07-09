# AdityaParate_SA_Capstone_2025

Dynamic Pricing for Urban Parking Lots
## Overview
This project implements an intelligent, data-driven dynamic pricing engine for 14 urban parking spaces using real-time data streams. The system adjusts parking prices based on demand fluctuations, occupancy patterns, and other environmental factors to optimize utilization and revenue.
The project was developed as part of the Summer Analytics 2025 Capstone Project hosted by Consulting & Analytics Club × Pathway.
Tech Stack

Programming Language: Python 3.x
Core Libraries:

numpy - Numerical computations
pandas - Data manipulation and analysis
pathway - Real-time data streaming and processing


Visualization: bokeh - Interactive real-time visualizations
Environment: Google Colab
Version Control: Git & GitHub

Architecture Diagram

    graph TB
    A[Raw Parking Data] --> B[Pathway Data Ingestion]
    B --> C[Real-time Data Stream]
    C --> D[Feature Engineering]
    D --> E[Dynamic Pricing Models]
    E --> F[Model 1: Baseline Linear]
    E --> G[Model 2: Demand-Based Function]
    E --> H[Model 3: Competitive Pricing]
    
    F --> I[Price Calculation Engine]
    G --> I
    H --> I
    
    I --> J[Real-time Price Updates]
    J --> K[Bokeh Visualization Dashboard]
    
    L[Historical Data] --> M[Windowed Aggregation]
    M --> N[Daily Fluctuation Analysis]
    N --> I
    
    subgraph "Data Processing Pipeline"
        B
        C
        D
        M
        N
    end
    
    subgraph "Pricing Models"
        F
        G
        H
        I
    end
    
    subgraph "Output & Visualization"
        J
        K
    end

## Project Architecture and Workflow

1. Data Ingestion & Preprocessing
The system processes parking data from 14 urban parking spaces collected over 73 days with 18 time points per day (8:00 AM to 4:30 PM, 30-minute intervals).
Data Features:

Location Information: Latitude, Longitude
Parking Lot Features: Capacity, Occupancy, Queue Length
Vehicle Information: Type (car, bike, truck)
Environmental Conditions: Traffic congestion, Special day indicators

2. Real-time Data Streaming with Pathway
The project uses Pathway for real-time data processing:

Time-based windowing for daily aggregation

delta_window = data_with_time.windowby(
    pw.this.t,  # Event time column
    instance=pw.this.day,  # Daily partitioning
    window=pw.temporal.tumbling(datetime.timedelta(days=1)),
    behavior=pw.temporal.exactly_once_behavior()
)

3. Pricing Models Implementation
Model 1: Baseline Linear Model
A simple linear relationship between occupancy and price:
Price(t+1) = Price(t) + α × (Occupancy/Capacity)

Model 2: Demand-Based Price Function
Advanced model incorporating multiple demand factors:

Demand = α×(Occupancy/Capacity) + β×QueueLength - γ×Traffic + δ×IsSpecialDay + ε×VehicleTypeWeight

Price(t) = BasePrice × (1 + λ × NormalizedDemand)

4. Real-time Processing Pipeline

Data Ingestion: Pathway streams parking data with timestamp preservation
Windowed Aggregation: Daily windows calculate min/max occupancy patterns
Feature Engineering: Extract demand indicators and fluctuation metrics
Price Calculation: Apply dynamic pricing formula
Real-time Updates: Continuously emit updated prices
Visualization: Bokeh dashboard displays live pricing trends

5. Visualization Dashboard
Real-time Bokeh visualizations include:

Price Trend Lines: Live pricing for all 14 parking spaces

Occupancy Patterns: Historical and current occupancy rates

Demand Indicators: Queue length, traffic levels, special events

Competitor Comparison: Price comparison with nearby lots (Model 3)

## Key Features

Real-time Processing: Pathway enables continuous data streaming and processing

Scalable Architecture: Modular design supports multiple pricing models

Smooth Price Transitions: Bounded price variations prevent erratic changes

Business Intelligence: Competitive analysis and rerouting suggestions

Interactive Visualization: Live dashboard for monitoring and analysis

## Model Assumptions

Base Price: $10 represents the minimum viable price point

Demand Fluctuation: Higher daily occupancy variance indicates higher demand volatility

Capacity Normalization: Fluctuation is normalized by capacity to ensure fair comparison across different lot sizes

Time Windows: Daily aggregation captures meaningful demand patterns

Price Bounds: Prices are implicitly bounded to prevent extreme values

## Future Enhancements

Machine Learning Integration: Implement ML models for demand prediction

Weather Integration: Include weather data for better demand forecasting

Dynamic Base Pricing: Adjust base price based on seasonal/weekly patterns

Multi-objective Optimization: Balance revenue optimization with utilization efficiency

Advanced Routing: Implement sophisticated vehicle rerouting algorithms
