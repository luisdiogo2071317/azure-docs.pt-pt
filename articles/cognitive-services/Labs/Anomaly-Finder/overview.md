---
title: O que é anomalias LCA? -Serviços cognitivos Microsoft | Microsoft Docs
description: Utilize algoritmos avançados anomalias LCA para ajudar a identificar anomalias nos dados de séries de tempo e devolvem informações nos serviços cognitivos da Microsoft.
services: cognitive-services
author: tonyxing
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 04/19/2018
ms.author: tonyxing
ms.openlocfilehash: 1080bb0ad1d901a8b9a5ace4993d4e0d46924a03
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353342"
---
# <a name="what-is-anomaly-finder"></a>O que é anomalias LCA?

LCA anomalias permite-lhe monitorizar dados ao longo do tempo e detetar anomalias com machine learning feita aos seus dados exclusivos aplicando automaticamente o direito análises modelo, independentemente da indústria, cenário ou dados de volume. Uma série de tempo a utilizar como entrada, o devolve anomalias LCA API se ou não um ponto de dados é uma anomalias, determina o valor esperado e os limites superior e inferiores para visualização. Como um serviço de AI prebuilt, anomalias LCA não requer qualquer do machine learning conhecimentos para além de compreender como utilizar uma API RESTful. Isto torna o desenvolvimento simples e versátil desde que funciona com quaisquer dados de séries de tempo e pode também ser incorporado em dados de transmissão em fluxo sistemas. Anomalias LCA abrange um intervalo amplo de casos de utilização – por exemplo, financeiros ferramentas para gerir fraude, furto, alterar mercados e potenciais incidentes de negócio ou monitorizar tráfego de dispositivos de IoT preservando anonymity. Esta solução também pode ser monetized como parte de um serviço para clientes de fim compreender as alterações nos dados, gastos, o retorno do investimento ou a atividade do utilizador.
Experimentar a API de LCA anomalias e obter uma compreensão mais aprofundada dos seus dados. 

O que pode criar com esta API, consulte:

* Saiba prever os valores esperados nos dados históricos de série de tempo
* Indique se a um ponto de dados é uma anomalias fora do padrão de histórico
* Gerar uma banda para visualizar o intervalo de valor de "normal"

![Anomaly_Finder](./media/anomaly_detection1.png) 

Fig. 1: Detetar anomalias nas receitas de vendas

![Anomaly_Finder](./media/anomaly_detection2.png)

Fig. 2: Detetar alterações de padrão nos pedidos de serviço

## <a name="requirements"></a>Requisitos

- Série de tempo mínimo dados de entrada: mínimo de 13 dados pontos de séries de tempo sem periodicidade simples, mínimo de 4 ciclos de dados aponta para a série de tempo com periodicidade conhecida. 
- Integridade dos dados: tempo de série de pontos de dados estão separados no mesmo intervalo e não existem pontos em falta. 

## <a name="identify-anomalies"></a>Identificar anomalias

Deteção de anomalias API devolve resultados que quer quaisquer pontos de dados indicadas anomalias ou não e fornece informações adicionais da seguinte forma
* Período - a periodicidade que a API utilizada para detetar os pontos de anomalias.
* WarningText - as informações de aviso possíveis.
* ExpectedValue - o valor previsto pelo learning baseada em modelo
* IsAnomaly - o resultado indica se os pontos de dados estiverem anomalias ou não
* IsAnomaly_Neg - o resultado indica se os pontos de dados estiverem anomalias na direção negativa (dips)
* IsAnomaly_Pos - o resultado indica se os pontos de dados estiverem anomalias na direção positiva (picos de)
* UpperMargin - a soma de ExpectedValue e UpperMargin determina o limite superior que o ponto de dados é ainda considerar como normal
* LowerMargin - (ExpectedValue - LowerMargin) determina vinculado de menor que o ponto de dados é ainda considerar como normal

> [!Note]
> UpperMargin e LowerMargin podem ser utilizados para gerar uma banda à volta de séries de tempo real para visualizar o intervalo de valores normais. 

## <a name="adjusting-lower-and-upper-bounds-in-post-processing-on-the-response"></a>Ajustar inferiores e superiores no processamento da resposta de post

Deteção de anomalias API devolve o resultado predefinido no se um ponto de dados está anomalias ou não e possível calcular o limite superior e inferior de ExpectedValue e UpperMargin/LowerMargin. Os valores predefinidos devem funcionar apenas ajustar para a maioria dos casos. No entanto, alguns cenários requerem limites diferentes à predefinição de campanhas. A prática Recomendamos é aplicar uma coefficiency no UpperMargin ou LowerMargin para ajustar os limites dinâmicos.

### <a name="examples-with-1152-as-coefficiency"></a>Exemplos com 1.5/1/2 como coefficiency

![Sensibilidade predefinido](./media/sensitivity_1.png)

![1.5 sensibilidade](./media/sensitivity_1.5.png)

![Sensibilidade 2](./media/sensitivity_2.png)

Pedido com dados de exemplo

[!INCLUDE [Request](./includes/request.md)]

Resposta JSON de exemplo

[!INCLUDE [Response](./includes/response.md)]
