---
title: Consulta de série de tempo | Documentos da Microsoft
description: Axure Time Series Insights (pré-visualização) consulta de série de tempo
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 11/27/2018
ms.openlocfilehash: f5370d354833e9507d0794d7bc407a7ea2294e1a
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52856471"
---
# <a name="time-series-query"></a>Série de tempo de consulta

**Consulta de série de tempo** (TSQ), facilita a computação e obter dados de séries de tempo armazenados no Time Series Insights (TSI) em escala. TSQ tira partido das definições de computacionais para transformar e recuperar dados de armazenamento. Isso é feito da **modelos de série de tempo** (TSM) ou através de definições de variável inline.

TSQ obtém dados de duas formas diferentes. TSQ pode recuperar dados conforme é registada no fornecedor de origem, ou pode reduzir os dados, ou pode reconstruir os sinais de aproveitar o método especificado para permitir aos clientes realizar operações para transformar, combinar e efetuar cálculos nos dados de séries de tempo.

Dados de informações de séries de tempo podem ser acedidos através da atualização TSI nativa Explorer ou a API de superfície pública. TSQ também oferece uma linguagem de expressão para que pode criar próprio consultas mais avançadas do TSI. Seguem-se as nossas metas com TSQ:

![Objetivo][1]

## <a name="core-apis"></a>APIs de núcleo

Seguem-se as principais APIs suportamos.

![tsq][2]

### <a name="getmodelsettings-api"></a>getModelSettings API

A API de getModelSettings é utilizada para devolver o modelo criado automaticamente para a chave de partição de ambientes.

A API de getModelSettings precisa dos seguintes parâmetros:

* *nome*: O nome do modelo que pretende recuperar.
* *timeSeriesIdProperties*

  * *name*
  * *tipo*

* *defaultTypeID*

#### <a name="getmodelsettings-json-request-and-response-example"></a>exemplo de solicitação e resposta do JSON de getModelSettings

Recebe um pedido GET HTTP:

```plaintext
https://YOUR_ENVIROMENT.env.timeseries.azure.com/timeseries/modelSettings?api-version=API_VERSION
```

| Nome | Descrição | Exemplo |
| --- | --- | --- |
| YOUR_ENVIRONMENT  |  O nome do seu ambiente  | `environment123` |
| API_VERSION  |  A especificação de API | `2018-11-01-preview` |

Resposta:

```JSON
{
    "modelSettings": {
        "name": "DefaultModel",
        "timeSeriesIdProperties": [
            {
                "name": "someType1",
                "type": "String"
            }
        ],
        "defaultTypeId": "1be09af9-f089-4d6b-9f0b-48018b5f7393"
    }
}
```

### <a name="gettypes-api"></a>getTypes API

A API de getTypes devolve todos os tipos de séries de tempo e as respetivas variáveis associados para o modelo.

A API de getTypes precisa dos seguintes parâmetros:

* *typeId*: O identificador exclusivo para o tipo imutável.
* *timeSeriesId*: A **ID da série de tempo** é a chave exclusiva para os dados dentro do fluxo de eventos e o modelo. Esta chave é o que utiliza o TSI para particionar os dados.
* *Descrição*: a descrição para o tipo.
* *hierarchyIds*: uma matriz de hierarchyIds associados para o tipo.
* *instanceFields*:
  * *state*
  * *Cidade*

#### <a name="gettypes-json-request-and-response-example"></a>exemplo de solicitação e resposta do JSON de getTypes

Recebe um pedido de POST HTTP:

```plaintext
https://YOUR_ENVIROMENT.env.timeseries.azure.com/timeseries/types/$batch?api-version=API_VERSION
```

| Nome | Descrição | Exemplo |
| --- | --- | --- |
| YOUR_ENVIRONMENT  |  O nome do seu ambiente  | `environment123` |
| API_VERSION  |  A especificação de API | `2018-11-01-preview` |

Com os seguintes JSON corpo e variável atributos:

| Atributo | Obrigatório ou opcional |
| --- | --- |
| **tipo**  |  Necessário  |
| **filter**  |  Opcional |
| **valor**  | Nulo ou não foi especificado  |
| **interpolação**  |  Opcional |
| **Agregação**  |  Necessário |

```JSON
{
    "get": null,
    "put": [
        {
            "name": "SampleType",
            "description": "This is type 2",
            "variables": {
                "Avg Temperature": {
                    "kind": "numeric",
                    "filter": null,
                    "value": { "tsx": "$event.temperature.Double" },
                    "interpolation": "None",
                    "aggregation": {"tsx": "avg($value)"}
                },
                "Count Temperature": {
                    "kind": "aggregate",
                    "filter": null,
                    "value": null,
                    "interpolation": "None",
                    "aggregation": {"tsx": "count()"}
                },
                "Min Temperature": {
                    "kind": "aggregate",
                    "filter": null,
                    "value": null,
                    "interpolation": "None",
                    "aggregation": {"tsx": "min($event.temperature)"}
                },
            }
        }
    ]
}
```

Resposta:

```JSON
{
    "get": null,
    "put": [
        {
            "timeSeriesType": {
                "id": "fc4f526c-da6e-4b85-87f7-16f6cf9b69be",
                "name": "type2",
                "description": "This is type 2",
                "variables": {
                    "Avg Temperature": {
                        "kind": "numeric",
                        "filter": null,
                        "value": { "tsx": "$event.temperature.Double" },
                        "interpolation": "None",
                        "aggregation": {"tsx": "avg($value)"}
                    },
                    "Count Temperature": {
                        "kind": "aggregate",
                        "filter": null,
                        "value": null,
                        "interpolation": "None",
                        "aggregation": {"tsx": "count()"}
                    },
                    "Min Temperature": {
                        "kind": "aggregate",
                        "filter": null,
                        "value": null,
                        "interpolation": "None",
                        "aggregation": {"tsx": "min($event.temperature)"}
                    }
                }
            },
            "error": null
        }
    ]
}
```

### <a name="gethierarchies-api"></a>getHierarchies API

A API de getHierarchies devolve todas as hierarquias de séries de tempo e todos os seus caminhos de campo associados.

A API de getHierarchies precisa dos seguintes parâmetros:

* *ID*: a chave de identificação de forma exclusiva para a hierarquia.
* *nome*: o nome da hierarquia
* *Origem*
* *instanceFields*
  * *Ano*
  * *Mês*

#### <a name="gethierarchies-json-request-and-response-example"></a>exemplo de solicitação e resposta do JSON de getHierarchies

Recebe um pedido de POST HTTP:

```plaintext
https://YOUR_ENVIROMENT.env.timeseries.azure.com/timeseries/hierarchies/$batch?api-version=API_VERSION
```

| Nome | Descrição | Exemplo |
| --- | --- | --- |
| YOUR_ENVIRONMENT  |  O nome do seu ambiente  | `environment123` |
| API_VERSION  |  A especificação de API | `2018-11-01-preview` |

Com o corpo JSON:

```JSON
{
    "get": null,
    "put": [
        {
            "id": "4c6f1231-f632-4d6f-9b63-e366d04175e3",
            "name": "Location",
            "source": {
                "instanceFieldNames": [
                    "state",
                    "city"
                ]
            }
        }
    ]
}
```

Resposta:

```JSON
{
    "get": null,
    "put": [
        {
            "hierarchy": {
                "id": "4c6f1231-f632-4d6f-9b63-e366d04175e3",
                "name": "Location",
                "source": {
                    "instanceFieldNames": [
                        "state",
                        "city"
                    ]
                }
            },
            "error": null
        }
    ]
}
```

### <a name="getinstances-api"></a>getInstances API

A API de getInstances é usado para retornar todas as instâncias de série de tempo e todos os seus campos de instância associada.

A API de getInstances precisa dos seguintes parâmetros:

* *nome*: o nome associado a instância, utilizada para consultar, bem como substituindo na experiência do usuário.
* *typeId*: a chave de forma exclusiva de identificação para o tipo.
* *timeSeriesId*: A **ID da série de tempo** é a chave exclusiva para os dados dentro do fluxo de eventos e o modelo. Esta chave é o que utiliza o TSI para particionar os dados.
* *Descrição*: a descrição da instância.
* *hierarchyIds*: uma matriz de um ou mais dos hierarchyIds que definem exclusivamente cada hierarquia.
* *instanceFields*:
  * *state*
  * *Cidade*

#### <a name="getinstances-json-request-and-response-example"></a>exemplo de solicitação e resposta do JSON de getInstances

Recebe um pedido de POST HTTP:

```plaintext
https://YOUR_ENVIROMENT.env.timeseries.azure.com/timeseries/instances/$batch?api-version=API_VERSION
```

| Nome | Descrição | Exemplo |
| --- | --- | --- |
| YOUR_ENVIRONMENT  |  O nome do seu ambiente  | `environment123` |
| API_VERSION  |  A especificação de API | `2018-11-01-preview` |

Com o corpo JSON:

```JSON
{
    "get": null,
    "put": [
        {
            "name": "SampleName",
            "typeId": "1be09af9-f089-4d6b-9f0b-48018b5f7393",
            "timeSeriesId": [
                "samplePartitionKeyValueOne"
            ],
            "description": "floor 100",
            "hierarchyIds": [
                "e37a4666-9650-42e6-a6d2-788f12d11158"
            ],
            "instanceFields": {
                "state": "California",
                "city": "Los Angeles"
            }
        }
    ]
}
```

Resposta:

```JSON
{
    "get": null,
    "put": [
        {
            "instance": null,
            "error": null
        },
        {
            "instance": null,
            "error": null
        }
    ]
}
```

### <a name="aggregateseries-api"></a>aggregateSeries API

AggregateSeries que API permite a consulta e obtenção de dados TSI dos eventos capturados amostragem e agregar gravados dados usando as funções de agregação ou de exemplo.

A API de aggregateSeries precisa dos seguintes parâmetros:

* *timeSeriesId*: A **ID da série de tempo** é a chave exclusiva para os dados dentro do fluxo de eventos e o modelo. Esta chave é o que utiliza o TSI para particionar os dados.
* *searchSpan*: O tamanho do intervalo de tempo e de registo para esta expressão de agregação.
* *Filtro*: cláusula de predicado opcional.
* *Intervalo*: intervalo a que dados devem ser computados.
* *ProjectedVariables*: variáveis que estão no âmbito para ser computada.
* *InlineVariables(optional)*: definições de variáveis que queremos usar para o de substituição do que trazer via TSM ou inline fornecido para o cálculo.

A API de getSeries retorna um TSV para cada variável, para cada intervalo, com base em fornecido **ID de série de tempo** e o conjunto de variáveis fornecidos. A API de getSeries alcança redução ao tirar partido das variáveis armazenados no TSM ou fornecida inline para dados de agregação ou de exemplo.

> [!NOTE]
> Interpolação variável não é atualmente suportada.

Tipos de agregação de suporte:

* `Min`
* `Max`
* `Sum`
* `Count`
* `Average`

#### <a name="aggregateseries-json-request-and-response-example"></a>exemplo de solicitação e resposta do JSON de aggregateSeries

Recebe um pedido de POST HTTP:

```plaintext
https://YOUR_ENVIROMENT.env.timeseries.azure.com/timeseries/query?api-version=API_VERSION
```

| Nome | Descrição | Exemplo |
| --- | --- | --- |
| YOUR_ENVIRONMENT  |  O nome do seu ambiente  | `environment123` |
| API_VERSION  |  A especificação de API | `2018-11-01-preview` |

Com o corpo JSON:

```JSON
{
    "aggregateSeries": {
        "timeSeriesId": ["da2754af-cc51-46b3-b18f-6231f8781a12","PU.98"],
        "searchSpan": {
            "from": {"dateTime": "2016-08-01T00:00:00Z"},
            "to": {"dateTime": "2016-08-01T00:10:00Z"}
        },
        //Optional
        //If provided, Reduction should be specified in Variable Definition
        "interval": "PT1M",
        "filter": null, //Optional
        "projectedVariables": [
            "Count",
            "Average Temperature",
            "Min Temperature"
        ],
        "inlineVariables": {
            "Average Temperature": {
                "kind": "numeric",
                "filter": null,
                "value": { "tsx": "$event.temperature.Double" },
                "interpolation": "None",
                "aggregation": {"tsx": "avg($value)"}
            },
            //Default Type Count Experience
            "Count": {
                "kind": "aggregate",
                "filter": null,
                "value": null,
                "interpolation": "None",
                "aggregation": {"tsx": "count()"}
            },
            "Min Temperature": {
                "kind": "aggregate",
                "filter": null,
                "value": null,
                "interpolation": "None",
                "aggregation": {"tsx": "min($event.temperature)"}
            },
        }
    }
}
```

Resposta:

```JSON
{
    "timestamps": [ "2016-08-01T00:00:00Z","2016-08-01T00:01:00Z","2016-08-01T00:02:00Z","2016-08-01T00:03:00Z","2016-08-01T00:04:00Z",
        "2016-08-01T00:05:00Z","2016-08-01T00:06:00Z","2016-08-01T00:07:00Z","2016-08-01T00:08:00Z","2016-08-01T00:09:00Z" ],
    "properties": [
        {
            "name": "Average Temperature",
            "type": "Double",
            "values": [ 68.699982037970059,68.889287593526234,69.089287593526919,69.28928759352759,69.489287593528246,69.689287593528917,69.8892875935296,70.089287593530273,70.289287593530929,70.4892875935316]
        },
        {
            "name": "Count Temperature",
            "type": "Double",
            "values": [ 60.0,60.0,60.0,60.0,60.0,60.0,60.0,60.0,60.0,60.0 ]
        },
        {
            "name": "Min Temperature",
            "type": "Double",
            "values": [ 68.645954260192127,68.790954260192578,68.990954260193249,69.190954260193919,69.39095426019459,69.590954260195261,69.790954260195932,69.9909542601966,70.190954260197273,70.390954260197944 ]
        }
    ]
}
```

### <a name="getseries-api"></a>getSeries API

GetSeries que API permite a consulta e obtenção de dados TSI dos eventos capturados ao tirar partido de dados registados na conexão com as variáveis definir no modelo, ou fornecidos inline.

> [!Note]
> Se a cláusula de interpolação e a agregação é fornecida na variável ou intervalo é especificado, serão ignorada.

A API de getSeries precisa dos seguintes parâmetros:

* *timeSeriesId*: A **ID da série de tempo** é a chave exclusiva para os dados dentro do fluxo de eventos e o modelo. Esta chave é o que utiliza o TSI para particionar os dados.
* *searchSpan*: O tamanho do intervalo de tempo e de registo para esta expressão de agregação.
* *Filtro*: cláusula de predicado opcional.
* *ProjectedVariables*: variáveis que estão no âmbito para ser computada.
* *InlineVariables(optional)*: definições de variáveis que queremos usar para o de substituição do que trazer via TSM ou inline fornecido para o cálculo.

A API de getSeries retorna um TSV para cada variável, com base em fornecido **ID de série de tempo** e o conjunto de variáveis fornecidos. GetSeries que alcança a API não suporta intervalos ou variável de redução/interpolação.  

#### <a name="getseries-json-request-and-response-example"></a>exemplo de solicitação e resposta do JSON de getSeries

Recebe um pedido de POST HTTP:

```plaintext
https://YOUR_ENVIROMENT.env.timeseries.azure.com/timeseries/query?api-version=API_VERSION
```

| Nome | Descrição | Exemplo |
| --- | --- | --- |
| YOUR_ENVIRONMENT  |  O nome do seu ambiente  | `environment123` |
| API_VERSION  |  A especificação de API | `2018-11-01-preview` |

Com o corpo JSON:

```JSON
{
    "getSeries": {
        "timeSeriesId": ["da2754af-cc51-46b3-b18f-6231f8781a12","PU.98"],
        "searchSpan": {
            "from": {"dateTime": "2016-08-01T00:00:00Z"},
            "to": {"dateTime": "2016-08-01T00:10:00Z"}
        },
        "interval": null, //Null or not specified
        "filter": null, //Optional
        "projectedVariables": [
            "Temperature",
            "Pressure",
        ],
        "inlineVariables": {
            "Temperature": {
                "kind": "numeric",
                "filter": null,
                "value": { "tsx": "$event.temperature.Double" },
                "interpolation": "None", // null, not specified or ignored if specified
                "aggregation": {"tsx": "avg($value)"} //null, not specified or ignored if specified
            },
            "Pressure": {
                "kind": "numeric",
                "filter": null,
                "value": { "tsx": "$event.pressure.Double" },
                "interpolation": "None", // null, not specified or ignored if specified
                "aggregation": {"tsx": "avg($value)"} //null, not specified or ignored if specified
            }
        }
    }
}
```

Resposta:

```JSON
{
    "timestamps": [ "2016-08-01T00:00:00Z","2016-08-01T00:01:00Z","2016-08-01T00:02:00Z","2016-08-01T00:03:00Z","2016-08-01T00:04:00Z",
        "2016-08-01T00:05:00Z","2016-08-01T00:06:00Z","2016-08-01T00:07:00Z","2016-08-01T00:08:00Z","2016-08-01T00:09:00Z" ],
    "properties": [
        {
            "name": "Temperature",
            "type": "Double",
            "values": [ 68.699982037970059,68.889287593526234,69.089287593526919,69.28928759352759,69.489287593528246,69.689287593528917,69.8892875935296,70.089287593530273,70.289287593530929,70.4892875935316 ]
        },
        {
            "name": "Pressure",
            "type": "Double",
            "values": [ 68.645954260192127,68.790954260192578,68.990954260193249,69.190954260193919,69.39095426019459,69.590954260195261,69.790954260195932,69.9909542601966,70.190954260197273,70.390954260197944 ]
        }
    ]
}
```

### <a name="getevents-api"></a>getEvents API

A API de getEvents permite consulta e a obtenção de dados TSI de eventos à medida que foram registradas no Time Series Insights do fornecedor de origem.

A API de getEvents precisa dos seguintes parâmetros:

* *timeSeriesId*: A **ID da série de tempo** é a chave exclusiva para os dados dentro do fluxo de eventos e o modelo. Esta chave é o que utiliza o TSI para particionar os dados.
* *searchSpan*: o tamanho do intervalo de tempo e de registo para esta expressão de agregação.
* *filtro*: permite-lhe especificar os valores de predicado para filtrar suas consultas, no entanto, o desejado.
* *(opcional) de projectedProperties*: permite a filtragem de coluna/propriedades.

Os valores, getEvents API devolve em bruto de eventos capturados conforme armazenado no TSI para um determinado **ID de série de tempo** e o intervalo de tempo. Ele não exige definições de variável (nem TSM nem as definições de variável são utilizadas).

### <a name="getevents-json-request-and-response-example"></a>exemplo de solicitação e resposta do JSON de getEvents

Recebe um pedido de POST HTTP:

```plaintext
https://YOUR_ENVIROMENT.env.timeseries.azure.com/timeseries/query?api-version=API_VERSION
```

| Nome | Descrição | Exemplo |
| --- | --- | --- |
| YOUR_ENVIRONMENT  |  O nome do seu ambiente  | `environment123` |
| API_VERSION  |  A especificação de API | `2018-11-01-preview` |

Com o corpo JSON:

```JSON
{
    "getEvents": {
        // Required and Supports 3 Key as tsId
        "timeSeriesId": [
            "PU.123","W00158","ABN.9890"
        ],
        // Required.
        "searchSpan": {
            "from": "2018-08-01T17:01:00Z",
            "to": "2018-08-20T17:01:00Z",
        },
        // Optional.
        "filter": {
            "tsx": "($event.Value != null) OR ($event.Status.String == 'Good')"
        },
        // Optional – array of simple TSXs that refer exactly one property
        // If not specified, we will return all the properties.
        "projectedProperties": [ 
            {"propertyName" : "Volts", "type": "double" } 
        ]
    }
}
```

Resposta:

```JSON
{
    "timestamps": [ "2016-08-01T00:00:00Z","2016-08-01T00:01:03Z","2016-08-01T00:02:05Z","2016-08-01T00:03:00Z",
        "2016-08-01T00:04:00Z","2016-08-01T00:05:05Z","2016-08-01T00:06:00Z","2016-08-01T00:07:08Z",
        "2016-08-01T00:08:00Z","2016-08-01T00:09:00Z" ],
    "properties": [
        {
            "name": "Volts",
            "type": "Double",
            "values": [ 68.699982037970059,68.889287593526234,69.089287593526919,69.28928759352759,69.489287593528246,69.689287593528917,69.8892875935296,70.089287593530273,70.289287593530929,70.4892875935316 ]
        }
    ]
}
```

### <a name="supported-query-operators"></a>Operadores de consulta suportados

Filtragem:

* `HAS`
* `IN`
* `AND`
* `AND NOT`
* `OR`
* `>`
* `<`
* `<=`
* `>=`
* `!=`
* `<>`
* `/`
* `*`

Temporal:

* `From`
* `To`
* `First/Last` (Apenas para o API)

Agregação/transformação:

* `MEASURE`
* `SUM` (de uma medida)
* `COUNT` (eventos)
* `AVG` (de uma medida)
* `MIN` (de uma medida)
* `MAX` (de uma medida) "
* `GROUP BY` (coluna categórica)
* `ORDER BY ASC, DSC` (relativamente à timestamp)
* `DateHistogram` (tamanho do registo)

## <a name="tsq-api-limits"></a>Limites de APIS de TSQ

> [!NOTE]
> A tabela a seguir especifica os limites a partir do **11/20/18**.

| Parâmetro | Limites |
| --- | --- |
| Tamanho do pedido TSQ | 32 KB |
| Limite de resposta TSQ | 16 MB |
| Limite de consulta paralela TSQ | 20 por ambiente |
| Obter eventos | 16 MB de tamanho ou 30s tempo |
| Validade do token | 1 hora |
| Série de introdução | 500 000 intervalos ou carimbos ou 16 MB de tamanho |
| Mín. | Limite de intervalo de 1 ms |
| Máx. | 50 variáveis previstas |
| Série de agregação | 500 000 intervalos ou carimbos ou 16 MB de tamanho |
| Mín. | Limite de intervalo de 1 ms |
| Máx. | 50 variáveis previstas |

## <a name="time-series-query-tutorial"></a>Tutorial de consultas de séries de tempo

Um tutorial TSQ completo será fornecido no futuro.

[!INCLUDE [tsi-update-docs](../../includes/time-series-insights-update-documents.md)]

## <a name="next-steps"></a>Passos Seguintes

Leitura a [Azure TSI (pré-visualização), armazenamento e de entrada](./time-series-insights-update-storage-ingress.md).

Saiba mais sobre a nova [modelo de série de tempo](./time-series-insights-update-tsm.md).

Leia sobre [de melhores práticas ao escolher um ID de série de tempo](./time-series-insights-update-how-to-id.md).

<!-- Images -->
[1]: media/v2-update-tsq/goal.png
[2]: media/v2-update-tsq/tsq.png
