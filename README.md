### Project Overview: Hotel Booking Demand, Customer Behaviour, and Cancellation Analysis

**Project Title:** Hotel Booking Demand, Customer Behaviour, and Cancellation Analysis

**Dataset:** `hotel_bookings.csv` (119,390 historical reservation records across 32 features, combining City Hotel and Resort Hotel data; 119,208 valid records after cleaning)

**Core Technologies:** Python, Pandas, NumPy, Matplotlib, Seaborn, Scikit-learn, and Streamlit

---

### 1. Problem Statement & Business Objective

Hotels experience substantial revenue loss and operational uncertainty due to high booking cancellation rates (~37% overall). Acting from the perspective of a hospitality data analyst, this project investigates historical reservation records across two distinct property types—**City Hotel** (66.45% of bookings) and **Resort Hotel** (33.55% of bookings)—to answer 10 core business questions:

* Why reservations get canceled and which booking variables drive churn.
* How booking patterns, pricing, and cancellation rates differ between City and Resort hotels.
* How seasonality, arrival months, lead times, customer segments, deposit policies, and special requests affect booking outcomes and Average Daily Rates (ADR).
* How hotel management can optimize booking policies, revenue management, and operational planning.

---

### 2. System Architecture (Object-Oriented Design)

The entire analytical and predictive workflow is implemented inside a single PEP 8-compliant Jupyter Notebook (`Hotel_Booking_Project_v3.ipynb`) structured with modular Python classes and PEP 257 docstrings, paired with an interactive Streamlit dashboard (`app.py`):

* **`HotelDataProcessor` (Data Ingestion, Auditing & Preprocessing):**
* Loads `hotel_bookings.csv`, standardizes column headers, and builds a structured Data Dictionary categorizing features into numerical, categorical, binary, and date/temporal types.
* Audits missing values (`company`, `agent`, `country`, `children`), exact duplicates, and logical anomalies, removing 182 invalid records (180 zero-guest bookings and 1 negative ADR record).
* Parses `arrival_date` and `reservation_status_date` into datetime objects and engineers domain attributes (`total_nights`, `total_guests`, `room_type_changed`, `stay_type`, and `estimated_value`).


* **`HotelEDAAnalyzer` (78-Task Exploratory Data Analysis Engine):**
* **Section B (Univariate Analysis):** Statistical profiling (mean, median, mode, standard deviation, variance, and 5th–99th percentiles) and distribution plots for `lead_time`, `adr`, stay duration, guest composition, meal plans, market segments, and special requests.
* **Section C (Hotel-Wise Comparative Analysis):** Benchmarks City Hotel vs. Resort Hotel across 13 operational metrics using both absolute and relative percentage differences.
* **Section D (Cancellation Factor Analysis):** Evaluates cancellation rates across 14 dimensions, including lead-time buckets (`0-7d` to `>365d`), deposit types, distribution channels, special requests, repeat guest status, prior cancellations, room reassignments, and top 15 countries.
* **Section E (Time-Series & Seasonality):** Analyzes monthly booking volumes, cancellation trends, seasonal ADR fluctuations, and Year-over-Year (2015–2017) performance.
* **Section F (Revenue Management & ADR Analysis):** Evaluates ADR distributions across hotels, market segments, and room types, computes booking-level `Estimated Value` (`ADR × total_nights`), and audits zero/extreme ADR records.


* **`CancellationPredictor` (Machine Learning Pipeline):**
* Prevents target leakage by removing post-booking columns (`reservation_status`, `reservation_status_date`) and engineers predictive features (`is_family`, `prior_cancellation_ratio`, top-15 country grouping).
* Uses a `ColumnTransformer` preprocessing pipeline (`SimpleImputer` + `StandardScaler` for numerical features; `SimpleImputer` + `OneHotEncoder` for categorical features) with a stratified 75/25 train-test split.
* Trains and benchmarks a balanced **Logistic Regression** baseline against a **Random Forest Classifier** (`n_estimators=100`, `max_depth=15`), evaluating models via Accuracy, F1-Score, ROC-AUC, and Gini Feature Importance.


* **Interactive Streamlit Web Application (`app.py`):**
* Provides an interactive front-end dashboard with hotel-type KPI filters, visual EDA charts, and a real-time cancellation risk simulator that outputs risk tiers and front-desk retention recommendations.



---

### 3. Key Analytical Findings & Strategic Recommendations

* **Hotel Divergence:** City Hotel experiences a **41.79%** cancellation rate compared to **27.77%** at Resort Hotel (+14.02 percentage points / +50.48% relative difference) and commands a higher median ADR ($99.96 vs. $75.00), while Resort Hotel attracts longer stays and more repeat guests (4.44% vs. 2.50%).
* **Lead Time & Guest Commitment:** Cancellation rates escalate from **9.60%** for bookings made within 0–7 days of arrival to **67.65%** for bookings made over 365 days in advance. Conversely, making even one special request drops cancellation risk from **47.77%** to **22.05%**.
* **Channel & Deposit Dynamics:** `Online TA` accounts for 47.32% of all bookings, while `Groups` (61.11% cancellation) and `Non Refund` deposits (99.36% cancellation, concentrated in speculative bulk blocks) represent the highest churn segments.
* **Actionable Business Policies:** Implement automated milestone re-confirmations for bookings exceeding 90 days lead time, tighten deposit verification on bulk OTA/group blocks, encourage pre-arrival special requests to increase guest commitment, and apply minimum-stay yield rules during peak July–August demand.
