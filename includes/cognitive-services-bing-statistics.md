---
title: incluir ficheiro
description: incluir ficheiro
services: cognitive-services
author: scottwhi-msft
ms.service: cognitive-services
ms.topic: include
ms.date: 04/09/2018
ms.author: scottwhi
ms.custom: include file
ms.openlocfilehash: 4e19c1afefdc5bcacebcb0d495193b48c7a6d724
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36313715"
---
Estatísticas do Bing fornece análises de APIs de pesquisa do Bing. Análise de inclui o volume de chamadas, cadeias de consulta superior, distribuição geográfica e muito mais. Para ativar o Bing estatísticas na sua pesquisa do Bing subscrição paga, navegue até à sua [dashboard do Azure](https://portal.azure.com/#create/Microsoft.CognitiveServicesBingSearch-v7), selecione a subscrição paga e clique em Ativar estatísticas do Bing. Ativar Bing estatísticas aumenta a taxa de subscrição ligeiramente (consulte [preços](https://aka.ms/bingstatisticspricing)).


> [!NOTE]
> Estatísticas de Bing está disponível com apenas para subscrições pagas - não está disponível com subscrições de avaliação gratuitas. 

> [!NOTE]
> Não pode utilizar os dados disponíveis através do dashboard de estatísticas do Bing para criar aplicações para distribuição a terceiros.

Bing dados de análise de atualizações a cada 24 horas e mantém cópias de segurança para compreendendo 13 meses do histórico.

## <a name="accessing-your-analytics"></a>Aceder à sua análise

Para aceder ao seu dashboard de análise, aceda a https://bingapistatistics.com. Certifique-se de que iniciou sessão com a mesma conta de Microsoft (MSA) que utilizou para obter a subscrição paga.


## <a name="filtering-the-data"></a>Os dados de filtragem

Por predefinição, os gráficos e gráficos refletem todos os dados de métricas que tenha acesso. Pode filtrar os dados apresentados nos gráficos e gráficos, selecionando a recursos, mercados, pontos finais e período de relatórios estiver interessado em. Os gráficos e gráficos alterar para refletir os filtros que se aplicam. Os seguintes descrevem os filtros que pode alterar.

- **ID de recurso**: O ID de recurso exclusivo que identifica a sua subscrição do Azure. A lista contiver vários IDs se subscreve a mais do que uma camada de API de pesquisa do Bing. Por predefinição, são selecionados todos os recursos.  
  
- **Mercados**: mercados em que os resultados provenientes. Por exemplo, en-us (inglês, Estados Unidos). Por predefinição, são selecionados todos os mercados. Tenha em atenção que o mercado en WW mercado que o Bing utiliza se a chamada não especificar um mercado e Bing é não é possível determinar o mercado do utilizador.  
  
- **Pontos finais**: pontos finais da API de pesquisa do Bing. A lista contém todos os pontos finais para o qual tem uma subscrição paga. Por predefinição, são selecionados todos os pontos finais.  

- **Período de tempo**: O período de relatório. Pode especificar:  
  
  - Todos os&mdash;inclui cópias de segurança para compreendendo 13 meses de dados  
  - Após 24 horas&mdash;inclui análise das últimas 24 horas  
  - Passado semana&mdash;inclui análise últimos sete dias  
  - Passado mês&mdash;inclui análise últimos 30 dias  
  - Um intervalo de datas personalizado&mdash;inclui analytics a partir do período de tempo especificado, se disponível  
  
  > [!NOTE]  
  > Pode demorar até 24 horas com base nas métricas a anexação no dashboard. O dashboard apresenta a data e hora que os dados foram atualizados pela última vez.  
  
  > [!NOTE]  
  > Métricas estão disponíveis a partir da hora de que ativar o suplemento de estatísticas do Bing. 


## <a name="charts-and-graphs"></a>Gráficos e gráficos

O dashboard mostra gráficos e gráficos de métricas disponíveis para o ponto final selecionado. Nem todas as métricas estão disponíveis para todos os pontos finais. Os gráficos e gráficos para cada ponto final são estáticos (poderá não selecionou os gráficos e gráficos para apresentar). O dashboard mostra apenas os gráficos e gráficos para os quais há dados. 

<!--
For example, if you don't include the User-Agent header in your calls, the dashboard will not include device-related graphs.
-->

Seguem-se as métricas possíveis. Cada métrica notas restrições de ponto final. 

- **Chamar Volume**: mostra o número de chamadas efetuadas durante o período de relatório. Se o período de relatório for para um dia, o gráfico mostra o número de chamadas efetuadas por hora. Caso contrário, o gráfico mostra o número de chamadas efetuadas por dia do período de relatório.  
  
  > [!NOTE]
  > O volume de chamadas pode ser diferentes das faturação relatórios, que inclui geralmente apenas chamadas bem-sucedidas. 
  
-  **Consultas de cima**: mostra os principais de consultas e o número de ocorrências de cada consulta durante o período de relatório. Pode configurar o número de consultas a apresentar. Por exemplo, pode mostrar as consultas de topo 25, 50 ou 75. Principais de consultas não está disponível para os seguintes pontos finais:  
  
  - /Images/trending
  - imagens/detalhes
  - imagens/visualsearch
  - /videos/trending
  - vídeos/detalhes
  - /News
  - notícias/trendingtopics
  - /Suggestions  
  
  > [!NOTE]  
  > Alguns termos de consulta podem ser suprimidos para remover informações confidenciais, tais como mensagens de correio eletrónico, números de telefone, SSN, etc.  

- **Distribuição geográfica**: mercados em que os resultados provenientes. Por exemplo, en-us (inglês, Estados Unidos). Bing utiliza o `mkt` consultar parâmetro para determinar o mercado, se for especificado. Caso contrário, o Bing utiliza sinais, tais como o endereço IP da função invocadora para determinar o mercado.  
  
- **Distribuição de código de resposta**: códigos de estado de HTTP o de todas as chamadas durante o período de relatório.  
  
- **Distribuição de origem de chamar**: os tipos de browsers utilizados pelos utilizadores. Por exemplo, Edge, Chrome, Safari e FireFox. Chamadas efetuadas a partir de fora do browser, tais como bots, Postman ou utilizando curl a partir de uma aplicação de consola, são agrupadas em bibliotecas. Determinar a origem com o valor de cabeçalho do agente de utilizador do pedido. Se o pedido não inclui o cabeçalho de agente do utilizador, o Bing tenta derivar a origem outros sinais.  
  
- **Distribuição de pesquisa seguro**: A distribuição dos valores de pesquisa seguro. Por exemplo, desativar, moderada ou strict. O `safeSearch` consulta parâmetro contém o valor, se for especificado. Caso contrário, o Bing assume o valor para moderada.  
  
- **Responde a pedidos distribuição**: A API de pesquisa de Web responde a pedidos no `responseFilter` parâmetro de consulta.  
  
- **Responde a distribuição devolvido**: as respostas que API Web do pesquisa devolvido na resposta.  
  
- **Distribuição de servidor de resposta**: O servidor de aplicações que são efetuados os pedidos de API. Os valores possíveis são Bing.com (para o tráfego proveniente de dispositivos de secretária e portáteis) e móveis Bing.com (para o tráfego proveniente de dispositivos móveis). O servidor é determinado utilizando o valor de cabeçalho do agente de utilizador do pedido. Se o pedido não inclui o cabeçalho de agente do utilizador, o Bing tenta derivar o servidor de outros sinais.  
  


O seguinte mostra a análise está disponível para cada ponto final.

![Distribuição pela matriz de suporte de ponto final](./media/cognitive-services-bing-statistics/bing-statistics-matrix.PNG)


