# Databricks MountedPoints Project (How to make)

##### This repository contains the personal Information for Conection and MontedPoints. 

## Description

### **English Version**:

This project only aims to show in a clear way how to perform a MontedPoint in AZURE, focusing on all the stages from the creation of a main service to access, such as the verification of files after the point mount.

### **French Version**:

Ce projet vise uniquement à montrer de manière claire comment effectuer un MountedsPoint dans AZURE, en se concentrant sur toutes les étapes depuis la création d’un service principal jusqu’à l’accès, comme la vérification des fichiers après le montage du point.

### **Spanish version**:
>
Este projecto solo pretende mostrar de una manera clara como realizar un MountedPoint en AZURE, enfocando todas las etapas desde la creacion de un servicio principal para el acceso, como la verificacion de archivos luego del montaje del punto


## Tooling

### Databricks <img src="https://ui-assets.cloud.databricks.com/static/media/databricks-text-dark-mode.2cbffc111b6c414acdb710ca52abbad7.svg" title="Databricks" alt="Databricks" width="40" height="40"/>

We take advantage of the free MongoDB cluster for this project.

* **Databricks Runtime Version:** 15.4 LTS (includes Apache Spark 3.5.0, Scala 2.12)
* **Node Type:** Standard_D3_v2 14GB Memory, 4cores
* **Access mode:** No isolation shared

### Azure <img src="https://github.com/devicons/devicon/blob/master/icons/azure/azure-original.svg" title="Azure" alt="Azure" width="40" height="40"/>

* **Azure AD**  Active Directory for Service Principal 
* **Storage accounts** Manage File input / output
* **Hosted Databricks** Databricks Workspace / Scope Secret / Databricks CLI (On Premises)
* **KeyVault** Gestion des secrets in KeyVault

> [!CAUTION]
> You can install Databricks CLI On Premises to create connections and secretScopes

### **Steps and instructions** 

1. Create a Service Principal
*	**Azure AD (ActiveDirectory)**
*	**App Registration**
*	**Create SP and Create Secret**

2. Storage Account
*	Access Control
*	Check Access to Service Principal 
*	Add Grant access to this resource (Storage Blob Data Contributor)

3. Configuration Databricks CLI
*	databricks configure --token
*	https://...
*	Create a PAT ( Personal Access Token ) into workspace Databricks
After =>databricks secrets list-scopes -- after
*	Create a SecretScope (recommended in Premium workspaces Databricks) 
*	Principal page workspace add #secrets/createScope to add secretScope (KeyVault URI + resource ID[KeyVault]) for all resources

## **Configuration**: 

`configs = {"fs.azure.account.auth.type": "OAuth",
          "fs.azure.account.oauth.provider.type": "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider",
          "fs.azure.account.oauth2.client.id": "<APPLICATION_CLIENT_ID_IN_SERVICEPRINCIPAL>",
          "fs.azure.account.oauth2.client.secret": dbutils.secrets.get(scope="<SECRETSCOPE_NAME>",key="<SECRETNAME_IN_KEYVAULT>"),
          "fs.azure.account.oauth2.client.endpoint": "https://login.microsoftonline.com/<DIRECTORY_TENANT_ID_IN_SERVICEPRINCIPAL>/oauth2/token"}

### Optionally, you can add <directory-name> to the source URI of your mount point.
dbutils.fs.mount(
  source = "abfss://<CONTAINER_NAME>@<STORAGE_ACCOUNT_NAME>.dfs.core.windows.net/",
  mount_point = "/mnt/<mnt_name>",
  extra_configs = configs)`


## License
[MIT License](LICENSE)
