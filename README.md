
# oci_waf_postman_collection

**OCI WAF Advanced features with Postman (REST APIs)**

**OCI WAF Advanced features with Postman (REST APIs)**

You can create and manage your OCI WAF Policies using the OCI web console, the CLI (Command Line Interface) or using REST APIs.

This document will guides you through the configuration and usage of Postman (as a REST Client) to demonstrate how you can easily manage your WAF policies and enable OCI WAF advanced features (Device Fingerprint Challenge, Human Interaction Challenge , Address rate limit , Threat feeds, etc. )

***Prerequisites:***
	A computer with outbound Internet access.
	OCI WAF Policy in ‘Available' state
	OCI API Keys and OCIDs mentioned below : https://docs.cloud.oracle.com/iaas/Content/API/Concepts/apisigningkey.htm 
o	RSA key pair in PEM format. 
o	Fingerprint of the public key.
o	Tenancy's OCID
o	User's OCID. 
	WAF policy OCID.

 

## 1- Step 1: Install Postman


1.1	- Download and Install Postman from its official location: https://www.getpostman.com/ on a computer with outbound internet access. 
1.2	- Launch Postman.
1.3	- Skip the user registration by clicking on the bottom link (‘Skip signing in…’) if you do not want to create a Postman account.

**Note:** You can review the benefits of creating a Postman account through Official Postman documentation.

1.4	– Close the ‘Create New’ Wizard. 
 
## 2-   Step 2: Import OCI WAF sample collection and environment templates 

2.1 – Open a web browser and download OCI WAF Collection and environment templates from the following location: 
https://github.com/BaptisS/oci_waf_postman_collection

2.2 – Extract the zip content to a local folder on your computer. 
Postman collection contains OCI WAF queries examples targeting a specific regional WAF API endpoint (‘waas.uk-london-1.oraclecloud.com’).

You can manage your OCI WAF Policies by using any WAF API endpoints available. We recommend using the one closest to your location to avoid latency issues.  

You can find more detailed information about OCI API endpoints here : https://docs.cloud.oracle.com/iaas/Content/API/Concepts/apiref.htm

If you are interested in using a different API endpoint than the default one (UK), open the sample collection file (‘OCI_WAF_REST_UK_COLLECTION_v4.postman_collection’) with your preferred text editor and ‘replace-all’ occurrences of ‘uk-london-1’ by the desired one : 

•	https://waas.ap-mumbai-1.oraclecloud.com
•	https://waas.ap-seoul-1.oraclecloud.com
•	https://waas.ap-sydney-1.oraclecloud.com
•	https://waas.ap-tokyo-1.oraclecloud.com
•	https://waas.ca-toronto-1.oraclecloud.com
•	https://waas.eu-frankfurt-1.oraclecloud.com
•	https://waas.eu-zurich-1.oraclecloud.com
•	https://waas.sa-saopaulo-1.oraclecloud.com
•	https://waas.uk-london-1.oraclecloud.com
•	https://waas.us-ashburn-1.oraclecloud.com
•	https://waas.us-phoenix-1.oraclecloud.com

Save the updated collection file. 

2.3 – In Postman’s ‘My Workspace’ Interface click on the “Import” button. (top left)
 
2.4 – Select ‘Import Folder’ tab, Click ‘Choose Folders’ button and select the folder containing the extracted templates.  
 
2.5 – Click ‘Import’ and ensure both the environment and the two collections have been imported successfully.  
 
## 3-   Step 3: Define your environment variables
 
3.1 – In the top right of the Postman Interface, Click on the ‘Manage Environments’ button (wheel). 
 
3.2 – Click the newly imported ‘OCI_Environment’ 
 
3.3- Review and complete the followings environment variables (Current Value) : 
 
- TenancyId:  Your Tenancy OCID (ocid1.tenancy.oc1..aaaaxxx123)
- AuthUserId : Your User OCI (ocid1.user.oc1.aaaaxxxxxx789)
- KeyFingerprint : Your OCI API Public key Fingerprint (xx:xx:xx)
- PrivateKey : Your OCI API Private key (---BEGIN RSA PRIVATE KEY----MIIEoxxxx)
- Apiversion : Keep the predefined value (‘20181116’)
- WafPolicy01: Your OCI WAF OCID (ocid1.waaspolicy.oc1..aaaxxx)

3.4 – Click ‘Update’ button and Close the ‘Manage Environments’ window. 

 
## 4- Step 4: INITIALIZE Postman for OCI

4.1 – In the Left pane, Select the “Collections” tab then expand the OCI_REST_INITIALIZATION Collection. 
 
4.2 – Select the ‘ONE_TIME_INITIALIZATION_CALL’ Query and click on the ‘Send’ button. 
 
4.3 – Review the response returned. Ensure initialization succeeded. (Status: 200 OK). 
 

If you get anything different than ‘Status: 200 OK’, ensure there is no local or intermediate firewall preventing outbound HTTPS connectivity from Postman client computer to public Internet endpoints. 

***IMPORTANT NOTE :***
Your REST client is now configured to manage your OCI WAF resources with the level of permissions assigned to the OCI user account referred in the environment variable. (OCI_Environment)
**DO NOT SEND A PUT REQUEST** before verifying the Request’s Body parameters otherwise you could unexpectedly modify your WAF configuration. 

Templates provided contains HTTP body’s parameters examples. 
You should review and modify them before executing any PUT request.  

 
## 5-    Step 5: Execute your first API query (GET WAF POLICY CONFIG) 

5.1 – In the Left pane, Select the “Collections” tab then expand the OCI_WAF_REST_XX_COLLECTION and select the ‘WAF_POLICY_GET’ query. 
 
5.2 – In the Top Right section, ensure the ‘OCI_Environment’ is selected.
 
5.3 - Click the ‘Send’ button 
 
5.4 – Review the Status Code returned and associated outputs. (In the Center lower pane) 
 
 
## 6-    Step 6: Modify WAF Configuration (PUT WAF POLICY CONFIG)

***IMPORTANT NOTE:***
**DO NOT SEND A PUT REQUEST** before checking the Request’s Body parameters otherwise you could unexpectedly modify your WAF configuration. Templates provided contains HTTP body’s parameters examples. You should review and modify them before executing any PUT request.  

6.1 – In the Left pane, Select the “Collections” tab then expand the OCI_WAF_REST_XX_COLLECTION and select the ‘WAF_POLICY_PUT’ query. 
6.2	– In the middle pane select the ‘Body’ tab and review the proposed configuration example. 
 
The proposed example can be used to modify your WAF policy Configuration as per below :
 
**-AddressRateLimiting:** Enabled (default is disabled) – 1 Request per IP per Second max allowed. 

**-Device Fingerprint Challenge:** Enabled (default is disabled) – Redirect to CAPTCHA if client fails the DFC Challenge more than 5 times in 60 seconds. / 10 Different source IPs allowed max for the same DF in 60seconds period. 

**-Human Interaction Challenge:** Enabled (default is disabled) – Redirect to CAPTCHA if client fails the HIC Challenge more than 5 times in 60 seconds. / WAF needs to catch 3 human interactions in a 10 seconds recording period. 

**-JavaScript Challenge:** Enabled (default is disabled) – Redirect to CAPTCHA if client fails the JSC Challenge more than 5 consecutive times. 

**-Protection Settings:** Default settings. Block Error Page display a custom message. 

**-Protection Rules:**   Set of common protection rules enabled in Block Mode. 

6.3 – After reviewing/updating if needed the Request’s body parameters, you can SEND the PUT request to update your WAF Policy. (Click only once)
 
6.3	– In the middle lower pane, review the Status returned. 
‘202 Accepted’ means the PUT request has been accepted by the OCI WAF. (Postman may briefly show up an error window. You can ignore it.)
 
6.5 – Select the ‘Headers’ tab and review the Response headers. 
 
6.5 – Copy the ‘Opc-Work-Request-id’ OCID.
6.6 – While the OCI WAF configuration is asynchronously updated across all OCI WAF endpoint worldwide (It can take up to 10min in some cases), you can check the status of the update by invoking the WAF_WORKREQUEST_GET request. 
 
6.7 – Paste the ‘Opc-Work-Request-id’ OCID in the url section as per below : 
 
6.8 – Click ‘Send’ button and review the response output : 
 
(…)
 

 

 

## Links & References: 

**General:** 
-	Invoking OCI REST APIs using POSTMAN: https://www.ateam-oracle.com/invoking-oci-rest-apis-using-postman
-	Oracle Cloud Infrastructure (OCI) REST call walkthrough with curl: https://www.ateam-oracle.com/oracle-cloud-infrastructure-oci-rest-call-walkthrough-with-curl 
-	OCI WAF API References: https://docs.cloud.oracle.com/iaas/api/#/en/waas/latest/

**OCI WAF Access Control:**
	https://docs.cloud.oracle.com/iaas/Content/WAF/Tasks/wafaccesscontrol.htm
	https://docs.cloud.oracle.com/iaas/api/#/en/waas/20181116/AccessRule/ 

**OCI WAF Address Rate Limit:**
The number of allowed requests per second from one IP address. If unspecified, defaults to "1”.
https://docs.cloud.oracle.com/iaas/api/#/en/waas/20181116/AddressRateLimiting/

**OCI WAF Device Fingerprint Challenge (BotManagement):** 
The DFC generates a hashed signature of both virtual and real browsers based on 50+ attributes. These proprietary signatures are then leveraged for real-time correlation to identify and block malicious bots.
The signature is based on a library of attributes detected via JavaScript listeners; the attributes include OS, screen resolution, fonts, UserAgent, IP address, etc. We are constantly making improvements and considering new libraries to include in our DFC build. We can also exclude attributes from the signature as needed.
DFC collects attributes to generate a hashed signature about a client – if a fingerprint is not possible, then it will result in a block or alert action. Actions can be enforced across multiple devices if they share they have the same fingerprint.
https://docs.cloud.oracle.com/iaas/api/#/en/waas/20181116/DeviceFingerprintChallenge/

**OCI WAF Human Interaction Challenge (BotManagement):** 
HIC is a countermeasure that allows the proxy to check the user's browser for various behaviors that distinguish a human presence from a bot.
At high level HIC first allows the client request then start recording any potential user activity such as ‘mouse move, scroll, button click, etc.’ 
https://docs.cloud.oracle.com/iaas/api/#/en/waas/20181116/HumanInteractionChallenge/

**OCI WAF JavaScript Challenge (BotManagement) :** 
JavaScript Challenge is the function to filter abnormal or malicious bots and allow access to real clients. JavaScript Challenge validates that the client can accept JavaScript with a binary decision
	https://docs.cloud.oracle.com/iaas/Content/WAF/Tasks/botmanagement.htm
	https://docs.cloud.oracle.com/iaas/api/#/en/waas/20181116/JsChallenge/

**OCI WAF Protection Rules:**
Protection rules match web traffic to rule conditions and determine the action to be taken when the conditions are met.
	https://docs.cloud.oracle.com/iaas/Content/WAF/Tasks/wafprotectionrules.htm
	https://docs.cloud.oracle.com/iaas/Content/WAF/Reference/protectionruleids.htm
	https://docs.cloud.oracle.com/iaas/api/#/en/waas/20181116/ProtectionRule/

**OCI WAF Threat Feeds Intelligence:**
You can block requests from IP addresses based on their reputations with various commercial and open source threat feeds.
	https://docs.cloud.oracle.com/iaas/Content/WAF/Tasks/threatintel.htm
	https://docs.cloud.oracle.com/iaas/api/#/en/waas/20181116/ThreatFeed/




