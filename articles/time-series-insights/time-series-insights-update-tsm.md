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
ms.date: 12/04/2018
ms.openlocfilehash: 5d5f94aebcd55474385e903246ce7945586456dd
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2018
ms.locfileid: "52890420"
---
# <a name="time-series-model"></a>Modelo de Série de Tempo

Detalhes deste documento a **modelo de série de tempo** parte (TSM) da atualização do Azure Time Series Insights (TSI). Ele descreve o próprio modelo, as suas capacidades e como começar a criar e para atualizar o seu próprio modelo.

Tradicionalmente, os dados recolhidos a partir de dispositivos IoT não possui informações contextuais, tornando difícil localizar e analisar rapidamente os sensores. A principal motivação para TSM é simplificar a localizar e analisar os dados de IoT através da organização, manutenção e enriquecimento de dados de séries de tempo para o ajudar a preparar conjuntos de dados de preparado para o consumidor. TSMs desempenham um papel fundamental em consultas e de navegação, uma vez que eles contextualizar os dispositivos e o dispositivo não entidades. Os dados mantidos em computações de consultas de séries de tempo powers TSM ao tirar partido das fórmulas armazenadas nas mesmas.

![TSM][1]

## <a name="key-capabilities"></a>Principais capacidades

Com o objetivo para que seja simples e sem esforço para gerir contextualization de série de tempo, TSM permite as seguintes funcionalidades no The Azure TSI (pré-visualização):

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

## <a name="time-series-model-type-json-example"></a>Exemplo de JSON de tipo de modelo de série de tempo

Exemplo:

```JSON
{
    "name": "SampleType",
    "description": "This is sample type",
    "variables": {
        "Avg Temperature": {
            "kind": "numeric",
            "filter": null,
            "value": { "tsx": "$event.temperature.Double" },
            "aggregation": {"tsx": "avg($value)"}
        },
        "Count Temperature": {
            "kind": "aggregate",
            "filter": null,
            "value": null,
            "aggregation": {"tsx": "count()"}
        }
    }
}
``````

Leia mais sobre os tipos de modelo de série de tempo a partir da [documentação de referência](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#types-api).

## <a name="variables"></a>Variáveis

Tipos TSI do Azure tem variáveis, que são nomeadas cálculos sobre os valores dos eventos. As definições de variável de TSI contêm regras de fórmula e computação. As definições de variável incluem o tipo, o valor, o filtro, a redução e limites. As variáveis são armazenadas na definição de tipo em TSM e podem ser fornecidas inline através de APIs de consulta para substituir a definição armazenada.

A matriz a seguir funciona como uma legenda para as definições de variável:

![tabela][2]

### <a name="variable-kind"></a>Tipo de variável

São suportados os tipos de variáveis seguintes:

* Numérico
* Agregar

### <a name="variable-filter"></a>Filtro de variável

Filtros de variável especifique uma cláusula de filtro opcional para restringir o número de linhas a ser considerado para computação com base em condições.

### <a name="variable-value"></a>Valor da variável

Os valores das variáveis são e devem ser usados no cálculo. Esta é a coluna nos eventos que podemos devem consultar.

### <a name="variable-aggregation"></a>Agregação de variável

A função de agregação a variável permite que a parte de computação. TSI irá suportar agregados regulares (ou seja, ele **min**, **máximo**, **média**, **soma**, e **contagem**).

## <a name="time-series-model-hierarchies"></a>Hierarquias de modelo de série de tempo

Hierarquias de organizam instâncias ao especificar os nomes das propriedades e suas relações. Pode ter uma hierarquia única ou várias hierarquias. Além disso, eles não tem de ser uma parte atual dos seus dados, mas cada instância deve ser mapeada para uma hierarquia. Uma instância TSM pode mapear para uma hierarquia única ou várias hierarquias.

Hierarquias são definidas pelas **ID de hierarquia**, **nome**, e **origem**. Hierarquias tiverem caminhos, um caminho é a ordem de cima para baixo principal-subordinado da hierarquia, que o usuário deseja criar. As propriedades de pai/filhos mapeiam campos de instância.

### <a name="time-series-model-hierarchy-json-example"></a>Exemplo de JSON de hierarquia de modelo de série de tempo

Exemplo:

```JSON
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
```

Saiba mais sobre hierarquias de modelo de série de tempo dos [documentação de referência](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#hierarchies-api).

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

As instâncias são a série de tempo propriamente ditas. Na maioria dos casos, o *deviceId* ou *assetId* será o identificador exclusivo do recurso no ambiente. As instâncias têm informações descritivas associadas a eles chamado propriedades de instância. No mínimo, as propriedades de instância incluem informações de hierarquia. Eles também podem incluir dados úteis e descritivos, como o fabricante, o operador ou a última data de serviço.

Instâncias são definidas pelas *timeSeriesId*, *typeId*, *hierarchyId*, e *instanceFields*. Cada instância é mapeado para um único *tipo*e um ou mais hierarquias. Instâncias de herdam todas as propriedades de hierarquias, enquanto adicionais *instanceFields* podem ser adicionados para definição de propriedade de instância ainda mais.

*instanceFields* são propriedades de uma instância e quaisquer dados estáticos que define uma instância. Elas definem valores de propriedades de hierarquia ou não da hierarquia, também dar suporte a indexação para realizar operações de pesquisa.

## <a name="time-series-model-instance-json-example"></a>Exemplo de JSON de instância de modelo de série de tempo

Exemplo:

```JSON
{
    "typeId": "1be09af9-f089-4d6b-9f0b-48018b5f7393",
    "timeSeriesId": ["sampleTimeSeriesId"],
    "description": "Sample Instance",
    "hierarchyIds": [
        "1643004c-0a84-48a5-80e5-7688c5ae9295"
    ],
    "instanceFields": {
        "state": "California",
        "city": "Los Angeles"
    }
}
```

Saiba mais sobre instâncias do modelo de série de tempo do [documentação de referência](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#instances-api).

## <a name="time-series-model-settings-example"></a>Exemplo de definições do modelo de série de tempo

Exemplo:

```JSON
{
    "modelSettings": {
        "name": "DefaultModel",
        "timeSeriesIdProperties": [
            {
                "name": "id",
                "type": "String"
            }
        ],
        "defaultTypeId": "1be09af9-f089-4d6b-9f0b-48018b5f7393"
    }
}
```

Saiba mais sobre as definições de modelo de série de tempo dos [documentação de referência](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#model-settings-api).

## <a name="next-steps"></a>Passos Seguintes

Leitura a [armazenamento do Azure TSI (pré-visualização) e de entrada](./time-series-insights-update-storage-ingress.md).

Leia o sobre a nova [modelo de série de tempo](https://docs.microsoft.com/rest/api/time-series-insights/preview-model).

<!-- Images -->
[1]: media/v2-update-tsm/tsm.png
[2]: media/v2-update-tsm/table.png
