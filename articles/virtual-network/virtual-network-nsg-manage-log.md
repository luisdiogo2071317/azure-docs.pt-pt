---
title: Eventos de grupo de segurança de rede e a regra de contador de registos de diagnóstico do Azure | Microsoft Docs
description: Saiba como ativar o evento e regra de contador os registos de diagnóstico para um grupo de segurança de rede do Azure.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 2e699078-043f-48bd-8aa8-b011a32d98ca
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/04/2018
ms.author: jdial
ms.openlocfilehash: c3f4a64c9e11d17899987bbe818506f61c415e3f
ms.sourcegitcommit: 4f9fa86166b50e86cf089f31d85e16155b60559f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34757064"
---
# <a name="diagnostic-logging-for-a-network-security-group"></a>Registo de diagnóstico para um grupo de segurança de rede

Um grupo de segurança de rede (NSG) inclui as regras que permitem ou negam o tráfego para uma sub-rede de rede virtual, a interface de rede ou ambos. Quando ativar o diagnóstico de registo para um NSG, pode iniciar sessão seguintes categorias de informações:

* **Evento:** forem registadas entradas para o NSG regras são aplicadas a VMs, com base no endereço MAC. O estado para estas regras é recolhido a cada 60 segundos.
* **Contador de regra:** contém entradas quantas vezes cada NSG regra é aplicada a negar e permitir o tráfego.

Os registos de diagnóstico só estão disponíveis para os NSGs implementados através do modelo de implementação Azure Resource Manager. Não é possível ativar o registo de diagnóstico para os NSGs implementadas através do modelo de implementação clássica. Para uma melhor compreensão dos dois modelos, consulte [modelos de implementação do Azure de compreender](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Registo de diagnóstico está ativado em separado para *cada* NSG que pretende recolher dados de diagnóstico para. Se estiver interessado em operacional, ou atividade, os registos em vez disso, consulte Azure [registo de atividade](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="enable-logging"></a>Ativar registo

Pode utilizar o [Portal do Azure](#azure-portal), ou [PowerShell](#powershell), para ativar o registo de diagnóstico.

### <a name="azure-portal"></a>Portal do Azure

1. Inicie sessão no [portal](https://portal.azure.com).
2. Selecione **todos os serviços**, em seguida, escreva *grupos de segurança de rede*. Quando **grupos de segurança de rede** aparecer nos resultados da pesquisa, selecione-o.
3. Selecione o NSG que pretende ativar o registo de.
4. Em **monitorização**, selecione **registos de diagnóstico**e, em seguida, selecione **ative os diagnósticos**, conforme mostrado na imagem seguinte:
 
    ![Ativar os diagnósticos](./media/virtual-network-nsg-manage-log/turn-on-diagnostics.png)

5. Em **as definições de diagnóstico**, introduzir, ou selecione as seguintes informações e, em seguida, selecione **guardar**:

    | Definição                                                                                     | Valor                                                          |
    | ---------                                                                                   |---------                                                       |
    | Nome                                                                                        | Nome da sua escolha.  Por exemplo: *myNsgDiagnostics*      |
    | **Arquivo para uma conta de armazenamento**, **fluxo para um hub de eventos**, e **enviar ao Log Analytics** | Pode selecionar destinos tantos como escolher. Para obter mais informações sobre cada um, consulte [destinos de registo](#log-destinations).                                                                                                                                           |
    | REGISTO                                                                                         | Selecione um ou ambos os categorias de registo. Para saber mais sobre os dados registados para cada categoria, consulte [categorias de registo](#log-categories).                                                                                                                                             |
6. Ver e analisar os registos. Para obter mais informações, consulte [ver e analisar registos](#view-and-analyze-logs).

### <a name="powershell"></a>PowerShell

Pode executar os comandos que se seguem na [Shell de nuvem do Azure](https://shell.azure.com/powershell), ou através da execução do PowerShell do seu computador. A Shell de nuvem do Azure é uma shell interativa livre. Tem as ferramentas comuns do Azure pré-instaladas e configuradas para utilização com a sua conta. Se executar o PowerShell a partir do seu computador, terá do *AzureRM* módulo do PowerShell, versão 6.1.1 ou posterior. Executar `Get-Module -ListAvailable AzureRM` no seu computador, para localizar a versão instalada. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente do PowerShell, também terá de executar `Login-AzureRmAccount` iniciar sessão no Azure com uma conta que tenha o [as permissões necessárias](virtual-network-network-interface.md#permissions)].

Para ativar o registo de diagnóstico, terá do Id de um NSG existente. Se não tiver um NSG existente, pode criar uma com [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup).

Obter o grupo de segurança de rede que pretende ativar o diagnóstico para o registo com [Get-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/get-azurermnetworksecuritygroup). Por exemplo, para obter um NSG chamada *myNsg* que existe num grupo de recursos com o nome *myResourceGroup*, introduza o seguinte comando:

```azurepowershell-interactive
$Nsg=Get-AzureRmNetworkSecurityGroup `
  -Name myNsg `
  -ResourceGroupName myResourceGroup
```

É possível escrever os registos de diagnóstico para três tipos de destino. Para obter mais informações, consulte [destinos de registo](#log-destinations). Neste artigo, os registos são enviados para o *Log Analytics* destino, como um exemplo. Obter uma área de trabalho de análise de registos existente com [Get-AzureRmOperationalInsightsWorkspace](/powershell/module/azurerm.operationalinsights/get-azurermoperationalinsightsworkspace). Por exemplo, para obter uma área de trabalho existente com o nome *myLaWorkspace* num grupo de recursos denominado *LaWorkspaces*, introduza o seguinte comando:

```azurepowershell-interactive
$Oms=Get-AzureRmOperationalInsightsWorkspace `
  -ResourceGroupName LaWorkspaces `
  -Name myLaWorkspace
```

Se não tiver uma área de trabalho existente, pode criar uma com [New-AzureRmOperationalInsightsWorkspace](/powershell/module/azurerm.operationalinsights/new-azurermoperationalinsightsworkspace).

Existem duas categorias de registo, pode ativar os registos para. Para obter mais informações, consulte [categorias de registo](#log-categories). Ativar o registo de diagnóstico para o NSG aos [Set-AzureRmDiagnosticSetting](/powershell/module/azurerm.insights/set-azurermdiagnosticsetting). O exemplo seguinte regista dados de categoria de evento e contador para a área de trabalho para um NSG, utilizando os IDs para o NSG e a área de trabalho que obteve anteriormente:

```azurepowershell-interactive
Set-AzureRmDiagnosticSetting `
  -ResourceId $Nsg.Id `
  -WorkspaceId $Oms.ResourceId `
  -Enabled $true
```

Se pretender apenas registar dados para a categoria ou a outra, em vez de ambos, adicione o `-Categories` opção para o comando anterior, seguido de *NetworkSecurityGroupEvent* ou *NetworkSecurityGroupRuleCounter*. Se pretender iniciar a outro [destino](#log-destinations) a uma área de trabalho de análise de registos, utilize os parâmetros adequados para um Azure [conta de armazenamento](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ou [Hub de eventos](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Ver e analisar os registos. Para obter mais informações, consulte [ver e analisar registos](#view-and-analyze-logs).

## <a name="log-destinations"></a>Destinos de registo

Dados de diagnóstico podem ser:
- [Escrito para uma conta de armazenamento do Azure](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json), de inspeção de auditoria ou manual. Pode especificar o período de retenção (em dias) utilizando as definições de diagnóstico do recurso.
- [Transmissão em fluxo para um hub de eventos](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) para ingestão por um serviço de terceiros ou solução de análise personalizada, tal como o Power BI.
- [Escrito para análise de registos do Azure](../log-analytics/log-analytics-azure-storage.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-diagnostics-direct-to-log-analytics).

## <a name="log-categories"></a>Categorias de registo

Dados formatado em JSON foi escritos para as categorias de registo seguinte:

### <a name="event"></a>Evento

O registo de eventos contém informações sobre os quais as regras do NSG são aplicadas a VMs, com base no endereço MAC. Os dados de exemplo seguintes são registados para cada evento:

```json
{
    "time": "[DATE-TIME]",
    "systemId": "[ID]",
    "category": "NetworkSecurityGroupEvent",
    "resourceId": "/SUBSCRIPTIONS/[SUBSCRIPTION-ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG-NAME]",
    "operationName": "NetworkSecurityGroupEvents",
    "properties": {
        "vnetResourceGuid":"[ID]",
        "subnetPrefix":"192.168.1.0/24",
        "macAddress":"00-0D-3A-92-6A-7C",
        "primaryIPv4Address":"192.168.1.4",
        "ruleName":"[SECURITY RULE NAME]",
        "direction":"In",
        "priority":1000,
        "type":"allow",
        "conditions":{
            "protocols":"6",
            "destinationPortRange":"[PORT RANGE]",
            "sourcePortRange":"0-65535",
            "sourceIP":"0.0.0.0/0",
            "destinationIP":"0.0.0.0/0"
            }
        }
}
```

### <a name="rule-counter"></a>Contador de regras

O registo de contador de regra contém informações sobre cada regra aplicada aos recursos. Os seguintes dados de exemplo são registados sempre que uma regra é aplicada:

```json
{
    "time": "[DATE-TIME]",
    "systemId": "[ID]",
    "category": "NetworkSecurityGroupRuleCounter",
    "resourceId": "/SUBSCRIPTIONS/[SUBSCRIPTION ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG-NAME]",
    "operationName": "NetworkSecurityGroupCounters",
    "properties": {
        "vnetResourceGuid":"[ID]",
        "subnetPrefix":"192.168.1.0/24",
        "macAddress":"00-0D-3A-92-6A-7C",
        "primaryIPv4Address":"192.168.1.4",
        "ruleName":"[SECURITY RULE NAME]",
        "direction":"In",
        "type":"allow",
        "matchedConnections":125
        }
}
```

> [!NOTE]
> O endereço IP de origem para a comunicação não é registado. Pode ativar [o registo de fluxo NSG](../network-watcher/network-watcher-nsg-flow-logging-portal.md) para um NSG no entanto, que regista todas as informações do contador de regras, bem como o endereço IP de origem que iniciou a comunicação. Os dados de registo de fluxo do NSG são escritos numa conta de Armazenamento do Microsoft Azure. Pode analisar os dados com o [análise de tráfego](../network-watcher/traffic-analytics.md) capacidade do observador de rede do Azure.

## <a name="view-and-analyze-logs"></a>Ver e analisar registos

Para saber como ver dados de diagnóstico de registo, consulte [descrição geral de registos de diagnóstico do Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Se enviar dados de diagnóstico para:
- **Análise de registo**: pode utilizar o [análises do grupo de segurança de rede](../log-analytics/log-analytics-azure-networking-analytics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-network-security-group-analytics-solution-in-log-analytics
) solução para insights avançadas. A solução fornece visualizações para regras NSG que permitem ou negam o tráfego, por endereço MAC, da interface de rede numa máquina virtual.
- **Conta de armazenamento do Azure**: dados são escritos num ficheiro PT1H.json. Pode encontrar o:
    - Registo de eventos no seguinte caminho: `insights-logs-networksecuritygroupevent/resourceId=/SUBSCRIPTIONS/[ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME-FOR-NSG]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG NAME]/y=[YEAR]/m=[MONTH/d=[DAY]/h=[HOUR]/m=[MINUTE]`
    - Registo de contador de regra no seguinte caminho: `insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/[ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME-FOR-NSG]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG NAME]/y=[YEAR]/m=[MONTH/d=[DAY]/h=[HOUR]/m=[MINUTE]`

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre [registo de atividade](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json), anteriormente conhecido como auditoria ou registos operacionais. Registo de atividade está ativado por predefinição para os NSGs criados através de um modelo de implementação do Azure. Para determinar as operações foram concluídas no NSGs no registo de atividade, procure entradas que contêm os seguintes tipos de recursos:
    - Microsoft.ClassicNetwork/networkSecurityGroups
    - Microsoft.ClassicNetwork/networkSecurityGroups/securityRules
    - Microsoft.Network/networkSecurityGroups
    - Microsoft.Network/networkSecurityGroups/securityRules
- Para saber como registar informações de diagnóstico, para incluir o endereço IP de origem para cada fluxo, consulte [o registo de fluxo NSG](../network-watcher/network-watcher-nsg-flow-logging-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json).