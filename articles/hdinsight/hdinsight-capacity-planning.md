---
title: Planeamento de capacidade no Azure HDInsight do cluster
description: Como especificar um cluster do HDInsight para a capacidade e desempenho.
services: hdinsight
author: maxluk
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 09/22/2017
ms.author: maxluk
ms.openlocfilehash: e3c5ba4a3b67bcd51b90b5395f4d373be57bfff0
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/07/2018
ms.locfileid: "39592701"
---
# <a name="capacity-planning-for-hdinsight-clusters"></a>Planeamento da capacidade para clusters do HDInsight

Antes de implementar um cluster do HDInsight, planeie a capacidade de agrupamento desejada determinando o desempenho necessário e a escala. Este planeamento ajuda otimizar a facilidade de utilização e custos. Algumas decisões de capacidade do cluster não podem ser alterados após a implementação. Se alterar os parâmetros de desempenho, um cluster pode ser dismantled e voltar a criar sem perder dados armazenados.

As perguntas importantes para perguntar para planejamento de capacidade são:

* Em cada região geográfica deve implementar o cluster?
* A quantidade de armazenamento precisa?
* Que tipo de cluster que deve implementar?
* Que tamanho e tipo de máquina virtual (VM) devem usar o os nós do cluster?
* Quantos nós de trabalho do cluster deve ter?

## <a name="choose-an-azure-region"></a>Escolha uma região do Azure

A região do Azure determina em que o seu cluster fisicamente está aprovisionado. Para minimizar a latência de leituras e escritas, o cluster deve estar perto seus dados.

HDInsight está disponível em muitas regiões do Azure. Para a região mais próxima, consulte a *Linux de HDInsight* entrada sob *dados + análise* na [Azure produtos disponíveis por região](https://azure.microsoft.com/regions/services/).

## <a name="choose-storage-location-and-size"></a>Escolha a localização de armazenamento e tamanho

### <a name="location-of-default-storage"></a>Localização de armazenamento predefinido

O armazenamento predefinido, uma conta de armazenamento do Azure ou o Azure Data Lake Store, tem de ser na mesma localização do seu cluster. O armazenamento do Azure está disponível em todos os locais. Data Lake Store está disponível em algumas regiões - ver a disponibilidade atual do Data Lake Store sob *armazenamento* na [Azure produtos disponíveis por região](https://azure.microsoft.com/regions/services/).

### <a name="location-of-existing-data"></a>Localização dos dados existentes

Se já tiver uma conta de armazenamento ou o Data Lake Store que contém os dados e pretende utilizar este armazenamento como armazenamento predefinido do seu cluster, tem de implementar o cluster nessa mesma localização.

### <a name="storage-size"></a>Tamanho de armazenamento

Depois de ter implementado um cluster do HDInsight, pode anexar a contas de armazenamento do Azure adicionais ou outros dados de data Lake Stores de acesso. Todas as suas contas de armazenamento têm de residir na mesma localização do seu cluster. Um Store de Lake de dados pode ser numa localização diferente, embora isso pode introduzir alguma latência de leitura/escrita de dados.

O armazenamento do Azure tem algumas [limites de capacidade](../azure-subscription-service-limits.md#storage-limits), enquanto o Data Lake Store é praticamente ilimitado.

Um cluster pode aceder a uma combinação de contas de armazenamento diferentes. Os exemplos típicos incluem:

* Quando a quantidade de dados é provável que excederem a capacidade de armazenamento de um contentor de armazenamento de blob único.
* Quando a taxa de acesso para o contentor de BLOBs pode exceder o limiar em que ocorre a limitação.
* Quando deseja tornar os dados, já tiver carregado para um contentor de BLOBs disponível para o cluster.
* Quando quiser isolar diferentes partes de armazenamento por motivos de segurança, ou para simplificar a administração.

Para um cluster de nó de 48 recomendamos as contas de armazenamento de 4 a 8. Embora já deve haver totais de armazenamento suficiente, cada conta de armazenamento fornece a largura de banda de rede adicional para os nós de computação. Quando tiver várias contas de armazenamento, utilize um nome aleatório para cada conta de armazenamento, sem um prefixo. O objetivo de atribuição de nomes aleatórios é reduzir a possibilidade de afunilamentos de armazenamento (limitação) ou falhas de modo comum em todas as contas. Para um melhor desempenho, utilize apenas um contêiner por conta de armazenamento.

## <a name="choose-a-cluster-type"></a>Escolha um tipo de cluster

O tipo de cluster determina a carga de trabalho que do cluster do HDInsight está configurado para executar, como Hadoop, Storm, Kafka ou Spark. Para obter uma descrição detalhada dos tipos de cluster disponíveis, consulte [introdução ao Azure HDInsight](hadoop/apache-hadoop-introduction.md#cluster-types-in-hdinsight). Cada tipo de cluster tem uma topologia de implementação específica que inclui os requisitos para o tamanho e número de nós.

## <a name="choose-the-vm-size-and-type"></a>Escolha o tipo e tamanho VM

Cada tipo de cluster tem um conjunto de tipos de nó e cada tipo de nó tem opções específicas para seu tipo e tamanho VM.

Para determinar o tamanho do cluster ideal para a sua aplicação, pode efetuar testes de desempenho a capacidade do cluster e aumentar o tamanho conforme indicado. Por exemplo, pode utilizar uma carga de trabalho simulada, ou uma *consulta canary*. Com uma carga de trabalho simulada, executar cargas de trabalho esperadas em clusters de tamanho diferente, aumentar gradualmente o tamanho até que seja atingido o desempenho pretendido. Uma consulta canary pode ser inserida, periodicamente, entre outras consultas de produção para mostrar se ou não o cluster tem recursos suficientes.

O tamanho da VM e o tipo é determinado por CPU processamento, o tamanho de RAM e latência de rede:

* CPU: O tamanho da VM determina o número de núcleos. O maior número de núcleos, quanto maior o grau de computação paralela pode alcançar a cada nó. Além disso, alguns tipos VM tem núcleos mais rápidos.

* RAM: O tamanho da VM também determina a quantidade de RAM disponível na VM. Para cargas de trabalho que armazenam dados na memória para processamento, em vez de leitura do disco, certifique-se os nós de trabalho tem memória suficiente para colocar os dados.

* Rede: Para a maioria dos tipos de cluster, os dados processados pelo cluster são não num disco local, mas num serviço de armazenamento externo, como o Data Lake Store ou o armazenamento do Azure. Considere a largura de banda de rede e o débito entre o nó de VM e o serviço de armazenamento. Normalmente, aumenta a largura de banda de rede disponível para uma VM com tamanhos maiores. Para obter detalhes, consulte [descrição geral de tamanhos de VM](https://docs.microsoft.com/azure/virtual-machines/linux/sizes).

## <a name="choose-the-cluster-scale"></a>Selecionar o dimensionamento do cluster

Dimensionamento de um cluster é determinado pela quantidade de seus nós VM. Para todos os tipos de cluster, existem tipos de nós que tenham um dimensionamento específico e tipos de nó que suportam o Escalamento horizontal. Por exemplo, um cluster pode exigir exatamente três nós ZooKeeper ou dois nós principais. Nós de trabalho que efetuar o processamento de dados de uma maneira distribuída podem beneficiar de aumentar horizontalmente, ao adicionar nós de trabalho adicionais.

Consoante o tipo de cluster, o aumento do número de nós de trabalho adiciona capacidades de computacionais adicionais (por exemplo, mais núcleos), mas também pode adicionar a quantidade total de memória necessária para todo o cluster oferecer suporte a dentro da memória armazenamento de dados a ser processados. Tal como acontece com a escolha do tipo e tamanho VM, selecionar a escala de adequado do cluster é normalmente contactado empiricamente, através de cargas de trabalho simuladas ou consultas canary.

Pode aumentar horizontalmente o seu cluster para os picos de procura de carga, em seguida, dimensioná-lo a voltar a reduzir esses nós adicionais já não são necessários. Para obter mais informações, consulte [clusters do HDInsight de escala](hdinsight-scaling-best-practices.md).

### <a name="cluster-lifecycle"></a>Ciclo de vida do cluster

É-lhe cobrada a duração de um cluster. Se houver apenas horas específicas que terá do cluster de cópia de segurança e em execução, pode [criar clusters a pedido com o Azure Data Factory](hdinsight-hadoop-create-linux-clusters-adf.md). Pode também criar scripts do PowerShell que aprovisionam e elimina o cluster e, em seguida, agendar esses scripts usando [automatização do Azure](https://azure.microsoft.com/services/automation/).

> [!NOTE]
> Quando um cluster é eliminado, o metastore de Hive padrão também é eliminado. Para manter o metastore para a próximo recriação de cluster, utilize um arquivo externo de metadados, tais como a base de dados do Azure ou o Oozie.
<!-- see [Using external metadata stores](hdinsight-using-external-metadata-stores.md). -->

### <a name="isolate-cluster-job-errors"></a>Isolar os erros de tarefas de cluster

Erros, às vezes, podem ocorrer devido à execução paralela de mapa vários e reduzir componentes num cluster de vários nós. Para ajudar a isolar o problema, tente testes distribuídos sendo executada em simultâneo vários trabalhos num cluster de nó único, em seguida, expanda essa abordagem para executar várias tarefas simultaneamente em clusters que contêm mais de um nó. Para criar um cluster de HDInsight nó único no Azure, utilize o *avançadas* opção.

Também pode instalar um ambiente de desenvolvimento de nó único no seu computador local e testar a solução aqui. Hortonworks fornece um ambiente de desenvolvimento local de nó único para soluções baseadas no Hadoop que é útil para o desenvolvimento inicial, prova de conceito e teste. Para obter mais informações, consulte [Sandbox da Hortonworks](http://hortonworks.com/products/hortonworks-sandbox/).

Identificar o problema num cluster local de nó único pode voltar a executar tarefas com falhas e ajustar os dados de entrada ou utilizar conjuntos de dados mais pequenos. Como executar essas tarefas depende da plataforma e o tipo de aplicação.

## <a name="quotas"></a>Quotas

Depois de determinar o tamanho VM do cluster de destino, o dimensionamento e o tipo, verifique os limites de capacidade de quota atual da sua subscrição. Quando atingir um limite de cota, poderá não conseguir implementar novos clusters ou aumente horizontalmente clusters existentes ao adicionar mais nós de trabalho. O limite da quota atingido mais comuns é a quota de núcleos de CPU que existe a subscrição, região e níveis de série VM. Por exemplo, a sua subscrição pode ter um limite total de núcleos de 200, com um limite de 30 núcleos na sua região e um limite de 30 núcleos em instâncias VM. Pode [contacte o suporte para pedir um aumento de quota](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request).

No entanto, existem alguns limites de quota fixo, por exemplo uma única subscrição do Azure pode ter no máximo, 10.000 núcleos. Para obter detalhes sobre estes limites, consulte [subscrição do Azure e limites do serviço, quotas e restrições](https://docs.microsoft.com/azure/azure-subscription-service-limits#limits-and-the-azure-resource-manager).

## <a name="next-steps"></a>Passos Seguintes

* [Configurar clusters no HDInsight com o Hadoop, Spark, Kafka e muito mais](hdinsight-hadoop-provision-linux-clusters.md): Saiba como definir e configurar clusters no HDInsight com o Hadoop, Spark, Kafka, Interactive Hive, HBase, serviços de ML ou Storm.
* [Monitorizar o desempenho do cluster](hdinsight-key-scenarios-to-monitor.md): Saiba mais sobre os principais cenários para monitorar para o seu cluster do HDInsight que pode afetar a capacidade do seu cluster.
