# Google Drive
Reusable subroutines for dealing with Google Drive files in Qlik Sense.

## Methods
GoogleDrive.qvs contains the following methods (subroutines)  
<br />
<br />
### GoogleDrive.Init

**Description:** Initializes the Google Drive by caching the folders and files. Folders are cached in the Qlik table \[GoogleDrive.Folders\] and files are cached in the Qlik table \[GoogleDrive.Files\]. Run this before calling the other subroutines.  
<br />
|Parameter|Description|In/Out|Optional|
|--|--|--|--|
|pDataConnection.Metadata|Name of the Data Connection for the Google Drive metadata connector, without lib:// prefix.|In|No|
|pDataConnection.Storage|Name of the Data Connection for the Google Drive file connector, without lib:// prefix.|In|No|  

**Example:**

    CALL GoogleDrive.Init('Google Drive (metadata)', 'Google Drive (storage)');

### GoogleDrive.Destroy

**Description:** Removes all GoogleDrive artifacts (tables, variables, etc.). Run this at the end of your Qlik script.  

**Example:**

    CALL GoogleDrive.Destroy;
