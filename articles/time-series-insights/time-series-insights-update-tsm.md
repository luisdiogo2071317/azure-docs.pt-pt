---
title: Modelo de série de tempo | Documentos da Microsoft
description: Noções básicas sobre o modelo de série de tempo
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 11/30/2018
ms.openlocfilehash: 4cb83b61068922002c0c308f4d129a2e069beadd
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52856447"
---
# <a name="time-series-model"></a>Modelo de série de tempo

Detalhes deste documento a **modelo de série de tempo** parte (TSM) da atualização do Azure Time Series Insights (TSI). Ele descreve o próprio modelo, as suas capacidades e como começar a criar e para atualizar o seu próprio modelo.

Tradicionalmente, os dados recolhidos a partir de dispositivos IoT não possui informações contextuais, tornando difícil localizar e analisar rapidamente os sensores. A principal motivação para TSM é simplificar a localizar e analisar os dados de IoT através da organização, manutenção e enriquecimento de dados de séries de tempo para o ajudar a preparar conjuntos de dados de preparado para o consumidor. **Modelos de série de tempo** desempenham um papel fundamental em consultas e de navegação, uma vez que eles contextualizar os entidades de dispositivo e não o dispositivo. Os dados mantidos em computações de consultas de séries de tempo powers TSM ao tirar partido das fórmulas armazenadas nas mesmas.

![TSM][1]

## <a name="key-capabilities"></a>Principais capacidades

Com o objetivo para que seja simples e sem esforço para gerir contextualization de série de tempo, TSM permite as seguintes funcionalidades no Azure Time Series Insights (pré-visualização):

* A capacidade de criar e gerir computações ou fórmulas, para transformar dados tirar partido das funções escalares, operações agregadas, etc.
* Definir relações de subordinados de principal para ativar a navegação e de referência para fornecer contexto para a telemetria de série de tempo.
* Definir propriedades associadas a parte de instâncias de campos de instância e utilizá-las para criar hierarquias.

## <a name="times-series-model-key-components"></a>Componentes principais do modelo de série de tempo

Existem três principais componentes do TSM:

* **Modelo de série de tempo** *tipos*
* **Modelo de série de tempo** *hierarquias*
* **Modelo de série de tempo** *instâncias*

## <a name="time-series-model-types"></a>Tipos de modelo de série de tempo

**Modelo de série de tempo** *tipos* permitem definir variáveis ou fórmulas para fazer cálculos e estão associadas uma determinada instância TSI. Um tipo pode ter uma ou mais variáveis. Por exemplo, uma instância do TSI pode ser do tipo **Sensor de temperatura**, que consiste as variáveis: *temperatura média*, *min temperatura*, e *máx. temperatura*. Vamos criar um tipo de padrão quando os dados começaram a fluir para o TSI. Podem ser obtido e atualizada de definições do modelo. Tipos predefinidos terão uma variável que conte o número de eventos.

## <a name="time-series-model-type-json-request-and-response-example"></a>Tempo o modelo de série tipo solicitação e resposta de exemplo de JSON

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
``````

R **predefinição** *tipo* resposta JSON:

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

## <a name="variables"></a>Variáveis

Tipos TSI do Azure tem variáveis, estes são denominados cálculos sobre os valores dos eventos. As definições de variável de TSI contêm regras de fórmula e computação. As definições de variável incluem o tipo, o valor, o filtro, a redução e limites. As variáveis são armazenadas na definição de tipo em TSM e podem ser fornecidas inline através de APIs de consulta para substituir a definição armazenada.

A matriz a seguir funciona como uma legenda para as definições de variável:

![tabela][2]

### <a name="variable-kind"></a>Tipo de variável

São suportados os tipos de variáveis seguintes:

* Numérico – contínua

### <a name="variable-filter"></a>Filtro de variável

Filtros de variável especifique uma cláusula de filtro opcional para restringir o número de linhas a ser considerado para computação com base em condições.

### <a name="variable-value"></a>Valor da variável

Os valores das variáveis são e devem ser usados no cálculo. Esta é a coluna nos eventos que podemos devem consultar.

### <a name="variable-interpolation"></a>Interpolação variável

O processo de conversão de um conjunto de valores para um valor por um intervalo é chamado uma redução de variável. Reduções de variável podem ser gravados, os dados agregados da origem ou reconstruído sinais usando interpolação e agregadas ou reconstruídas sinais usando interpolação e amostragem. Limites de variáveis podem ser adicionados a interpolação, eles permitem que os cálculos incluir os eventos fora do intervalo de pesquisa.

O Azure TSI (pré-visualização) suporta a interpolação de variável seguinte: `linear`, `stepright`, e `none`.

### <a name="variable-aggregation"></a>Agregação de variável

A função de agregação a variável permite que a parte de computação. Se for de interpolação de variável `null` ou `none`, em seguida, o TSI irá suportar agregados regulares (ou seja, ele **min**, **máximo**, **média**, **soma** , e **contagem**). Se estiver a interpolação variável `stepright` ou `linear`, em seguida, o TSI suportará **twmin**, **twmax**, **twavg**, e **twsum**. Não é possível especificar a contagem de interpolação.

## <a name="time-series-model-hierarchies"></a>Hierarquias de modelo de série de tempo

Hierarquias de organizam instâncias ao especificar os nomes das propriedades e suas relações. Pode ter uma hierarquia única ou várias hierarquias. Além disso, eles não tem de ser uma parte atual dos seus dados, mas cada instância deve ser mapeada para uma hierarquia. Uma instância TSM pode mapear para uma hierarquia única ou várias hierarquias.

Hierarquias são definidas pelas **ID de hierarquia**, **nome**, e **origem**. Hierarquias tiverem caminhos, um caminho é a ordem de cima para baixo principal-subordinado da hierarquia, que o usuário deseja criar. As propriedades de pai/filhos mapeiam campos de instância.

### <a name="time-series-model-hierarchy-json-request-and-response-example"></a>Tempo o modelo de série hierarquia solicitação e resposta de exemplo de JSON

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

### <a name="hierarchy-definition-behavior"></a>Comportamento de definição de hierarquia

Considere o exemplo seguinte, em que hierarquia H1 tem "Criar", "floor" e "sala de" como parte de sua definição:

```plaintext
 H1 = [“building”, “floor”, “room”]
```

Consoante os campos de instância, os atributos de hierarquia e valores aparecerá irá mostrar: 

| ID de série de tempo | Campos de instância |
| --- | --- |
| ID1 | "Criar" = "1000", "piso" = "10", "espaço" = "55"  |
| ID2 | "Criar" = "1000", "espaço" = "55" |
| ID3 |  "floor" = "10" |
| ID4 | "Criar" = "1000", "piso" = "10"  |
| ID5 | |

No acima de exemplo, ID1 mostra como parte da hierarquia H1 na interface do Usuário/UX, enquanto o resto estão classificados como `Unparented Instances` , uma vez que eles não estão em conformidade com a hierarquia de dados especificada.

## <a name="time-series-model-instances"></a>Instâncias do modelo de série de tempo

As instâncias são a série de tempo propriamente ditas. Na maioria dos casos será a *deviceId* ou *assetId* ou seja, o identificador exclusivo do recurso no ambiente. As instâncias têm informações descritivas associadas a eles chamado propriedades de instância. No mínimo, as propriedades de instância incluem informações de hierarquia. Eles também podem incluir dados úteis e descritivos, como o fabricante, o operador ou a última data de serviço.

Instâncias são definidas pelas *timeSeriesId*, *typeId*, *hierarchyId*, e *instanceFields*. Cada instância é mapeado para um único *tipo*e um ou mais hierarquias. Instâncias de herdam todas as propriedades de hierarquias, enquanto adicionais *instanceFields* podem ser adicionados para definição de propriedade de instância ainda mais.

*instanceFields* são propriedades de uma instância e quaisquer dados estáticos que define uma instância. Elas definem valores de propriedades de hierarquia ou não da hierarquia, também dar suporte a indexação para realizar operações de pesquisa.

## <a name="time-series-model-instance-json-request-and-response-example"></a>Tempo o modelo de série instância solicitação e resposta de exemplo de JSON

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

## <a name="time-series-model-settings-example"></a>Exemplo de definições do modelo de série de tempo

Recebe um pedido de POST HTTP:

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

## <a name="time-series-model-limits"></a>Limites de modelo de série de tempo

| Parâmetro |   Limites |
| --- | --- |
| Tamanho do objeto para entidades de modelo (tipos, hierarquias e instâncias)|  32 KB, inclui propriedades |
| Chaves permitidas como propriedade TSID configurada através do Portal do Azure |   Máx. de 3 |
| N. º máximo de tipos num ambiente |    1000|
| N. º máximo de variáveis num tipo |    50|
| N. º máximo de hierarquias num ambiente|   32|
| Profundidade da hierarquia de máx. | 32|
| N. º máximo de hierarquias associados com 1 instância   |21|
| N. º máximo de instâncias num ambiente |    500,000|
| N. º máximo de campos de instância por instância |   50|
| Operação de upsert/atualizar/eliminar modelos de objetos por segundo   |100 por segundo|
| Tamanho do pedido de API de modelo de série de tempo: Batch |  8 MB ou objetos de modelo de 1000 (que ocorrer primeiro)|
| Tamanho do pedido de modelo de série de tempo: pesquisa/Sugerir   | 32 KB|
| Tamanho do pedido de API de modelo de série de tempo: Batch    | 32 MB|
| Pesquisa/sugira é de 32 MB|  32 MB|

[!INCLUDE [tsi-update-docs](../../includes/time-series-insights-update-documents.md)]

## <a name="next-steps"></a>Passos Seguintes

Leitura a [Azure TSI (pré-visualização), armazenamento e de entrada](./time-series-insights-update-storage-ingress.md).

Saiba mais sobre a nova [modelo de série de tempo](./time-series-insights-update-tsm.md).

<!-- Images -->
[1]: media/v2-update-tsm/tsm.png
[2]: media/v2-update-tsm/table.png
