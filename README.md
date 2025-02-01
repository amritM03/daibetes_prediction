# daibetes_prediction
# IBM Cloud Machine Learning API - README

## Overview
This project demonstrates how to interact with IBM Cloud's Machine Learning API to make predictions using a deployed AI model. It includes authentication, token retrieval, and making API requests for scoring data.

## Prerequisites
Before using this script, ensure you have:
- An **IBM Cloud** account
- An **API Key** from IBM Cloud
- A deployed **Machine Learning model** on IBM Watson Machine Learning

## Installation
1. Install Python dependencies:
   ```bash
   pip install requests
   ```

2. Set up an IBM Cloud Machine Learning service and retrieve your **API Key** and **deployment endpoint URL**.

## Usage
### 1. Authentication
The script retrieves an **OAuth token** using your **IBM Cloud API Key**:
```python
API_KEY = "<your API key>"
token_response = requests.post('https://iam.cloud.ibm.com/identity/token', 
                               data={"apikey": API_KEY, "grant_type": 'urn:ibm:params:oauth:grant-type:apikey'})
mltoken = token_response.json()["access_token"]
```

### 2. Define Input Data
Modify `payload_scoring` to match the format expected by your deployed model:
```python
payload_scoring = {
    "input_data": [{
        "fields": [array_of_input_fields], 
        "values": [array_of_values_to_be_scored, another_array_of_values_to_be_scored]
    }]
}
```

### 3. Make Predictions
Use the retrieved token to send a request to the model's deployment endpoint:
```python
response_scoring = requests.post(
    'https://private.eu-gb.ml.cloud.ibm.com/ml/v4/deployments/bf52e823-1a83-43d9-b71e-b7760500bbba/predictions?version=2021-05-01',
    json=payload_scoring,
    headers={'Authorization': 'Bearer ' + mltoken}
)
print("Scoring response")
print(response_scoring.json())
```

## Notes
- Replace `<your API key>` with your IBM Cloud API key.
- Replace `<deployment-id>` with your actual deployment ID.
- Ensure that `fields` and `values` match the structure expected by your model.

## Troubleshooting
- If authentication fails, verify that your API Key is correct.
- Ensure your deployment endpoint is active and accessible.
- Check the JSON structure of `payload_scoring` to match your model's expected format.

## License
This project is released under the MIT License.

