---
title: Resolver problemas de um cluster do HDInsight lento ou falhar - Azure HDInsight | Microsoft Docs
description: Diagnosticar e resolver problemas de um cluster do HDInsight lento ou falhar.
services: hdinsight
documentationcenter: 
tags: azure-portal
author: ashishthaps
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: ashishth
ms.openlocfilehash: 59382931d4b5478888238760b268af7f962a10b5
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2018
---
# <a name="troubleshoot-a-slow-or-failing-hdinsight-cluster"></a>Resolver problemas de um cluster do HDInsight lento ou falhar

Se um cluster do HDInsight está a ser executadas lentamente ou falhar com um código de erro, tem várias opções de resolução de problemas. Se as tarefas estão a demorar mais tempo para executar a que o esperado ou vir tempos de resposta lento em geral, pode haver falhas upstream do seu cluster, como os serviços no qual o cluster é executada. No entanto, a causa mais comum destas abrandamentos é dimensionamento insuficientes. Quando cria um novo cluster do HDInsight, selecione as adequadas [tamanhos de máquinas virtuais](hdinsight-component-versioning.md#default-node-configuration-and-virtual-machine-sizes-for-clusters)

Para diagnosticar um cluster lento ou falhar, recolha informações sobre todos os aspetos do ambiente, tais como serviços do Azure associados, a configuração de cluster e as informações de execução da tarefa. É um diagnóstico útil ao tentar reproduzir o estado de erro no outro cluster.

* Passo 1: Recolher dados sobre o problema
* Passo 2: Validar o ambiente de cluster do HDInsight 
* Passo 3: Ver o estado de funcionamento do seu cluster
* Passo 4: Consulte a pilha de ambiente e versões
* Passo 5: Examinar os ficheiros de registo do cluster
* Passo 6: Verificar as definições de configuração
* Passo 7: Reproduzir a falha de um cluster diferente 

## <a name="step-1-gather-data-about-the-issue"></a>Passo 1: Recolher dados sobre o problema

O HDInsight fornece diversas ferramentas que pode utilizar para identificar e resolver problemas com clusters. Os seguintes passos guiá-lo através destas ferramentas e fornecem sugestões para pinpointing o problema.

### <a name="identify-the-problem"></a>Identificar o problema

Para ajudar a identificar o problema, considere as seguintes perguntas:

* O que espera acontecer? Em vez disso, o que aconteceu?
* Quanto o processo necessário para executar? Quanto deve ser ter executado?
* Tem a minha tarefas sempre executadas lentamente neste cluster? São executadas mais rapidamente em outro cluster?
* Quando este problema primeiro ocorriam? Frequência tiver ocorrido desde-lo?
* Nada foi alterado na minha configuração de cluster?

### <a name="cluster-details"></a>Detalhes do cluster

Informações importantes de cluster incluem:

* Nome do cluster.
* Cluster região - procurar [falhas de região](https://azure.microsoft.com/status/).
* Tipo de cluster do HDInsight e a versão.
* O tipo e número de instâncias de HDInsight especificado para os nós head e de trabalho.

O portal do Azure pode fornecer estas informações:

![Portal do HDInsight Azure informações](./media/hdinsight-troubleshoot-failed-cluster/portal.png)

Também pode utilizar a CLI do Azure:

```
    azure hdinsight cluster list
    azure hdinsight cluster show <ClusterName>
```

Outra opção é utilizar o PowerShell. Para obter mais informações, consulte [clusters do Hadoop gerir no HDInsight com o Azure PowerShell](hdinsight-administer-use-powershell.md).

## <a name="step-2-validate-the-hdinsight-cluster-environment"></a>Passo 2: Validar o ambiente de cluster do HDInsight

Cada cluster do HDInsight baseia-se em vários serviços do Azure e, no software open source como o Apache Spark e o Apache HBase. Clusters do HDInsight também podem chamar outros serviços do Azure, tais como redes virtuais do Azure.  Uma falha de cluster pode ser causada por qualquer um dos serviços em execução no seu cluster, ou por um serviço externo.  Uma alteração de configuração do serviço de cluster também pode causar o falha do cluster.

### <a name="service-details"></a>Detalhes do serviço

* Verifique as versões de lançamento da biblioteca de open source
* Verifique a existência de [falhas no serviço do Azure](https://azure.microsoft.com/status/) 
* Verifique a existência de limites de utilização do serviço do Azure 
* Verifique a configuração de sub-rede de rede Virtual do Azure 

### <a name="view-cluster-configuration-settings-with-the-ambari-ui"></a>Ver definições de configuração de cluster com a IU do Ambari

Apache Ambari fornece gestão e monitorização de um cluster do HDInsight com uma IU da web e uma API REST. Ambari está incluído nos clusters do HDInsight baseado em Linux. Selecione o **Cluster Dashboard** painel na página de HDInsight portal do Azure.  Selecione o **dashboard de cluster do HDInsight** painel para abrir a IU do Ambari e introduza as credenciais de início de sessão do cluster.  

![IU do Ambari](./media/hdinsight-troubleshoot-failed-cluster/ambari-ui.png)

Para abrir uma lista de vistas de serviço, selecione **vistas do Ambari** na página do portal do Azure.  Esta lista depende de quais bibliotecas estão instaladas. Por exemplo, poderá ver o Gestor de filas YARN, a vista do Hive e a vista de Tez.  Selecione uma ligação de serviço para ver informações do serviço de configuração e.

#### <a name="check-for-azure-service-outages"></a>Verifique a existência de falhas no serviço do Azure

HDInsight depende de vários serviços do Azure. Este é executado servidores virtuais no Azure HDInsight, arquivos de dados e scripts no Blob storage do Azure ou do Azure DataLake Store, e ficheiros de registo de índices no Table storage do Azure. Interrupções para estes serviços, apesar de raros, podem causar problemas no HDInsight. Se tiver abrandamentos inesperados ou falhas do cluster, verifique o [Dashboard de estado de Azure](https://azure.microsoft.com/status/). O estado de cada serviço é listado por região. Verifique o seu cluster região e também regiões de quaisquer serviços relacionados.

#### <a name="check-azure-service-usage-limits"></a>Verifique os limites de utilização do serviço do Azure

Se estão a iniciar um cluster de grandes dimensões ou ter sido iniciada em simultâneo vários clusters, um cluster pode falhar se excedeu um limite de serviço do Azure. Limites de serviço variam, dependendo da sua subscrição do Azure. Para obter mais informações, consulte [subscrição do Azure e limites de serviço, quotas e restrições](https://docs.microsoft.com/azure/azure-subscription-service-limits).
Pode pedir que o Microsoft aumentar o número de recursos do HDInsight disponíveis (por exemplo, núcleos VM e instâncias de VM) com um [pedido de aumento de quota de núcleos de Gestor de recursos](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request).

#### <a name="check-the-release-version"></a>Verificação da versão de lançamento

Compare a versão do cluster com a versão mais recente do HDInsight. Cada versão do HDInsight inclui melhoramentos como novas aplicações, funcionalidades, patches e correções de erros. O problema que está a afetar o seu cluster pode ter foi corrigido na versão mais recente. Se for possível, execute novamente o cluster utilizando a versão mais recente do HDInsight e as bibliotecas associadas, tais como o Apache HBase, o Apache Spark e outros.

#### <a name="restart-your-cluster-services"></a>Reinicie os serviços de cluster

Se ocorrerem abrandamentos no seu cluster, considere reiniciar os serviços através da IU do Ambari ou a CLI do Azure. O cluster pode ser a experienciar erros transitórios e reiniciar é a forma mais rápida stabilize o seu ambiente e possivelmente melhorar o desempenho.

## <a name="step-3-view-your-clusters-health"></a>Passo 3: Ver o estado de funcionamento do seu cluster

Clusters do HDInsight são compostas por diferentes tipos de nós em execução em instâncias de máquina virtual. Cada nó pode ser monitorizado carência de recursos, problemas de conectividade de rede e outros problemas que podem abrandar o cluster. Cada cluster contém dois nós principais e a maioria dos tipos de cluster contêm uma combinação de trabalho e nós de limite. 

<!-- For a description of the various nodes each cluster type uses, see [HDInsight Architecture](hdinsight-architecture.md). -->

As secções seguintes descrevem como verificar o estado de funcionamento de cada nó e do cluster geral.

### <a name="get-a-snapshot-of-the-cluster-health-using-the-ambari-ui-dashboard"></a>Obter um instantâneo do Estado de funcionamento de cluster utilizar o dashboard da IU do Ambari

O [dashboard da IU do Ambari](#view-cluster-configuration-settings-with-the-ambari-ui) (`https://<clustername>.azurehdinsight.net`) fornece uma descrição geral do Estado de funcionamento de cluster, como o tempo de atividade, memória, rede e utilização da CPU, a utilização de disco do HDFS e assim sucessivamente. Utilize a secção de anfitriões do Ambari para ver os recursos a um nível de anfitrião. Também pode parar e reiniciar os serviços.

### <a name="check-your-webhcat-service"></a>Verifique o serviço de WebHCat

Para tarefas do Hive, Pig ou Sqoop falhar um cenário comum é uma falha com o [WebHCat](hdinsight-hadoop-templeton-webhcat-debug-errors.md) (ou *Templeton*) service. WebHCat é uma interface REST para a execução da tarefa remota, tais como o Hive, Pig, Scoop e MapReduce. WebHCat traduz os pedidos de submissão da tarefa para aplicações de YARN e devolve um Estado derivado o estado da aplicação YARN.  As secções seguintes descrevem os códigos de estado de HTTP de WebHCat comuns.

#### <a name="badgateway-502-status-code"></a>BadGateway (código de estado 502)

Esta é uma mensagem genérica de nós de gateway e é o código de estado de falha mais comuns. Uma causa possível para este é o serviço de WebHCat a ser para baixo no nó principal do Active Directory. Para verificar esta possibilidade, utilize o seguinte comando CURL:

```bash
$ curl -u admin:{HTTP PASSWD} https://{CLUSTERNAME}.azurehdinsight.net/templeton/v1/status?user.name=admin
```

Ambari apresenta um alerta que mostra os anfitriões em que o serviço de WebHCat está desativado. Pode tentar colocar o serviço de WebHCat cópia de segurança reiniciando o serviço no anfitrião.

![Reinicie o servidor de WebHCat](./media/hdinsight-troubleshoot-failed-cluster/restart-webhcat.png)

Se um servidor de WebHCat ainda não pensar, em seguida, verifique o registo de operações para as mensagens de falha. Para obter mais informações, consulte o `stderr` e `stdout` ficheiros referenciados no nó.

#### <a name="webhcat-times-out"></a>WebHCat exceder o tempo limite

Um Gateway de HDInsight exceder o tempo limite as respostas que demorar mais do que dois minutos, devolvendo `502 BadGateway`. WebHCat consulta os serviços de YARN para Estados das tarefas e, se YARN demora mais de dois minutos a responder, que pode o tempo limite do pedido.

Neste caso, reveja os seguintes registos no `/var/log/webhcat` diretório:

* **webhcat.log** é o registo de log4j para que os registos do servidor escritas
* **webhcat console.log** é stdout do servidor quando iniciado
* **consola de webhcat-error.log** é stderr do processo de servidor

> [!NOTE]
> Cada `webhcat.log` ser distribuída diariamente, a geração de ficheiros com o nome `webhcat.log.YYYY-MM-DD`. Selecione o ficheiro adequado para o intervalo de tempo que está a investigar.

As secções seguintes descrevem algumas causas possíveis para tempos limite de WebHCat.

##### <a name="webhcat-level-timeout"></a>Tempo limite de nível de WebHCat

Quando WebHCat está sob carga, com mais de 10 sockets abertas, demora mais para estabelecer novas ligações de socket, que podem resultar num limite de tempo. Para listar as ligações de rede e de WebHCat, utilize `netstat` no headnode Active Directory atual:

```bash
$ netstat | grep 30111
```

30111 é a porta que webhcat escuta. O número de sockets abra deve ser inferior a 10.

Se não houver nenhuma sockets abertos, o comando anterior não produz um resultado. Para verificar se Templeton está a funcionar e está a escutar na porta 30111, utilize:

```bash
$ netstat -l | grep 30111
```

##### <a name="yarn-level-timeout"></a>Tempo limite ao nível do YARN

Templeton chama YARN para executar tarefas e a comunicação entre Templeton e YARN pode provocar a limite de tempo.

Ao nível do YARN, existem dois tipos de tempos limite:

1. Submete uma tarefa YARN pode demorar suficientemente longa para fazer com que um tempo limite.

    Se abrir o `/var/log/webhcat/webhcat.log` ficheiro de registo e procure "tarefa em fila", poderá ver várias entradas onde o tempo de execução é excessivamente longo (> 2000 ms), com as entradas que mostra aumentar Aguarde vezes.

    O tempo para as tarefas em fila continua a aumentar, porque a taxa a que as novas tarefas obterem submetidas é superior à velocidade a que o antigas tarefas estão concluídas. Depois da memória YARN é 100% utilizado, o *joblauncher fila* já não pode borrow a capacidade dos *fila predefinido*. Por conseguinte, as tarefas novas não podem ser aceites para a fila de joblauncher. Este comportamento pode fazer com que o tempo de espera para se tornar mais e mais tempo, a causar um erro de tempo limite que é, normalmente, seguida de muitas outras.

    A imagem seguinte mostra a fila de joblauncher % 714.4 overused. O que é aceitável, desde que não há capacidade livre ainda na fila de predefinição borrow do. No entanto, quando o cluster está a ser utilizado totalmente e a memória YARN tem capacidade de 100%, novas tarefas terão de aguardar, que eventualmente provoca tempos limite.

    ![Fila de Joblauncher](./media/hdinsight-troubleshoot-failed-cluster/joblauncher-queue.png)

    Existem duas formas para resolver este problema: Reduza a velocidade de novas tarefas a ser submetidas, ou aumentar a velocidade de consumo antigo de tarefas por como aumentar verticalmente o cluster.

2. Processamento de YARN pode demorar muito tempo, que pode provocar tempos limite.

    * Listar todos os trabalhos: Esta é uma chamada para um processo demorada. Esta chamada enumera as aplicações de ResourceManager o YARN e para cada aplicação concluída, obtém o estado de JobHistoryServer o YARN. Com os números mais elevados de tarefas, esta chamada pode tempo limite.

    * Lista de trabalhos com mais de sete dias: O JobHistoryServer de YARN do HDInsight está configurado para manter as informações de tarefa concluídos durante sete dias (`mapreduce.jobhistory.max-age-ms` valor). Ao tentar enumerar os resultados de tarefas removidos no limite de tempo.

Para diagnosticar estes problemas:

    1. Determinar o intervalo de tempo UTC para resolver problemas
    2. Selecione as adequadas `webhcat.log` ficheiros
    3. Procure mensagens de erro e aviso durante este período

#### <a name="other-webhcat-failures"></a>Outras falhas de WebHCat

1. Código de estado HTTP 500

    Na maioria dos casos onde WebHCat devolve 500, a mensagem de erro contém detalhes sobre a falha. Caso contrário, examine `webhcat.log` para mensagens de erro e aviso.

2. Falhas de tarefas

    Pode haver casos em que as interações com WebHCat forem efetuadas com êxito, mas as tarefas estão a falhar.

    Recolhe o resultado da consola da tarefa como Templeton `stderr` no `statusdir`, muitas vezes, que é útil para resolução de problemas. `stderr`contém o identificador da aplicação YARN da consulta real.

## <a name="step-4-review-the-environment-stack-and-versions"></a>Passo 4: Consulte a pilha de ambiente e versões

A IU do Ambari **pilha e versão** página fornece informações sobre o histórico da versão do cluster serviços configuration e serviço.  Versões de biblioteca de serviço de Hadoop incorretas podem ser uma causa da falha do cluster.  Na IU do Ambari, selecione o **Admin** menu e, em seguida, **pilhas e versões**.  Selecione o **versões** separador na página para ver informações de versão do serviço:

![A pilha e versões](./media/hdinsight-troubleshoot-failed-cluster/stack-versions.png)

## <a name="step-5-examine-the-log-files"></a>Passo 5: Examinar os ficheiros de registo

Existem muitos tipos de registos que são gerados a partir de muitos serviços e componentes que compõem um cluster do HDInsight. [Ficheiros de registo de WebHCat](#check-your-webhcat-service) descritas anteriormente. Existem vários outros útil ficheiros de registo que pode investigar para restringir a problemas com o cluster, conforme descrito nas secções seguintes.

![Exemplo de ficheiro de registo do HDInsight](./media/hdinsight-troubleshoot-failed-cluster/logs.png)

* Clusters do HDInsight consistem de vários nós, a maioria das quais está a tarefa para executar tarefas submetidas. As tarefas são executadas em simultâneo, mas os ficheiros de registo podem apresentar apenas resultados de forma linear. HDInsight executa tarefas de novo, terminar com outras pessoas que não é concluída primeiro. Todos os esta atividade é registada para o `stderr` e `syslog` ficheiros.

* Os ficheiros de registo de ação de script mostram erros ou alterações de configuração inesperado durante o processo de criação do cluster.

* Os registos de passo Hadoop identificam tarefas do Hadoop lançadas como parte de um passo que contêm erros.

### <a name="check-the-script-action-logs"></a>Verifique os registos de ação de script

HDInsight [ações de script](hdinsight-hadoop-customize-cluster-linux.md) executar scripts num cluster manualmente ou quando especificado. Por exemplo, ações de script podem ser utilizadas para instalar o software adicional no cluster ou para alterar as definições de configuração os valores predefinidos. A verificar os registos de ação de script, pode fornecer informações sobre os erros que ocorreram durante a configuração do cluster e a configuração.  Pode ver o estado de uma ação de script, selecionando o **ops** botão na IU do Ambari, ou ao aceder aos registos da conta do storage predefinida.

Os registos de ação de script de residir no `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\CLUSTER_NAME\DATE` diretório.

### <a name="view-hdinsight-logs-using-ambari-quick-links"></a>Ver registos de HDInsight com ligações rápidas Ambari

A IU do Ambari HDInsight inclui um número de **ligações rápidas** secções.  Para aceder as ligações de registo para um determinado serviço no cluster do HDInsight, abra a IU do Ambari para o cluster, em seguida, selecione a ligação de serviço da lista no lado esquerdo. Selecione o **ligações rápidas** lista pendente, em seguida, o nó de HDInsight de interesse e, em seguida, selecione a hiperligação para o respetivo registo associados.

Por exemplo, para os registos do HDFS:

![Ligações de Ambari rápido para ficheiros de registo](./media/hdinsight-troubleshoot-failed-cluster/quick-links.png)

### <a name="view-hadoop-generated-log-files"></a>Ver ficheiros de registo gerado de Hadoop

Um cluster do HDInsight gera registos que são escritos para armazenamento de Blobs do Azure e as tabelas do Azure. YARN cria os seus próprios registos de execução. Para obter mais informações, consulte [gerir os registos para um cluster do HDInsight](hdinsight-log-management.md#access-the-hadoop-log-files).

### <a name="review-heap-dumps"></a>Reveja as informações de área dinâmica para dados

Área dinâmica para dados capturas contém um instantâneo de memória da aplicação, incluindo os valores das variáveis nessa altura, que são úteis para diagnosticar problemas que ocorram em tempo de execução. Para obter mais informações, consulte [informações de estado da área dinâmica para dados de ativação para os serviços do Hadoop no HDInsight baseado em Linux](hdinsight-hadoop-collect-debug-heap-dump-linux.md).

## <a name="step-6-check-configuration-settings"></a>Passo 6: Verificar as definições de configuração

Clusters do HDInsight são previamente configurados com as predefinições para os serviços relacionados, tais como Hadoop, Hive, HBase e assim sucessivamente. Dependendo do tipo de cluster, a configuração do hardware, o número de nós, os tipos de tarefas estiver a executar e os dados que está a trabalhar com (e como os dados está a ser processados), poderá ter de otimizar a configuração.

<!-- For detailed instructions on optimizing performance configurations for most scenarios, see [Changing configurations with Ambari](hdinsight-changing-configs-via-ambari.md). When using Spark, see [Optimizing Spark jobs for performance](spark/apache-spark-perf.md).  -->

## <a name="step-7-reproduce-the-failure-on-a-different-cluster"></a>Passo 7: Reproduzir a falha de um cluster diferente

Para ajudar a diagnosticar a origem de um erro de cluster, inicie um novo cluster com a mesma configuração e, em seguida, volte a submeter passos a tarefa falhada um por um. Verifique os resultados de cada passo antes de processamento o seguinte. Este método dá-lhe a oportunidade para corrigir e volte a executar um único passo falhado. Este método também tem a vantagem de apenas uma vez a carregar os dados de entrada.

1. Crie um novo cluster de teste com a mesma configuração do cluster com falhas.
2. O primeiro passo de tarefa para o cluster de teste de envio.
3. Quando o passo de concluir o processamento, verifique se existem erros nos ficheiros de registo de passo. Ligar ao nó principal do cluster de teste e ver os ficheiros de registo não existe. Os ficheiros de registo do passo apenas são apresentados depois do passo for executado durante algum tempo, depois de concluída, ou falha.
4. Se foi concluída com êxito o primeiro passo, execute o passo seguinte. Se existirem erros, investigue o erro nos ficheiros de registo. Se esta tiver um erro no código, efetuar a correção e volte a executar o passo. 
5. Continue até todos os passos executado sem erros.
6. Quando tiver concluído a depuração o cluster de teste, elimine-o.

## <a name="next-steps"></a>Passos Seguintes

* [Gerir clusters do HDInsight utilizando a IU da Web do Ambari](hdinsight-hadoop-manage-ambari.md)
* [Analisar os registos do HDInsight](hdinsight-debug-jobs.md)
* [Registo de aplicações de YARN de acesso no HDInsight baseado em Linux](hdinsight-hadoop-access-yarn-app-logs-linux.md)
* [Ativar capturas de área dinâmica para dados de serviços do Hadoop no HDInsight baseado em Linux](hdinsight-hadoop-collect-debug-heap-dump-linux.md)
* [Problemas conhecidos para o cluster do Apache Spark no HDInsight](hdinsight-apache-spark-known-issues.md)
