---
title: Melhores práticas para formação JSON em consultas de informações de séries de tempo do Azure.
description: Saiba como melhorar a eficiência de consulta de informações de séries de tempo.
services: time-series-insights
author: ashannon7
manager: timlt
ms.service: time-series-insights
ms.topic: article
ms.date: 05/24/2018
ms.author: bryanla
ms.openlocfilehash: 29919a3ce8c18982c88f7f0e6bbd774c612e9c44
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34661092"
---
# <a name="how-to-shape-json-to-maximize-query-performance"></a>Como a forma JSON para maximizar o desempenho de consulta 

Este artigo fornece orientação para formação JSON, para maximizar a eficiência das suas consultas Insights de séries de tempo do Azure (TSI).

## <a name="best-practices"></a>Melhores práticas

É importante refletir sobre a forma como pode enviar eventos para informações de séries de tempo. Nomeadamente, deve sempre:

1. envie dados através da rede mais eficientemente possível.
2. Certifique-se de que os dados são armazenados de forma que lhe permite efetuar agregações adequadas para o seu cenário.
3. Certifique-se de não atingiu limites de propriedade máximo do TSI do
   - 600 propriedades (colunas) para ambientes de S1.
   - 800 propriedades (colunas) para ambientes de S2.

As seguintes orientações ajuda a garantir o melhor desempenho de consulta possíveis:

1. Não utilize as propriedades dinâmicas, tais como um ID de tag como nome de propriedade, como os contribuem para atingir o limite máximo de propriedades.
2. Não envie propriedades desnecessárias. Se uma propriedade de consulta não é necessária, é melhor não enviá-lo e evitar limitações de armazenamento.
3. Utilize [referência a dados](time-series-insights-add-reference-data-set.md), para evitar o envio de dados estáticos através da rede.
4. Propriedades de dimensão entre vários eventos, para enviar dados através da rede de forma mais eficiente da partilha.
5. Não utilize o aninhamento de matriz profunda. TSI suporta até dois níveis de matrizes aninhadas que contêm objetos. TSI flattens matrizes nas mensagens de em vários eventos com pares de valor de propriedade.
6. Se apenas existirem algumas medidas para a maioria ou todos os eventos, é melhor enviar estas medidas como propriedades separadas dentro do mesmo objeto. Enviá-los separadamente reduz o número de eventos e poderá fazer consultas mais performant como menos eventos têm de ser processada. Quando existem várias medidas, enviando-as como valores numa única propriedade minimiza a possibilidade de atingir o limite máximo de propriedade.

## <a name="examples"></a>Exemplos

Os seguintes dois exemplos demonstram envio de eventos, para realçar as recomendações anteriores. Os seguintes cada exemplo, pode ver como as recomendações foram aplicadas.

Os exemplos são baseados num cenário em que vários dispositivos as enviam medidas ou sinais. Foi possível medidas ou sinais fluxo de velocidade, motor petróleo pressão, temperatura e humidade. No primeiro exemplo, existem algumas medidas em todos os dispositivos. No segundo exemplo, existem vários dispositivos e envia cada muitos valores exclusivos.

### <a name="scenario-only-a-few-measurementssignals-exist"></a>Cenário: existem apenas algumas medidas/sinais

**Recomendação:** enviar cada medida como uma propriedade/coluna distinta.

No exemplo seguinte, não há uma única mensagem de IoT Hub, em que a matriz externa contém uma secção partilhada de valores de dimensão comuns. A matriz externa utiliza dados de referência para aumentar a eficiência da mensagem. Dados de referência contém os metadados do dispositivo, que não se altera com cada evento, mas fornece propriedades útil para análise de dados. Os valores de dimensão comuns de criação de batches e utilização de dados de referência, guarda em bytes enviados através da transmissão, deste modo, pelo que a mensagem mais eficiente.

Payload JSON de exemplo:

```json
[
    {
        "deviceId": "FXXX",
        "timestamp": "2018-01-17T01:17:00Z",
        "series": [
            {
                "Flow Rate ft3/s": 1.0172575712203979,
                "Engine Oil Pressure psi ": 34.7
            },
            {
                "Flow Rate ft3/s": 2.445906400680542,
                "Engine Oil Pressure psi ": 49.2
            }
        ]
    },
    {
        "deviceId": "FYYY",
        "timestamp": "2018-01-17T01:18:00Z",
        "series": [
            {
                "Flow Rate psi": 0.58015072345733643,
                "Engine Oil Pressure psi ": 22.2
            }
        ]
    }
]
```

Tabela de referência de dados (propriedade da chave é deviceId):

| deviceId | messageId | deviceLocation |
| --- | --- | --- |
| FXXX | LINHA\_DADOS | UE |
| FYYY | LINHA\_DADOS | EUA |

Tabela de eventos de informações de séries de tempo (após pelo):

| deviceId | messageId | deviceLocation | carimbo de data/hora | série. Fluxo de velocidade ft3/s | série. Psi de petróleo pressão do motor |
| --- | --- | --- | --- | --- | --- |
| FXXX | LINHA\_DADOS | UE | 2018-01-17T01:17:00Z | 1.0172575712203979 | 34.7 |
| FXXX | LINHA\_DADOS | UE | 2018-01-17T01:17:00Z | 2.445906400680542 | 49.2 |
| FYYY | LINHA\_DADOS | EUA | 2018-01-17T01:18:00Z | 0.58015072345733643 | 22.2 |

Tenha em atenção o seguinte no exemplo anterior:

- O **deviceId** coluna funciona como o cabeçalho de coluna para vários dispositivos numa frota. A tentar efetuar deviceId valor seu próprio nome de propriedade, seria limitada total de dispositivos para 595 (S1 ambientes) ou 795 (S2 ambientes), com as outras cinco colunas.

- Propriedades desnecessárias são evitadas de exemplo, informações de marca e o modelo, etc. Uma vez que estes não serão consultados no futuro, eliminar-los permite um melhor rede e a eficiência de armazenamento.

- Dados de referência são utilizados para reduzir o número de bytes transferidos através da rede. Dois atributos, **messageId** e **deviceLocation** , estão associados a utilizar a propriedade de chave, **deviceId**. Estes dados estão associados com os dados de telemetria ao tempo de entrada e, subsequentemente, armazenados na TSI para consultas.

- Duas camadas de aninhamento são utilizadas, que é a quantidade máxima de aninhamento suportado pelo TSI. É fundamental para evitar matrizes profundamente aninhadas.

- As medidas são enviadas como propriedades separadas dentro do mesmo objeto, uma vez que existem algumas medidas. Aqui, **série. Fluxo psi taxa** e **série. Motor de petróleo pressão ft3/s** colunas exclusivo.

### <a name="scenario-several-measures-exist"></a>Cenário: Existem várias medidas

**Recomendação:** enviar medidas como "type", "unidade", "valor" cadeias de identificação.

Payload JSON de exemplo:

```json
[
    {
        "deviceId": "FXXX",
        "timestamp": "2018-01-17T01:17:00Z",
        "series": [
            {
                "tagId": "pumpRate",
                "value": 1.0172575712203979
            },
            {
                "tagId": "oilPressure",
                "value": 49.2
            },
            {
                "tagId": "pumpRate",
                "value": 2.445906400680542
            },
            {
                "tagId": "oilPressure",
                "value": 34.7
            }
        ]
    },
    {
        "deviceId": "FYYY",
        "timestamp": "2018-01-17T01:18:00Z",
        "series": [
            {
                "tagId": "pumpRate",
                "value": 0.58015072345733643
            },
            {
                "tagId": "oilPressure",
                "value": 22.2
            }
        ]
    }
]
```

Dados de referência (as propriedades chave são deviceId e series.tagId):

| deviceId | series.tagId | messageId | deviceLocation | tipo | unidade |
| --- | --- | --- | --- | --- | --- |
| FXXX | pumpRate | LINHA\_DADOS | UE | Taxa de fluxo | ft3/s |
| FXXX | oilPressure | LINHA\_DADOS | UE | Motor petróleo pressão | psi |
| FYYY | pumpRate | LINHA\_DADOS | EUA | Taxa de fluxo | ft3/s |
| FYYY | oilPressure | LINHA\_DADOS | EUA | Motor petróleo pressão | psi |

Tabela de eventos de informações de séries de tempo (após pelo):

| deviceId | series.tagId | messageId | deviceLocation | tipo | unidade | carimbo de data/hora | Series.Value |
| --- | --- | --- | --- | --- | --- | --- | --- |
| FXXX | pumpRate | LINHA\_DADOS | UE | Taxa de fluxo | ft3/s | 2018-01-17T01:17:00Z | 1.0172575712203979 |
| FXXX | oilPressure | LINHA\_DADOS | UE | Motor petróleo pressão | psi | 2018-01-17T01:17:00Z | 34.7 |
| FXXX | pumpRate | LINHA\_DADOS | UE | Taxa de fluxo | ft3/s | 2018-01-17T01:17:00Z | 2.445906400680542 |
| FXXX | oilPressure | LINHA\_DADOS | UE | Motor petróleo pressão | PSI | 2018-01-17T01:17:00Z | 49.2 |
| FYYY | pumpRate | LINHA\_DADOS | EUA | Taxa de fluxo | ft3/s | 2018-01-17T01:18:00Z | 0.58015072345733643 |
| FYYY | oilPressure | LINHA\_DADOS | EUA | Motor petróleo pressão | psi | 2018-01-17T01:18:00Z | 22.2 |

Tenha em atenção o seguinte no exemplo anterior e semelhante ao exemplo primeiro:

- colunas **deviceId** e **series.tagId** servir como os cabeçalhos de coluna para os vários dispositivos e as etiquetas numa frota. Utilizar cada como o seu próprio atributo seria limitada a consulta 594 (S1 ambientes) ou 794 (S2 ambientes) total de dispositivos com as outras seis colunas.

- propriedades desnecessárias foram evitadas pela razão citou no primeiro exemplo.

- dados de referência são utilizados para reduzir o número de bytes transferidos através da rede, introduzindo **deviceId**, para um par único de **messageId** e **deviceLocation**. É utilizada uma chave composta, **series.tagId**, para o par exclusivo de **tipo** e **unidade**. Permite que a chave composta o **deviceId** e **series.tagId** par a ser utilizado para fazer referência aos quatro valores: **messageId, deviceLocation, tipo,** e **unidade** . Estes dados estão associados com os dados de telemetria ao tempo de entrada e, subsequentemente, armazenados na TSI para consultas.

- duas camadas de aninhamento são utilizadas para a razão citou no primeiro exemplo.

### <a name="for-both-scenarios"></a>Para ambos os cenários

Se tiver uma propriedade com um grande número de valores possíveis, é melhor enviar como valores distintos dentro de uma única coluna, em vez de criar uma nova coluna para cada valor. Dos dois exemplos anteriores:
  - No primeiro exemplo, existem algumas propriedades que têm vários valores, pelo que é adequado tornar cada uma propriedade de diferente. 
  - No entanto, o segundo exemplo, pode ver se as medidas não estão especificadas como propriedades individuais, mas em vez disso, uma matriz de valores/medidas sob uma propriedade comum da série. É enviada uma nova chave, **tagId** , que cria uma nova coluna, **series.tagId** na tabela simplificada. Novas propriedades são criadas, **tipo** e **unidade**, utilizando os dados de referência, assim a impedir que o limite de propriedade que está a ser atingido.

## <a name="next-steps"></a>Passos Seguintes

Para colocar estas diretrizes em prática, consulte [sintaxe de consulta de informações de séries de tempo de Azure](/rest/api/time-series-insights/time-series-insights-reference-query-syntax) para saber mais sobre a sintaxe de consulta para os dados TSI aceder à REST API.