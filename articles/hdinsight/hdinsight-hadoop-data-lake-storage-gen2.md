---
title: Utilizar o Azure Data Lake Storage (ADLS) Gen2 com o Apache Hadoop no HDInsight do Azure
description: Fornece uma descrição geral de geração 2 de armazenamento do Azure Data Lake e como ele funciona com o Azure HDInsight.
services: hdinsight,storage
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 11/21/2018
ms.openlocfilehash: b7cde3a8990a51e95a8ce9ad85bca524d5669e0c
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/21/2018
ms.locfileid: "53721126"
---
# <a name="use-azure-data-lake-storage-gen2-with-apache-hadoop-in-azure-hdinsight"></a>Utilizar o Azure Data Lake Storage Gen2 com o Apache Hadoop no HDInsight do Azure

Recursos de geração 1 de armazenamento do Azure Data Lake como um sistema de ficheiros compatível do Hadoop, Azure Active Directory, de núcleos de demora de geração 2 de armazenamento (ADLS) do Azure Data Lake e POSIX com base em ACLs e integra-se no armazenamento de Blobs do Azure. Esta combinação permite-lhe tirar partido do desempenho de geração 1 de armazenamento do Azure Data Lake ao utilizar também o gerenciamento de ciclo de vida de dados e a criação de camadas de armazenamento de Blobs.

## <a name="core-functionality-of-azure-data-lake-storage-gen2"></a>Funcionalidade principal de geração 2 de armazenamento do Azure Data Lake

- Acesso compatível do Hadoop: Geração de armazenamento 2 do Azure Data Lake permite-lhe gerir e aceder a dados, tal como faria com um Hadoop Distributed File System (HDFS). O driver ABFS está disponível em todos os ambientes do Apache Hadoop, incluindo o Azure HDInsight e o Azure Databricks para acessar dados armazenados na geração 2 de armazenamento do Data Lake.

- Um conjunto mais amplo de permissões de POSIX: O modelo de segurança para a geração 2 do Data Lake suporta permissões de ACL e POSIX, juntamente com alguns extra granularidade específica de geração 2 de armazenamento do Data Lake. Definições podem ser configuradas por meio de ferramentas de administração ou estruturas como o Apache Hive e Apache Spark.

- Económico: Geração 2 de armazenamento do Data Lake oferece a capacidade de armazenamento de baixo custo e transações. Recursos como o ciclo de vida de armazenamento de Blobs do Azure ajudam a reduzir os custos ao ajustar as taxas de faturas, como os dados são movidos através de seu ciclo de vida.

- Funciona com aplicações, estruturas e ferramentas de armazenamento de BLOBs: Geração 2 de armazenamento do Data Lake continua a trabalhar com um vasto leque de ferramentas, estruturas e aplicativos que existem hoje mesmo para o armazenamento de Blobs.

- Controlador otimizada: O driver ABFS está otimizado especificamente para análise de macrodados. As APIs REST correspondente são apresentadas através do ponto final de dfs, dfs.core.windows.net.

## <a name="whats-new-about-azure-data-lake-storage-gen-2"></a>Quais são as novidades sobre o Azure Data Lake armazenamento Gen 2

### <a name="managed-identities-for-secure-file-access"></a>Gerido identidades para acesso de arquivo seguro

O Azure HDInsight utiliza identidades geridas para proteger o acesso de cluster para ficheiros na geração 2 de armazenamento do Azure Data Lake. Identidades geridas são uma funcionalidade do Azure Active Directory que fornece serviços do Azure com um conjunto de credenciais automaticamente geridos. Estas credenciais podem ser utilizadas para autenticar a qualquer serviço que suporta a autenticação do AD. A utilização de identidades geridas não requer armazenamento de credenciais nos arquivos de código ou configuração.

Para obter mais informações, consulte [o que há de identidades geridas para recursos do Azure](../active-directory/managed-identities-azure-resources/overview.md).

### <a name="azure-blob-filesystem-abfs-driver"></a>Driver de sistema de ficheiros (ABFS) de Blob do Azure

Aplicações do Apache Hadoop nativamente esperam ler e gravar dados de armazenamento de disco local. Um driver de sistema de ficheiros Hadoop como ABFS permite que as aplicações do Hadoop trabalhar com o armazenamento na cloud emulando regulares operações de sistema de ficheiros Hadoop para a aplicação. O driver converte, em seguida, essas operações em operações que compreende a plataforma de armazenamento na cloud reais.

Anteriormente, o driver de sistema de ficheiros Hadoop seria converter todas as operações de sistema de ficheiros para chamadas de API de REST de armazenamento do Azure no lado do cliente e, em seguida, invocar a API REST. Este cliente conversão, no entanto, resultado na API de REST vários chama-se de uma operação de sistema de ficheiros único como mudar o nome de um ficheiro. ABFS foi movido parte da lógica de sistema de ficheiros de Hadoop do lado do cliente para o lado do servidor e a API de geração 2 do ADLS agora é executado em paralelo com a API de Blobs. Esta migração melhora o desempenho, porque agora podem ser executadas operações de sistema de ficheiros comuns do Hadoop com uma chamada à REST API.

Para obter mais informações, consulte [driver de sistema de ficheiros de Blob do Azure (ABFS): Um driver de armazenamento do Azure dedicado para o Apache Hadoop](../storage/data-lake-storage/abfs-driver.md).

### <a name="adls-gen-2-uri-scheme"></a>Esquema de URI do ADLS Gen 2

Geração 2 do ADLS usa um novo esquema de URI para aceder a ficheiros no armazenamento do Azure do HDInsight:

`abfs[s]://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/<PATH>`

O esquema URI fornece acesso de encriptação SSL (`abfss://` prefixo) e não encriptados, acesso (`abfs://` prefixo). Recomendamos que utilize `abfss` sempre que possível, até mesmo ao aceder a dados que se encontrem dentro da mesma região no Azure.

`<FILE_SYSTEM_NAME>` identifica o caminho do sistema de arquivos de geração 2 de armazenamento do Data Lake.

`<ACCOUNT_NAME>` identifica o nome da conta de armazenamento do Azure. É necessário um nome de domínio completamente qualificado (FQDN).

O `<PATH>` é o nome de caminho de ficheiro ou diretório HDFS.

Se os valores para `<FILE_SYSTEM_NAME>` e `<ACCOUNT_NAME>` não estiver especificado, é utilizado o sistema de ficheiros predefinido. Para os ficheiros no sistema de ficheiros predefinido, pode utilizar um caminho relativo ou um caminho absoluto. Por exemplo, o `hadoop-mapreduce-examples.jar` ficheiro que é fornecido com clusters do HDInsight pode ser referido utilizando um dos seguintes caminhos:

```
abfss://myfilesystempath@myaccount.dfs.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
abfss:///example/jars/hadoop-mapreduce-examples.jar /example/jars/hadoop-mapreduce-examples.jar
```

> [!NOTE]  
> O nome de ficheiro é `hadoop-examples.jar` em clusters do HDInsight versões 2.1 e 1.6. Ao trabalhar com arquivos fora do HDInsight, a maioria dos utilitários não reconhece os ABFS formatar e em vez disso, esperar que um formato de caminho básico, tal como `example/jars/hadoop-mapreduce-examples.jar`.

Para obter mais informações, consulte [utilizam o URI de geração 2 de armazenamento do Azure Data Lake](../storage/data-lake-storage/introduction-abfs-uri.md).

## <a name="next-steps"></a>Passos Seguintes
- [Introdução ao Azure Data Lake Storage Gen2](../storage/data-lake-storage/introduction.md)
- [Utilizar a pré-visualização do Azure Data Lake Storage Gen2 com clusters do HDInsight do Azure](../storage/data-lake-storage/use-hdi-cluster.md)