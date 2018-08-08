---
title: Utilizar a IU do Tez com o HDInsight baseado em Windows - Azure
description: Saiba como utilizar a IU do Tez para depurar tarefas do Tez no HDInsight do HDInsight baseado em Windows.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/17/2017
ms.author: jasonh
ROBOTS: NOINDEX
ms.openlocfilehash: f54cc60f9490b8a5ca1872a290c3895ea8b0c5e4
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/07/2018
ms.locfileid: "39590906"
---
# <a name="use-the-tez-ui-to-debug-tez-jobs-on-windows-based-hdinsight"></a>Utilize a IU do Tez para depurar tarefas do Tez no HDInsight baseado em Windows
Pode ser utilizado a IU do Tez para depurar tarefas do Hive que utilizam o Tez como motor de execução. A IU do Tez visualiza a tarefa, como um gráfico de itens ligados, pode explorar cada item e obter estatísticas e informações de registo.

> [!IMPORTANT]
> Os passos neste documento exigem um cluster do HDInsight que utiliza o Windows. O Linux é o único sistema operativo utilizado na versão 3.4 ou superior do HDInsight. Para obter mais informações, veja [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (Desativação do HDInsight no Windows).

## <a name="prerequisites"></a>Pré-requisitos
* Um cluster do HDInsight baseado em Windows. Para obter passos sobre como criar um novo cluster, consulte [começar a utilizar o HDInsight baseado em Windows](hdinsight-hadoop-tutorial-get-started-windows.md).

  > [!IMPORTANT]
  > A IU do Tez só está disponível em clusters do HDInsight baseado em Windows criados após 8 de Fevereiro de 2016.
  >
  >
* Um cliente de ambiente de trabalho remota baseado em Windows.

## <a name="understanding-tez"></a>Noções básicas sobre Tez
Tez é uma estrutura extensível para processamento de dados no Hadoop e fornece maior 1000x maiores do que o processamento de MapReduce tradicional. Pode ativar o Tez, incluindo o seguinte texto como parte de uma consulta do Hive:

    set hive.execution.engine=tez;

Tez cria um direcionado acíclicos Graph (DAG) que descreve a ordem de execução das ações necessários para a tarefa. Ações individuais são chamadas de vértices e executar uma parte da tarefa geral. A execução real do trabalho descrito por um vértice é chamada de uma tarefa e pode ser distribuída por vários nós no cluster.

### <a name="understanding-the-tez-ui"></a>Noções básicas sobre a IU do Tez
A IU do Tez é que uma página da web fornece informações sobre processos que utilizam Tez. Ele poderá oferecer informações úteis nos seguintes cenários:

* Monitorização de execução longa processa, ver o progresso do mapa e reduzir as tarefas.
* Análise de dados históricos para processos com êxito ou falhados saber como o processamento pode ser melhorado ou por que falhou.

## <a name="generate-a-dag"></a>Gerar um DAG
A IU do Tez contém dados se uma tarefa utiliza o mecanismo de Tez está atualmente em execução ou tiver sido executada no passado. Consultas do Hive simples podem normalmente ser resolvidas sem utilizar Tez. Consultas mais complexas que fazer a filtragem, agrupamento, pedidos, associações, etc. requerem Tez.

Utilize os seguintes passos para executar uma consulta do Hive que utiliza Tez.

1. Num browser, navegue até https://CLUSTERNAME.azurehdinsight.net, onde **CLUSTERNAME** é o nome do cluster do HDInsight.
2. No menu na parte superior da página, selecione o **Editor do Hive**. Esta ação apresenta uma página com a seguinte consulta de exemplo.

        Select * from hivesampletable

    Apagar a consulta de exemplo e substitua-o com o seguinte.

        set hive.execution.engine=tez;
        select market, state, country from hivesampletable where deviceplatform='Android' group by market, country, state;
3. Selecione o **submeter** botão. O **tarefa sessão** secção na parte inferior da página apresenta o estado da consulta. Assim que o estado muda para **concluído**, selecione a **ver detalhes** ligação para ver os resultados. O **saída da tarefa** deve ser semelhante ao seguinte:

        en-GB   Hessen      Germany
        en-GB   Kingston    Jamaica
        en-GB   Nairobi Area    Kenya

## <a name="use-the-tez-ui"></a>Utilizar a IU do Tez
> [!NOTE]
> A IU do Tez só está disponível no ambiente de trabalho de nós principais do cluster, deve usar o ambiente de trabalho remoto para ligar para os nós principais.
>
>

1. Partir do [portal do Azure](https://portal.azure.com), selecione o cluster do HDInsight. Na parte superior do painel do HDInsight, selecione o **ambiente de trabalho remoto** ícone. Esta ligação apresenta o painel de área de trabalho remoto

    ![Ícone de área de trabalho remoto](./media/hdinsight-debug-tez-ui/remotedesktopicon.png)
2. No painel da área de trabalho remota, selecione **Connect** para ligar ao nó principal do cluster. Quando lhe for pedido, utilize o nome de utilizador do ambiente de trabalho remoto de cluster e a palavra-passe para autenticar a ligação.

    ![Ícone de ligação de ambiente de trabalho remoto](./media/hdinsight-debug-tez-ui/remotedesktopconnect.png)

   > [!NOTE]
   > Se não tiver ativado a conectividade de ambiente de trabalho remoto, forneça um nome de utilizador, a palavra-passe e a data de expiração, em seguida, selecione **ativar** para ativar o ambiente de trabalho remoto. Assim que tiver sido ativada, utilize os passos anteriores para ligar.
   >
   >
3. Assim que estiver ligado, abra o Internet Explorer no ambiente de trabalho remoto, selecione o ícone de engrenagem no canto superior direito do browser e, em seguida, selecione **ver definições de compatibilidade**.
4. Na parte inferior **ver definições de compatibilidade**, desmarque a caixa de verificação **apresentar sites de intranet na vista de compatibilidade** e **listas de compatibilidade da Microsoft de utilização**, e em seguida, selecione **fechar**.
5. No Internet Explorer, navegue para http://headnodehost:8188/tezui/#/. Esta ação apresenta a IU do Tez

    ![IU do Tez](./media/hdinsight-debug-tez-ui/tezui.png)

    Quando a IU do Tez for carregada, verá uma lista de DAGs que estão atualmente em execução, ou que foram executados no cluster. A exibição padrão inclui o nome do DAG, Id, submissor, estado, hora de início, hora de fim, a duração, ID da aplicação e fila. Mais colunas podem ser adicionadas com o ícone de engrenagem na linha à direita da página.

    Se tiver apenas uma entrada, é para a consulta que tenha sido executada na secção anterior. Se tiver várias entradas, pode procurar introduzindo critérios de pesquisa nos campos acima os DAGs, em seguida, prima **Enter**.
6. Selecione o **nome do Dag** para a entrada mais recente do DAG. Esta ligação apresenta informações sobre o DAG, bem como a opção para transferir um zip do ficheiros JSON que contêm informações sobre o DAG.

    ![Detalhes do DAG](./media/hdinsight-debug-tez-ui/dagdetails.png)
7. Acima da **detalhes do DAG** são várias ligações que podem ser utilizadas para apresentar informações sobre o DAG.

   * **Contadores do DAG** apresenta informações de contadores para este DAG.
   * **Vista gráfica** apresenta uma representação gráfica deste DAG.
   * **Todos os vértices** apresenta uma lista de vértices neste DAG.
   * **Todas as tarefas** apresenta uma lista de tarefas para todos os vértices neste DAG.
   * **Todos os TaskAttempts** apresenta informações sobre as tentativas de executar tarefas para esta DAG.

     > [!NOTE]
     > Se se deslocar para a exibição de coluna para vértices, tarefas e TaskAttempts, tenha em atenção que existem links para exibir **contadores** e **ver ou transferir registos** para cada linha.
     >
     >

     Se tiver ocorrido uma falha com a tarefa, os detalhes de DAG apresentam um Estado de FALHADO, juntamente com ligações para informações sobre a tarefa falhada. Informações de diagnóstico é apresentada abaixo os detalhes do DAG.
8. Selecione **visualização gráfica**. Esta ação apresenta uma representação gráfica do DAG. Pode colocar o mouse sobre cada vértice na vista para apresentar informações sobre ele.

    ![Vista de gráfica](./media/hdinsight-debug-tez-ui/dagdiagram.png)
9. Clicar num vértice carrega os **detalhes de vértice** para esse item. Clique nas **mapa 1** vértice para apresentar os detalhes para este item. Selecione **confirmar** para confirmar a navegação.

    ![Detalhes de vértice](./media/hdinsight-debug-tez-ui/vertexdetails.png)
10. Tenha em atenção que agora tem ligações na parte superior da página que estão relacionadas com vértices e tarefas.

    > [!NOTE]
    > Também pode chegar a esta página voltando ao **detalhes do DAG**, ao selecionar **detalhes de vértice**e, em seguida, selecionando o **1 do mapa** vértice.
    >
    >

    * **Contadores de vértice** apresenta informações do contador para esse vértice.
    * **Tarefas** apresenta tarefas para esse vértice.
    * **Tentativas de tarefas** apresenta informações sobre tentativas de executar tarefas para esse vértice.
    * **Origens & coletores** apresenta origens de dados e sinks para esse vértice.

      > [!NOTE]
      > Como com o menu anterior, pode rolar a exibição de coluna para tarefas, tentativas de tarefas e origens & Sinks__ apresentar ligações para obter mais informações para cada item.
      >
      >
11. Selecione **tarefas**e, em seguida, selecione o item chamado **00_000000**. Esta ligação apresenta **detalhes da tarefa** para esta tarefa. Neste ecrã, pode ver **tarefa contadores** e **tentativas de tarefa**.

    ![Detalhes da tarefa](./media/hdinsight-debug-tez-ui/taskdetails.png)

## <a name="next-steps"></a>Próximos Passos
Agora que aprendeu como utilizar o modo de exibição do Tez, saiba mais sobre [utilizar o Hive no HDInsight](hadoop/hdinsight-use-hive.md).

Para obter mais informações técnicas sobre o Tez, consulte a [página, Tez em Hortonworks](http://hortonworks.com/hadoop/tez/).
