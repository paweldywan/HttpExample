# HttpExample - Azure Functions HTTP Trigger

A simple Azure Functions application demonstrating HTTP trigger functionality using .NET 8 and the Azure Functions Worker model.

## Overview

This project contains a basic Azure Function with an HTTP trigger that responds to both GET and POST requests. The function returns a welcome message and demonstrates the fundamentals of Azure Functions development with the isolated worker process model.

## Features

- **HTTP Trigger**: Accepts both GET and POST requests
- **Anonymous Authorization**: No authentication required for testing
- **Application Insights Integration**: Built-in telemetry and logging
- **ASP.NET Core Integration**: Leverages ASP.NET Core HTTP abstractions
- **.NET 8**: Built on the latest .NET framework

## Project Structure

```
├── HttpExample.cs          # Main function implementation
├── Program.cs              # Application host configuration
├── HttpExample.csproj      # Project file with dependencies
├── host.json              # Function app configuration
├── local.settings.json    # Local development settings
└── Properties/
    └── launchSettings.json # Debug launch configuration
```

## Prerequisites

- [.NET 8 SDK](https://dotnet.microsoft.com/download/dotnet/8.0)
- [Azure Functions Core Tools v4](https://docs.microsoft.com/en-us/azure/azure-functions/functions-run-local)
- [Visual Studio Code](https://code.visualstudio.com/) with [Azure Functions extension](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) (recommended)

## Getting Started

### Local Development

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd HttpExample
   ```

2. **Restore dependencies**
   ```bash
   dotnet restore
   ```

3. **Build the project**
   ```bash
   dotnet build
   ```

4. **Run the function locally**
   ```bash
   func start
   ```
   
   Or use the VS Code task: `func: 4`

5. **Test the function**
   
   Open your browser or use curl to test:
   ```bash
   # GET request
   curl http://localhost:7071/api/HttpExample
   
   # POST request
   curl -X POST http://localhost:7071/api/HttpExample
   ```

### Available Tasks

The project includes several VS Code tasks for common operations:

- `clean (functions)`: Clean build artifacts
- `build (functions)`: Build the project
- `publish (functions)`: Create a release build
- `func: 4`: Start the function host locally

## Function Details

### HttpExample Function

- **Route**: `/api/HttpExample`
- **Methods**: GET, POST
- **Authorization**: Anonymous
- **Response**: JSON object with welcome message

The function logs incoming requests and returns a simple welcome message, making it perfect for testing Azure Functions connectivity and deployment.

## Configuration

### Application Insights

The application is configured with Application Insights for monitoring and telemetry:

- Sampling is enabled for performance
- Request telemetry is excluded from sampling
- Live metrics filters are enabled

### Local Settings

Create a `local.settings.json` file for local development (this file should not be committed to source control):

```json
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "UseDevelopmentStorage=true",
    "FUNCTIONS_WORKER_RUNTIME": "dotnet-isolated"
  }
}
```

## Deployment

### Deploy to Azure

1. **Using Azure CLI**
   ```bash
   # Create a resource group
   az group create --name myResourceGroup --location eastus
   
   # Create a storage account
   az storage account create --name mystorageaccount --location eastus --resource-group myResourceGroup --sku Standard_LRS
   
   # Create a function app
   az functionapp create --resource-group myResourceGroup --consumption-plan-location eastus --runtime dotnet-isolated --functions-version 4 --name myFunctionApp --storage-account mystorageaccount
   
   # Deploy the function
   func azure functionapp publish myFunctionApp
   ```

2. **Using VS Code**
   - Install the Azure Functions extension
   - Use the command palette: `Azure Functions: Deploy to Function App...`
   - Follow the prompts to create or select existing Azure resources

## Dependencies

- **Microsoft.Azure.Functions.Worker** (2.0.0): Core worker functionality
- **Microsoft.Azure.Functions.Worker.Extensions.Http.AspNetCore** (2.0.2): ASP.NET Core HTTP integration
- **Microsoft.ApplicationInsights.WorkerService** (2.23.0): Application Insights telemetry

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test locally
5. Submit a pull request

## License

This project is provided as-is for educational and demonstration purposes.

## Resources

- [Azure Functions Documentation](https://docs.microsoft.com/en-us/azure/azure-functions/)
- [Azure Functions .NET Worker Guide](https://docs.microsoft.com/en-us/azure/azure-functions/dotnet-isolated-process-guide)
- [Azure Functions HTTP Triggers](https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-http-webhook)