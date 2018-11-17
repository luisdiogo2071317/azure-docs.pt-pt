---
title: Gerir registos de fluxo do grupo de segurança da rede com o observador de rede do Azure - PowerShell | Documentos da Microsoft
description: Esta página explica como gerir registos de fluxo do grupo de segurança da rede de mensagens em fila no observador de rede do Azure com o PowerShell
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: 2dfc3112-8294-4357-b2f8-f81840da67d3
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 752370564c52513d59e99b18d5343b0575900463
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/16/2018
ms.locfileid: "51819360"
---
# <a name="configuring-network-security-group-flow-logs-with-powershell"></a>Configurar registos de fluxo do grupo de segurança da rede com o PowerShell

> [!div class="op_single_selector"]
> - [Portal do Azure](network-watcher-nsg-flow-logging-portal.md)
> - [PowerShell](network-watcher-nsg-flow-logging-powershell.md)
> - [CLI do Azure](network-watcher-nsg-flow-logging-cli.md)
> - [API REST](network-watcher-nsg-flow-logging-rest.md)

Os registos de fluxo do grupo de segurança de rede são uma funcionalidade do observador de rede permite-lhe ver informações sobre o tráfego IP de entrada e de saída através de um grupo de segurança de rede. Estes registos de fluxo são escritos no formato json e Mostrar fluxos de saída e entrados numa base por regra, o NIC que o fluxo de mensagens em fila aplica-se a informações de 5 cadeias de identificação sobre o fluxo (IP de origem/destino, porta de origem/destino, protocolo), e se o tráfego foi permitido ou negado.

> [!NOTE] 
> Versão de registos de fluxo 2 estão apenas disponíveis no Central região E.u.a. oeste. A configuração está disponível através do Portal do Azure e a REST API. Ativar a versão 2 registos numa região não suportada resultará nos registos de versão 1 debitados à sua conta de armazenamento.

## <a name="register-insights-provider"></a>Registar o fornecedor do Insights

Para que o fluxo de registo para trabalhar com êxito, o **Microsoft. insights** fornecedor tem de estar registado. Se não tiver a certeza se o **Microsoft. insights** fornecedor está registado, execute o seguinte script.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Insights
```

## <a name="enable-network-security-group-flow-logs"></a>Registos de fluxo de ativar o grupo de segurança de rede

O comando para ativar os registos de fluxo é mostrado no exemplo a seguir:

```powershell
$NW = Get-AzurermNetworkWatcher -ResourceGroupName NetworkWatcherRg -Name NetworkWatcher_westcentralus
$nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName nsgRG -Name nsgName
$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName StorageRG -Name contosostorage123
Get-AzureRmNetworkWatcherFlowLogStatus -NetworkWatcher $NW -TargetResourceId $nsg.Id
Set-AzureRmNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $true
```

A conta de armazenamento especificada não pode ter regras de rede configuradas para o mesmo que restringem o acesso de rede apenas aos serviços da Microsoft ou as redes virtuais específicas. A conta de armazenamento pode ser o mesmo, ou uma subscrição do Azure diferente, que o NSG que ativar o registo de fluxo para. Se usar a subscrições diferentes, têm ambos de ser associados ao mesmo inquilino do Azure Active Directory. A conta que utiliza para cada subscrição tem de ter o [as permissões necessárias](required-rbac-permissions.md).

## <a name="disable-network-security-group-flow-logs"></a>Registos de fluxo de desativar o grupo de segurança de rede

Utilize o exemplo seguinte para desativar os registos de fluxo:

```powershell
Set-AzureRmNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $false
```

## <a name="download-a-flow-log"></a>Transferir o registo de um fluxo

A localização de armazenamento de um registo de fluxo é definida durante a criação. Uma ferramenta conveniente para aceder a estes registos de fluxo guardados para uma conta de armazenamento é o Explorador de armazenamento do Microsoft Azure, que pode ser baixado aqui:  http://storageexplorer.com/

Se for especificada uma conta de armazenamento, os arquivos de captura de pacotes são guardados para uma conta de armazenamento na seguinte localização:

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId=/SUBSCRIPTIONS/{subscriptionID}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/{nsgName}/y={year}/m={month}/d={day}/h={hour}/m=00/macAddress={macAddress}/PT1H.json
```

Para obter informações sobre a estrutura do registo de visite [descrição geral do fluxo do grupo de segurança da rede registo](network-watcher-nsg-flow-logging-overview.md)

## <a name="next-steps"></a>Próximos Passos

Saiba como [Visualize os seus registos de fluxo NSG com o PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md)

Saiba como [Visualize os seus registos de fluxo NSG com ferramentas de código aberto](network-watcher-visualize-nsg-flow-logs-open-source-tools.md)
