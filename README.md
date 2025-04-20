Prerequisites
An Azure subscription - Create one for free.
Node.js LTS
Ensure that the individual deploying the template has the Azure AI Developer role assigned at the resource group level where the template is being deployed.
Additionally, to deploy the template, you need to have the preset Role Based Access Administrator role at the subscription level.
The Owner role at the subscription level satisfies this requirement.
The specific admin role that is needed is Microsoft.Authorization/roleAssignments/write
Ensure that each team member who wants to use the Agent Playground or Agent SDK to create or edit agents has been assigned the built-in Azure AI Developer RBAC role for the project.
Note: assign these roles after the template has been deployed
The minimum set of permissions required is: agents/*/read, agents/*/action, agents/*/delete
Install the Azure CLI and the machine learning extension. If you have the CLI already installed, make sure it's updated to the latest version.
Set up your Azure AI Hub and Agent project
The following section shows you how to set up the required resources for getting started with Azure AI Agent Service:

Creating an Azure AI Hub to set up your app environment and Azure resources.

Creating an Azure AI project under your Hub creates an endpoint for your app to call, and sets up app services to access to resources in your tenant.

Connecting an Azure OpenAI resource or an Azure AI Services resource

Choose Basic or Standard Agent Setup
Basic Setup: Agents use multitenant search and storage resources fully managed by Microsoft. You don't have visibility or control over these underlying Azure resources.

Standard Setup: Agents use customer-owned, single-tenant search and storage resources. With this setup, you have full control and visibility over these resources, but you incur costs based on your usage.

 Note

The Standard Agent Setup now supports Bring your own (BYO) Thread Storage using an Azure Cosmos DB for NoSQL account. This feature lets you store all messages and conversation history in your own Azure Cosmos DB for NoSQL account. You can use the following automated bicep templates to deploy either a Standard or Basic agent project. You can also create a basic project using the Azure AI Foundry portal. The Azure AI Foundry portal currently doesn't support creating a Standard project.

Description and Autodeploy	Diagram (click to zoom in)
Deploy a basic agent setup that uses Managed Identity for authentication. Resources for the AI hub, AI project, storage account, and AI Services are created for you.

The AI Services account is connected to your project and hub, and a gpt-4o-mini model is deployed in the eastus region. A Microsoft-managed key vault is used by default.

Deploy To Azure	An architecture diagram for basic agent setup.
Deploy a standard agent setup that uses Managed Identity for authentication.

Resources for the AI hub, AI project, key vault, storage account, AI Services, and AI Search are created for you.

The AI Services, AI Search, key vault, and storage account are connected to your project and hub. A gpt-4o-mini model is deployed in eastus region.

Deploy To Azure	An architecture diagram for standard agent setup.
[Optional] Model selection in autodeploy template
You can customize the model used by your agent by editing the model parameters in the autodeploy template. To deploy a different model, you need to update at least the modelName and modelVersion parameters.

By default, the deployment template is configured with the following values:

Model Parameter	Default Value
modelName	gpt-4o
modelFormat	OpenAI (for Azure OpenAI)
modelVersion	2024-11-20
modelSkuName	GlobalStandard
modelLocation	eastus
 Important

Don't change the modelFormat parameter.

The templates only support deployment of Azure OpenAI models. See which Azure OpenAI models are supported in the Azure AI Agent Service model support documentation.

[Optional] Use your own resources during agent setup
 Note

If you use an existing AI Services or Azure OpenAI resource, no model will be deployed. You can deploy a model to the resource after the agent setup is complete.

First, initialize a new project by running:

Console

Copy
npm init -y
Run the following commands to install the npm packages required.

Console

Copy
npm install @azure/ai-projects
npm install @azure/identity
npm install dotenv
Next, to authenticate your API requests and run the program, use the az login command to sign into your Azure subscription.

Azure CLI

Copy
az login

 Tip

You can also find your connection string in the overview for your project in the Azure AI Foundry portal, under Project details > Project connection string. A screenshot showing the connection string in the Azure AI Foundry portal.
