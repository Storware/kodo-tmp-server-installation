---
description: >-
  WINDOWS and OSX tabs allow you to configure setting and Continuous Data
  Protection for Windows and OS X/MacOS based desktops.
---

# Windows ans MacOS protection

## **BASIC SETTINGS** <a id="basic-settings"></a>

**Deduplication**   
Turn on or off client-side data deduplication

**Deduplication cache patch**   
Path to temporary folder used in data deduplication process

**Deduplication cache size \[MB\]**   
Max. cache size for data deduplication process

**Compression**   
Turn on or off client-side data compression

**Max sessions**   
Max. number of sessions used to send data to the server

**Max file size \[MB\]**   
Max. protected file size, files larger than the specified size will be omitted during backup process

**Total Data Warning Threshold \[%\]**   
Percent of the total data quota, exceeding which will result in user notification.

**Total Data Quota \[GB\]**   
Max. amount of space that can by used by device backup copy

**Kodo temporary folder**   
Path to folder used by KODO for temporary backup space

## **INCLUDE BACKUP SETTINGS**  <a id="include-backup-settings"></a>

A section defining which paths and file extensions should be protected in continuous data protection mode.

**DIRECTORY**   
Path to be protected in CDP mode

**EXTENSIONS**   
Extensions of files that should be protected in selected path

**RETENTION**   
Data retention for protected data, how long backup data should be kept on KODO server

## **EXCLUDE BACKUP SETTINGS**  <a id="exclude-backup-settings"></a>

A section defining which paths and file extensions should be excluded from data protection process.

{% hint style="info" %}
Exclude settings are stronger than include
{% endhint %}

The list is displayed as a table with the following columns:

**DIRECTORY**   
Path to be excluded from protection

**EXTENSIONS**   
File extensions to be excluded from protection

