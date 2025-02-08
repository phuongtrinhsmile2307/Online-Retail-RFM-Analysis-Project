# Customer Segmentation With RFM Analysis Using Power BI | Tata Group Virtual Case Experience 
## Project Overview 
**Case:** The CEO and CMO have recently met to finalise the requirements and I would like you to provide them with some analysis and visuals that would help answer their questions. Both, the executives are interested in viewing and understanding how they can use the data to make more meaningful decisions. I would need to provide insights which they can use to create the expansion strategy. The executives want to analyse the trends and the breakdown by different categories so that they have clarity on how the revenue is being generated and what are the main factors affecting the online store. 

>I have created some visuals in terms of Country, Products, and Customer Performance. Specifically, I have applied the RFM model for customer segmentation. In this description, I will focus on the process of conducting RFM analysis and its results.
## About RFM Analysis
![image](https://github.com/user-attachments/assets/3b198433-ee66-4ecf-8f20-e518f09a5791)

[RFM Analysis](https://www.putler.com/rfm-analysis/) is a model used in marketing analysis that segments a company’s consumer base by their purchasing patterns or habits.

RFM = Recency + Frequency + Monetary
- Recency (R) tells us when is the latest purchase date.
- Frequency (F) tells us how frequently do they make purchases.
- Monetary (M) tells us how large their average ticket size is made. Actually, it is customer lifetime value.

### How to generate RFM scores

Basically, to create RFM scores, I define 4 parameters:
- Identify the customer: CustomerID
- **R-value:**  The amount of elapsed time since the last purchase.
- **F-value:**  Count of purchases made during period.
- **M-value:** Average amount of purchases made during period.

To define RFM score on a scale of 1–5 for each customer in terms of recency, frequency, and monetary value. 1 is the lowest and 5 is the highest.

### **RFM Segments and Scores**
| **Customer Segment** | **Activity** | **Actionable Tip** | **RFM Score** |
| --- | --- | --- | --- |
| **Champions** | Bought recently, buy often, and spend the most! | Reward them. Can be early adopters for new products. Will promote your brand. | 555, 554, 544, 545, 454, 445 |
| **Loyal Customers** | Spend good money with us often. Responsive to promotions. | Upsell higher-value products. Ask for reviews. Engage them. | 543, 444, 435, 355, 354, 345, 344, 335 |
| **Potential Loyalist** | Recent customers, but spent a good amount and bought more than once. | Offer membership/loyalty program, and recommend other products. | 553, 551, 552, 541, 542, 532, 531, 452, 451, 442, 441, 431, 453, 433, 432, 423, 353, 352, 351, 342, 341, 333, 323 |
| **Recent Customers** | Bought most recently, but not often. | Provide onboarding support, give them early success, and start building relationships. | 512, 511, 422, 421, 412, 411, 311 |
| **Promising** | Recent shoppers, but haven’t spent much. | Create brand awareness, and offer free trials | 525, 524, 523, 522, 521, 515, 514, 513, 425, 424, 413, 414, 415, 315, 314, 313 |
| **Customers Needing Attention** | Above average recency, frequency, and monetary values. May not have bought it very recently though. | Make limitedtime offers, and recommend based on past purchases. Reactivate them. | 535, 534, 443, 434, 343, 334, 325, 324 |
| **About To Sleep** | Below average recency, frequency, and monetary values. Will lose them if not reactivated. | Share valuable resources, recommend popular products/renewals at discounts, and reconnect with them. | 331, 321, 312, 221, 213 |
| **At Risk** | Spent big money and purchased often. But a long time ago. Need to bring them back! | Send personalized emails to reconnect, offer renewals, and provide helpful resources. | 255, 254, 245, 244, 253, 252, 243, 242, 235, 234, 225, 224, 153, 152, 145, 143, 142, 135, 134, 133, 125, 124 |
| **Can’t Lose Them** | Made biggest purchases, and often. But haven’t returned for a long time. | Win them back via renewals or newer products, don’t lose them to competition, talk to them. | 155, 154, 144, 214, 215, 115, 114, 113 |
| **Hibernating** | Last purchase was long back, with low spenders and a low number of orders. | Offer other relevant products and special discounts. Recreate brand value. | 332, 322, 231, 241, 251, 233, 232, 223, 222, 132, 122, 212, 211 |
| **Lost** | Lowest recency, frequency, and monetary scores. | Revive interest with reach-out campaign, ignore otherwise. | 111, 112, 121, 131, 141, 151 |


>Here is my process for analysis
## Datasources
Online Retail Data: The primary dataset used for this analysis is the [Online Retail Data Set.xlsx](https://github.com/phuongtrinhsmile2307/Projects/blob/main/Online%20Retail%20Data%20Set.xlsx)  file , containing detailed information about online retail sales made by the company.
## Tools:
- Power Query - Data Cleaning
- Power BI - Data Modeling - Analysis - Visualization
## Data Cleaning/Preparation: 
The two datasets provided were Excel files. They were assessed for data quality and prepared for analysis to answer the business question.

**Key issues identified:**
- Incorrect values in **UnitPrice** and **Quantity**
- Some rows in the dataset had empty values in the **CustomerID** column

**Data transformation and cleaning steps in Power Query Editor:**
- Replaced null values in the **CustomerID** column with “unknown”.
- Formatting columns to the appropriate data types.
- Filtered **UnitPrice** to include only values greater than 0.
- Filtered **Quantity** to include only values greater than or equal to 1.

## DAX For RFM Analysis

### **Calculating the R, F & M values:**

Since R measures the most recent transaction, we need to get the last transaction date of each customer to determine their R-value.

```
last transaction date = 
MAXX(FILTER('Online Retail','Online Retail'[CustomerID]='Online Retail'[CustomerID]),'Online Retail'[InvoiceDateTime])
```

**R-value:**  The amount of elapsed time since the last purchase.

```
Rvalue = DATEDIFF([last transaction date],"31/12/2011",DAY)
```

As the dataset was record transaction in 2011 so I used the last transaction date on the dataset, “31/12/2011”. 

**F-value:**  Count of purchases made during period.

```
Fvalue = DISTINCTCOUNT('Online Retail'[InvoiceNo])
```

**M-value:** Average spending on a purchase made during period.

```
Mvalue = 
var TotalSales = SUM('Online Retail'[Revenue])
var TotalPurchases = DISTINCTCOUNT('Online Retail'[InvoiceNo])
Return 
DIVIDE (TotalSales,TotalPurchases,0)
```

### **Generate the new table called ‘RFM table’**

```
RFM table = SUMMARIZE(
   'Online Retail','Online Retail'[CustomerID],
   "R Value",[RValue],
   "F Value",[FValue],
   "M Value",[MValue])
```

### **Calculating the RFM scores**

Create three new columns ‘R Score’, ‘F Score, and ‘M Score’ by separating group each data by percentile.

```
R Score = 

    SWITCH (
        TRUE (),
        [Rvalue] <= PERCENTILE.INC ( 'RFM table'[R Value], 0.20 ), "5",
        [Rvalue] <= PERCENTILE.INC ( 'RFM table'[R Value], 0.40 ), "4",
        [RValue] <= PERCENTILE.INC ( 'RFM table'[R Value], 0.60 ), "3",
        [Rvalue] <= PERCENTILE.INC ( 'RFM table'[R Value], 0.80 ), "2",
        "1"
    ) 
```
```    
F Score = 
SWITCH (
  TRUE (),
   [Fvalue] <= PERCENTILE.INC ( 'RFM table'[F Value], 0.20 ), "1",    
   [Fvalue] <= PERCENTILE.INC ( 'RFM table'[F Value], 0.40 ), "2", 
   [FValue] <= PERCENTILE.INC ( 'RFM table'[F Value], 0.60 ), "3", 
   [Fvalue] <= PERCENTILE.INC ( 'RFM table'[F Value], 0.80 ), "4",
   "5"
       )
```
```       
M Score = 
SWITCH (
  TRUE (),
   [Mvalue] <= PERCENTILE.INC ( 'RFM table'[M Value], 0.20 ), "1",    
   [Mvalue] <= PERCENTILE.INC ( 'RFM table'[M Value], 0.40 ), "2", 
   [MValue] <= PERCENTILE.INC ( 'RFM table'[M Value], 0.60 ), "3", 
   [Mvalue] <= PERCENTILE.INC ( 'RFM table'[M Value], 0.80 ), "4",
   "5"
       )
```

Create the new column called ‘RFM’ by concatenating column ‘R Score’, ‘F Score’, and ‘M Score’.

```
RFM = 'RFM table'[R Score]& 'RFM table'[F Score]&'RFM table'[M Score]
```

### **Define criteria for each segment**

Import the table ‘Segment Scores Table’ into Power BI.

**Model the tables:** Create a relationship between the RFM table — RFM score column and the Segment scores table — scores column.

## **Analysis & Visualization**
Here’s a the report created for this task.
