# Fetch User Sales and Product Analysis

Note: Due to Github File size restrictions, The following files have been uploaded to a google drive folder and respective link has been provided :

products.csv - this is the dataset provided in the assessment size : 65.6 MB
products_new.csv - this is the file that contains all the processed data of respective file size : 81.6 MB
data_analysis_assignment.db - this is the db that is being used in the assignment. size : 187.7 MB

Google Drive Link: https://drive.google.com/drive/folders/1Am-8tRYmiNTwPxrkYJsZjLHw3ERegvrL?usp=sharing

**Overview:**
The dataset provided consists of three CSV files: products, transactions, and users, each detailing different aspects of a retail environment. 
The products file contains information about various product categories, manufacturers, brands, and barcodes, giving insights into the product taxonomy and branding. 
The transactions file records individual purchase events, including receipt IDs, purchase and scan dates, store names, user IDs, and details about products and quantities purchased. 
The users file provides demographic information about the customers, such as their account creation dates, birthdates, states of residence, preferred languages, and gender.

**Understanding Entity Relationship Model:**

<img width="482" alt="image" src="https://github.com/user-attachments/assets/4406dd47-146b-4ba6-bdba-3c78e3b4ccd9">

 The entity-relationship (ER) model depicts the structure and relationships among the datasets provided. 
 It comprises three main entities: Users, Transactions, and Products. 
 The Users entity includes demographic and account information such as user IDs, creation dates, birth dates, states, languages, and gender. 
 The Transactions entity captures details of each purchase, linking it to users and products through fields like receipt ID, purchase and scan dates, store names, and barcodes. 
 The Products entity contains product classifications, manufacturers, brands, and barcodes.

This ER model is essential for understanding how these entities interact, allowing us to join data tables based on shared keys (e.g., user IDs and barcodes). 
It guides the structuring of our code and ensures that analyses accurately reflect user behavior, product sales, and transaction details, thus helping to extract meaningful insights from the dataset.

**PART 1: DATA EXPLORATION**

**1)  Are there any data quality issues present?**

The analysis of the three datasets has revealed various data quality issues across all tables. 
Below is a summary of the identified problems for each dataset, accompanied by relevant codes and screenshots:

Data quality checks for all tables

 <img width="472" alt="image" src="https://github.com/user-attachments/assets/b765129c-ac67-4a70-9874-6ba1d383dfe9">

**TABLE 1: USER DATA**

**•	Missing Values:**
Out of 100,000 rows, there are significant missing values in critical fields:
o	BIRTH_DATE: 3,675 nulls 
o	STATE: 4,812 nulls 
o	LANGUAGE: 30,508 nulls 
o	GENDER: 5,892 nulls 

**•	Inconsistent Data Types:**
o	Key fields like CREATED_DATE and BIRTH_DATE are stored as text instead of the appropriate datetime format. This misalignment complicates date-related calculations and may lead to inaccurate analyses.

**•	Inconsistencies in Entries:**
The "Gender" column exhibits inconsistencies in how responses are recorded. Some entries use the phrase "Prefer not to say," while others are recorded as "prefer_not_to_say." 
These variations can lead to challenges in data analysis and aggregation, as they may be interpreted as distinct categories despite conveying the same meaning.
OUTPUT:

 <img width="454" alt="image" src="https://github.com/user-attachments/assets/a528afc6-5a7e-4834-a1a8-8a1ee6f748b3">

**INCONSISTENT VALUES USED IN THE GENDER COLUMN:**

 <img width="499" alt="image" src="https://github.com/user-attachments/assets/54b8941d-70e8-41c2-8f53-6feca964bac6">


**TABLE 2: PRODUCT DATA**
•	Missing Values: There are numerous null entries across key fields, including:
•	CATEGORY_1: 111 nulls
•	CATEGORY_2: 1,424 nulls
•	CATEGORY_3: 60,566 nulls
•	CATEGORY_4: 778,093 nulls
•	MANUFACTURER: 226,474 nulls
•	BRAND: 226,472 nulls
•	BARCODE: 4,025 nulls 

•	Data Type Issues: The BARCODE field is classified as "real," despite containing both floating-point and integer representations (e.g., 7.28048E+11 and 22600601012). 
It should be standardized as an integer to avoid processing complications. Additionally, the BARCODE field, which is intended to be a unique identifier (primary key), contains duplicate entries. 
This violation of uniqueness further compromises the integrity of the dataset and can lead to inaccuracies in data retrieval and analysis.

**OUTPUT:**

<img width="474" alt="image" src="https://github.com/user-attachments/assets/849b8949-34b1-43c8-988c-7904232ac7b6">


**TABLE 3: TRANSACTION DATA**

OUTPUT:

 <img width="434" alt="image" src="https://github.com/user-attachments/assets/b656a2e0-85d3-49c3-8773-515a0e2c5047">

•	Missing Values: Among the 50,000 rows, the dataset has one critical area of concern:
BARCODE: 5,762 null entries, which can affect product identification and transaction integrity. All other fields are complete.
•	Data Type Issues: PURCHASE_DATE and SCAN_DATE are stored as text but should be formatted as datetime for accurate date operations.
BARCODE is categorized as "real," but it should be stored as an integer to properly reflect its unique identifier role.
FINAL_QUANTITY and FINAL_SALE are recorded as text, whereas they should be numeric to facilitate calculations and analyses.
•	Inconsistencies in Entries:
There are inconsistencies in the entries for the STORE_NAME and FINAL_QUANTITY fields. Standardizing these entries is essential to ensure data accuracy and facilitate reliable analysis.
Output:

<img width="434" alt="image" src="https://github.com/user-attachments/assets/f70d7867-5520-4738-8540-706466d0d84f">

VISUALIZATIONS ( Distributions and Top Performers)

<img width="452" alt="image" src="https://github.com/user-attachments/assets/a05f3403-e4a2-4886-a44a-90f9e642ecd4">
 
1. Distribution of user's age
   
 <img width="405" alt="image" src="https://github.com/user-attachments/assets/ac95950c-a76b-4a41-a3f2-6a7e7228774d">

3. Distribution of products
   
<img width="516" alt="image" src="https://github.com/user-attachments/assets/cf683dc3-f2e3-44b7-a120-95d8f9303226">


**2)	Are there any fields that are challenging to understand?**

**Understanding Challenges in Dataset Fields**

**TABLE 1: USER DATA**

•	STATE and LANGUAGE: The STATE and LANGUAGE fields in the Users dataset use abbreviations and codes that are difficult to interpret without a reference key. To make effective use of these fields in analysis, additional context or decoding is needed.

**TABLE 2: PRODUCT DATA**

**•	CATEGORY:**

It is difficult to understand the role of categories in forming a hierarchical tree for each product record, as there are no clear guidelines or captions provided. The CATEGORY_1 to CATEGORY_4 columns appear to represent different classification levels, but without a clear hierarchy, it's difficult to see how these categories connect to individual products.

**•	BARCODE:**
The Bar Code field raises questions about uniqueness, as multiple products share the same barcode even though it is designated as the primary key in the dataset. This inconsistency poses issues regarding data integrity.

**TABLE 3: TRANSACTION DATA**

**•	FINAL_QUANTITY:**

There appears to be an inconsistency between the FINAL_QUANTITY and FINAL_SALE fields. Even when FINAL_QUANTITY is recorded as zero, there are still values present in FINAL_SALE. This discrepancy makes it challenging to interpret the relationship between these two fields, as it’s unclear if a quantity of zero should correspond to any sale amount. Additional clarification on how these fields interact would help improve data accuracy and understanding.
Also, the FINAL_QUANTITY field is difficult to interpret due to the presence of decimal values. This can be confusing and makes it harder to understand the quantity sold.

During my analysis of the Products and Transactions tables, I noticed missing values in the Barcode fields. Unlike other numeric fields, where it might make sense to fill in missing values with an average or mode, the Barcode field required a different approach for a couple of reasons:
	Uniqueness: Each barcode is a unique identifier tied to a specific product, so filling in missing values could create duplicates or incorrect associations.
	Non-Integer Format: The barcodes are stored in a non-integer format (like scientific notation or long numbers), making it impractical to replace missing values without impacting accuracy.

**Decision:** Given these factors, I chose not to fill missing values in the Barcode field. By leaving these as null, I maintained the uniqueness of each product identifier and avoided making unsupported assumptions about the data. 

In the final CSV files, you’ll see that missing barcodes remain unfilled, reflecting the original data structure while preserving data integrity.

**PART 2: SQL QUERIES**

**Closed-ended questions:**

**1. What are the top 5 brands by receipts scanned among users 21 and over?**

This query identifies the five brands with the highest number of receipts scanned by users aged 21 and older. The calculation of user age is based on the difference between the scan date and the birth date. According to the results, Equate leads significantly, followed by CVS, Pepsi, and Nature’s Bounty. This suggests that these brands have strong engagement or visibility among adult users on the platform, potentially indicating popular choices within this demographic.

 <img width="511" alt="image" src="https://github.com/user-attachments/assets/88c6a305-29ca-49de-8f0a-42fd344b0dba">


**2. What are the top 5 brands by sales among users that have had their account for at least six months?**
   
<img width="516" alt="image" src="https://github.com/user-attachments/assets/216fc242-9302-4182-a5a3-c08317d2c7c9">

 
This query ranks the top five brands based on total sales among users who have held an account for six months or longer. The results show Equate as the leading brand in terms of sales, followed by Pepsi, CVS, and Spring Valley Vitamins. These findings suggest that long-term users prefer these brands, indicating potentially strong brand loyalty or frequent purchasing habits for these products over time.

Open-ended questions: Which is the leading brand in the Dips & Salsa category?

 <img width="454" alt="image" src="https://github.com/user-attachments/assets/2472c754-9402-49cf-ae5b-d2df90e112e6">


This query identifies the top-selling brand in the Dips & Salsa category by calculating total sales. The results show Marketside as the leading brand, with a total sales value of 196,900.06. This indicates that Marketside products are highly popular among customers in this category, making it a strong performer in the Dips & Salsa market. This insight could guide marketing strategies or product placement to maximize visibility and sales in this category.

**Assumptions:**

In this analysis, we interpreted transactions.FINAL_SALE as a dollar amount reflecting a portion of the total transaction, where a float value (e.g., 3.5) represents a higher dollar figure, such as $3,500. For the open-ended question about identifying the leading brand in the Dips & Salsa category, we assumed that summing these final sale amounts would accurately indicate the brand’s popularity. We also assumed that the highest total sum of final sales would effectively identify the leading brand, reflecting consumer preference in this category.

**Part 3: Reporting to Stakeholders through email**

**Subject: Data Quality Concerns and Request for Clarification**

Dear Stakeholder,
Our recent data review highlighted several issues across the User, Product, and Transaction datasets, I’ll highlight the key data quality issues, interesting trends, and Requests for action in this email.

**Key data quality issues**

1. Missing Values: Many essential fields, such as dates, product categories, and barcodes, have missing data, affecting analysis completeness.
2. Inconsistent Data Types: Some fields are stored in unsuitable formats (e.g., dates stored as text), complicating calculations.
3. Inconsistent Entries: Similar entries, like gender options, appear in different formats, impacting data aggregation.
4. Clarification on certain fields and Database normalization: Additionally, we need clarification on certain fields, such as product category hierarchy, state/language codes, and the relationship between quantity and sales in transactions.
5. Database Normalization issue:During the normalization of the database tables, we identified several integrity constraint violations that prevented data from being loaded into the new tables following the defined foreign constraints. Specifically, certain products were missing barcodes, which led to transactions being recorded without associated barcodes. This resulted in null values that violate the foreign key constraints outlined in the ER diagram.
   
<img width="504" alt="image" src="https://github.com/user-attachments/assets/e37446c0-c176-4e98-86da-8f1397f60236">

 

**Interesting trends in the data**

This data analysis reveals valuable insights into our business performance. Understanding the dynamics of the top retail partners and user demographics is essential for informed decision-making and strategic growth
1. Top Stores by Sales: Walmart leads significantly, contributing over 50% of the total sales among the top 10 stores. This highlights Walmart as our most influential retail partner by a large margin.
Other stores like Costco, Sam’s Club, and CVS each hold smaller but notable shares, ranging from 5-9%. This distribution suggests that while Walmart is dominant, diversifying and strengthening partnerships with these other stores could provide additional growth.

**Top 10 stores by final sale**
 
 <img width="426" alt="image" src="https://github.com/user-attachments/assets/63bf9031-833b-4c66-afec-280d6ef3d33c">

**2. User Demographics by State:**

Texas, Florida, and California have the highest user counts, each with over 8,000 users. This concentration indicates that these states are key markets for us, where further marketing and customer engagement could yield significant returns.
States like New York and Illinois show lower but still substantial user counts, indicating additional areas where user engagement can be further optimized.

<img width="340" alt="image" src="https://github.com/user-attachments/assets/8990c7e6-8d38-4ccb-9a48-7806a14b87e1">

 
**4. Top 10 states by user count**

Request for action
Additionally, we need clarification on certain fields such as product category, hierarchy, state/language codes, and the relationship between quantity and sales in transactions.
Could you provide any documentation or insights to help us address these issues? This information will enhance the accuracy of our analysis.

Thanks and regards,
Ram Charan Jangam

