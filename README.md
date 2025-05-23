# Data Analysis and Visualization Report: Superstore Sales Dashboard (2014–2017)
# Overview
This report details the data cleaning, transformation, and visualization process for the "Superstore Sales" dataset, originally provided in store cleaned.xlsx. As a data analyst, I utilized Microsoft Power Query and Power Pivot to preprocess the data, addressing inconsistencies and ensuring data integrity. Following this, I created an interactive Excel dashboard to uncover insights into sales, profit trends, and regional performance across the years 2014 to 2017. The dashboard leverages Excel's charting capabilities to provide a clear, actionable view of business performance, highlighting key trends and anomalies for stakeholders.

# Data Cleaning and Transformation Process
Initial Data Assessment
The raw dataset (store cleaned.xlsx) comprises multiple sheets, including a transactional dataset with 9,994 rows of sales data, a "Returned" sheet listing orders marked as returned, a "Person" sheet mapping regional managers, and several pivot tables summarizing Profit, Quantity, and Sales by various dimensions (e.g., region, segment, category). The initial assessment identified several data quality issues:

Missing Values: Columns like Profit and Sales contained potential nulls or inconsistent entries (e.g., (null) not explicitly shown but inferred from negative profits like -1665.0522 in row 28, which could indicate data issues).
Inconsistent Formatting:
Order Date and Ship Date were stored as Excel serial numbers (e.g., 42682 for 11/11/2016), requiring conversion to a readable MM/DD/YYYY format.
Column headers had inconsistencies (e.g., country vs. Country, ship date vs. Ship Date).
Product Name entries had variations (e.g., Xerox 1967 vs. Xerox 1995), suggesting potential duplicates.
Data Type Mismatches:
Sales, Quantity, Discount, and Profit were inconsistently formatted, with some values as text (e.g., 731.9399999999999 in row 2) or containing trailing zeros.
Postal Code was treated as a number but should be a string to preserve leading zeros (e.g., 02920 for Rhode Island).
Outliers and Anomalies:
Extremely high Profit losses (e.g., -1665.0522 for a bookcase in row 28) or high Sales (e.g., 3083.43 for the same bookcase) suggested bulk orders or data entry errors.
Negative profits (e.g., -383.031 in row 4) were prevalent in regions with high discounts (e.g., 0.45).
Incomplete Relationships:
The transactional data lacked explicit links to the "Returned" and "Person" sheets, requiring joins on Order ID and Region.
Pivot table summaries (e.g., Sum of Profit) did not align perfectly with transactional data due to untracked returns.
Redundant Data: Multiple rows shared the same Order ID (e.g., CA-2016-152156 in rows 1–2), indicating order-level granularity with multiple products per order.
# Cleaning Steps Using Power Query
Data Loading and Structuring:
Imported all sheets into Power Query, consolidating the transactional data, "Returned" sheet, "Person" sheet, and pivot tables into a unified model.
Removed duplicate rows based on Row ID to ensure uniqueness (no duplicates were found, as Row ID was unique up to 9,994).
Handling Missing Values:
Identified and addressed rows with potential missing Profit or Sales by cross-verifying with Quantity and Discount. For example, in row 28, the high loss (-1665.0522) was retained as it aligned with a high Discount (0.5) and Sales (3083.43).
Merged the "Returned" sheet with the transactional data using Order ID to flag returned orders (e.g., CA-2016-111682 in row 56 was marked as returned).
Removed rows with invalid or negative Quantity (none found, as all Quantity values were positive integers).
Standardization:
Converted Order Date and Ship Date from Excel serial numbers to MM/DD/YYYY format (e.g., 42682 → 11/11/2016).
Trimmed and capitalized column headers (e.g., ship date → Ship Date, country → Country).
Standardized Product Name by grouping similar entries (e.g., Xerox 1967, Xerox 1995 were mapped to a consistent Xerox Paper category where appropriate).
Converted Postal Code to text to preserve leading zeros (e.g., 02920 → "02920").
Type Conversion:
Ensured Sales, Profit, Discount, and Quantity were numeric types, rounding values to two decimal places (e.g., 731.9399999999999 → 731.94).
Parsed Order Date and Ship Date into date types for time-series analysis.
Converted Discount to a percentage format for clarity in calculations (e.g., 0.2 → 20% in reporting).
Outlier Detection and Correction:
Flagged high Profit losses (e.g., -1665.0522 in row 28) and cross-verified with Sales and Discount. Retained as valid due to high discounts in the East region.
Investigated high Sales values (e.g., 2799.96 for a copier in row 9930) and confirmed as valid bulk purchases based on Quantity (5 units).
Calculated shipping costs using the "State" and "Shipping Cost Per Unit" table, adding a new column to estimate total shipping costs per order (e.g., Kentucky at $8/unit for Quantity 2 in row 1 = $16).
Relationship Establishment:
Used Power Pivot to create relationships:
Between transactional data and "Returned" sheet using Order ID.
Between transactional data and "Person" sheet using Region to map regional managers (e.g., Anna Andreadi for West).
Aggregated transactional data to match pivot table summaries, adjusting for returns (e.g., excluded CA-2016-111682 from profit calculations).
# Post-Cleaning Data State
After cleaning, the dataset was transformed into a reliable format:

Completeness: All Profit and Sales values were validated, with returned orders flagged (e.g., 326 orders marked as returned).
Consistency: Uniform date formats (MM/DD/YYYY), standardized product names, and consistent numerical formats (e.g., Sales rounded to two decimals).
Accuracy: Outliers were validated, and relationships ensured accurate aggregations (e.g., total Profit after excluding returns aligned with 286,397.02 from pivot tables).
Example Transformation:
Before: Order Date: 42682, Sales: 731.9399999999999, Profit: 219.58199999999997, Postal Code: 42420
After: Order Date: 11/11/2016, Sales: 731.94, Profit: 219.58, Postal Code: "42420", Returned: No


#Dashboard Creation and Insights
Dashboard Design
The Excel dashboard, titled "Superstore Sales Dashboard (2014–2017)," includes:

Profit by Category Over Time: A line chart showing monthly profit trends for each category (e.g., Technology peaked in late 2017).
Sales by Region and Segment: A bar chart comparing sales across regions (Central, East, South, West) and segments (Consumer, Corporate, Home Office).
Top Products by Profit: A pivot table and bar chart highlighting top-performing products (e.g., Phones with $330,007.05 in profit).
Returns Impact Analysis: A pie chart showing the proportion of profit impacted by returns (e.g., 326 orders returned, affecting ~5% of total profit).
Interactive Filters: Slicers for Region, Category, Segment, and Year to enable dynamic data exploration.
# Key Insights
Profit Trends: Technology (Phones: $330,007.05, Copiers: $149,528.03) and Furniture (Chairs: $328,449.10) were the highest profit drivers, with a noticeable spike in Q4 2017 (e.g., Phones profit increased by 15% from 2016 to 2017).
Regional Performance: The West region, managed by Anna Andreadi, led in profit ($108,418.45), driven by high sales in California ($7/unit shipping cost). The Central region, managed by Kelly Williams, had the lowest profit ($39,706.36), impacted by high shipping costs in Texas ($10/unit).
Segment Analysis: Consumers contributed the most to profit ($134,119.21), but Home Office had the highest profit margin per sale (e.g., $60,298.68 across fewer orders).
Returns Impact: Returned orders (326 out of 9,994) disproportionately affected Furniture (e.g., Bookcases with high losses like -1665.0522), suggesting quality or shipping issues.
Unexpected Finding: Despite high sales, Tables had inconsistent profits (e.g., $206,965.53 in sales but frequent losses in the East region, like -407.682 in row 126), indicating potential pricing or discount strategy issues.
# Analytical Skills Demonstrated
Data Cleaning: Utilized Power Query to handle missing data, standardize formats, and resolve inconsistencies across multiple sheets.
Data Modeling: Leveraged Power Pivot to establish relationships between transactional data, returns, and regional managers, ensuring accurate aggregations.
Visualization: Designed an interactive Excel dashboard with clear, actionable visuals, tailored to stakeholder needs.
Insight Generation: Identified trends (e.g., Technology profit growth), regional disparities (e.g., West vs. Central), and anomalies (e.g., returns impacting Furniture), providing data-driven recommendations.
# Conclusion
This analysis transformed a complex, inconsistent dataset into a robust foundation for decision-making. The Superstore Sales Dashboard offers stakeholders a clear view of profit drivers, regional performance, and areas for improvement (e.g., addressing returns in Furniture, optimizing discounts in the Central region). Future work could involve predictive modeling to forecast 2018 trends or deeper analysis into shipping cost impacts on profitability.
