---
description: This article provide examples for using PSResourceGet with Azure Container Registry (ACR) repositories.
ms.date: 03/23/2024
ms.topic: reference
title: Use ACR repositories with PSResourceGet
---

# Use ACR repositories with PSResourceGet

## Create a repository

- https://learn.microsoft.com/azure/container-registry/container-registry-get-started-powershell

1. Sign in to Azure

   Connect-AzAccount

1. Create resource group

   New-AzResourceGroup -Name myResourceGroup -Location EastUS

1. Create container registry

   $registry = New-AzContainerRegistry -ResourceGroupName "myResourceGroup" -Name "mycontainerregistry" -EnableAdminUser -Sku Basic

1. Log in to registry

   Connect-AzContainerRegistry -Name $registry.Name

1. Push image to registry

   docker tag mcr.microsoft.com/hello-world mycontainerregistry.azurecr.io/hello-world:v1
   docker push <login-server>/hello-world:v1
   docker rmi <login-server>/hello-world:v1

1. Run image from registry

   docker run <login-server>/hello-world:v1

## Get the URL for the repository

## Register the repository

## Authentication

- https://learn.microsoft.com/azure/container-registry/tutorial-customer-managed-keys

## Use the ACR repository interactively

### Publish a package

### Search for packages

### Install a package

## Use the ACR repository in a pipeline

## Limitations
