# How to mount azure storage as Databricks DBFS

[Official Document](https://learn.microsoft.com/en-us/azure/databricks/dbfs/mounts)

## sample code
```
configs = {"fs.azure.account.auth.type": "OAuth",
          "fs.azure.account.oauth.provider.type": "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider",
          "fs.azure.account.oauth2.client.id": "<application-id>",
          "fs.azure.account.oauth2.client.secret": dbutils.secrets.get(scope="<scope-name>",key="<service-credential-key-name>"),
          "fs.azure.account.oauth2.client.endpoint": "https://login.microsoftonline.com/<directory-id>/oauth2/token"}

# Optionally, you can add <directory-name> to the source URI of your mount point.
dbutils.fs.mount(
  source = "abfss://<container-name>@<storage-account-name>.dfs.core.windows.net/",
  mount_point = "/mnt/<mount-name>",
  extra_configs = configs)
```

## find arguments
* directory-id  
    
    directory-id here is also called Tenant ID in Azure
    ![pic](https://github.com/mou-tw/Databricks_notes/blob/main/img/tenant_properties.JPG  )

    ![pic](https://github.com/mou-tw/Databricks_notes/blob/main/img/tenant_properties_page.JPG  )

* application-id

    find in AAD App registration overview page

* secret

    click "Certificates & secrets" blade in AAD App registration page
    create or use client secrets

    ![pic](https://github.com/mou-tw/Databricks_notes/blob/main/img/secret_get.JPG  )
