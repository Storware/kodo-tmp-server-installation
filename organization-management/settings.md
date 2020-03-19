# Settings

You can configure the following:

* Reporting
* User synchronization sources
* SMS notification

{% hint style="info" %}
For notification feature SMS gateway is required. Gateway needs to provide e-mail to SMS support.
{% endhint %}

To chance configuration select `Settings` from the top portal bar.

## GENERAL

### **USAGE & ACTIVITY REPORT**

This section allows you to configure reports about space usage and device activity

**Send reports every \(days\)**  
The interval with which the reports are sent

**Send reports to \(comma separated emails\)**  
The email addresses to which the report should been sent. Multiple email addresses need be separated using commas

**Send report time**  
Time of report sending

**Unprotected devices notification time**  
Time of SMS notification

Confirm the settings with the `SAVE CHANGES` button

### **MASTER PASSWORD**

Here you can set master password that is required to provide when erasing android device.

Type new password and save it by clicking on the `UPDATE PASSWORD` and confirm change with the `Ok` button.

## LDAP

The LDAP configuration are described in [Sync users with LDAP]() chapter.

## USER SYNCHRONIZATION

Here you can configure source of user synchronization from external source.

**Source type**  
Source of user synchronization:

* **NONE** - no synchronization with external
* **LDAP** - users synchronization with LDAP service
* **MDM** - user synchronization with MDM system

{% hint style="info" %}
KODO supports users synchronization only with KODO-EMM \(Famoc\) MDM system
{% endhint %}

**Auto synchronize users**  
User synchronization will be performed automatically according to configured frequency \(see below\)

**Auto synchronize groups**  
Users group synchronization will be performed automatically according to configured frequency \(see below\).  
_This feature is supported only when synchronizing with MDM system. Groups synchronization is NOT supported with LDAP._

**Auto synchronization frequency \(minutes\)**  
Every how many minutes automatic users and/or groups will be performed. 

## NOTIFICATIONS

Here you can configure support for users SMS notification about unprotected devices.

**SMS Notification Provider**  
Here you can choose communication way with your SMS gateway. At this moment only e-mail communication is supported.

**Phone number digits to include in notifcation address \(from end\)**  
Phone number is taken from user profile \(_TELEPHONE_ field\). Here you can configure how many digits, counting from the end, should be used to create e-mail message template. Use "0" digit to not trim phone numer at all. 

**Address Construction Template**  
User variables to build "from" template e-mail address that will be used to communicate with SMS gateway. You can use following variables:

* **%PHONE%** - user phone number will be used instead

Example:  
_Address Construction Template: %PHONE%@example-sms-gateway.com  
E-mail from address: 123420123@example-sms-gateway.com_

