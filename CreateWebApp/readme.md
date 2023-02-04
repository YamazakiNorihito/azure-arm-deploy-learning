- [チュートリアル:初めての ARM テンプレートを作成してデプロイする](https://learn.microsoft.com/ja-jp/azure/azure-resource-manager/templates/template-tutorial-create-first-template?tabs=azure-powershell)
  - [プロバイダーを使用してリソースを定義する](https://learn.microsoft.com/ja-jp/azure/templates/)

これを勉強するよ

下準備
まずは[クイックスタート: Visual Studio Code を使用して ARM テンプレートを作成する](https://learn.microsoft.com/ja-jp/azure/azure-resource-manager/templates/quickstart-create-templates-use-visual-studio-code?tabs=CLI)をやるよ

install

- VScode
  - [Azure Resource Manager (ARM) Tools](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)
- [Azure CLI ](https://learn.microsoft.com/ja-jp/cli/azure/)
- A[rm Template Browse code samples](https://learn.microsoft.com/en-us/samples/browse/?expanded=azure&products=azure-resource-manager)




```bash
az login

# 初回のみ
az group create --name myResourceGroup--location eastus

az deployment group create --name deployname --resource-group myResourceGroup --template-file azuredeploy.json --parameters azuredeploy.parameters.json

# リソースをクリーンアップする
az group delete --name arm-vscode
```


[チュートリアル:リンク済みテンプレートをデプロイする](https://learn.microsoft.com/ja-jp/azure/azure-resource-manager/templates/deployment-tutorial-linked-template?tabs=azure-powershell)
```bash
# link deploy
az storage account create --name noridevteststorage --resource-group myResourceGroupDev --location 
"East US"  --sku Standard_RAGRS   --encryption-services blob

az storage container create --name "templates" --connection-string "{primarykey string}"

z storage blob upload --account-name noridevteststorage --container-name templates --file azuredeploy.json --name azuredeploy.json

az storage blob upload --account-name noridevteststorage --container-name templates --file linkedStorageAccount.json  --name linkedStorageAccount.json

az storage account keys list -g myResourceGroupDev -n noridevteststorage --query [0].value -o tsv

az storage container generate-sas --account-name  noridevteststorage --account-key {accountkey} --name "templates" --permissions r --expiry 2023-1-30

az deployment group create --name DeployLinkedTemplate --resource-group myResourceGroupDev --template-uri https://noridevteststorage.blob.core.windows.net/templates/azuredeploy.json  --parameters azuredeploy.parameters.json --query-string "{上記のコマンド結果}"

```

