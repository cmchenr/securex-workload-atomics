### **SecureX Atomic** - *"Workload - Generic API Request"* - Test Plan

Enables generic access to the Secure Workload API.  Secure Workload uses a digest authentication format that is not natively supported by SecureX Orchestrator.  This action provides the appropriate authentication and request formatting to integrate with the REST API.

Target: Secure Workload URL.  For SaaS it would be something like "customer.tetrationcloud.com".  The target does not need authentication.  For testing we will use "https://tet-pov-rtp1.cpoc.co"

***API Key:*** API Key for Secure Workload target.

***API Secret:*** API Secret for Secure Workload target.

***Method:*** GET/POST/PUT/DELETE

***JSON Body:*** Body of the request for POST/PUT methods

Returns a "Response Body" as a JSON-formatted for successful requests.

More information about this API: https://www.cisco.com/c/en/us/td/docs/security/workload_security/tetration-analytics/sw/config/b_Tetration_OpenAPI/m_OpenAPI_Authentication.html

**Test Environment**
 1. Log into https://tet-pov-rtp1.cpoc.co In the upper right hand
    corner, in a blue box, you should see "SecureX-QA".  That is your
    Tenant. 
 2. Ensure you have an API key.  If you don't have one, you can create one by going to: Settings -> API Keys
 3. In the slide out menu on the left, click Segmentation.  The tests below act against the "applications" API.  A sample application has been configured called "Sample Application".  In the POST/PUT/DELETE tests, you will create a new application, modify it's name, and delete it.
 4. In SecureX Orchestrator, create a target pointing to "https://tet-pov-rtp1.cpoc.co" with no authentication.

---
**Testing GET**
Path: `/openapi/v1/applications`
Method: `GET` 

Output:  You should see the JSON representation of the "Sample Application" that is visible.

---
**Testing POST**
Path: `/openapi/v1/applications`
Method: `POST`
JSON Body: 
```
{"app_scope_id": "606f61a5755f020d92a0f349", "name": "SecureX Application", "absolute_policies": [{"provider_filter_id": "606f61a5755f020d92a0f349", "consumer_filter_id": "606f61a5755f020d92a0f349", "action": "ALLOW", "l4_params": [{"proto": 6, "port": [80, 80]}]}], "catch_all_action": "ALLOW"}
```
After testing post, grab the "id" value of the Response Body for use in the next 2 commands.  Refresh the "Segmentation" page in Tetration.  You should now see a new Application Workspace named "SecureX Application"

---
**Testing PUT**
Path: `/openapi/v1/applications/{id_from_post_test}`
Method: `PUT`
JSON Body: 
```
{"name": "SecureX App - Modified", "description": "Updated Description", "primary": "true"}
```
After testing, refresh the "Segmentation" page in Tetration.  You should now see the application has been renamed to "SecureX App - Modified"

---
**Testing DELETE**
Path: `/openapi/v1/applications/{id_from_post_test}`
Method: `DELETE`

After testing, refresh the "Segmentation" page in Tetration.  The application workspace will have been deleted.