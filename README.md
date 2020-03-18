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
payload = str(payload).replace("'","\"")
headers = {'Authorization': log_in(USERNAME, PASSWORD),
           'Content-Type': "application/json",
           'Host': "moira.pitt.edu"
          }
response = requests.request("POST", 
                            "https://moira.pitt.edu/api/get_rate", 
                            data=payload, 
                            headers=headers)

print(response.text)
```
