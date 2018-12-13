---
title: Explorar o ambiente de demonstração do Azure Time Series Insights (pré-visualização) | Documentos da Microsoft
description: Compreender o ambiente de demonstração do Azure Time Series Insights (pré-visualização)
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: anshan
ms.topic: quickstart
ms.workload: big-data
ms.custom: mvc
ms.date: 12/03/2018
ms.openlocfilehash: 76bdc122cab5b4ee87717c9c4df9774ecc3bd8f7
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2018
ms.locfileid: "52888788"
---
# <a name="explore-the-azure-time-series-insights-preview-demo-environment"></a>Explorar o ambiente de demonstração do Azure Time Series Insights (pré-visualização)

Este guia de introdução mostra-lhe como começar com o Explorador de pré-visualização do Azure Time Series Insight (TSI) num ambiente de demonstração gratuito. Saiba como utilizar o seu navegador da web para visualizar grandes volumes de dados históricos de IoT industriais e realizar uma visita guiada das principais funcionalidades do Explorador do Azure Time Series Insights (pré-visualização).

O Azure TSI fornece um ponto-a-ponto plataforma-como-um-serviço ingerir, processar, armazenar e consultar dados de dimensionamento IoT contextualizados altamente otimizado de série de tempo, para exploração de dados ad-hoc, bem como a análise operacional. O Time Series Insights é uma oferta diferenciada sob medida para necessidades exclusivas de implementações de IoT industriais.

O ambiente de demonstração mostra uma eletricidade geração da empresa, Contoso, usando o TSI para localizar informações acionáveis em seus dados e realizar uma análise da causa curto. Contoso opera dois farms de turbina vento, cada um com 10 turbines, e cada turbina tem 20 sensores dados de relatórios a cada minuto hub IoT do Azure. Sensores reunir informações sobre condições meteorológicas, pitch Painel & posição yaw, desempenho de gerador, comportamento gearbox e monitores de segurança.

O TSI (pré-visualização) é utilizado para analisar o conjunto de dados cada vez maior dos dois últimos anos – atualmente em 40 GB – para compreender melhor e preveja falhas críticas e problemas de manutenção de movimento lento.

Se não tiver uma subscrição do Azure, crie uma [conta do Azure gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de começar.

## <a name="explore-time-series-insights-explorer-in-a-demo-environment"></a>Explore o explorador do Time Series Insights num ambiente de demonstração

1. No seu browser, navegue até [insights.timeseries.azure.com/preview/samples](https://insights.timeseries.azure.com/preview/samples).  

1. Se lhe for pedido, inicie sessão no Explorador de TSI com as suas credenciais de conta do Azure.

### <a name="demo-step-one"></a>Passo de demonstração um

1. Vamos dar uma olhada **turbina eólica #7 no farm de vento #1**.  

    * **Ação**: atualizar o intervalo de visualização para `1/1/17 20:00 – 3/10/17 20:00 (UTC)` e adicione o `Farm 1 > W7 > Generator > GeneratorSpeed` sensor. Em seguida, apresenta os valores resultantes.

       ![Início rápido, um][1]

1. Recentemente, **Contoso encontrados de incêndio na turbina eólica #7**. Vejamos com mais detalhes aqui. Podemos ver o sensor de alerta de fogo ativado durante o período da acionar.

    * **Ação**: atualizar o intervalo de visualização para `3/9/17 20:00 – 3/10/17 20:00 (UTC)` e adicione o `Safety > FireAlert` sensor.

      ![Início rápido dois][2]

1. Vamos ver o que mais tenham acontecido perto da hora a acionar. Ambos petróleo pressão e avisos ativos pelo imediatamente antes do fogo, mas por esse ponto, que já era tarde demais para avert o problema.

    * **Ação**: Adicione o `Pitch > HydraulicOilPressure` sensor e o `Pitch > ActiveWarning` sensor.

      ![Início rápido três][3]

1. Se Vamos reduzir, podemos ver havia sinais que levou à acionar. Variou entre ambos os sensores. Portanto, isso aconteceu antes?

    * **Ação**: atualizar o intervalo de visualização para `2/24/17 20:00 – 3/10/17 20:00 (UTC)`.

      ![Início rápido quatro][4]

1. Se examinar os todos dois anos de dados, podemos ver um evento de fogo anterior com o mesmo inicia de sessão. Com estes dados, podemos criar sistemas para capturar problemas como esse desde o início.

    * **Ação**: atualizar o intervalo de visualização para `1/1/16 – 12/31/17` (todos os dados).

       ![Início rápido cinco][5]

### <a name="demo-step-two"></a>Passo dois da demonstração

1. Outros problemas são mais sutis e mais difícil de diagnosticar. O Time Series Insights oferece uma gama de capacidades para ajudar-na rastrear problemas difíceis. Aqui podemos ver uma interrupção do sensor de aviso no `turbine #6` no `6/25`. Mas o que realmente está acontecendo?

    * **Ação**: remover os sensores atuais. Em seguida, atualize o intervalo de visualização para `6/1/17 20:00 – 7/1/17 20:00 (UTC)` e adicione o `Farm 1 > W6 > Safety > VoltageActuatorSwitchWarning`.

       ![Início rápido seis][6]

1. O aviso indica um problema com a tensão que está a ser o resultado pelo gerador de. Mas o que é a causa? A eficiência geral de saída o gerador parece bem num intervalo granular. Mas ao agregar os dados, podemos ver uma lista definitiva.

    * **Ação**: remoto a `VoltageActuatorSwitchWarning` e adicione `Generator > ActivePower` e o intervalo de atualização para `3d`.

       ![Início rápido sete][7]

1. Se, navegar para a frente do conjunto de dados, podemos ver que isso não é apenas um problema transitório. O problema é continuar.

    * **Ação**: expandir o intervalo de tempo para a direita.

       ![Início rápido oito][8]

1. Vamos explorar ainda mais. Podemos outros pontos de dados de sensor para ver a tensão por fase. Mas todos eles procuram comparáveis. Vamos remover um marcador para ver os valores reais. Parece que três é um problema com a saída de fase 3.

    * **Ação**: `Add Generator > GridVoltagePhase1, 2, & 3`. Remova um marcador no último ponto de dados na área visível.

       ![Início rápido oito][8]

1. Se podemos ver todos os três na mesma escala, faz a fase 3 drop desativar ainda mais óbvia. Neste momento, estamos prontos para fazer referência este problema para a nossa equipa de manutenção com uma oportunidade potencial boa sobre a causa do aviso.  

    * **Ação**: atualizar a apresentação de colocar todos os sensores na mesma escala de gráfico.

       ![Início rápido nove][9]

## <a name="next-steps"></a>Passos Seguintes

Está pronto para criar seu próprio ambiente do Azure TSI (pré-visualização):

> [!div class="nextstepaction"]
> [Planejar seu ambiente do Azure TSI (pré-visualização)](time-series-insights-update-plan.md)

<!-- Images -->
[1]: media/v2-update-quickstart/quickstart-one.png
[2]: media/v2-update-quickstart/quickstart-two.png
[3]: media/v2-update-quickstart/quickstart-three.png
[4]: media/v2-update-quickstart/quickstart-four.png
[5]: media/v2-update-quickstart/quickstart-five.png
[6]: media/v2-update-quickstart/quickstart-six.png
[7]: media/v2-update-quickstart/quickstart-seven.png
[8]: media/v2-update-quickstart/quickstart-eight.png
[9]: media/v2-update-quickstart/quickstart-nine.png