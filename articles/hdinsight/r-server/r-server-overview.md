---
title: Introdução ao servidor R no HDInsight do Azure | Microsoft Docs
description: Saiba como utilizar o servidor R no HDInsight para criar aplicações para análise de macrodados.
services: hdinsight
documentationcenter: ''
author: nitinme
manager: cgronlun
editor: cgronlun
ms.assetid: 6dc21bf5-4429-435f-a0fb-eea856e0ea96
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 03/23/2018
ms.author: nitinme
ms.openlocfilehash: 19c286db9a8a2aa537badc83d98a1b74b73e9873
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
ms.locfileid: "31414723"
---
# <a name="introduction-to-r-server-and-open-source-r-capabilities-on-hdinsight"></a>Introdução ao servidor R e as capacidades de R de open source do HDInsight

R Server da Microsoft (também saber como Microsoft Machine Learning Server) está disponível como uma opção de implementação quando criar clusters do HDInsight no Azure. O tipo de cluster que fornece esta opção é denominado **R Server**. Esta capacidade fornece cientistas de dados, statisticians e programadores de R com acesso a pedido para dimensionável, distribuída métodos de análise no HDInsight.

[!INCLUDE [hdinsight-price-change](../../../includes/hdinsight-enhancements.md)]

Servidor R no HDInsight fornece funcionalidades mais recentes para análise R baseado em conjuntos de dados de praticamente qualquer tamanho, carregado para o armazenamento de Blobs do Azure ou de Data Lake. Uma vez que o cluster de servidor R está incorporado no R open source, as aplicações com base em R que cria podem tirar partido de qualquer 8000 + open source R pacotes. Rotinas no ScaleR, pacote de análise de macrodados da Microsoft incluído com o servidor R, também estão disponíveis.

O nó de extremidade de um cluster fornece um local conveniente para ligar ao cluster e executar os scripts de R. Com um nó de extremidade, tem a opção de executar as funções distribuídas parallelized do ScaleR em todos os núcleos do servidor edge nó. Também pode executá-los em todos os nós do cluster através Hadoop mapa reduzir do ScaleR ou contextos de computação de Spark.

Os modelos ou predições que resultam de análise podem ser transferidas para utilização no local. Estes podem também ser operationalized noutro local no Azure, em particular através de [Azure Machine Learning Studio](http://studio.azureml.net) [serviço web](../../machine-learning/studio/publish-a-machine-learning-web-service.md).

## <a name="get-started-with-r-server-on-hdinsight"></a>Introdução ao R Server no HDInsight

Para criar um cluster de servidor R no HDInsight do Azure, selecione o **R Server** tipo de cluster, ao criar um cluster do HDInsight no portal do Azure. O tipo de cluster de servidor R inclui R Server em nós do cluster de dados e no nó de extremidade, o que funciona como uma zona de destino para a análise baseada em servidor R. Consulte [introdução ao servidor R no HDInsight](r-server-get-started.md) para obter instruções sobre como criar o cluster.

## <a name="why-choose-r-server-in-hdinsight"></a>Por que motivo escolhê servidor R no HDInsight?

R Server no HDInsight fornece a opção para utilizar o R Server no Azure. R Server inclui:

+ **A melhor inovação AI da Microsoft e de código aberto**

  Microsoft strives tornar o AI acessível a cada indivíduo e a organização. R Server inclui um conjunto avançado de altamente dimensionável e distribuído, conjunto de algoritmos, tais como [RevoscaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler), [revoscalepy](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/revoscalepy-package), e [microsoftML](https://docs.microsoft.com/machine-learning-server/python-reference/microsoftml/microsoftml-package) que podem trabalhar em tamanhos de dados maiores que o tamanho de memória física e executada uma ampla variedade de plataformas de forma distribuída. Saiba mais sobre a recolha de Microsoft's personalizado [pacotes R](https://docs.microsoft.com/machine-learning-server/r-reference/introducing-r-server-r-package-reference) e [pacotes de Python](https://docs.microsoft.com/machine-learning-server/python-reference/introducing-python-package-reference) incluídos com o produto.
  
  R Server bridges estes inovações da Microsoft e os que são provenientes da Comunidade (toolkits R, Python e AI) open source para todos os por cima de uma plataforma única de nível empresarial. Qualquer pacote de R ou Python aprendizagem de código aberto pode trabalhar lado a lado com qualquer inovação proprietária da Microsoft. 

+ **Operationalization Simple, segura e alta escala e administração**

  As empresas depender paradigmas operationalization tradicional e ambientes acabar investir muito tempo e esforço para esta área. Pontos de tensão, muitas vezes, resultavam os custos inflated e atrasos incluem: a hora de tradução de modelos, iterações para mantê-las válido e atuais, aprovação de regulamentação, gestão de permissões através de operationalization.

  R Server oferece melhor na classe [operationalization](https://docs.microsoft.com/machine-learning-server/what-is-operationalization) – desde o momento em que um modelo de machine learning é concluído, demora apenas alguns cliques para gerar serviços APIs da web. Estes [serviços web](https://docs.microsoft.com/machine-learning-server/operationalize/concept-what-are-web-services) estão alojados numa grelha de servidor no local ou na nuvem e pode ser integrado com aplicações de linha de negócio. A capacidade de implementar permite uma grelha elástico dimensionamento totalmente integrada com as necessidades da sua empresa, tanto para batch e classificação em tempo real. Para obter instruções, consulte [Operacionalizar servidor R no HDInsight](r-server-operationalize.md).

+ **Ações de envolvimento do ecossistema avançada para fornecer ao cliente efetuada com êxito ideal custo total de propriedade**

  Indivíduos aderir journey de efetuar as respetivas aplicações inteligente ou simplesmente intenção saber o novo universo de AI e do machine learning, tem dos recursos corretos para o ajudar a começar a utilizar. Para além desta documentação, a Microsoft fornece vários recursos de aprendizagem e tem parte vários parceiros de formação para o ajudar a conhecer e passam a ser produtivos rapidamente.


## <a name="key-features-of-r-server-on-hdinsight"></a>Principais funcionalidades de servidor R no HDInsight

As funcionalidades seguintes estão incluídas no servidor R no HDInsight.

| Categoria de funcionalidade | Descrição |
|------------------|-------------|
| R-ativado | [Pacotes de R](https://docs.microsoft.com/machine-learning-server/r-reference/introducing-r-server-r-package-reference) para soluções escritas no R, com uma distribuição de código aberto do R e a infraestrutura de tempo de execução para a execução do script. |
| Ativar o Python | [Módulos do Python](https://docs.microsoft.com/machine-learning-server/python-reference/introducing-python-package-reference) para soluções de escritas no Python, com uma distribuição de código aberto da infraestrutura de tempo de execução para a execução do script e Python.  
| [Modelos de formação previamente](https://docs.microsoft.com/machine-learning-server/install/microsoftml-install-pretrained-models) | Para análise visual e análise de dados de sentimento de texto, pronto para pontuar os dados fornece. |
| [Implementar e consumir](r-server-operationalize.md) | Operacionalizar o seu servidor e implementar soluções como um serviço web. |
| [execução remota](r-server-hdinsight-manage.md#connect-remotely-to-microsoft-ml-server-or-client) | Iniciem sessões remotas no servidor de R na sua rede da sua estação de trabalho do cliente. |


## <a name="data-storage-options-for-r-server-on-hdinsight"></a>Opções de armazenamento de dados para o servidor R no HDInsight

Armazenamento de predefinido para o sistema de ficheiros do HDFS de clusters do HDInsight pode ser associado a uma conta de armazenamento do Azure ou um Azure Data Lake Store. Esta associação garante que qualquer dados foi carregados para o cluster de armazenamento durante a análise é efetuado persistente e os dados estão disponíveis, mesmo depois do cluster é eliminado. Existem várias ferramentas para processar a transferência de dados para a opção de armazenamento que selecionar, incluindo a instalações de carregamento com base no portal da conta de armazenamento e a [AzCopy](../../storage/common/storage-use-azcopy.md) utilitário.

Tem a opção de adição de acesso para o Blob adicional e armazena o Data lake durante o processo, independentemente da opção de armazenamento primário em utilização de aprovisionamento de cluster. Consulte [introdução ao servidor R no HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-get-started) para obter informações sobre a adição de acesso a contas adicionais. Consulte o suplementares [opções de armazenamento do Azure para o servidor R no HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-storage) artigo para obter mais informações sobre como utilizar várias contas de armazenamento.

Também pode utilizar [ficheiros do Azure](../../storage/files/storage-how-to-use-files-linux.md) como uma opção de armazenamento para utilização no nó de extremidade. Ficheiros do Azure permite-lhe montar uma partilha de ficheiros que foi criada no armazenamento do Azure para o sistema de ficheiros do Linux. Para obter mais informações sobre estas opções de armazenamento de dados para o servidor R num cluster do HDInsight, consulte [opções de armazenamento do Azure para o servidor R no HDInsight](r-server-storage.md).

## <a name="access-machine-learning-server-on-the-cluster"></a>Servidor do acesso Machine Learning no cluster

Pode ligar ao servidor do Microsoft Machine Learning no nó de extremidade utilizando um browser. É instalado por predefinição durante a criação do cluster. Para obter mais informações, consulte [obter stared com o servidor R no HDInsight](r-server-get-started.md). Também pode ligar ao servidor de ML na linha de comandos utilizando SSH/PuTTY para aceder à consola do R. 

## <a name="develop-and-run-r-scripts"></a>Desenvolver e executar scripts de R

Os scripts de R criar e executar podem utilizar qualquer um dos 8000 + open source R pacotes além rotinas paralelizadas e distribuídos disponíveis na biblioteca ScaleR. Em geral, um script que é executado com o servidor R num nó de extremidade é executado dentro do interpretador de R nesse nó. As exceções são esses passos que tem de chamar uma função de ScaleR com um contexto de computação que está definido para o Hadoop mapa reduzir (RxHadoopMR) ou Spark (RxSpark). Neste caso, a função é executada de forma distribuída entre os nós do cluster de dados (tarefas) que estão associados com os dados referenciados. Para obter mais informações sobre as opções de contexto de computação diferentes, consulte [computação as opções de contexto para o servidor R no HDInsight](r-server-compute-contexts.md).

## <a name="operationalize-a-model"></a>Operacionalizar um modelo

Quando a modelação de dados estiver concluída, pode operacionalizar o modelo para fazer predições para novos dados a partir do Azure e no local. Este processo é conhecido como a classificação. A classificação pode ser feita no HDInsight, do Azure Machine Learning ou no local.

### <a name="score-in-hdinsight"></a>Pontuação no HDInsight
Para Pontuar no HDInsight, escreva uma função de R que chama o seu modelo para fazer predições para um novo ficheiro de dados tiver carregado para a sua conta de armazenamento. Em seguida, guarde serão as predições novamente para a conta de armazenamento. Pode executar este rotina a pedido no nó de extremidade do cluster ou utilizando uma tarefa agendada.

### <a name="score-in-azure-machine-learning-aml"></a>Pontuação no Azure Machine Learning (AML)
Para pontuar utilizando o Azure Machine Learning, utilize o pacote do Azure Machine Learning R de open source conhecido como [AzureML](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) para publicar o seu modelo como um serviço web do Azure. Para sua comodidade, este pacote é pré-instalados no nó de extremidade. Em seguida, utilize as instalações no Azure Machine Learning para criar uma interface de utilizador para o serviço web e, em seguida, chame o serviço web, conforme necessário para classificação.

Se escolher esta opção, tem de converter os objetos de modelo ScaleR objetos equivalentes de open source de modelo para utilização com o serviço web. Utilize as funções de adesão ScaleR, tais como `as.randomForest()` para modelos com base em ensemble, para esta conversão.

### <a name="score-on-premises"></a>Pontuação no local
Para Pontuar no local depois de criar o seu modelo, pode serializar o modelo de R, transferi-lo, anular a serialização e, em seguida, utilizá-lo para a classificação de dados de novo. Pode Pontuar novos dados ao utilizar a abordagem descrita anteriormente no [classificação no HDInsight](#scoring-in-hdinsight) ou utilizando [DeployR](https://deployr.revolutionanalytics.com/).

## <a name="maintain-the-cluster"></a>Manter o cluster

### <a name="install-and-maintain-r-packages"></a>Instalar e manter os pacotes de R

A maioria dos pacotes de R que utilizar é necessárias no nó de extremidade desde a maior parte dos passos para os scripts de R executados não existe. Para instalar pacotes de R adicionais num nó de extremidade, pode utilizar o habitual `install.packages()` método em R.

Se estiver apenas a utilizar rotinas da biblioteca de ScaleR no cluster, não normalmente, tem de instalar os pacotes de R adicionais em nós de dados. No entanto, poderá ser necessário pacotes adicionais para suportar a utilização de **rxExec** ou **RxDataStep** execução em nós de dados.

Nestes casos, os pacotes adicionais podem ser instalados com uma ação de script depois de criar o cluster. Para obter mais informações, consulte [Gerir servidor R no cluster do HDInsight](r-server-hdinsight-manage.md).

### <a name="change-hadoop-mapreduce-memory-settings"></a>Alterar as definições de memória de MapReduce de Hadoop

Um cluster pode ser modificado para alterar a quantidade de memória que está disponível para o servidor R quando está a executar uma tarefa de MapReduce. Para modificar um cluster, utilize a IU do Apache Ambari que está disponível através do painel do portal do Azure para o cluster. Para obter instruções sobre como aceder à IU do Ambari para o cluster, consulte [gerir clusters do HDInsight utilizando a IU da Web do Ambari](../hdinsight-hadoop-manage-ambari.md).

Também é possível alterar a quantidade de memória que está disponível para o servidor R utilizando comutadores do Hadoop na chamada para **RxHadoopMR** da seguinte forma:

    hadoopSwitches = "-libjars /etc/hadoop/conf -Dmapred.job.map.memory.mb=6656"  

### <a name="scale-your-cluster"></a>Dimensionar o cluster

Um cluster existente do servidor R no HDInsight pode ser escalado para cima ou para baixo através do portal. Por como aumentar verticalmente, pode obter a capacidade adicional que poderá ser necessário para tarefas de processamento superiores ou, pode dimensionar novamente um cluster quando estiver inativo. Para obter instruções sobre como dimensionar um cluster, consulte [gerir clusters do HDInsight](../hdinsight-administer-use-portal-linux.md).

### <a name="maintain-the-system"></a>Manter o sistema

Manutenção para aplicar patches de SO e outras atualizações é efetuada nas VMs Linux subjacente de um cluster de HDInsight durante off-hours. Normalmente, manutenção é feita às 3:30 AM (com base na hora local para a VM), cada segunda-feira e a quinta-feira. As atualizações são executadas de forma a que não afetam mais de um trimestre do cluster de cada vez.  

Uma vez que os nós principais são redundantes e nem todos os nós de dados são afetados, as tarefas que estão em execução durante este tempo, poderão abrandar. No entanto, ainda deve executar para conclusão. Qualquer personalizadas de software ou dados locais que tiver são preservados entre estes eventos de manutenção ocorra uma falha catastrófica que necessita de uma reconstrução do cluster.

## <a name="ide-options-for-r-server-on-an-hdinsight-cluster"></a>Opções de IDE para o servidor R num cluster do HDInsight

O nó de extremidade de Linux de um cluster do HDInsight é a zona de destino para a análise baseada em R. Recentes versões do HDInsight fornecem uma instalação padrão do servidor do RStudio no nó de extremidade como um IDE baseada no browser. Utilização do RStudio Server como um IDE para o desenvolvimento e a execução de scripts de R podem ser consideravelmente mais produtivos e permite que apenas utilizando a consola do R.

Além disso, pode instalar um IDE de ambiente de trabalho e utilizá-lo para aceder ao cluster através da utilização de um contexto de computação de MapReduce ou Spark remoto. As opções incluem da Microsoft [R Tools para Visual Studio](https://www.visualstudio.com/features/rtvs-vs.aspx) (RTVS), do RStudio e Walware baseado no Eclipse [StatET](http://www.walware.de/goto/statet).

Por último, podem aceder a consola de R no nó de extremidade, escrevendo **R** na linha de comandos do Linux depois de se ligar através de SSH ou PuTY. Quando utilizar a interface da consola, é conveniente executar um editor de texto para o desenvolvimento de script do R na outra janela e cortar e colar secções do seu script na consola do R conforme necessário.

## <a name="pricing"></a>Preços

Os preços que estão associados um cluster do HDInsight com o servidor de R estão estruturados do mesmo modo para que os preços para os clusters do HDInsight standard. Têm como base o dimensionamento de VMs subjacentes entre o nome, dados e nós de limite, com a adição de um uplift horas de núcleo. Para obter mais informações sobre preços do HDInsight, consulte [preços do HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre como utilizar o R Server nos clusters do HDInsight, consulte os seguintes tópicos:

* [Introdução ao cluster de servidor R no HDInsight](r-server-get-started.md)
* [Opções do contexto de cálculo para o cluster do R Server no HDInsight](r-server-compute-contexts.md)
* [Opções do Armazenamento do Azure para o cluster do R Server no HDInsight](r-server-storage.md)
