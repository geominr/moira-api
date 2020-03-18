# moira-api
Instructions for using the MOIRA API

## Getting Started


## Login

```python
def log_in(username, password):
    
    # Set configs
    payload = "username="+username+"&password="+password
    headers = {'Host': "moira.pitt.edu",'Content-Type': "application/x-www-form-urlencoded"}
    
    # Generate fresh API key
    response = requests.request("POST", "https://moira.pitt.edu/api/login", data=payload, headers=headers)
    x = response.json()
    token = "Bearer {}".format(x["token"]) 
    
    # Return API key
    return token 
```

## Payload
The API takes payload of selection criteria for each MOIRA query in the form of a dictionary with the following fields:
* timePeriod
    * 
* geoLevel
    * "National", "State", and "County" level data is available. The geographic region can be individually selected or joined into custom aggregated regions.
* abbreviatedStates
    * Include all state abbreviations in a list
* countyFIPS
    * Include all 5-digit county FIPS codes in a list
* allCounties
    * if "true", selects all counties from the states in abbreviatedStates
* geoAggregate
    * "Aggregate Selected States" or "Aggregate Selected Counties"
* ageGroup
    * List of age groupings such as "15 - 19", "20 - 24", etc.
* ageGroupCategory
    * "12", "13" or "Custom"
* race
    * "White", "Black", "American Indian or Alaska Native", "Asian or Pacific Islander" are available depending on the time period
* hispanicOrigin
    * "Hispanic or Latino", "Not Hispanic or Latino", "Not Stated"
* sex
    * "Male" or "Female" or "All"
* codCategory
    * "63": Standard *63* OCMAP categories are available for cause of death grouping. 
    * "113": Additionally, the *113* MOIRA specific categories (ICD-9 and/or ICD-10 codes only)
    * "Custom": Custom ICD categories can be selected by providing individual ICD codes. The ICD code revision is required to correspond to the selected time period. See example in code snippet below.
* cod
    * Cause of Death codes can be found at:
    * https://moira.pitt.edu/codCategory/63
    * https://moira.pitt.edu/codCategory/113
* groupBy
    * Group results by "timePeriod", "ageGroup", "race", "sex", "hispanicOrigin"
* ratesPer
    * Calculate rates per 1,000 or 100,000 people
* ageAdjustment
    * "true" or "false" indicates whether rates are caculated with age adjustment

```python
payload={"timePeriod": [2000, 2001, 2002, 2003, 2004, 2005],
         "geoLevel":  "County",
         "abbreviatedStates":["CA","AZ"],
         # Optional geographical grouping/aggregation:
         "geoAggregate": "Aggregate Selected Counties",
         "allCounties":"true",
         # Demographics:
         "ageGroup": "All Default",
         "ageGroupCategory": "12",
         "race": "All",         
         "sex": "All",         
         "codCategory": "113",
         "cod": [31,32,33,34],
         # Data grouping and calculation
         "groupBy": ["timePeriod","ageGroup","race","sex"},
         "ratesPer": 1000,
         "ageAdjustment": "true"
        }
```

## Query

```python
# The API doesn't like single quotes
payload = str(payload).replace("'","\"")
headers = {'Authorization': log_in(USERNAME, PASSWORD),
           'Content-Type': "application/json",
           'Host': "moira.pitt.edu"
          }
          
# POST request returns response with MOIRA as data comma-delimited text
response = requests.request("POST", 
                            "https://moira.pitt.edu/api/get_rate", 
                            data=payload, 
                            headers=headers)
print(response.text)
```
#### Out:
"Time_Period","Time_Period_Label","Formatted_FIPS","County","State","Deaths","Population","Age_Adjusted_Rate"
"1","2000","04001","Apache","AZ","SUPPRESSED","69423","SUPPRESSED"
"1","2000","04003","Cochise","AZ","27","117755","0.210001"
"1","2000","04005","Coconino","AZ","SUPPRESSED","116320","SUPPRESSED"
"1","2000","04007","Gila","AZ","15","51335","0.212762"
...
