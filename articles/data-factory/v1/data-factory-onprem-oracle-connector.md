---
title: Copiar dados de ou para Oracle com o Data Factory | Documentos da Microsoft
description: Saiba como copiar dados de/para Oracle banco de dados no local com o Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: 3c20aa95-a8a1-4aae-9180-a6a16d64a109
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/15/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 10535e75a32a9f95e759340cf14d693f43639473
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37856846"
---
# <a name="copy-data-to-or-from-on-premises-oracle-using-azure-data-factory"></a>Copiar dados de ou para Oracle no local com o Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versão 1](data-factory-onprem-oracle-connector.md)
> * [Versão 2 (versão atual)](../connector-oracle.md)

> [!NOTE]
> Este artigo aplica-se à versão 1 do Data Factory. Se estiver a utilizar a versão atual do serviço Data Factory, veja [conector Oracle no V2](../connector-oracle.md).


Este artigo explica como utilizar a atividade de cópia no Azure Data Factory para mover dados de/para uma base de dados do Oracle no local. Ele se baseia no [atividades de movimento de dados](data-factory-data-movement-activities.md) artigo, que apresenta uma visão geral do movimento de dados com a atividade de cópia.

## <a name="supported-scenarios"></a>Cenários suportados
Pode copiar dados **uma base de dados do Oracle** para os seguintes dados armazena:

[!INCLUDE [data-factory-supported-sink](../../../includes/data-factory-supported-sinks.md)]

Pode copiar dados de arquivos de dados seguintes **para uma base de dados do Oracle**:

[!INCLUDE [data-factory-supported-sources](../../../includes/data-factory-supported-sources.md)]

## <a name="prerequisites"></a>Pré-requisitos
O Data Factory suporta a ligação a origens de Oracle no local com o Data Management Gateway. Ver [Data Management Gateway](data-factory-data-management-gateway.md) artigo para saber mais sobre o Data Management Gateway e [mover dados do local para a cloud](data-factory-move-data-between-onprem-and-cloud.md) artigo para obter instruções passo a passo sobre como configurar o gateway de um pipeline de dados Mova dados.

Gateway é necessário, mesmo que o Oracle é hospedado numa VM de IaaS do Azure. Pode instalar o gateway na mesma VM de IaaS, como o arquivo de dados ou numa VM diferente, desde que o gateway consiga estabelecer ligação à base de dados.

> [!NOTE]
> Ver [resolver problemas de gateway](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) para obter sugestões sobre resolução de problemas do gateway de ligação/problemas relacionados com.

## <a name="supported-versions-and-installation"></a>Versões suportadas e instalação
Este conector Oracle suporta duas versões de controladores:

- **Microsoft driver para Oracle (recomendado)**: a partir de Data Management Gateway versão 2.7, um driver para Oracle é instalado automaticamente, juntamente com o gateway, pelo que não precisa para além disso lidar com o driver da Microsoft estabelecer conectividade a Oracle, e também pode assistir a melhor desempenho de cópia com este controlador. Abaixo as versões do Oracle, bases de dados são suportados:
    - R1 Oracle 12c (12.1)
    - Oracle 11g R1, R2 (11.1, 11.2)
    - Oracle 10g R1, R2 (10.1, 10.2)
    - Oracle 9i R1, R2 (9.0.1, 9.2)
    - Oracle 8i R3 (8.1.7)

> [!NOTE]
> Servidor de proxy do Oracle não é suportada.

> [!IMPORTANT]
> Atualmente a Microsoft driver para Oracle suporta apenas a cópia de dados do Oracle, mas não escrevendo para Oracle. E tenha em atenção que a capacidade de ligação de teste no separador de diagnóstico do Gateway de gestão de dados não suporta este controlador. Em alternativa, pode utilizar o Assistente para copiar para validar a conectividade.
>

- **Fornecedor de dados do Oracle para .NET:** também pode optar por utilizar o fornecedor de dados Oracle para copiar dados de/para Oracle. Este componente está incluído no [Oracle Data Access componentes para o Windows](http://www.oracle.com/technetwork/topics/dotnet/downloads/). Instale a versão adequada (32/64 bits) na máquina onde o gateway está instalado. [Fornecedor de dados Oracle .NET 12.1](http://docs.oracle.com/database/121/ODPNT/InstallSystemRequirements.htm#ODPNT149) pode aceder à base de dados do Oracle 10g versão 2 ou posterior.

    Se optar por "Instalação XCopy", siga os passos no Readme. Recomendamos que escolher o instalador com interface do Usuário (não-XCopy um).

    Depois de instalar o fornecedor **reiniciar** o serviço de anfitrião de Gateway de gestão de dados no seu computador com serviços miniaplicativo (ou) Gestor de configuração do Data Management Gateway.  

Se utilizar o Assistente para copiar para criar o pipeline da cópia, o tipo de driver será determinado para automática. Controlador Microsoft será utilizado por predefinição, a menos que a sua versão do gateway é inferior ao 2.7 ou escolher Oracle como sink.

## <a name="getting-started"></a>Introdução
Pode criar um pipeline com uma atividade de cópia que move os dados de/para uma base de dados do Oracle no local através de APIs/ferramentas diferentes.

A maneira mais fácil para criar um pipeline é utilizar o **Assistente para copiar**. Ver [Tutorial: criar um pipeline com o Assistente para copiar](data-factory-copy-data-wizard-tutorial.md) para um rápido passo a passo sobre como criar um pipeline com o Assistente para copiar dados.

Também pode utilizar as seguintes ferramentas para criar um pipeline: **portal do Azure**, **Visual Studio**, **Azure PowerShell**, **modelo Azure Resource Manager** , **API de .NET**, e **REST API**. Ver [tutorial da atividade de cópia](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) para obter instruções passo a passo Criar um pipeline com uma atividade de cópia.

Se usar as ferramentas ou APIs, que execute os seguintes passos para criar um pipeline que move os dados de um arquivo de dados de origem para um arquivo de dados de sink:

1. Criar uma **fábrica de dados**. Uma fábrica de dados pode conter um ou mais pipelines. 
2. Crie **serviços ligados** para ligar a dados de entrada e saídos armazena à fábrica de dados. Por exemplo, se estiver a copiar dados de uma base de dados Oralce para um armazenamento de Blobs do Azure, criar dois serviços ligados para ligar a sua base de dados Oracle e a conta de armazenamento do Azure à fábrica de dados. Para propriedades de serviço ligado que são específicas a Oracle, consulte [propriedades do serviço ligado](#linked-service-properties) secção.
3. Crie **conjuntos de dados** para representar os dados de entrada e saídos da operação de cópia. O exemplo mencionado no último passo, vai criar um conjunto de dados para especificar a tabela na base de dados do Oracle que contém os dados de entrada. Além disso, crie outro conjunto de dados para especificar o contentor de BLOBs e a pasta que contém os dados copiados da base de dados Oracle. Para as propriedades do conjunto de dados que são específicas para Oracle, consulte [propriedades do conjunto de dados](#dataset-properties) secção.
4. Criar uma **pipeline** com uma atividade de cópia que usa um conjunto de dados como entrada e um conjunto de dados como uma saída. No exemplo mencionado anteriormente, vai utilizar OracleSource como uma origem e BlobSink como sink para a atividade de cópia. Da mesma forma, se estiver a copiar de armazenamento de Blobs do Azure para a base de dados Oracle, utilize BlobSource e OracleSink a atividade de cópia. Para propriedades de atividade de cópia que são específicas para a base de dados Oracle, consulte [propriedades da atividade copy](#copy-activity-properties) secção. Para obter detalhes sobre como usar um arquivo de dados como uma origem ou sink, clique na ligação na secção anterior para seu armazenamento de dados. 

Quando utiliza o assistente, definições de JSON para estas entidades do Data Factory (serviços ligados, conjuntos de dados e pipeline) são criadas automaticamente para. Ao utilizar ferramentas/APIs (exceto a .NET API), define essas entidades do Data Factory com o formato JSON.  Para exemplos com definições de JSON para entidades do Data Factory que são utilizadas para copiar dados de/para uma base de dados do Oracle no local, consulte [exemplos JSON](#json-examples-for-copying-data-to-and-from-oracle-database) seção deste artigo.

As secções seguintes fornecem detalhes sobre as propriedades JSON, que são utilizadas para definir entidades do Data Factory:

## <a name="linked-service-properties"></a>Propriedades do serviço ligado
A tabela seguinte fornece uma descrição para elementos JSON específicos ao serviço ligado do Oracle.

| Propriedade | Descrição | Necessário |
| --- | --- | --- |
| tipo |A propriedade de tipo deve ser definida como: **OnPremisesOracle** |Sim |
| driverType | Especifique qual driver para utilizar para copiar dados de/para a base de dados Oracle. Valores permitidos são **Microsoft** ou **ODP** (predefinição). Ver [suportada a instalação e a versão](#supported-versions-and-installation) seção sobre detalhes do controlador. | Não |
| connectionString | Especifica as informações necessárias para ligar à instância de base de dados Oracle para a propriedade connectionString. | Sim |
| gatewayName | Nome do gateway que é utilizado para ligar ao servidor Oracle no local |Sim |

**Exemplo: utilizar o controlador Microsoft:**

>[!TIP]
>Se tiver atingido o ditado de erro "ORA 01025: parâmetro UPI fora do intervalo" e o Oracle é de versões 8i, adicionar `WireProtocolMode=1` para sua cadeia de ligação e tente novamente.

```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString":"Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

**Exemplo: com o controlador ODP**

Consulte a [este site](https://www.connectionstrings.com/oracle-data-provider-for-net-odp-net/) para os formatos permitidos.

```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "connectionString": "Data Source=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=<hostname>)(PORT=<port number>))(CONNECT_DATA=(SERVICE_NAME=<SID>)));
User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

## <a name="dataset-properties"></a>Propriedades do conjunto de dados
Para obter uma lista completa das secções e propriedades disponíveis para definir conjuntos de dados, consulte a [criar conjuntos de dados](data-factory-create-datasets.md) artigo. Seções, como a estrutura, disponibilidade e a política de um conjunto de dados JSON são semelhantes para todos os tipos de conjunto de dados (Oracle, BLOBs do Azure, tabela do Azure, etc.).

A secção typeProperties é diferente para cada tipo de conjunto de dados e fornece informações sobre a localização dos dados no arquivo de dados. A secção typeProperties do conjunto de dados do tipo OracleTable tem as seguintes propriedades:

| Propriedade | Descrição | Necessário |
| --- | --- | --- |
| tableName |Nome da tabela na base de dados Oracle que o serviço ligado refere-se a. |Não (se **oracleReaderQuery** dos **OracleSource** for especificado) |

## <a name="copy-activity-properties"></a>Propriedades da atividade Copy
Para obter uma lista completa das secções e propriedades disponíveis para a definição de atividades, consulte a [criar Pipelines](data-factory-create-pipelines.md) artigo. Propriedades, tais como o nome, descrição, entrada e saída de tabelas e a política estão disponíveis para todos os tipos de atividades.

> [!NOTE]
> A atividade de cópia precisar apenas uma entrada e saída de apenas um.

Ao passo que, propriedades disponíveis na secção typeProperties da atividade variar de acordo com cada tipo de atividade. Para a atividade de cópia, elas variam consoante os tipos de origens e sinks.

### <a name="oraclesource"></a>OracleSource
Na atividade de cópia, quando a origem é do tipo **OracleSource** as seguintes propriedades estão disponíveis no **typeProperties** secção:

| Propriedade | Descrição | Valores permitidos | Necessário |
| --- | --- | --- | --- |
| oracleReaderQuery |Utilize a consulta personalizada para ler dados. |Cadeia de consulta SQL. Por exemplo: selecionar * de MyTable <br/><br/>Se não for especificado, a instrução SQL que é executada: selecionar * de MyTable |Não (se **tableName** dos **conjunto de dados** for especificado) |

### <a name="oraclesink"></a>OracleSink
**OracleSink** suporta as seguintes propriedades:

| Propriedade | Descrição | Valores permitidos | Necessário |
| --- | --- | --- | --- |
| writeBatchTimeout |Tempo para a operação de inserção de lote seja concluída antes de atingir o tempo limite de espera. |período de tempo<br/><br/> Exemplo: 30: 00:00 (30 minutos). |Não |
| writeBatchSize |Insere dados na tabela SQL quando o tamanho do buffer atinge writeBatchSize. |Número inteiro (número de linhas) |Não (predefinição: 100) |
| sqlWriterCleanupScript |Especifica uma consulta para a atividade de cópia executar de forma a que os dados de um setor específico é limpo. |Uma instrução de consulta. |Não |
| sliceIdentifierColumnName |Especifique o nome da coluna para a atividade de cópia preencher com o identificador de setor gerado automaticamente, o que é utilizado para limpar os dados de um setor específico quando voltar a executar. |Nome da coluna de uma coluna com o tipo de dados de binary(32). |Não |

## <a name="json-examples-for-copying-data-to-and-from-oracle-database"></a>Exemplos JSON para copiar dados para e da base de dados Oracle
O exemplo seguinte fornece definições de JSON de exemplo que pode utilizar para criar um pipeline com [portal do Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) ou [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) ou [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Eles mostram como copiar dados de/para uma base de dados do Oracle em armazenamento de Blobs do Azure. No entanto, os dados podem ser copiados para qualquer um dos sinks indicados [aqui](data-factory-data-movement-activities.md#supported-data-stores-and-formats) usando a atividade de cópia no Azure Data Factory.   

## <a name="example-copy-data-from-oracle-to-azure-blob"></a>Exemplo: Copiar dados do Oracle para BLOBs do Azure

O exemplo possui as seguintes entidades do data factory:

1. Um serviço ligado do tipo [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).
2. Um serviço ligado do tipo [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Entrada [conjunto de dados](data-factory-create-datasets.md) do tipo [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).
4. Uma saída [conjunto de dados](data-factory-create-datasets.md) do tipo [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. R [pipeline](data-factory-create-pipelines.md) com a atividade de cópia que utiliza [OracleSource](data-factory-onprem-oracle-connector.md#copy-activity-properties) como origem e [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) como sink.

O exemplo copia dados de uma tabela numa base de dados Oracle no local para um blob de hora a hora. Para obter mais informações sobre várias propriedades usada nesse exemplo, consulte a documentação nas seções a seguir os exemplos.

**Serviço ligado do Oracle:**

```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString":"Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

**Serviço ligado do armazenamento de Blobs do Azure:**

```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
    }
}
```

**Conjunto de dados entrado de Oracle:**

O exemplo assume que criou uma tabela "MyTable" Oracle e contém uma coluna chamada "timestampcolumn" para dados de séries de tempo.

A definição "externo": "true" informa o serviço Data Factory que o conjunto de dados é externo à fábrica de dados e não é produzido por uma atividade na fábrica de dados.

```json
{
    "name": "OracleInput",
    "properties": {
        "type": "OracleTable",
        "linkedServiceName": "OnPremisesOracleLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "offset": "01:00:00",
            "interval": "1",
            "anchorDateTime": "2014-02-27T12:00:00",
            "frequency": "Hour"
        },
        "policy": {     
            "externalData": {        
                "retryInterval": "00:01:00",    
                "retryTimeout": "00:10:00",       
                "maximumRetry": 3       
            }     
        }
    }
}
```

**Conjunto de dados de Blobs do Azure:**

Os dados são escritos para um blob novo a cada hora (frequência: hora, intervalo: 1). O nome de ficheiro e caminho de pasta para o blob dinamicamente são avaliados com base na hora de início do setor que está a ser processado. O caminho da pasta utiliza ano, mês, dia e partes de horas da hora de início.

```json
{
    "name": "AzureBlobOutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "partitionedBy": [
                {
                    "name": "Year",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "yyyy"
                    }
                },
                {
                    "name": "Month",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "MM"
                    }
                },
                {
                    "name": "Day",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "dd"
                    }
                },
                {
                    "name": "Hour",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "HH"
                    }
                }
            ],
            "format": {
                "type": "TextFormat",
                "columnDelimiter": "\t",
                "rowDelimiter": "\n"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**Pipeline com atividade de cópia:**

O pipeline contém uma atividade de cópia que está configurado para utilizar os conjuntos de dados de entrada e saídos e é agendada para ser executada uma vez por hora. No pipeline de definição de JSON, o **origem** tipo está definido como **OracleSource** e **sink** tipo está definido como **BlobSink**.  A consulta SQL especificada com **oracleReaderQuery** propriedade seleciona os dados na hora anterior para copiar.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
            {
                "name": "OracletoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                    {
                        "name": " OracleInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "OracleSource",
                        "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "OldestFirst",
                    "retry": 0,
                    "timeout": "01:00:00"
                }
            }
        ]
    }
}
```

## <a name="example-copy-data-from-azure-blob-to-oracle"></a>Exemplo: Copiar dados de Blobs do Azure para Oracle
Este exemplo mostra como copiar dados de um armazenamento de Blobs do Azure para uma base de dados do Oracle no local. No entanto, os dados podem ser copiados **diretamente** de qualquer uma das origens indicadas [aqui](data-factory-data-movement-activities.md#supported-data-stores-and-formats) usando a atividade de cópia no Azure Data Factory.  

O exemplo possui as seguintes entidades do data factory:

1. Um serviço ligado do tipo [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).
2. Um serviço ligado do tipo [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Entrada [conjunto de dados](data-factory-create-datasets.md) do tipo [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. Uma saída [conjunto de dados](data-factory-create-datasets.md) do tipo [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).
5. R [pipeline](data-factory-create-pipelines.md) com a atividade de cópia que utiliza [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) como origem [OracleSink](data-factory-onprem-oracle-connector.md#copy-activity-properties) como sink.

O exemplo copia dados de um blob para uma tabela numa base de dados Oracle no local a cada hora. Para obter mais informações sobre várias propriedades usada nesse exemplo, consulte a documentação nas seções a seguir os exemplos.

**Serviço ligado do Oracle:**
```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "connectionString": "Data Source=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=<hostname>)(PORT=<port number>))(CONNECT_DATA=(SERVICE_NAME=<SID>)));
            User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

**Serviço ligado do armazenamento de Blobs do Azure:**
```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
    }
}
```

**Conjunto de dados de entrada BLOBs do Azure**

Dados são captados um blob novo a cada hora (frequência: hora, intervalo: 1). O nome de ficheiro e caminho de pasta para o blob dinamicamente são avaliados com base na hora de início do setor que está a ser processado. O caminho da pasta utiliza ano, mês e parte do dia da hora de início e nome de ficheiro utiliza a parte de hora a hora de início. "externo": "true" definição informa o serviço Data Factory que esta tabela é externa à fábrica de dados e não é produzida por uma atividade na fábrica de dados.

```json
{
    "name": "AzureBlobInput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
            "partitionedBy": [
                {
                    "name": "Year",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "yyyy"
                    }
                },
                {
                    "name": "Month",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "MM"
                    }
                },
                {
                    "name": "Day",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "dd"
                    }
                }
            ],
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            }
        },
        "external": true,
        "availability": {
            "frequency": "Day",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

**Conjunto de dados do Oracle saída:**

O exemplo parte do princípio de que criou uma tabela "MyTable" Oracle. Crie a tabela no Oracle com o mesmo número de colunas, como espera que o ficheiro CSV de BLOBs para conter. Novas linhas são adicionadas à tabela de cada hora.

```json
{
    "name": "OracleOutput",
    "properties": {
        "type": "OracleTable",
        "linkedServiceName": "OnPremisesOracleLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "availability": {
            "frequency": "Day",
            "interval": "1"
        }
    }
}
```

**Pipeline com atividade de cópia:**

O pipeline contém uma atividade de cópia que está configurado para utilizar os conjuntos de dados de entrada e saídos e é agendada para ser executada a cada hora. No pipeline de definição de JSON, o **origem** tipo está definido como **BlobSource** e o **sink** tipo está definido como **OracleSink**.  

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-05T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
            {
                "name": "AzureBlobtoOracle",
                "description": "Copy Activity",
                "type": "Copy",
                "inputs": [
                    {
                        "name": "AzureBlobInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "OracleOutput"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "OracleSink"
                    }
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "OldestFirst",
                    "retry": 0,
                    "timeout": "01:00:00"
                }
            }
        ]
    }
}
```


## <a name="troubleshooting-tips"></a>Sugestões de resolução de problemas
### <a name="problem-1-net-framework-data-provider"></a>Problema 1: Fornecedor de dados .NET Framework

As seguintes **mensagem de erro**:

    Copy activity met invalid parameters: 'UnknownParameterName', Detailed message: Unable to find the requested .Net Framework Data Provider. It may not be installed”.  

**Causas possíveis:**

1. O fornecedor de dados do .NET Framework para Oracle não foi instalado.
2. O .NET Framework Data Provider Pro Oracle foi instalado para o .NET Framework 2.0 e não foi encontrado nas pastas do .NET Framework 4.0.

**Resolução/solução:**

1. Se ainda não instalou o fornecedor de .NET para o Oracle, [instalá-lo](http://www.oracle.com/technetwork/topics/dotnet/downloads/) e repita o cenário.
2. Se receber a mensagem de erro, mesmo após a instalação do fornecedor, siga os passos abaixo:
   1. Abra a configuração de máquina do .NET 2.0 a partir da pasta: <system disk>: \Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG\machine.config.
   2. Procure **Oracle Data Provider para .NET**, e deve ser capaz de localizar uma entrada, conforme mostrado no exemplo a seguir sob **System. data** -> **DbProviderFactories**: "<add name="Oracle Data Provider for .NET" invariant="Oracle.DataAccess.Client" description="Fornecedor de dados do oracle para .NET" type="Oracle.DataAccess.Client.OracleClientFactory, Oracle.DataAccess, Version=2.112.3.0, Culture=neutral, PublicKeyToken=89b483f429c47342" />”
3. Copie esta entrada para o arquivo Machine config na seguinte pasta v4.0: <system disk>: \Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config e, altere a versão para 4.xxx.x.x.
4. Instalar o "< caminho de instalação ODP.NET > \11.2.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll" para a cache de global assembly (GAC) executando `gacutil /i [provider path]`. # # sugestões de resolução de problemas

### <a name="problem-2-datetime-formatting"></a>Problema 2: formatação de datetime

As seguintes **mensagem de erro**:

    Message=Operation failed in Oracle Database with the following error: 'ORA-01861: literal does not match format string'.,Source=,''Type=Oracle.DataAccess.Client.OracleException,Message=ORA-01861: literal does not match format string,Source=Oracle Data Provider for .NET,'.

**Resolução/solução:**

Poderá ter de ajustar a cadeia de consulta em sua atividade de cópia com base em como as datas são configuradas na sua base de dados Oracle, conforme mostrado no exemplo a seguir (usando a função de to_date):

    "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= to_date(\\'{0:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\')  AND timestampcolumn < to_date(\\'{1:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\') ', WindowStart, WindowEnd)"


## <a name="type-mapping-for-oracle"></a>Mapeamento de tipo para Oracle
Conforme mencionado na [atividades de movimento de dados](data-factory-data-movement-activities.md) artigo atividade de cópia executa conversões de tipos automáticas de tipos de origem para o sink de tipos com a seguinte abordagem de passo 2:

1. Converter entre tipos de origem nativas para o tipo de .NET
2. Converter de tipo de .NET para o tipo de sink nativo

Ao mover dados do Oracle, são utilizados os seguintes mapeamentos de tipo de dados Oracle para o tipo .NET e vice-versa.

| Tipo de dados Oracle | Tipo de dados do .NET framework |
| --- | --- |
| BFILE |Byte[] |
| BLOB |Byte[]<br/>(suportado apenas no Oracle 10g e quando superior com o controlador Microsoft) |
| CHAR |Cadeia |
| CLOB |Cadeia |
| DATA |DateTime |
| NÚMERO DE VÍRGULA FLUTUANTE |Número decimal, cadeia de caracteres (se precisão > 28) |
| NÚMERO INTEIRO |Número decimal, cadeia de caracteres (se precisão > 28) |
| INTERVALO DE ANO PARA MÊS |Int32 |
| DIA DO INTERVALO COMO SEGUNDO |Período de tempo |
| LONGA |Cadeia |
| HÁ MUITO TEMPO NÃO PROCESSADOS |Byte[] |
| NCHAR |Cadeia |
| NCLOB |Cadeia |
| NÚMERO |Número decimal, cadeia de caracteres (se precisão > 28) |
| NVARCHAR2 |Cadeia |
| NÃO PROCESSADOS |Byte[] |
| ROWID |Cadeia |
| TIMESTAMP |DateTime |
| TIMESTAMP COM O FUSO HORÁRIO LOCAL |DateTime |
| TIMESTAMP COM O FUSO HORÁRIO |DateTime |
| NÚMERO INTEIRO NÃO ASSINADO |Number |
| VARCHAR2 |Cadeia |
| XML |Cadeia |

> [!NOTE]
> Tipo de dados **intervalo ano para mês** e **intervalo dia TO segundo** não são suportadas ao utilizar o controlador Microsoft.

## <a name="map-source-to-sink-columns"></a>Origem do mapa para colunas de sink
Para saber mais sobre as colunas de mapeamento no conjunto de dados de origem para colunas no conjunto de dados de sink, veja [mapeamento de colunas do conjunto de dados no Azure Data Factory](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>Leitura passível de repetição de fontes relacionais
Quando armazena a cópia de dados de dados relacionais, tenha a capacidade de repetição em mente para evitar resultados indesejados. No Azure Data Factory, pode voltar a executar um setor manualmente. Também pode configurar a política de repetição para um conjunto de dados para que um setor será novamente executado quando ocorre uma falha. Quando um setor será novamente executado de qualquer forma, terá de certificar-se de que os mesmos dados é de leitura não questão número de vezes que um setor é executado. Ver [Repeatable ler a partir de origens relacionais](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Desempenho e Otimização
Ver [desempenho de atividade de cópia e guia de ajuste](data-factory-copy-activity-performance.md) para saber mais sobre os fatores chave a que um impacto no desempenho de movimento de dados (atividade de cópia) no Azure Data Factory e diversas maneiras para otimizá-lo.
