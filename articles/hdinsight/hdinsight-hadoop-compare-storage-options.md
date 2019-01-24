---
title: Compare as opções de armazenamento para utilização com clusters do HDInsight do Azure
description: Fornece uma visão geral dos tipos de armazenamento e como eles funcionam com o Azure HDInsight.
services: hdinsight,storage
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/20/2019
ms.openlocfilehash: 14db76068cc11d3f57a72e3e540a5e0da7e1c254
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54853617"
---
# <a name="compare-storage-options-for-use-with-azure-hdinsight-clusters"></a>Compare as opções de armazenamento para utilização com clusters do HDInsight do Azure

Os utilizadores do Azure HDInsight, podem escolher entre algumas opções de armazenamento diferente durante a criação de clusters do HDInsight:

* Armazenamento do Azure Data Lake Ger2
* Storage do Azure
* Armazenamento do Azure Data Lake Ger1

Este artigo fornece uma visão geral desses tipos de armazenamento diferentes e seus recursos exclusivos.

## <a name="use-azure-data-lake-storage-gen2-with-apache-hadoop-in-azure-hdinsight"></a>Utilizar o Azure Data Lake Storage Gen2 com o Apache Hadoop no HDInsight do Azure

Para obter mais informações sobre a geração 2 de armazenamento do Azure Data Lake, veja [introdução ao Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md).

O Azure Data Lake Gen2 de armazenamento leva core recursos de geração 1 de armazenamento do Azure Data Lake como um sistema de ficheiros compatível Hadoop, o Azure Active Directory e o acesso baseadas em POSIX (ACLs) de listas de controle e integra-se no armazenamento de Blobs do Azure. Esta combinação permite-lhe tirar partido do desempenho de geração 1 de armazenamento do Azure Data Lake ao utilizar também o gerenciamento de ciclo de vida de dados e a criação de camadas de armazenamento de Blobs.

### <a name="core-functionality-of-azure-data-lake-storage-gen2"></a>Funcionalidade principal de geração 2 de armazenamento do Azure Data Lake

* Acesso compatível do Hadoop: Geração de armazenamento 2 do Azure Data Lake permite-lhe gerir e aceder a dados, tal como faria com um Hadoop Distributed File System (HDFS). O driver de sistema de ficheiros de Blob do Azure (ABFS) está disponível em todos os ambientes do Apache Hadoop, incluindo o Azure HDInsight e o Azure Databricks para acessar dados armazenados na geração 2 de armazenamento do Data Lake.

* Um conjunto mais amplo de permissões de POSIX: O modelo de segurança para a geração 2 do Data Lake suporta permissões de ACL e POSIX, juntamente com alguns extra granularidade específica de geração 2 de armazenamento do Data Lake. Definições podem ser configuradas por meio de ferramentas de administração ou estruturas como o Apache Hive e Apache Spark.

* Económico: Geração 2 de armazenamento do Data Lake oferece a capacidade de armazenamento de baixo custo e transações. Recursos como o ciclo de vida de armazenamento de Blobs do Azure ajudam a reduzir os custos ao ajustar as taxas de faturas, como os dados são movidos através de seu ciclo de vida.

* Funciona com aplicações, estruturas e ferramentas de armazenamento de BLOBs: Geração 2 de armazenamento do Data Lake continua a trabalhar com um vasto leque de ferramentas, estruturas e aplicativos que existem hoje mesmo para o armazenamento de Blobs.

* Controlador otimizada: O driver ABFS está otimizado especificamente para análise de macrodados. As APIs REST correspondente são apresentadas através do ponto final de dfs, dfs.core.windows.net.

### <a name="whats-new-about-azure-data-lake-storage-gen-2"></a>Quais são as novidades sobre o Azure Data Lake armazenamento Gen 2

#### <a name="managed-identities-for-secure-file-access"></a>Gerido identidades para acesso de arquivo seguro

O Azure HDInsight utiliza identidades geridas para proteger o acesso de cluster para ficheiros na geração 2 de armazenamento do Azure Data Lake. Identidades geridas são uma funcionalidade do Azure Active Directory que fornece serviços do Azure com um conjunto de credenciais automaticamente geridos. Estas credenciais podem ser utilizadas para autenticar a qualquer serviço que suporta a autenticação do AD. A utilização de identidades geridas não requer armazenamento de credenciais nos arquivos de código ou configuração.

Para obter mais informações, consulte [o que há de identidades geridas para recursos do Azure](../active-directory/managed-identities-azure-resources/overview.md).

#### <a name="azure-blob-filesystem-abfs-driver"></a>Driver de sistema de ficheiros (ABFS) de Blob do Azure

Aplicações do Apache Hadoop nativamente esperam ler e gravar dados de armazenamento de disco local. Um driver de sistema de ficheiros Hadoop como ABFS permite que as aplicações do Hadoop trabalhar com o armazenamento na cloud emulando regulares operações de sistema de ficheiros Hadoop. O driver converte esses comandos recebidos da aplicação em operações que compreende a plataforma de armazenamento na cloud reais.

Anteriormente, o driver de sistema de ficheiros Hadoop seria converter todas as operações de sistema de ficheiros para chamadas de API de REST de armazenamento do Azure no lado do cliente e, em seguida, invocar a API REST. Este cliente conversão, no entanto, resultado na API de REST vários chama-se de uma operação de sistema de ficheiros único como mudar o nome de um ficheiro. ABFS foi movido parte da lógica de sistema de ficheiros de Hadoop do lado do cliente para a API de geração 2 de armazenamento do Azure Data Lake e do servidor agora é executado em paralelo com a API de Blobs. Esta migração melhora o desempenho, porque agora podem ser executadas operações de sistema de ficheiros comuns do Hadoop com uma chamada à REST API.

Para obter mais informações, consulte [driver de sistema de ficheiros de Blob do Azure (ABFS): Um driver de armazenamento do Azure dedicado para o Hadoop](../storage/blobs/data-lake-storage-abfs-driver.md).

#### <a name="azure-data-lake-storage-gen-2-uri-scheme"></a>Esquema de URI do Data Lake armazenamento Gen 2 do Azure

Geração de armazenamento 2 do Azure Data Lake utiliza um novo esquema de URI para aceder a ficheiros no armazenamento do Azure do HDInsight:

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

> [!Note]
> O nome de ficheiro é `hadoop-examples.jar` em clusters do HDInsight versões 2.1 e 1.6. Ao trabalhar com arquivos fora do HDInsight, a maioria dos utilitários não reconhece os ABFS formatar e em vez disso, esperar que um formato de caminho básico, tal como `example/jars/hadoop-mapreduce-examples.jar`.

Para obter mais informações, consulte [utilizam o URI de geração 2 de armazenamento do Azure Data Lake](../storage/blobs/data-lake-storage-introduction-abfs-uri.md).

## <a name="use-azure-storage"></a>Utilizar o armazenamento do Azure

O armazenamento do Azure é uma solução de armazenamento para fins gerais robusta que se integra perfeitamente no HDInsight. O HDInsight pode utilizar um contentor de blobs no Armazenamento do Azure como o sistema de ficheiros predefinido para o cluster. Através de uma interface HDFS (Sistema de Ficheiros Distribuído Hadoop), o conjunto completo de componentes do HDInsight pode operar diretamente em dados estruturados ou não estruturados armazenados como blobs.

> [!WARNING]  
> Existem várias opções disponíveis quando criar uma conta de Armazenamento do Azure. A tabela seguinte fornece informações sobre as opções suportadas no HDInsight:

| Tipo de conta de armazenamento | Serviços suportados | Escalões de desempenho suportados | Suporte de acesso |
|----------------------|--------------------|-----------------------------|------------------------|
| Para fins gerais V2   | Blobs               | Standard                    | Frequente, esporádico, arquivo *    |
| Para fins gerais V1   | Blobs               | Standard                    | N/A                    |
| Armazenamento de blobs         | Blobs               | Standard                    | Frequente, esporádico, arquivo *    |

Não recomendamos que utilize o contentor de BLOBs predefinido para armazenar dados empresariais. Eliminar o contentor de blobs predefinido depois de cada utilização para reduzir o custo de armazenamento é uma prática recomendada. O contentor predefinido contém o aplicativo e sistema de registos. Certifique-se de que obtém os registos antes de eliminar o contentor.

Utilizar um contentor de BLOBs como o sistema de ficheiros predefinido para múltiplos clusters não é suportada.
 
 > [!NOTE]  
 > A camada de acesso de arquivo é uma camada offline que tenha uma latência de obtenção de hora várias e não é recomendada para utilização com o HDInsight. Para obter mais informações, consulte [camada de acesso de arquivo](../storage/blobs/storage-blob-storage-tiers.md#archive-access-tier).

### <a name="hdinsight-storage-architecture"></a>Arquitetura de armazenamento do HDInsight
O diagrama seguinte apresenta uma vista abstrata da arquitetura de armazenamento do HDInsight relativamente à utilização do Armazenamento do Azure:

![Os clusters do Hadoop utilizam a API do HDFS para aceder e armazenar dados estruturados e não estruturados no Armazenamento de Blobs.](./media/hdinsight-hadoop-compare-storage-options/HDI.WASB.Arch.png "Arquitetura do Armazenamento do HDInsight")

O HDInsight fornece acesso ao sistema de ficheiros distribuído que está ligado localmente aos nós de computação. É possível aceder a este sistema de ficheiros ao utilizar o URI completamente qualificado, por exemplo:

    hdfs://<namenodehost>/<path>

Além disso, o HDInsight permite aceder aos dados armazenados no Armazenamento do Azure. A sintaxe é:

    wasb[s]://<containername>@<accountname>.blob.core.windows.net/<path>

Seguem-se algumas considerações sobre a utilização da conta do Azure Storage com os clusters do HDInsight.

* **Contentores nas contas de armazenamento que estão ligadas a um cluster:** Como o nome de conta e chave são associados ao cluster durante a criação, tem acesso total aos blobs desses contentores.

* **Contentores públicos ou blobs públicos em contas de armazenamento que não estão ligados a um cluster:** Tem a permissão só de leitura para os blobs dos contentores.
  
  > [!NOTE]  
  > Os contentores públicos permitem obter uma lista de todos os blobs disponíveis nesse contentor, bem como os metadados do mesmo. Os blobs públicos permitem aceder aos blobs apenas se souber o URL exato. Para obter mais informações, consulte [gerir o acesso a contentores e blobs](../storage/blobs/storage-manage-access-to-resources.md).

* **Contentores privados em contas de armazenamento que não estão ligados a um cluster:** Não é possível aceder os blobs dos contentores, a menos que define a conta de armazenamento, ao submeter as tarefas de WebHCat. Isto é explicado posteriormente neste artigo.

As contas do Storage definidas durante o processo de criação e as respetivas chaves são armazenadas em %HADOOP_HOME%/conf/core-site.xml nos nós do cluster. O comportamento predefinido do HDInsight é utilizar as contas do Storage definidas no ficheiro core-site.xml. Pode modificar esta definição através de [Apache Ambari](./hdinsight-hadoop-manage-ambari.md).

Várias tarefas de WebHCat, incluindo Apache Hive, MapReduce, transmissão em fluxo do Apache Hadoop e Apache Pig, podem conter uma descrição das contas de armazenamento e de metadados com eles. (Atualmente, isto funciona para o Pig com contas do Storage, mas não com metadados.) Para obter mais informações, consulte [Utilizar um Cluster do HDInsight com Contas do Storage e Metastores Alternativos](https://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx).

Os blobs podem ser utilizados para dados estruturados e não estruturados. Os contentores de blobs armazenam dados como pares chave/valor e não existe uma hierarquia de diretórios. No entanto, o caráter de barra (/) pode ser utilizado no nome da chave para fazer com que pareça que um ficheiro está armazenado numa estrutura de diretórios. Por exemplo, a chave de um blob pode ser `input/log1.txt`. Não real `input` o diretório existe, mas devido à presença do caráter de barra no nome da chave, tem a aparência de um caminho de ficheiro.

### <a id="benefits"></a>Vantagens do Armazenamento do Azure
O custo de desempenho implícito de não ter clusters de cálculo e recursos de armazenamento colocalizados é atenuado pela forma como os clusters de cálculo são criados perto dos recursos da conta de armazenamento na região do Azure, onde a rede de alta velocidade permite que os nós de cálculo acedam aos dados de armazenamento do Azure de forma eficiente.

Existem várias vantagens associadas ao armazenamento de dados no armazenamento do Azure em vez do HDFS:

* **Reutilização de dados e de partilha:** Os dados no HDFS estão localizados dentro do cluster de computação. Apenas as aplicações que têm acesso ao cluster de cálculo podem utilizar os dados ao utilizar as APIs do HDFS. Os dados no armazenamento do Azure podem ser acedidos através das APIs do HDFS ou através de APIs de REST do armazenamento de Blobs. Assim, pode-se utilizar um conjunto maior de ferramentas e aplicações (incluindo outros clusters do HDInsight) para produzir e consumir dados.
* **Arquivamento de dados:** Armazenamento de dados no armazenamento do Azure permite que os clusters do HDInsight utilizados para o cálculo a eliminar em segurança sem perda de dados de utilizador.
* **Custo de armazenamento de dados:** Armazenar dados no DFS a longo prazo é mais dispendioso do que armazenar os dados no armazenamento do Azure, uma vez que o custo de um cluster de cálculo é superior ao custo de armazenamento do Azure. Além disso, uma vez que os dados não tem de ser recarregado para que cada geração de cluster de computação, está também reduz os custos de carregamento de dados.
* **Aumento horizontal elástico:** Embora o HDFS forneça um sistema de ficheiros de escalamento horizontal, o dimensionamento é determinado pelo número de nós que criar para o seu cluster. A alteração do dimensionamento pode tornar-se um processo mais complexo do que depender das capacidades de dimensionamento elástico que obtém automaticamente no armazenamento do Azure.
* **Georreplicação:** O armazenamento do Azure pode ser georreplicado. Apesar de esta funcionalidade oferecer recuperação geográfica e redundância de dados, uma ativação pós-falha para a localização georreplicada afeta seriamente o desempenho e pode implicar custos adicionais. Por isso, recomendamos que escolha a georreplicação de forma sensata e apenas se o valor dos dados justificar o custo adicional.

Determinados pacotes e tarefas de MapReduce podem criar resultados intermédios que não quer realmente armazenar no armazenamento do Azure. Nesse caso, pode optar por armazenar os dados no HDFS local. Na verdade, o HDInsight utiliza o DFS para vários destes resultados intermédios nas tarefas do Hive e noutros processos.

> [!NOTE]  
> Maioria dos comandos HDFS (por exemplo, `ls`, `copyFromLocal` e `mkdir`) continuarão a funcionar conforme esperado. Apenas os comandos que são específicos para a implementação nativa do HDFS (que é conhecida como DFS), tal como `fschk` e `dfsadmin`, apresentam um comportamento diferente no armazenamento do Azure.

## <a name="use-azure-data-lake-storage-gen1"></a>Utilizar o Azure Data Lake Storage Gen1

Para obter mais informações sobre a geração 1 de armazenamento do Azure Data Lake, veja [descrição geral do Azure Data Lake armazenamento Gen1](../data-lake-store/data-lake-store-overview.md).

Geração de armazenamento 1 do Azure Data Lake é um repositório de hiperescala de toda a empresa para cargas de trabalho de análise de macrodados. O Azure Data Lake permite-lhe capturar dados de qualquer tamanho, tipo e velocidade de ingestão num único local para análises exploratórias e operacionais.

Geração 1 de armazenamento do Data Lake pode ser acedido a partir do Hadoop (disponível com um cluster do HDInsight) com as APIs de REST compatíveis com WebHDFS. Ele foi projetado para permitir análises em dados armazenados e está otimizado para desempenho para cenários de análises de dados. Fácil de utilizar, inclui todas as funcionalidades de nível empresarial (segurança, capacidade de gestão, escalabilidade, fiabilidade e disponibilidade), essenciais para casos de utilização empresarial reais.

![Armazenamento do Azure Data Lake](./media/hdinsight-hadoop-compare-storage-options/data-lake-store-concept.png "arquitetura de armazenamento do HDInsight")

Seguem-se alguns dos principais recursos de geração 1 de armazenamento do Data Lake.

### <a name="built-for-hadoop"></a>Incorporado para o Hadoop

Geração 1 de armazenamento do Data Lake é um sistema de ficheiros Apache Hadoop compatível com o Hadoop Distributed File System (HDFS) e funciona com o ecossistema Hadoop.  O HDInsight aplicações ou serviços que utilizam a API de WebHDFS podem integrar facilmente com geração 1 de armazenamento do Data Lake. Geração 1 de armazenamento do Data Lake também expõe uma interface REST compatível com WebHDFS para aplicações

Dados armazenados no Data Lake Storage Gen1 podem ser facilmente analisados utilizando as estruturas de análise de Hadoop, tais como MapReduce ou Hive. Clusters do Microsoft Azure HDInsight podem ser aprovisionados e configurados para aceder diretamente aos dados armazenados no Data Lake Storage Gen1.

### <a name="unlimited-storage-petabyte-files"></a>Armazenamento ilimitado, ficheiros petabyte

Geração 1 de armazenamento do Data Lake fornece armazenamento ilimitado e é adequado para armazenar diversos dados para análise. Não tem quaisquer limites de tamanhos de conta, de ficheiros ou de volume de dados que podem ser armazenados num data lake. Os ficheiros individuais podem variar entre kilobytes e petabytes em termos de tamanho, sendo uma escolha ideal para armazenar qualquer tipo de dados. Os dados são armazenados de maneira duradoura, ao criar várias cópias e não existe limite de tempo para o armazenamento dos dados no data lake.

### <a name="performance-tuned-for-big-data-analytics"></a>Desempenho otimizado para análise de macrodados

Geração 1 de armazenamento do Data Lake foi concebido para executar sistemas de análise que requeiram débito em massa para consultar e analisar grandes quantidades de dados de grande escala. O data lake propaga partes de um ficheiro ao longo de um número de servidores de armazenamento individuais. Isto melhora o débito de leitura ao ler o ficheiro em paralelo para efetuar análise de dados.

### <a name="enterprise-ready-highly-available-and-secure"></a>Pronto para empresas: Elevada disponibilidade e segura

Geração 1 de armazenamento do Data Lake fornece disponibilidade de norma da indústria e fiabilidade. Os recursos de dados são armazenados de maneira duradoura, ao fazer cópias redundantes para proteger contra quaisquer falhas inesperadas. As empresas podem utilizar a geração 1 de armazenamento do Data Lake nas respetivas soluções como uma parte importante da respetiva plataforma de dados.

Geração 1 de armazenamento do Data Lake também fornece segurança de nível empresarial para os dados armazenados. Para obter mais informações, consulte [proteger dados no Azure Data Lake Storage Gen1](#DataLakeStoreSecurity).

### <a name="all-data"></a>Todos os Dados

Geração 1 de armazenamento do Data Lake pode armazenar quaisquer dados no seu formato nativo, tal como está, sem a necessidade de qualquer transformações anteriores. Geração 1 de armazenamento do Data Lake não requer um esquema antes dos dados são carregados, deixá-lo para a estrutura de análise individual interpreta os dados e definir um esquema no momento da análise. A capacidade para armazenar os arquivos de formatos e tamanhos arbitrários torna possível para a geração 1 de armazenamento do Data Lake processar dados estruturados, semiestruturados e não estruturados.

Contentores de Lake Gen1 de armazenamento de dados para os dados são essencialmente pastas e ficheiros. Utilizar os dados armazenados através de SDKs, o portal do Azure e o Azure Powershell. Desde que coloque os dados num arquivo utilizando estas interfaces e utilize os contentores adequados, pode armazenar qualquer tipo de dados. Geração 1 de armazenamento do Data Lake não efetua qualquer processamento especial de dados com base no tipo de dados que armazena.

## <a name="DataLakeStoreSecurity"></a>Proteger dados no Data Lake Storage Gen1
Utiliza o Data Lake Gen1 de armazenamento do Azure Active Directory para autenticação e acesso controla listas (ACLs) para gerir o acesso aos seus dados.

| Funcionalidade | Descrição |
| --- | --- |
| Autenticação |Gen1 de armazenamento do Data Lake está integrado com o Azure Active Directory (AAD) para a gestão de identidades e acessos para todos os dados armazenados no Data Lake Storage Gen1. Como resultado de integração, benefícios de geração 1 de armazenamento do Data Lake com todas as funcionalidades do AAD incluindo autenticação multifator, acesso condicional, controlo de acesso baseado em funções, monitorização da utilização de aplicações, segurança de monitorização e alertas, etc. Geração 1 de armazenamento do Data Lake suporta o protocolo de OAuth 2.0 para a autenticação com na REST interface. Consulte [autenticação de geração 1 de armazenamento do Data Lake](../data-lake-store/data-lakes-store-authentication-using-azure-active-directory.md)|
| Controlo de acesso |Geração 1 de armazenamento do Data Lake fornece controlo de acesso ao suportar permissões do estilo POSIX expostas pelo protocolo WebHDFS. As ACLs podem ser ativadas na pasta raiz, nas subpastas e nos ficheiros individuais. Para obter mais informações sobre como as ACLs funcionam no contexto da geração 1 de armazenamento do Data Lake, veja [controlo de acesso no Data Lake Storage Gen1](../data-lake-store/data-lake-store-access-control.md). |
| Encriptação |Geração 1 de armazenamento do Data Lake também fornece encriptação para dados armazenados na conta. Especifique as definições de encriptação durante a criação de uma conta de geração 1 de armazenamento do Data Lake. Pode optar por encriptar os seus dados ou por não utilizar encriptação. Para obter mais informações, consulte [encriptação no Data Lake Storage Gen1](../data-lake-store/data-lake-store-encryption.md). Para obter instruções sobre como fornecer configuração relacionada com encriptação, consulte [introdução à geração 1 de armazenamento do Azure Data Lake com o portal do Azure](../data-lake-store/data-lake-store-get-started-portal.md). |

Quer saber mais sobre como proteger dados no Data Lake Storage Gen1? Siga as ligações abaixo.

* Para obter instruções sobre como proteger os dados na geração 1 de armazenamento do Data Lake, veja [proteger dados no Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-secure-data.md).

## <a name="applications-compatible-with-data-lake-storage-gen1"></a>Aplicações compatíveis com o Data Lake Storage Gen1
Geração 1 de armazenamento do Data Lake é compatível com a maioria dos componentes de código-fonte aberto no ecossistema do Hadoop. Também é integrado corretamente com outros serviços do Azure.  Siga os links abaixo para obter mais informações sobre como geração 1 de armazenamento do Data Lake pode ser utilizado tanto com componentes de código-fonte aberto, bem como outros serviços do Azure.

* Ver [aplicativos e serviços compatíveis com o Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-compatible-oss-other-applications.md) para obter uma lista de aplicações de código aberto interoperáveis com geração 1 de armazenamento do Data Lake.
* Ver [integração com outros serviços do Azure](../data-lake-store/data-lake-store-integrate-with-other-services.md) para compreender como geração 1 de armazenamento do Data Lake pode ser utilizado com outros serviços do Azure para ativar uma vasta gama de cenários.
* Ver [cenários de utilização de geração 1 de armazenamento do Data Lake](../data-lake-store/data-lake-store-data-scenarios.md) para aprender a utilizar o Data Lake Storage Gen1 em cenários como a ingestão de dados, processamento de dados, transferência de dados e visualização de dados.

## <a name="what-is-data-lake-storage-gen1-file-system-adl"></a>O que é o sistema de ficheiros de geração 1 de armazenamento do Data Lake (adl: / /)?
Geração 1 de armazenamento do Data Lake pode ser acedido através do novo sistema de ficheiros, o AzureDataLakeFilesystem (adl: / /), em ambientes do Hadoop (disponíveis com o cluster do HDInsight). As aplicações e serviços que utilizam adl: / / são capazes de tirar partido da otimização do desempenho que não estão atualmente disponíveis no WebHDFS. Como resultado, Gen1 de armazenamento do Data Lake dá-lhe a flexibilidade de beneficiar do melhor desempenho com a opção recomendada de utilização adl: / / ou manter o código existente ao continuar a utilizar a API WebHDFS diretamente. O Azure HDInsight tira total partido do AzureDataLakeFilesystem para proporcionar o melhor desempenho na geração 1 de armazenamento do Data Lake.

Pode aceder aos seus dados através da geração 1 de armazenamento do Data Lake `adl://<data_lake_storage_gen1_name>.azuredatalakestore.net`. Para obter mais informações sobre como acessar os dados no Data Lake Storage Gen1, consulte [ver as propriedades dos dados armazenados](../data-lake-store/data-lake-store-get-started-portal.md#properties)



## <a name="next-steps"></a>Passos Seguintes

* [Introdução ao Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md).
* [Introdução ao Armazenamento do Azure](../storage/common/storage-introduction.md)