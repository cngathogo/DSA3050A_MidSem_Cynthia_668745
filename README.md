# DSA3050A_MidSem_Cynthia_668745
# README: Business Intelligence Project
## Course: DSA3050UA 

**Name:** Cynthia Gathogo  
**Student ID:** 668745  

---

## 📊 About the Dataset
* **Source:** UCI Machine Learning Repository (Online Retail II dataset). It contains real e-commerce sales transactions.
* **Size:** Over 10,000 rows and 8 original columns (`Invoice`, `StockCode`, `Description`, `Quantity`, `InvoiceDate`, `Price`, `Customer ID`, and `Country`).
### 💾 Raw Dataset Access
* 📄 **[Download Raw Online Retail II Dataset (CSV)]https://www.kaggle.com/datasets/mashlyn/online-retail-ii-uci** *Link provided due to GitHub browser file size limits)*
---

## 🎯 The Business Problem
The main goal of this assignment is to take a really messy retail dataset and clean it up using **Power Query** so we can build a proper dashboard. 

The raw data has a lot of issues: missing customer IDs, duplicate entries, columns with the wrong data types, and negative numbers from order cancellations. If we don't fix these things first, our final charts and sales totals will be completely wrong. Cleaning this data ensures the numbers we show to management are accurate.

---

## 🧹 Power Query Steps Performed

### 1. Basic Cleaning (Question 1A)
* **Renamed Columns:** Changed vague headers to clearer ones (`Invoice` -> `Invoice_No`, `Customer ID` -> `Customer_ID`, and `Price` -> `Unit_Price`).
* **Fixed Data Types:** Set ID numbers (`Invoice_No` and `StockCode`) to Text because some contain letters. Set `Quantity` to Whole Number and `Unit_Price` to Fixed Decimal Number (Currency).
* **Removed Duplicates & Blanks:** Deleted over 5,200 identical rows and cleared out empty lines using the **Remove Duplicates** and **Remove Blank Rows** buttons.
* **Cleaned Up Text:** Used the **Trim** and **Clean** tools on the `Description` and `Country` columns to get rid of accidental spaces and weird system characters.
* **Standardized Values:** Used **Replace Values** in the `Country` column to make sure regional names were spelled consistently (e.g., changing "United Kingdom" to "UK").

### 2. Intermediate Steps (Question 1B)
* **Split Date and Time:** Split the `InvoiceDate` column using a space delimiter to make two separate columns: `Order_Date` and `Order_Time`.
* **Merged Columns:** Combined `StockCode` and `Description` with a hyphen to create a new column called `Product_Details`.
* **Created Total Sales Column:** Added a **Custom Column** named `Total_Sales` using the formula: `[Quantity] * [Unit_Price]`
* **Created Market Segment Column:** Added a **Conditional Column** named `Market_Segment`. The rule was: *If the country is United Kingdom, label it "Domestic". Otherwise, label it "International".*
* **Extracted Date Parts:** Used the built-in Date tool to pull out the `Year`, `Month`, `Quarter`, and `Day` from our date column.
* **Filtered Rows (2 Conditions):** Filtered the `Quantity` column to keep rows where the quantity is greater than 0 **AND** less than 50,000. This removed negative returns and unrealistic data errors.
* **Sorted Data:** Sorted the `Order_Date` column in **Descending** order so the newest transactions appear at the top.
* **Added an Index:** Added a standard index column starting from 1 to give every row a unique number.

### 3. Advanced Steps (Question 1C)
* **Used Column Profiling:** Turned on **Column Quality**, **Column Distribution**, and **Column Profile** from the View tab, and set it to base statistics on the *entire dataset*. This visually exposed all the missing values, errors, and outliers before cleaning.
* **Extracted Prefix:** Used **Extract First Characters** on the `Invoice_No` column to pull out the letter "C" from cancellation numbers.
* **Handled Errors:** Used **Remove Errors** on the `Total_Sales` column so any broken math calculations wouldn't crash our data model.
* **Created a Summary Table (Reference):** Right-clicked our main query and chose **Reference** to create a separate table called `Country_Summary` without duplicating the data.
* **Grouped Data:** Used the **Group By** tool on our summary table to calculate the total sales and average quantity for each country at the same time.

---

## 🖼️ Visuals Created on the Dashboard

* **Total Revenue Card:** A KPI card showing the overall sum of sales.
* **Total Quantity Card:** A KPI card tracking the total number of items sold.
* **Total Orders Card:** A KPI card tracking unique orders using a **Distinct Count** of invoice numbers.
* **Horizontal Bar Chart ("Revenue Contribution by Country"):** Shows which countries brought in the most money.
* **Vertical Column Chart ("Order Distribution by Market Segment"):** Compares the number of orders from the UK versus international buyers.
* **Line Chart ("Monthly Sales Trend"):** Shows how sales go up and down month by month over time.
* **Donut Chart ("Order Processing Distribution"):** Breaks down standard sales versus canceled orders.
* **Map Visual:** Plots bubbles over countries, where bigger bubbles mean more sales.
* **Treemap ("Top Performing Products"):** Shows our best-selling items based on quantity.
* **Gauge Visual:** Compares our current total sales against a fixed target goal.
* **Matrix Grid:** A pivot table crossing years and quarters to show sales trends.
* **Detailed Table:** A row-by-row lookup table at the bottom for auditing specific transaction details.

---

## 🧠 3 Real Business Insights Discovered

### 1. The "111111" Impact (Found via Column Profiling)
When turning on **Column Profiling**, the *Column Quality* indicators showed that over 1,000 rows in the `Customer_ID` column were completely blank (`null`). Instead of deleting them, I realized these represent customers checking out as guests without making an account. By keeping these rows, the dashboard shows that guest checkouts actually bring in a huge chunk of money. Deleting them would have made our total revenue look much lower than it actually is.

### 2. Negative Outliers Mess Up the Data
During the data profile check, the minimum values for quantity and price showed negative numbers. This happens when the system logs returns or order cancellations. If we don't filter these out of our main sales charts, they cancel out regular sales and mess up our revenue tracking. Filtering them into their own category keeps our main dashboard numbers accurate.

### 3. Relying Too Heavily on the UK Market
By looking at the map and cross-filtering the charts, it is immediately obvious that **over 80% of all sales** come from the UK (Domestic) market. Even though the business ships internationally, it relies almost entirely on local UK buyers. This is a big risk for the business because if the UK economy slows down, sales will drop drastically. The business needs to start marketing more heavily to international segments to split the risk.
