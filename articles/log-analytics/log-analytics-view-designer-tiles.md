---
title: "Um guia de referência para os mosaicos do estruturador de vistas no Log Analytics do Azure | Microsoft Docs"
description: "Ao utilizar o estruturador de vistas da análise de registos, pode criar vistas personalizadas no portal do Azure que apresentam uma variedade de visualizações de dados na sua área de trabalho de análise de registos. Este artigo é um guia de referência para as definições para os mosaicos que estão disponíveis no seu vistas personalizadas."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: 41787c8f-6c13-4520-b0d3-5d3d84fcf142
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/17/2018
ms.author: bwren
ms.openlocfilehash: f341cb9430c7750909c1fc1f50c15f0620e74366
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="reference-guide-to-view-designer-tiles-in-log-analytics"></a>Guia de referência para os mosaicos do estruturador de vistas na análise de registos
Ao utilizar o estruturador de vistas no Log Analytics do Azure, pode criar vistas personalizadas no portal do Azure que apresentam uma variedade de visualizações de dados na sua área de trabalho de análise de registos. Este artigo é um guia de referência para as definições para os mosaicos que estão disponíveis no seu vistas personalizadas.

Para obter mais informações sobre o estruturador de vistas, consulte:

* [Ver Designer](log-analytics-view-designer.md): fornece uma descrição geral do estruturador de vistas e procedimentos para criar e editar vistas personalizadas.
* [Referência de parte de visualização](log-analytics-view-designer-parts.md): Fornece um guia de referência para as definições para as peças de visualização estão disponíveis no seu vistas personalizadas.


Os mosaicos do estruturador de vistas disponíveis são descritos na tabela seguinte:  

| Mosaico | Descrição |
|:--- |:--- |
| [Número](#number-tile) |A contagem de registos por uma consulta. |
| [Dois números](#two-numbers-tile) |O número de registos de duas consultas diferentes. |
| [anel](#donut-tile) | Um gráfico que é baseado numa consulta, com um valor de resumo no Centro de mensagens em fila. |
| [Gráfico de linhas e de chamada](#line-chart-amp-callout-tile) | Um gráfico de linhas com base na consulta e uma chamada com um valor de resumo. |
| [Gráfico de linhas](#line-chart-tile) |Um gráfico de linhas que é baseado numa consulta. |
| [Duas linhas cronológicas](#two-timelines-tile) | Um gráfico de colunas com duas séries, cada baseada numa consulta separada. |

As secções seguintes descrevem os tipos de mosaico e as respetivas propriedades em detalhe.

## <a name="number-tile"></a>Mosaico de número
O **número** mosaico mostra ambas a contagem de registos a partir de uma consulta de registo e uma etiqueta.

![Mosaico de número](media/log-analytics-view-designer/tile-number.png)

| Definição | Descrição |
|:--- |:--- |
| Nome |O texto que é apresentado na parte superior do mosaico. |
| Descrição |O texto que é apresentado sob o nome do mosaico. |
| **Mosaico** | |
| Legenda |O texto que é apresentado sob o valor. |
| Consulta |A consulta que é executada. É apresentada a contagem de registos que são devolvidos pela consulta. |
| **Avançadas** |**> Verificação de-fluxo de dados** |
| Ativado |Selecione esta ligação se a verificação de fluxo de dados deve ser ativada para o mosaico. Esta abordagem fornece uma mensagem alternativa se os dados estão disponíveis. Normalmente utiliza a abordagem para fornecer uma mensagem durante o período de temporário quando a vista está instalada e os dados ficam disponíveis. |
| Consulta |A consulta que é executada para determinar se os dados estão disponíveis para a vista. Se a consulta devolve não existem resultados, é apresentada uma mensagem em vez do valor da consulta principal. |
| Mensagem |A mensagem é apresentada se a consulta de verificação de fluxo de dados devolver sem dados. Não se fornecer nenhuma mensagem, um *efetuar avaliação* é apresentada a mensagem de estado. |


## <a name="two-numbers-tile"></a>Mosaico de dois números
Este mosaico mostra a contagem de registos de duas consultas de registo diferentes e uma etiqueta para cada.

![Mosaico de dois números](media/log-analytics-view-designer/tile-two-numbers.png)

| Definição | Descrição |
|:--- |:--- |
| Nome |O texto que é apresentado na parte superior do mosaico. |
| Descrição |O texto que é apresentado sob o nome do mosaico. |
| **Primeiro mosaico** | |
| Legenda |O texto que é apresentado sob o valor. |
| Consulta |A consulta que é executada. É apresentada a contagem de registos que são devolvidos pela consulta. |
| **Mosaico segundo** | |
| Legenda |O texto que é apresentado sob o valor. |
| Consulta |A consulta que é executada. É apresentada a contagem de registos que são devolvidos pela consulta. |
| **Avançadas** |**> Verificação de-fluxo de dados** |
| Ativado |Selecione esta ligação se a verificação de fluxo de dados deve ser ativada para o mosaico. Esta abordagem fornece uma mensagem alternativa se os dados estão disponíveis. Normalmente utiliza a abordagem para fornecer uma mensagem durante o período de temporário quando a vista está instalada e os dados ficam disponíveis. |
| Consulta |A consulta que é executada para determinar se os dados estão disponíveis para a vista. Se a consulta devolve não existem resultados, é apresentada uma mensagem em vez do valor da consulta principal. |
| Mensagem |A mensagem é apresentada se a consulta de verificação de fluxo de dados devolver sem dados. Não se fornecer nenhuma mensagem, um *efetuar avaliação* é apresentada a mensagem de estado. |


## <a name="donut-tile"></a>Mosaico de anel
O **anel** mosaico apresenta um número único que resume uma coluna de valor de uma consulta de registo. O anel graficamente apresenta resultados dos registos de três principais.

![Mosaico de anel](media/log-analytics-view-designer/tile-donut.png)

| Definição | Descrição |
|:--- |:--- |
| Nome |O texto que é apresentado na parte superior do mosaico. |
| Descrição |O texto que é apresentado sob o nome do mosaico. |
| **anel** | |
| Consulta |A consulta que está a ser executada para o anel. A primeira propriedade é um valor de texto e a segunda propriedade é um valor numérico. Esta consulta normalmente utiliza o *medidas* palavra-chave para resumir os resultados. |
| **anel** |**> Center** |
| Texto |O texto que é apresentado sob o valor de anel. |
| Operação |A operação é efetuada a propriedade value para resumi-lo como um valor único.<ul><li>Soma: Adicione os valores de todos os registos com o valor da propriedade.</li><li>Percentagem: Percentagem dos valores summed de registos com o valor da propriedade em comparação comparada os valores summed de todos os registos.</li></ul> |
| Valores de resultado utilizados na operação do System center |Opcionalmente, selecione o sinal de adição (+) para adicionar um ou mais valores. Os resultados da consulta estão limitados a registos com os valores de propriedade que especificar. Se não existem valores são adicionados, todos os registos estão incluídos na consulta. |
| **anel** |**> Opções adicionais** |
| Cores |A cor que é apresentada para cada um dos três propriedades superiores. Para especificar cores alternativas para os valores de propriedade específica, utilize *avançadas mapeamento de cor*. |
| Mapeamento de cor avançadas |Apresenta uma cor que representa valores de propriedade específica. Se tiver o valor que especifica três parte superior, a cor alternativa é apresentada em vez da cor do padrão. Se a propriedade não está a ser três parte superior, a cor não é apresentada. |
| **Avançadas** |**> Verificação de-fluxo de dados** |
| Ativado |Selecione esta ligação se a verificação de fluxo de dados deve ser ativada para o mosaico. Esta abordagem fornece uma mensagem alternativa se os dados estão disponíveis. Normalmente utiliza a abordagem para fornecer uma mensagem durante o período de temporário quando a vista está instalada e os dados ficam disponíveis. |
| Consulta |A consulta que é executada para determinar se os dados estão disponíveis para a vista. Se a consulta devolve não existem resultados, é apresentada uma mensagem em vez do valor da consulta principal. |
| Mensagem |A mensagem é apresentada se a consulta de verificação de fluxo de dados devolver sem dados. Não se fornecer nenhuma mensagem, um *efetuar avaliação* é apresentada a mensagem de estado. |


## <a name="line-chart-tile"></a>Mosaico de gráfico de linha
Este mosaico está um gráfico de linhas que apresenta várias séries por uma consulta de registo ao longo do tempo. 

![Mosaico de gráfico e da chamada de linha](media/log-analytics-view-designer/tile-line-chart.png)

| Definição | Descrição |
|:--- |:--- |
| Nome |O texto que é apresentado na parte superior do mosaico. |
| Descrição |O texto que é apresentado sob o nome do mosaico. |
| **Gráfico de linhas** | |
| Consulta |A consulta que está a ser executada para o gráfico de linhas. A primeira propriedade é um valor de texto e a segunda propriedade é um valor numérico. Esta consulta normalmente utiliza o *medidas* palavra-chave para resumir os resultados. Se a consulta utiliza a *intervalo* palavra-chave, o eixo x utiliza este intervalo de tempo. Se a consulta não utiliza o *intervalo* palavra-chave, os intervalos de hora a hora do eixo x utiliza. |
| **Gráfico de linhas** |**> Eixo y** |
| Utilizar escala logarítmica |Selecione esta ligação para utilizar uma escala logarítmica para o eixo y. |
| Unidades |Especifique as unidades para os valores devolvidos pela consulta. Estas informações são utilizadas para apresentar as etiquetas no gráfico indicar os tipos de valor e, opcionalmente, para converter os valores. O **um tipo de unidade** Especifica a categoria da unidade e define o **atual um tipo de unidade** valores que estão disponíveis. Se selecionar um valor na **converter** , em seguida, os valores numéricos são convertidos do **unidade atual** escreva para a **converter** tipo. |
| Etiqueta personalizada |O texto que é apresentado para o eixo y junto a etiqueta para o *unidade* tipo. Não se for especificada nenhuma etiqueta, apenas o *unidade* tipo é apresentado. |
| **Avançadas** |**> Verificação de-fluxo de dados** |
| Ativado |Selecione esta ligação se a verificação de fluxo de dados deve ser ativada para o mosaico. Esta abordagem fornece uma mensagem alternativa se os dados estão disponíveis. Normalmente utiliza a abordagem para fornecer uma mensagem durante o período de temporário quando a vista está instalada e os dados ficam disponíveis. |
| Consulta |A consulta que é executada para determinar se os dados estão disponíveis para a vista. Se a consulta devolve não existem resultados, é apresentada uma mensagem em vez do valor da consulta principal. |
| Mensagem |A mensagem é apresentada se a consulta de verificação de fluxo de dados devolver sem dados. Não se fornecer nenhuma mensagem, um *efetuar avaliação* é apresentada a mensagem de estado. |


## <a name="line-chart-and-callout-tile"></a>Mosaico de gráfico e da chamada de linha
Este mosaico tem ambos os uma linha que apresenta várias séries de um registo de consulta ao longo do tempo e uma chamada com um valor resumido do gráfico. 

![Mosaico de gráfico e da chamada de linha](media/log-analytics-view-designer/tile-line-chart-callout.png)

| Definição | Descrição |
|:--- |:--- |
| Nome |O texto que é apresentado na parte superior do mosaico. |
| Descrição |O texto que é apresentado sob o nome do mosaico. |
| **Gráfico de linhas** | |
| Consulta |A consulta que está a ser executada para o gráfico de linhas. A primeira propriedade é um valor de texto e a segunda propriedade é um valor numérico. Esta consulta normalmente utiliza o *medidas* palavra-chave para resumir os resultados. Se a consulta utiliza a *intervalo* palavra-chave, o eixo x utiliza este intervalo de tempo. Se a consulta não utiliza o *intervalo* palavra-chave, os intervalos de hora a hora do eixo x utiliza. |
| **Gráfico de linhas** |**> Chamada** |
| Título da chamada | O texto que é apresentado acima o valor da chamada. |
| Nome da série |O valor da propriedade séries a ser utilizado como o valor da chamada. Se não for fornecido nenhum série, todos os registos da consulta são utilizados. |
| Operação |A operação é efetuada a propriedade value para resumi-lo como um valor único para a chamada.<ul><li>Média: A média dos valores de todos os registos.</li><li>Contagem: Contagem de todos os registos que são devolvidos pela consulta.</li><li>Última de exemplo: O valor do último intervalo de que está incluído no gráfico.</li><li>Máx.: O valor máximo de intervalos que estão incluídos no gráfico.</li><li>Mínimo: O valor mínimo de intervalos que estão incluídos no gráfico.</li><li>Soma: A soma dos valores de todos os registos.</li></ul> |
| **Gráfico de linhas** |**> Eixo y** |
| Utilizar escala logarítmica |Selecione esta ligação para utilizar uma escala logarítmica para o eixo y. |
| Unidades |Especifique as unidades para os valores devem ser devolvidos pela consulta. Estas informações são utilizadas para apresentar gráfico rótulos que indicam os tipos de valor e, opcionalmente, para converter os valores. O *unidade* tipo Especifica a categoria da unidade e define disponíveis *unidade atual* tipo de valores. Se selecionar um valor na *converter*, os valores numéricos são convertidos do *unidade atual* escreva para a *converter* tipo. |
| Etiqueta personalizada |O texto que é apresentado para o eixo y junto a etiqueta para o *unidade* tipo. Não se for especificada nenhuma etiqueta, apenas o *unidade* tipo é apresentado. |
| **Avançadas** |**> Verificação de-fluxo de dados** |
| Ativado |Selecione esta ligação se a verificação de fluxo de dados deve ser ativada para o mosaico. Esta abordagem fornece uma mensagem alternativa se os dados estão disponíveis. Normalmente utiliza a abordagem para fornecer uma mensagem durante o período de temporário quando a vista está instalada e os dados ficam disponíveis. |
| Consulta |A consulta que é executada para determinar se os dados estão disponíveis para a vista. Se a consulta devolve não existem resultados, é apresentada uma mensagem em vez do valor da consulta principal. |
| Mensagem |A mensagem é apresentada se a consulta de verificação de fluxo de dados devolver sem dados. Não se fornecer nenhuma mensagem, um *efetuar avaliação* é apresentada a mensagem de estado. |


## <a name="two-timelines-tile"></a>Mosaico de duas linhas cronológicas
O **duas linhas cronológicas** mosaico apresenta os resultados das duas consultas de registo ao longo do tempo como gráficos de colunas. É apresentada uma chamada para cada série. 

![Mosaico de duas linhas cronológicas](media/log-analytics-view-designer/tile-two-timelines.png)

| Definição | Descrição |
|:--- |:--- |
| Nome |O texto que é apresentado na parte superior do mosaico. |
| Descrição |O texto que é apresentado sob o nome do mosaico. |
| Gráfico primeiro | |
| Legenda |O texto que é apresentado em chamada para a série primeiro. |
| Cor |A cor que é utilizada para as colunas da série primeiro. |
| Consulta de gráfico |A consulta que é executada para a série primeiro. A contagem de registos ao longo de cada intervalo de tempo é representada pelas colunas de gráfico. |
| Operação |A operação é efetuada a propriedade value para resumi-lo como um valor único para a chamada.<ul><li>Média: A média dos valores de todos os registos.</li><li>Contagem: Contagem de todos os registos que são devolvidos pela consulta.</li><li>Última de exemplo: O valor do último intervalo de que está incluído no gráfico.</li><li>Máx.: O valor máximo de intervalos que estão incluídos no gráfico.</li></ul> |
| **Segundo gráfico** | |
| Legenda |O texto que é apresentado em chamada para a série segundo. |
| Cor |A cor que é utilizada para as colunas na série de segundo. |
| Consulta de gráfico |A consulta que é executada para a série segundo. A contagem de registos ao longo de cada intervalo de tempo é representada pelas colunas de gráfico. |
| Operação |A operação é efetuada a propriedade value para resumi-lo como um valor único para a chamada.<ul><li>Média: A média dos valores de todos os registos.</li><li>Contagem: Contagem de todos os registos que são devolvidos pela consulta.</li><li>Última de exemplo: O valor do último intervalo de que está incluído no gráfico.</li><li>Máx.: O valor máximo de intervalos que estão incluídos no gráfico. |
| **Avançadas** |**> Verificação de-fluxo de dados** |
| Ativado |Selecione esta ligação se a verificação de fluxo de dados deve ser ativada para o mosaico. Esta abordagem fornece uma mensagem alternativa se os dados estão disponíveis. Normalmente utiliza a abordagem para fornecer uma mensagem durante o período de temporário quando a vista está instalada e os dados ficam disponíveis. |
| Consulta |A consulta que é executada para determinar se os dados estão disponíveis para a vista. Se a consulta devolve não existem resultados, é apresentada uma mensagem em vez do valor da consulta principal. |
| Mensagem |A mensagem é apresentada se a consulta de verificação de fluxo de dados devolver sem dados. Não se fornecer nenhuma mensagem, um *efetuar avaliação* é apresentada a mensagem de estado. |


## <a name="next-steps"></a>Passos Seguintes
* Saiba mais sobre [pesquisas de registo](log-analytics-log-searches.md) para suportar as consultas de mosaicos.
* Adicionar [partes de visualização](log-analytics-view-designer-parts.md) à vista personalizada.
