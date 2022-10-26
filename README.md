# Ed-Fi-Community-Postman-Collection

Ed-Fi Resource Postman Collection Scripts 
    This is a working list of the Ed-Fi Resouces (aka EndPoints) and their Postman scripts.  These scripts will help you Get, Put, Post and Delete data using a JSON format against your Ed-Fi ODS.   These scripts are there to help you verify data in your ODS as well as help you determine how to build a JSON formated function call. 

## Legal Information

Copyright (c) 2021 Ed-Fi Alliance, LLC and contributors.

Licensed under the [Apache License, Version 2.0](LICENSE) (the "License").

Unless required by applicable law or agreed to in writing, software distributed
under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR
CONDITIONS OF ANY KIND, either express or implied. See the License for the
specific language governing permissions and limitations under the License.

See [NOTICES](NOTICES.md) for additional copyright and license notifications.

------------------------------------------------------------------------------------------- 
How to use Postman API Platform to access the Ed-Fi ODS using JSON API calls.
 
What is Postman? 

Postman is an open-source platform for interfacing with APIs. It provides the ability to use templates to reduce the effort required to make every API request by hand. 

How is Postman different from Swagger?

Swagger provides a convenient way to access the Ed-Fi API. Swagger is advantageous because it displays the JSON structure required for submitting data and lists all of the API endpoints. Postman is a lower-level tool and is helpful in situations where more direct access to the API is needed.  For example, if you do not have access to Swagger in the environment where you ODS is located or in a production environments where Swagger is not usually installed.   

Prerequisites
A working Ed-Fi instance
A key / secret pair for that instance
Access to Swagger for that instance, or an instance that has the same extensions (if any)
Step1: Install and Configure Postman
Download the Ed-Fi Postman collections included in this Ed-Fi Exchange branch. 
Download and install the Postman app from:  https://www.postman.com/downloads/
After the install is complete, click 'Skip and go to the app'
Import the Ed-Fi Postman collection: 
Open Postman, navigate to My Workspace, and click the Collections icon.
Click “Import”
Select ‘Upload Files'
Now configure your Postman installation for your API. You will need to obtain two urls: “dataManagementApi” and “oauth” values.  Below is one way you may be able to obtain this information. 
Inside of IIS, expand the desired SQL Server version; for example edfi_53_mssql, and click on the webApi link.
To browse the WebAPI, click on the Manage Application -  Browse Application link; for example ‘Browse *.443 (https)’ Browse to the web API.   
Note the information located under these two urls: “dataManagementApi” and “oauth” values.  You will need these two values inside of Postman on the Authorization tab.



Get a key and secret value that will work with your API. These will be displayed when a new sandbox is created in the sandbox admin app, or when a new API client is generated in the Admin App. See documentation here about generating new API clients. 
In Postman, open the Ed-Fi collection and click on the Variables tab. Fill out the four values in both the initial value and current value columns, with ApiUrl as the dataManagementApi value, ClientId as the key, ClientSecret as the secret, and oauth as the oauthTokenUrl. Save your values.  




Variable

	

Initial Value

	

Current Value




ApiUrl

	

“dataManagementApi” value

	

“dataManagementApi” value




ClientID

	

key

	

key




ClientSecret

	

secret

	

secret




oauthTokenUrl

	

“oauth” value

	

“oauth” value




        




Once the variables are saved, you will need to generate a new access token. Go back to the Authentication tab, scroll to the bottom, and click the "Get New Access Token" button. Set the Configuration Options to match the values in the Variables tab as well as other default values shown below. 

                




Possible error messages:
If you receive an error when trying to generate the Access Token, then go back and make sure your ClientId (aka Key) and Client Secret are correct. Make sure that the Client Secret is the Non-Encrypted value. 
If you get an "authentication failed' message, check the console. 
If there is an "Error: unable to verify the first certificate"  message, or other types of error messages,  then go to file -> settings and turn off "SSL Certificate Verification" and try it again.



                

                 




If the request is successful, the token will pop up. Click the 'Use token' button
Step 2: Try Some Simple GET Requests

Every time you use Postman you will have to get a fresh access token. This is done through the Authorization tab and using the the 'Get access token' and 'Use token' buttons above.

GET a list of schools 

Open the school folder and the GET request.  
Make sure that the address bar is set to "GET" and has the value "{{ApiUrl}}ed-fi/schools".  The {{ApuUrl}} will get replaced with the variable you defined in Step 1.
On the Authorization tab, make sure the Type value is set to "inherit auth from parent"
If this is not set, then you will receive a ‘401 Unauthorized’ error message
Click the send button.
There should be a list of schools returned.  

           




STEP 3: GET with a filter

Get a list of schools that belong to a specific LEA. 

You can insert the field and value on the Params tab, see example below
Or update the URL with parameters that are passed in
"{{ApiUrl}}ed-fi/schools?localEducationAgencyId=255901

                      




GET with a Limit number of rows returned. 

You can insert the word ‘Limit’ in the Key field and the number of records you want returned in the Value column.

              




STEP 4. POST a record

Submit a school. 

This is an 'upsert' operation in the database, either adding a new record if it does not exist or changing values if it does. POST records require building a JSON body that has the information being submitted. Use the ‘Body’ tab for the JSON structure. See step 5 ‘Use Swagger to find the endpoints and JSON specs’ step below for details. 
For this example, we are specifying the name of the school, the schoolId (which will also be the EducationOrganizationId in the database), the type of EducationAgency it is, and the grades.  These are the minimum required elements needed for a POST command, for this endpoint.  
Click send. If a successful message is returned the record should be available in the database.

          

STEP 5. Use Swagger to find the endpoints and JSON specs

Swagger is the best way to see what API endpoints are available and how to interact with them. The Swagger instance should have the same extensions (if any) as the API that is being addressed in Postman. Ideally they are from the same server. Note that the Swagger for non-extended Ed-Fi versions can be found at https://api.ed-fi.org/  Follow the documentation link for the appropriate version.

Expand any endpoints to show the address to use for that endpoint. This (along with the ApiUrl variable) is what goes in your address bar.

           




Swagger also shows the JSON specification that is needed to POST data.  Expanding the POST option under an endpoint will show 'example value'.  Copy this to a text editor and replace the values with the values that are to be posted.


Clicking the "model" tab will show what elements are required by the API.  These are the ones with a red asterisk.  

       

If you do not insert any additional information, then delete any values that you are not submitting.

Additional Resources

Tech Doc Links to Identify the Resource (EndPoint) Loading Order: 

Student Information System Certification Resource Dependency Order: 

           https://techdocs.ed-fi.org/display/ODSAPIS3V53/Resource+Dependency+Order

API Metadata Resource Loading Order:

           https://api.ed-fi.org/v5.3/api//metadata/data/v3/dependencies
