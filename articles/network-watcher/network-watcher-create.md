---
title: Criar uma instância de observador de rede do Azure | Microsoft Docs
description: Saiba como ativar o observador de rede numa região do Azure.
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: b1314119-0b87-4f4d-b44c-2c4d0547fb76
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 9d3e579cd58bc6c7d67b29998ea5a48a65548b0a
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30904005"
---
# <a name="create-an-azure-network-watcher-instance"></a>Criar uma instância de observador de rede do Azure

Observador de rede é um serviço regional que lhe permite monitorizar e diagnosticar condições a um nível de cenário rede, para e do Azure. Monitorização ao nível do cenário permite-lhe diagnosticar problemas de uma vista de nível de rede ponto a ponto. Diagnóstico de rede e ferramentas de visualização disponíveis com o observador de rede ajudam-na compreender, diagnosticar e obter informações sobre a sua rede no Azure.

## <a name="create-a-network-watcher-in-the-portal"></a>Criar um observador de rede no portal

Navegue para **todos os serviços** > **redes** > **observador de rede**. Pode selecionar todas as subscrições que pretende ativar o observador de rede para. Esta ação cria um observador de rede em cada região que está disponível.

![criar um observador de rede](./media/network-watcher-create/figure1.png)

Quando ativar o observador de rede através do portal, o nome da instância do observador de rede é automaticamente definido para *NetworkWatcher_region_name* onde *region_name* corresponde à região do Azure onde a instância está ativada. Por exemplo, um observador de rede ativada na região Central EUA oeste com o nome *NetworkWatcher_westcentralus*.

A instância de observador de rede é criada automaticamente um grupo de recursos com o nome *NetworkWatcherRG*. O grupo de recursos é criado se já existir.

Se pretender personalizar o nome de uma instância de observador de rede e o grupo de recursos está a ser colocado, pode utilizar o Powershell, a CLI do Azure, a REST API ou métodos ARMClient descritos nas secções que se seguem. Cada opção, o grupo de recursos tem de existir antes de criar um observador de rede no mesmo.  

## <a name="create-a-network-watcher-with-powershell"></a>Criar um observador de rede com o PowerShell

Para criar uma instância do observador de rede, execute o seguinte exemplo:

```powershell
New-AzureRmNetworkWatcher -Name "NetworkWatcher_westcentralus" -ResourceGroupName "NetworkWatcherRG" -Location "West Central US"
```

## <a name="create-a-network-watcher-with-the-azure-cli"></a>Criar um observador de rede com a CLI do Azure

Para criar uma instância do observador de rede, execute o seguinte exemplo:

```azurecli
az network watcher configure --resource-group NetworkWatcherRG --locations westcentralus --enabled
```

## <a name="create-a-network-watcher-with-the-rest-api"></a>Criar um observador de rede com a API REST

O ARMclient é utilizada para chamar a API de REST utilizando o PowerShell. O ARMClient for encontrado no chocolatey em [ARMClient no Chocolatey](https://chocolatey.org/packages/ARMClient)

### <a name="log-in-with-armclient"></a>Iniciar sessão com ARMClient

```powerShell
armclient login
```

### <a name="create-the-network-watcher"></a>Criar o observador de rede

```powershell
$subscriptionId = '<subscription id>'
$networkWatcherName = '<name of network watcher>'
$resourceGroupName = '<resource group name>'
$apiversion = "2016-09-01"
$requestBody = @"
{
'location': 'West Central US'
}
"@

armclient put "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}?api-version=${api-version}" $requestBody
```

## <a name="next-steps"></a>Passos Seguintes

Agora que tem uma instância do observador de rede, saiba mais sobre as funcionalidades disponíveis:

* [Topologia](network-watcher-topology-overview.md)
* [Captura de pacotes](network-watcher-packet-capture-overview.md)
* [Verificação do fluxo de IP](network-watcher-ip-flow-verify-overview.md)
* [Salto seguinte](network-watcher-next-hop-overview.md)
* [Vista do grupo de segurança](network-watcher-security-group-view-overview.md)
* [Registo de fluxo do NSG](network-watcher-nsg-flow-logging-overview.md)
* [Resolução de problemas de Gateway de rede virtual](network-watcher-troubleshoot-overview.md)

Assim que estiver uma instância de observador de rede, pode ativar a captura de pacotes das máquinas virtuais. Para saber como, consulte [criar uma captura de pacotes accionadas alerta](network-watcher-alert-triggered-packet-capture.md)
