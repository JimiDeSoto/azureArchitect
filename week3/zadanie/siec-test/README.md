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