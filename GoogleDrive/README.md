# Google Drive
Reusable subroutines for dealing with Google Drive files in Qlik Sense.

## Methods
GoogleDrive.qvs contains the following methods (subroutines)  
<br />
### GoogleDrive.Init

**Description:** Initializes the Google Drive by setting the names of the Data Connection variables *GoogleDrive.DataConnection.Metadata* and *GoogleDrive.DataConnection.Storage* and caching the folders and files. Folders are cached in the Qlik table *\[GoogleDrive.Folders\]* and files are cached in the Qlik table *\[GoogleDrive.Files\]*. Run this before calling the other subroutines.  
<br />
|Parameter|Description|In/Out|Optional|
|--|--|--|--|
|pDataConnection.Metadata|Name of the Data Connection for the Google Drive metadata connector, without lib:// prefix.|In|No|
|pDataConnection.Storage|Name of the Data Connection for the Google Drive file connector, without lib:// prefix.|In|No|  

**Example:**

    CALL GoogleDrive.Init('Google Drive (metadata)', 'Google Drive (storage)');
<br />
### GoogleDrive.Destroy

**Description:** Removes all GoogleDrive artifacts (tables, variables, etc.). Run this at the end of your Qlik script.  
<br />
**Example:**

    CALL GoogleDrive.Destroy;
<br />
### GoogleDrive.ListFiles

**Description:** Queries the Google Drive metadata connector for a list of files matching the query provided by the *pQuery* parameter. Outputs the result to a Qlik table, for which the name is provided via the *pTableReturn* parameter.
<br />
|Parameter|Description|In/Out|Optional|
|--|--|--|--|
|pQuery|Query to run through the Google Drive metadata connector, see .... for syntax.|In|No|
|pTableReturn|Name of the table to place the results in.|Out|No| 

**Example:**

    CALL GoogleDrive.ListFiles('trashed = false and mimeType != "application/vnd.google-apps.folder"', 'GoogleDrive.Files.Temp');
<br />
