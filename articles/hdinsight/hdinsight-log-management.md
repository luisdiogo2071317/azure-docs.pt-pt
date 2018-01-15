---
title: Gerir registos para um cluster do HDInsight - Azure HDInsight | Microsoft Docs
description: "Determine os tipos, tamanhos e políticas de retenção para os ficheiros de registo de atividade do HDInsight."
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
ms.openlocfilehash: a161a5c639ff02e1e8a2ea987d9f913ff41c5618
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2018
---
# <a name="manage-logs-for-an-hdinsight-cluster"></a>Gerir registos para um cluster do HDInsight

Um cluster do HDInsight produz uma variedade de ficheiros de registo. Por exemplo, o Apache Hadoop e serviços relacionados, tais como o Apache Spark produzem registos de execução da tarefa de detalhado. Gestão de ficheiros de registo faz parte da manutenção de um cluster do HDInsight em bom estado. Também pode ser requisitos regulamentares para arquivar de registo.  Devido ao número e tamanho dos ficheiros de registo, otimizar o armazenamento de registo e o arquivamento ajuda-o com o serviço Gestão de custo.

Gerir registos de cluster do HDInsight inclui informações sobre todos os aspetos de ambiente de cluster de retenção. Estas informações incluem associados todos os registos de serviço do Azure, configuração de cluster, as informações de execução da tarefa, os Estados de erro e outros dados conforme necessário.

Passos comuns na gestão de registo do HDInsight são:

* Passo 1: Determinar as políticas de retenção de registo
* Passo 2: Gerir registos de configuração de versões de serviço de cluster
* Passo 3: Gerir ficheiros de registo de execução de trabalho de cluster
* Passo 4: Previsão tamanhos de armazenamento de volume de registo e os custos
* Passo 5: Determinar os processos e políticas de arquivo de registo

## <a name="step-1-determine-log-retention-policies"></a>Passo 1: Determinar as políticas de retenção de registo

É o primeiro passo na criação de uma estratégia de gestão de registo de cluster do HDInsight para recolher informações sobre cenários de negócios e requisitos de armazenamento do histórico de execução do trabalho.

### <a name="cluster-details"></a>Detalhes do cluster

Os seguintes detalhes de cluster são úteis para ajudar a recolher informações na sua estratégia de gestão do registo. Recolha estas informações de todos os clusters do HDInsight que criou uma conta do Azure específica.

* Nome do cluster
* Região do cluster e o horário de disponibilidade do Azure
* Estado do cluster, incluindo detalhes da última alteração de estado
* Tipo e número de instâncias de HDInsight especificado para o master, núcleos e os nós de tarefas

Pode obter a maioria destas informações de nível superior no portal do Azure.  Em alternativa, pode utilizar a CLI do Azure para obter informações sobre o cluster(s) HDInsight:

```
    azure hdinsight cluster list
    azure hdinsight cluster show <ClusterName>
```

Também pode utilizar o PowerShell para ver estas informações.  Para obter mais informações, consulte [clusters do Hadoop gerir no HDInsight com o Azure PowerShell](hdinsight-administer-use-powershell.md).

### <a name="understand-the-workloads-running-on-your-clusters"></a>Compreender as cargas de trabalho em execução nos seus clusters

É importante compreender os tipos de carga de trabalho em execução no seu cluster(s) HDInsight conceção apropriada estratégias para cada tipo de registo.

* São as cargas de trabalho experimental (por exemplo, programação ou testes) ou de qualidade de produção?
* Com que frequência são as cargas de trabalho de produção qualidade normalmente executadas?
* São qualquer uma das cargas de trabalho intensivas em recursos e/ou de execução longa?
* Qualquer uma das cargas de trabalho a utilizar um conjunto complexo de serviços do Hadoop para o qual são produzidos vários tipos de registos?
* Qualquer uma das cargas de trabalho associou requisitos de linhagem execução regulamentação?

### <a name="example-log-retention-patterns-and-practices"></a>Práticas e padrões de retenção do registo de exemplo

* Considere manter linhagem de dados de controlo, adicionando um identificador para cada entrada de registo ou através de outras técnicas. Isto permite-lhe a origem dos dados e a operação de efectuar o rastreio e siga os dados através de cada fase, para compreender a consistência e a validade.

* Considere como recolher registos do cluster, ou a partir de mais de um cluster e agrupamento-los para fins tais como a auditoria, monitorização, planeamento e alertas. Poderá utilizar uma solução personalizada para aceder e transferir os ficheiros de registo regularmente e combinar e analisá-las para fornecer uma apresentação do dashboard. Também pode adicionar capacidades adicionais para alertas de segurança ou a deteção de falha. Pode criar estes utilitários utilizando o PowerShell, o HDInsight SDKs ou o código que acede ao modelo de implementação clássica do Azure.

* Considere se uma solução de monitorização ou serviço seria uma vantagem útil. O Microsoft System Center fornece um [pacote de gestão do HDInsight](https://www.microsoft.com/download/details.aspx?id=42521). Também pode utilizar ferramentas de terceiros, tais como Chukwa e Ganglia para recolher e centralizar o processamento de registos. Muitas empresas oferecem serviços monitorizar soluções de macrodados baseado no Hadoop, por exemplo Centerity, Compuware APM, Sematext SPM e Zettaset Orchestrator.

## <a name="step-2-manage-cluster-service-versions-and-view-script-action-logs"></a>Passo 2: Gerir versões do serviço de cluster e ver registos de ação de Script

Um cluster do HDInsight típico utiliza vários serviços e pacotes de software open source (como o Apache HBase, o Apache Spark e assim sucessivamente). Para algumas cargas de trabalho, tais como bioinformatics, poderá ser necessário para manter histórico de registos de configuração de serviço para além dos registos de execução da tarefa.

### <a name="view-cluster-configuration-settings-with-the-ambari-ui"></a>Ver definições de configuração de cluster com a IU do Ambari

Apache Ambari simplifica a gestão, configuração e monitorização de um cluster do HDInsight, fornecendo uma web IU e uma API REST. Ambari está incluído nos clusters do HDInsight baseado em Linux. Selecione o **Cluster Dashboard** painel na página de HDInsight portal do Azure para abrir o**' Dashboards de clusters** página de ligação.  Em seguida, selecione o **dashboard de cluster do HDInsight** painel para abrir a IU do Ambari.  Lhe forem pedidas as credenciais de início de sessão do cluster.

Para abrir uma lista de vistas de serviço, selecione o **vistas do Ambari** painel na página do portal do Azure para o HDInsight.  Esta lista varia consoante as bibliotecas que tenha instalado.  Por exemplo, poderá ver o Gestor de filas YARN, a vista do Hive e a vista de Tez.  Selecione qualquer ligação de serviço para ver informações do serviço de configuração e.  A IU do Ambari **pilha e versão** página fornece informações sobre a configuração dos serviços de cluster e histórico da versão do serviço. Para navegar para esta secção da IU do Ambari, selecione o **Admin** menu e, em seguida, **pilhas e versões**.  Selecione o **versões** separador para ver informações de versão do serviço.

![A pilha e versões](./media/hdinsight-log-management/stack-versions.png)

Utilizando a IU do Ambari, pode transferir a configuração para os serviços de qualquer (ou todos os) em execução num anfitrião específico (ou nós) no cluster.  Selecione o **anfitriões** menu, em seguida, a ligação para o anfitrião de interesse. Na página de nesse anfitrião, selecione o **anfitrião ações** botão e, em seguida, **folhas de cliente transferir**. 

![Folhas de cliente de anfitrião](./media/hdinsight-log-management/client-configs.png)

### <a name="view-the-script-action-logs"></a>Ver os registos de ação de script

HDInsight [ações de script](hdinsight-hadoop-customize-cluster-linux.md) executar scripts num cluster, manualmente ou quando especificado. Por exemplo, ações de script podem ser utilizadas para instalar o software adicional no cluster ou para alterar as definições de configuração os valores predefinidos. Os registos de ação de script podem fornecer informações sobre os erros que ocorreram durante a configuração do cluster bem como as alterações de definições de configuração que podem afetar o desempenho de cluster e disponibilidade.  Para ver o estado de uma ação de script, selecione o **ops** botão na sua IU do Ambari ou acesso regista o estado na conta do storage predefinida. Os registos de armazenamento estão disponíveis em `/STORAGE_ACCOUNT_NAME/DEFAULT_CONTAINER_NAME/custom-scriptaction-logs/CLUSTER_NAME/DATE`.

## <a name="step-3-manage-the-cluster-job-execution-log-files"></a>Passo 3: Gerir os ficheiros de registo de execução de trabalho de cluster

O passo seguinte é rever os ficheiros de registo de execução de tarefas para vários serviços.  Os serviços podem incluir Apache HBase, o Apache Spark e muitas outras. Um cluster do Hadoop produz um grande número de registos verbosos, para que determinar quais os registos são úteis (e que não estão) pode ser morosa.  Noções sobre o sistema de registo é importante para a gestão de destino dos ficheiros de registo.  Segue-se um ficheiro de registo de exemplo.

![Exemplo de ficheiro de registo do HDInsight](./media/hdinsight-troubleshoot-failed-cluster/logs.png)

### <a name="access-the-hadoop-log-files"></a>Aceder aos ficheiros de registo do Hadoop

HDInsight armazena os ficheiros de registo no sistema de ficheiros de cluster e no armazenamento do Azure. Pode examinar os ficheiros de registo do cluster abrindo uma ligação SSH ao cluster e navegação no sistema de ficheiros, ou utilizando o portal de Hadoop YARN Estado no servidor remoto nó principal. Pode examinar os ficheiros de registo no armazenamento do Azure, utilizando qualquer uma das ferramentas que podem aceder e transferir dados de armazenamento do Azure. Os exemplos são AZCopy, CloudXplorer e o Explorador de servidores do Visual Studio. Pode também utilizar o PowerShell e as bibliotecas de cliente de armazenamento do Azure ou os SDKs de .NET do Azure, para aceder a dados no armazenamento de Blobs do Azure.

Hadoop executa o trabalho de tarefas como *tarefas tentativas* em vários nós no cluster. HDInsight pode iniciar tarefas speculative tentativas, terminar quaisquer outras tentativas de tarefas que não concluído primeiro. Isto gera significativa atividade que é registada para o controlador, stderr e syslog registo ficheiros no-a-momento. Além disso, várias tentativas de tarefas em execução em simultâneo, mas um ficheiro de registo pode apresentar apenas resultados de forma linear.

#### <a name="hdinsight-logs-written-to-azure-tables"></a>Registos de HDInsight escritos para as tabelas do Azure

Os registos de escrita de tabelas do Azure fornecem informações sobre o que acontece com um cluster do HDInsight. Quando cria um cluster do HDInsight baseado em Linux, são automaticamente criadas seis tabelas no armazenamento de tabelas predefinido:

* hdinsightagentlog
* syslog
* daemonlog
* hadoopservicelog
* ambariserverlog
* ambariagentlog

#### <a name="hdinsight-logs-written-to-azure-blob-storage"></a>Registos de HDInsight escritos para armazenamento de Blobs do Azure

Clusters do HDInsight são configurados para escrever os registos de tarefas para uma conta de armazenamento de Blobs do Azure para qualquer tarefa que é submetida utilizando cmdlets do Azure PowerShell ou a submissão da tarefa APIs .NET.  Se submeter as tarefas através de SSH para o cluster, em seguida, as informações de registo de execução são armazenadas nas tabelas do Azure, tal como descrito na secção anterior.

Para além dos ficheiros de registo de núcleos gerados pelo HDInsight, serviços instalados, tais como YARN gerar ficheiros de registo de execução de tarefas.  O número e tipo de ficheiros de registo depende os serviços instalados.  Serviços comuns são Apache HBase, o Apache Spark e assim sucessivamente.  Investigue os ficheiros de execução do registo de tarefas para cada serviço compreender que o registo global ficheiros disponível no seu cluster.  Cada serviço tem os suas próprias métodos exclusivos do registo e localizações para armazenar ficheiros de registo.  Por exemplo, os detalhes para aceder aos ficheiros de registo mais comuns do serviço (a partir do YARN) são abordados na secção seguinte.

### <a name="hdinsight-logs-generated-by-yarn"></a>Registos de HDInsight gerados pelo YARN

YARN agrega registos através de todos os contentores num nó de trabalho e armazena esses registos como um ficheiro de registo agregados por nó de trabalho. Esse registo é armazenado no sistema de ficheiros predefinido após a conclusão de uma aplicação. A aplicação pode utilizar centenas ou milhares de contentores, mas os registos para todos os contentores que são executados num nó único do worker sempre são agregados para um único ficheiro. Não há apenas um registo por nó de trabalho utilizada pela sua aplicação. Agregação de registo está ativada por predefinição em clusters do HDInsight versão 3.0 e posteriores. Agregados registos estão localizados no armazenamento de predefinido para o cluster.

```
    /app-logs/<user>/logs/<applicationId>
```

Os registos de agregados não são diretamente legíveis, como são escritos num formato binário TFile indexado por contentor. Utilize os registos YARN ResourceManager ou ferramentas da CLI para ver estes registos como texto não encriptado para aplicações ou contentores de interesse.

#### <a name="yarn-cli-tools"></a>Ferramentas da CLI do YARN

Para utilizar as ferramentas da CLI do YARN, tem primeiro de ligar ao cluster do HDInsight através de SSH. Especifique o `<applicationId>`, `<user-who-started-the-application>`, `<containerId>`, e `<worker-node-address>` informações quando executar estes comandos. Pode ver os registos como texto simples com um dos seguintes comandos:

```bash
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>
```

#### <a name="yarn-resourcemanager-ui"></a>IU do YARN ResourceManager

A IU do YARN ResourceManager é executado no nó principal do cluster e é acedida através da IU da web do Ambari. Utilize os seguintes passos para ver os registos YARN:

1. Num browser, navegue para `https://CLUSTERNAME.azurehdinsight.net`. Substitua CLUSTERNAME com o nome do cluster do HDInsight.
2. Na lista de serviços no lado esquerdo, selecione o YARN.
3. Na lista pendente ligações rápidas, selecione um de nós principais do cluster e, em seguida, selecione **ResourceManager registos**. É-lhe apresentada uma lista de ligações para registos YARN.

## <a name="step-4-forecast-log-volume-storage-sizes-and-costs"></a>Passo 4: Previsão tamanhos de armazenamento de volume de registo e os custos

Depois de concluir os passos anteriores, tem um conhecimento dos tipos e os volumes de ficheiros de registo que é produzir cluster(s) do HDInsight.

Em seguida, analise o volume de dados de registo em localizações de armazenamento de chave de registo durante um período de tempo. Por exemplo, pode analisar volume e crescimento mais de 30-60-90 períodos de dia.  Gravar estas informações numa folha de cálculo ou utilizar outras ferramentas, como o Visual Studio, o Explorador de armazenamento do Azure ou o Power Query para Excel. Para obter mais informações, consulte [HDInsight analisar registos](hdinsight-debug-jobs.md).  

Agora tem informações suficientes para criar uma estratégia de gestão do registo para os registos de chaves.  Utilize a folha de cálculo (ou a ferramenta escolhidas) previsão ambas crescimento de tamanho do registo e iniciar os custos do serviço do Azure de armazenamento, passa.  Considere também os requisitos de retenção de registo para o conjunto de registos que está a examinar.  Agora pode reforecast registo futuras os custos de armazenamento, após determinar os ficheiros de registo podem ser eliminados (se aplicável) e os registos devem ser retidos e são arquivados menos dispendioso armazenamento do Azure.

## <a name="step-5-determine-log-archive-policies-and-processes"></a>Passo 5: Determinar os processos e políticas de arquivo de registo

Após determinar os ficheiros de registo podem ser eliminados, pode ajustar os parâmetros de registo em vários serviços do Hadoop para eliminar automaticamente os ficheiros de registo após um período de tempo especificado.

Para determinados ficheiros de registo, pode utilizar um ficheiro de registo um preço inferior arquivar abordagem. Para os registos de atividade do Azure Resource Manager, pode explorar esta abordagem no portal do Azure.  Configurar arquivo dos registos do ARM, selecionando o **registo de atividade**' ligação no portal do Azure para a instância do HDInsight.  No topo da página de pesquisa de registo de atividade, selecione o **exportar** item de menu para abrir o **exportar registo de atividade** painel.  Preencha a subscrição, região, se pretende exportar para uma conta de armazenamento e quantos dias a manter os registos. No painel mesmo, também pode indicar se pretende exportar para um hub de eventos. 

![Exportar ficheiros de registo](./media/hdinsight-log-management/archive.png)

Em alternativa, pode aplicar o script registo Arquivar com o PowerShell.  Para obter um exemplo de script do PowerShell, consulte [da automatização do Azure do arquivo de registos para o Blob Storage do Azure](https://gallery.technet.microsoft.com/scriptcenter/Archive-Azure-Automation-898a1aa8).

### <a name="accessing-azure-storage-metrics"></a>Aceder às métricas do storage do Azure

Armazenamento do Azure pode ser configurado para operações de armazenamento de registo e acesso. Pode utilizar estes registos muito detalhados para a capacidade de monitorização e planeamento e para auditar pedidos para o armazenamento. As informações com sessão iniciada incluem detalhes de latência, permitindo-lhe monitorizar e otimizar o desempenho das suas soluções.
Pode utilizar o SDK .NET do Hadoop para examinar os ficheiros de registo gerados para o armazenamento do Azure que contém os dados para um cluster do HDInsight.

### <a name="control-the-size-and-number-of-backup-indexes-for-old-log-files"></a>Controlar o tamanho e número de índices de cópia de segurança para os ficheiros de registo antigos

Para controlar o tamanho e o número de ficheiros de registo retidos, defina as seguintes propriedades do `RollingFileAppender`:

* `maxFileSize`é o tamanho do ficheiro, acima, que o ficheiro é revertido crítico. O valor predefinido é de 10 MB.
* `maxBackupIndex`Especifica o número de ficheiros de cópia de segurança criados, predefinição 1.

### <a name="other-log-management-techniques"></a>Outras técnicas de gestão do registo

Para evitar a execução sem espaço em disco, pode utilizar algumas ferramentas de SO como `logrotate` para gerir o processamento de ficheiros de registo. Pode configurar `logrotate` para executar diariamente, a compressão do registo de ficheiros e remover aqueles antigos. A abordagem depende nos seus requisitos, tais como quanto a manter o logfiles em nós locais. 

Também pode verificar se o registo de depuração está ativado para um ou mais serviços, que aumenta significativamente o tamanho do registo de saída. 

Para recolher os registos de todos os nós para uma localização central, pode criar um fluxo de dados, tais como a ingestão de relacionadas todas as entradas de registo para Solr.

## <a name="next-steps"></a>Passos Seguintes

* [Monitorização e registo prática para o HDInsight](https://msdn.microsoft.com/library/dn749790.aspx)
* [Registo de aplicações de YARN de acesso no HDInsight baseado em Linux](hdinsight-hadoop-access-yarn-app-logs-linux.md)
* [Como controlar o tamanho dos ficheiros de registo para vários componentes do Hadoop](https://community.hortonworks.com/articles/8882/how-to-control-size-of-log-files-for-various-hdp-c.html)
