# Defining the application

## Setting up an application to use API to access ReadCallRecording API Permission

1. API Permissions
    * ReadCallLog
    
| Permission                 | Description                        | Access-Type         | Included Permissions |
|----------------------------|------------------------------------|---------------------|----------------------|
| ReadCallRecording          | Downloading call recording content | Read Only           | ReadCallLog          |

NOTE: ReadCallRecording is an "Advanced" API Permission which needs to be added by RingCentral on the application. 

2. Software Architecture Considerations 
    
    * Third Party Storage Solutions 
    Some of the third party storage solutions whihc could be integrated to store the Call Recordings are :
     1. Amazon S3
     2. Dropbox
     3. Google Drive
     4. FTP/FTPS
     
    * Data Archiving
    The application logic could be set to acheive one of the following :
    1. Retrieve the CallRecordings on an ongoing continuous process as and when they are generated.
    2. Retrieve the CallRecordings at the end of a business day.
    3. Custom retrieval of CallRecordings ( by specifying dateFrom and dateTo ) 
    
3. Client-Facing Use Case Examples
    * Common Use Cases
    * Scenarios



