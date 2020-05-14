# Google Drive
Reusable subroutines for dealing with Google Drive files in Qlik Sense.

## Methods
GoogleDrive.qvs contains the following methods (subroutines)  
<br />

### GoogleDrive.Init

**Description:** Initializes the Google Drive by setting the names of the Data Connection variables *GoogleDrive.DataConnection.Metadata* and *GoogleDrive.DataConnection.Storage* and caching the folders and files. Folders are cached in the Qlik table *\[GoogleDrive.Folders\]* and files are cached in the Qlik table *\[GoogleDrive.Files\]*. Run this before calling the other subroutines.  
<br />
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
<br />
|Parameter|Description|In/Out|Optional|
|--|--|--|--|
|pQuery|Query to run through the Google Drive metadata connector, see .... for syntax.|In|No|
|pTableReturn|Name of the table to place the results in.|Out|No| 

**Example:**

    CALL GoogleDrive.ListFiles('trashed = false', 'GoogleDrive.Files.Temp');
<br />

### GoogleDrive.GetFolderId

**Description:** Returns the Id used for a folder in Google Drive, based on the path provided in the parameter *pPath*. Places the result in the variable specified by the parameter *pVarReturn*. If no matching Id was found, the value NOT_FOUND is returned.
<br />
<br />
|Parameter|Description|In/Out|Optional|
|--|--|--|--|
|pPath|The path of the folder, without leading or trailing backslashes.|In|No|
|pVarReturn|The name of the variable to store the resulting Id in.|Out|No| 

**Example:**

    CALL GoogleDrive.GetFolderId('Qlik/QVD/Transformed', 'vFolderId');
<br />

### GoogleDrive.FolderExists

**Description:** Checks if a folder, identified by a path provided in the parameter *pPath*, exists and returns true (-1) or false (0) in the variable specified in the *pVarReturn* variable.
<br />
<br />
|Parameter|Description|In/Out|Optional|
|--|--|--|--|
|pPath|The path of the folder, without leading or trailing backslashes.|In|No|
|pVarReturn|The name of the variable to store the resulting Id in.|Out|No| 

**Example:**

    CALL GoogleDrive.FolderExists('Qlik/QVD/Transformed', 'vFolderExists');
<br />

### GoogleDrive.SetActiveFolder

**Description:** The 'Active Folder' contains the present working directory in Google Drive. Using this can be beneficial when reading/storing many different files from a single folder as the *GoogleDrive.ActiveFolder* properties offer additional metadata to work with the files in the folder.
<br />
The *GoogleDrive.SetActiveFolder* method lets you set and change the present working directory. 
<br />
<br />
|Parameter|Description|In/Out|Optional|
|--|--|--|--|
|pPath|The path of the folder to set the Active Folder to, without leading or trailing backslashes.|In|No|

**Example:**

    CALL GoogleDrive.SetActiveFolder('Qlik/QVD/Transformed');
<br />
