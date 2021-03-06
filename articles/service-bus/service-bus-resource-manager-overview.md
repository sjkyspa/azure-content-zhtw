<properties
    pageTitle="使用 Azure Resource Manager 範本建立服務匯流排資源 | Microsoft Azure"
    description="使用 Azure Resource Manager 範本自動建立服務匯流排資源"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="07/11/2016"
    ms.author="sethm"/>

# 使用 Azure Resource Manager 範本建立服務匯流排資源

本文說明如何使用 Azure Resource Manager 範本、PowerShell 和服務匯流排資源提供者來建立和部署服務匯流排和事件中樞資源。

Azure Resource Manager 範本會協助您定義要部署給解決方案的資源，以及指定參數和變數，讓您可以針對不同的環境來輸入值。範本由 JSON 與運算式所組成，可讓您用來為部署建構值。如需撰寫 Azure Resource Manager 範本和討論範本格式的詳細資訊，請參閱[編寫 Azure Resource Manager 範本](../resource-group-authoring-templates.md)。

>[AZURE.NOTE] 本文中的範例會示範如何使用 Azure Resource Manager 來建立服務匯流排命名空間和訊息實體 (佇列)。如需其他範本範例，請造訪 [Azure 快速入門範本][]資源庫並搜尋「服務匯流排」。

## 服務匯流排和事件中樞 Resource Manager 範本

這些服務匯流排和事件中樞 Azure Resource Manager 範本可供下載和部署。按一下下列連結可取得各自的詳細資料，並附有 GitHub 上的範本連結：

- [建立服務匯流排命名空間](service-bus-resource-manager-namespace.md)
- [建立服務匯流排命名空間與佇列](service-bus-resource-manager-namespace-queue.md)
- [建立服務匯流排命名空間與主題和訂用帳戶](service-bus-resource-manager-namespace-topic.md)
- [建立服務匯流排命名空間與佇列和授權規則](service-bus-resource-manager-namespace-auth-rule.md)
- [建立事件中樞命名空間與事件中樞和取用者群組](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)

## 使用 PowerShell 部署

下列程序描述如何使用 PowerShell 部署建立了**標準**層服務匯流排命名空間的 Azure Resource Manager 範本，以及如何在該命名空間內部署佇列。這個範例是以[建立有佇列的服務匯流排命名空間](https://github.com/Azure/azure-quickstart-templates/tree/master/201-servicebus-create-queue)範本為基礎。近似的工作流程如下︰

1. 安裝 PowerShell。
2. 建立範本和 (選擇性) 參數檔案。
2. 在 PowerShell 中登入您的 Azure 帳戶。
3. 如果沒有資源群組，請建立一個新的。
4. 測試部署。
5. 如有需要，請設定部署模式。
6. 部署範本。

如需部署 Azure Resource Manager 範本的完整資訊，請參閱[使用 Azure Resource Manager 範本部署資源][]。

### 安裝 PowerShell

遵循[如何安裝並設定 Azure PowerShell](../powershell-install-configure.md) 的指示安裝 Azure PowerShell。

### 建立範本

從 GitHub 複製 [201-servicebus-create-queue](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.json) 範本：

```
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "type": "string",
            "metadata": {
                "description": "Name of the Service Bus namespace"
            }
        },
        "serviceBusQueueName": {
            "type": "string",
            "metadata": {
                "description": "Name of the Queue"
            }
        },
        "serviceBusApiVersion": {
            "type": "string",
            "defaultValue": "2015-08-01",
            "metadata": {
                "description": "Service Bus ApiVersion used by the template"
            }
        }
    },
    "variables": {
        "location": "[resourceGroup().location]",
        "sbVersion": "[parameters('serviceBusApiVersion')]",
        "defaultSASKeyName": "RootManageSharedAccessKey",
        "authRuleResourceId": "[resourceId('Microsoft.ServiceBus/namespaces/authorizationRules', parameters('serviceBusNamespaceName'), variables('defaultSASKeyName'))]"
    },
    "resources": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "StandardSku",
            "tier": "Standard"
        },
        "resources": [{
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusQueueName')]",
            "type": "Queues",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusQueueName')]"
            }
        }]
    }],
    "outputs": {
        "NamespaceConnectionString": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryConnectionString]"
        },
        "SharedAccessPolicyPrimaryKey": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryKey]"
        }
    }
}
```

### 建立參數檔案 (選擇性)

若要使用選擇性的參數檔案，請複製 [201-servicebus-create-queue](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.parameters.json) 檔案。以您要在這個部署中建立的服務匯流排命名空間名稱取代 `serviceBusNamespaceName` 值，並以您要建立的佇列名稱取代 `serviceBusQueueName` 值。

```
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "value": "<myNamespaceName>"
        },
        "serviceBusQueueName": {
            "value": "<myQueueName>"
        },
        "serviceBusApiVersion": {
            "value": "2015-08-01"
        }
    }
}
```

如需詳細資訊，請參閱[參數檔案](../resource-group-template-deploy.md#parameter-file)主題。

### 登入 Azure 並設定 Azure 訂用帳戶

從 PowerShell 提示字元中執行下列命令：

```
Login-AzureRmAccount
```

系統會提示您登入您的 Azure 帳戶。登入之後，執行下列命令以檢視可用的訂用帳戶。

```
Get-AzureRMSubscription
```

這個命令會傳回可用的 Azure 訂用帳戶清單。執行下列命令為目前的工作階段選擇訂用帳戶。以您要使用的 Azure 訂用帳戶 GUID取代 `<YourSubscriptionId>`。

```
Set-AzureRmContext -SubscriptionID <YourSubscriptionId>
```

### 設定資源群組

如果沒有現成的資源群組，請使用 **New-AzureRmResourceGroup** 命令建立新的資源群組。提供您要使用的資源群組名稱和位置。例如：

```
New-AzureRmResourceGroup -Name MyDemoRG -Location "West US"
```

如果成功，就會顯示新資源群組的摘要。

```
ResourceGroupName : MyDemoRG
Location          : westus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/<GUID>/resourceGroups/MyDemoRG
```

### 測試部署

執行 `Test-AzureRmResourceGroupDeployment` Cmdlet 驗證部署。測試部署時，請提供與執行部署時完全一致的參數。

```
Test-AzureRmResourceGroupDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

### 建立部署

若要建立新的部署，請執行 `New-AzureRmResourceGroupDeployment` 命令，並於提示出現時提供必要的參數。參數會包含部署的名稱、資源群組的名稱，以及範本檔案的路徑或 URL。如未指定 **Mode** 參數，即會使用預設值 **Incremental**。如需詳細資訊，請參閱[累加部署與完整部署](../resource-group-template-deploy.md#incremental-and-complete-deployments)。

下列命令會提示您在 PowerShell 視窗中輸入三個參數︰

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

若要改為指定參數檔案，請使用下列命令。

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -TemplateParameterFile <path to parameters file>\azuredeploy.parameters.json
```

執行部署 Cmdlet 時，您也可以使用內嵌參數。命令如下所示︰

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -parameterName "parameterValue"
```

若要執行[完整](../resource-group-template-deploy.md#incremental-and-complete-deployments)部署，請將 **Mode** 參數設為 **Complete**：

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -Mode Complete -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json 
```

### 驗證部署

如果資源成功部署，PowerShell 視窗中就會顯示部署的摘要︰

```
DeploymentName    : MyDemoDeployment
ResourceGroupName : MyDemoRG
ProvisioningState : Succeeded
Timestamp         : 4/19/2016 10:38:30 PM
Mode              : Incremental
TemplateLink      :
Parameters        :
                    Name             Type                       Value
                    ===============  =========================  ==========
                    serviceBusNamespaceName  String             <namespaceName>
                    serviceBusQueueName  String                 <queueName>
                    serviceBusApiVersion  String                2015-08-01

```

## 後續步驟

您現在已了解部署 Azure Resource Manager 範本的基本工作流程和命令。如需更多的詳細資訊，請瀏覽下列連結內容：

- [Azure 資源管理員概觀][]
- [使用 Azure Resource Manager 範本部署資源][]
- [撰寫範本](../resource-group-authoring-templates.md)


[Azure 資源管理員概觀]: ../resource-group-overview.md
[使用 Azure Resource Manager 範本部署資源]: ../resource-group-template-deploy.md
[Azure 快速入門範本]: https://azure.microsoft.com/documentation/templates/?term=service+bus

<!---HONumber=AcomDC_0831_2016-->