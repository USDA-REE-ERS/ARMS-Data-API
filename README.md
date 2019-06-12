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

* GraphQL

GraphQL is a modern method to easily interact with an API. More information on GraphQL is available. A GraphQL client, such as ChromeiQL, is needed to interact with the ARMS API. 

The GraphQL endpoint for ARMS data is https://api.ers.usda.gov/data/arms/graphql.
 
* Rest API

Rest API endpoint: https://api.ers.usda.gov/data/

**REST Methods**

The REST API supports POST and GET methods for calling data, except where noted.

Each endpoint also supports the OPTIONS method that returns the endpoint schema and details on input fields and requirements, such as format, whether the field allows

**/arms/state**

This API resource gets all States and the metadata and variables available for each of the States.

_Input fields:_ not needed

_Method_: supports GET only

`GET: https://api.ers.usda.gov/data/arms/state`

**/arms/year**

This API resource gets all available years for which data are available.

_Input fields:_ not needed

_Method:_ supports GET only

`GET: https://api.ers.usda.gov/data/arms/year`

**/arms/surveydata**

_Input fields:_ "Year" is a required field; at least one of the two input fields ("report" or "variable") is also required

_Method:_ supports GET and POST

_OPTION:_ returns the schema for the survey data REST resource

`POST https://api.ers.usda.gov/data/arms/surveydata
{
 "year": [2011, 2012, 2013, 2014, 2015, 2016]
 "state": "all"
 "variable": "igcfi"
 "category": "NASS Regions"
 "category2": "Collapsed Farm Typology"
}`

The above retrieves "Gross Farm Income" for years 2011 through 2016 for "All States" and broken by Category = NASS regions and Category2 = Collapsed Farm Typology.

`GET https://api.ers.usda.gov/data/arms/surveydata?year=2015,2016&state=all&report=income+statement&farmtype=operator+households&category=collapsed+farm+typology&category_value=commercial`
This GET request returns income statements for years 2015 and 2016 of commercial farms across all States.

**Note:** Multiple values for fields that allow multiple values are separated by "commas" (,), and names of fields exceeding more than one word are separated by a "plus" (+). Different query parameters such as year, states, report, etc. are separated by an "ampersand" (&).


