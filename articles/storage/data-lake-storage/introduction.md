---
title: Introdução de pré-visualização do Azure Data Lake armazenamento Gen2
description: Fornece uma descrição geral de pré-visualização do Azure Data Lake armazenamento Gen2
services: storage
documentationcenter: ''
author: jamesbak
manager: jahogg
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: jamesbak
ms.component: data-lake-storage-gen2
ms.openlocfilehash: 0631b1d0c8da925858f0b7fb1d778cb1161eb737
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37061529"
---
# <a name="introduction-to-azure-data-lake-storage-gen2-preview"></a>Introdução à pré-visualização do Azure Data Lake armazenamento Gen2

Azure Data Lake armazenamento Gen2 Preview é um conjunto de capacidades dedicado a análise de macrodados, incorporada de [Blob storage do Azure](../blobs/storage-blobs-introduction.md). Permite-lhe interface com os seus dados através de ambos os paradigmas de armazenamento de sistema e o objeto de ficheiro. Isto torna Gen2 de armazenamento do Data Lake o serviço de apenas armazenamento em múltiplos modal baseado na nuvem, permitindo-lhe extraia o valor de análise de todos os dados.

Gen2 de armazenamento do Data Lake funcionalidades qualities todos os que são necessárias para o ciclo de vida completo de dados de análise. Isto resulta da converging as capacidades dos nossos dois serviços de armazenamento existente. Funcionalidades de [Gen1 de armazenamento do Azure Data Lake](../../data-lake-store/index.md), tais como a semântica de sistema de ficheiros, ficheiro de nível de segurança e escala são combinados com armazenamento de baixo custo, em camadas, as capacidades de recuperação de desastre/disponibilidade elevada e SDK/ferramentas grande ecossistema de [Blob storage do Azure](../blobs/storage-blobs-introduction.md). Gen2 de armazenamento do Data Lake, todos os qualities de armazenamento de objeto permanecem ao adicionar as vantagens de uma interface de sistema de ficheiros otimizados para análise de cargas de trabalho.

## <a name="designed-for-enterprise-big-data-analytics"></a>Concebido para análise de macrodados enterprise

Gen2 de armazenamento do Data Lake é o serviço de armazenamento para a criação de enterprise lagos de dados (EDL) no Azure. Concebido desde o início ao serviço vários petabytes de informações ao sustaining centenas de gigabits de débito, Gen2 de armazenamento do Data Lake dá-lhe uma forma fácil de gerir quantidades enormes de dados.

Uma funcionalidade fundamental do Data Lake armazenamento Gen2 consiste na adição de um [espaço de nomes hierárquico](./namespace.md) para o serviço de armazenamento de BLOBs que organiza os objetos/ficheiros para uma hierarquia de diretórios para acesso a dados performant. O espaço de nomes hierárquico também permite Gen2 de armazenamento do Data Lake suportar o arquivo de objeto e paradigmas de sistema de ficheiros ao mesmo tempo. Por exemplo, uma convenção de nomenclatura objeto ao arquivo comuns utiliza barras no nome para imitar a estrutura de pasta hierárquica. Esta estrutura fica real com Gen2 de armazenamento do Data Lake. Operações como mudar o nome ou eliminar um diretório tornar-se operações de metadados de atómico único no diretório em vez de enumerar e processar todos os objetos que partilham o prefixo do nome do diretório.

No passado, a análise baseado na nuvem que tiveram de comprometer nas áreas de desempenho, gestão e segurança. Gen2 de armazenamento do Data Lake endereços cada estes aspetos das seguintes formas:

- **Desempenho** está otimizado porque não tem de copiar ou transformar dados como um pré-requisito para análise. O espaço de nomes hierárquico melhora consideravelmente o desempenho das operações de gestão do diretório que melhora o desempenho global de tarefa.

- **Gestão** é mais fácil, porque pode organizar e manipular ficheiros através de diretórios e subdiretórios.

- **Eficácia de custos** é disponibilizada como Gen2 de armazenamento do Data Lake é desenvolvida com o baixo custo [Blob storage do Azure](../blobs/storage-blobs-introduction.md). As funcionalidades adicionais inferior adicionais o custo total de propriedade para executar a análise de macrodados no Azure.

## <a name="key-features-of-data-lake-storage-gen2"></a>Principais funcionalidades de Gen2 de armazenamento do Data Lake

> [!NOTE]
> Durante a pré-visualização pública do Data Lake armazenamento Gen2, algumas das funcionalidades listadas abaixo podem variar na respetiva disponibilidade. Como novas funcionalidades e regiões são lançadas durante o programa de pré-visualização, estas informações serão comunicadas.
> [Inscrever-se](https://aka.ms/adlsgen2signup) para a pré-visualização pública do Data Lake armazenamento Gen2.  

- **Acesso compatível Hadoop**: Gen2 de armazenamento do Data Lake permite-lhe gerir e aceder aos dados, tal como faria com um [distribuídas ficheiro sistema Hadoop (HDFS)](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html). A nova [controlador ABFS](./abfs-driver.md) está disponível a todos os ambientes do Apache Hadoop, incluindo [Azure HDInsight](../../hdinsight/index.yml) e [Azure Databricks](../../azure-databricks/index.yml) para aceder a dados armazenados no armazenamento do Data Lake Gen2.

- **Acesso a dados protocolo multi e múltiplos modal**: Gen2 de armazenamento do Data Lake é considerado um **modal Multi** serviço de armazenamento dado que fornece armazenamento de objeto e o ficheiro de interfaces de sistema para os mesmos dados **ao mesmo tempo tempo**. Isto é conseguido fornecendo vários pontos finais de protocolo que são capazes de aceder aos mesmos dados. 

    Ao contrário de outras soluções de análise, os dados armazenados na Gen2 de armazenamento do Data Lake não precisam de mover ou ser transformados antes de poder executar uma variedade de ferramentas de análise. Pode aceder aos dados através de tradicional [APIs de armazenamento de BLOBs](../blobs/storage-blobs-introduction.md) (por exemplo: ingestão de dados através de [capturar os Hubs de eventos](../../event-hubs/event-hubs-capture-enable-through-portal.md)) e processar dados com o HDInsight ou Databricks do Azure ao mesmo tempo. 

- **Económico**: Gen2 de armazenamento do Data Lake inclui a capacidade de armazenamento de baixo custo e transações. Como transições de dados através do respetivo ciclo de vida completo, taxas de faturação alterar os custos de manutenção para um mínimo através de funcionalidades incorporadas, tais como [ciclo de vida de armazenamento de Blobs do Azure](../common/storage-lifecycle-managment-concepts.md).

- **Funciona com aplicações, estruturas e ferramentas de armazenamento de BLOBs**: Gen2 de armazenamento do Data Lake continua a trabalhar com um grande número de ferramentas, arquiteturas e aplicações que atualmente existem para o Blob storage.

- **Controlador otimizada**: O `abfs` controlador é [especificamente otimizado](./abfs-driver.md) para análise de macrodados. As APIs REST correspondente estão anexadas através de `dfs` ponto final, `dfs.core.windows.net`.

## <a name="scalability"></a>Escalabilidade

Storage do Azure é dimensionável por predefinição, se aceder através de interfaces de armazenamento Gen2 de armazenamento do Data Lake ou Blob. Para armazenar e servir *muitos exabytes de dados*. Esta quantidade de armazenamento está disponível com o débito, medido em gigabits por segundo (Gbps) em níveis elevados de operações de entrada/saída por segundo (IOPS). Além apenas persistência, o processamento é executado em latências quase constante por pedidos que são avaliadas no serviço, conta e níveis de ficheiro.

## <a name="cost-effectiveness"></a>Relação custo-eficácia

Um dos muitos benefícios da criação do Data Lake armazenamento Gen2 por cima do Blob storage do Azure é a [baixo custo](https://azure.microsoft.com/pricing/details/storage) de capacidade de armazenamento e transações. Ao contrário de outros serviços de armazenamento em nuvem, Gen2 de armazenamento do Data Lake reduz os custos porque não são necessários para ser movida ou transformados antes de efetuar análise de dados.

Além disso, as funcionalidades, tais como o [espaço de nomes hierárquico](./namespace.md) significativamente melhorar o desempenho global de várias tarefas de análise. Este melhoramento do desempenho significa que precisa de menos potência de computação para processar a mesma quantidade de dados, resultando num custo mais baixo total de propriedade (TCO) para a tarefa de análise de ponto a ponto.

## <a name="next-steps"></a>Passos Seguintes

Os artigos seguintes descrevem alguns dos conceitos principais de Gen2 de armazenamento do Data Lake e detalhes como armazenar, aceder, gerir e obter conhecimentos aprofundados sobre os dados:

* [Espaço de nomes hierárquico](./namespace.md)
* [Criar uma conta de armazenamento](./quickstart-create-account.md)
* [Criar um cluster do HDInsight com Gen2 de armazenamento do Azure Data Lake](./quickstart-create-connect-hdi-cluster.md)
* [Utilizar uma conta do Azure Data Lake armazenamento Gen2 no Azure Databricks](./quickstart-create-databricks-account.md) 