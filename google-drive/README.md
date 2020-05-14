![alt text](https://github.com/bitmetric-bv/qlik-sense-web-connectors/blob/master/google-drive/google-drive-logo.png "Reusable subroutines for dealing with Google Drive files and folders in Qlik Sense, using the Google Drive Connectors in Qlik.")
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
The *GoogleDrive.SetActiveFolder* method lets you set and change the present working directory. This populates the Qlik table *\[GoogleDrive.ActiveFolder.Files\]* with a list of the files in the active folder. Additionally, it sets the following properties:

* *GoogleDrive.ActiveFolder.IsSet* : True (-1) when an active folder is set, or false (0) when it is not.
* *GoogleDrive.ActiveFolder.Id* : The Id for the active folder in Google Drive.
* *GoogleDrive.ActiveFolder.Path* : The (human-readable) path of the active folder.
* *GoogleDrive.ActiveFolder.NoOfFiles* : The number of files in the active folder, can be used to loop through the files in the folder.
<br />
<br />

|Parameter|Description|In/Out|Optional|
|--|--|--|--|
|pPath|The path of the folder to set the Active Folder to, without leading or trailing backslashes.|In|No|

**Example:**

    CALL GoogleDrive.SetActiveFolder('Qlik/QVD/Transformed');
<br />

### GoogleDrive.ActiveFolder.GetFilenameByIndex

**Description:** Returns the name of the file in the place identified by the *pIndex* parameter. Outputs the name to the variable identified by the *pVarReturn* parameter.
<br />
<br />
|Parameter|Description|In/Out|Optional|
|--|--|--|--|
|pIndex|Index of the file in the folder. Possible values range from zero to *GoogleDrive.ActiveFolder.NoOfFiles - 1* |In|No|
|pVarReturn|Name of the variable where the name of the file is output to|Out|No|

**Example:**

    FOR i = 0 TO $(GoogleDrive.ActiveFolder.NoOfFiles) - 1
    
        CALL ActiveFolder.GetFilenameByIndex($(i), 'vFilename');
        
        Data:
        LOAD
            *
        FROM [lib://$(GoogleDrive.DataConnection.Storage)/$(GoogleDrive.ActiveFolder.Id)/$(vFilename)]
        (qvd)
        ;
        
    NEXT
<br />

### GoogleDrive.GetFileId

**Description:** Returns the Id for the file specified by the *pFilename* parameter, located in the folder specified by the *pPath* parameter. The result is placed in the variable specified in the *pVarReturn* parameter. If no matching Id was found, the value NOT_FOUND is returned.
<br />
<br />
|Parameter|Description|In/Out|Optional|
|--|--|--|--|
|pPath|The path of the folder, without leading or trailing backslashes.|In|No|
|pFilename|The filename for which to get the Id.|In|No|
|pVarReturn|The name of the variable to store the resulting Id in.|Out|No| 

**Example:**

    CALL GoogleDrive.GetFileId('Qlik/QVD/Transformed', 'Sales.qvd', 'vFolderId');
<br />

### GoogleDrive.FileExists

**Description:** Checks if the file specified by the *pFilename* parameter and located in the folder specified by the *pPath* parameter exists. Outputs the result -1 (true) or 0 (false) to the variable specified by the *pVarReturn* parameter.
<br />
<br />
|Parameter|Description|In/Out|Optional|
|--|--|--|--|
|pPath|The path of the folder, without leading or trailing backslashes.|In|No|
|pFilename|The filename to check exists.|In|No|
|pVarReturn|The name of the variable to store the result in, 0 for false or -1 for true.|Out|No| 

**Example:**

    CALL GoogleDrive.FileExists('Qlik/QVD/Transformed', 'Sales.qvd', 'vFolderId');
<br />

### GoogleDrive.GetFilesInPath

**Description:** Returns a table (specified by the *pTableReturn* parameter) containing the Id, Filename, FileExtension and FileSize for all files located in the folder specified by the *pPath* parameter.
<br />
<br />
|Parameter|Description|In/Out|Optional|
|--|--|--|--|
|pPath|The path of the folder, without leading or trailing backslashes.|In|No|
|pTableReturn|The name of the table to store the file list in.|Out|No| 

**Example:**

    CALL GoogleDrive.GetFilesInPath('Qlik/QVD/Transformed', 'TableTransformedFiles');
<br />

### GoogleDrive.StoreQVD

**Description:** Stores the table specified by the *pTable* parameter to the folder (specified by *pPath*) and filename (specified by *pFilename*). Optionally can drop the table after the file is stored, by providing the *pDropAfterStore* parameter.
<br />
<br />
|Parameter|Description|In/Out|Optional|
|--|--|--|--|
|pTable|The name of the Qlik table.|In|No|
|pPath|The path of the output folder, without leading or trailing backslashes.|In|No|
|pFilename|The name of the output file, including extension.|In|No| 
|pDropAfterStore|Boolean indicating if the table should be dropped after storing. -1 for true, 0 for false.|In|Yes| 

**Example:**

    CALL GoogleDrive.StoreQVD('Sales', Qlik/QVD/Transformed', 'Sales.qvd', -1);  // Store and drop table
    CALL GoogleDrive.StoreQVD('Sales', Qlik/QVD/Transformed', 'Sales.qvd');      // Store and keep table
    CALL GoogleDrive.StoreQVD('Sales', Qlik/QVD/Transformed', 'Sales.qvd', 0);   // Store and keep table
<br />
