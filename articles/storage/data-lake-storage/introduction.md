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
ms.openlocfilehash: c86609ae5b993328beced468b74c7f2a1b65def4
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/08/2018
ms.locfileid: "51283619"
---
# <a name="introduction-to-azure-data-lake-storage-gen2-preview"></a>Introdução à pré-visualização do Azure Data Lake Storage geração 2

Pré-visualização da geração 2 do armazenamento do Azure Data Lake é um conjunto de recursos dedicado a análise de macrodados, incorporado ao [armazenamento de Blobs do Azure](../blobs/storage-blobs-introduction.md). Permite-lhe interagir com os dados através de ambos os paradigmas de armazenamento de sistema e o objeto de ficheiro. A adição de geração 2 de armazenamento do Data Lake torna a única plataforma baseada na cloud multimodal, permitindo-lhe extrair o valor de análise de todos os seus dados de armazenamento do Azure.

Geração 2 de armazenamento do Data Lake traz as qualidades que são necessárias para o ciclo de vida completo de dados de análise para o armazenamento do Azure. É o resultado da convergir as capacidades dos nossos serviços armazenamento existente dois, armazenamento de Blobs do Azure e de geração 1 de armazenamento do Azure Data Lake. Funcionalidades do [Gen1 de armazenamento do Azure Data Lake](../../data-lake-store/index.md), como semântica do sistema de ficheiros, ao nível do ficheiro segurança e o dimensionamento são combinados com o armazenamento em camadas e de baixo custo, capacidades de recuperação de alta disponibilidade/desastre de [Azure Armazenamento de BLOBs](../blobs/storage-blobs-introduction.md).

## <a name="designed-for-enterprise-big-data-analytics"></a>Concebido para análise de macrodados de enterprise

Geração 2 de armazenamento do Data Lake torna o armazenamento do Azure, a base para a criação de enterprise lakes de dados no Azure. Projetada desde o início para atender a vários petabytes de informações ao suporte a centenas de gigabits de débito, geração 2 de armazenamento do Data Lake permite-lhe gerir facilmente quantidades enormes de dados.

Uma parte fundamental de geração 2 de armazenamento do Data Lake é a adição de um [espaço de nomes hierárquico](./namespace.md) para o serviço de armazenamento de BLOBs que organiza os ficheiros de objetos de/para uma hierarquia de diretórios para acesso a dados eficiente. O espaço de nomes hierárquico também permite a geração 2 de armazenamento do Data Lake suportar o arquivo de ambos os objetos e paradigmas de sistema de ficheiros ao mesmo tempo. Por exemplo, uma convenção de nomenclatura objeto ao arquivo comuns utiliza barras no nome para imitar a estrutura de pastas hierárquica. Esta estrutura torna-se real com geração 2 de armazenamento do Data Lake. Operações como mudar o nome ou eliminar um diretório tornam-se operações de metadados de atômica única no diretório em vez de enumerar e processamento de todos os objetos que compartilham o prefixo do nome do diretório.

No passado, análise baseada na cloud tinha de comprometer nas áreas de desempenho, gerenciamento e segurança. Geração 2 de armazenamento do Data Lake resolve cada um desses aspectos das seguintes formas:

- **Desempenho** está otimizado porque não é necessário copiar ou transformar os dados como um pré-requisito para análise. O espaço de nomes hierárquico melhora consideravelmente o desempenho de operações de gestão de diretório que melhora o desempenho geral da tarefa.

- **Gestão** é mais fácil, pois pode organizar e manipular arquivos por meio de diretórios e subdiretórios.

- **Segurança** é praticável, pois pode definir permissões de acesso POSIX nos ficheiros de pastas ou indivíduos.

- **Relação custo-eficácia** se tornou possível de geração 2 de armazenamento do Data Lake é criado com base em cima o baixo custo [armazenamento de Blobs do Azure](../blobs/storage-blobs-introduction.md). As funcionalidades adicionais reduzir o custo total de propriedade para executar a análise de macrodados no Azure.

## <a name="key-features-of-data-lake-storage-gen2"></a>Principais recursos de geração 2 de armazenamento do Data Lake

> [!NOTE]
> Durante a pré-visualização pública de geração 2 de armazenamento do Data Lake, alguns dos recursos listados abaixo podem variar na sua disponibilidade. À medida que novos recursos e regiões são lançadas durante o programa de pré-visualização, estas informações serão comunicadas.
> [Inscreva-se](https://aka.ms/adlsgen2signup) para a pré-visualização pública da geração 2 de armazenamento do Data Lake.  

- **Acesso compatível do Hadoop**: geração 2 de armazenamento do Data Lake permite-lhe gerir e aceder aos dados, tal como faria com um [Hadoop Distributed File System (HDFS)](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html). A nova [ABFS driver](./abfs-driver.md) está disponível em todos os ambientes do Apache Hadoop, incluindo [Azure HDInsight](../../hdinsight/index.yml) e [Azure Databricks](../../azure-databricks/index.yml) para acessar dados armazenados no armazenamento do Data Lake Geração 2.

- **Um conjunto mais amplo de permissões de POSIX**: O modelo de segurança para a geração 2 do Data Lake suporta permissões de ACL e POSIX, juntamente com alguns extra granularidade específica de geração 2 de armazenamento do Data Lake. Definições podem ser configuradas por meio de ferramentas de administração ou estruturas como o Hive e do Spark.

- **Custo-eficiência**: geração 2 de armazenamento do Data Lake oferece a capacidade de armazenamento de baixo custo e transações. Como transições de dados por meio de seu ciclo de vida completo, taxas de faturação alterar os custos de manutenção ao mínimo possível por meio de recursos incorporados, tal como [ciclo de vida de armazenamento de Blobs do Azure](../common/storage-lifecycle-managment-concepts.md).

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