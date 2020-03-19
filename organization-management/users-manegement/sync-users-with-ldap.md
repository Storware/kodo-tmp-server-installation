# Sync users with LDAP

## Configure connection with LDAP server

1. **Go to settings by clicking Settings in the top right corner**
2. **Go to the LDAP tab**

Provide necessary information:

* **Server URL -** directory server ip or domain address
* **Login -** username used for synchronization
* **Domain Prefix -** domain name if different than default
* **Search -** directory service search filter
* **Group filter -** LDAP group filter

{% hint style="warning" %}
If you configuring synchronization for the first time before clicking the  `SAVE CHANGES` button fill the LDAP user password fields.
{% endhint %}

Click on the `SAVE CHANGES` button to confirm your changes.

## Configure users synchronization source

1. **Go to settings by clicking on Settings in the top right corner**
2. **Go to tab** `USERS SYNCHRONIZATION`
3. **As source of synchronization \(**`Source type` **\) select:** `LDAP`
4. **Set the Synchronize users switch to On**
5. **Set how many minutes KODO has to synchronize with the LDAP server  \(0 - no synchronization\)**
6. **Save changes with** `SAVE CHANGES`



