# 📈 Real-Time Dynamic Pricing Engine for Urban Parking



*A live demonstration of the dashboard visualizing real-time price adjustments for all 14 parking lots.*

---

## 📋 Project Overview

This project implements a sophisticated, data-driven dynamic pricing engine for a system of 14 urban parking lots. Moving beyond inefficient static pricing, this solution leverages a real-time data streaming pipeline built with **Pathway** to adjust parking fees based on a multi-layered analysis of live demand, traffic, vehicle types, special events, and competitor pricing.

The system ingests a continuous stream of parking data, applies the intelligent pricing algorithm, and visualizes the results on a "mission control" style dashboard, providing a complete, live overview of the entire parking network.

---

## ✨ Key Features

-   🧠 **Multi-Layered Pricing Strategy**: A sophisticated model that goes beyond simple occupancy to consider a rich set of real-world factors.
-   🗺️ **Competitive Location Intelligence**: Uses geographic coordinates (Haversine formula) to identify nearby competitors (within a 2 km radius) and strategically adjusts prices to stay attractive and maximize revenue.
-   🚗 **Context-Aware Modifiers**: Prices are intelligently modified based on:
    -   **Traffic Conditions**: A 15% price surge during 'high' traffic.
    -   **Vehicle Type**: A 25% surcharge for 'trucks' and a 10% discount for 'bikes'/'cycles'.
    -   **Special Days**: A 20% "event surge" is applied on holidays or event days.
-   🖥️ **Live "Mission Control" Dashboard**: Built with Panel and Bokeh, the dashboard displays live-updating time-series plots for all 14 parking lots simultaneously, allowing for at-a-glance system monitoring.
-   🔄 **Continuous Data Simulation**: Leverages Pathway's `mode="streaming"` to treat a historical dataset as an unbounded, continuous source, perfectly simulating a 24/7 operational environment.

---

## 🛠️ Tech Stack

-   **Backend & Data Processing**: 🐍 Python, 🌊 **Pathway** (for real-time data streaming)
-   **Data Manipulation**: 🐼 Pandas, 🔢 NumPy
-   **Visualization & Dashboarding**: 📊 **Panel**, 📈 Bokeh

---

## 🧠 The Pricing Model: A Deeper Dive

The heart of this project is a multi-layered pricing algorithm that combines internal and external factors to produce a single, smart price.

#### Layer 1: Internal Demand Score
The foundation of the price is determined by the lot's current status.
-   **Formula**: `(0.7 * Occupancy Rate) + (0.3 * Queue Pressure)`
-   **Logic**: This score provides a baseline of how "in-demand" a lot is at any given moment. A nearly full lot with a long queue will have a much higher initial price than an empty one.

#### Layer 2: Contextual Modifiers
The price is then adjusted based on the current environment. These multipliers are applied sequentially:
-   **Traffic**: `high` traffic increases price by **15%**, and `average` traffic by **5%**.
-   **Vehicle Type**: `trucks` incur a **25%** surcharge, while `bikes` get a **10%** discount.
-   **Special Days**: A flat **20%** surge multiplier is applied on event days.

#### Layer 3: Competitive Analysis
Next, the model looks outward to the competition. For each lot, it calculates the `avg_competitor_price` based on other lots within a 2km radius.

#### Layer 4: Strategic Price Adjustment
This is the final intelligence layer, where the model compares its own context-adjusted price against the market:
-   **Scenario A: Over-Priced & Busy**: If a lot is >80% full AND more expensive than its competitors, the price is lowered to the average of its price and the competitor's price. This prevents losing customers when demand is high.
-   **Scenario B: Under-Priced**: If the lot is cheaper than its competitors, its price is nudged up by 5%, capitalizing on the opportunity to increase revenue without losing its competitive edge.

#### Final Step: Business Rule Clipping
The final calculated price is clipped to stay within a sane business range of **$5.00 to $25.00**. The upper bound is raised to $25 to account for potential surge scenarios from the combined multipliers.

---

## 🚀 How to Run

This project is designed for Google Colab.

1.  **Upload `dataset.csv`** to your Colab session's file system.
2.  **Install Dependencies**: Run the first code cell to install the necessary packages.
    ```bash
    !pip install pathway bokeh panel --quiet
    ```
3.  **Execute All Cells**: Run the notebook cells in order from top to bottom. This will:
    -   Load and preprocess the data.
    -   Define the pricing functions and the Pathway streaming graph.
    -   Render the static layout of the 14-plot dashboard.
4.  **Start the Simulation**: Execute the final code cell.
    ```python
    # This command starts the Pathway streaming engine.
    pw.run()
    ```
    The dashboard in the cell above will activate, and all 14 plots will come to life with live data. To stop the simulation, you must **interrupt the kernel** (click the stop button in Colab). A `KeyboardInterrupt` error upon stopping is normal.