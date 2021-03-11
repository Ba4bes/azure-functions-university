# Configuration & Settings

Watch the recording of this lesson [on YouTube]().

## Goal 🎯

The goal of this lesson is to learn where you can store application settings and use them in your Azure Functions.

This lessons consists of the following exercises:

|Nr|Exercise
|-|-
|0|[Prerequisites](#0-prerequisites)
|1|[Why do we use configuration?](#1-why-do-we-use-configuration)
|2|[Built-in settings](#2-built-in-settings)
|3|[Adding custom application settings](#3-adding-custom-application-settings)
|4|[Using App Configuration Service](#4-using-app-configuration-service)
|5|[Using Azure KeyVault for Secrets](#5-using-azure-keyvault-for-secrets)
|6|[Homework](#6-homework)
|7|[More info](#7-more-info)

---

## 0. Prerequisites

| Prerequisite | Exercise
| - | -

## 1. Why do we use configuration? (Stace)

Configuration. One or more values, read into your application at run time, which can change the way that it runs, or change the data sources it uses at runtime.

When we run our applications locally, we run them against a known environment. We know the location of all resources we need, and if the code is staying on our machines then we also know all of our secrets.

In the early years of software development configuration was not as important to smaller applications. Production and development environments looked similar, or there is only a production environment. You deployed your code and it just worked.

That didn't work for larger applications though, and in this day and age it doesn't really work at all.

As applications became larger, as they became more complex and distributed, and as they were deployed to more diverse environments, you - the developer - lost the knowledge of where and how the application was going to run.

You could make a change to your application and recompile it for each environment that you deploy into - but that is both a lot of extra (manual) work, and introduces risks as what is deployed to test and production environments are all different applications. This is not a realistic approach.

The solution to this is to introduce configuration settings to your application. Something that can be changed externally to your code to allow the same code to work in multiple different places, and to allow a change in behavior per environment. That is, use environment specific settings to set certain values in your code at runtime instead of compile time.

Some examples of the type of data we want to separate from our code are:

* Connection strings
* The execution mode of the application (dev, test, production etc)
* API URLs
* Service account details

Another reason for configuration settings is security. Our code is not just on our local machines. It lives in source control systems such as: Azure DevOps, GitHub etc.

Putting sensitive information into these source control systems, even in private repositories, is a security risk as it allows anyone with access to the repository to know sensitive information about all of your environments. If your repository is public the risk is even greater!

Instead sensitive information (secrets) should be accessed via configuration variables, allowing for each environment to use it's own secrets, and keeping those secrets private to the environments where they need to be kept.

> 📝 **Tip** - When writing your application start using configuration settings from the start, that way everything that should be configurable is configurable and sensitive information isn't missed when moving hard coded values before committing to source control

> 🔎 **Observation** - A primary use case for environment variables is to limit the need to modify and re-release an application due to changes in configuration data

## 2. Built-in settings (Marc)

https://docs.microsoft.com/en-us/azure/azure-functions/functions-app-settings
https://docs.microsoft.com/en-us/azure/azure-functions/functions-how-to-use-azure-function-app-settings?tabs=portal#settings

### Steps

1.
2.
3.

> 📝 **Tip** - < TIP >

> 🔎 **Observation** - < OBSERVATION >

> ❔ **Question** - < QUESTION >

## 3. Adding custom application settings

### 3.1. Using local.settings.json (Stace)

### 3.2. Using local env settings on OS (Stace)

### 3.3. Publish settings using VS Code (Marc)

### 3.4. Publish settings using Azure CLI / Functions CLI (Marc)

Include using GitHub secret in GH action in the Deployment lesson.

### Steps

1.
2.
3.

> 📝 **Tip** - < TIP >

> 🔎 **Observation** - < OBSERVATION >

> ❔ **Question** - < QUESTION >

## 4. Using App Configuration Service (Stace)

But there is now a new problem, we have all of our environment variables set up inside our function application itself.

This comes with many drawbacks:

* Management of configuration gets complex
* Management of configuration is spread over multiple locations for multiple apps
* We need a way of sharing this configuration with all developers (without sharing sensitive information in our repo!)

What we need is a centrally managed store of configuration that we can use. Thankfully there is one: **Azure App Configuration**

The Azure App Configuration Service is a fully managed store that allows for fast retrieval of data from *any* Azure application. Perfect for use with Azure Functions.

More than that, the data is encrypted, both at rest and in transit, and has native integration with many popular frameworks.

Now, rather than keeping all environment variables inside of the function themselves, we only need one. Something to point to the App Configuration service itself. This connection string is also all we need to share between our developers as well. We can now make sure that all developers are using the same configuration when running the application locally.

### Steps

1. Inside the Azure Portal, in a Resource Group Click the 'Add' button
![Create Config App](../img/lessons/configuration/resource-group-create-resource.png)
2. In the search box type 'App Configuration'
![Add Resource Search Box](../img/lessons/configuration/create-resource-search-config-service.png)
3. Click the 'Create' button
![App Config Create Button](../img/lessons/configuration/create-resource-config-service.png)
4. Fill in the details for the App Config service. Pick the free tier for this tutorial.
![App Config Creation Screen](../img/lessons/configuration/create-resource-config-service.png)
5. Click the 'Review + create' button, followed by 'Create' button
![App Config Creation Screen](../img/lessons/configuration/review-create-to-create.png)
6. When the resource has been created click 'Go to resource'
![Created Config App](../img/lessons/configuration/create-resource-app-config-deploy-complete.png)





Add a config value to the application

Create function application/Open Function application

2. App: setup startup.cs file, add code to get config service
3. Use setting inside of application: inject  the IConfig
4. IN the function get the value out of the 

> 📝 **Tip** - Have multiple settings, or multiple apps needing the same setting? Use an App Configuration Service

> 🔎 **Observation** - Setup work needed, overall better

> ❔ **Question** - How do you ensure that you settings are always in sync when multiple need to change

https://docs.microsoft.com/en-us/azure/azure-app-configuration/quickstart-azure-functions-csharp

## 5. Using Azure KeyVault for Secrets (Marc)

https://docs.microsoft.com/en-us/azure/app-service/app-service-key-vault-references?toc=/azure/azure-functions/toc.json

### Steps

1.
2.
3.

> 📝 **Tip** - < TIP >

> 🔎 **Observation** - < OBSERVATION >

> ❔ **Question** - < QUESTION >

## 6. Homework (Marc)

Make a template repo with hard coded values that need to be rewritten to make use of app settings, App Config Service and Azure KeyVault.

## 7. More info

https://docs.microsoft.com/en-us/azure/azure-functions/functions-app-settings

---
[◀ Previous lesson](<previous>.md) | [🔼 Index](_index.md) | [Next lesson ▶](<next>.md)
