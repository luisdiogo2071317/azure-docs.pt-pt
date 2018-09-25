---
title: Enviar métricas de SO convidado para a métrica do Azure Monitor armazenam utilizando um modelo do Resource Manager para um conjunto de dimensionamento de máquinas virtuais do Windows
description: Enviar métricas de SO convidado para a métrica do Azure Monitor armazenam utilizando um modelo do Resource Manager para um conjunto de dimensionamento de máquinas virtuais do Windows
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: howto
ms.date: 09/24/2018
ms.author: ancav
ms.component: metrics
ms.openlocfilehash: d896cb01c7dc2cd4ed028db418f838809c7ce25c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46987004"
---
# <a name="send-guest-os-metrics-to-the-azure-monitor-metric-store-using-a-resource-manager-template-for-a-windows-virtual-machine-scale-set"></a>Enviar métricas de SO convidado para a métrica do Azure Monitor armazenam utilizando um modelo do Resource Manager para um conjunto de dimensionamento de máquinas virtuais do Windows

O Azure Monitor [extensão de diagnóstico do Windows Azure](azure-diagnostics.md) (WAD) permite-lhe recolher métricas e registos da execução de sistema de operativo convidado (SO convidado) como parte de um cluster do Service Fabric, serviço Cloud ou Máquina Virtual.  A extensão pode enviar telemetria para vários locais diferentes listados no artigo ligado anteriormente.  

Este artigo descreve o processo de envio métricas de desempenho do SO de convidado para um dimensionamento de máquinas virtuais do Windows definido para o arquivo de dados do Azure Monitor. Começando com WAD versão 1.11, pode escrever métricas diretamente com o armazenamento de métricas do Azure Monitor, onde as métricas de plataforma padrão já são recolhidas. Armazená-los nesta localização permite que acesse as mesmas ações disponíveis para as métricas de plataforma.  Ações incluem quase em tempo real de alerta, criação de gráficos, encaminhamento, aceder a partir da REST API e muito mais.  No passado, a extensão WAD escreveu para o armazenamento do Azure, mas não o arquivo de dados do Azure Monitor.  

Se estiver familiarizado com modelos do Resource Manager, saiba mais sobre [implementações de modelo](../azure-resource-manager/resource-group-overview.md)e sua estrutura e a sintaxe.  

## <a name="pre-requisites"></a>Pré-requisitos

- A sua subscrição tem de estar registrada com [Microsoft. insights](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-6.8.1) 

- Tem de ter [do Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-6.8.1) instalado, ou pode utilizar [Azure CloudShell](https://docs.microsoft.com/azure/cloud-shell/overview.md) 


## <a name="set-up-azure-monitor-as-a-data-sink"></a>Configurar o Azure Monitor como um sink de dados 
A extensão de diagnóstico do Azure utiliza um recurso chamado "Coletores de dados" para a métrica das rotas e registos para localizações diferentes.  Os passos seguintes mostram como utilizar um modelo do Resource Manager e o PowerShell para implementar uma VM com o novo sink de dados de "Do Azure Monitor". 

## <a name="author-resource-manager-template"></a>Modelo de Gestor de recursos do autor 
Neste exemplo, pode utilizar um modelo de exemplo disponíveis publicamente. Os modelos de início são em https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-autoscale  

- **Azuredeploy. JSON** é um modelo do Resource Manager previamente configurada para a implementação de um conjunto de dimensionamento de máquinas virtuais

- **Azuredeploy** é um ficheiro de parâmetros que armazena informações como o nome de utilizador e palavra-passe que pretende definir para a sua VM. Durante a implementação, o modelo do Resource Manager utiliza os parâmetros definidos neste ficheiro. 

Transfira e guarde ambos os ficheiros localmente. 

###  <a name="modify-azuredeployparametersjson"></a>Modificar azuredeploy
Abra o *azuredeploy* ficheiro 

- Fornecer um **vmSKU** gostaria de implementar (Recomendamos Standard_D2_v3) 
- Especifique um **windowsOSVersion** desejar para o conjunto de dimensionamento de máquina virtual (Recomendamos 2016 Datacenter) 
- Nome do dimensionamento da máquina virtual definido recursos a serem implantados com uma **vmssName** propriedade. Por exemplo, *VMSS-WAD-teste*.    
- Especifique o número de VMs que gostaria de estar em execução no conjunto de dimensionamento com o **instanceCount** propriedade
- Introduza os valores para **adminUsername** e **adminPassword** para o dimensionamento de máquinas virtuais definido. Esses parâmetros são usados para acesso remoto para as VMs no conjunto de dimensionamento. Esses parâmetros são usados para acesso remoto à VM. Não utilize os neste modelo para evitar que a sua VM hijacked. A internet para nomes de utilizador e palavras-passe em repositórios de Github públicos de análise de bots. É provável que realizar testes VMs com esses padrões. 


###  <a name="modify-azuredeployjson"></a>Modificar azuredeploy. JSON
Abra o *azuredeploy. JSON* ficheiro 

Adicione uma variável para conter as informações de conta de armazenamento no modelo do Resource Manager. Ainda tem de fornecer uma conta de armazenamento como parte da instalação da extensão do diagnóstico. Todos os registos e/ou contadores de desempenho especificados no ficheiro de configuração de diagnóstico são escritos para a conta de armazenamento especificada, além de que está a ser enviados para o arquivo de métrica do Azure Monitor. 

```json
"variables": { 
//add this line       
"storageAccountName": "[concat('storage', uniqueString(resourceGroup().id))]", 
 ```
 
Localize a definição do conjunto de dimensionamento de Máquina Virtual na seção recursos e adicione a secção "identity" para a configuração. Isto garante que o Azure atribui-lhe uma identidade de sistema. Este passo garante que as VMs no conjunto de dimensionamento podem emitir as métricas de convidado sobre si para o Azure Monitor.  

```json
  { 
      "type": "Microsoft.Compute/virtualMachineScaleSets", 
      "name": "[variables('namingInfix')]", 
      "location": "[resourceGroup().location]", 
      "apiVersion": "2017-03-30", 
      //add these lines below
      "identity": { 
           "type": "systemAssigned" 
       }, 
       //end of lines to add
 ```

Na máquina virtual recurso de conjunto de dimensionamento, localize a **virtualMachineProfile** secção. Adicionar um novo perfil chamado **extensionsProfile** para gerir extensões.  


Na **extensionProfile**, adicionar uma nova extensão para o modelo, conforme exibido a **secção VMSS-WAD-extension**.  Esta secção é a extensão de identidade de serviço gerida (MSI) que garante que as métricas que está a ser emitidas são aceites pelo Azure Monitor. O **nome** campo pode conter qualquer nome. 

O código abaixo abaixo a extensão MSI também adiciona a extensão de diagnóstico e a configuração como um recurso de extensão para o recurso de conjunto de dimensionamento de máquina virtual. Fique à vontade Adicionar/remover contadores de desempenho, conforme necessário. 

```json
          "extensionProfile": { 
            "extensions": [ 
            // BEGINNING of added code  
            // Managed service identity   
                { 
                 "name": "VMSS-WAD-extension", 
                 "properties": { 
                       "publisher": "Microsoft.ManagedIdentity", 
                       "type": "ManagedIdentityExtensionForWindows", 
                       "typeHandlerVersion": "1.0", 
                       "autoUpgradeMinorVersion": true, 
                       "settings": { 
                             "port": 50342 
                           }, 
                       "protectedSettings": {} 
                     } 
                                
            }, 
            // add diagnostic extension. (Remove this comment after pasting.)
            { 
              "name": "[concat('VMDiagnosticsVmExt','_vmNodeType0Name')]", 
              "properties": { 
                   "type": "IaaSDiagnostics", 
                   "autoUpgradeMinorVersion": true, 
                   "protectedSettings": { 
                        "storageAccountName": "[variables('storageAccountName')]", 
                        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName')),'2015-05-01-preview').key1]", 
                        "storageAccountEndPoint": "https://core.windows.net/" 
                   }, 
                   "publisher": "Microsoft.Azure.Diagnostics", 
                   "settings": { 
                        "WadCfg": { 
                              "DiagnosticMonitorConfiguration": { 
                                   "overallQuotaInMB": "50000", 
                                   "PerformanceCounters": { 
                                       "scheduledTransferPeriod": "PT1M", 
                                       "sinks": "AzMonSink", 
                                       "PerformanceCounterConfiguration": [ 
                                          { 
              "counterSpecifier": "\\Memory\\% Committed Bytes In Use", 
                                              "sampleRate": "PT15S" 
           }, 
           { 
              "counterSpecifier": "\\Memory\\Available Bytes", 
              "sampleRate": "PT15S" 
           }, 
           { 
              "counterSpecifier": "\\Memory\\Committed Bytes", 
              "sampleRate": "PT15S" 
           } 
                                       ] 
                                 }, 
                                 "EtwProviders": { 
                                       "EtwEventSourceProviderConfiguration": [ 
                                           { 
                                              "provider": "Microsoft-ServiceFabric-Actors", 
                                              "scheduledTransferKeywordFilter": "1", 
                                              "scheduledTransferPeriod": "PT5M", 
                                              "DefaultEvents": { 
                                              "eventDestination": "ServiceFabricReliableActorEventTable" 
                                           } 
                                           }, 
                                           { 
                                              "provider": "Microsoft-ServiceFabric-Services", 
                                              "scheduledTransferPeriod": "PT5M", 
                                              "DefaultEvents": { 
                                                   "eventDestination": "ServiceFabricReliableServiceEventTable" 
                                              } 
                                           } 
                                     ], 
                                     "EtwManifestProviderConfiguration": [ 
                                           { 
                                              "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8", 
                                              "scheduledTransferLogLevelFilter": "Information", 
                                              "scheduledTransferKeywordFilter": "4611686018427387904", 
                                              "scheduledTransferPeriod": "PT5M", 
                                              "DefaultEvents": { 
                                                   "eventDestination": "ServiceFabricSystemEventTable" 
                                              } 
                                          } 
                                     ] 
                               } 
                               }, 
                               "SinksConfig": { 
                                     "Sink": [ 
                                          { 
                                              "name": "AzMonSink", 
                                              "AzureMonitor": {} 
                                          } 
                                      ] 
                               } 
                         }, 
                         "StorageAccount": "[variables('storageAccountName')]" 
                         }, 
                        "typeHandlerVersion": "1.11" 
                  } 
           } 
            ] 
          }
          }
      }
    },
    //end of added code plus a few brackets. Be sure that the number and type of brackets match properly when done. 
    {
      "type": "Microsoft.Insights/autoscaleSettings",
...
```


Adicione um dependsOn para a conta de armazenamento garantir que ele é criado na ordem correta. 
```json
"dependsOn": [ 
"[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]", 
"[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]" 
//add this line below
"[concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]" 
 ```

Crie uma conta de armazenamento, se uma já não é criada no modelo.  
```json
"resources": [
  // add this code    
  {
     "type": "Microsoft.Storage/storageAccounts",
     "name": "[variables('storageAccountName')]",
     "apiVersion": "2015-05-01-preview",
     "location": "[resourceGroup().location]",
     "properties": {
       "accountType": "Standard_LRS"
      }
  },
  // end added code
  { 
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[variables('virtualNetworkName')]",
```

Guarde e feche ambos os ficheiros. 

## <a name="deploy-the-resource-manager-template"></a>Implementar o modelo do Resource Manager 

> [!NOTE]
> Tem de executar a versão 1.5 da extensão de diagnóstico do Azure ou superior e tem "autoUpgradeMinorVersion": propriedade definida como *true* no modelo do Resource Manager.  Azure, em seguida, carrega a extensão correta quando iniciar a VM. Se não tiver estas definições no seu modelo, alterá-los e Reimplementar o modelo. 


Para implementar o modelo do Resource Manager, iremos tirar partido do Azure PowerShell.  

1. Iniciar o PowerShell 
1. Inicie sessão no Azure com `Login-AzureRmAccount`
1. Obter a lista de subscrições através de `Get-AzureRmSubscription`
1. Definir a subscrição que irá que criar/atualizar a máquina virtual numa 

   ```PowerShell
   Select-AzureRmSubscription -SubscriptionName "<Name of the subscription>" 
   ```
1. Criar um novo grupo de recursos para a VM que está a ser implementada, execute o comando abaixo 

   ```PowerShell
    New-AzureRmResourceGroup -Name "VMSSWADtestGrp" -Location "<Azure Region>" 
   ```

   > [!NOTE]  
   > Lembre-se utilizar uma região do Azure que está ativada para métricas personalizadas. Ver 
 
1. Execute os seguintes comandos para implementar a VM com o  
   > [!NOTE] 
   > Se quiser atualizar um conjunto de dimensionamento existente, basta adicionar *-modo Incremental* ao final do comando seguinte. 
 
   ```PowerShell
   New-AzureRmResourceGroupDeployment -Name "VMSSWADTest" -ResourceGroupName "VMSSWADtestGrp" -TemplateFile "<File path of your azuredeploy.JSON file>" -TemplateParameterFile "<File path of your azuredeploy.parameters.JSON file>"  
   ```

1. Assim que for concluída com êxito a implementação deve ser capaz de encontrar o conjunto de dimensionamento no portal do Azure e deve ser a emitir métricas para o Azure Monitor. 

   > [!NOTE] 
   > Poderão ocorrer erros em todo o vmSkuSize selecionado. Se isto acontecer, regresse ao seu ficheiro azuredeploy JSON e atualize o valor predefinido do parâmetro vmSkuSize. Neste caso, é recomendável tentar "Standard_DS1_v2"). 


## <a name="chart-your-metrics"></a>As suas métricas do gráfico 

1. Iniciar sessão no portal do Azure 

1. No menu da esquerda, clique em **Monitor** 

1. Na página do Monitor, clique em **métricas**. 

   ![Página de métricas](./media/metrics-store-custom-rest-api/metrics.png) 

1. Alterar o período de agregação para **últimos 30 minutos**.  

1. No recurso da lista pendente, selecione o conjunto de dimensionamento de máquina virtual que acabou de criar.  

1. Nos espaços de nome pendente, selecione **azure.vm.windows.guest** 

1. As métricas de lista pendente, selecione **memória\%Bytes dedicados em utilização**.  

Em seguida, também é possível usar as dimensões nesta métrica para esta métrica de gráfico para uma VM específica no conjunto de dimensionamento ou a representar cada VM no conjunto de dimensionamento. 



## <a name="next-steps"></a>Passos Seguintes
- Saiba mais sobre [métricas personalizadas](metrics-custom-overview.md).

