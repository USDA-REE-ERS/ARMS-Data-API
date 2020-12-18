## 1. Introduction

USDA’s Economic Research Services (ERS) is making data from USDA’s Agriculture Resource Management Survey (ARMS) available through an Application Programming Interface (API) to better serve customers. The data in the API are available in JSON format and provide attribute-based querying. The data are also available in bulk files.

This page provides a brief explanation of the API to get you started. For user convenience, we provide demos using R: open source statistical programming language.

A list of variables, by report, is provided in section 3. An exhaustive list of all variables and detailed descriptions is found in the file <a href="https://www.ers.usda.gov/media/10257/allvariables.csv">AllVariables.csv</a>.

## 2. Getting Started with the ARMS-Data-API

You may use the ARMS Data API to develop a service to search, display, analyze, retrieve, view, and otherwise "get" information from ARMS data.

### 2.1 
Users of the ARMS data API are required to sign up for an API key via api.data.gov, a free API management service for Federal agencies. A valid email address is required to obtain a key. Once users have completed the signup requirements, the API key will be automatically sent to the user-provided email address. API keys provide a means to notify users of changes in the APIs.

**Note:** A key is not required to use the bulk download facility.

### 2.2 Using the API
The API support GraphQL and REST API methods:

* Rest API

Rest API endpoint: https://api.ers.usda.gov/data/

**REST Methods**

The REST API supports POST and GET methods for calling data, except where noted.

Each endpoint also supports the OPTIONS method that returns the endpoint schema and details on input fields and requirements, such as format, whether the field allows

**/arms/state**

This API resource gets all States and the metadata and variables available for each of the States.

_Input fields:_ not needed

_Method_: supports GET only

``` 
GET https://api.ers.usda.gov/data/arms/state?api_key=YOUR_API_KEY
```

**/arms/year**

This API resource gets all available years for which data are available.

_Input fields:_ not needed

_Method:_ supports GET only

```
GET https://api.ers.usda.gov/data/arms/year?api_key=YOUR_API_KEY
```

**/arms/surveydata**

_Input fields:_ "Year" is a required field; at least one of the two input fields ("report" or "variable") is also required

_Method:_ supports GET and POST

_OPTION:_ returns the schema for the survey data REST resource

```
POST https://api.ers.usda.gov/data/arms/surveydata?api_key=YOUR_API_KEY

{
 "year": [2011, 2012, 2013, 2014, 2015, 2016]
 "state": "all"
 "variable": "igcfi"
 "category": "NASS Regions"
 "category2": "Collapsed Farm Typology"
}
```

The above retrieves "Gross Farm Income" for years 2011 through 2016 for "All States" and broken by Category = NASS regions and Category2 = Collapsed Farm Typology.

```GET https://api.ers.usda.gov/data/arms/surveydata?api_key=YOUR_API_KEY?year=2015,2016&state=all&report=income+statement&farmtype=operator+households&category=collapsed+farm+typology&category_value=commercial```
This GET request returns income statements for years 2015 and 2016 of commercial farms across all States.

**Note:**  Multiple values for fields that allow multiple values are separated by "commas" (,), and names of fields exceeding more than one word are separated by a "plus" (+). Different query parameters such as year, states, report, etc. are separated by an "ampersand" (&).

**/arms/category**

This API resource lists categories and subcategories within each category. It also provides relevant metadata and variables available for each of the categories and subcategories.

_Input fields:_ not required but can be passed on; see below.

_Method:_ supports GET and POST

 This resource can be used in two ways:

   1. When used without any input variables, ALL categories and subcategories are returned.

`POST https://api.ers.usda.gov/data/arms/category?api_key=YOUR_API_KEY`

`GET https://api.ers.usda.gov/data/arms/category?api_key=YOUR_API_KEY`

   2. When used with a specific category name, all details and subcategories within that category are returned

`POST https://api.ers.usda.gov/data/arms/category?api_key=YOUR_API_KEY`

```{ 
 "name": "Collapsed Farm Typology" 
}
```   

`GET https://api.ers.usda.gov/data/arms/category?id=age,ftypll`   

This query returns Category-related information for "Operator age" (id=age) and "Farm Typology" (id = ftypll)

**/arms/report**

 This API resource gets available reports with the relevant metadata and variables available for each report. This resource can be used  in two ways:

   1. Retrieve a list of ALL reports
    
`POST https://api.ers.usda.gov/data/arms/report?api_key=YOUR_API_KEY`

`GET https://api.ers.usda.gov/data/arms/report?api_key=YOUR_API_KEY`

   2. Retrieve metadata and variables for a specific report based on name, ID, or keyword.

`POST https://api.ers.usda.gov/data/arms/report?api_key=YOUR_API_KEY`

```{
"name": "balance sheet"
}
```

**/arms/variable**

This API resource gets variables with the relevant information and metadata for each of the variables used in ARMS. This resource can be used in two ways:

  1. List ALL variables.

```POST https://api.ers.usda.gov/data/arms/variable?api_key=YOUR_API_KEY```

```GET https://api.ers.usda.gov/data/arms/variable?api_key=YOUR_API_KEY```

  2. Search "variable" by ID, report, name, and keywords. All input fields are optional, and one can use all or none of them to get the desired data.

```POST https://api.ers.usda.gov/data/arms/variable?api_key=YOUR_API_KEY
 {
  "report": "Business Balance Sheet"          
  "name": "Liabilities current debt"
 }
 ```
 
**/arms/farmtype**

One can use this REST resource to get all Farm Types or search by keyword, name, or ID.

_Input fields:_ optional

_Method:_ supports GET and POST

`POST https://api.ers.usda.gov/data/arms/farmtype?api_key=YOUR_API_KEY`

`{
  "name": "operator households"
}` 

`GET https://api.ers.usda.gov/data/arms/farmtype?api_key=YOUR_API_KEY`
`GET https://api.ers.usda.gov/data/arms/farmtype?api_key=YOUR_API_KEY&name=operator+households`

* GraphQL

GraphQL is a modern method to easily interact with an API. The GraphQL endpoint for ARMS data is https://api.ers.usda.gov/data/arms/graphql.

* GraphQL methods

GraphQL is for advanced users. If you are not familiar with GraphQL, use our REST API. A GraphQL client, such as ChromeiQL, is needed to interact with the ARMS API. <a href="https://graphql.org/">More information on GraphQL</a> is available.

## 3. Reports

ARMS reports are a collection of financial data points about U.S.-based farms that can be sorted and viewed in a variety of ways, including national level, State level, farmer type, and farmer age.

The ARMS team has created some commonly used tailored reports from the ARMS dataset.  Currently, the following tailored reports are available on the WebTool and through the API:

* Farm Business Balance Sheet
* Farm Business Income Statement
* Farm Business Financial Ratios
* Structural Characteristics
* Farm Business Debt Repayment Capacity
* Government Payments
* Operator Household Income
* Operator Household Balance Sheet
 
## 4. Variables
Each report contains a different set of variables. A list of all variables sorted by report and with detailed descriptions and short IDs may be found in the file <a href="https://www.ers.usda.gov/media/10257/allvariables.csv">AllVariables.csv</a>.

## 5. Available Categories
Categories are meta tags that provide context for the data. The available categories are listed below with their short IDs inside the parenthesis. Values for each category and their definitions may be found in the file <a href="https://www.ers.usda.gov/media/10257/allvariables.csv">AllVariables.csv</a>.

* Residence farms
* Intermediate farms
* Commercial farms

Economic Class (sal)

* $1,000,000 or more
* $500,000 to $999,999
* $250,000 to $499,999
* $100,000 to $249,999
* less than $100,000

Farm Typology (ftyppl)

* Retirement farms (2012 to present)
* Off-farm occupation farms (2012 to present)
* Farming occupation/lower sales farms (2012 to present)
* Farming occupation/moderate-sales farms (2012 to present)
* Midsize farms (2012 to present)
* Large farms (2012 to present)
* Very large farms (2012 to present)
* Nonfamily farms (2012 to present)
* Retirement farms (1996 through 2011)
* Residential/lifestyle farms (1996 through 2011)
* Farming occupation/lower sales farms (1996 through 2011)
* Farming occupation/moderate-sales farms (1996 through 2011)
* Large farms (1996 through 2011)
* Very large farms (1996 through 2011)
* Nonfamily farms (1996 through 2011)

Operator age (age)

* 34 years or younger
* 35 to 44 years old
* 45 to 54 years old
* 55 to 64 years old
* 65 years or older

Farm Resource Region (reg)

* Heartland
* Northern Crescent
* Northern Great Plains
* Prairie Gateway
* Eastern Uplands
* Southern Seaboard
* Fruitful Rim
* Basin and Range
* Mississippi Portal

NASS region (n5reg)

* Atlantic region
* South region
* Midwest region
* Plains region
* West region

Production Specialty (spec)

* Tobacco, cotton, peanuts
* Other field crops
* Cattle
* Dairy
* Hogs, poultry, other
* Vegetables
* Nursery and greenhouse
* All cash grains
* Specialty crops
* All other livestock
 
## 6. Updates and Revisions

**December 18, 2020**

A database update was released on December 18, 2020, containing new data for 2019 and updates and revisions to historical data across 1996-2018. See full description on the ARMS data product <a href="https://www.ers.usda.gov/data-products/arms-farm-financial-and-crop-production-practices/update-and-revision-history/">here</a>).

This update includes the release of new farm and household finances data for 2019 and includes changes to improve internal consistency in data-handling methods.

* Previously posted estimates of Adjusted Gross Income (AGI) for 1996-2018 were updated to reflect a change in methods for how limitations to deduct retirement contributions  as well other deductions are modeled across time. The new limitations result in an increase in estimated AGI of $4,400 at the national level (5.3 percent) compared to the   previous method.

* Estimated value of production for farm operations in 2018 was revised downward by $3.83 billion (1.1 percent) due to corrections in processing the underlying survey data, especially potato production, and due to a restatement of a marketing contract record. Value of production is directly reported in the structural characteristics tailored report. Also, because value of production is used to classify the commodity specialization of operations restating value of production resulted in changes to the number of farms for several of the commodity specialization categories. The largest increase in operations within a single category was in operations classified as primarily specialty crop (fruit, vegetable, and nut) operations, raising the total of specialty crop operations by 1029, or 0.7 percent, to 148,931 operations, with smaller increases in the number of farms classified as "other field crops" and "general cash grains" operations, and also resulted in a corresponding decrease in the number of farms otherwise classified as "wheat," "corn," "soybeans," or "tobacco, cotton, or peanuts" operations.

* For 2018 data, estimates for four financial ratios included in the financial indicators report are updated with new information on agricultural wage rates released in the National Agricultural Statistics Service (NASS) Farm Labor survey. The agricultural wage rate is used as an implicit hourly wage for reported operator labor hours. The implicit labor cost is used in calculating the rate of return to assets and equity, operating profit margin, and economic cost-to-output ratio. The wage rate increase reduced the return to assets and equity by 0.1 and 0.2 percentage points, to 0.4 and 0.0 percent respectively, and reduced the operating profit margin by 1.2 percentage points to 3.1 percent. It increased the economic cost-to-output ratio by 2.3 percentage points to 111.5.

* For 2011 data, a procedure was implemented to impute reported government payment amounts by category for certain operations, resulting in a small (less than 0.1 percent) change in the number of operations receiving direct payments and countercyclical-type payments, as well as the average amounts of these types of payments, and the average gross cash and net cash farm income for recipients of these types of program payments. The total amount of government payments recorded in 2011 is unchanged.

* The standard method for calculating land tenure status for farm operations, which classified operations as either a full ownership operation, part owner operation, or full tenant operation, was extended to data from 1997 to 2003, changing the share of full owner operations downward by 0.4 percentage points, on average, and part owner operations up an equal amount. In determining the categorical status, the standard method accounts for acres rented in by operations as well as acres rented out.

**December 10, 2019**

**Errata:** On December 10, 2019, the data found in Tailored Reports: Farm Structure and Finance was revised to correct an error in the calculation of average and median net farm income estimates for 2012-17. Revised net farm income estimates impact all "subject" and "filter" selections for average and median net farm income, 2012-17. Estimates of U.S. average net farm income, for example, are revised downward by -4.5 to -8.8 percent across the 6 years, lowering these estimates by $1,858 to $4,409. The error was restricted to the underlying calculation of the net farm income variable alone; no other data values were affected within the income statement. However, net farm income is a component in the calculation of a subset of financial ratios also found in the tailored reports. Thus, the reported rate of return on assets, rate of return on equity, operating profit margin, and term debt coverage ratio estimates for 2012-17 are revised as well.

New data in this release include:
* In the Government Payments tailored report, a revised classification of government payments from 1996 to the present into five separate program types (direct payments,    countercyclical-type payments, marketing loan benefit payments, conservation payments, and other program payments). These categories are described more fully in <a href="https://www.ers.usda.gov/publications/pub-details/?pubid=85833">The Evolving Distribution of Payments From Commodity, Conservation, and Federal Crop Insurance Programs</a>(EIB-184, November 2017). The change will improve comparisons across years because the previous method for categorizing government programs referred to individual farm bill programs that did not persist across the entire time period.

* In the Farm Household Income tailored report, a new variable from 1996 onward showing a household's Adjusted Gross Income (AGI). Adjusted Gross Income is calculated using formulas and specifications found in IRS Form 1040 of each corresponding tax year. AGI as reported in the table includes deductions for self-employed health insurance and self-employment taxes. Adjusted Gross Income as calculated from ARMS data is described more fully in <a href="https://www.ers.usda.gov/publications/pub-details/?pubid=89355">Estimated Effects of the Tax Cuts and Jobs Act on Farms and Farm Households</a>(ERR-252, June 2018).

* Within the Farm Business Income Statement tailored report, a new variable is shown from 2012 onward for Adjusted Breeding Livestock Income. The variable is one of five variables that account for the difference between net cash farm income and net farm income. For operations with income from the sale of breeding livestock, net cash farm income includes the cash receipts from the sale of breeding livestock, while net farm income includes only the realized gain or loss from the sale of breeding livestock, calculated as cash receipts minus acquisition costs. All five variables accounting for the difference between net cash farm income and net farm income in the Farm Business income statement (nonmoney income, value of inventory change, depreciation, labor non-cash benefits, and adjusted breeding livestock income) are show in the farm business income statement beginning with this release.

Additionally, this update improves data-handling methods for improved consistency across time.

* ERS' standard imputation method for handling missing survey data on farm debt for 2012 to the present was extended back to 2009 data. The change in methods resulted in an increase in total debt by 7 percent, on average, across 2009-11, and also affect debt-based financial ratios including the current ratio, the working capital-to-expense ratio, the debt-to-asset ratio, and the term debt coverage ratio.

* Prevailing interest rate data from Chicago Federal Reserve's <a href="https://www.chicagofed.org/publications/agletter/index">AgLetter</a> was used to replace multiple sources of interest rate data across 1996-2018. The difference in interest rates used for formula calculations averaged 65 basis points (0.65 percentage points) per year. This data is used in calculating Financial Ratios and Debt Repayment Capacity tailored reports from 1996 to 2008, and market interest rates are also used to impute debt appearing in the Farm Business Balance Sheet and the Operator Household Balance Sheet from 1996 to 2018. Debt-based financial ratios including the current ratio, the working capital-to-expense ratio, the debt-to-asset ratio, and the term debt coverage ratio are similarly impacted.

* In the Structural Characteristics tailored report, the standard method for calculating the classification of farms by their land tenure categories "including a full owner, part owner, and full tenant category" was consistently applied to all farm operations across time, changing previously-reported values for 1997 and 2004-06. The standard method takes into account acres rented in by operators as well as acres rented out. From 3 to 5 percent of farms switched from their previous categories as a result of changes in 1997 and 2004-06.


**August 29, 2019**

A database update was released on August 27, 2019, containing revised data for 2017 and other changes (see full description on the ARMS data product <a href="https://www.ers.usda.gov/data-products/arms-farm-financial-and-crop-production-practices/update-and-revision-history/">here</a>).

* The “searching”, and “formatting” properties of the “info” section of the API query result have been removed in this version of the API.
* Each record of the category query result contains a new category “description” property.
* Each record of the report query result contains a new report “desc” property.
* Each record of the farmtype query result contains a new farmtype “desc” property.
* Each record of the surveydata query result contains a new “decimal_display” property, indicating the proper number of decimals to display for the value.
