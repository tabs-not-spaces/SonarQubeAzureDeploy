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
- `dockerHubUsername`: Docker Hub username for container image authentication.
- `dockerHubPassword`: Docker Hub password or access token for container image authentication.

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
### Prerequisites
- Docker Hub account with username and password/access token
- Azure subscription and resource group

### Using Azure CLI
```bash
az deployment group create \
  --resource-group <your-resource-group> \
  --template-file src/main.bicep \
  --parameters sqlServerAdminPassword="<your-secure-password>" \
               dockerHubUsername="<your-dockerhub-username>" \
               dockerHubPassword="<your-dockerhub-password>"
```

### Using Azure Portal
To deploy this template using the Azure Portal, you have several options:

#### Option 1: One-Click Deploy Button
Click the button below to deploy directly from GitHub:

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Ftabs-not-spaces%2FSonarQubeAzureDeploy%2Fmain%2Fsrc%2Fmain.json)

This will:
1. Open the Azure Portal with the template pre-loaded
2. Prompt you to select a subscription and resource group
3. Ask you to provide the required parameters (including Docker Hub credentials)
4. Deploy the resources

#### Option 2: Custom Deployment
1. Go to [Azure Portal](https://portal.azure.com)
2. Search for "Deploy a custom template" or navigate to **Create a resource** → **Template deployment (deploy using custom templates)**
3. Click **Build your own template in the editor**
4. Either:
   - **Upload file**: Select the `main.json` file from this repository
   - **Load file**: Copy and paste the contents of `main.bicep`
5. Click **Save**
6. Fill in the required parameters:
   - **sqlServerAdminPassword**: A secure password for your SQL Server (min 8 chars, must include uppercase, lowercase, number, and special character)
   - **dockerHubUsername**: Your Docker Hub username
   - **dockerHubPassword**: Your Docker Hub password or access token (recommended)
7. Select your subscription and resource group
8. Click **Review + create** then **Create**

#### Option 3: Upload Template and Parameters
1. Update the `main.parameters.json` file with your actual values
2. Follow steps 1-4 from Option 2
3. Click **Load parameters file** and upload your updated `main.parameters.json`
4. Review the auto-filled parameters and proceed with deployment

**Note:** Due to Docker Hub's authentication requirements, you must provide valid Docker Hub credentials during deployment. You can use either your Docker Hub password or an access token (recommended for better security).

### Parameters File
You can also use the provided `main.parameters.json` file as a template:
```bash
az deployment group create \
  --resource-group <your-resource-group> \
  --template-file src/main.bicep \
  --parameters @src/main.parameters.json
```

Make sure to update the parameter values in the file before deployment.

### Secure Deployment with Azure Key Vault (Recommended)
For production deployments, it's recommended to store sensitive information like passwords in Azure Key Vault. Use the `main.parameters.keyvault.json` template:

```bash
az deployment group create \
  --resource-group <your-resource-group> \
  --template-file src/main.bicep \
  --parameters @src/main.parameters.keyvault.json
```

Before using this approach:
1. Create an Azure Key Vault
2. Store your SQL admin password and Docker Hub password as secrets
3. Update the Key Vault ID in the parameters file
4. Ensure your deployment principal has access to the Key Vault


