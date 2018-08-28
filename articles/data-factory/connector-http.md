---
title: Copiar dados de origem HTTP com o Azure Data Factory | Documentos da Microsoft
description: Saiba como copiar dados de uma origem HTTP na cloud ou no local para os arquivos de dados de sink suportado com uma atividade de cópia num pipeline do Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/24/2018
ms.author: jingwang
ms.openlocfilehash: 5afb2fccd5c7b8ca306079941837d854c0b21349
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43091722"
---
# <a name="copy-data-from-http-endpoint-using-azure-data-factory"></a>Copiar dados a partir do ponto final HTTP com o Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versão 1](v1/data-factory-http-connector.md)
> * [Versão atual](connector-http.md)

Este artigo descreve como utilizar a atividade de cópia no Azure Data Factory para copiar dados de um ponto final HTTP. Ele se baseia no [copiar descrição geral da atividade](copy-activity-overview.md) artigo apresenta uma visão geral da atividade de cópia.

## <a name="supported-capabilities"></a>Capacidades suportadas

Pode copiar dados de origem HTTP para qualquer arquivo de dados de sink suportados. Para obter uma lista dos arquivos de dados que são suportados como origens/sinks a atividade de cópia, consulte a [arquivos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats) tabela.

Especificamente, este conector HTTP suporta:

- Recuperar dados do ponto final HTTP/s através de HTTP **Obtenha** ou **POST** método.
- Obter dados com as seguintes autenticações: **anónimo**, **básica**, **Digest**, **Windows**, e  **ClientCertificate**.
- Copiar a resposta HTTP como-é ou analisá-lo com o [formatos de arquivo e codecs de compressão suportados](supported-file-formats-and-compression-codecs.md).

A diferença entre este conector e a [conector de tabela de Web](connector-web-table.md) é que a última opção é utilizado para extrair o conteúdo da tabela de página da web HTML.

>[!TIP]
>Para testar o pedido HTTP para dados de recuperação antes de configurar o conector HTTP no ADF, pode aprender com a especificação de API no cabeçalho e requisitos de corpo e usar ferramentas como o navegador da web ou o Postman para validar.

## <a name="getting-started"></a>Introdução

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

As secções seguintes fornecem detalhes sobre as propriedades que são utilizadas para definir entidades do Data Factory específicas para o conector HTTP.

## <a name="linked-service-properties"></a>Propriedades do serviço ligado

As seguintes propriedades são suportadas para o serviço ligado de HTTP:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo deve ser definida como: **HttpServer**. | Sim |
| url | URL de base para o servidor Web | Sim |
| enableServerCertificateValidation | Especifique se pretende ativar a validação de certificado do servidor SSL ao ligar ao ponto final de HTTP. Quando o servidor HTTPS está a utilizar o certificado autoassinado, defina esta opção como falso. | Não, a predefinição é verdadeiro |
| authenticationType | Especifica o tipo de autenticação. Valores permitidos são: **anónimo**, **básica**, **Digest**, **Windows**, **ClientCertificate**. <br><br> Consulte a secções abaixo desta tabela em mais propriedades e exemplos JSON para esses tipos de autenticação, respetivamente. | Sim |
| connectVia | O [Integration Runtime](concepts-integration-runtime.md) a ser utilizado para ligar ao arquivo de dados. Pode utilizar o Runtime de integração do Azure ou o Runtime de integração autoalojado (se o seu armazenamento de dados está localizado numa rede privada). Se não for especificado, ele usa o padrão do Runtime de integração do Azure. |Não |

### <a name="using-basic-digest-or-windows-authentication"></a>Utilizar a autenticação básica, Digest ou do Windows

Defina a propriedade de "authenticationType" como **básica**, **Digest**, ou **Windows**e especifique as seguintes propriedades, juntamente com as propriedades genéricas descritas no anterior secção:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| userName | Nome de utilizador para aceder ao ponto final HTTP. | Sim |
| palavra-passe | Palavra-passe para o utilizador (nome de utilizador). Marcar esse campo como uma SecureString armazena de forma segura na fábrica de dados, ou [referenciar um segredo armazenado no Azure Key Vault](store-credentials-in-key-vault.md). | Sim |

**Exemplo**

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "HttpServer",
        "typeProperties": {
            "authenticationType": "Basic",
            "url" : "<HTTP endpoint>",
            "userName": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="using-clientcertificate-authentication"></a>Utilizar a autenticação de ClientCertificate

Para utilizar a autenticação de ClientCertificate, defina a propriedade de "authenticationType" como **ClientCertificate**e especifique as seguintes propriedades juntamente com as propriedades genéricas, descritas na secção anterior:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| embeddedCertData | Dados do certificado codificado em Base64. | Especifique a `embeddedCertData` ou `certThumbprint`. |
| certThumbprint | O thumbprint do certificado que está instalado no arquivo de certificados da sua máquina de Integration Runtime autoalojado. Aplica-se apenas quando o tipo de hospedagem interna do Integration Runtime é especificado no connectVia. | Especifique a `embeddedCertData` ou `certThumbprint`. |
| palavra-passe | Palavra-passe associado ao certificado. Marcar esse campo como uma SecureString armazena de forma segura na fábrica de dados, ou [referenciar um segredo armazenado no Azure Key Vault](store-credentials-in-key-vault.md). | Não |

Se usar "certThumbprint" para a autenticação e o certificado está instalado no arquivo pessoal do computador local, tem de conceder a permissão de leitura para o Runtime de integração autoalojado:

1. Inicie o Console de gerenciamento Microsoft (MMC). Adicionar a **certificados** snap-in direcionada para o **computador Local**.
2. Expanda **certificados**, **pessoais**e clique em **certificados**.
3. O certificado do arquivo pessoal com o botão direito e selecione **todas as tarefas** -> **gerir chaves privadas...**
3. Sobre o **segurança** separador, adicione a conta de utilizador sob a qual serviço de anfitrião do Integration Runtime (DIAHostService) está em execução com o acesso de leitura para o certificado.

**Exemplo 1: utilizar certThumbprint**

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "HttpServer",
        "typeProperties": {
            "authenticationType": "ClientCertificate",
            "url": "<HTTP endpoint>",
            "certThumbprint": "<thumbprint of certificate>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Exemplo 2: utilizar embeddedCertData**

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "HttpServer",
        "typeProperties": {
            "authenticationType": "ClientCertificate",
            "url": "<HTTP endpoint>",
            "embeddedCertData": "<base64 encoded cert data>",
            "password": {
                "type": "SecureString",
                "value": "password of cert"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Propriedades do conjunto de dados

Para obter uma lista completa das secções e propriedades disponíveis para definir conjuntos de dados, consulte o artigo de conjuntos de dados. Esta seção fornece uma lista de propriedades suportadas pelo conjunto de dados HTTP.

Para copiar dados de HTTP, defina a propriedade de tipo de conjunto de dados para **HttpFile**. São suportadas as seguintes propriedades:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo do conjunto de dados tem de ser definida como: **HttpFile** | Sim |
| relativeUrl | Um URL relativo ao recurso que contém os dados. Quando esta propriedade não for especificada, é utilizado apenas o URL especificado na definição do serviço ligado. | Não |
| requestMethod | Método HTTP.<br/>Valores permitidos são **Obtenha** (predefinição) ou **Post**. | Não |
| additionalHeaders | Cabeçalhos de pedido HTTP adicionais. | Não |
| RequestBody | Corpo de pedido HTTP. | Não |
| Formato | Se quiser **recuperar dados do ponto de extremidade HTTP como-é** sem analisando-lo e copiar para um armazenamento baseado em arquivo, ignore a secção de formato em ambas as definições do conjunto de dados de entrada e saída.<br/><br/>Se pretender analisar o conteúdo de resposta HTTP durante a cópia de segurança, os seguintes tipos de formato de ficheiro são suportados: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Definir o **tipo** propriedade em formato para um dos seguintes valores. Para obter mais informações, consulte [formato Json](supported-file-formats-and-compression-codecs.md#json-format), [formato de texto](supported-file-formats-and-compression-codecs.md#text-format), [formato Avro](supported-file-formats-and-compression-codecs.md#avro-format), [formato Orc](supported-file-formats-and-compression-codecs.md#orc-format), e [formato Parquet](supported-file-formats-and-compression-codecs.md#parquet-format) secções. |Não |
| Compressão | Especifica o tipo e o nível de compressão dos dados. Para obter mais informações, consulte [formatos de arquivo e codecs de compressão suportados](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Tipos suportados são: **GZip**, **Deflate**, **BZip2**, e **ZipDeflate**.<br/>Níveis suportados são: **Optimal** e **Fastest**. |Não |

>[!NOTE]
>O tamanho de payload de pedido HTTP suportado é de cerca de 500KB. Se o tamanho da carga que pretende passar para o ponto final de web é maior do que isso, considere a colocar em lotes em segmentos mais pequenos.

**Exemplo 1: utilizar o método Get (predefinição)**

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "HttpFile",
        "linkedServiceName": {
            "referenceName": "<HTTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "relativeUrl": "<relative url>",
            "additionalHeaders": "Connection: keep-alive\nUser-Agent: Mozilla/5.0\n"
        }
    }
}
```

**Exemplo 2: utilizar o método Post**

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "HttpFile",
        "linkedServiceName": {
            "referenceName": "<HTTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "relativeUrl": "<relative url>",
            "requestMethod": "Post",
            "requestBody": "<body for POST HTTP request>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propriedades da atividade Copy

Para obter uma lista completa das secções e propriedades disponíveis para a definição de atividades, consulte a [Pipelines](concepts-pipelines-activities.md) artigo. Esta seção fornece uma lista de propriedades suportadas pela origem HTTP.

### <a name="http-as-source"></a>HTTP como origem

Para copiar dados de HTTP, definir o tipo de origem na atividade de cópia para **HttpSource**. As seguintes propriedades são suportadas na atividade de cópia **origem** secção:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo de origem de atividade de cópia tem de ser definida: **HttpSource** | Sim |
| httpRequestTimeout | O tempo limite (intervalo de tempo) para o pedido HTTP para obter uma resposta. É o tempo limite para obter uma resposta, não o tempo limite para ler os dados de resposta.<br/> Valor predefinido é: 00:01:40  | Não |

**Exemplo:**

```json
"activities":[
    {
        "name": "CopyFromHTTP",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<HTTP input dataset name>",
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
                "type": "HttpSource",
                "httpRequestTimeout": "00:01:00"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```


## <a name="next-steps"></a>Passos Seguintes
Para obter uma lista dos arquivos de dados suportados como origens e sinks, a atividade de cópia no Azure Data Factory, veja [arquivos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats).
