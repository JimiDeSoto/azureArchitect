# Az
az login
az group create --name scAzureArch --location westeurope
az group deployment create --name siec01 \
    --resource-group scAzureArch \
    --template-file "./sieciauto01.json" \
    --parameters sieciauto01.parameters.json


az group deployment create --name siec2 \
    --resource-group scAzureArch \
    --template-file "./sieci-bez-linked.json" \
    --parameters ./sieci.parameters.json

https://github.com/Azure/azure-quickstart-templates/blob/master/101-security-group-create/azuredeploy.json
https://github.com/bsilux/arm

az group deployment create --name siec2 --resource-group scAzureArch --template-file "sieci-bez-linked.json" --parameters sieci.parameters.json


az group deployment create --name siec4 --resource-group scAzureArch --template-file "sieci-4 -testlink.json" --parameters sieci.parameters.json
```

az account list-locations \
    --query "[].{Region:name}" \
    --out table

Utworzenie konta storage
az storage account create \
    --name armstorerk01\
    --resource-group scAzureArch \
    --location westeurope \
    --sku Standard_LRS \
    --kind StorageV2

Pobranie kluczy
az storage account keys list \
    --account-name armstorerk01 \
    --resource-group scAzureArch \
    --output table

export AZURE_STORAGE_ACCOUNT="armstorerk01"
export AZURE_STORAGE_KEY=""

Utworzenie kontenera
az storage container create --name arm-template --account-name armstorerk01

az storage blob upload \
    --container-name arm-template \
    --name 1-linked-nsg.json \
    --file 1-linked-nsg.json \
    --account-name armstorerk01

    az storage blob upload --container-name arm-template --name 1-linked-nsg.json --file 1-linked-nsg.json --account-name armstorerk01

az storage blob list --container arm-template --account-name armstorerk01

```

expiretime=$(date -u -d '90 minutes' +%Y-%m-%dT%H:%MZ)
connection=$(az storage account show-connection-string \
  --resource-group scAzureArch \
  --name armstorerk01 \
  --query connectionString)
token=$(az storage container generate-sas \
  --name arm-template \
  --expiry $expiretime \
  --permissions r \
  --output tsv \
  --connection-string $connection)
url=$(az storage blob url \
  --container-name arm-template \
  --name linked-keyvault.json \
  --output tsv \
  --connection-string $connection)
parameter='{"containerSasToken":{"value":"?'$token'"}}'

(Get-AzureRmKeyVault -VaultName rk01-key-vault).ResourceId

az storage blob upload \
    --container-name arm-template \
    --name linked-nsg.json \
    --file linked-nsg.json


az storage blob upload --container-name arm-template --name linked-nsg.json --file linked-nsg.json --account-name armstorerk01


az storage blob list --container arm-template --account-name armstorerk01

```

expiretime=$(date -u -d '120 minutes' +%Y-%m-%dT%H:%MZ)
connection=$(az storage account show-connection-string \
  --resource-group scAzureArch \
  --name armstorerk01 \
  --query connectionString)
token=$(az storage container generate-sas \
  --name arm-template \
  --expiry $expiretime \
  --permissions r \
  --output tsv \
  --connection-string $connection)
url=$(az storage blob url \
  --container-name arm-template \
  --name 1-linked-nsg.json \
  --output tsv \
  --connection-string $connection)
parameter='{"containerSasToken":{"value":"?'$token'"}}'

az group deployment create --name siec7 --resource-group scAzureArch --template-file "1-keyvault.json" --parameters 1-keyvault.parameters.json

az group deployment create --name siec8 --resource-group scAzureArch --template-file 1-main.parameters.json --parameters 2-sub-main.parameters.json