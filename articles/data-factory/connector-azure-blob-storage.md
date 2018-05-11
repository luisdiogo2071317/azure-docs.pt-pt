---
title: Copiar dados de ou para armazenamento de Blobs do Azure utilizando o Data Factory | Microsoft Docs
description: Saiba como copiar dados de arquivos de dados de origem suportada para o armazenamento de Blobs do Azure ou de armazenamento de BLOBs para os arquivos de dados dependente suportados, utilizando o Data Factory.
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: article
ms.date: 04/27/2018
ms.author: jingwang
ms.openlocfilehash: 1b7926dace37803d26ee2dd54a3eeebf37edbab2
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="copy-data-to-or-from-azure-blob-storage-by-using-azure-data-factory"></a>Copiar dados de ou para armazenamento de Blobs do Azure utilizando o Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versão 1 - Disponibilidade geral](v1/data-factory-azure-blob-connector.md)
> * [Versão 2 - Pré-visualização](connector-azure-blob-storage.md)

Este artigo descreve como utilizar a atividade de cópia no Azure Data Factory para copiar dados de e para o Blob storage do Azure. Baseia-se no [descrição geral da atividade de cópia](copy-activity-overview.md) artigo que apresenta uma descrição geral da atividade de cópia.

> [!NOTE]
> Este artigo aplica-se à versão 2 do Data Factory, que está atualmente em pré-visualização. Se utilizar a versão 1 do Data Factory, o que é geralmente disponível, consulte [conector de armazenamento de BLOBs na versão 1](v1/data-factory-azure-blob-connector.md).


## <a name="supported-capabilities"></a>Capacidades suportadas

Pode copiar dados a partir de qualquer arquivo de dados de origem suportada para o Blob storage. Também pode copiar dados do armazenamento de BLOBs para qualquer arquivo de dados suportados sink. Para obter uma lista dos arquivos de dados que são suportados como origens ou sinks pela atividade de cópia, consulte o [arquivos de dados suportados](copy-activity-overview.md) tabela.

Especificamente, este conector de armazenamento de BLOBs suporta:

- Copiar os blobs e de contas do storage do Azure para fins gerais e de armazenamento de BLOBs/útil. 
- Copiar os blobs utilizando a chave de conta e o serviço partilhado autenticações de assinatura de acesso.
- Cópia de blobs de blocos, acrescentar ou blobs de páginas e copiar os dados apenas de blobs de blocos. Armazenamento Premium do Azure não é suportado como um sink porque esteja associada a blobs de páginas.
- Copiar os blobs como está, analisar ou gerar blobs com [suportado os formatos de ficheiro e compressão codecs](supported-file-formats-and-compression-codecs.md).

## <a name="get-started"></a>Introdução

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

As secções seguintes fornecem detalhes sobre as propriedades que são utilizados para definir entidades do Data Factory específicas para o Blob storage.

## <a name="linked-service-properties"></a>Propriedades de serviço ligado

### <a name="use-an-account-key"></a>Utilizar uma chave de conta

Pode criar um serviço ligado de armazenamento utilizando a chave de conta. Fornece a fábrica de dados com acesso global para o armazenamento. São suportadas as seguintes propriedades.

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo tem de ser definida **AzureStorage**. |Sim |
| connectionString | Especifique as informações necessárias para ligar ao armazenamento para a propriedade connectionString. Marcar este campo como um SecureString armazena de forma segura na fábrica de dados, ou [referenciar um segredo armazenado no Cofre de chaves do Azure](store-credentials-in-key-vault.md). |Sim |
| connectVia | O [integração runtime](concepts-integration-runtime.md) para ser utilizado para ligar ao arquivo de dados. Pode utilizar o Runtime de integração do Azure ou o tempo de execução do Self-hosted integração (se o arquivo de dados estiver numa rede privada). Se não for especificado, utiliza a predefinição de Runtime de integração do Azure. |Não |

**Exemplo:**

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="use-service-shared-access-signature-authentication"></a>Utilizar a autenticação de assinatura de acesso partilhado do serviço

Também pode criar um serviço ligado do Storage utilizando uma assinatura de acesso partilhado. Fornece a fábrica de dados com acesso restrito/vínculo de tempo para recursos de todos os/específico (/ contentor do blob) no armazenamento.

Uma assinatura de acesso partilhado fornece acesso delegado a recursos na sua conta de armazenamento. Pode utilizar uma assinatura de acesso partilhado para conceder que um cliente limitada permissões para objetos na sua conta de armazenamento para um período de tempo especificado. Não têm de partilhar as chaves de acesso da conta. A assinatura de acesso partilhado é um URI que abrange nos respetivos parâmetros de consulta todas as informações necessárias para acesso autenticado a um recurso de armazenamento. Para aceder a recursos de armazenamento com a assinatura de acesso partilhado, o cliente só tem de passar a assinatura de acesso partilhado para o método ou construtor adequado. Para mais informações sobre assinaturas de acesso partilhado, consulte [assinaturas de acesso partilhado: compreender o modelo de assinatura de acesso partilhado](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

> [!IMPORTANT]
> Fábrica de dados agora suporta apenas assinaturas de acesso partilhado do serviço, mas não assinaturas de acesso de conta partilhada. Para obter mais informações sobre estes dois tipos e de como construi-las, consulte [tipos de assinaturas de acesso partilhado](../storage/common/storage-dotnet-shared-access-signature-part-1.md#types-of-shared-access-signatures). O acesso partilhado o URL de assinatura gerado a partir do portal do Azure ou no Explorador de armazenamento do Azure é uma assinatura de acesso partilhado de conta, que não é suportada.

> [!TIP]
> Pode executar os seguintes comandos do PowerShell para gerar uma assinatura de acesso partilhado do serviço para a sua conta de armazenamento. Substitua os marcadores de posição e conceder a permissão necessária.
> `$context = New-AzureStorageContext -StorageAccountName <accountName> -StorageAccountKey <accountKey>`
> `New-AzureStorageContainerSASToken -Name <containerName> -Context $context -Permission rwdl -StartTime <startTime> -ExpiryTime <endTime> -FullUri`

Para utilizar a autenticação de assinatura de acesso partilhado do serviço, são suportadas as seguintes propriedades.

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo tem de ser definida **AzureStorage**. |Sim |
| sasUri | Especifique a URI de assinatura de acesso partilhado para os recursos de armazenamento, como BLOBs, contentor ou tabela. Marcar este campo como um SecureString armazena de forma segura na fábrica de dados, ou [referenciar um segredo armazenado no Cofre de chaves do Azure](store-credentials-in-key-vault.md). |Sim |
| connectVia | O [integração runtime](concepts-integration-runtime.md) para ser utilizado para ligar ao arquivo de dados. Pode utilizar o tempo de execução de integração do Azure ou o tempo de execução de integração Self-hosted (se o arquivo de dados está localizado numa rede privada). Se não for especificado, utiliza a predefinição de Runtime de integração do Azure. |Não |

**Exemplo:**

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "sasUri": {
                "type": "SecureString",
                "value": "<SAS URI of the Azure Storage resource>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

Quando cria uma assinatura de acesso partilhado URI, considere os seguintes pontos:

- Definir permissões de leitura/escrita adequada em objetos com base na forma como o serviço ligado (leitura, escrita, leitura/escrita) é utilizado na fábrica de dados.
- Definir **hora de expiração** adequadamente. Certifique-se de que o acesso a objetos de armazenamento não expira com o período ativo do pipeline.
- O URI deve ser criado no nível de contentor/blob ou tabela à direita com base na necessidade. Uma assinatura de acesso partilhado URI para um blob permite Data Factory para aceder a esse blob específico. Uma assinatura de acesso partilhado URI para um contentor de Blob storage permite Data Factory para iterar através de blobs no contentor. Para fornecer acesso a objetos mais ou menos mais tarde, ou para atualizar a assinatura de acesso partilhado URI, não se esqueça de atualizar o serviço ligado com o URI de novo.

## <a name="dataset-properties"></a>Propriedades do conjunto de dados

Para uma lista completa das secções e propriedades disponíveis para definir os conjuntos de dados, consulte o [conjuntos de dados](concepts-datasets-linked-services.md) artigo. Esta secção fornece uma lista de propriedades suportado pelo conjunto de dados de armazenamento de Blobs.

Para copiar dados para e do armazenamento de BLOBs, defina a propriedade de tipo do conjunto de dados para **AzureBlob**. São suportadas as seguintes propriedades.

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo do conjunto de dados tem de ser definida **AzureBlob**. |Sim |
| folderPath | Caminho para o contentor e a pasta no armazenamento de Blobs. Filtro de caráter universal não é suportado. Um exemplo é myblobcontainer/myblobfolder /. |Sim |
| fileName | **Filtro de nome ou o caráter universal** para blob(s) sob a "folderPath" especificado. Se não especificar um valor para esta propriedade, o conjunto de dados aponta para todos os blobs na pasta. <br/><br/>Para o filtro, permitidos são de carateres universais: `*` (vários carateres) e `?` (único caráter).<br/>-Exemplo 1: `"fileName": "*.csv"`<br/>-Exemplo 2: `"fileName": "???20180427.txt"`<br/>Utilize `^` para o escape se o nome de ficheiro real tem carateres universais ou este caráter de escape no interior.<br/><br/>Quando o nome de ficheiro não está especificado para um conjunto de dados de saída e **preserveHierarchy** não esteja especificado o sink de atividade, a atividade de cópia gera automaticamente o nome do blob com o seguinte padrão: "*dados. [ id de execução da atividade GUID]. [GUID se FlattenHierarchy]. [formato se configurado]. [compressão se configurado]* ". Um exemplo é "Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.gz". |Não |
| formato | Se pretender copiar ficheiros como entre arquivos baseados em ficheiros (cópia binário), ignore a secção de formato em ambas as definições do conjunto de dados de entrada e de saída.<br/><br/>Se pretender analisar ou gerar ficheiros com um formato específico, são suportados os seguintes tipos de formato de ficheiro: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, e **ParquetFormat**. Definir o **tipo** propriedade **formato** para um destes valores. Para obter mais informações, consulte o [formato de texto](supported-file-formats-and-compression-codecs.md#text-format), [formato JSON](supported-file-formats-and-compression-codecs.md#json-format), [formato Avro](supported-file-formats-and-compression-codecs.md#avro-format), [Orc formato](supported-file-formats-and-compression-codecs.md#orc-format), e [formato Parquet](supported-file-formats-and-compression-codecs.md#parquet-format) secções. |Nenhum (apenas para cenário de cópia binário) |
| Compressão | Especifique o tipo e o nível de compressão de dados. Para obter mais informações, consulte [suportado os formatos de ficheiro e compressão codecs](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Tipos suportados são **GZip**, **Deflate**, **BZip2**, e **ZipDeflate**.<br/>Níveis suportados são **Optimal** e **Fastest**. |Não |

>[!TIP]
>Para copiar todos os blobs numa pasta, especifique **folderPath** apenas.<br>Para copiar um blob único com um nome específico, especifique **folderPath** com parte de pasta e **fileName** com o nome de ficheiro.<br>Para copiar um subconjunto de blobs de uma pasta, especifique **folderPath** com parte de pasta e **fileName** com filtro de caráter universal. 

**Exemplo:**

```json
{
    "name": "AzureBlobDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": {
            "referenceName": "<Azure Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "mycontainer/myfolder",
            "fileName": "myfile.csv.gz",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propriedades da atividade Copy

Para uma lista completa das secções e propriedades disponíveis para definir as atividades, consulte o [Pipelines](concepts-pipelines-activities.md) artigo. Esta secção fornece uma lista de propriedades suportado pela origem de armazenamento de BLOBs e sink.

### <a name="blob-storage-as-a-source-type"></a>Armazenamento de BLOBs como um tipo de origem

Para copiar dados de armazenamento de BLOBs, defina o tipo de origem na atividade de cópia para **BlobSource**. As seguintes propriedades são suportadas na atividade de cópia **origem** secção.

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo da origem de atividade de cópia tem de ser definida **BlobSource**. |Sim |
| Recursiva | Indica se os dados é leitura recursivamente as subpastas ou apenas a pasta especificada. Tenha em atenção que quando recursiva está definida como VERDADEIRO e o sink é um arquivo baseado em ficheiros, uma pasta vazia ou subpasta não é copiada ou criada ao sink.<br/>Valores permitidos são **verdadeiro** (predefinição) e **falso**. | Não |

**Exemplo:**

```json
"activities":[
    {
        "name": "CopyFromBlob",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure Blob input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "BlobSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="blob-storage-as-a-sink-type"></a>Armazenamento de BLOBs como um tipo de sink

Para copiar dados para armazenamento de BLOBs, defina o tipo de sink na atividade de cópia para **BlobSink**. São suportadas as seguintes propriedades no **sink** secção.

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo de sink de atividade de cópia tem de ser definida **BlobSink**. |Sim |
| copyBehavior | Define o comportamento de cópia quando a origem é ficheiros a partir de um arquivo de dados baseada em ficheiros.<br/><br/>Valores permitidos são:<br/><b>-PreserveHierarchy (predefinição)</b>: preserva a hierarquia de ficheiros na pasta de destino. O caminho relativo do ficheiro de origem para a pasta de origem é idêntico ao caminho relativo do ficheiro de destino para a pasta de destino.<br/><b>-FlattenHierarchy</b>: todos os ficheiros da pasta de origem estão no primeiro nível da pasta de destino. Os ficheiros de destino têm nomes gerado automaticamente. <br/><b>-MergeFiles</b>: une todos os ficheiros da pasta de origem para um ficheiro. Se não for especificado o nome de ficheiro ou de blob, o nome de ficheiro intercalado é o nome especificado. Caso contrário, é um nome de ficheiro gerado automaticamente. | Não |

**Exemplo:**

```json
"activities":[
    {
        "name": "CopyToBlob",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure Blob output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "BlobSink",
                "copyBehavior": "PreserveHierarchy"
            }
        }
    }
]
```

### <a name="some-recursive-and-copybehavior-examples"></a>Alguns exemplos de recursiva e copyBehavior

Esta secção descreve o comportamento resultante da operação de cópia para diferentes combinações de valores recursiva e copyBehavior.

| Recursiva | copyBehavior | Estrutura de pasta de origem | Destino resultante |
|:--- |:--- |:--- |:--- |
| true |preserveHierarchy | Pasta1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | a pasta de destino Pasta1 é criada com a mesma estrutura de origem:<br/><br/>Pasta1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 |
| true |flattenHierarchy | Pasta1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | o destino Pasta1 é criado com a estrutura seguinte: <br/><br/>Pasta1<br/>&nbsp;&nbsp;&nbsp;&nbsp;nome gerado automaticamente para File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;nome gerado automaticamente para File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;nome gerado automaticamente para File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;nome gerado automaticamente para File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;nome gerado automaticamente para File5 |
| true |mergeFiles | Pasta1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | o destino Pasta1 é criado com a estrutura seguinte: <br/><br/>Pasta1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + File3 + File4 + File5 é intercalado conteúdo para um ficheiro com um nome de ficheiro gerado automaticamente. |
| false |preserveHierarchy | Pasta1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | a pasta de destino Pasta1 é criada com a estrutura seguinte: <br/><br/>Pasta1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/><br/>Não é escolhido Subfolder1 com File3, File4 e File5. |
| false |flattenHierarchy | Pasta1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | a pasta de destino Pasta1 é criada com a estrutura seguinte: <br/><br/>Pasta1<br/>&nbsp;&nbsp;&nbsp;&nbsp;nome gerado automaticamente para File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;nome gerado automaticamente para File2<br/><br/>Não é escolhido Subfolder1 com File3, File4 e File5. |
| false |mergeFiles | Pasta1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5 | a pasta de destino Pasta1 é criada com a seguinte estrutura<br/><br/>Pasta1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 conteúdos são intercalados para um ficheiro com um nome de ficheiro gerado automaticamente. nome gerado automaticamente para File1<br/><br/>Não é escolhido Subfolder1 com File3, File4 e File5. |

## <a name="next-steps"></a>Passos Seguintes
Para obter uma lista dos arquivos de dados suportados como origens e sinks pela atividade de cópia numa fábrica de dados, consulte [arquivos de dados suportados](copy-activity-overview.md##supported-data-stores-and-formats).
