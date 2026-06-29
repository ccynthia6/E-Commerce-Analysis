E-Commerce Clickstream Analysis: Why Are Customers Not Buying?
SQL · SQLite · DBeaver · Python · Tableau

Business Problem
An e-commerce retailer has a fundamental question it cannot answer from its sales reports alone: why are so many customers browsing but not buying?
Revenue numbers tell you how much was sold. Clickstream data tells you why customers didn't buy — where they dropped off, what they viewed, what they added to cart and then abandoned, and what the highest-value lost opportunities look like.
This analysis uses October 2019 clickstream data to diagnose the conversion problem, identify which categories, brands, and products are underperforming relative to their traffic, and surface actionable recommendations for the business.

Key questions this project answers:
Where exactly in the funnel are customers dropping off?
Which product categories drive traffic but fail to convert?
Which brands are losing the most revenue to cart abandonment?
Is price a barrier to conversion, or does something else explain the drop-off?
Who are the customers — are they returning, or mostly one-time visitors?

Dataset & Tools

DatasetUCI E-Commerce Clickstream — October 2019
Sample Size20,000 rows
Event TypesView, Cart, Purchase
Fields event_time, event_type, product_id, category_code, brand, price, user_id, user_session
DatabaseSQLite via DBeaver
AutomationPython (CSV exports for Tableau)
VisualisationTableau Public

Table of Contents
Data Cleaning
Funnel Analysis
Trend Analysis
Category & Sub-Category Performance
Revenue Analysis
Brand Analysis
User Behaviour Analysis
Cart Abandonment Analysis
Product-Level Analysis
Key Findings & Recommendations

1. Data Cleaning
Before any analysis could begin, the raw data needed significant work. The dataset contained inconsistent casing, missing values, and no derived fields to support time-based or hierarchical analysis.

Sampling & Deduplication
The full dataset was large, so I first checked and removed duplicate records — rows where every field was identical, indicating double-logged events.
ss
Standardising Text Fields
Event types and brand names had inconsistent capitalisation ('purchase', 'Purchase'). I standardised all text fields to title case.
ss
Parsing Hierarchical Category Codes.
The category_code field contained dot-separated hierarchies like electronics.smartphone or appliances.kitchen.coffee_machine. I split this into three separate columns(Category,Sub-Category and Product) for cleaner analysis.
ss
Adding Time Columns & Handling Nulls
ss
Result: Clean, structured table with consistent casing, parsed hierarchies, time dimensions, and no silent nulls — ready for analysis.

Funnel Analysis
ss
The funnel is the most important view in this analysis. It answers the most basic question: of all the people who visited, how many actually bought something?
ss
Results
Stage    Sessions   Users
View     19,17118   859
Cart      438       438
Purchase  346       346

Conversion Rate              %
View → Cart                2.32%
Cart → Purchase            79.00%
Overall(View → Purchase)   1.83%

The critical finding: 97.7% of sessions that view a product never add anything to cart. But once a user does add to cart, 79% go on to purchase. The business is not losing customers at checkout — it is losing them during browsing. The entire conversion problem sits at the View → Cart stage.
The 312-session gap between View sessions (19,171) and View users (18,859) tells us that at least 312 users came back to browse in a new session, suggesting some level of repeat intent — but still not converting to cart.

Trend Analysis
ss
With the conversion problem identified, the next question is: does engagement vary over time, and are there patterns the business can act on?
ss
Results
Daily views ranged from ~486 to ~816 — relatively stable across the month with no dramatic spikes, indicating no major promotions or traffic campaigns ran during this period.

Weekly view volumes:
Week Beginning   Views
28 Sept          3,968
3Oct             4,565 
8Oct             4,649
13 Oct           4,356
23 Oct           1,678 (partial week)

Day of week traffic:
Day       Total Events
Tuesday   3,207 (highest)
Wednesday 3,040
Thursday  3,003
Sunday    2,851
Friday    2,687
Saturday  2,742
Monday    2,470 (lowest)

Traffic peaks mid-week (Tuesday–Thursday) and dips on Monday. If the business is running promotions or sending email campaigns, Tuesday is the highest-engagement day. Weekend traffic is moderate but consistent — suggesting a mix of impulse and deliberate shoppers.

Category & Sub-Category Performance
2-ss
ss-code
Results

Category performance:

CategoryViewsCartsPurchasesConversion RateElectronics7,1343261962.75%Appliances2,24837241.07%Computers1,07122121.12%Furniture588130.51%Apparel713030.42%Sport96033.13% (highest)Auto472451.06%
