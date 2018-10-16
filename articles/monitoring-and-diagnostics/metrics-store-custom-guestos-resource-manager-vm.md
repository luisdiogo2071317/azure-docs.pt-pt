---
title: Enviar métricas de SO convidado para a métrica do Azure Monitor armazenam utilizando um modelo do Resource Manager para uma máquina de Virtual do Windows
description: Enviar métricas de SO convidado para a métrica do Azure Monitor armazenam utilizando um modelo do Resource Manager para uma máquina de Virtual do Windows
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: howto
ms.date: 09/24/2018
ms.author: ancav
ms.component: metrics
ms.openlocfilehash: f3076054eb6e18eb5143a34ba558c1f9e43ea4a5
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49345191"
---
# <a name="send-guest-os-metrics-to-the-azure-monitor-metric-store-using-a-resource-manager-template-for-a-windows-virtual-machine"></a>Enviar métricas de SO convidado para a métrica do Azure Monitor armazenam utilizando um modelo do Resource Manager para uma máquina de Virtual do Windows

O Azure Monitor [extensão de diagnóstico do Windows Azure](azure-diagnostics.md) (WAD) permite-lhe recolher métricas e registos da execução de sistema de operativo convidado (SO convidado) como parte de um cluster do Service Fabric, serviço Cloud ou Máquina Virtual.  A extensão pode enviar telemetria para vários locais diferentes listados no artigo ligado anteriormente.  

Este artigo descreve o processo de métricas de desempenho de SO de convidado de envio para uma máquina de Virtual do Windows para o arquivo de dados do Azure Monitor. Começando com WAD versão 1.11, pode escrever métricas diretamente com o armazenamento de métricas do Azure Monitor, onde as métricas de plataforma padrão já são recolhidas. Armazená-los nesta localização permite que acesse as mesmas ações disponíveis para as métricas de plataforma.  Ações incluem quase em tempo real de alerta, criação de gráficos, encaminhamento, aceder a partir da REST API e muito mais.  No passado, a extensão WAD escreveu para o armazenamento do Azure, mas não o arquivo de dados do Azure Monitor.   

Se estiver familiarizado com modelos do Resource Manager, saiba mais sobre [implementações de modelo](../azure-resource-manager/resource-group-overview.md)e sua estrutura e a sintaxe.  

## <a name="pre-requisites"></a>Pré-requisitos

- A sua subscrição tem de estar registrada com [Microsoft. insights](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-6.8.1) 

- Tem de ter [do Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-6.8.1) instalado, ou pode utilizar [Azure CloudShell](https://docs.microsoft.com/azure/cloud-shell/overview.md) 

 
## <a name="set-up-azure-monitor-as-a-data-sink"></a>Configurar o Azure Monitor como um sink de dados 
A extensão de diagnóstico do Azure utiliza um recurso chamado "Coletores de dados" para a métrica das rotas e registos para localizações diferentes.  Os passos seguintes mostram como utilizar um modelo do Resource Manager e o PowerShell para implementar uma VM com o novo sink de dados de "Do Azure Monitor". 

## <a name="author-resource-manager-template"></a>Modelo de Gestor de recursos do autor 
Neste exemplo, pode utilizar um modelo de exemplo disponíveis publicamente. Os modelos de início são em https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows 

- **Azuredeploy. JSON** é um modelo do Resource Manager previamente configurada para a implementação de uma Máquina Virtual. 

- **Azuredeploy** é um ficheiro de parâmetros que armazena informações como o nome de utilizador e palavra-passe que pretende definir para a sua VM. Durante a implementação, o modelo do Resource Manager utiliza os parâmetros definidos neste ficheiro. 

Transfira e guarde ambos os ficheiros localmente. 

###  <a name="modify-azuredeployparametersjson"></a>Modificar azuredeploy
Abra o *azuredeploy* ficheiro 

1. Introduza os valores para *adminUsername* e *adminPassword* para a VM. Esses parâmetros são usados para acesso remoto à VM. Não utilize os neste modelo para evitar que a sua VM hijacked. A internet para nomes de utilizador e palavras-passe em repositórios de Github públicos de análise de bots. É provável que realizar testes VMs com esses padrões.  

1. Crie um dnsname exclusivo para a VM.  

### <a name="modify-azuredeployjson"></a>Modificar azuredeploy. JSON

Abra o *azuredeploy. JSON* ficheiro 

Adicionar um ID de conta de armazenamento para o **variáveis** secção do modelo após a entrada para **storageAccountName**.  

```json
// Find these lines 
"variables": { 
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'sawinvm')]", 

// Add this line directly below.  
    "accountid": "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]", 
```

Adicione este identidades geridas para a extensão de recursos do Azure para o modelo na parte superior da seção "recursos".  A extensão garante que o Azure Monitor aceita as métricas que está a ser emitidas.  

```json
//Find this code 
"resources": [
// Add this code directly below
    { 
        "type": "Microsoft.Compute/virtualMachines/extensions", 
        "name": "WADExtensionSetup", 
        "apiVersion": "2015-05-01-preview", 
        "location": "[resourceGroup().location]", 
        "dependsOn": [ 
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]" ], 
        "properties": { 
            "publisher": "Microsoft.ManagedIdentity", 
            "type": "ManagedIdentityExtensionForWindows", 
            "typeHandlerVersion": "1.0", 
            "autoUpgradeMinorVersion": true, 
            "settings": { 
                "port": 50342 
            } 
        } 
    }, 
```

Adicione a configuração de "identity" para o recurso da VM para garantir que o Azure atribui a extensão MSI uma identidade de sistema. Este passo garante que a VM pode emitir as métricas de convidado sobre si próprio para o Azure Monitor 

```json
// Find this section
                "subnet": {
            "id": "[variables('subnetRef')]"
            }
        }
        }
    ]
    }
},
{ 
    "apiVersion": "2017-03-30", 
    "type": "Microsoft.Compute/virtualMachines", 
    "name": "[variables('vmName')]", 
    "location": "[resourceGroup().location]", 
    // add these 3 lines below
    "identity": {  
    "type": "SystemAssigned" 
    }, 
    //end of added lines
    "dependsOn": [
    "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
    ],
    "properties": {
    "hardwareProfile": {   
    ...
```

Adicione a seguinte configuração para ativar a extensão de diagnóstico numa máquina de Virtual do Windows.  Para um simples baseado no Resource Manager Máquina Virtual, podemos adicionar a configuração da extensão para a matriz de recursos para a Máquina Virtual. A linha "sinks": "AzMonSink" e correspondente "SinksConfig" mais tarde na secção ativar a extensão para emitir métricas diretamente para o Azure Monitor. Fique à vontade Adicionar/remover contadores de desempenho, conforme necessário.  


```json
        "networkProfile": {
            "networkInterfaces": [
            {
                "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
            }
            ]
        },
"diagnosticsProfile": { 
    "bootDiagnostics": { 
    "enabled": true, 
    "storageUri": "[reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))).primaryEndpoints.blob]" 
    } 
} 
}, 
//Start of section to add 
"resources": [        
{ 
            "type": "extensions", 
            "name": "Microsoft.Insights.VMDiagnosticsSettings", 
            "apiVersion": "2015-05-01-preview", 
            "location": "[resourceGroup().location]", 
            "dependsOn": [ 
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]" 
            ], 
            "properties": { 
            "publisher": "Microsoft.Azure.Diagnostics", 
            "type": "IaaSDiagnostics", 
            "typeHandlerVersion": "1.4", 
            "autoUpgradeMinorVersion": true, 
            "settings": { 
                "WadCfg": { 
                "DiagnosticMonitorConfiguration": { 
    "overallQuotaInMB": 4096, 
    "DiagnosticInfrastructureLogs": { 
                    "scheduledTransferLogLevelFilter": "Error" 
        }, 
                    "Directories": { 
                    "scheduledTransferPeriod": "PT1M", 
    "IISLogs": { 
                        "containerName": "wad-iis-logfiles" 
                    }, 
                    "FailedRequestLogs": { 
                        "containerName": "wad-failedrequestlogs" 
                    } 
                    }, 
                    "PerformanceCounters": { 
                    "scheduledTransferPeriod": "PT1M", 
                    "sinks": "AzMonSink", 
                    "PerformanceCounterConfiguration": [ 
                        { 
                        "counterSpecifier": "\\Memory\\Available Bytes", 
                        "sampleRate": "PT15S" 
                        }, 
                        { 
                        "counterSpecifier": "\\Memory\\% Committed Bytes In Use", 
                        "sampleRate": "PT15S" 
                        }, 
                        { 
                        "counterSpecifier": "\\Memory\\Committed Bytes", 
                        "sampleRate": "PT15S" 
                        } 
                    ] 
                    }, 
                    "WindowsEventLog": { 
                    "scheduledTransferPeriod": "PT1M", 
                    "DataSource": [ 
                        { 
                        "name": "Application!*" 
                        } 
                    ] 
                    }, 
                    "Logs": { 
                    "scheduledTransferPeriod": "PT1M", 
                    "scheduledTransferLogLevelFilter": "Error" 
                    } 
                }, 
                "SinksConfig": { 
                    "Sink": [ 
                    { 
                        "name" : "AzMonSink", 
                        "AzureMonitor" : {} 
                    } 
                    ] 
                } 
                }, 
                "StorageAccount": "[variables('storageAccountName')]" 
            }, 
            "protectedSettings": { 
                "storageAccountName": "[variables('storageAccountName')]", 
                "storageAccountKey": "[listKeys(variables('accountid'),'2015-06-15').key1]", 
                "storageAccountEndPoint": "https://core.windows.net/" 
            } 
            } 
        } 
        ] 
//End of section to add 
```


Guarde e feche ambos os ficheiros 
 

## <a name="deploy-the-resource-manager-template"></a>Implementar o modelo do Resource Manager 

> [!NOTE]
> Tem de executar a versão 1.5 da extensão de diagnóstico do Azure ou superior e tem "autoUpgradeMinorVersion": propriedade definida como 'true' no modelo do Resource Manager.  Azure, em seguida, carrega a extensão correta quando iniciar a VM. Se não tiver estas definições no seu modelo, alterá-los e Reimplementar o modelo. 


Para implementar o modelo do Resource Manager, irá tirar partido do Azure PowerShell.  

1. Iniciar o PowerShell 
1. Início de sessão para o Azure com `Login-AzureRmAccount`
1. Obter a lista de subscrições através de `Get-AzureRmSubscription`
1. Definir a subscrição que irá que criar/atualizar a máquina virtual numa 

   ```PowerShell
   Select-AzureRmSubscription -SubscriptionName "<Name of the subscription>" 
   ```
1. Criar um novo grupo de recursos para a VM que está a ser implementada, execute o comando abaixo 

   ```PowerShell
    New-AzureRmResourceGroup -Name "<Name of Resource Group>" -Location "<Azure Region>" 
   ```
   > [!NOTE] 
   > Lembre-se [utilizam uma região do Azure que está ativada para métricas personalizadas](metrics-custom-overview.md). 
 
1. Execute os seguintes comandos para implementar a VM com o  
   > [!NOTE] 
   > Se desejar atualizar de uma VM existente, basta adicionar *-modo Incremental* ao final do comando seguinte. 
 
   ```PowerShell
   New-AzureRmResourceGroupDeployment -Name "<NameThisDeployment>" -ResourceGroupName "<Name of the Resource Group>" -TemplateFile "<File path of your Resource Manager template>" -TemplateParameterFile "<File path of your parameters file>" 
   ```
  
1. Assim que for concluída com êxito a implementação deve ser capaz de encontrar a VM no Portal do Azure e deve ser a emitir métricas para o Azure Monitor. 

   > [!NOTE] 
   > Poderão ocorrer erros em todo o vmSkuSize selecionado. Se isto acontecer, regresse ao seu ficheiro azuredeploy JSON e atualize o valor predefinido do parâmetro vmSkuSize. Neste caso, é recomendável tentar "Standard_DS1_v2"). 

## <a name="chart-your-metrics"></a>As suas métricas do gráfico 

1. Inicie sessão no Portal do Azure 

1. No menu da esquerda, clique em **Monitor** 

1. Na página do Monitor, clique em **métricas**. 

   ![Página de métricas](./media/metrics-store-custom-rest-api/metrics.png) 

1. Alterar o período de agregação para **últimos 30 minutos**.  

1. No recurso da lista pendente, selecione a VM que acabou de criar. Se não alterar o nome do modelo, deve ser *SimpleWinVM2*.  

1. Nos espaços de nome pendente, selecione **azure.vm.windows.guest** 

1. As métricas de lista pendente, selecione **memória\%Bytes dedicados em utilização**.  
 

## <a name="next-steps"></a>Passos Seguintes
- Saiba mais sobre [métricas personalizadas](metrics-custom-overview.md).