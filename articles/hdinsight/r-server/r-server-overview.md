---
title: Introdução aos serviços de ML no Azure HDInsight | Microsoft Docs
description: Saiba como utilizar os serviços de ML no HDInsight para criar aplicações para análise de macrodados.
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
ms.date: 06/27/2018
ms.author: nitinme
ms.openlocfilehash: 9633a7df1cb72f3e9e5ee79be0c332565e7e8f2a
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37054107"
---
# <a name="introduction-to-ml-services-and-open-source-r-capabilities-on-hdinsight"></a>Introdução aos serviços de ML e as capacidades de R de open source do HDInsight

> [!NOTE]
> Em Setembro de 2017, Microsoft R Server foi lançada com o novo nome de **Microsoft Machine Learning Server** ou servidor de ML. Por conseguinte, o cluster de servidor R no HDInsight é agora designada **serviços Machine Learning** ou **serviços ML** cluster no HDInsight. Para obter mais informações sobre a alteração do nome de servidor R, consulte [Microsoft R Server está agora Microsoft Machine Learning Server](https://docs.microsoft.com/machine-learning-server/rebranding-microsoft-r-server#get-support-for-r-server).

Servidor do Microsoft Machine Learning está disponível como uma opção de implementação quando criar clusters do HDInsight no Azure. O tipo de cluster que fornece esta opção é denominado **serviços ML**. Esta capacidade fornece cientistas de dados, statisticians e programadores de R com acesso a pedido para dimensionável, distribuída métodos de análise no HDInsight.

[!INCLUDE [hdinsight-price-change](../../../includes/hdinsight-enhancements.md)]

Serviços de ML no HDInsight fornece funcionalidades mais recentes para análise de R nos conjuntos de dados de praticamente qualquer tamanho, carregado para o armazenamento de Blobs do Azure ou de Data Lake. Uma vez que o cluster de serviços de ML está incorporado no R open source, as aplicações com base em R que cria podem tirar partido de qualquer 8000 + open source R pacotes. Rotinas no ScaleR, o pacote de análise de macrodados da Microsoft também estão disponíveis.

O nó de extremidade de um cluster fornece um local conveniente para ligar ao cluster e executar os scripts de R. Com um nó de extremidade, tem a opção de executar as funções distribuídas parallelized do ScaleR em todos os núcleos do servidor edge nó. Também pode executá-los em todos os nós do cluster através Hadoop mapa reduzir do ScaleR ou contextos de computação de Spark.

Os modelos ou predições que resultam de análise podem ser transferidas para utilização no local. Estes podem também ser operationalized noutro local no Azure, em particular através de [Azure Machine Learning Studio](http://studio.azureml.net) [serviço web](../../machine-learning/studio/publish-a-machine-learning-web-service.md).

## <a name="get-started-with-ml-services-on-hdinsight"></a>Começar com os serviços de ML no HDInsight

Para criar um cluster de ML serviços no Azure HDInsight, selecione o **serviços ML** tipo de cluster, ao criar um cluster do HDInsight no portal do Azure. O tipo de cluster de serviços de ML inclui ML servidor em nós do cluster de dados e no nó de extremidade, o que funciona como uma zona de destino para a análise baseado em serviços de ML. Consulte [introdução aos serviços de ML no HDInsight](r-server-get-started.md) para obter instruções sobre como criar o cluster.

## <a name="why-choose-ml-services-in-hdinsight"></a>Por que motivo escolhê ML serviços no HDInsight?

Serviços de ML no HDInsight fornece as seguintes vantagens:

### <a name="ai-innovation-from-microsoft-and-open-source"></a>Inovação AI da Microsoft e de open source

  Serviços de ML inclui altamente dimensionável e distribuído, conjunto de algoritmos, tais como [RevoscaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler), [revoscalepy](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/revoscalepy-package), e [microsoftML](https://docs.microsoft.com/machine-learning-server/python-reference/microsoftml/microsoftml-package) que podem trabalhar em tamanhos de dados superiores a tamanho da memória física e executada uma ampla variedade de plataformas de forma distribuída. Saiba mais sobre a recolha de Microsoft's personalizado [pacotes R](https://docs.microsoft.com/machine-learning-server/r-reference/introducing-r-server-r-package-reference) e [pacotes de Python](https://docs.microsoft.com/machine-learning-server/python-reference/introducing-python-package-reference) incluídos com o produto.
  
  Serviços de ML bridges estes inovações da Microsoft e contribuições provenientes da Comunidade de open source (toolkits R, Python e AI) todos os por cima de uma plataforma única de nível empresarial. Qualquer pacote de R ou Python open source aprendizagem pode trabalhar side by side with qualquer inovação proprietária da Microsoft.

### <a name="simple-secure-and-high-scale-operationalization-and-administration"></a>Operationalization Simple, segura e alta escala e administração

  As empresas depender paradigmas tradicionais e ambientes de investem muito tempo e esforço para operationalization. Isto resulta em custos inflated e atrasa incluindo o tempo de tradução de modelos, iterações para mantê-las válido e aprovação atual, de regulamentação e gerir permissões através de operationalization.

  Serviços de ML oferece o nível de enterprise [operationalization](https://docs.microsoft.com/machine-learning-server/what-is-operationalization), em que, depois de concluída um modelo de machine learning, demora apenas alguns cliques para gerar serviços APIs da web. Estes [serviços web](https://docs.microsoft.com/machine-learning-server/operationalize/concept-what-are-web-services) estão alojados numa grelha server na nuvem e pode ser integrado com aplicações de linha de negócio. A capacidade de implementar permite uma grelha elástico dimensionamento totalmente integrada com as necessidades da sua empresa, tanto para batch e classificação em tempo real. Para obter instruções, consulte [Operacionalizar serviços ML no HDInsight](r-server-operationalize.md).

<!---
* **Deep ecosystem engagements to deliver customer success with optimal total cost of ownership**

  Individuals embarking on the journey of making their applications intelligent or simply wanting to learn the new world of AI and machine learning, need the right resources to help them get started. In addition to this documentation, Microsoft provides several learning resources and has engaged several training partners to help you ramp up and become productive quickly.
--->

## <a name="key-features-of-ml-services-on-hdinsight"></a>Principais funcionalidades de ML serviços no HDInsight

As funcionalidades seguintes estão incluídas nos serviços de ML no HDInsight.

| Categoria de funcionalidade | Descrição |
|------------------|-------------|
| R-ativado | [Pacotes de R](https://docs.microsoft.com/machine-learning-server/r-reference/introducing-r-server-r-package-reference) para soluções escritas no R, com uma distribuição de código aberto do R e a infraestrutura de tempo de execução para a execução do script. |
| Ativar o Python | [Módulos do Python](https://docs.microsoft.com/machine-learning-server/python-reference/introducing-python-package-reference) para soluções de escritas no Python, com uma distribuição de código aberto do Python e a infraestrutura de tempo de execução para a execução do script.
| [Modelos de formação previamente](https://docs.microsoft.com/machine-learning-server/install/microsoftml-install-pretrained-models) | Para análise visual e análise de dados de sentimento de texto, pronto para pontuar os dados fornece. |
| [Implementar e consumir](r-server-operationalize.md) | Operacionalizar o seu servidor e implementar soluções como um serviço web. |
| [Execução remota](r-server-hdinsight-manage.md#connect-remotely-to-microsoft-ml-services) | Iniciem sessões remotas no cluster de ML serviços na sua rede da sua estação de trabalho do cliente. |


## <a name="data-storage-options-for-ml-services-on-hdinsight"></a>Opções de armazenamento de dados para os serviços de ML no HDInsight

Armazenamento de predefinido para o sistema de ficheiros do HDFS de clusters do HDInsight pode ser associado a uma conta de armazenamento do Azure ou um Azure Data Lake Store. Esta associação garante que qualquer dados foi carregados para o cluster de armazenamento durante a análise é efetuado persistente e os dados estão disponíveis, mesmo depois do cluster é eliminado. Existem várias ferramentas para processar a transferência de dados para a opção de armazenamento que selecionar, incluindo a instalações de carregamento com base no portal da conta de armazenamento e a [AzCopy](../../storage/common/storage-use-azcopy.md) utilitário.

Tem a opção de ativar o acesso aos BLOBs adicionais e armazena o Data lake durante o processo, independentemente da opção de armazenamento primário em utilização de aprovisionamento de cluster. Consulte [introdução aos serviços de ML no HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-get-started) para obter informações sobre a adição de acesso a contas adicionais. Consulte [opções de armazenamento do Azure ML serviços no HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-r-server-storage) artigo para obter mais informações sobre como utilizar várias contas de armazenamento.

Também pode utilizar [ficheiros do Azure](../../storage/files/storage-how-to-use-files-linux.md) como uma opção de armazenamento para utilização no nó de extremidade. Ficheiros do Azure permite-lhe montar uma partilha de ficheiros que foi criada no armazenamento do Azure para o sistema de ficheiros do Linux. Para obter mais informações sobre estas opções de armazenamento de dados para os serviços de ML num cluster do HDInsight, consulte [opções de armazenamento do Azure ML serviços no HDInsight](r-server-storage.md).

## <a name="access-ml-services-edge-node"></a>Nó de extremidade de ML serviços de acesso

Pode ligar ao Microsoft ML Server no nó de extremidade utilizando um browser. É instalado por predefinição durante a criação do cluster. Para obter mais informações, consulte [obter stared com os serviços de ML no HDInsight](r-server-get-started.md). Também pode ligar ao nó de extremidade de cluster na linha de comandos utilizando SSH/PuTTY para aceder à consola do R.

## <a name="develop-and-run-r-scripts"></a>Desenvolver e executar scripts de R

Os scripts de R criar e executar podem utilizar qualquer um dos 8000 + open source R pacotes além rotinas paralelizadas e distribuídos disponíveis na biblioteca ScaleR. Em geral, um script que é executado com os serviços de ML num nó de extremidade é executado dentro do interpretador de R nesse nó. As exceções são esses passos que tem de chamar uma função de ScaleR com um contexto de computação que está definido para o Hadoop mapa reduzir (RxHadoopMR) ou Spark (RxSpark). Neste caso, a função é executada de forma distribuída entre os nós do cluster de dados (tarefas) que estão associados com os dados referenciados. Para obter mais informações sobre as opções de contexto de computação diferentes, consulte [computação as opções de contexto para os serviços de ML no HDInsight](r-server-compute-contexts.md).

## <a name="operationalize-a-model"></a>Operacionalizar um modelo

Quando a modelação de dados estiver concluída, pode operacionalizar o modelo para fazer predições para novos dados a partir do Azure ou no local. Este processo é conhecido como a classificação. A classificação pode ser feita no HDInsight, do Azure Machine Learning ou no local.

### <a name="score-in-hdinsight"></a>Pontuação no HDInsight

Para Pontuar no HDInsight, escreva uma função de R que chama o seu modelo para fazer predições para um novo ficheiro de dados tiver carregado para a sua conta de armazenamento. Em seguida, guarde serão as predições novamente para a conta de armazenamento. Pode executar este rotina a pedido no nó de extremidade do cluster ou utilizando uma tarefa agendada.

### <a name="score-in-azure-machine-learning-aml"></a>Pontuação no Azure Machine Learning (AML)

Para pontuar utilizando o Azure Machine Learning, utilize o pacote do Azure Machine Learning R de open source conhecido como [AzureML](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) para publicar o seu modelo como um serviço web do Azure. Para sua comodidade, este pacote é pré-instalados no nó de extremidade. Em seguida, utilize as instalações no Azure Machine Learning para criar uma interface de utilizador para o serviço web e, em seguida, chame o serviço web, conforme necessário para classificação.

Se escolher esta opção, tem de converter os objetos de modelo ScaleR objetos equivalentes de open source de modelo para utilização com o serviço web. Utilize as funções de adesão ScaleR, tais como `as.randomForest()` para modelos com base em ensemble, para esta conversão.

### <a name="score-on-premises"></a>Pontuação no local

Para Pontuar no local depois de criar o seu modelo, pode serializar o modelo de R, transferi-lo, anular a serialização e, em seguida, utilizá-lo para a classificação de dados de novo. Pode Pontuar novos dados ao utilizar a abordagem descrita anteriormente no [classificação no HDInsight](#scoring-in-hdinsight) ou utilizando [DeployR](https://deployr.revolutionanalytics.com/).

## <a name="maintain-the-cluster"></a>Manter o cluster

### <a name="install-and-maintain-r-packages"></a>Instalar e manter os pacotes de R

A maioria dos pacotes de R que utilizar é necessárias no nó de extremidade desde a maior parte dos passos para os scripts de R executados não existe. Para instalar pacotes de R adicionais num nó de extremidade, pode utilizar o `install.packages()` método em R.

Se estiver apenas a utilizar rotinas da biblioteca de ScaleR no cluster, não normalmente, tem de instalar os pacotes de R adicionais em nós de dados. No entanto, poderá ser necessário pacotes adicionais para suportar a utilização de **rxExec** ou **RxDataStep** execução em nós de dados.

Nestes casos, os pacotes adicionais podem ser instalados com uma ação de script depois de criar o cluster. Para obter mais informações, consulte [gerir serviços de ML no cluster do HDInsight](r-server-hdinsight-manage.md).

### <a name="change-hadoop-mapreduce-memory-settings"></a>Alterar as definições de memória de MapReduce de Hadoop

Um cluster pode ser modificado para alterar a quantidade de memória que está disponível para os serviços de ML quando está a executar uma tarefa de MapReduce. Para modificar um cluster, utilize a IU do Apache Ambari que está disponível através do painel do portal do Azure para o cluster. Para obter instruções sobre como aceder à IU do Ambari para o cluster, consulte [gerir clusters do HDInsight utilizando a IU da Web do Ambari](../hdinsight-hadoop-manage-ambari.md).

Também é possível alterar a quantidade de memória que está disponível para os serviços de ML utilizando comutadores do Hadoop na chamada para **RxHadoopMR** da seguinte forma:

    hadoopSwitches = "-libjars /etc/hadoop/conf -Dmapred.job.map.memory.mb=6656"  

### <a name="scale-your-cluster"></a>Dimensionar o cluster

Um cluster de ML serviços existente no HDInsight pode ser escalado para cima ou para baixo através do portal. Por como aumentar verticalmente, pode obter a capacidade adicional que poderá ser necessário para tarefas de processamento superiores ou, pode dimensionar novamente um cluster quando estiver inativo. Para obter instruções sobre como dimensionar um cluster, consulte [gerir clusters do HDInsight](../hdinsight-administer-use-portal-linux.md).

### <a name="maintain-the-system"></a>Manter o sistema

Manutenção para aplicar patches de SO e outras atualizações é efetuada nas VMs Linux subjacente de um cluster de HDInsight durante off-hours. Normalmente, manutenção é feita às 3:30 AM (com base na hora local para a VM), cada segunda-feira e a quinta-feira. As atualizações são executadas de forma a que não afetam mais de um trimestre do cluster de cada vez.

Uma vez que os nós principais são redundantes e nem todos os nós de dados são afetados, as tarefas que estão em execução durante este tempo, poderão abrandar. No entanto, ainda deve executar para conclusão. Qualquer personalizadas de software ou dados locais que tiver são preservados entre estes eventos de manutenção ocorra uma falha catastrófica que necessita de uma reconstrução do cluster.

## <a name="ide-options-for-ml-services-on-hdinsight"></a>Opções de IDE para serviços de ML no HDInsight

O nó de extremidade de Linux de um cluster do HDInsight é a zona de destino para a análise baseada em R. Recentes versões do HDInsight fornecem uma instalação padrão do servidor do RStudio no nó de extremidade como um IDE baseada no browser. Utilização do RStudio Server como um IDE para o desenvolvimento e a execução de scripts de R podem ser consideravelmente mais produtivos e permite que apenas utilizando a consola do R.

Além disso, pode instalar um IDE de ambiente de trabalho e utilizá-lo para aceder ao cluster através da utilização de um contexto de computação de MapReduce ou Spark remoto. As opções incluem da Microsoft [R Tools para Visual Studio](https://www.visualstudio.com/features/rtvs-vs.aspx) (RTVS), do RStudio e Walware baseado no Eclipse [StatET](http://www.walware.de/goto/statet).

Além disso, podem aceder a consola de R no nó de extremidade, escrevendo **R** na linha de comandos do Linux depois de se ligar através de SSH ou PuTTY. Quando utilizar a interface da consola, é conveniente executar um editor de texto para o desenvolvimento de script do R na outra janela e cortar e colar secções do seu script na consola do R conforme necessário.

## <a name="pricing"></a>Preços

Os preços que estão associados um cluster do HDInsight de serviços de ML estão estruturados do mesmo modo para que os preços para outros tipos de cluster do HDInsight. Têm como base o dimensionamento de VMs subjacentes entre o nome, dados e nós de limite, com a adição de um uplift horas de núcleo. Para obter mais informações, consulte [preços do HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre como utilizar os serviços de ML nos clusters do HDInsight, consulte os seguintes tópicos:

* [Introdução ao cluster de ML Serices no HDInsight](r-server-get-started.md)
* [Opções de contexto do cluster de ML serviços no HDInsight de computação](r-server-compute-contexts.md)
* [Opções de armazenamento para o cluster de ML serviços no HDInsight](r-server-storage.md)