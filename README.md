Insights from Failed Orders — Gett Taxi
Overview
This project is a take-home assignment based on real data from Gett (previously GetTaxi), an Israeli-developed corporate Ground Transportation Management platform. The goal is to investigate matching metrics for orders that did not complete successfully — meaning the customer did not end up getting a car.
The analysis covers cancellation patterns, time-based trends, ETA distribution, and geospatial clustering of failed orders.

Dataset
Two CSV files are used:

data_orders.csv — contains order-level data including timestamp, coordinates, order status, driver assignment, and cancellation time
data_offers.csv — contains offer-level data linking orders to driver offers

Key columns in data_orders.csv:

order_datetime — time the order was placed
origin_latitude / origin_longitude — pickup coordinates
order_status_key — 4 = cancelled by client, 9 = rejected by system
is_driver_assigned_key — 0 = no driver assigned, 1 = driver assigned
m_order_eta — estimated time of arrival in seconds
cancellations_time_in_seconds — time elapsed before cancellation


Tools & Libraries

Python
Pandas
NumPy
Matplotlib
Seaborn
H3
Folium


Analysis
1. Distribution of Failed Orders by Reason
Orders were categorized into three failure types by combining order_status_key and is_driver_assigned_key:

Cancelled — No Driver
Cancelled — With Driver
Rejected by System

Finding: Cancellations before driver assignment are the most common failure type (~4,500 orders), suggesting the matching system is too slow and customers give up while waiting.

2. Failure Distribution by Hour
The three failure categories were plotted across all 24 hours to identify trends.
Finding: Hour 8 (morning rush) and hours 21-23 (late evening) show the highest failure counts across all categories. The quietest period is early morning hours 4-6.

3. Average Time to Cancellation by Hour
Focused on client-initiated cancellations (status 4) only. Outliers were removed using the IQR method with a 3×IQR upper bound. Average cancellation time was plotted by hour, split by whether a driver was assigned or not.
Finding: Customers with an assigned driver consistently wait longer before cancelling across all hours of the day. This suggests that faster driver assignment would directly reduce cancellation rates.

4. Average ETA by Hour
Focused on orders where a driver was assigned (status 4, driver assigned = 1), as these are the only orders with meaningful ETA data.
Finding: ETA peaks sharply during morning rush hours (7-8) at nearly 10 minutes, and again during evening rush (hour 17). These same hours show the highest cancellation counts, confirming that high ETA is a strong predictor of cancellations.

5. Geospatial Hexagonal Analysis
Using H3 at resolution 8 and Folium, orders were mapped to hexagonal cells (~0.74 km² each) and visualized on an interactive map colored by failure count.
Finding: Just 23 hexagons contain 80% of all failed orders, all concentrated in and around Reading town center. This geographic concentration confirms that failures are systemic in high-demand urban areas rather than randomly distributed.

Key Takeaways

The majority of failures happen before a driver is even assigned — a supply and matching speed problem
Rush hours (7-8am and 9-11pm) are the most critical periods for failures
Driver assignment significantly increases customer patience
80% of failed orders are concentrated in just 23 city-center hexagons
Targeted interventions in these specific areas and time windows could dramatically reduce failure rates


Source
Project original assignment from StrataScratch
