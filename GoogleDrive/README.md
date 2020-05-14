# Google Drive
Reusable subroutines for dealing with Google Drive files in Qlik Sense.

## Methods
GoogleDrive.qvs contains the following methods (subroutines)



**GoogleDrive.Init**

Initializes the Google Drive by caching the folders and files.


|Parameter|Description|In/Out|Optional|
|--|--|--|--|
|pDataConnection.Metadata|Name of the Data Connection for the Google Drive metadata connector, without lib:// prefix.|In|No|
|pDataConnection.Storage|Name of the Data Connection for the Google Drive file connector, without lib:// prefix.|In|No|


Example:

    CALL GoogleDrive.Init('Google Drive (metadata)', 'Google Drive (storage)');
