---
title: Copiar dados de ramo de registo utilizando o Azure Data Factory | Microsoft Docs
description: "Saiba como copiar dados de ramo de registo para os arquivos de dados dependente suportados através da utilização de uma atividade de cópia no pipeline Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/30/2017
ms.author: jingwang
ms.openlocfilehash: 8842adcc00a1230f252411d64c22d497faeec5b2
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2018
---
# <a name="copy-data-from-hive-using-azure-data-factory"></a>Copiar dados de ramo de registo utilizando o Azure Data Factory 

Este artigo descreve como utilizar a atividade de cópia no Azure Data Factory para copiar dados do Hive. Baseia-se no [copiar descrição geral da atividade](copy-activity-overview.md) artigo que apresenta uma descrição geral da atividade de cópia.

> [!NOTE]
> Este artigo aplica-se à versão 2 do Data Factory, que está atualmente em pré-visualização. Se estiver a utilizar a versão 1 do serviço do Data Factory, o que é geralmente disponível (DG), consulte [atividade de cópia no V1](v1/data-factory-data-movement-activities.md).

## <a name="supported-capabilities"></a>Capacidades suportadas

Pode copiar dados de ramo de registo para qualquer arquivo de dados suportados sink. Para obter uma lista dos arquivos de dados que são suportados como origens/sinks pela atividade de cópia, consulte o [arquivos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats) tabela.

O Azure Data Factory fornece um controlador incorporado para ativar a conetividade, pelo que não precisa de instalar manualmente o controlador de utilizar este conector.

## <a name="getting-started"></a>Introdução

[!INCLUDE [data-factory-v2-connector-get-started-2](../../includes/data-factory-v2-connector-get-started-2.md)]

As secções seguintes fornecem detalhes sobre as propriedades que são utilizados para definir entidades do Data Factory específicas para o conector do ramo de registo.

## <a name="linked-service-properties"></a>Propriedades de serviço ligado

As seguintes propriedades são suportadas para o serviço ligado do ramo de registo:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo tem de ser definida: **ramo de registo** | Sim |
| anfitrião | Nome anfitrião ou endereço IP do servidor do Hive, separado por ';' para vários anfitriões (apenas quando serviceDiscoveryMode está activada).  | Sim |
| porta | A porta TCP que o servidor de ramo de registo utiliza para escutar ligações de cliente.  | Não |
| serverType | O tipo de servidor de ramo de registo. <br/>Valores permitidos são: **HiveServer1**, **HiveServer2**, **HiveThriftServer** | Não |
| thriftTransportProtocol | O protocolo de transporte para utilizar na camada de Thrift. <br/>Valores permitidos são: **binário**, **SASL**, * * HTTP * * | Não |
| authenticationType | O método de autenticação utilizado para aceder ao servidor de ramo de registo. <br/>Valores permitidos são: **anónimo**, **Username**, **UsernameAndPassword**, **WindowsAzureHDInsightService** | Sim |
| serviceDiscoveryMode | Verdadeiro para indicar a utilizar o serviço de ZooKeeper false não.  | Não |
| zooKeeperNameSpace | O espaço de nomes no ZooKeeper em que Server 2 ramo de registo são adicionados nós.  | Não |
| useNativeQuery | Especifica se o controlador utiliza consultas de HiveQL nativas ou converte-os para um formato equivalente na HiveQL.  | Não |
| o nome de utilizador | O nome de utilizador que utilizar para aceder ao servidor do Hive.  | Não |
| palavra-passe | A palavra-passe correspondente ao nome de utilizador fornecida no campo do nome de utilizador pode escolher marcar este campo como um SecureString para armazenar com segurança no ADF, ou armazenar a palavra-passe no Cofre de chaves do Azure e permitem a extração de atividade de cópia a partir daí, quando efetuar a cópia de dados - lea Rn mais [armazenar credenciais no Cofre de chaves](store-credentials-in-key-vault.md). | Não |
| httpPath | O URL parcial correspondente para o servidor de ramo de registo.  | Não |
| enableSsl | Especifica se as ligações ao servidor são encriptadas com SSL. O valor predefinido é falso.  | Não |
| trustedCertPath | O caminho completo do ficheiro. pem que contém os certificados de AC fidedigna para verificar o servidor ao ligar através de SSL. Esta propriedade só pode ser definida quando a utilizar SSL no IR. personalizada alojada O valor predefinido é o ficheiro de cacerts.pem instalado com o IR.  | Não |
| useSystemTrustStore | Especifica se deve utilizar um certificado de AC a partir do arquivo de confiança de sistema ou a partir de um ficheiro PEM especificado. O valor predefinido é falso.  | Não |
| allowHostNameCNMismatch | Especifica se requer um nome de certificado SSL emitidos pela AC para corresponder ao nome do anfitrião do servidor ao ligar através de SSL. O valor predefinido é falso.  | Não |
| allowSelfSignedServerCert | Especifica se pretende permitir que os certificados autoassinados do servidor. O valor predefinido é falso.  | Não |
| connectVia | O [integração Runtime](concepts-integration-runtime.md) para ser utilizado para ligar ao arquivo de dados. Pode utilizar o Runtime de integração Self-hosted ou Runtime de integração do Azure (se o arquivo de dados acessível publicamente). Se não for especificado, utiliza a predefinição de Runtime de integração do Azure. |Não |

**Exemplo:**

```json
{
    "name": "HiveLinkedService",
    "properties": {
        "type": "Hive",
        "typeProperties": {
            "host" : "<cluster>.azurehdinsight.net",
            "port" : "<port>",
            "authenticationType" : "WindowsAzureHDInsightService",
            "username" : "<username>",
            "password": {
                 "type": "SecureString",
                 "value": "<password>"
            },
            "httpPath" : "gateway/sandbox/spark"
        }
    }
}
```

## <a name="dataset-properties"></a>Propriedades do conjunto de dados

Para uma lista completa das secções e propriedades disponíveis para definir os conjuntos de dados, consulte o [conjuntos de dados](concepts-datasets-linked-services.md) artigo. Esta secção fornece uma lista de propriedades suportadas por conjunto de dados do Hive.

Para copiar dados de ramo de registo, defina a propriedade de tipo do conjunto de dados para **HiveObject**. Não há nenhuma propriedade de tipo específicas adicional neste tipo de conjunto de dados.

**Exemplo**

```json
{
    "name": "HiveDataset",
    "properties": {
        "type": "HiveObject",
        "linkedServiceName": {
            "referenceName": "<Hive linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propriedades da atividade de cópia

Para uma lista completa das secções e propriedades disponíveis para definir as atividades, consulte o [Pipelines](concepts-pipelines-activities.md) artigo. Esta secção fornece uma lista de propriedades suportado pela origem de ramo de registo.

### <a name="hivesource-as-source"></a>HiveSource como origem

Para copiar dados de ramo de registo, defina o tipo de origem na atividade de cópia para **HiveSource**. As seguintes propriedades são suportadas na atividade de cópia **origem** secção:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo da origem de atividade de cópia tem de ser definida: **HiveSource** | Sim |
| consulta | Utilize a consulta SQL personalizada para ler os dados. Por exemplo: `"SELECT * FROM MyTable"`. | Sim |

**Exemplo:**

```json
"activities":[
    {
        "name": "CopyFromHive",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Hive input dataset name>",
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
                "type": "HiveSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="next-steps"></a>Passos Seguintes
Para obter uma lista dos arquivos de dados suportados como origens e sinks pela atividade de cópia no Azure Data Factory, consulte [arquivos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats).
