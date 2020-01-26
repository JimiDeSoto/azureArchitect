## Zadnie tydzien3
rGroup=scAzureArch
location=westeurope

az group create --name scAzureArch --location westeurope

az group deployment create --name LinuxTest01 \
    --resource-group $rGroup \
    --template-file "./azuredeploy.json"