



Custom Bulk User Import Tool Documentation

This tool is designed to facilitate the bulk import of users into WSO2 Identity Server 7.0. It supports both primary and secondary user stores and can be used in the super tenant or any other tenant. The tool reads user data from CSV files and imports users into the configured user store. It also generates two CSV files: one for successful user imports and another for failed user imports.

Note: Before using this tool in production, test it with a small set of users in a lower environment to ensure the complete flow works as expected.  We have locally tested with one million users, and the time taken was 3,697,311 ms is approximately 61.62 minutes. However, this depends on the environment, so it is recommended to execute the process in a lower environment first to assess performance before running it in production.


1. Configuration Properties File

1.1. File Location
- Place the bulk.user.properties file in the following directory:
  <IS_HOME>/repository/conf/


1.2. File Content
The config.properties file should contain the following key-value pairs:


 ```Tenant and User Domain Configuration
tenantDomain=carbon.super
userDomain=SECONDARY


 File Paths
csvFilePath=./repository/resources/identity/users/
PropertiesFilePath=./repository/conf/
outputDirectory=./repository/conf/
```


Explanation:
Configurations can be changed according to your requirements.
- tenantDomain: Specifies the tenant domain (e.g., carbon.super for the super tenant and configure any other specific tenant).
- userDomain: Specifies the user store (e.g., SECONDARY for a secondary user store). Leave this blank for the primary user store. No need to specifically configure this for the primary user store.
- csvFilePath: Specifies the directory where the input CSV files are located.
- PropertiesFilePath: Specifies the directory where the config.properties file is located.
- outputDirectory: Specifies the directory where the successful_users.csv and failed_users.csv files will be saved.


2. CSV File Configuration


2.1. CSV File Location
- Place the CSV file in the directory specified by the csvFilePath property in the bulk.user.properties file:


2.2. CSV File Format
The CSV file should follow the format below:


```UserName,Password,<claim_url_01>,<claim_url_02>,...,<claim_url_n>
<username_value>,<password_value>,<claim_value_01>,<claim_value_02>,...,<claim_value_n>
```


Example CSV File:

```
UserName,Password,http://wso2.org/claims/givenname,http://wso2.org/claims/lastname,http://wso2.org/claims/customClaim1
user1,password,John,Doe,value1
user2,password,Jane,Smith,value2
```


3. Deploying the Custom OSGi Component


3.1. Copy the JAR File
- Copy the org.wso2.carbon.custom.bulk.user.migration-2.0.jar file into the following directory:
  <IS_HOME>/repository/components/dropins


3.2. Start the Server
- Start the WSO2 Identity Server with the following command to trigger the bulk user import:
  sh wso2server.sh -Dbulkupload=true


4. Successful and Failed User CSV Files


During the bulk user import process, the tool generates two CSV files:
- successful_users.csv: Contains the list of users that were successfully imported.
- failed_users.csv: Contains the list of users that failed to import.


These files are saved in the directory specified by the outputDirectory property in the bulk.user.properties file. 









