---
services: compute
platforms: java
author: alvadb
---

# Manage Azure resources with Java

**On this page**

- [Run the sample](#run)
- [What is ManageResources.java doing?](#example)
   - [Create a resource](#create)
   - [Update a resource](#update)
   - [List resources](#list)
   - [Delete a resource](#delete)
 
<a id="run"></a>
## Running the sample

1. Set the environment variable `AZURE_AUTH_LOCATION` with the full path for an auth file. See [how to create an auth file](https://github.com/Azure/azure-sdk-for-java/blob/master/AUTH.md).

2. Clone the repository.

```
git clone https://github.com/Azure-Samples/resources-java-manage-resource.git
```

3. Run the sample

```
cd resources-java-manage-resource
mvn clean compile exec:java
```

<a id="example"></a>
## What is ManageResourceGroup.java doing?

The sample starts by creating some variables that it uses in the various tasks.

```java
final String rgName = ResourceNamer.randomResourceName("rgRSMA", 24);
final String resourceName1 = ResourceNamer.randomResourceName("rn1", 24);
final String resourceName2 = ResourceNamer.randomResourceName("rn2", 24);
```

Then is signs in to the account using the authentication file.

```java
final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));

Azure azure = Azure.configure()
        .withLogLevel(HttpLoggingInterceptor.Level.NONE)
        .authenticate(credFile)
        .withDefaultSubscription();
```

It creates a resource group.

```java
azure.resourceGroups()
    .define(rgName)
    .withRegion(Region.US_WEST)
    .create();
```

<a id="create"></a>
### Create a resource

The resource that this sample creates is a storage account.

```java
StorageAccount storageAccount = azure.storageAccounts()
    .define(resourceName1)
    .withRegion(Region.US_WEST)
    .withExistingResourceGroup(rgName)
        .create();
```

<a id="update"></a>
### Update a resource

```java
storageAccount.update()
    .withSku(SkuName.STANDARD_RAGRS)
    .apply();
```

<a id="list"></a>
### List resources

```java
azure.storageAccounts().list();
```

<a id="delete"></a>
### Delete a resource group

```java
azure.storageAccounts().delete(storageAccount2.id());
```

## More information ##

[http://azure.com/java] (http://azure.com/java)

If you don't have a Microsoft Azure subscription you can get a FREE trial account [here](http://go.microsoft.com/fwlink/?LinkId=330212)

---

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.