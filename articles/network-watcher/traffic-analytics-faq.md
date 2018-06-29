---
title: Análise de tráfego do Azure perguntas mais frequentes | Microsoft Docs
description: Obtenha respostas a algumas perguntas mais frequentes sobre a análise de tráfego.
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/08/2018
ms.author: jdial
ms.openlocfilehash: a4b87d92751c84d96bc70915d16adae7943c145e
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37062882"
---
# <a name="traffic-analytics-frequently-asked-questions"></a>Análise de tráfego perguntas mais frequentes

Este artigo recolhe num único local muitas das perguntas mais frequentes sobre a análise de tráfego na observador de rede do Azure.

## <a name="what-are-the-prerequisites-to-use-traffic-analytics"></a>Quais são os pré-requisitos para utilizar a análise de tráfego?

Análise de tráfego necessita dos seguintes pré-requisitos:

- Uma subscrição de observador de rede ativada.
- Registos de fluxo do grupo de segurança de rede (NSG) ativados para os NSGs que pretende monitorizar.
- Uma conta de armazenamento do Azure, para armazenar em bruto flog registos.
- Uma área de Log Analytics do Azure, com acesso de escrita e leitura.

A conta tem de cumprir um dos seguintes para ativar a análise de tráfego:

- A conta tem de ser atribuída a uma das seguintes funções ao nível da subscrição: administrador da conta, o administrador de serviço ou coadministrador.
- A conta tem de ter qualquer uma das seguintes funções de controlo (RBAC) de acesso baseado em funções no âmbito de subscrição: proprietário, Contribuidor, leitor ou contribuinte de rede.
- Se a sua conta não está atribuída a uma das funções listadas anteriormente, tem de ser atribuído a uma função personalizada que é atribuída as seguintes ações, ao nível da subscrição.
            
    - Microsoft.Network/applicationGateways/read
    - Microsoft.Network/connections/read
    - Microsoft.Network/loadBalancers/read 
    - Microsoft.Network/localNetworkGateways/read 
    - Microsoft.Network/networkInterfaces/read 
    - Microsoft.Network/networkSecurityGroups/read 
    - Microsoft.Network/publicIPAddresses/read
    - Microsoft.Network/routeTables/read
    - Microsoft.Network/virtualNetworkGateways/read 
    - Microsoft.Network/virtualNetworks/read
        
Para verificar funções atribuídas a um utilizador para uma subscrição:

1. Inicie sessão no Azure utilizando **Login-AzureRmAccount**. 

2. Selecione a subscrição necessária utilizando **Select-AzureRmSubscription**. 

3. Para listar todas as funções que estão atribuídas a um utilizador especificado, utilize **Get-AzureRmRoleAssignment - SignInName [e-mail do utilizador] - IncludeClassicAdministrators**. 

Se não está a ver resultados, contacte o administrador de subscrição respetivos obter acesso para executar os comandos. Para obter mais detalhes, consulte [gerir o controlo de acesso baseado em funções com o Azure PowerShell](https://docs.microsoft.com/en-us/azure/role-based-access-control/role-assignments-powershell).


## <a name="in-which-azure-regions-are-traffic-analytics-available"></a>Na qual regiões estão análise de tráfego disponíveis?

Pode utilizar a análise de tráfego para os NSGs em qualquer um dos seguintes regiões suportadas: Central EUA oeste, EUA leste, EUA Leste 2, Norte Central nos, Sul Central dos EUA, EUA Central, EUA oeste, EUA oeste 2, Europa Ocidental, Europa do Norte, RU oeste, sul do RU, leste da Austrália, Sudeste da Austrália e Sudeste asiático. A área de trabalho de análise de registos tem de existir o Central EUA oeste, EUA leste, Europa Ocidental, sul do RU, Sudeste da Austrália ou a região Sudeste asiático.

## <a name="can-the-nsgs-i-enable-flow-logs-for-be-in-different-regions-than-my-workspace"></a>Pode NSGs ativar o fluxo de registos para estar em regiões diferentes a minha área de trabalho?

Sim, estes NSGs podem ser em regiões diferentes à sua área de trabalho de análise de registos.

## <a name="can-multiple-nsgs-be-configured-within-a-single-workspace"></a>Podem ser configurados vários NSGs dentro de uma única área de trabalho?

Sim.

## <a name="can-i-use-an-existing-workspace"></a>Pode utilizar uma área de trabalho existente?

Sim. Se selecionou uma área de trabalho existente, certifique-se de que foi migrada para o novo idioma de consulta. Se não pretender atualizar a área de trabalho, terá de criar um novo. Para obter mais informações sobre o novo idioma de consulta, consulte [Log Analytics do Azure atualizar para a nova pesquisa de registo](../log-analytics/log-analytics-log-search-upgrade.md).

## <a name="can-my-azure-storage-account-be-in-one-subscription-and-my-operations-management-suite-workspace-be-in-a-different-subscription"></a>Pode ser minha conta do Storage do Azure numa subscrição e a minha área de trabalho do Operations Management Suite ser numa subscrição diferente?

Sim, a conta do Storage do Azure pode ser uma subscrição e a área de trabalho do Operations Management Suite pode ser numa subscrição diferente.

## <a name="can-i-store-raw-logs-in-a-different-subscription"></a>Pode armazenar os registos não processados numa subscrição diferente?

Não. Pode armazenar os registos não processados em qualquer conta de armazenamento onde um NSG é ativado para os registos de fluxo. No entanto, a conta de armazenamento e os registos em bruto tem de ser na mesma subscrição e região.

## <a name="what-if-i-cant-configure-an-nsg-for-traffic-analytics-due-to-a-not-found-error"></a>E se não é possível configurar um NSG para análise de tráfego devido a um erro "Não encontrada"?

Selecione uma região suportada. Se selecionar uma região não suportado, receberá um erro "Não encontrada". Regiões suportadas são listadas anteriormente neste artigo.

## <a name="what-if-i-am-getting-the-status-failed-to-load-under-the-nsg-flow-logs-page"></a>E se posso estou a obter o estado, "Falha ao carregar," sob a página de registos de fluxo NSG?

O fornecedor de insights tem de estar registado para o fluxo de registo para funcionar corretamente. Se não tem a certeza se o fornecedor de insights está registado para a sua subscrição, substitua *xxxxx-xxxxx-xxxxxx-xxxx* no comando seguinte e execute os seguintes comandos a partir do PowerShell:

```powershell-interactive
**Select-AzureRmSubscription** -SubscriptionId xxxxx-xxxxx-xxxxxx-xxxx
**Register-AzureRmResourceProvider** -ProviderNamespace Microsoft.Insights
```

## <a name="i-have-configured-the-solution-why-am-i-not-seeing-anything-on-the-dashboard"></a>Posso ter configurado a solução. Por que razão estou não ver nada no dashboard?

O dashboard pode demorar até 30 minutos a aparecer na primeira vez. A solução deve primeiro agregar dados suficientes para o mesmo derivar insights significativos. Em seguida, gera relatórios. 

## <a name="what-if-i-get-this-message-we-could-not-find-any-data-in-this-workspace-for-selected-time-interval-try-changing-the-time-interval-or-select-a-different-workspace"></a>E se receber esta mensagem: "não foi possível localizar quaisquer dados nesta área de trabalho para o intervalo de tempo selecionado. Experimente alterar o intervalo de tempo ou selecione uma área de trabalho diferentes. "?

Experimente as seguintes opções:
- Altere o intervalo de tempo na barra superior.
- Selecione uma área de trabalho de análise de registos diferente na barra superior.
- Tente aceder a análise de tráfego após 30 minutos, se tiver sido recentemente ativado.
    
Se os problemas persistirem, criar problemas no [fórum de voz do utilizador](https://feedback.azure.com/forums/217313-networking?category_id=195844).

## <a name="what-if-i-get-this-message-analyzing-your-nsg-flow-logs-for-the-first-time-this-process-may-take-20-30-minutes-to-complete-check-back-after-some-time-2-if-the-above-step-doesnt-work-and-your-workspace-is-under-the-free-sku-then-check-your-workspace-usage-here-to-validate-over-quota-else-refer-to-faqs-for-further-information"></a>E se receber esta mensagem: "analisar o NSG fluir registos pela primeira vez. Este processo pode demorar 20-30 minutos a concluir. Verifique novamente daqui a algum tempo. 2) se o passo anterior não funciona e a sua área de trabalho está sob o SKU livre, em seguida, certifique-se a utilização da área de trabalho aqui para validar através de quota, ou consulte a perguntas mais frequentes para obter mais informações. "?

Poderá ver esta mensagem porque:
- Análise de tráfego recentemente foi ativada e poderá não ainda tem agregar dados suficientes para o mesmo derivar insights significativos.
- Está a utilizar a versão gratuita da área de trabalho do Operations Management Suite e excedia os limites de quota. Poderá ter de utilizar uma área de trabalho com uma maior capacidade.
    
Se os problemas persistirem, criar problemas no [fórum de voz do utilizador](https://feedback.azure.com/forums/217313-networking?category_id=195844).
    
## <a name="what-if-i-get-this-message-looks-like-we-have-resources-data-topology-and-no-flows-information-meanwhile-click-here-to-see-resources-data-and-refer-to-faqs-for-further-information"></a>E se receber esta mensagem: "temos e não existem informações de fluxos de dados de recursos (topologia). Entretanto, clique aqui para ver os dados de recursos e consulte Perguntas mais frequentes para obter mais informações. "?

Está a ver as informações de recursos no dashboard; No entanto, não existem estatísticas relacionadas com o fluxo estão presentes. Dados poderão não estar presentes devido a não existem fluxos de comunicação entre os recursos. Aguarde durante 60 minutos e Reverificar estado. Se o problema persistir, e tiver a certeza de que existem fluxos de comunicação entre os recursos, criar problemas no [fórum de voz do utilizador](https://feedback.azure.com/forums/217313-networking?category_id=195844).

## <a name="can-i-configure-traffic-analytics-using-powershell-or-an-azure-resource-manager-template-or-client"></a>Posso configurar a análise de tráfego através do PowerShell ou um modelo Azure Resource Manager ou do cliente?

Pode configurar a análise de tráfego através do Windows PowerShell a partir da versão 6.2.1 em diante. Para configurar o registo de fluxo e análise de tráfego para um NSG específico utilizando o cmdlet do conjunto, consulte [conjunto AzureRmNetworkWatcherConfigFlowLog](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermnetworkwatcherconfigflowlog?view=azurermps-6.3.0). Para obter o registo de fluxo e o estado de análise de tráfego para um NSG específico, consulte [Get-AzureRmNetworkWatcherFlowLogStatus](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermnetworkwatcherflowlogstatus?view=azurermps-6.3.0).

Atualmente, não é possível utilizar um modelo Azure Resource Manager para configurar a análise de tráfego.

Para configurar a análise de tráfego através da utilização de um cliente do Azure Resource Manager, consulte os exemplos seguintes.

**Exemplo de cmdlet do set:**
```
#Requestbody parameters
$TAtargetUri ="/subscriptions/<NSG subscription id>/resourceGroups/<NSG resource group name>/providers/Microsoft.Network/networkSecurityGroups/<name of NSG>"
$TAstorageId = "/subscriptions/<storage subscription id>/resourcegroups/<storage resource group name> /providers/microsoft.storage/storageaccounts/<storage account name>"
$networkWatcherResourceGroupName = "<network watcher resource group name>"
$networkWatcherName = "<network watcher name>"

$requestBody = 
@"
{
    'targetResourceId': '${TAtargetUri}',
    'properties': 
    {
        'storageId': '${TAstorageId}',
        'enabled': '<true to enable flow log or false to disable flow log>',
        'retentionPolicy' : 
        {
            days: <enter number of days like to retail flow logs in storage account>,
            enabled: <true to enable retention or false to disable retention>
        }
    },
    'flowAnalyticsConfiguration':
    {
                'networkWatcherFlowAnalyticsConfiguration':
      {
        'enabled':,<true to enable traffic analytics or false to disable traffic analytics>
        'workspaceId':'bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb',
        'workspaceRegion':'<workspace region>',
        'workspaceResourceId':'/subscriptions/<workspace subscription id>/resourcegroups/<workspace resource group name>/providers/microsoft.operationalinsights/workspaces/<workspace name>'
        
      }

    }
}
"@
$apiversion = "2016-09-01"

armclient login
armclient post "https://management.azure.com/subscriptions/<NSG subscription id>/resourceGroups/<network watcher resource group name>/providers/Microsoft.Network/networkWatchers/<network watcher name>/configureFlowlog?api-version=${apiversion}" $requestBody
```
**Obter o exemplo de cmdlet:**
```
#Requestbody parameters
$TAtargetUri ="/subscriptions/<NSG subscription id>/resourceGroups/<NSG resource group name>/providers/Microsoft.Network/networkSecurityGroups/<NSG name>"


$requestBody = 
@"
{
    'targetResourceId': '${TAtargetUri}'
}
“@


armclient login
armclient post "https://management.azure.com/subscriptions/<NSG subscription id>/resourceGroups/<network watcher resource group name>/providers/Microsoft.Network/networkWatchers/<network watcher name>/queryFlowLogStatus?api-version=${apiversion}" $requestBody
```



## <a name="how-is-traffic-analytics-priced"></a>Como a análise de tráfego tem um preço?

Análise de tráfego é limitado. A medição baseia-se no processamento de fluxo de dados de registo pelo serviço, e armazenar resultante avançada registos numa área de trabalho de análise de registos. Para obter mais detalhes, consulte o [plano de preços](https://azure.microsoft.com/en-us/pricing/details/network-watcher/). 

## <a name="how-can-i-navigate-by-using-the-keyboard-in-the-geo-map-view"></a>Como pode navegar utilizando o teclado na vista de mapa de georreplicação?

A página de mapa de georreplicação contém duas secções principais:
    
- **Faixa**: A faixa na parte superior do mapa georreplicação fornece botões para selecionar os filtros de distribuição de tráfego (por exemplo, implementação, o tráfego de países e maliciosos). Quando seleciona um botão, o respetivo filtro é aplicado no mapa. Por exemplo, se selecionar o botão de Active Directory, o mapa realça os datacenters de Active Directory na sua implementação.
- **Mapa**: abaixo a faixa, a secção de mapa mostra a distribuição de tráfego entre os datacenters do Azure e países.
    
### <a name="keyboard-navigation-on-the-banner"></a>Navegação do teclado na faixa
    
- Por predefinição, a seleção na página de mapa de georreplicação para a faixa é o filtro de "Controladores de domínio do Azure".
- Para mover para outro filtro, utilize o `Tab` ou `Right arrow` chave. Para mover trás, utilize o `Shift+Tab` ou `Left arrow` chave. Navegação de avanço é esquerda para a direita, seguido de parte superior para parte inferior.
- Prima `Enter` ou `Down` tecla de seta para aplicar o filtro selecionado. Um ou vários nós na secção mapa com base na seleção de filtro e a implementação, são realçados.
- Alternar entre faixa e mapa, prima `Ctrl+F6`.
        
### <a name="keyboard-navigation-on-the-map"></a>Navegação do teclado no mapa
    
- Depois de ter selecionado qualquer filtro na faixa e premido `Ctrl+F6`, foco é movido para um de nós realçados (**datacenter do Azure** ou **país/região**) na vista de mapa.
- Para mover para outros nós realçados no mapa, utilize `Tab` ou `Right arrow` chave para o movimento de reencaminhar. Utilize `Shift+Tab` ou `Left arrow` chave para movimento com versões anteriores.
- Para selecionar qualquer nó realçado no mapa, utilize o `Enter` ou `Down arrow` chave.
- Seleção de quaisquer de nós, de foco move para o **informações ferramenta caixa** para o nó. Por predefinição, foco são transmitidos para o botão fechado no **informações ferramenta caixa**. Para mover mais dentro de **caixa** ver, utilize `Right arrow` e `Left arrow` chaves mover reencaminhar e trás, respetivamente. Premir `Enter` tem o mesmo efeito que selecionar o botão focado no **informações ferramenta caixa**.
- Quando prime `Tab` enquanto o foco incide no **informações ferramenta caixa**, move o foco para os pontos finais no continente mesmo que o nó selecionado. Utilize o `Right arrow` e `Left arrow` chaves mover através estes pontos finais.
- Para mover para outros pontos finais de fluxo ou clusters continente antes, utilize `Tab` para movimento direta e `Shift+Tab` para movimento com versões anteriores.
- Quando o foco incide na **continente clusters**, utilize o `Enter` ou `Down` teclas de seta para destacar os pontos finais dentro do cluster continente antes. Para mover através de pontos finais e o botão de fechar a caixa de informações do cluster continente antes, utilize o `Right arrow` ou `Left arrow` chave para movimento direta e com versões anteriores, respetivamente. Em qualquer ponto final, pode utilizar `Shift+L` para mudar para a linha de ligação a partir do nó selecionado para o ponto final. Pode premir `Shift+L` novamente para mover para o ponto final selecionado.
        
### <a name="keyboard-navigation-at-any-stage"></a>Navegação do teclado em qualquer fase
    
- `Esc` Fecha a seleção expandida.
- O `Up arrow` chave efetua a mesma ação como `Esc`. O `Down arrow` chave efetua a mesma ação como `Enter`.
- Utilize `Shift+Plus` para aplicar zoom no, e `Shift+Minus` para reduzir.

## <a name="how-can-i-navigate-by-using-the-keyboard-in-the-virtual-network-topology-view"></a>Como pode navegar utilizando o teclado na vista de topologia de rede virtual?

A página de topologia de redes virtuais contém duas secções principais:
    
- **Faixa**: A faixa na parte superior da topologia de redes virtuais fornece botões para selecionar os filtros de distribuição de tráfego (por exemplo, redes virtuais ligadas, redes virtuais desligados e IPs públicos). Quando seleciona um botão, o respetivo filtro é aplicado a topologia. Por exemplo, se selecionar o botão de Active Directory, a topologia realça as redes virtuais Active Directory na sua implementação.
- **Topologia**: abaixo a faixa, a secção de topologia mostra a distribuição de tráfego entre redes virtuais.
    
### <a name="keyboard-navigation-on-the-banner"></a>Navegação do teclado na faixa
    
- Por predefinição, a seleção na página de topologia de redes virtuais para a faixa é o filtro de "VNets ligados".
- Para mover para outro filtro, utilize o `Tab` chave para avançar. Para mover trás, utilize o `Shift+Tab` chave. Navegação de avanço é esquerda para a direita, seguido de parte superior para parte inferior.
- Prima `Enter` para aplicar o filtro selecionado. Com base na seleção de filtro e a implementação, um ou vários nós (rede virtual) na secção topologia são realçados.
- Alternar entre a faixa e a topologia, prima `Ctrl+F6`.
        
### <a name="keyboard-navigation-on-the-topology"></a>Navegação do teclado na topologia
    
- Depois de ter selecionado qualquer filtro na faixa e premido `Ctrl+F6`, foco é movido para um de nós realçados (**VNet**) na vista de topologia.
- Para mover para outros nós realçado na vista de topologia, utilize o `Shift+Right arrow` chave para o movimento de reencaminhar. 
- Nós realçado, o foco move para o **informações ferramenta caixa** para o nó. Por predefinição, o foco move para o **mais detalhes** botão no **informações ferramenta caixa**. Para mover mais dentro de **caixa** ver, utilize o `Right arrow` e `Left arrow` chaves mover reencaminhar e trás, respetivamente. Premir `Enter` tem o mesmo efeito que selecionar o botão focado no **informações ferramenta caixa**.
- Na seleção de quaisquer de nós, pode visitar todas as suas ligações, um de cada, premindo o `Shift+Left arrow` chave. Move foco para o **informações ferramenta caixa** dessa ligação. Em qualquer momento, o foco pode desviado novamente para o nó ao premir `Shift+Right arrow` novamente.
    

## <a name="how-can-i-navigate-by-using-the-keyboard-in-the-subnet-topology-view"></a>Como pode navegar utilizando o teclado na vista de topologia de sub-rede?

A página de topologia de sub-redes virtuais contém duas secções principais:
    
- **Faixa**: A faixa na parte superior da topologia de sub-redes virtuais fornece botões para selecionar os filtros de distribuição de tráfego (por exemplo, sub-redes Active Directory, médios e Gateway). Quando seleciona um botão, o respetivo filtro é aplicado a topologia. Por exemplo, se selecionar o botão de Active Directory, a topologia realça a sub-rede virtual Active Directory na sua implementação.
- **Topologia**: abaixo a faixa, a secção de topologia mostra a distribuição de tráfego entre sub-redes virtuais.
    
### <a name="keyboard-navigation-on-the-banner"></a>Navegação do teclado na faixa
    
- Por predefinição, a seleção na página de topologia de sub-redes virtuais para a faixa é o filtro de "Sub-redes".
- Para mover para outro filtro, utilize o `Tab` chave para avançar. Para mover trás, utilize o `Shift+Tab` chave. Navegação de avanço é esquerda para a direita, seguido de parte superior para parte inferior.
- Prima `Enter` para aplicar o filtro selecionado. Com base na seleção de filtro e a implementação, um ou vários nós (sub-rede) na secção topologia são realçados.
- Alternar entre a faixa e a topologia, prima `Ctrl+F6`.
        
### <a name="keyboard-navigation-on-the-topology"></a>Navegação do teclado na topologia
    
- Depois de ter selecionado qualquer filtro na faixa e premido `Ctrl+F6`, foco é movido para um de nós realçados (**sub-rede**) na vista de topologia.
- Para mover para outros nós realçado na vista de topologia, utilize o `Shift+Right arrow` chave para o movimento de reencaminhar. 
- Nós realçado, o foco move para o **informações ferramenta caixa** para o nó. Por predefinição, o foco move para o **mais detalhes** botão no **informações ferramenta caixa**. Para mover mais dentro de **caixa** ver, utilize `Right arrow` e `Left arrow` chaves mover reencaminhar e trás, respetivamente. Premir `Enter` tem o mesmo efeito que selecionar o botão focado no **informações ferramenta caixa**.
- Na seleção de quaisquer de nós, pode visitar todas as suas ligações, um de cada, premindo `Shift+Left arrow` chave. Move foco para o **informações ferramenta caixa** dessa ligação. Em qualquer momento, o foco pode desviado novamente para o nó ao premir `Shift+Right arrow` novamente.    

