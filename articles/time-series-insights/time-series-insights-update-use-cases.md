---
title: Casos de utilização do Azure Time Series Insights (pré-visualização) | Documentos da Microsoft
description: Casos de utilização de noções básicas sobre o Azure Time Series Insights (pré-visualização)
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 11/27/2018
ms.openlocfilehash: 9d9fab9f0a515cacdf2a1425c4da06c9e3d4c364
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52856558"
---
# <a name="azure-time-series-insights-preview-use-cases"></a>Casos de utilização do Azure Time Series Insights (pré-visualização)

Este artigo fornece uma visão geral dos vários casos de utilização comuns para o Azure Time Series Insights (TSI). As recomendações neste artigo servem como um ponto de partida ao desenvolver aplicativos e soluções com o TSI.

Depois de ler este artigo, será capaz de responder às seguintes perguntas:

* Quais são os casos de utilização comuns para o Azure TSI?
* Quais são as vantagens de utilizar o Azure TSI para exploração de dados e deteção de anomalias visual?
* Quais são as vantagens de utilizar o Azure TSI para análises operacionais e eficiência do processo?
* Quais são as vantagens de utilizar o Azure TSI para análise avançada?

Este documento fornece uma visão geral do que o Azure tempo série Insights pré-visualização privada foi concebido para casos de utilização.

## <a name="data-exploration-and-visual-anomaly-detection"></a>Exploração de dados e deteção visual de anomalias

Explore e analise instantaneamente milhares de milhões de eventos para detetar anomalias e descobrir tendências ocultas nos dados. O Azure TSI fornece perto de desempenho em tempo real para as cargas de trabalho de análise de IoT e DevOps.

![Explorador de dados][1]

De todas as forças de Azure TSI, a maioria dos clientes concorde com que o tempo de resposta está entre mais forte. O TSI não requer nenhuma preparação de dados inicial e trabalha depressa, permite ligá-lo a milhões de eventos no seu IoT Hub do Azure ou o Hub de eventos em minutos.  Assim que estiver ligado, pode visualizar e analisar milhares de milhões de eventos para detetar anomalias e descobrir tendências ocultas nos seus dados.  Como o TSI é intuitivo e fácil de utilizar, começará a interagir com os seus dados sem ter de escrever uma única linha de código. Além disso, não existe nenhuma linguagem a aprender; o Time Series Insights oferece consulta granular baseada em texto para utilizadores avançados familiarizados com o SQL, bem como a exploração selecionar e clicar para novatos.

Podemos ver que os clientes a aproveitar esta rapidez para diagnosticar problemas relacionados com o recurso rapidamente, a realização DevOps para obter a causa de um bug numa solução de IoT e a identificar as áreas a investigar quanto a iniciativas de ciência de dados.  

Existem três formas de interagir com dados armazenados no TSI:

1. A primeira e mais fácil começar a utilizar são nossa visualização, o Explorer, que permite-lhe visualizar rapidamente todos os seus dados de IoT num único local. Ele fornece ferramentas, como o mapa térmico, que simplificam a detetar visualmente anomalias nos seus dados, bem como a exibição de ponto de vista, o que permite-lhe comparam até quatro vistas a partir de um ou mais ambientes de TSI num único dashboard, dando-lhe uma vista dos seus dados de séries temporais em todas as suas localizações. Saiba mais sobre o [Explorador de TSI](./time-series-insights-update-explorer.md). Para planejar seu ambiente de atualização TSI, leia [TSI atualizar planeamento](./time-series-insights-update-plan.md).

1. A segunda maneira é usar o SDK JavaScript para incorporar rapidamente poderosos de gráficos em seu próprio aplicativo da web. Com apenas algumas linhas de código, pode criar consultas eficientes para preencher os gráficos de linhas, gráficos circulares, gráficos de barras, mapas térmicos, grades de dados e muito mais. Todos esses elementos existem de-pronta a utilizar o SDK. O SDK também abstrai APIs, permitindo-lhe criar predicados do tipo SQL para consultar os dados que pretende mostrar um dashboard de consulta do TSI. Para soluções de camada de apresentação de híbrida, o TSI oferece URLs parametrizados, que fornecem os pontos de ligação totalmente integrada com o Explorador para tópicos profunda sobre os dados. Para saber mais sobre o JavaScript SDK, veja a [biblioteca de cliente do TSI JS](https://docs.microsoft.com/azure/time-series-insights/tutorial-explore-js-client-lib) e o [cliente TSI](https://github.com/Microsoft/tsiclient) documentação. Para saber mais sobre URLs parametrizados, leia o nosso artigo sobre [parametrizado URLs](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-parameterized-urls).  

1. Por fim, o TSI fornece poderosas APIs para consultar dados armazenados no TSI. O TSI tem operadores temporais, como, para, primeiro e último, agregações e transformações como average, min, max, dividir por, ordenar por e DateHistogram e filtragem de operadores, como tem, no e, em alternativa, maior, REGEX, etc. Todos esses operadores ativar aplicações a jusante encontrar rapidamente as tendências e padrões interessantes nos seus dados e podem ser usados para preencher os próprios visualizações para detetar anomalias.  

Para obter mais informações sobre as ofertas do Azure para IoT, veja [criar a sua Internet das coisas](https://www.microsoft.com/internet-of-things).

## <a name="operational-analysis-and-driving-process-efficiency"></a>Promover a eficiência dos processos e análise operacional

Ative a monitorização do Estado de funcionamento, a utilização e o desempenho dos equipamentos em escala, oferecendo uma forma fácil de medir a eficiência operacional. O Time Series Insights ajuda a gerir cargas de trabalho de IoT diversas e imprevisíveis, sem sacrificar a ingestão ou o desempenho de consultas.

![descrição geral][2]

Processamento contínuo de dados provenientes de processos operacionais e de transmissão em fluxo com êxito podem transformar qualquer negócio se juntamente com a tecnologia/solução certa. Muitas vezes, essas soluções são uma combinação de vários sistemas que permitem a exploração e análise de dados que é alterada constantemente, especialmente no realm de IoT. Estes sistemas juntos agilizar os cenários que partilham um padrão comum quando se trata de ingestão, processo, armazenar, analisar e visualizar os dados de IoT.

Parte da solução, necessidade de sistemas para ingerir milhões de eventos a partir de dispositivos e sensores que abrangem várias localidades. Em seguida, estes sistemas de processam e analisar dados de transmissão em fluxo para derivar informações em tempo real. Os dados, em seguida, são arquivados para o armazenamento de acesso pouco frequente e esporádica para quase em tempo real e análise de lotes.

Em seguida, estes sistemas tem processo Limpar os dados a ser recolhidos para ativar e contextualization, armazenando os dados para permitir cenários de consulta e análise de downstream. O Microsoft Azure oferece serviços avançados que podem ser aplicados para estes cenários de IoT, incluindo o Azure TSI, IoT Hub do Azure, os Hubs de eventos do Azure, Azure Stream Analytics, as funções do Azure, Azure Logic Apps, Azure Databricks, Azure Machine Learning e Microsoft Power BI.

Com a configuração da solução acima, podem ser ingeridos dados através do Azure IoT ou Hubs de eventos, que oferece ingestão de dados de débito elevado com baixa latência. Podem ser funneled dados ingeridos e que precisam de ser processados para informações em tempo real para o Azure Stream Analytics, Azure Logic Apps e as funções do Azure. O resultado, em seguida, pode ser alimentado ao Power BI para vistas em tempo real, bem como pode ser carregado para o Azure Time Series Insights para alertas e monitorização compará-los com a propagação históricos. Dados ingeridos que precisa de exploração de dados em tempo real ou ad hoc consultar tendências históricas, podem ser carregados diretamente para o Azure Time Series Insights. Os dados carregados estão prontos para ser consultados, juntamente com dados históricos ilimitados para análises operacionais e de análise para otimização de processos para máxima eficácia. Todos os dados ou apenas as alterações dos dados em que carregados mais recentemente podem ser utilizadas como dados de referência como parte da análise em tempo real. Além disso, dados ainda podem ser refinados e processados por ligar os dados do Azure Time Series Insights para o HDInsight para o mapa/redução, Hive, tarefas de etc. Por fim disponibilizar estes dados no Power BI e em qualquer aplicativo de cliente por meio de nossa consulta de superfície pública APIs.

## <a name="advanced-analytics"></a>Análise Avançada

Integração com serviços de análises avançadas, como o Azure Machine Learning e o Azure Databricks. Dados não processados de ingresses TSI de milhões de dispositivos e adiciona dados contextuais que podem ser consumidos por um conjunto de serviços de análise do Azure de forma totalmente integrada.

![análise][3]

Análise avançada e de machine learning consumirem e processam grandes volumes de dados para tomar decisões condicionadas por dados e realizar análises preditivas. Em casos de utilização do IoT, os algoritmos de análise avançada Aprenda com os dados recolhidos a partir de milhões de dispositivos que podem transmitir dados várias vezes por segundo. No entanto, os dados recolhidos a partir de dispositivos IoT é não processados e não tem informações contextuais, como a localização do dispositivo, unidade da leitura de sensor etc. Estes dados não podem ser acessados diretamente para análises avançadas.

O Azure TSI preenche a lacuna entre os dados de IoT e análise avançada de uma forma simple e económica. O TSI recolhe dados de telemetria não processados de milhões de dispositivos, otimiza os dados com informações contextuais e transforma os dados em "formato parquet" que pode ser facilmente integrados com um número de serviços do Azure Advanced Analytics, como o Azure Machine Learning, Azure DataBricks e suas próprias aplicações de terceiros. Modelos de análise avançados aprender continuamente com dados recebidos de telemetria de IoT para fazer previsões mais precisas.

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre o [Explorador de TSI](./time-series-insights-update-explorer.md).

* Para planejar seu ambiente, leia [TSI (pré-visualização) planejamento](./time-series-insights-update-plan.md).

* Leitura a [cliente do TSI](https://github.com/Microsoft/tsiclient) documentação.

<!-- Images -->
[1]: media/v2-update-use-cases/data-explorer.png
[2]: media/v2-update-use-cases/overview.png
[3]: media/v2-update-use-cases/advanced-analytics.png
