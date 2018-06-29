---
title: O controlador de rastreio de Blob do Azure para a pré-visualização do Azure Data Lake armazenamento Gen2
description: O controlador de sistema de ficheiros do ABFS Hadoop
services: storage
keywords: ''
author: jamesbak
manager: jahogg
ms.topic: article
ms.author: jamesbak
ms.date: 06/27/2018
ms.service: storage
ms.component: data-lake-storage-gen2
ms.openlocfilehash: a726779e731be2534e457ba595d93fe51c023601
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036281"
---
# <a name="the-azure-blob-filesystem-driver-abfs-a-dedicated-azure-storage-driver-for-hadoop"></a>O controlador de sistema de ficheiros do Azure Blob (ABFS): um controlador de armazenamento do Azure dedicado para o Hadoop

Um dos métodos de acesso primária para os dados na pré-visualização do Azure Data Lake armazenamento Gen2 é através de [Hadoop FileSystem](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/filesystem/index.html). Funcionalidades de Gen2 do armazenamento do Azure Lake de dados de um controlador associado, o controlador de sistema de ficheiros de Blob do Azure ou `ABFS`. ABFS faz parte do Apache Hadoop e está incluído em muitas as distribuições comerciais do Hadoop. Utilizar este controlador, muitas arquiteturas e aplicações podem aceder aos dados no Data Lake armazenamento Gen2 sem qualquer código explicitamente a referenciar o serviço de Gen2 de armazenamento do Data Lake.

## <a name="prior-capability-the-windows-azure-storage-blob-driver"></a>Capacidade anterior: controlador o Blob de armazenamento do Azure Windows

O controlador de Blob de armazenamento do Windows Azure ou [controlador WASB](https://hadoop.apache.org/docs/current/hadoop-azure/index.html) fornecido o suporte original para o Blobs Storage do Azure. Este controlador efetuada a complexa tarefa de sistema de ficheiros de mapeamento semântica (conforme necessário, a interface de sistema de ficheiros Hadoop) para que o objeto de arquivo de interface de estilo exposta ao Blob Storage do Azure. Este controlador continua a suportar a este modelo, fornecer acesso de elevado desempenho aos dados armazenados em Blobs, mas contém uma quantidade significativa de código desta mapeamento, tornando difícil manter a efetuar. Além disso, algumas operações, tais como [FileSystem.rename()](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/filesystem/filesystem.html#boolean_renamePath_src_Path_d) e [FileSystem.delete()](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/filesystem/filesystem.html#boolean_deletePath_p_boolean_recursive) quando aplicada aos diretórios requerem o controlador para efetuar um grande número de operações (devido a falta de arquivos de objeto suporte para diretórios) que frequentemente leva a degradação do desempenho.

Assim, a ultrapassar o objetivo de design inerente de WASB, o novo serviço de armazenamento do Azure Data Lake foi implementado com o suporte do controlador da ABFS novo.

## <a name="the-azure-blob-file-system-driver"></a>O controlador de sistema de ficheiros de Blob do Azure

O [interface REST de armazenamento do Azure Data Lake](https://docs.microsoft.com/en-us/rest/api/datalakestorage/) foi concebido para suportar a semântica de sistema de ficheiros ao longo do Blob Storage do Azure. Uma vez que o sistema de ficheiros Hadoop também foi concebido para suportar a mesma semântica não é necessário para um mapeamento de complexo, o controlador. Assim, o controlador de sistema de ficheiros de Blob do Azure (ou ABFS) é um shim de cliente mere para a API REST.

No entanto, existem algumas funções de que o controlador tem ainda de efetuar:

### <a name="uri-scheme-to-reference-data"></a>Esquema URI para dados de referência

Consistente com outras implementações de sistema de ficheiros dentro do Hadoop, o controlador ABFS define o seu próprio esquema de URI para que os recursos (ficheiros e diretórios) podem ser resolvidos de forma distinta. O esquema URI é descrito da [utilize o URI de Gen2 de armazenamento do Azure Data Lake](./introduction-abfs-uri.md). A estrutura do URI é: `abfs[s]://file_system@account_name.dfs.core.windows.net/<path>/<path>/<file_name>`

Utilizar o formato URI acima, standard ferramentas Hadoop e estruturas podem ser utilizadas para fazer referência a estes recursos:

```bash
hdfs dfs -mkdir -p abfs://fileanalysis@myanalytics.dfs.core.windows.net/tutorials/flightdelays/data 
hdfs dfs -put flight_delays.csv abfs://fileanalysis@myanalytics.dfs.core.windows.net/tutorials/flightdelays/data/ 
```

Internamente, o controlador ABFS traduz os recursos de especificado no URI de ficheiros e diretórios e faz com que chamadas para a API de REST de armazenamento do Azure Data Lake com essas referências.

### <a name="authentication"></a>Autenticação

O controlador ABFS atualmente suporta a autenticação de chave partilhada para que a aplicação de Hadoop com segurança pode aceder aos recursos contidos Gen2 de armazenamento do Data Lake. A chave é encriptada e armazenada na configuração do Hadoop.

### <a name="configuration"></a>Configuração

Toda a configuração para o controlador ABFS é armazenado no <code>core-site.xml</code> ficheiro de configuração. Sobre as distribuições de Hadoop com [Ambari](http://ambari.apache.org/), a configuração também pode ser gerida utilizando o web portal ou a API de REST do Ambari.

Detalhes de todas as entradas de configuração suportados estão especificados no [documentação do Hadoop oficial](http://hadoop.apache.org/docs/current/hadoop-azure/index.html).

### <a name="hadoop-documentation"></a>Documentação do Hadoop

O controlador ABFS é documentado na íntegra a [documentação do Hadoop oficial](http://hadoop.apache.org/docs/current/hadoop-azure/index.html)

## <a name="next-steps"></a>Passos Seguintes

- [Configurar Clusters do HDInsight](./quickstart-create-connect-hdi-cluster.md)
- [Criar um Cluster de Databricks do Azure](./quickstart-create-databricks-account.md)
- [Utilize o Gen2 Lake armazenamento de dados do Azure URI](./introduction-abfs-uri.md)