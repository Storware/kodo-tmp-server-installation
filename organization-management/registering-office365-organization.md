---
description: >-
  To use Office365 backup you need first register KODO4Cloud application and
  give it the permissions to your organizatio. To do this follow the
  instructions below.
---

# Registering Office365 organization

Log in to Windows system and as an administrator run PowerShell script provided by your KODO Server administrator:

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD_wiewObVYm0U3i0bU%2F-LD_wr1aLtkk1zA9JxEv%2F-LD_x1nW0q85XN08K3zH%2Fo3_ps.png?generation=1527497634283674&alt=media)

**NOTE: Script requires AzureAD module to run, if not present, module will be installed by script.**

You will need to login to your Office365 organization, use administrative account to sign in to your O365 organization.

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD_wiewObVYm0U3i0bU%2F-LD_wr1aLtkk1zA9JxEv%2F-LD_x1nah4b9VC3Tn7_L%2Fo5_ad.png?generation=1527497634297795&alt=media)

Script will register KODO4Cloud application and display necessary information that you will need to provide in your KODO organization configuration:

* **Tenat ID**
* **Client ID**
* **Client Secret**
* **Certificate for Exchnage Online connection**

**You will also find all those values in KodoADSettings.json file that is created by script in location from where you run script. You will need this file to copy those values to KODO configuration in next step.**

After successful application registration you need to log in to your Azure portal \([https://portal.azure.com](https://portal.azure.com/)\) and set up correct permissions for KODO4Cloud app.

1. Select "Azure Active Directory" item from the main menu
2. Next go to the "App registration"
3. Click on the application name
4. Click on "All settings-&gt;"
5. Select "Required Permissions"
6. Click on "Grant Permissions"

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD_wiewObVYm0U3i0bU%2F-LD_wr1aLtkk1zA9JxEv%2F-LD_x1nlKSlM5hysJv3p%2Fo3_grant.png?generation=1527497634294686&alt=media)

Login to your KODO organization as administrator:

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD_wiewObVYm0U3i0bU%2F-LD_wr1aLtkk1zA9JxEv%2F-LD_x1fFLlkyx3ZCxqd9%2Fo3_login.png?generation=1527497633739134&alt=media)

Select "Services" item from left main menu:

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD_wiewObVYm0U3i0bU%2F-LD_wr1aLtkk1zA9JxEv%2F-LD_x1fImg4WIIvkIxmv%2Fo3_services.png?generation=1527497633689355&alt=media)

**NOTE: If the "Services" item is not present, please contact with main server administrator to enable it for your organization.**

**NOTE: To use Office365 Backup feature you need first register KODO users accounts that must have the same email address that is associated with their ONEDRIVE service.**

The information about available services will be displayed, click the gear icon to configure it.

Select "Configuration" tab and click "+ ADD CONFIGURATION" button:

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD_wiewObVYm0U3i0bU%2F-LD_wr1aLtkk1zA9JxEv%2F-LD_x1f_2duDVBCYR4-t%2Fo3_conf.png?generation=1527497633713839&alt=media)

Open **KodoADSettings.json** file generated in previous step. Provide name for the service \(any\) and information from json file:

* **Tenat ID**
* **Client ID**
* **Client Secret**
* **Certificate Password**
* **Private Certificate**

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD_wiewObVYm0U3i0bU%2F-LPzTnpJWlaJrQ1bDEam%2F-LPzU4JiAR4RbAWquS6h%2Fservice.png?alt=media&token=28929536-9773-426d-be15-3a43f08eff35)

Click "SAVE" button.

Now you can import or synchronize user from O365 organization:

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD_wiewObVYm0U3i0bU%2F-LPzUcdW_VFduK-qO3xf%2F-LPzVDUcXlzJ8b-YN-cm%2Fusers_o365.png?alt=media&token=1adb7351-3420-4970-a53c-7abf41448913)

Usage:

* IMPORT USERS - to import and created new user from you O365 organization
* SYNCHRONIZE USERS - to synchronize existing users with you O365 organization

Select "Schedules" tab and click "+ ADD SCHEDULE" follow instruction from wizard to create new backup scheduler.

Click "SAVE" button.

