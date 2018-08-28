---
title: Aumentar horizontalmente o Azure Analysis Services | Documentos da Microsoft
description: Replicar servidores do Azure Analysis Services com o Escalamento horizontal
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 08/27/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: f89a6bdbe906d490231725cf528396928faebe47
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43092099"
---
# <a name="azure-analysis-services-scale-out"></a>Aumentar horizontalmente o Azure Analysis Services

Com o aumento horizontal, consultas de cliente podem ser distribuídas entre várias *as réplicas de consultas* num conjunto de consultas, reduzindo os tempos de resposta durante cargas de trabalho de consulta elevado. Também pode separar o processamento do conjunto de consultas, garantindo a consultas de cliente não são afetadas negativamente por operações de processamento. Escalamento horizontal pode ser configurado no portal do Azure ou com a API de REST do Analysis Services.

## <a name="how-it-works"></a>Como funciona

Numa implementação de servidor típico, um servidor serve como o servidor de processamento e o servidor de consulta. Se o número de consultas de cliente em relação a modelos no seu servidor excede o processamento de consulta unidades (QPU) para o plano do seu servidor, ou se ocorrer o processamento do modelo ao mesmo tempo que cargas de trabalho de consulta elevado, pode reduzir o desempenho. 

Com o aumento horizontal, pode criar um conjunto de consultas com até sete réplicas de consulta adicionais (oito no total, incluindo o seu servidor). Pode dimensionar o número de réplicas de consulta para atender às demandas QPU em momentos críticos e pode separar um servidor de processamento do conjunto de consultas em qualquer altura. Todas as réplicas de consulta são criadas na mesma região que o seu servidor.

Independentemente do número de réplicas de consulta que ter num conjunto de consultas, o processamento de cargas de trabalho não são distribuídas entre réplicas de consulta. Um único servidor serve como o servidor de processamento. Réplicas de consulta servem apenas de consultas em modelos sincronizados entre cada réplica no agrupamento de consulta. 

Quando as operações de processamento forem concluídas, tem de ser efetuada uma sincronização entre o servidor de processamento e servidores de réplica de consulta. Ao automatizar as operações de processamento, é importante configurar uma operação de sincronização após a conclusão bem-sucedida de operações de processamento. Sincronização pode ser executada manualmente no portal ou utilizando o PowerShell ou a REST API.

> [!NOTE]
> Escalamento horizontal está disponível para os servidores no escalão de preço padrão. Cada réplica de consulta é cobrada à mesma taxa que o seu servidor.

> [!NOTE]
> Escalamento horizontal não aumenta a quantidade de memória disponível para o seu servidor. Para aumentar a memória, terá de atualizar o seu plano.

## <a name="region-limits"></a>Limites de região

O número de réplicas de consulta, que pode configurar está limitado por região do que seu servidor está em. Para obter mais informações, consulte [disponibilidade por região](analysis-services-overview.md#availability-by-region).

## <a name="monitor-qpu-usage"></a>Monitorizar a utilização QPU

 Para determinar se o Escalamento horizontal para o servidor é necessário, monitorize o seu servidor no portal do Azure ao uso de métricas. Se sua QPU regularmente maxes horizontalmente, significa que o número de consultas em relação a seus modelos está a exceder o limite QPU para o seu plano. Métrica de comprimento de fila de tarefa de conjunto de consulta também aumenta quando o número de consultas na fila de pool de threads a consulta excede QPU disponível. Para saber mais, consulte as [métricas do servidor de Monitorização](analysis-services-monitor.md).

## <a name="configure-scale-out"></a>Configurar o Escalamento horizontal

### <a name="in-azure-portal"></a>No portal do Azure

1. No portal, clique em **horizontal**. Utilize o controlo de deslize para selecionar o número de servidores de réplica de consulta. É o número de réplicas que escolher, além do servidor existente.

2. Na **separar o servidor de processamento do conjunto consulta**, selecione Sim para excluir o seu servidor de processamento de servidores de consulta.

   ![Controlo de deslize de escalamento horizontal](media/analysis-services-scale-out/aas-scale-out-slider.png)

3. Clique em **guardar** para aprovisionar os novos servidores de réplica de consulta. 

Modelos em tabela no seu servidor primário são sincronizados com os servidores de réplica. Quando a sincronização estiver concluída, o conjunto de consultas começa a distribuir entradas consultas entre os servidores de réplica. 


## <a name="synchronization"></a>Sincronização 

Quando aprovisionar novos réplicas de consulta, o Azure Analysis Services replica automaticamente os seus modelos em todas as réplicas. Também pode efetuar uma sincronização manual com o portal ou a REST API. Quando processar seus modelos, deve efetuar uma sincronização para que as atualizações são sincronizadas entre as réplicas de consulta.

### <a name="in-azure-portal"></a>No portal do Azure

Na **descrição geral** > modelo > **sincronizar modelo**.

![Controlo de deslize de escalamento horizontal](media/analysis-services-scale-out/aas-scale-out-sync.png)

### <a name="rest-api"></a>API REST
Utilize o **sincronização** operação.

#### <a name="synchronize-a-model"></a>Sincronizar um modelo   
`POST https://<region>.asazure.windows.net/servers/<servername>:rw/models/<modelname>/sync`

#### <a name="get-sync-status"></a>Obter o estado de sincronização  
`GET https://<region>.asazure.windows.net/servers/<servername>:rw/models/<modelname>/sync`

### <a name="powershell"></a>PowerShell
Antes de utilizar o PowerShell, [instalar ou atualizar o módulo mais recente do AzureRM](https://github.com/Azure/azure-powershell/releases). 

Para definir o número de réplicas de consulta, utilize [Set-AzureRmAnalysisServicesServer](https://docs.microsoft.com/powershell/module/azurerm.analysisservices/set-azurermanalysisservicesserver). Especifique o opcional `-ReadonlyReplicaCount` parâmetro.

Para executar a sincronização, utilize [sincronização AzureAnalysisServicesInstance](https://docs.microsoft.com/powershell/module/azurerm.analysisservices/sync-azureanalysisservicesinstance).



## <a name="connections"></a>Ligações

Na página de descrição geral do seu servidor, existem dois nomes de servidor. Se ainda não tiver configurado Escalamento horizontal para um servidor, ambos os nomes de servidor funcionam da mesma. Depois de configurar o Escalamento horizontal para um servidor, tem de especificar o nome de servidor adequado, consoante o tipo de ligação. 

Para ligações de cliente do utilizador final, como o Power BI Desktop, Excel e aplicações personalizadas, de utilização **nome do servidor**. 

Para o SSMS, SSDT e cadeias de ligação no PowerShell, aplicações de função do Azure e o AMO, utilizam **nome do servidor de gestão**. O nome do servidor de gestão inclui um especial `:rw` qualificador (leitura / escrita). Todas as operações de processamento ocorrerem no servidor de gestão.

![Nomes de servidor](media/analysis-services-scale-out/aas-scale-out-name.png)

## <a name="related-information"></a>Informações relacionadas

[Monitorizar as métricas do servidor](analysis-services-monitor.md)   
[Gerir o Azure Analysis Services](analysis-services-manage.md) 

