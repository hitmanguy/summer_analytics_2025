# Real-Time Dynamic Pricing for Parking Lots

This project simulates a dynamic pricing engine for a system of 14 urban parking lots. It uses a real-time data streaming pipeline to adjust parking fees based on live demand and competitor pricing, and visualizes the results on a live dashboard.

## Project Highlights

-   **Dynamic Pricing Model**: Prices are not static. They are calculated in real-time based on:
    -   **Occupancy Rate**: How full the parking lot is.
    -   **Queue Length**: How many cars are waiting to enter.
    -   **Competitor Pricing**: The model identifies nearby competitors (within a 2km radius) and adjusts prices to stay competitive.

-   **Real-Time Simulation**: A historical dataset of parking information is replayed as a continuous live stream, mimicking a 24/7 operational environment.

-   **Live Dashboard**: The final output is a "mission control" dashboard displaying **14 live-updating plots**—one for each parking lot. This allows for an immediate, system-wide view of how prices are changing across all locations.

## Technologies Used

-   **Python**
-   **Pathway**: For building the real-time data streaming pipeline.
-   **Pandas & NumPy**: For data handling and calculations.
-   **Panel & Bokeh**: For creating the live dashboard.

## How to Run

This notebook is designed for Google Colab.

1.  **Upload `dataset.csv`** to your Colab environment.
2.  **Run all cells** from top to bottom.
    -   The first cell installs necessary packages.
    -   The following cells preprocess the data, define the pricing logic, and set up the dashboard layout.
3.  **Execute the final cell** containing `pw.run()` to start the live simulation.
    -   The dashboard in the cell above it will activate, and all 14 plots will begin updating.
    -   To stop the simulation, interrupt the kernel (click the stop button).