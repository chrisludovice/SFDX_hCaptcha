# SFDX_hCaptcha
## This project shows how to simply integrate HCaptcha into your Salesforce using VFP and Aura Frameworks.

###### Important Note: This only contains my version of solution but may not be the most efficient one; and might still be subject for refactoring. 

*******************************************************************************************************
**Step 1: Sign up on hCaptcha.com** <br/>
  a. You’ll need your "site key" and "secret" to proceed.

**Step 2: Salesforce Security Configuration** <br/>
  a. Whitelist https://hcaptcha.com/siteverify from your Remote Site Setting. <br/>
  b. LEX:Include assets.hcaptcha.com in your CSP Trusted Sites.

**Step 3: Add the JavaScript Library** <br/>
  a. LEX: Include the hCaptcha js as a static resource. JS can't be directly added in cmp. <br/>
  b. VFP: Simply add the js library to your vf page.<br/>

 ```ruby
 <script src='https://www.hCaptcha.com/1/api.js' async defer></script>
  ```
Note: *Since js is not automatically served from hcaptcha's server in LEX, it's your job to update it periodically.* :shipit:

**Step 4: Add the hCaptcha Widget to your component / vfp** <br/>
 a. Add this HTML code where you want to show the hCaptcha button, for example inside a case form. <br/>
 b. Remember to replace "your_site_key" with your actual site key. <br/>
 
 ```ruby
 <div class="h-captcha" data-sitekey="your_site_key"></div>"
 ```
 
**Step 5: Validate the result on your backend server.** <br/>
 a. When the captcha succeeded, the hCaptcha script inserted a token into your form data.
  ```ruby
 <textarea name="h-captcha-response">CLIENT-RESPONSE</textarea>
  ```
 b. Get the value of "h-captcha-response" during form submission using simple js hack. You'll need this token as one of the parameters in your API request.
  ```ruby
  document.getElementsByName("h-captcha-response")[0].value;
   ```
 c. Check the token is indeed valid, you must now verify it at the API endpoint:
  ```ruby
 curl -d "response=CLIENT-RESPONSE&secret=YOUR-SECRET" -X POST https://hcaptcha.com/siteverify
  ```
  d. Open the apex cls file to check the exact POST request code version for VFP and Aura controllers. <br/>
  
  *******************************************************************************************************
  **Reference:** <br/>
  API docs: https://docs.hcaptcha.com/ 
