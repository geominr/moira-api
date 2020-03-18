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




Custom Cause of Death
Custom ICD categories can be selected by providing individual ICD codes. The ICD code revision is required to correspond to the selected time period.


```python
payload={"""
         
         
         
         """
         "timePeriod": [2000, 2001, 2002, 2003, 2004, 2005],
         """
         National, state, and county level data is available. 
         The geographic region can be individually selected or 
         joined into custom aggregated regions.

         NOTE:
         Alaska data is not available at the county level before 1979.
         Alaska and Hawaii data is not available before 1960.
         New Jersey data is not available from 1962-1963.
         """
         # "National", "State", or "County"
         "geoLevel":  "County",
         
         
         """
         
         
         """
         "abbreviatedStates":["CA","AZ"],
         """
         
         
         """
         # Optional:
         # "Aggregate selected states" or "Aggregate selected counties"
         "geoAggregate": "Aggregate selected states",
         
         # Select all counties within selected states
         "allCounties":"true",
         """
         
         
         """
         "ageGroup": "All Default",         
         """
         
         
         """
         "ageGroupCategory": "12",         
         """
         
         
         """
         "race": "All",         
         """
         
         
         """
         "sex": "All",         
         """
         Cause of Death Categories:
         Standard 63 OCMAP categories are available for cause of death grouping. 
         Additionally, the 113 MOIRA specific categories (ICD-9 and/or ICD-10 codes only)
         """
         "codCategory": "113",
         """
         Cause of Death codes can be found at:
            https://moira.pitt.edu/codCategory/63
            https://moira.pitt.edu/codCategory/113
         """
         "cod": [31,32,33,34],
         """
         Group by time period, age group, race and sex. 
         MOIRA groups data geographically by default.
         """
         "groupBy": ["timePeriod","ageGroup","race","sex"},
         """
         Calculate mortality rates per 1000 or per 100,000         
         """
         "ratesPer": 1000,
         """
         
         """
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
