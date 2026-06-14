# End-to-End Azure/.NET: Fullstack, Data, & AI

Build end-to-end expertise to design, build, and operate scalable .NET and Azure solutions with data and AI; deliver a portfolio-ready project and be job-ready across full‑stack, data engineering, and solution architecture roles.

## Build cloud‑ready services with .NET

### Introduction

`Unit 1/5`
Imagine you're a software developer for an online retailer. The retailer's online storefront is a cloud-native, microservices-based ASP.NET Core app. You've been asked to add the ability to the app to have seasonal sales. The sales and the discounts need to be controlled by the sales team, so that app can't be recompiled or redeployed to see the changes.

This module guides you through implementing a feature flags library. This library creates a feature flag to toggle the visibility of the seasonal sale. The configuration values that support this feature flag are centralized by using the Azure App Configuration service.

You use your own Azure subscription to deploy the resources in this module. If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/en-us/pricing/purchase-options/azure-account?cid=msft_learn) before you begin.

> [!IMPORTANT]
>To avoid unnecessary charges in your Azure subscription, be sure to delete your Azure resources when you're done with this module.

#### Development Container

This module includes configuration files that define a [development container](https://containers.dev/), or *dev container*. Using a dev container ensures a standardized environment that's preconfigured with the required tools.

The dev container can run in either of two environments. Before you begin, follow the steps in one of the following links to set up your environment, including installing Docker and the necessary Visual Studio Code extensions.

* [Visual Studio Code and a supported Docker environment on your local machine.](https://learn.microsoft.com/en-us/training/modules/use-docker-container-dev-env-vs-code/)
* [GitHub Codespaces (costs may apply).](http://github.com/features/codespaces)

#### Learning Objectives

* Review ASP.NET Core app configuration concepts.
* Implement real-time feature toggling with the .NET Feature Management library.
* Implement a centralized Azure App Configuration store.
* Implement code to use features and configuration settings from the Azure App Configuration store.

#### Prerequisites

* Familiarity with C# and ASP.NET Core development at the beginner level.
* Familiarity with RESTful service concepts at the beginner level.
* Conceptual knowledge of containers.
* Access to an Azure subscription with **Owner** privilege.
* Ability to run development containers in Visual Studio Code or GitHub Codespaces.

### Review app configuration concepts

`Unit 2/5`

Creating microservices for a distributed environment presents a significant challenge. Cloud-hosted microservices often run in multiple containers in various regions. Implementing a solution that separates each service's code from configuration eases the triaging of issues across all environments.

In this unit, explore how to integrate ASP.NET Core and Docker configuration features with Azure App Configuration to tackle this challenge in an effective way.

You'll review the:

* ASP.NET Core configuration infrastructure.
* Kubernetes configuration abstraction—the ConfigMap.
* Azure App Configuration service.
* .NET Feature Management library.
* Feature flag components implemented in the app.

#### ASP.NET Core Configuration

Configuration in an ASP.NET Core project is contained in one or more .NET *configuration providers*. A [configuration provider](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/configuration/?view=aspnetcore-10.0#configuration-providers) is an abstraction over a specific configuration source, such as a JSON file. The configuration source's values are represented as a collection of key-value pairs.

An ASP.NET Core app can register multiple configuration providers to read settings from various sources. With the default application host, several configuration providers are automatically registered. The following configuration sources are available in the order listed:

* JSON file (*appsettings.json*)
* JSON file (*appsettings.{environment}.json*)
* User secrets
* Environment variables
* Command line

Each configuration provider can contribute its own key value. Furthermore, any provider can override a value from a provider that was registered earlier in the chain than itself. Given the registration order in the preceding list, a `UseFeatureManagement` command-line parameter overrides a `UseFeatureManagement` environment variable. Likewise, a `UseFeatureManagement` key within *appsettings.json* can be overridden by a `UseFeatureManagement` key stored in *appsettings.Development.json*.

Configuration key names can describe a hierarchy. For example, the notation eShop:Store:SeasonalSale refers to the SeasonalSale feature within the Store microservice of the eShop app. This structure can also map configuration values to an object graph or an array.