---
title: Monitorizar o HBase com a análise de registos do Azure - o Azure HDInsight | Microsoft Docs
description: Utilize Log Analytics do Azure para monitorizar clusters do HBase do HDInsight.
services: hdinsight
documentationcenter: ''
tags: azure-portal
author: ashishthaps
manager: jhubbard
editor: cgronlun
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: ashishth
ms.openlocfilehash: 12ec60049cdf267834d251c6c927b35e3c363a4e
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
---
# <a name="monitor-hbase-with-log-analytics"></a>Monitor de HBase com a análise de registos

Monitorização de HBase do HDInsight utiliza o Log Analytics do Azure para recolher métricas de desempenho do HBase do HDInsight a partir os nós de cluster do HDInsight. O Monitor fornece visualizações de HBase específicos e dashboards, ferramentas para pesquisar as métricas e a capacidade de criar regras personalizadas de monitorização e alertas. Pode monitorizar as métricas de vários clusters do HBase do HDInsight através de várias subscrições do Azure.

Análise de registos é um serviço no [Azure](../../operations-management-suite/operations-management-suite-overview.md) que monitoriza a sua nuvem e no local ambientes para manter a respetiva disponibilidade e desempenho. Análise de registos recolhe dados gerados pelos recursos nos seus ambientes de nuvem e no local e a partir de outras ferramentas de monitorização, para fornecer analysis através de várias origens.

[As soluções de gestão de análise de registo](../../log-analytics/log-analytics-add-solutions.md) adicionar funcionalidades à análise de registos, que fornece ferramentas de análise e dados adicionais. Soluções de gestão de análise do registo são uma coleção de regras de aquisição lógica, visualização e os dados que fornecem as métricas para uma área específica. Uma solução também pode definir novos tipos de registo a ser recolhido e estes registos podem ser analisados pesquisas de registo ou com novas funcionalidades de interface de utilizador.

[Conhecimentos aprofundados & análise](https://azure.microsoft.com/pricing/details/insight-analytics/) baseia-se na plataforma de análise de registos. Pode optar por utilizar as capacidades de análise de registos e paga por GB ingerido no serviço, ou mudar a sua área de trabalho para a camada de conhecimentos aprofundados & análise e paga por nó gerido pelo serviço. Conhecimentos aprofundados & análise oferece um superconjunto das funcionalidades oferecidas pelas análise de registos. A solução de monitorização de HBase está disponível com a análise de registos ou conhecimentos aprofundados & análise.

Quando aprovisionar uma solução de monitorização de HBase do HDInsight, criar uma área de trabalho de análise de registos. Cada área de trabalho como um ambiente de análise de registos exclusivo com as suas próprias repositório de dados, as origens de dados e soluções. Pode criar várias áreas de trabalho na sua subscrição para suportar vários ambientes, como produção e teste.

## <a name="provision-hdinsight-hbase-monitoring"></a>Aprovisionar o HBase do HDInsight monitorização

1. Iniciar sessão para o [portal do Azure](https://portal.azure.com) através da sua subscrição do Azure.
2. No **novo** painel em **Marketplace**, selecione **monitorização + gestão**.
3. No **monitorização + gestão** painel, selecione **ver todos os**.

    ![Monitorização + painel de gestão](./media/apache-hbase-monitor-with-oms/monitoring-management-blade.png)  

4. Na lista, procure o **soluções de gestão** banda. À direita do **soluções de gestão**, selecione **mais**.

    ![Monitorização + painel de gestão](./media/apache-hbase-monitor-with-oms/management-solutions.png) 

5. No **soluções de gestão** painel, selecione a solução de gestão de monitorização de HBase do HDInsight para adicionar a uma área de trabalho.

    ![Painel de soluções de gestão](./media/apache-hbase-monitor-with-oms/hbase-solution.png)  
6. No painel de solução de gestão, reveja as informações sobre a solução de gestão e, em seguida, selecione **criar**. 
7. No *nome da solução de gestão* painel, selecione uma área de trabalho existente para associar a solução de gestão ou criar uma nova área de trabalho de análise de registos e, em seguida, selecioná-lo.
8. Alterar as definições da área de trabalho para a subscrição do Azure, o grupo de recursos e a localização conforme apropriado. 
    ![área de trabalho da solução](./media/apache-hbase-monitor-with-oms/solution-workspace.png)  
9. Selecione **Criar**.  
10. Para utilizar esta nova solução de gestão na respetiva área de trabalho, navegue até à **Log Analytics** > ***nome da área de trabalho*** > **soluções**. Uma entrada para a sua solução de gestão é apresentada na lista. Selecione a entrada para navegar para a solução.

    ![Soluções de análise do registo](./media/apache-hbase-monitor-with-oms/log-analytics-solutions.png)  

11. É apresentado o painel para a sua solução de monitorização do HBase do HDInsight.

    ![Painel de soluções de HBase do HDInsight](./media/apache-hbase-monitor-with-oms/hdinsight-hbase-solution.png) 

12. Os mosaicos de resumos não mostram todos os dados porque ainda não configurou o cluster de HBase do HDInsight para enviar dados para análise de registos.

## <a name="connect-hdinsight-hbase-cluster-to-log-analytics"></a>Ligue o cluster de HBase do HDInsight ao Log Analytics

Para utilizar as ferramentas fornecidas pelo HDInsight HBase monitorização, terá de configurar o cluster, de modo a que transmite as métricas do respetivo servidor de região, nós principais e nós de ZooKeeper à análise de registos. Esta configuração é efetuada através da execução de uma ação de Script no seu cluster HBase do HDInsight.

### <a name="get-log-analytics-workspace-id-and-workspace-key"></a>Obter ID de área de trabalho de análise do registo e a chave de área de trabalho

Terá do ID de área de trabalho de análise do registo e a área de trabalho chave para permitir que os nós do cluster para se autenticar com a análise de registos. Para obter estes valores:

1. A partir do seu painel monitorização do HBase no portal do Azure, selecione descrição geral.

    ![Painel de solução de monitorização de HBase](./media/apache-hbase-monitor-with-oms/hdinsight-hbase-solution.png) 

2. Selecione o Portal do OMS para iniciar o Portal do OMS num novo separador do browser ou janela.

    ![Selecione o Portal do OMS](./media/apache-hbase-monitor-with-oms/select-oms-portal.png) 

3. A home page Portal do OMS, selecione as definições.

    ![Portal do OMS](./media/apache-hbase-monitor-with-oms/oms-portal-settings.png) 

4. Selecione ligado origem, servidores Linux.

    ![Selecionar origem ligada, em seguida, servidores Linux](./media/apache-hbase-monitor-with-oms/select-linux-servers.png) 

5. Tenha em atenção os valores de ID da área de trabalho e a chave primária apresentados, como estes são os valores que precisa para a ação de Script.

### <a name="run-the-script-action"></a>Execute a ação de Script

Para ativar a recolha de dados do seu cluster HBase do HDInsight, execute uma ação de Script em relação a todos os nós do cluster:

1. Navegue para o painel para o seu cluster HBase do HDInsight no portal do Azure.
2. Selecione **ações de Script**.

    ![Ações de script](./media/apache-hbase-monitor-with-oms/script-actions.png) 

3. Selecione **submeter novo**.

    ![Submeter de novo](./media/apache-hbase-monitor-with-oms/script-actions-submit-new.png)  

4. No **submeter script** ação, defina o tipo de Script para `"- Custom"`.
5. Forneça um nome para este script.
6. Para o **Bash script URI**, cole o URI seguinte:

        https://raw.githubusercontent.com/hdinsight/HDInsightOMS/master/monitoring/script2.sh 

7. Para o **tipos de nó**, selecione todas as três (**Head**, **região**, **ZooKeeper**).
8. No **parâmetros** caixa de texto, introduza o seu ID da área de trabalho e a chave da área de trabalho, envolvente cada valor aspas e separados por um espaço.

        "<Workspace ID>" "<Workspace Key>"

9. Selecione **manter esta ação de script** para voltar a executar a ação quando são adicionados novos nós ao cluster.

    ![Definições de ação de script](./media/apache-hbase-monitor-with-oms/submit-script-action.png)  

10. Selecione **Criar**.
11. A ação de script demora alguns minutos para ser executada. Pode monitorizar o estado do painel de ações de Script.

    ![Ação de script em execução](./media/apache-hbase-monitor-with-oms/script-action-running.png)  

12. Quando concluir a ação de Script, deverá ver uma marca de verificação verde junto ao nome do script na listagem.

    ![Ação de script concluída](./media/apache-hbase-monitor-with-oms/script-action-done.png)  

## <a name="view-the-hdinsight-hbase-monitoring-solution"></a>Ver a solução de monitorização de HBase do HDInsight

Depois de concluída a ação de Script, deve ver os dados na solução de monitorização dentro de alguns minutos.

1. No portal do Azure, navegue para o painel da sua solução de HBase do HDInsight.
2. Selecione o **descrição geral** separador.
3. Em **resumo**, verá um mosaico que indica o número de servidores de região que estão a ser monitorizados.

    ![Mosaico de resumo de monitorização](./media/apache-hbase-monitor-with-oms/monitoring-summary-tile.png)  

4. Selecione o mosaico com o número de servidor de região. Dashboard principal é apresentado.
5. Este dashboard fornece acesso às estatísticas sobre as regiões, a contagem de registo de escrita antecipada (WLAN) em utilização, uma coleção de pesquisas de análise de registos (os registos, tais como servidor de região, registos de Phoenix e exceções) e uma coleção de populares gráficos para visualizar relevantes métricas. 

    ![Dashboard principal](./media/apache-hbase-monitor-with-oms/main-dashboard.png)  

6. Selecionar qualquer uma destas será desagregar para a vista de pesquisa de registo, onde pode refinar a consulta e explorar os dados mais detalhadamente.

## <a name="next-steps"></a>Passos Seguintes

* [Criar alertas na análise de registos](../../log-analytics/log-analytics-alerts-creating.md)
* [Encontrar dados com pesquisas de registo no Log Analytics do Azure](../../log-analytics/log-analytics-log-searches.md).
