---
title: Como eventos de forma com o Azure Time Series Insights (pré-visualização) | Documentos da Microsoft
description: Noções básicas sobre como formatar eventos com o Azure Time Series Insights (pré-visualização)
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 11/30/2018
ms.openlocfilehash: edc1dac05a8ab4281eee3ee0eb4c5e6b7571b404
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52856435"
---
# <a name="shaping-events-with-azure-time-series-insights-preview"></a>Formatação de eventos com o Azure Time Series Insights (pré-visualização)

Este artigo fornece orientações para formatação JSON, para maximizar a eficiência que sejam as consultas do Azure Time Series Insights (pré-visualização).

## <a name="best-practices"></a>Melhores práticas

> [!NOTE]
> Para o Azure TSI (pré-visualização), os limites de propriedade de 600 800 para S1/S2 não se aplicam.

É importante pensar sobre como enviar eventos para o Azure TSI. Ou seja, deve sempre:

1. envie dados através da rede mais eficientemente possível.
1. Certifique-se de que os seus dados são armazenados de forma que lhe permite realizar agregações adequadas para o seu cenário.

As seguintes orientações ajuda a garantir o melhor desempenho de consulta possíveis:

1. Não envie propriedades desnecessárias. O TSI (pré-visualização) irá cobrar-lhe na sua utilização e é uma prática recomendada para armazenar e processar dados que irá consultar.
1. Utilize os campos de instância para dados estáticos para evitar o envio de dados estáticos na rede. Campos de instância, um componente do modelo de série de tempo, funcionam como os dados no serviço do TSI em disponibilidade geral de referência. Para saber mais sobre o campo de instância, leia [modelos de série de tempo](./time-series-insights-update-tsm.md).
1. Partilhar as propriedades de dimensão entre vários eventos, para enviar dados através da rede com mais eficiência.
1. Não utilize o aninhamento de matriz profunda. O TSI suporta até dois níveis de matrizes aninhadas que contêm objetos. O TSI nivela as matrizes de nas mensagens, em vários eventos com pares de valor de propriedade.
1. Se apenas existirem algumas medidas para todas as ou a maioria dos eventos, é melhor enviar essas medidas como propriedades separadas dentro do mesmo objeto. Enviar-lhe em separado reduz o número de eventos e pode fazer consultas com um melhor desempenho, já que menos eventos precisam ser processada.

## <a name="example"></a>Exemplo

O exemplo se baseia num cenário em que vários dispositivos enviam medições ou sinais. Foi possível medidas ou sinais **taxa de fluxo**, **pressão do motor de petróleo**, **temperatura**, e **humidade**.

No exemplo a seguir, há uma única mensagem do IoT Hub, em que a matriz externa contém uma secção partilhada comuns de valores de dimensão. A matriz externa utiliza dados de instâncias de série de tempo para aumentar a eficiência da mensagem. Instância de série de tempo contém metadados do dispositivo, que não se altera com cada evento, mas fornece propriedades úteis para análise de dados. Os valores de dimensão comuns de criação de batches e empregando metadados de instância de série de tempo, salva em bytes enviados pela rede, tornando a mensagem mais eficiente.

Payload JSON de exemplo:

```JSON
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

Instância de série de tempo (Nota: os **ID de série de tempo** é *deviceId*):

```JSON
{
    "timeSeriesId": [
      "FXXX"
    ],
    "typeId": "17150182-daf3-449d-adaf-69c5a7517546",
    "hierarchyIds": [
      "b888bb7f-06f0-4bfd-95c3-fac6032fa4da"
    ],
    "description": null,
    "instanceFields": {
      "L1": "REVOLT SIMULATOR",
      "L2": "Battery System",
    }
  },
  {
    "timeSeriesId": [
      "FYYY"
    ],
    "typeId": "17150182-daf3-449d-adaf-69c5a7517546",
    "hierarchyIds": [
      "b888bb7f-06f0-4bfd-95c3-fac6032fa4da"
    ],
    "description": null,
    "instanceFields": {
      "L1": "COMMON SIMULATOR",
      "L2": "Battery System",
    }
  },
```

O TSI Ingressou na tabela (depois de mesclar) durante o tempo de consulta. A tabela inclui colunas adicionais, tais como o tipo. Este exemplo demonstra como pode modelar seus dados de telemetria:

| deviceId  | Tipo | L1 | ERROS DE L2 | carimbo de data/hora | série. Taxa de fluxo ft3/s | série. Psi de petróleo pressão do motor |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| `FXXX` | Default_Type | SIMULADOR DE REVOLT | Sistema de bateria | 2018-01-17T01:17:00Z |    1.0172575712203979 |    34.7 |
| `FXXX` | LINE_DATA REVOLT | SIMULADOR |    Sistema de bateria |    2018-01-17T01:17:00Z | 2.445906400680542 |  49.2 |
| `FYYY` | LINE_DATA COMUNS | SIMULADOR |    Sistema de bateria |    2018-01-17T01:18:00Z | 0.58015072345733643 |    22,2 |

Tenha em atenção o seguinte no exemplo anterior:

* Propriedades estáticas são armazenadas como TSI para otimizar os dados enviados através da rede.
* Dados de TSI são associados no momento da consulta ao utilizar o *timeSeriesId* definido na instância.
* São utilizadas duas camadas de aninhamento, que é a quantidade máxima de aninhamento suportado pelo TSI. É fundamental para evitar matrizes profundamente aninhadas.
* Medidas são enviadas como propriedades separadas dentro do mesmo objeto, uma vez que existem algumas medidas. Aqui, **série. Fluxo psi taxa**, série **pressão do motor de petróleo**, e **ft3/s** são colunas exclusivas.

>[!IMPORTANT]
> Campos de instância não são armazenados com telemetria, eles são armazenados com metadados no **modelo de série de tempo**.
> A tabela acima representa a vista de consulta.

## <a name="next-steps"></a>Passos Seguintes

Para colocar estas diretrizes em prática, consulte [sintaxe de consulta do Azure TSI](./time-series-insights-query-data-csharp.md) para saber mais sobre a sintaxe de consulta para a REST API de acesso os dados do TSI.
