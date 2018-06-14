---
title: Um guia de referência para as partes do estruturador de vistas no Log Analytics do Azure | Microsoft Docs
description: Ao utilizar o estruturador de vistas da análise de registos, pode criar vistas personalizadas no portal do Azure que apresentam uma variedade de visualizações de dados na sua área de trabalho de análise de registos. Este artigo é um guia de referência para as definições para as peças de visualização estão disponíveis no seu vistas personalizadas.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: 5718d620-b96e-4d33-8616-e127ee9379c4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: bwren
ms.openlocfilehash: a2573eef3c90c1840c0d53b2f8aa2cfe2d3a7242
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
ms.locfileid: "29949536"
---
# <a name="reference-guide-to-view-designer-visualization-parts-in-log-analytics"></a>Guia de referência para as partes de visualização do estruturador de vistas na análise de registos
Ao utilizar o estruturador de vistas no Log Analytics do Azure, pode criar vistas personalizadas no portal do Azure que apresentam uma variedade de visualizações de dados da sua área de trabalho de análise de registos. Este artigo é um guia de referência para as definições para as peças de visualização estão disponíveis no seu vistas personalizadas.

Para obter mais informações sobre o estruturador de vistas, consulte:

* [Ver Designer](log-analytics-view-designer.md): fornece uma descrição geral do estruturador de vistas e procedimentos para criar e editar vistas personalizadas.
* [Mosaico referência](log-analytics-view-designer-tiles.md): fornece uma referência para as definições para cada mosaico disponível no seu vistas personalizadas.


Os tipos de mosaico do estruturador de vistas disponíveis são descritos na tabela seguinte:

| Tipo de vista | Descrição |
|:--- |:--- |
| [Lista de consultas](#list-of-queries-part) |Mostra uma lista de consultas de pesquisa de registo. Pode selecionar cada consulta para ver os respetivos resultados. |
| [Número e a lista](#number-and-list-part) |O cabeçalho apresenta um número único que mostra uma contagem de registos por uma consulta de pesquisa de registo. A lista apresenta os resultados de dez principais de uma consulta, com um gráfico que indica o valor relativo de uma coluna numérica ou respetivas alterações ao longo do tempo. |
| [Dois números e lista](#two-numbers-and-list-part) |O cabeçalho apresenta dois números que mostrem contagens de registos de consultas de pesquisa de registo separados. A lista apresenta os resultados de dez principais de uma consulta, com um gráfico que indica o valor relativo de uma coluna numérica ou respetivas alterações ao longo do tempo. |
| [Anel e lista](#donut-and-list-part) |O cabeçalho apresenta um número único que resume uma coluna de valor de uma consulta de registo. O anel graficamente apresenta resultados dos registos de três principais. |
| [Duas linhas cronológicas e lista](#two-timelines-and-list-part) |O cabeçalho apresenta os resultados das duas consultas de registo ao longo do tempo como gráficos de colunas, com uma chamada que apresenta um número único que resume uma coluna de valor de uma consulta de registo. A lista apresenta os resultados de dez principais de uma consulta, com um gráfico que indica o valor relativo de uma coluna numérica ou respetivas alterações ao longo do tempo. |
| [Informações](#information-part) |Apresenta o cabeçalho de texto estático e uma ligação opcional. A lista apresenta um ou mais itens com um título estático e texto. |
| [Gráfico de linhas, chamada e lista](#line-chart-callout-and-list-part) |O cabeçalho apresenta um gráfico de linhas com várias séries por uma consulta de registo ao longo do tempo e uma chamada com um valor resumido. A lista apresenta os resultados de dez principais de uma consulta, com um gráfico que indica o valor relativo de uma coluna numérica ou respetivas alterações ao longo do tempo. |
| [Gráfico de linhas e de lista](#line-chart-and-list-part) |O cabeçalho apresenta um gráfico de linhas com várias séries por uma consulta de registo ao longo do tempo. A lista apresenta os resultados de dez principais de uma consulta, com um gráfico que indica o valor relativo de uma coluna numérica ou respetivas alterações ao longo do tempo. |
| [Pilha da peça de gráficos de linha](#stack-of-line-charts-part) |Apresenta os três separado gráficos, com várias séries por uma consulta de registo ao longo do tempo. |

As secções seguintes descrevem os tipos de mosaico e as respetivas propriedades em detalhe.

## <a name="list-of-queries-part"></a>Lista de parte de consultas
A lista de parte de consultas apresenta uma lista de consultas de pesquisa de registo. Pode selecionar cada consulta para ver os respetivos resultados. A vista inclui uma única consulta por predefinição e pode selecionar **+ consulta** adicionar consultas adicionais.

![Lista de vista de consultas](media/log-analytics-view-designer/view-list-queries.png)

| Definição | Descrição |
|:--- |:--- |
| **Geral** | |
| Cargo |O texto que é apresentado na parte superior da vista. |
| Novo grupo |Selecione esta hiperligação para criar um novo grupo na vista, a partir da vista atual. |
| Filtros previamente selecionados |Uma lista delimitada por vírgulas de propriedades para incluir no painel esquerdo do filtro ao selecionar uma consulta. |
| Modo de composição |A vista inicial que é apresentada quando a consulta está selecionada. Pode selecionar qualquer vistas disponíveis após abrirem a consulta. |
| **Consultas** | |
| Consulta de pesquisa |A consulta para ser executada. |
| Nome amigável | O nome descritivo que é apresentado. |

## <a name="number-and-list-part"></a>Número e lista parte
O cabeçalho apresenta um número único que mostra uma contagem de registos por uma consulta de pesquisa de registo. A lista apresenta os resultados de dez principais de uma consulta, com um gráfico que indica o valor relativo de uma coluna numérica ou respetivas alterações ao longo do tempo.

![Lista de vista de consultas](media/log-analytics-view-designer/view-number-list.png)

| Definição | Descrição |
|:--- |:--- |
| **Geral** | |
| Título de grupo |O texto que é apresentado na parte superior da vista. |
| Novo grupo |Selecione esta hiperligação para criar um novo grupo na vista, a partir da vista atual. |
| Ícone |O ficheiro de imagem que é apresentado junto ao resultado no cabeçalho. |
| Ícone de utilização |Selecione esta hiperligação para o ícone de apresentação. |
| **Title** (Título) | |
| Legenda |O texto que é apresentado na parte superior do cabeçalho. |
| Consulta |A consulta para o cabeçalho. É apresentada a contagem de registos que são devolvidos pela consulta. |
| Clique em através da navegação | Ação tomada quando clica no cabeçalho.  Para obter mais informações, consulte [definições comuns](#click-through-navigation). |
| **List** | |
| Consulta |A consulta para a lista. São apresentadas as primeiras duas propriedades dos registos primeiro dez nos resultados da. A primeira propriedade é um valor de texto e a segunda propriedade é um valor numérico. Barras são criadas automaticamente que são baseadas no valor da coluna numérica relativo.<br><br>Utilize o `Sort` comando da consulta para ordenar os registos na lista. Para executar a consulta e devolver todos os registos, pode selecionar **ver todos os**. |
| Ocultar gráfico |Selecione esta hiperligação para desativar o gráfico no lado direito da coluna numérica. |
| Ativar sparklines |Selecione esta hiperligação para apresentar um gráfico sparkline em vez de uma barra horizontal. Para obter mais informações, consulte [definições comuns](#sparklines). |
| Cor |A cor de barras ou sparklines. |
| Nome e valor de separação |O delimitador de caráter a utilizar para analisar a propriedade de texto em vários valores. Para obter mais informações, consulte [definições comuns](#sparklines). |
| Clique em através da navegação | Ação tomada quando clica no item na lista.  Para obter mais informações, consulte [definições comuns](#click-through-navigation). |
| **List** |**> Títulos de coluna** |
| Nome |O texto que é apresentado na parte superior da coluna primeiro. |
| Valor |O texto que é apresentado na parte superior da segunda coluna. |
| **List** |**> Limiares** |
| Ativar os limiares |Selecione esta ligação para ativar os limiares. Para obter mais informações, consulte [definições comuns](#thresholds). |

## <a name="two-numbers-and-list-part"></a>Dois números e parte da lista
O cabeçalho tem dois números que mostra uma contagem de registos de consultas de pesquisa de registo separados. A lista apresenta os resultados de dez principais de uma consulta, com um gráfico que indica o valor relativo de uma coluna numérica ou respetivas alterações ao longo do tempo.

![& Vista de lista de dois números](media/log-analytics-view-designer/view-two-numbers-list.png)

| Definição | Descrição |
|:--- |:--- |
| **Geral** | |
| Título de grupo |O texto que é apresentado na parte superior da vista. |
| Novo grupo |Selecione esta hiperligação para criar um novo grupo na vista, a partir da vista atual. |
| Ícone |O ficheiro de imagem que é apresentado junto ao resultado no cabeçalho. |
| Ícone de utilização |Selecione esta hiperligação para o ícone de apresentação. |
| **Navegação de título** | |
| Clique em através da navegação | Ação tomada quando clica no cabeçalho.  Para obter mais informações, consulte [definições comuns](#click-through-navigation). |
| **Title** (Título) | |
| Legenda |O texto que é apresentado na parte superior do cabeçalho. |
| Consulta |A consulta para o cabeçalho. É apresentada a contagem de registos que são devolvidos pela consulta. |
| **List** | |
| Consulta |A consulta para a lista. São apresentadas as primeiras duas propriedades dos registos primeiro dez nos resultados da. A primeira propriedade é um valor de texto e a segunda propriedade é um valor numérico. Barras são automaticamente criadas com base no valor da coluna numérica relativo.<br><br>Utilize o `Sort` comando da consulta para ordenar os registos na lista. Para executar a consulta e devolver todos os registos, pode selecionar **ver todos os**. |
| Ocultar gráfico |Selecione esta hiperligação para desativar o gráfico no lado direito da coluna numérica. |
| Ativar sparklines |Selecione esta hiperligação para apresentar um gráfico sparkline em vez de uma barra horizontal. Para obter mais informações, consulte [definições comuns](#sparklines). |
| Cor |A cor de barras ou sparklines. |
| Operação |A operação de efetuar para o gráfico sparkline. Para obter mais informações, consulte [definições comuns](#sparklines). |
| Nome e valor de separação |O delimitador de caráter a utilizar para analisar a propriedade de texto em vários valores. Para obter mais informações, consulte [definições comuns](#sparklines). |
| Clique em através da navegação | Ação tomada quando clica no item na lista.  Para obter mais informações, consulte [definições comuns](#click-through-navigation). |
| **List** |**> Títulos de coluna** |
| Nome |O texto que é apresentado na parte superior da coluna primeiro. |
| Valor |O texto que é apresentado na parte superior da segunda coluna. |
| **List** |**> Limiares** |
| Ativar os limiares |Selecione esta ligação para ativar os limiares. Para obter mais informações, consulte [definições comuns](#thresholds). |

## <a name="donut-and-list-part"></a>Parte da lista e de anel
O cabeçalho apresenta um número único que resume uma coluna de valor de uma consulta de registo. O anel graficamente apresenta resultados dos registos de três principais.

![Vista de lista e de anel](media/log-analytics-view-designer/view-donut-list.png)

| Definição | Descrição |
|:--- |:--- |
| **Geral** | |
| Título de grupo |O texto que é apresentado na parte superior do mosaico. |
| Novo grupo |Selecione esta hiperligação para criar um novo grupo na vista, a partir da vista atual. |
| Ícone |O ficheiro de imagem que é apresentado junto ao resultado no cabeçalho. |
| Ícone de utilização |Selecione esta hiperligação para o ícone de apresentação. |
| **Cabeçalho** | |
| Cargo |O texto que é apresentado na parte superior do cabeçalho. |
| Subtítulo |O texto que é apresentado sob o título na parte superior do cabeçalho. |
| **anel** | |
| Consulta |A consulta para o anel. A primeira propriedade é um valor de texto e a segunda propriedade é um valor numérico. |
| Clique em através da navegação | Ação tomada quando clica no cabeçalho.  Para obter mais informações, consulte [definições comuns](#click-through-navigation). |
| **anel** |**> Center** |
| Texto |O texto que é apresentado sob o valor de anel. |
| Operação |A operação para efetuar a propriedade de valor para resumi-lo como um valor único.<ul><li>Soma: Adiciona os valores de todos os registos.</li><li>Percentagem: O rácio dos registos devolvido pelos valores na **resultar valores utilizados na operação de center** para o total de registos na consulta.</li></ul> |
| Valores de resultado utilizados na operação do System center |Opcionalmente, selecione o sinal de adição (+) para adicionar um ou mais valores. Os resultados da consulta estão limitados a registos com os valores de propriedade que especificar. Se não existem valores são adicionados, todos os registos estão incluídos na consulta. |
| **Opções adicionais** |**> Cores** |
| Cor 1<br>Cor 2<br>Cor 3 |Selecione a cor para cada um dos valores que são apresentados no anel. |
| **Opções adicionais** |**> Mapeamento de cor avançadas** |
| Valor do campo |Escreva o nome de um campo para apresentá-la como uma cor diferente se está incluído no anel. |
| Cor |Selecione a cor para o campo exclusivo. |
| **List** | |
| Consulta |A consulta para a lista. É apresentada a contagem de registos que são devolvidos pela consulta. |
| Ocultar gráfico |Selecione esta hiperligação para desativar o gráfico no lado direito da coluna numérica. |
| Ativar sparklines |Selecione esta hiperligação para apresentar um gráfico sparkline em vez de uma barra horizontal. Para obter mais informações, consulte [definições comuns](#sparklines). |
| Cor |A cor de barras ou sparklines. |
| Operação |A operação de efetuar para o gráfico sparkline. Para obter mais informações, consulte [definições comuns](#sparklines). |
| Nome e valor de separação |O delimitador de caráter a utilizar para analisar a propriedade de texto em vários valores. Para obter mais informações, consulte [definições comuns](#sparklines). |
| Clique em através da navegação | Ação tomada quando clica no item na lista.  Para obter mais informações, consulte [definições comuns](#click-through-navigation). |
| **List** |**> Títulos de coluna** |
| Nome |O texto que é apresentado na parte superior da coluna primeiro. |
| Valor |O texto que é apresentado na parte superior da segunda coluna. |
| **List** |**> Limiares** |
| Ativar os limiares |Selecione esta ligação para ativar os limiares. Para obter mais informações, consulte [definições comuns](#thresholds). |

## <a name="two-timelines-and-list-part"></a>Dois parte linhas cronológicas e lista
O cabeçalho apresenta os resultados das duas consultas de registo ao longo do tempo como gráficos de colunas, com uma chamada que apresenta um número único que resume uma coluna de valor de uma consulta de registo. A lista apresenta os resultados de dez principais de uma consulta, com um gráfico que indica o valor relativo de uma coluna numérica ou respetivas alterações ao longo do tempo.

![Duas linhas cronológicas e lista de vista](media/log-analytics-view-designer/view-two-timelines-list.png)

| Definição | Descrição |
|:--- |:--- |
| **Geral** | |
| Título de grupo |O texto que é apresentado na parte superior do mosaico. |
| Novo grupo |Selecione esta hiperligação para criar um novo grupo na vista, a partir da vista atual. |
| Ícone |O ficheiro de imagem que é apresentado junto ao resultado no cabeçalho. |
| Ícone de utilização |Selecione esta hiperligação para o ícone de apresentação. |
| **Navegação de título** | |
| Clique em através da navegação | Ação tomada quando clica no cabeçalho.  Para obter mais informações, consulte [definições comuns](#click-through-navigation). |
| **Primeiro gráfico<br>segundo gráfico** | |
| Legenda |O texto que é apresentado em chamada para a série primeiro. |
| Cor |A cor a utilizar para as colunas da série. |
| Consulta |A consulta para ser executada para a série primeiro. A contagem de registos ao longo de cada intervalo de tempo é representada pelas colunas de gráfico. |
| Operação |A operação para efetuar a propriedade de valor para resumi-lo como um valor único para a chamada.<ul><li>Soma: A soma dos valores de todos os registos.</li><li>Média: A média dos valores de todos os registos.</li><li>Última de exemplo: O valor no último intervalo de que está incluído no gráfico.</li><li>Primeiro exemplo: O valor do primeiro intervalo de que está incluído no gráfico.</li><li>Contagem: Contagem de todos os registos que são devolvidos pela consulta.</li></ul> |
| **List** | |
| Consulta |A consulta para a lista. É apresentada a contagem de registos que são devolvidos pela consulta. |
| Ocultar gráfico |Selecione esta hiperligação para desativar o gráfico no lado direito da coluna numérica. |
| Ativar sparklines |Selecione esta hiperligação para apresentar um gráfico sparkline em vez de uma barra horizontal. Para obter mais informações, consulte [definições comuns](#sparklines). |
| Cor |A cor de barras ou sparklines. |
| Operação |A operação de efetuar para o gráfico sparkline. Para obter mais informações, consulte [definições comuns](#sparklines). |
| Clique em através da navegação | Ação tomada quando clica no item na lista.  Para obter mais informações, consulte [definições comuns](#click-through-navigation). |
| **List** |**> Títulos de coluna** |
| Nome |O texto que é apresentado na parte superior da coluna primeiro. |
| Valor |O texto que é apresentado na parte superior da segunda coluna. |
| **List** |**> Limiares** |
| Ativar os limiares |Selecione esta ligação para ativar os limiares. Para obter mais informações, consulte [definições comuns](#thresholds). |

## <a name="information-part"></a>Parte de informações
Apresenta o cabeçalho de texto estático e uma ligação opcional. A lista apresenta um ou mais itens com um título estático e texto.

![Vista de informações](media/log-analytics-view-designer/view-information.png)

| Definição | Descrição |
|:--- |:--- |
| **Geral** | |
| Título de grupo |O texto que é apresentado na parte superior do mosaico. |
| Novo grupo |Selecione esta hiperligação para criar um novo grupo na vista, a partir da vista atual. |
| Cor |A cor de fundo do cabeçalho. |
| **Cabeçalho** | |
| Imagem |O ficheiro de imagem que é apresentado no cabeçalho. |
| Etiqueta |O texto que é apresentado no cabeçalho. |
| **Cabeçalho** |**> Link** |
| Etiqueta |O texto da ligação. |
| Url |O Url para a ligação. |
| **Itens de informações** | |
| Cargo |O texto que é apresentado para o título de cada item. |
| Conteúdo |O texto que é apresentado para cada item. |

## <a name="line-chart-callout-and-list-part"></a>Gráfico de linhas, chamada e parte da lista
O cabeçalho apresenta um gráfico de linhas com várias séries por uma consulta de registo ao longo do tempo e uma chamada com um valor resumido. A lista apresenta os resultados de dez principais de uma consulta, com um gráfico que indica o valor relativo de uma coluna numérica ou respetivas alterações ao longo do tempo.

![Gráfico de linhas, chamada e vista de lista](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| Definição | Descrição |
|:--- |:--- |
| **Geral** | |
| Título de grupo |O texto que é apresentado na parte superior do mosaico. |
| Novo grupo |Selecione esta hiperligação para criar um novo grupo na vista, a partir da vista atual. |
| Ícone |O ficheiro de imagem que é apresentado junto ao resultado no cabeçalho. |
| Ícone de utilização |Selecione esta hiperligação para o ícone de apresentação. |
| **Cabeçalho** | |
| Cargo |O texto que é apresentado na parte superior do cabeçalho. |
| Subtítulo |O texto que é apresentado sob o título na parte superior do cabeçalho. |
| **Gráfico de linhas** | |
| Consulta |A consulta para o gráfico de linhas. A primeira propriedade é um valor de texto e a segunda propriedade é um valor numérico. Esta consulta normalmente utiliza o *medidas* palavra-chave para resumir os resultados. Se a consulta utiliza a *intervalo* palavra-chave, o eixo x do gráfico utiliza este intervalo de tempo. Se a consulta não inclui o *intervalo* palavra-chave, os intervalos de hora a hora do eixo x utiliza. |
| Clique em através da navegação | Ação tomada quando clica no cabeçalho.  Para obter mais informações, consulte [definições comuns](#click-through-navigation). |
| **Gráfico de linhas** |**> Chamada** |
| Título da chamada |O texto que é apresentado acima o valor da chamada. |
| Nome da série |Valor da propriedade para a série a utilizar para o valor da chamada. Se não for fornecido nenhum série, todos os registos da consulta são utilizados. |
| Operação |A operação para efetuar a propriedade de valor para resumi-lo como um valor único para a chamada.<ul><li>Média: A média dos valores de todos os registos.</li><li>Contagem: Contagem de todos os registos que são devolvidos pela consulta.</li><li>Última de exemplo: O valor no último intervalo de que está incluído no gráfico.</li><li>Máx.: O valor máximo de intervalos que estão incluídos no gráfico.</li><li>Mínimo: O valor mínimo de intervalos que estão incluídos no gráfico.</li><li>Soma: A soma dos valores de todos os registos.</li></ul> |
| **Gráfico de linhas** |**> Eixo y** |
| Utilizar escala logarítmica |Selecione esta ligação para utilizar uma escala logarítmica para o eixo y. |
| Unidades |Especifique as unidades para os valores devem ser devolvidos pela consulta. Estas informações são utilizadas para apresentar gráfico rótulos que indicam os tipos de valor e, opcionalmente, para converter os valores. O *unidade* tipo Especifica a categoria da unidade e define disponíveis *unidade atual* tipo de valores. Se selecionar um valor na *converter*, os valores numéricos são convertidos do *unidade atual* escreva para a *converter* tipo. |
| Etiqueta personalizada |O texto que é apresentado para o eixo y junto a etiqueta para o *unidade* tipo. Não se for especificada nenhuma etiqueta, apenas o *unidade* tipo é apresentado. |
| **List** | |
| Consulta |A consulta para a lista. É apresentada a contagem de registos que são devolvidos pela consulta. |
| Ocultar gráfico |Selecione esta hiperligação para desativar o gráfico no lado direito da coluna numérica. |
| Ativar sparklines |Selecione esta hiperligação para apresentar um gráfico sparkline em vez de uma barra horizontal. Para obter mais informações, consulte [definições comuns](#sparklines). |
| Cor |A cor de barras ou sparklines. |
| Operação |A operação de efetuar para o gráfico sparkline. Para obter mais informações, consulte [definições comuns](#sparklines). |
| Nome e valor de separação |O delimitador de caráter a utilizar para analisar a propriedade de texto em vários valores. Para obter mais informações, consulte [definições comuns](#sparklines). |
| Clique em através da navegação | Ação tomada quando clica no item na lista.  Para obter mais informações, consulte [definições comuns](#click-through-navigation). |
| **List** |**> Títulos de coluna** |
| Nome |O texto que é apresentado na parte superior da coluna primeiro. |
| Valor |O texto que é apresentado na parte superior da segunda coluna. |
| **List** |**> Limiares** |
| Ativar os limiares |Selecione esta ligação para ativar os limiares. Para obter mais informações, consulte [definições comuns](#thresholds). |

## <a name="line-chart-and-list-part"></a>Parte de lista e gráfico de linha
O cabeçalho apresenta um gráfico de linhas com várias séries por uma consulta de registo ao longo do tempo. A lista apresenta os resultados de dez principais de uma consulta, com um gráfico que indica o valor relativo de uma coluna numérica ou respetivas alterações ao longo do tempo.

![Vista de lista e gráfico de linha](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| Definição | Descrição |
|:--- |:--- |
| **Geral** | |
| Título de grupo |O texto que é apresentado na parte superior do mosaico. |
| Novo grupo |Selecione esta hiperligação para criar um novo grupo na vista, a partir da vista atual. |
| Ícone |O ficheiro de imagem que é apresentado junto ao resultado no cabeçalho. |
| Ícone de utilização |Selecione esta hiperligação para o ícone de apresentação. |
| **Cabeçalho** | |
| Cargo |O texto que é apresentado na parte superior do cabeçalho. |
| Subtítulo |O texto que é apresentado sob o título na parte superior do cabeçalho. |
| **Gráfico de linhas** | |
| Consulta |A consulta para o gráfico de linhas. A primeira propriedade é um valor de texto e a segunda propriedade é um valor numérico. Esta consulta normalmente utiliza o *medidas* palavra-chave para resumir os resultados. Se a consulta utiliza a *intervalo* palavra-chave, o eixo x do gráfico utiliza este intervalo de tempo. Se a consulta não inclui o *intervalo* palavra-chave, os intervalos de hora a hora do eixo x utiliza. |
| Clique em através da navegação | Ação tomada quando clica no cabeçalho.  Para obter mais informações, consulte [definições comuns](#click-through-navigation). |
| **Gráfico de linhas** |**> Eixo y** |
| Utilizar escala logarítmica |Selecione esta ligação para utilizar uma escala logarítmica para o eixo y. |
| Unidades |Especifique as unidades para os valores devem ser devolvidos pela consulta. Estas informações são utilizadas para apresentar gráfico rótulos que indicam os tipos de valor e, opcionalmente, para converter os valores. O *unidade* tipo Especifica a categoria da unidade e define disponíveis *unidade atual* tipo de valores. Se selecionar um valor na *converter*, os valores numéricos são convertidos do *unidade atual* escreva para a *converter* tipo. |
| Etiqueta personalizada |O texto que é apresentado para o eixo y junto a etiqueta para o *unidade* tipo. Não se for especificada nenhuma etiqueta, apenas o *unidade* tipo é apresentado. |
| **List** | |
| Consulta |A consulta para a lista. É apresentada a contagem de registos que são devolvidos pela consulta. |
| Ocultar gráfico |Selecione esta hiperligação para desativar o gráfico no lado direito da coluna numérica. |
| Ativar sparklines |Selecione esta hiperligação para apresentar um gráfico sparkline em vez de uma barra horizontal. Para obter mais informações, consulte [definições comuns](#sparklines). |
| Cor |A cor de barras ou sparklines. |
| Operação |A operação de efetuar para o gráfico sparkline. Para obter mais informações, consulte [definições comuns](#sparklines). |
| Nome e valor de separação |O delimitador de caráter a utilizar para analisar a propriedade de texto em vários valores. Para obter mais informações, consulte [definições comuns](#sparklines). |
| Clique em através da navegação | Ação tomada quando clica no item na lista.  Para obter mais informações, consulte [definições comuns](#click-through-navigation). |
| **List** |**> Títulos de coluna** |
| Nome |O texto que é apresentado na parte superior da coluna primeiro. |
| Valor |O texto que é apresentado na parte superior da segunda coluna. |
| **List** |**> Limiares** |
| Ativar os limiares |Selecione esta ligação para ativar os limiares. Para obter mais informações, consulte [definições comuns](#thresholds). |

## <a name="stack-of-line-charts-part"></a>Pilha da peça de gráficos de linha
A pilha do gráfico de linhas apresenta os três separado gráficos, com várias séries por uma consulta de registo ao longo do tempo, conforme mostrado aqui:

![Pilha de gráficos de linha](media/log-analytics-view-designer/view-stack-line-charts.png)

| Definição | Descrição |
|:--- |:--- |
| **Geral** | |
| Título de grupo |O texto que é apresentado na parte superior do mosaico. |
| Novo grupo |Selecione esta hiperligação para criar um novo grupo na vista, a partir da vista atual. |
| Ícone |O ficheiro de imagem que é apresentado junto ao resultado no cabeçalho. |
| **Gráfico 1<br>gráfico 2<br>gráfico 3** |**> Cabeçalho** |
| Cargo |O texto que é apresentado na parte superior do gráfico. |
| Subtítulo |O texto que é apresentado sob o título na parte superior do gráfico. |
| **Gráfico 1<br>gráfico 2<br>gráfico 3** |**Gráfico de linhas** |
| Consulta |A consulta para o gráfico de linhas. A primeira propriedade é um valor de texto e a segunda propriedade é um valor numérico. Esta consulta normalmente utiliza o *medidas* palavra-chave para resumir os resultados. Se a consulta utiliza a *intervalo* palavra-chave, o eixo x do gráfico utiliza este intervalo de tempo. Se a consulta não inclui o *intervalo* palavra-chave, os intervalos de hora a hora do eixo x utiliza. |
| Clique em através da navegação | Ação tomada quando clica no cabeçalho.  Para obter mais informações, consulte [definições comuns](#click-through-navigation). |
| **Chart** |**> Eixo y** |
| Utilizar escala logarítmica |Selecione esta ligação para utilizar uma escala logarítmica para o eixo y. |
| Unidades |Especifique as unidades para os valores devem ser devolvidos pela consulta. Estas informações são utilizadas para apresentar gráfico rótulos que indicam os tipos de valor e, opcionalmente, para converter os valores. O *unidade* tipo Especifica a categoria da unidade e define disponíveis *unidade atual* tipo de valores. Se selecionar um valor na *converter*, os valores numéricos são convertidos do *unidade atual* escreva para a *converter* tipo. |
| Etiqueta personalizada |O texto que é apresentado para o eixo y junto a etiqueta para o *unidade* tipo. Não se for especificada nenhuma etiqueta, apenas o *unidade* tipo é apresentado. |

## <a name="common-settings"></a>Definições comuns
As secções seguintes descrevem as definições que são comuns a várias partes de visualização.

### <a name="name-value-separator"></a>Nome e valor de separação
O separador de nome e valor é o delimitador de caráter a utilizar para analisar a propriedade de texto por uma consulta de lista para valores múltiplos. Se especificar um delimitador, pode fornecer nomes para cada campo, separado pelo delimitador no mesmo a **nome** caixa.

Por exemplo, considere uma propriedade denominada *localização* que incluídos como valores *Redmond edifício 41* e *Bellevue edifício 12*. Pode especificar um travessão (-) para o separador de nome e valor e *cidade edifício* para o nome. Esta abordagem analisa a cada valor em duas propriedades chamadas *Cidade* e *edifício*.

### <a name="click-through-navigation"></a>Clique em-através da navegação
Clique em através da navegação define que ação será tomada quando clica num cabeçalho ou uma lista item numa vista.  Se esta ação irá abrir uma consulta a [portal de registo de pesquisa](log-analytics-log-search-portals.md#log-search) ou inicie outra vista.

A tabela seguinte descreve as definições de navegação clique-through.

| Definição           | Descrição |
|:--|:--|
| Pesquisa de registo (automático) | Pesquisa de registo para ser executada quando selecciona um item de cabeçalho.  Este é a mesmo pesquisa de registo, o item com base no.
| Pesquisa de Registos        | Pesquisa de registo para ser executada quando selecciona um item numa lista.  Escreva a consulta para o **consulta navegação** caixa.   Utilize *{item selecionado}* para incluir a sintaxe para o item que o utilizador selecionado.  Por exemplo, se a consulta tem uma coluna chamada *computador* e a consulta de navegação *{item selecionado}*, uma consulta, tais como *computador = "MyComputer"* é executado quando seleciona um computador. Se a consulta de navegação é *tipo = eventos {item selecionado}*, a consulta *tipo = evento computador = "MyComputer"* é executado. |
| Vista              | Vista abrir ao selecionar um item de cabeçalho ou de um item numa lista.  Selecione o nome de uma vista na sua área de trabalho a **nome da vista** caixa. |



### <a name="sparklines"></a>Sparklines
Um gráfico sparkline é um gráfico de linhas pequenas que ilustra o valor de uma entrada da lista ao longo do tempo. Para visualização as partes com uma lista, pode selecionar se pretende apresentar uma barra horizontal, que indica o valor relativo de uma coluna numérica ou um gráfico sparkline, que indica o valor ao longo do tempo.

A tabela seguinte descreve as definições para sparklines:

| Definição | Descrição |
|:--- |:--- |
| Ativar Sparklines |Selecione esta hiperligação para apresentar um gráfico sparkline em vez de uma barra horizontal. |
| Operação |Se sparklines estiver ativada, esta é a efetuar em cada propriedade na lista para calcular os valores para o gráfico sparkline da operação.<ul><li>Última de exemplo: O último valor para a série ao longo do intervalo de tempo.</li><li>Máx.: O valor máximo para a série ao longo do intervalo de tempo.</li><li>Mínimo: O valor mínimo para a série ao longo do intervalo de tempo.</li><li>Soma: A soma dos valores para a série ao longo do intervalo de tempo.</li><li>Resumo: Utiliza o mesmo `measure` comando como a consulta no cabeçalho.</li></ul> |

### <a name="thresholds"></a>Limiares
Ao utilizar os limiares, pode apresentar um ícone colorido junto a cada item numa lista. Limiares dão-lhe um indicador de visual rápido dos itens que exceder um valor específico ou enquadram-se dentro de um intervalo específico. Por exemplo, pode apresentar um ícone verde para itens com um valor aceitável, amarelo, se o valor for dentro de um intervalo que indica um aviso e vermelho, se exceder um valor de erro.

Quando ativar os limiares de uma parte, tem de especificar um ou mais limiares. Se o valor de um item é maior do que um valor de limiar e inferior ao valor de limiar seguinte, a cor para esse valor será utilizada. Se o item for superior ao valor de limiar mais elevado, é utilizada a cor do outro. 

Cada conjunto de limiar tem um limiar com um valor de **predefinido**. Esta é a cor que é definida se não existem outros valores for excedidos. Pode adicionar ou remover limiares selecionando o **adicionar** (+) ou **eliminar** (botão x).

A tabela seguinte descreve as definições de limiares:

| Definição | Descrição |
|:--- |:--- |
| Ativar os limiares |Selecione esta hiperligação para apresentar um ícone cor à esquerda de cada valor. O ícone indica o estado de funcionamento do valor em relação ao limiares especificados. |
| Nome |O nome do valor de limiar. |
| Limiar |O valor do limiar de. A cor do Estado de funcionamento para cada item de lista está definida para a cor do valor de limiar mais elevado, que é excedido pelo valor do item. Se não existem valores de limiar for excedidos, é utilizada uma cor predefinida. |
| Cor |A cor que indica o valor de limiar. |

## <a name="next-steps"></a>Passos Seguintes
* Saiba mais sobre [pesquisas de registo](log-analytics-log-searches.md) para suportar as consultas em partes de visualização.
