---
title: Avaliar aplicações do Service Fabric com o Log Analytics no portal do Azure | Documentos da Microsoft
description: Pode utilizar a solução de Service Fabric no Log Analytics com o portal do Azure para avaliar o risco e estado de funcionamento das suas aplicações Service Fabric, microsserviços, nós e clusters.
services: log-analytics
documentationcenter: ''
author: niniikhena
manager: jochan
editor: ''
ms.assetid: 9c91aacb-c48e-466c-b792-261f25940c0c
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/19/2018
ms.author: nini
ms.component: na
ms.openlocfilehash: 9bc1f7d9eab73a086e664dcc520ecf26befbbf0e
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39432896"
---
# <a name="assess-service-fabric-applications-and-micro-services-with-the-azure-portal"></a>Avaliar aplicações do Service Fabric e os microsserviços com o portal do Azure

> [!div class="op_single_selector"]
> * [Resource Manager](log-analytics-service-fabric-azure-resource-manager.md)
> * [PowerShell](log-analytics-service-fabric.md)
>
>

![Símbolo do Service Fabric](./media/log-analytics-service-fabric/service-fabric-assessment-symbol.png)

Este artigo descreve como utilizar a solução de Service Fabric no Log Analytics para ajudar a identificar e resolver problemas no seu cluster do Service Fabric.

A solução de Service Fabric utiliza dados de diagnóstico do Azure suas VMS de recursos de infraestrutura do serviço, através da recolha de dados de tabelas do Azure WAD. O log Analytics, em seguida, lê os eventos de estrutura de Service Fabric, incluindo **eventos de Reliable Service**, **eventos de Ator**, **eventos operacionais**, e **personalizado Eventos do ETW**. Com o dashboard de solução, é possível ver problemas relevantes e eventos relevantes no seu ambiente do Service Fabric.

Para começar a utilizar com a solução, terá de ligar o seu cluster do Service Fabric a uma área de trabalho do Log Analytics. Seguem-se três cenários a considerar:

1. Se não tiver implementado o cluster do Service Fabric, utilize os passos em ***implementar um Cluster do Service Fabric ligado a uma área de trabalho do Log Analytics*** implementar um novo cluster e fazê-lo configurados para reportar para o Log Analytics.
1. Se pretender recolher contadores de desempenho a partir dos anfitriões para utilizar outras soluções de gestão, como a segurança no seu Cluster do Service Fabric, siga os passos em ***implementar um Cluster do Service Fabric ligado a uma área de trabalho do Log Analytics com a extensão de VM instalado.***
1. Se já tiver implementado o cluster do Service Fabric e pretender ligá-la para o Log Analytics, siga os passos no ***adicionar uma conta de armazenamento existente para o Log Analytics.***

## <a name="deploy-a-service-fabric-cluster-connected-to-a-log-analytics-workspace"></a>Implemente um Cluster do Service Fabric ligado a uma área de trabalho do Log Analytics.
Este modelo faz o seguinte:

1. Implementa um cluster do Azure Service Fabric já ligado a uma área de trabalho do Log Analytics. Tem a opção de criar uma nova área de trabalho ao implementar o modelo ou introduzir o nome de uma área de trabalho do Log Analytics já existente.
1. Adiciona a conta de armazenamento do diagnóstico para a área de trabalho do Log Analytics.
1. Ativa a solução de Service Fabric na sua área de trabalho do Log Analytics.

[![Implementar no Azure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-oms%2F%2Fazuredeploy.json)

Depois de selecionar o botão implementar acima, é aberto o portal do Azure com parâmetros para editar. Certifique-se de que criar um novo grupo de recursos, se inserir um novo nome de área de trabalho do Log Analytics:

![Service Fabric](./media/log-analytics-service-fabric/2.png)

![Service Fabric](./media/log-analytics-service-fabric/3.png)

Aceite os termos legais e clique em **criar** para iniciar a implementação. Depois de concluída a implementação, deverá ver a nova área de trabalho e cluster criado e o WADServiceFabric * tabelas de evento, WADWindowsEventLogs e WADETWEvent adicionadas:

![Service Fabric](./media/log-analytics-service-fabric/4.png)

## <a name="deploy-a-service-fabric-cluster-connected-to-a-log-analytics-workspace-with-vm-extension-installed"></a>Implemente um Cluster do Service Fabric ligado a uma área de trabalho do Log Analytics com a extensão de VM instalado.

Este modelo faz o seguinte:

1. Implementa um cluster do Azure Service Fabric já ligado a uma área de trabalho do Log Analytics. Pode criar uma nova área de trabalho ou utilize um já existente.
1. Adiciona as contas de armazenamento do diagnóstico para a área de trabalho do Log Analytics.
1. Ativa a solução de Service Fabric na área de trabalho do Log Analytics.
1. Instala a extensão do agente MMA em cada conjunto de dimensionamento no seu cluster do Service Fabric. Com o agente MMA instalado, é possível ver as métricas de desempenho sobre os nós.

[![Implementar no Azure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-vmss-oms%2F%2Fazuredeploy.json)

Ao seguir os mesmos passos acima, os parâmetros necessários de entrada e iniciar uma implementação. Mais uma vez deverá ver a nova área de trabalho, o cluster e a tabelas WAD tudo criadas:

![Service Fabric](./media/log-analytics-service-fabric/5.png)

### <a name="viewing-performance-data"></a>Ver dados de desempenho

Para ver dados de desempenho de seus nós:


[!INCLUDE [log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

- Inicie a área de trabalho do Log Analytics no portal do Azure.
  ![Service Fabric](./media/log-analytics-service-fabric/6.png)
- Aceda às definições no painel da esquerda e selecione dados >> contadores de desempenho do Windows >> "Adicionar os contadores de desempenho selecionados": ![Service Fabric](./media/log-analytics-service-fabric/7.png)
- Na pesquisa de registos, utilize as seguintes consultas para se aprofundar nas métricas-chave sobre os nós:

    a. Compare a utilização média da CPU de todos os seus nós na última hora para ver quais os nós estão a ter problemas e o intervalo de tempo, um nó tivesse um pico:

    ```
    Type=Perf ObjectName=Processor CounterName="% Processor Time"|measure avg(CounterValue) by Computer Interval 1HOUR.
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/10.png)

    b. Ver gráficos de linhas semelhante para a memória disponível em cada nó com esta consulta:

    ```
    Type=Perf ObjectName=Memory CounterName="Available MBytes Memory" | measure avg(CounterValue) by Computer Interval 1HOUR.
    ```

    Para ver uma lista de todos os seus nós, que mostra o valor médio exato de MBytes disponíveis para cada nó, utilize esta consulta:

    ```
    Type=Perf (ObjectName=Memory) (CounterName="Available MBytes") | measure avg(CounterValue) by Computer
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/11.png)

    c. No caso de que pretende fazer uma busca detalhada num nó específico, examinando a média por hora, a utilização de CPU mínima, máxima e percentil 75, pode fazer isso usando esta consulta (substituir campo computador):

    ```
    Type=Perf CounterName="% Processor Time" InstanceName=_Total Computer="BaconDC01.BaconLand.com"| measure min(CounterValue), avg(CounterValue), percentile75(CounterValue), max(CounterValue) by Computer Interval 1HOUR
    ```

    ![Service Fabric](./media/log-analytics-service-fabric/12.png)

Leia mais informações sobre as métricas de desempenho do Log Analytics com o [blog do Operations Management Suite](https://blogs.technet.microsoft.com/msoms/tag/metrics/).


## <a name="adding-an-existing-storage-account-to-log-analytics"></a>Adicionar uma conta de armazenamento existente para o Log Analytics

Este modelo simplesmente adiciona contas de armazenamento existentes para uma área de trabalho do Log Analytics de nova ou existente.

[![Implementar no Azure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Foms-existing-storage-account%2Fazuredeploy.json)

> [!NOTE]
> Seleção de um grupo de recursos, se estiver trabalhando com uma área de trabalho do Log Analytics já existente, selecione "Utilizar existente" e procure o grupo de recursos que contém a área de trabalho do Log Analytics. Criar um novo um se caso contrário.
> ![Service Fabric](./media/log-analytics-service-fabric/8.png)
>
>

Depois de implementar este modelo, será capaz de ver a conta de armazenamento ligada à sua área de trabalho do Log Analytics. Neste caso, adicionei uma conta de armazenamento mais para a área de trabalho do Exchange que criou acima.
![Service Fabric](./media/log-analytics-service-fabric/9.png)

## <a name="view-service-fabric-events"></a>Ver eventos do Service Fabric

Depois das implementações são concluídas e a solução de Service Fabric foi ativada na sua área de trabalho, selecione o **Service Fabric** mosaico no portal do Log Analytics para iniciar o dashboard do Service Fabric. O dashboard inclui as colunas da tabela seguinte. Cada coluna apresenta os 10 eventos principais por contagem que satisfaçam os critérios dessa coluna para o intervalo de tempo especificado. Pode executar uma pesquisa de registos que fornece toda a lista clicando **ver todas as** na parte inferior direita de cada coluna ou ao clicar no cabeçalho da coluna.

| **Eventos do Service Fabric** | **Descrição** |
| --- | --- |
| Problemas Relevantes |Uma apresentação de questões como RunAsyncFailures RunAsynCancellations e listas de nó. |
| Eventos operacionais |Eventos notáveis operacionais, como a atualização da aplicação e implementações. |
| Eventos de Reliable Services |Eventos de serviço fiável notáveis tais um Runasyncinvocations. |
| Eventos de ator |Eventos de ator notáveis gerados pelo seu microsserviços, tais como exceções acionadas por um método de ator, ativações de ator e desativações e assim por diante. |
| Eventos da aplicação |Todos os personalizados Eventos ETW gerados pelas suas aplicações. |

![Dashboard do Service Fabric](./media/log-analytics-service-fabric/sf3.png)

![Dashboard do Service Fabric](./media/log-analytics-service-fabric/sf4.png)

A tabela seguinte mostra os métodos de recolha de dados e outros detalhes sobre como os dados são recolhidos para o Service Fabric.

| Plataforma | Agente Direto | Agente do Operations Manager | Storage do Azure | Gestor de operações necessárias? | Dados de agente do Operations Manager enviados por grupo de gestão | Frequência de recolha |
| --- | --- | --- | --- | --- | --- | --- |
| Windows |  |  | &#8226; |  |  |10 minutos |

> [!NOTE]
> Pode alterar o âmbito de um desses eventos na solução de Service Fabric clicando **dados com base nos últimos 7 dias** na parte superior do dashboard. Também pode mostrar eventos gerados durante os últimos sete dias, um dia ou seis horas. Em alternativa, pode selecionar **personalizado** para especificar um intervalo de datas personalizadas.
>
>

## <a name="next-steps"></a>Passos Seguintes

* Uso [pesquisas de registos no Log Analytics](log-analytics-log-searches.md) para ver os dados de evento detalhados do Service Fabric.
