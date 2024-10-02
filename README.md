# Install instructions

## Prerequisites

- Windows, macOS, or Linux

## Step 1: Install Postman client

1. Download Postman client at https://www.postman.com/downloads/

## Step 2: Import postman collections and environment json files
2. Import the postman collections and environment via https://learning.postman.com/docs/getting-started/importing-and-exporting/importing-from-git/

Note: Another alternative would be to import Postman collection (.json) files and environment manually to the Postman client via https://learning.postman.com/docs/getting-started/importing-and-exporting/importing-data/


## Step 3: Select the imported environment before running the API tests 
3. To use the new environment, select environment named `pet_circle` from the environment selector at the top right of the workbench. This makes it the active environment and sets all variables to the values specified in the environment.

<img width="387" alt="image" src="https://github.com/user-attachments/assets/0e367d9f-1cc5-4823-b1c5-4417d64a2225">

## Step 4: Run API collections via Postman runner
4. Select the postman collection to be executed manually - https://learning.postman.com/docs/collections/running-collections/intro-to-collection-runs/#configure-a-collection-run

## Step 5: Debug and monitor test execution results
5. Postman would display test execution result and all related details - https://learning.postman.com/docs/collections/running-collections/intro-to-collection-runs/#configure-a-collection-run 

## Note: Alternative method of running tests using Github actions (newman cli)
1. Go to https://github.com/CloseTheTicket/petcircle_demo/actions
2. Click on the available runner with the runner name (newman_api_test_runner)
3. Click on the `Run workflow` button on the rightmost part of the page
4. On the displayed popup click `Run workflow` button in green background color


Related links:
- https://petstore.swagger.io/#/
- https://learning.postman.com/docs/introduction/overview/

# Observed testing notes while writing API tests:
- Since generated pet id from POST /v2/pet is too large for postman's javascript to handle - it causes saved values to be rounded off or not be handled properly. Solution is to avoid saving responses as json, instead saving it as raw text, and using regex to extract the generated id:
  ```
    // Use a regular expression to extract the ID as a string
    let idMatch = responseText.match(/"id":(\d+)/);
    
    if (idMatch && idMatch[1]) {
        let petId = idMatch[1]; // Extracted id as a string
        console.log("Extracted Pet ID:", petId);
  ```
- POST `/v2/pet` contains a key value: `"id": 0` wherein it allows passing of custom id as identifier. This can cause additional issues in record duplication. Identifier like pet_id for the endpoint should be auto generated instead.
- POST and PUT `/v2/pet` endpoints contains no built in field validations to sanitize data inputs
- Identifier id is currently a large int data type, which can cause representation issues on UI especially for javascript, can be changed to a string instead as to be more consistent across any representation. It also increases the chance of generating more unique values
- Added grouped assertions as packages but currently only available on Postman desktop clients and not on newman, so I had to re export all assertions as post requests scripts to be visible on newman runs https://learning.postman.com/docs/tests-and-scripts/write-scripts/package-library/ 

  
