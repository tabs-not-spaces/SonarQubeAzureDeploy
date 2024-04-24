# SonarQubeAzureDeploy
Deploy a SonarQube instance to Azure Container instances connected to Azure SQL &amp; Azure Storage

## Parameters
- `location`: The location of the resource group.
- `storageAccountName`: The name of the storage account to use as the persistence layer.
- `containerInstanceName`: The name of the container instance to use as the SonarQube host.
- `dnsName`: The DNS name for the SonarQube instance.
- `sqlServerName`: The name of the Azure SQL Server to use for SonarQube.
- `sqlDatabaseName`: The name of the Azure SQL Database to use for SonarQube.
- `sqlServerAdminLogin`: The database login for SonarQube.
- `sqlServerAdminPassword`: The database password for SonarQube.

## Resources
### 1. Storage Account
The storage account is used to persist the data for the SonarQube instance. 

### 2. SQL Server & Database
The Azure SQL Server and Database are used to store the SonarQube data.

### 3. Container Instances
This deployment contains two container instances. 
- `sonarqube-container`: This container runs the SonarQube instance. It should be configured to connect to the SQL database and the storage account. It also has volume mounts for configuration, data, logs, and extensions.
- `caddy-container`: This container runs the Caddy web server. It is used to provide HTTPS termination for the SonarQube instance. It is also configured with a volume mount for the Caddy data.

## Deployment
To deploy this template, click the button below:


[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FJcardif%2FSonarQubeAzureDeploy%2Fmain%2Fsrc%2Fmain.json)


