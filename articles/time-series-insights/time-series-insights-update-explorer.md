---
title: Visualize os dados na atualização do Explorador do Azure Time Series Insights | Documentos da Microsoft
description: Atualize o Azure Time Series Insights explorer
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/03/2018
ms.openlocfilehash: 48f3be2de2f23cef311994c603d50544b5d13a6e
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52879796"
---
# <a name="visualize-data-in-the-explorer-preview"></a>Visualizar dados no explorer (pré-visualização)

Este artigo descreve os recursos e as opções disponíveis dentro do Azure Time Series Insights (pré-visualização) [aplicação web de explorer](https://insights.timeseries.azure.com/preview/samples).

## <a name="prerequisites"></a>Pré-requisitos

Antes de utilizar o Explorador do Azure Time Series Insights (pré-visualização), faça o seguinte:

* Tem um ambiente de Time Series Insights aprovisionado. Saiba mais sobre o aprovisionamento de um ambiente do Time Series Insights aqui.
* Fornece acesso a dados para o ambiente do Time Series Insights que criou para a conta. Acesso pode ser fornecido para outras pessoas, bem como por conta própria.
* Adicione uma origem de evento para o ambiente do Time Series Insights para enviar dados para o ambiente.

## <a name="learn-about-the-azure-time-series-insights-preview-explorer"></a>Saiba mais sobre o Explorador do Azure Time Series Insights (pré-visualização)

  ![um Explorador][1]

O Explorador do Azure Time Series Insights (pré-visualização) é dividido em sete elementos:

1. Barra de navegação do TSI (pré-visualização) permite que alterne entre as páginas de análise e o modelo.
1. Árvore de hierarquia do TSI (pré-visualização) selecionar elementos de dados específicos para ser colocada em gráficos.
1. Série de tempo do TSI (pré-visualização) também mostra todos os seus elementos de dados atualmente selecionados.
1. Painel de gráfico do TSI (pré-visualização) mostra o gráfico de trabalho atual.
1. Linha cronológica do TSI (pré-visualização) permite-lhe modificar o intervalo de tempo de trabalho.
1. Barra de aplicativo do TSI (pré-visualização) contém as opções de gestão de utilizador (como inquilino atual) e permite-lhe alterar as definições de idioma e o tema.

## <a name="tsi-preview-environment-panel"></a>Painel de ambiente de TSI (pré-visualização)

O painel de ambiente apresenta todos os ambientes de TSI que tem acesso. Isto inclui uma lista dos ambientes de pay as you go (pré-visualização), bem como ambientes de S1/S2 (GA). Basta clicar o ambiente do TSI que pretende utilizar.

  ![Explorador de dois][2]

## <a name="time-series-insights-preview-navigation-menu"></a>Menu de navegação do Time Series Insights (pré-visualização)

  ![três Explorer][3]

O menu de navegação permite que alterne entre as aplicações de TSI:

* Analise – permite-lhe do gráfico e efetuar análises avançadas nos seus dados de série de tempo unmodeled ou modelados.

* Modele - lhe permite novos tipos de atualização TSI, hierarquias e instâncias ao seu modelo TSI.

## <a name="time-series-insights-update-model-authoring"></a>Criação do modelo de atualização do Time Series Insights

Esta aplicação dá-lhe a capacidade de executar operações CRUD em seu modelo de série de tempo.  

* Tipo de TSM - os tipos TSI permitem definir variáveis ou fórmulas para fazer cálculos, que estiverem associados uma determinada instância TSI. Um tipo pode ter uma ou mais variáveis.
* Hierarquia TSM - hierarquias são organizações sistemática dos seus dados. Hierarquias retratar as relações entre diferentes entidades nos seus dados do TSI.
* Instância TSM - instâncias são a série de tempo propriamente ditas. Na maioria dos casos, esta será a **DeviceID** ou **AssetID**, que é o identificador exclusivo do recurso no ambiente.

Para saber mais sobre o TSM, leia [modelos de série de vezes](./time-series-insights-update-tsm.md).

## <a name="time-series-insights-preview-model-search-panel"></a>Painel de pesquisa do Time Series Insights (pré-visualização) modelo

O painel de pesquisa de modelo permite-lhe facilmente procurar e navegue até a hierarquia TSM para localizar as instâncias de série de tempo específico que pretende apresentar no gráfico. Quando seleciona as instâncias, eles não só são adicionados ao gráfico atual, mas também são adicionados aos dados também.

  ![quatro Explorer][4]

## <a name="time-series-insights-preview-well"></a>Tempo Series Insights (pré-visualização) de bem

O também apresenta os campos de instância e outros metadados associados com instâncias de série de tempo selecionado. As caixas de verificação no lado direito permitem-lhe ocultar ou exibir instâncias específicas de um gráfico atual. Também pode remover elementos de dados específicos a partir dos seus dados atuais ao clicar no controle o x vermelho à direita do elemento.

  ![cinco Explorer][5]

Também pode destacar o painel de telemetria para obter uma melhor vista vertical dos elementos nos seus dados também.

  ![seis Explorer][6]

> [!NOTE]
> Se vir o ícone que se segue, a instância não tem quaisquer dados durante o período de tempo selecionado.
> Para corrigir o problema, pode aumentar o período de tempo selecionado e/ou confirmar que a instância está a enviar dados.

  ![sete de Explorer][7]

## <a name="time-series-insights-preview-chart"></a>Gráfico de Insights (pré-visualização) de série de tempo

O gráfico permite-lhe apresentar as instâncias de série de tempo como linhas. Pode fechar o painel de ambiente, o modelo de dados e o painel de controlo de span de tempo ao clicar em controles da web para aumentar o gráfico.

  ![Explorador de oito][8]

1. **Selecionar intervalo de datas** – o selecionado atualmente o intervalo de datas para o painel de gráfico, este controla quais elementos de dados estarão disponíveis para visualização.

1. **Ferramenta de controlo de deslize de intervalo de datas interna** - utilizar os dois controles de ponto final ao clicar e arrastá-los sobre o tempo pretendido span.

1. **Tempo o controlo de recolhimento span** -esse controle reduz e expande o editor de span painel de tempo.

1. **Controlo de formato do eixo y** – clique neste controlo para percorrer as opções de exibição disponíveis do eixo y. As opções de vista do eixo y disponíveis são:

    * `Default`  -cada linha tem um eixo y individual
    * `Stacked` – Isto permite-lhe várias linhas no mesmo eixo y de pilha, com o eixo y a alteração de dados com base na linha selecionada
    * `Shared` – Todos os dados do eixo y apresentados em conjunto

1. **Elemento de dados atual** – o selecionado atualmente o elemento de dados e os respetivos detalhes associados.

Podem desagregar ainda mais num setor de dados específicos por left-clicking um ponto de dados no gráfico atual ao premir o mouse e, em seguida, arrastando a área selecionada para o ponto final da sua preferência. Com o botão direito a área de cinzento, selecionada e clique em zoom, conforme mostrado abaixo. Também pode:

  ![Explorer e nove][9]

Depois de executar a ação de zoom, agora verá o conjunto de dados selecionado. Clique no controle de formato do eixo y para percorrer as três diferentes representações de eixo y dos seus dados do TSI.

  ![dez Explorer][10]

Aqui pode ver um exemplo de um eixo y partilhado.

  ![Explorer 11][11]

## <a name="time-series-insights-preview-time-editor-panel"></a>Painel de Editor em tempo de Insights (pré-visualização) de série de tempo

Ao trabalhar com TSI, primeiro irá selecionar um intervalo de tempo. O intervalo de tempo selecionado controlará o conjunto de dados que está disponível para a manipulação com os widgets de atualização do TSI. Os controles da web seguintes estão disponíveis na atualização do TSI para selecionar o seu intervalo de tempo de trabalho.

  ![Explorador de doze][12]

1. **Ferramenta de controlo de deslize de intervalo de datas interna** - utilizar os dois controles de ponto final ao clicar e arrastá-los sobre o tempo pretendido span. Este intervalo de "Data interna" serão restritos pelo controlo de deslize de intervalo "Data externa" referido abaixo.

1. Aumentar e diminuir data intervalo botões * * - aumentar ou diminuir o intervalo de tempo ao clicar em qualquer botão para o intervalo que pretende.

1. **Tempo o controlo de recolhimento span** -esse controle web permite que oculte todos os controles, exceto para a ferramenta de controlo de deslize de intervalo de datas interna.

1. **Controlo de deslize de intervalo de datas OUTER** -usando os controles de ponto final de selecionar o intervalo de datas externa, que estará disponível para o seu controle de intervalo "Data interna".

1. **Vezes rápidas pendente do intervalo de datas** -dá-lhe a capacidade de alternar rapidamente entre as seleções de span hora predefinidas, como os últimos 30 minutos, as últimas 12 horas, intervalo personalizado, etc. Alterar este valor também é alterado os intervalos de intervalo disponível discutidos na ferramenta de controlo de deslize de tamanho do intervalo, em seguida.

1. **Ferramenta de controlo de deslize de intervalo de tamanho** -a ferramenta de controlo de deslize de tamanho do intervalo permite-lhe ampliar e intervalos de reduzir durante o mesmo intervalo de tempo. Esta opção fornece um controle mais preciso de movimento entre grandes frações de tempo que mostram tendências uniformes para baixo para setores tão pequena como milissegundo, permitindo que veja reduções de alta resolução, granulares dos seus dados. Ponto de partida de predefinição do controlo de deslize está definida como a exibição ideal dos dados da sua seleção; balanceamento de resolução, velocidade de consulta e granularidade.

1. **Intervalo de e para o controle da web de datas** – com esse controle da web pode facilmente clicar e selecione a data pretendida e os intervalos de tempo. Também pode utilizar o controlo de alternar entre diferentes fusos horários. Depois de efetuar as alterações, gostaria de aplicar a sua atual espaço de trabalho clique em salvar botão.

  ![treze-Explorer][13]

## <a name="time-series-insights-preview-navigation-panel"></a>Painel de navegação do Time Series Insights (pré-visualização)

O painel de navegação de atualização do TSI fornece as seguintes funcionalidades:

  ![14-Explorer][14]

### <a name="current-session-share-link-control"></a>Controlo de sessão atual de ligação de partilha

  ![quinze Explorer][15]

Clique no controle de web de ligação dentro de um círculo para gerar um URL para guardar ou partilhar a sua sessão de trabalho do Azure TSI atual, que inclui:

* Intervalo de tempo atualmente selecionado
* Tamanho do intervalo selecionado
* Dados atualmente selecionados bem

### <a name="tenant-section"></a>Secção de inquilino

  ![16 de Explorer][16]

* Apresenta as informações da conta de início de sessão TSI atuais
* Permite que alternar entre os temas do TSI disponíveis.

### <a name="theme-selection"></a>Seleção de tema

O Azure TSI (pré-visualização) oferece suporte a dois temas:

* **Lite tema**: Este é o tema predefinido mostrado em todo este documento.
* **Tema escuro**: esta opção processa o explorer num tema escuro, conforme mostrado abaixo:

  ![Explorer seventeen][17]

Aqui, pode também alterar entre idiomas suportados.

## <a name="s1s2-environment-controls"></a>Controles de ambiente de S1/S2

### <a name="time-series-insights-preview-terms-panel"></a>Painel de termos do Time Series Insights (pré-visualização)

Esta secção aplica-se apenas a ambientes de S1/S2 existentes tentar utilizar o Explorador na interface do Usuário atualizada. Pode querer fazer isso para usar o produto de disponibilidade geral e atualizar (pré-visualização) em conjunto com um do outro. Podemos adicionar alguma funcionalidade de interface do Usuário existente para o explorer atualizado, mas sei que pode obter sempre a experiência de interface do Usuário completa para o ambiente de S1/S2 no Explorador de TSI existente.  

Em lugar da hierarquia, verá o painel de termos do TSI. É onde definimos consultas no seu ambiente. Ele dá-lhe a capacidade de filtrar os seus dados com base a utilizar um predicado.

  ![Explorador de dezoito][18]

O painel de Editor de termos do TSI (pré-visualização) precisa dos seguintes parâmetros

**Onde**: where cláusula permite-lhe filtrar rapidamente os seus eventos usando o conjunto de operandos listados abaixo. Se realizar uma pesquisa ao selecionar/clicar, o predicado serão atualizados automaticamente com base no que a pesquisa. Tipos de operando suportados incluem:

| Operação | Tipos suportados   | Notas |
| --- | --- | --- |
| `<`, `>`, `<=`, `>=` |    Período de tempo de Double, DateTime,  | |
| `=`, `!=`, `<>`   | Cadeia de caracteres, Bool, Double, DateTime, TimeSpan, nulo    |
| `IN` |    Cadeia de caracteres, Bool, Double, DateTime, TimeSpan, nulo |    Todos os operandos devem ser do mesmo tipo ou ser constante NULL. |
| `HAS` |   Cadeia |    Literais de cadeia de caracteres constante só são permitidas no lado direito. Cadeia de caracteres vazia e NULL não são permitidas. |

### <a name="examples-of-where-clauses"></a>Exemplos de onde as cláusulas

  ![dezenove-Explorer][19]

**Medida**: esta lista pendente mostra todas as colunas numéricas (**duplicatas**) que pode utilizar como elementos para o gráfico atual.

**Dividir por**: esta lista pendente mostra todas as colunas categóricas (cadeias de caracteres) no seu modelo disponível, que pode utilizar para agrupar os dados pelo. Pode adicionar até cinco termos para ver no mesmo eixo x. Introduza os parâmetros pretendidos e, em seguida, utilize o **adicionar** botão para adicionar um novo termo.

  ![vinte Explorer][20]

Pode ocultar e mostrar os elementos do painel de gráfico ao clicar no ícone de visível, conforme mostrado abaixo. Pode remover por completo consultas ao clicar o vermelho `X` mostrado abaixo.

  ![Explorer-vinte-um][21]

## <a name="next-steps"></a>Passos Seguintes

Leitura a [armazenamento do Azure TSI (pré-visualização) e de entrada](./time-series-insights-update-storage-ingress.md).

Leia sobre [modelação de dados](./time-series-insights-update-tsm.md).

Saiba mais sobre [diagnosticando e resolução de problemas](./time-series-insights-update-how-to-troubleshoot.md).

<!-- Images -->
[1]: media/v2-update-explorer/explorer-one.png
[2]: media/v2-update-explorer/explorer-two.png
[3]: media/v2-update-explorer/explorer-three.png
[4]: media/v2-update-explorer/explorer-four.png
[5]: media/v2-update-explorer/explorer-five.png
[6]: media/v2-update-explorer/explorer-six.png
[7]: media/v2-update-explorer/explorer-seven.png
[8]: media/v2-update-explorer/explorer-eight.png
[9]: media/v2-update-explorer/explorer-nine.png
[10]: media/v2-update-explorer/explorer-ten.png
[11]: media/v2-update-explorer/explorer-eleven.png
[12]: media/v2-update-explorer/explorer-twelve.png
[13]: media/v2-update-explorer/explorer-thirteen.png
[14]: media/v2-update-explorer/explorer-fourteen.png
[15]: media/v2-update-explorer/explorer-fifteen.png
[16]: media/v2-update-explorer/explorer-sixteen.png
[17]: media/v2-update-explorer/explorer-seventeen.png
[18]: media/v2-update-explorer/explorer-eighteen.png
[19]: media/v2-update-explorer/explorer-nineteen.png
[20]: media/v2-update-explorer/explorer-twenty.png
[21]: media/v2-update-explorer/explorer-twenty-one.png