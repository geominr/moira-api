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

```python
payload = {"timePeriod": [2000, 2001, 2002, 2003, 2004, 2005],
 "geoLevel":  "County",
 "abbreviatedStates":["CA","AZ"],
 "allCounties":"true",
 "ageGroup": "All Default",
 "ageGroupCategory": "12",
 "race": "All",
 "sex": "All",
 "codCategory": "113",
 "cod": [31,32,33,34],
 "groupBy": ["timePeriod","ageGroup"],
 "ratesPer": 1000,
 "ageAdjustment": "false"
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
"1","2000","04009","Graham","AZ","SUPPRESSED","33489","SUPPRESSED"
"1","2000","04011","Greenlee","AZ","SUPPRESSED","8547","SUPPRESSED"
"1","2000","04012","La Paz","AZ","SUPPRESSED","19715","SUPPRESSED"
