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
ms.date: 12/03/2018
ms.openlocfilehash: 67be21ae7f0cb997563f17130b9d5ecb7d359b31
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52873874"
---
# <a name="azure-time-series-insights-preview-use-cases"></a>Casos de utilização do Azure Time Series Insights (pré-visualização)

Este artigo fornece uma visão geral dos vários casos de utilização comuns para o Azure Time Series Insights (TSI). As recomendações neste artigo servem como um ponto de partida ao desenvolver aplicativos e soluções com o TSI.

Depois de ler este artigo, será capaz de responder às seguintes perguntas:

* Quais são os casos de utilização comuns para o Azure TSI?
* Quais são as vantagens de utilizar o Azure TSI para exploração de dados e deteção de anomalias visual?
* Quais são as vantagens de utilizar o Azure TSI para análises operacionais e eficiência do processo?
* Quais são as vantagens de utilizar o Azure TSI para análise avançada?

Este documento fornece uma visão geral de casos de utilização concebido para a pré-visualização do Azure TSI privada.

## <a name="introduction"></a>Introdução

O Azure TSI é um ponto-a-ponto plataforma-como-um-serviço para ingerir, processar, armazenar e consultar contextualizados altamente otimizado de série de tempo, dados de IoT à escala. Como tal, o Azure TSI é ideal para exploração de dados ad-hoc, bem como a análise operacional. O TSI é um serviço de exclusivamente extensível e personalizado, oferta que satisfazem amplo precisa de implementações de IoT industriais.

## <a name="data-exploration-and-visual-anomaly-detection"></a>Exploração de dados e deteção visual de anomalias

Explore e analise instantaneamente milhares de milhões de eventos para detetar anomalias e descobrir tendências ocultas nos dados. O Azure TSI fornece perto de desempenho em tempo real para as cargas de trabalho de análise de IoT e DevOps.

![Explorador de dados][1]

De todas as forças de Azure TSI, a maioria dos clientes concorde com que o tempo de resposta está entre mais forte. O TSI não requer nenhuma preparação de dados inicial e trabalha depressa, permite ligá-lo a milhões de eventos no seu IoT Hub do Azure ou o Hub de eventos em minutos.  Assim que estiver ligado, pode visualizar e analisar milhares de milhões de eventos para detetar anomalias e descobrir tendências ocultas nos seus dados.  Como o TSI é intuitivo e fácil de utilizar, começará a interagir com os seus dados sem ter de escrever uma única linha de código. Além disso, não existe nenhuma linguagem a aprender; o Time Series Insights oferece consulta granular baseada em texto para utilizadores avançados familiarizados com o SQL, bem como a exploração selecionar e clicar para novatos.

Podemos ver que os clientes a aproveitar esta rapidez para diagnosticar problemas relacionados com o recurso rapidamente, a realização DevOps para obter a causa de um bug numa solução de IoT e a identificar as áreas a investigar quanto a iniciativas de ciência de dados.  

Existem três formas de interagir com dados armazenados no TSI:

1. A primeira e mais fácil começar a utilizar são nossa visualização, o explorer, que permite-lhe visualizar rapidamente todos os seus dados de IoT num único local. Ele fornece ferramentas, como o mapa térmico, que simplificam a detetar visualmente anomalias nos seus dados. Ele também fornece a vista de ponto de vista, o que permite comparar até quatro modos de exibição de um ou mais ambientes de TSI num único dashboard, dando-lhe uma vista dos seus dados de séries temporais em todas as suas localizações. Saiba mais sobre o [Explorador TSI](./time-series-insights-update-explorer.md). Para planejar seu ambiente de atualização TSI, leia [TSI atualizar planeamento](./time-series-insights-update-plan.md).

1. A segunda maneira é usar o SDK JavaScript para incorporar rapidamente poderosos de gráficos em seu próprio aplicativo da web. Com apenas algumas linhas de código, pode criar consultas eficientes para preencher os gráficos de linhas, gráficos circulares, gráficos de barras, mapas térmicos, grades de dados e muito mais. Todos esses elementos existem de-pronta a utilizar o SDK. O SDK também abstrai APIs, permitindo-lhe criar predicados do tipo SQL para consultar os dados que pretende mostrar um dashboard de consulta do TSI. Para soluções de camada de apresentação de híbrida, o TSI oferece URLs parametrizados, que fornecem os pontos de ligação totalmente integrada com o Explorador para tópicos profunda sobre os dados. Para saber mais sobre o JavaScript SDK, veja a [biblioteca de cliente do TSI JS](https://docs.microsoft.com/azure/time-series-insights/tutorial-explore-js-client-lib) e o [cliente TSI](https://github.com/Microsoft/tsiclient) documentação. Para saber mais sobre URLs parametrizados, leia o nosso artigo sobre [parametrizado URLs](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-parameterized-urls).  

1. Por fim, o TSI fornece poderosas APIs para consultar dados armazenados no TSI. O TSI tem operadores temporais, como, para, primeiro e último, agregações e transformações como average, min, max, dividir por, ordenar por e DateHistogram e filtragem de operadores, como tem, no e, em alternativa, maior, REGEX, etc. Todos esses operadores ativar aplicações a jusante encontrar rapidamente as tendências e padrões interessantes nos seus dados e podem ser usados para preencher os próprios visualizações para detetar anomalias.  

## <a name="operational-analysis-and-driving-process-efficiency"></a>Promover a eficiência dos processos e análise operacional

Ative a monitorização do Estado de funcionamento, a utilização e o desempenho dos equipamentos em escala, oferecendo uma forma fácil de medir a eficiência operacional. O Time Series Insights ajuda a gerir cargas de trabalho de IoT diversas e imprevisíveis, sem sacrificar a ingestão ou o desempenho de consultas.

![descrição geral][2]

Processamento contínuo de dados provenientes de processos operacionais e de transmissão em fluxo com êxito podem transformar qualquer negócio se juntamente com a tecnologia/solução certa. Muitas vezes, essas soluções são uma combinação de vários sistemas que permitem a exploração e análise de dados que é alterada constantemente, especialmente no realm de IoT e partilham um padrão comum.

Esses padrões, muitas vezes, começam com plataformas de IoT ativada que ingerir milhões de eventos a partir de dispositivos e sensores que abrangem várias localidades. Estes sistemas de processam e analisar dados de transmissão em fluxo para derivar informações em tempo real e ações, os dados normalmente são arquivados para o armazenamento de acesso pouco frequente e esporádica para quase em tempo real e análise de lotes. Dados recolhidos passem pela série de processamento para limpar e a contextualizar os cenários de consulta e análise de downstream. O Microsoft Azure oferece serviços avançados que podem ser aplicados a esses cenários de IoT (Manutenção ativos, fabrico, etc.). Estes incluem-se ao Azure TSI, IoT Hub do Azure, os Hubs de eventos do Azure, Azure Stream Analytics, as funções do Azure, Azure Logic Apps, Azure Databricks, Azure Machine Learning e Microsoft Power BI.

Esta arquitetura de solução pode ser obtida da seguinte forma – ingestão de dados através do IoT Hub do Azure ou o Hub de eventos do Azure para segurança de melhor em classe, débito e latência. Efetue o processamento de dados e computações, funneling dados ingeridos através de serviços como o Azure Stream Analytics, Azure Logic Apps, funções de Azure consoante as necessidades de processamento de dados específico. Sinais calculadas do pipeline de processamento são enviados por push para o Azure TSI para armazenar e análise. O Azure Time Series Insights oferece quase exploração de dados em tempo real e com base no recurso de informações em dados históricos. Consoante as necessidades de negócio, as tarefas de MapReduce e Hive podem ser executadas em dados armazenados no Time Series Insights, ao ligar o Time Series Insights para o HDInsight. Dados armazenados no Time Series Insights podem ser disponibilizados para o Power BI e outras aplicações de cliente por meio pública superfície consulta dos Time Series Insights APIs para cenários de informações operacionais e de completos para empresas.

## <a name="advanced-analytics"></a>Análise Avançada

Integração com serviços de análises avançadas, como o Azure Machine Learning e o Azure Databricks. O serviço Time Series Insights introduz dados não processados de milhões de dispositivos e adiciona dados contextuais que podem ser consumidos de forma totalmente integrada por um conjunto de serviços de análise do Azure.

![análise][3]

Análise avançada e de machine learning consumirem e processam grandes volumes de dados para tomar decisões condicionadas por dados e realizar análises preditivas. Em casos de utilização do IoT, os algoritmos de análise avançada Aprenda com os dados recolhidos a partir de milhões de dispositivos que podem transmitir dados várias vezes por segundo. No entanto, os dados recolhidos a partir de dispositivos IoT é não processados e não tem informações contextuais, como a localização do dispositivo, unidade da leitura de sensor etc., tornando difícil para os dados para serem consumidas diretamente para análises avançadas.

O Azure Time Series Insights preenche a lacuna entre os dados de IoT e análise avançada de duas formas simples e económicas. Em primeiro lugar, atualização de Time Series Insights recolhe dados de telemetria não processados de milhões de dispositivos através do hub IoT, otimiza os dados com informações contextuais e transforma os dados em "formato parquet" que pode integrar facilmente com uma série de serviços de análises avançadas como, por exemplo como o Azure Machine Learning, Azure Databricks e outras aplicações de terceiros.  O Time Series Insights pode servir como a origem de-de-verdade para todos os dados em toda a organização, criando assim um repositório central para análise de downstream cargas de trabalho para consumir.  Desde o Time Series Insights é um quase em tempo real de armazenamento do serviço, análises avançadas, modelos de aprender continuamente com dados recebidos de telemetria de IoT para fazer previsões mais precisas.

Em segundo lugar, o Time Series Insights pode ser alimentado a saída da modelos de previsão e de aprendizagem automática para visualizar e armazenar os resultados, portanto, ajuda as organizações a otimizar e ajustar seus modelos.  Além disso, o Time Series Insights torna simples para visualizar a transmissão em fluxo de dados de telemetria no mesmo plano como saídas do modelo treinado para ajudar as equipes de ciência de dados detetar anomalias e identificar padrões.  

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre o [Explorador TSI](./time-series-insights-update-explorer.md).

Para planejar seu ambiente, leia [TSI (pré-visualização) planejamento](./time-series-insights-update-plan.md).

Leitura a [cliente do TSI](https://github.com/Microsoft/tsiclient) documentação.

<!-- Images -->
[1]: media/v2-update-use-cases/data-explorer.svg
[2]: media/v2-update-use-cases/overview.svg
[3]: media/v2-update-use-cases/advanced-analytics.svg
