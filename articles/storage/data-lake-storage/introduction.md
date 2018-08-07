---
title: Introdução de pré-visualização do Azure Data Lake Storage geração 2
description: Fornece uma descrição geral de pré-visualização do Azure Data Lake Storage geração 2
services: storage
author: jamesbak
ms.service: storage
ms.topic: article
ms.date: 06/27/2018
ms.author: jamesbak
ms.component: data-lake-storage-gen2
ms.openlocfilehash: 51f38cf7ade01b58ad5ce7925af5546d1a4f1a0c
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/06/2018
ms.locfileid: "39525387"
---
# <a name="introduction-to-azure-data-lake-storage-gen2-preview"></a>Introdução à pré-visualização do Azure Data Lake Storage geração 2

Pré-visualização da geração 2 do armazenamento do Azure Data Lake é um conjunto de recursos dedicado a análise de macrodados, criado sobre [armazenamento de Blobs do Azure](../blobs/storage-blobs-introduction.md). Permite-lhe interagir com os dados através de ambos os paradigmas de armazenamento de sistema e o objeto de ficheiro. Isso torna a geração 2 de armazenamento do Data Lake o serviço de apenas armazenamento em multimodal baseado na nuvem, permitindo-lhe extrair o valor de análise de todos os seus dados.

Geração 2 de armazenamento do Data Lake inclui todas as qualidades que são necessárias para o ciclo de vida completo de dados de análise. Isso resulta de convergir as capacidades dos nossos dois serviços de armazenamento existente. Funcionalidades do [Gen1 de armazenamento do Azure Data Lake](../../data-lake-store/index.md), como semântica do sistema de ficheiros, ao nível do ficheiro segurança e o dimensionamento são combinados com armazenamento em camadas e de baixo custo, capacidades de recuperação após desastre/disponibilidade elevada e uma grande/ferramentas do SDK o ecossistema de desde [armazenamento de Blobs do Azure](../blobs/storage-blobs-introduction.md). No ger2 de armazenamento do Data Lake, todas as qualidades de armazenamento de objetos permanecem enquanto adicionar as vantagens de uma interface de sistema de ficheiros otimizados para análise de cargas de trabalho.

## <a name="designed-for-enterprise-big-data-analytics"></a>Concebido para análise de macrodados de enterprise

Geração 2 de armazenamento do Data Lake é o serviço de armazenamento fundamentais para a criação de enterprise lakes de dados (EDL) no Azure. Projetada desde o início para atender a vários petabytes de informações ao suporte a centenas de gigabits de débito, geração 2 de armazenamento do Data Lake dá-lhe uma forma fácil de gerenciar grandes quantidades de dados.

Um recurso fundamental de geração 2 de armazenamento do Data Lake é a adição de um [espaço de nomes hierárquico](./namespace.md) para o serviço de armazenamento de BLOBs que organiza os ficheiros de objetos de/para uma hierarquia de diretórios para acesso a dados de elevado desempenho. O espaço de nomes hierárquico também permite a geração 2 de armazenamento do Data Lake suportar o arquivo de ambos os objetos e paradigmas de sistema de ficheiros ao mesmo tempo. Por exemplo, uma convenção de nomenclatura objeto ao arquivo comuns utiliza barras no nome para imitar a estrutura de pastas hierárquica. Esta estrutura torna-se real com geração 2 de armazenamento do Data Lake. Operações como mudar o nome ou eliminar um diretório tornam-se operações de metadados de atômica única no diretório em vez de enumerar e processamento de todos os objetos que compartilham o prefixo do nome do diretório.

No passado, análise baseada na cloud tinha de comprometer nas áreas de desempenho, gerenciamento e segurança. Geração 2 de armazenamento do Data Lake resolve cada um desses aspectos das seguintes formas:

- **Desempenho** está otimizado porque não é necessário copiar ou transformar os dados como um pré-requisito para análise. O espaço de nomes hierárquico melhora consideravelmente o desempenho de operações de gestão de diretório que melhora o desempenho geral da tarefa.

- **Gestão** é mais fácil, pois pode organizar e manipular arquivos por meio de diretórios e subdiretórios.

- **Relação custo-eficácia** se tornou possível de geração 2 de armazenamento do Data Lake é criado com base em cima o baixo custo [armazenamento de Blobs do Azure](../blobs/storage-blobs-introduction.md). As funcionalidades adicionais reduzir o custo total de propriedade para executar a análise de macrodados no Azure.

## <a name="key-features-of-data-lake-storage-gen2"></a>Principais recursos de geração 2 de armazenamento do Data Lake

> [!NOTE]
> Durante a pré-visualização pública de geração 2 de armazenamento do Data Lake, alguns dos recursos listados abaixo podem variar na sua disponibilidade. À medida que novos recursos e regiões são lançadas durante o programa de pré-visualização, estas informações serão comunicadas.
> [Inscreva-se](https://aka.ms/adlsgen2signup) para a pré-visualização pública da geração 2 de armazenamento do Data Lake.  

- **Acesso compatível do Hadoop**: geração 2 de armazenamento do Data Lake permite-lhe gerir e aceder aos dados, tal como faria com um [Hadoop Distributed File System (HDFS)](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html). A nova [ABFS driver](./abfs-driver.md) está disponível em todos os ambientes do Apache Hadoop, incluindo [Azure HDInsight](../../hdinsight/index.yml) e [Azure Databricks](../../azure-databricks/index.yml) para acessar dados armazenados no armazenamento do Data Lake Geração 2.

- **Acesso a dados de protocolos múltiplos e multimodal**: geração 2 de armazenamento do Data Lake é considerado um **multi modal** serviço de armazenamento à medida que fornece tanto o arquivo de objetos como o arquivo de interfaces do sistema para os mesmos dados **ao mesmo tempo tempo**. Isto é conseguido fornecendo vários pontos de extremidade de protocolo que poderá aceder aos mesmos dados. 

    Ao contrário de outras soluções de análise, os dados armazenados na geração 2 de armazenamento do Data Lake não tem de mover ou transformado antes de poder executar uma variedade de ferramentas de análise. Pode aceder a dados por meio de tradicional [APIs de armazenamento de BLOBs](../blobs/storage-blobs-introduction.md) (por exemplo: ingerir dados via [captura de Hubs de eventos](../../event-hubs/event-hubs-capture-enable-through-portal.md)) e processar dados com o HDInsight ou o Azure Databricks, ao mesmo tempo. 

- **Custo-eficiência**: geração 2 de armazenamento do Data Lake inclui a capacidade de armazenamento de baixo custo e transações. Como transições de dados por meio de seu ciclo de vida completo, taxas de faturação alterar os custos de manutenção ao mínimo possível por meio de recursos incorporados, tal como [ciclo de vida de armazenamento de Blobs do Azure](../common/storage-lifecycle-managment-concepts.md).

- **Funciona com aplicações, estruturas e ferramentas de armazenamento de BLOBs**: geração 2 de armazenamento do Data Lake continua a trabalhar com um vasto leque de ferramentas, estruturas e aplicativos que existem hoje mesmo para o armazenamento de Blobs.

- **Controlador otimizada**: A `abfs` driver é [otimizado especificamente](./abfs-driver.md) para análise de macrodados. As APIs REST correspondente são apresentadas os `dfs` ponto de extremidade, `dfs.core.windows.net`.

## <a name="scalability"></a>Escalabilidade

Armazenamento do Azure é dimensionável por design, se acessar por meio de interfaces de armazenamento de BLOBs ou de geração 2 de armazenamento do Data Lake. Ele é capaz de armazenar e servir *muitos exabytes de dados*. Esta quantidade de armazenamento está disponível com débito medido em gigabits por segundo (Gbps) num altos níveis de operações de entrada/saída por segundo (IOPS). Além apenas persistência, o processamento é executado em latências de perto constante por solicitação, que são medidas ao nível de ficheiro, da conta e serviço.

## <a name="cost-effectiveness"></a>Relação custo-eficácia

Um dos muitos benefícios da criação de geração 2 de armazenamento do Data Lake com base no armazenamento de Blobs do Azure é o [baixo custo](https://azure.microsoft.com/pricing/details/storage) de capacidade de armazenamento e transações. Ao contrário de outros serviços de armazenamento na cloud, a geração 2 de armazenamento do Data Lake reduz os custos porque não são necessários para ser movida ou transformadas antes de efetuar a análise de dados.

Além disso, funcionalidades como a [espaço de nomes hierárquico](./namespace.md) melhorar significativamente o desempenho geral de muitas tarefas de análise. Esta melhoria no desempenho significa que exigem menos poder de computação para processar a mesma quantidade de dados, que resulta num menor custo total de propriedade (TCO) para a tarefa de análise de ponto-a-ponto.

## <a name="next-steps"></a>Passos Seguintes

Os seguintes artigos descrevem alguns dos principais conceitos de geração 2 de armazenamento do Data Lake e detalhes como armazenar, acessar, gerir e obter informações a partir dos seus dados:

* [Espaço de nomes hierárquico](./namespace.md)
* [Criar uma conta de armazenamento](./quickstart-create-account.md)
* [Criar um cluster do HDInsight com geração 2 de armazenamento do Azure Data Lake](./quickstart-create-connect-hdi-cluster.md)
* [Utilizar uma conta de geração 2 de armazenamento do Azure Data Lake no Azure Databricks](./quickstart-create-databricks-account.md) 