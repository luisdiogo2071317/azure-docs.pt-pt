---
title: Explorar a biblioteca de cliente JavaScript do Time Series Insights
description: Saiba mais sobre a biblioteca de cliente JavaScript do Time Series Insights e o modelo de programação associado.
documentationcenter: ''
services: time-series-insights
author: BryanLa
manager: timlt
editor: ''
tags: ''
ms.assetid: ''
ms.service: time-series-insights
ms.workload: na
ms.tgt_pltfrm: ''
ms.devlang: na
ms.topic: tutorial
ms.date: 05/16/2018
ms.author: bryanla
ms.openlocfilehash: 3fbd4f54fb511ae737abf28ae7b1b50750ab5d69
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
ms.locfileid: "34210482"
---
# <a name="tutorial-explore-the-time-series-insights-javascript-client-library"></a>Tutorial: Explorar a biblioteca de cliente JavaScript do Time Series Insights

Para ajudar os programadores a consultar e visualizar dados armazenados no Time Series Insights (TSI), desenvolvemos uma biblioteca de controlo baseada em D3 para JavaScript que facilita este trabalho. Ao utilizar um exemplo de aplicação Web, este tutorial vai orientá-lo ao longo de uma exploração da biblioteca de cliente JavaScript do TIS e do modelo de programação associado.

Os tópicos abordados dão-lhe oportunidades para experimentar, obter uma compreensão mais aprofundada sobre como aceder a dados o TSI e utilizar controlos de gráfico para compor e visualizar dados. O objetivo consiste em disponibilizar-lhe suficiente detalhes, para que possa utilizar a biblioteca na sua própria aplicação Web.

Neste tutorial, ficará a saber mais sobre:

> [!div class="checklist"]
> * A aplicação de exemplo do TSI
> * A biblioteca de cliente JavaScript do TSI
> * De que forma é que aplicação de exemplo utiliza a biblioteca para visualizar dados do TSI

## <a name="prerequisites"></a>Pré-requisitos

Este tutorial utiliza a funcionalidade "Ferramentas de Programador" (também conhecida como DevTools ou F12), disponível na maioria dos browsers mais modernos, como o [Edge](/microsoft-edge/devtools-guide), o [Chrome](https://developers.google.com/web/tools/chrome-devtools/), o [FireFox](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_are_browser_developer_tools), o [Safari](https://developer.apple.com/safari/tools/), entre outros. Se ainda não estiver familiarizado com esta funcionalidade, pode explorá-la no browser antes de continuar.

## <a name="the-time-series-insights-sample-application"></a>A Aplicação de Exemplo do Time Series Insights

Ao longo deste tutorial, a aplicação de exemplo do Time Series Insights vai ser utilizada para explorar o código de origem por trás da aplicação, incluindo a utilização da biblioteca de cliente JavaScript do TSI. A aplicação é uma Aplicação de Página Única (SPA), que mostra a utilização da biblioteca para consultar e visualizar dados a partir de um ambiente do TSI de exemplo.

1. Navegue para a [aplicação de exemplo do Time Series Insights](https://insights.timeseries.azure.com/clientsample). Verá uma página semelhante à seguinte, que lhe pede para iniciar sessão: ![exemplo de mensagem a pedir para iniciar sessão no Cliente TSI](media/tutorial-explore-js-client-lib/tcs-sign-in.png)

2. Clique no botão "Log in" (Iniciar sessão) e introduza ou selecione as suas credenciais. Pode utilizar uma conta de empresa/organização (Azure Active Directory) ou uma conta pessoal (Conta Microsoft ou MSA).

   ![Pedido de credenciais de Exemplo de Cliente TSI](media/tutorial-explore-js-client-lib/tcs-sign-in-enter-account.png)

3. Após o início de sessão com êxito, verá uma página semelhante à seguinte, que contém vários estilos de gráficos de exemplo, preenchidos com dados do TSI. Repare também na conta de utilizador e na ligação “log out” (“terminar sessão”) no canto superior direito: ![Página principal de Exemplo do Cliente TSI após o início de sessão](media/tutorial-explore-js-client-lib/tcs-main-after-signin.png)

### <a name="page-source-and-structure"></a>Origem e estrutura da página

Primeiro, vamos ver o código de origem HTML e JavaScript subjacente à página que foi composta no browser. Não vamos orientá-lo por todos os elementos, mas vai aprender as secções principais, o que lhe dá uma noção do funcionamento da página.

1. Abra “Ferramentas do Programador” no browser e inspecione os elementos HTML que compõem a página atual, também conhecidos como HTML ou árvore DOM.

2. Expanda os elementos `<head>` e `<body>` e repare nas seguintes secções:
   - Em `<head>`, vai encontrar elementos que extraem ficheiros adicionais, para ajudar ao funcionamento da página:
     - um elemento `<script>` para referenciar a Biblioteca de Autenticação do Azure Active Directory(adal.min.js). Também conhecida como ADAL, esta é uma biblioteca JavaScript que fornece autenticação OAuth 2.0 (início de sessão) e aquisição de tokens para aceder a APIs:
     - elementos `<link>` para folhas de estilo (sampleStyles.css, tsiclient.css). Também conhecidas como CSS, são utilizadas para controlar pormenores de estilo das páginas, como cores, tipos de letra, espaçamento, etc.
     - um elemento `<script>` para referenciar a biblioteca do cliente TSI (tsiclient.js). Trata-se de uma biblioteca JavaScript que a página utiliza para chamar as APIs de serviço do TSI e compor os controlos de gráficos na página.

     >[!NOTE]
     > O código de origem para a biblioteca JavaScript do ADAL está disponível no [repositório azure-activedirectory-library-for-js](https://github.com/AzureAD/azure-activedirectory-library-for-js).
     > O código de origem da biblioteca JavaScript do Cliente TSI está disponível no [repositório tsiclient](https://github.com/Microsoft/tsiclient).

   - Em `<body>`, vai encontrar os elementos `<div>`, que funcionam como contentores para definir o esquema dos itens na página, bem como outro elemento `<script>`:
     - o primeiro `<div>` especifica a caixa de diálogo "Log In" (“Iniciar Sessão”) (`id="loginModal"`).
     - o segundo `<div>` funciona como elemento principal de:
       - um cabeçalho `<div>`, utilizado para mensagens de estado e informações de início de sessão junto à parte superior da página (`class="header"`).
       - um `<div>` para o resto dos elementos do corpo da página, incluindo todos os gráficos (`class="chartsWrapper"`).
       - uma secção `<script>`, que contém todos os JavaScript utilizados para controlar a página.

   [![Exemplo de Cliente TSI com DevTools](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-head-body.png)](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-head-body.png#lightbox)

3. Expanda o elemento `<div class="chartsWrapper">` e obterá mais elementos `<div>` subordinados, que são utilizados para posicionar cada exemplo de controlo de gráfico. Repare que existem vários pares de elementos `<div>`, um para cada exemplo de gráfico:
   - O primeiro (`class="rowOfCardsTitle"`) contém um título descritivo para resumir o que o gráfico ou gráficos ilustram. Por exemplo: "Static Line Charts With Full-Size Legends" ("Gráficos de Linhas Estáticas com Legendas em Tamanho Completo")
   - O segundo (`class="rowOfCards"`) é um elemento principal, que contém elementos `<div>` subordinados adicionais que posicionam o controlo ou controlos de gráfico reais numa linha.

  ![Ver os elementos "divs" do corpo](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-divs.png)

4. Agora, expanda o elemento `<script type="text/javascript">`, diretamente abaixo do elemento `<div class="chartsWrapper">`. Verá o início da secção de JavaScript ao nível da página, que é utilizado para processar toda a lógica da página para coisas como autenticação, chamar as APIs de serviço do TSI, compor os controlos de gráficos, entre outras:

  ![Ver o script do corpo](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-script.png)

## <a name="tsi-client-javascript-library-concepts"></a>Conceitos da biblioteca de JavaScript do cliente TSI

Embora não a analisaremos em detalhe, essencialmente a biblioteca do Cliente TSI (tsclient.js) fornece uma abstração para duas categorias importantes:

- **Métodos de encapsulamento em wrapper para chamar as APIs de Consulta do TSI** - APIs REST que lhe permitem consultar os dados do TSI com expressões agregadas e que estão organizadas no espaço de nomes `TsiClient.Server` da biblioteca.
- **Métodos para criar e preencher vários tipos de controlos de gráficos** - utilizados para compor os dados agregados do TSI numa página Web e que estão organizados no espaço de nomes `TsiClient.UX` da biblioteca.

Os seguintes conceitos são universais e aplicam-se às APIs da biblioteca do Cliente TSI em geral.

### <a name="authentication"></a>Autenticação

Conforme mencionado anteriormente, este exemplo é uma Aplicação de Página Única que utiliza o suporte para OAuth 2.0 do ADAL para a autenticação dos utilizadores. Eis alguns pontos de interesse nesta secção do script:

1. Utilizar o ADAL para autenticação requer que a aplicação cliente se registe a si própria no registo de aplicações do Azure Active Directory (Azure AD). Sendo uma SPA, esta aplicação é registada para utilizar o fluxo de concessão de autorização OAuth 2.0 "implícito". Do mesmo modo, a aplicação especifica algumas das propriedades de registo no tempo de execução, como o GUID de ID do cliente (`clientId`) e o URI de redirecionamento (`postLogoutRedirectUri`), para participar no fluxo.

2. Mais tarde, a aplicação pede um "token de acesso" ao Azure AD. O token de acesso é emitido para um conjunto de permissões finito, para um identificador de serviço/API específico (https://api.timeseries.azure.com)), também conhecido como o token "audience". As permissões do token são emitidas em nome do utilizador com sessão iniciada. O identificador do serviço/API é outra das propriedades que estão presentes no registo da aplicação no Azure AD. Assim que o ADAL devolver o token de acesso à aplicação, aquele é transmitido como um "token de portador" durante o aceso às APIs de serviço do TSI.

   [!code-javascript[head-sample](~/samples-javascript/pages/tutorial/index.html?range=145-204&highlight=4-9,36-39)]

### <a name="control-identification"></a>Identificação dos controlos

Conforme abordado anteriormente, os elementos `<div>` em `<body>` fornecem o esquema para todos os controlos de gráficos demonstrados na página. Cada um desses elementos especifica propriedades para a colocação e atributos visuais do controlo de gráfico, incluindo uma propriedade `id`. A propriedade `id` fornece um identificador exclusivo, que é utilizado no código JavaScript para identificar e vincular a cada controlo, para composição e atualização.

### <a name="aggregate-expressions"></a>Expressões agregadas

As APIs da biblioteca do Cliente TSI utilizam intensivamente as expressões agregadas. As expressões agregadas oferecem a capacidade de construir um ou mais "termos de pesquisa." As APIs são semelhantes à forma como o [explorador do Time Series Insights](https://insights.timeseries.azure.com/demo) utiliza os valores de “search span” (“intervalo de pesquisa”), “where” (“onde”), “predicate” (“predicado”), “measures” (“medidas”) e “split-by” (“dividir por”). A maioria das APIs da biblioteca utiliza uma matriz de expressões agregadas, que o serviço utiliza para criar uma consulta de dados do TSI.

### <a name="call-pattern"></a>Padrão de chamada

O preenchimento e a composição dos controlos dos gráficos seguem um padrão geral. Esse padrão é utilizado em todo o JavaScript da página, o que instancia e carrega os controlos da Aplicação de Exemplo do TSI:

1. Declare uma matriz para conter uma ou mais expressões agregadas do TSI.

   ```javascript
   var aes =  [];
   ```

2. Crie 1 a n objetos de expressões agregadas e adicione-os à matriz de expressões agregadas.

   ```javascript
   var ae = new tsiClient.ux.aggregateExpression(predicateObject, measureObject, measureTypes, searchSpan, splitByObject, color, alias, contextMenuActions);
   aes.push(ae);
   ```
   **parâmetros de aggregateExpression**

   | Parâmetro | Descrição | Exemplo |
   | --------- | ----------- | ------- |
   | predicateObject | A expressão de Filtragem dos dados. |`{predicateString: "Factory = 'Factory3'"}` |
   | measureObject   | O nome da propriedade da medida que está a ser utilizada. | `{property: 'Temperature', type: "Double"}` |
   | measureTypes    | Agregações pretendidas da propriedade da medida. | `['avg', 'min']` |
   | searchSpan      | A duração e o tamanho do intervalo da expressão agregada. | `{from: startDate, to: endDate, bucketSize: '2m'}` |
   | splitByObject   | A propriedade da cadeia que pretende dividir (opcional; pode ser nula). | `{property: 'Station', type: 'String'}` |
   | color           | A cor dos objetos que pretende compor. | `'pink'` |
   | alias           | Um nome amigável para a expressão agregada. | `'Factory3Temperature'` |
   | contextMenuActions | Uma matriz das ações a serem vinculadas aos objetos de séries de tempo numa visualização (opcional). | Veja [Menus de contexto pop-up na secção Funcionalidades avançadas.](#popup-context-menus) |

3. Utilize as APIs `TsiClient.Server` para chamar uma Consulta do TSI e pedir os dados agregados.

   ```javascript
   tsiClient.server.getAggregates(token, envFQDN, aeTsxArray);
   ```
   **getAggregates parameters**

   | Parâmetro | Descrição | Exemplo |
   | --------- | ----------- | ------- |
   | token     | Token de acesso da API do TSI | `authContext.getTsiToken()` Veja a [secção Autenticação.](#authentication) |
   | envFQDN     | Nome de Domínio Completamente Qualificado do ambiente do TSI | No portal do Azure, por exemplo `10000000-0000-0000-0000-100000000108.env.timeseries.azure.com` |
   | aeTsxArray | Matriz de expressões de consultas do TSI | Utilize a variável `aes`, conforme descrito anteriormente: `aes.map(function(ae){return ae.toTsx()}` |

4. Transforme o resultado comprimido que a Consulta do TSI devolveu em JSON, para visualização.

   ```javascript
   var transformedResult = tsiClient.ux.transformAggregatesForVisualization(result, aes);
   ```

5. Crie um controlo de gráfico com as APIs `TsiClient.UX` e vincule-o a um dos elementos `<div>` na página.

   ```javascript
   var lineChart = new tsiClient.ux.BarChart(document.getElementById('chart3'));
   ```

6. Preencha o controlo do gráfico com o objeto ou objetos de dados JSON transformados e componha-os na página.

   ```javascript
   lineChart.render(transformedResult, {grid: true, legend: 'compact', theme: 'light'}, aes);
   ```

## <a name="rendering-controls"></a>Controlos de composição

Atualmente, a biblioteca expõe oito controlos de análise exclusivos. Esses controlos incluem um gráfico de linhas, um gráfico circular, um gráfico de barras, um mapa térmico, controlos hierárquicos, uma grelha acessível, barras cronológicas de eventos discretos e barras cronológicas de transição de estado.

### <a name="line-bar-pie-chart-examples"></a>Exemplos de gráficos de linhas, barras e circulares

Vamos começar por ver os códigos subjacentes a alguns dos controlos de gráficos padrão demonstrados na aplicação, bem como o modelo/padrão de programação para criá-los. Mais concretamente, vai examinar a secção do HTML abaixo do comentário `// Example 3/4/5`, o que compõe controlos com os valores de ID `chart3`, `chart4`e `chart5`.

Como vimos no passo 3 da [secção Origem e estrutura da página](#page-source-and-structure), os controlos dos gráficos são dispostos em linhas na página, cada qual com uma linha com um título descritivo. Neste exemplo, os três gráficos que estão a ser preenchidos estão no elemento `<div>` de título "Multiple Chart Types From the Same Data" ("Vários Tipos de Gráficos a partir dos Mesmos Dados") e vinculados aos três elementos `<div>` abaixo do mesmo:

[!code-javascript[code-sample1-line-bar-pie](~/samples-javascript/pages/tutorial/index.html?range=59-73&highlight=1,5,9,13)]

A secção seguinte do código JavaScript utiliza o padrão descrito anteriormente para criar expressões agregadas do TSI, utilizá-las para consultar dados do TSI e compor os três gráficos. Repare nos três tipos utilizados a partir do espaço de nomes `tsiClient.ux`, `LineChart`, `BarChart` e `PieChart`, para criar e compor os respetivos gráficos. Repare também que os três gráficos podem utilizar os mesmos dados da expressão agregada, `transformedResult`:

[!code-javascript[code-sample2-line-bar-pie](~/samples-javascript/pages/tutorial/index.html?range=241-262&highlight=13-14,16-17,19-20)]

Depois de serem compostos, os gráficos aparecem da seguinte forma:

[![Multiple Chart Types From the Same Data](media/tutorial-explore-js-client-lib/tcs-multiple-chart-types-from-the-same-data.png)](media/tutorial-explore-js-client-lib/tcs-multiple-chart-types-from-the-same-data.png#lightbox) (Vários Tipos de Gráficos a partir dos Mesmos Dados)

## <a name="advanced-features"></a>Funcionalidades avançadas

A biblioteca também expõe algumas funcionalidades avançadas opcionais que poderá querer aproveitar.

### <a name="states-and-events"></a>Estados e eventos

Um exemplo de uma funcionalidade avançada oferecida é a capacidade de poder adicionar transições de estado e eventos discretos aos gráficos. Esta funcionalidade é útil para destacar incidentes, alertas e comutadores de estado, como ligado/desligado.

Aqui, vai ver o código subjacente à secção do HTML abaixo do comentário `// Example 10`. O código compõe um controlo de linha abaixo do título "Line Charts with Multiple Series Types", vinculando-o a `<div>` com o valor de ID `chart10`:

1. Primeiro, é definida uma estrutura denominada `events4`, que vai conter os elementos de alteração de estado que vão ser acompanhados. Contém:
   - Uma chave de cadeia com o nome `"Component States"`
   - Uma matriz de objetos de valor que representam os estados, incluindo, cada um:
     - Uma chave de cadeia que contém um carimbo de data/hora ISO de JavaScript
     - Uma matriz que contém as características do estado
       - uma cor
       - uma descrição

2. Em seguida, a estrutura de `events5` é definida para `"Incidents"`, que contém uma matriz dos elementos do evento que vão ser acompanhados. A estrutura da matriz tem a mesma forma daquela que foi descrita para `events4`.

3. Por fim, o gráfico de linhas é composto, transmitindo as duas estruturas com os parâmetros de opções do gráfico, `events:` e `states:`. Repare nos outros parâmetros de opções, para especificar `tooltip:`, `theme:` ou `grid:`.

[!code-javascript[code-sample-states-events](~/samples-javascript/pages/tutorial/index.html?range=337-389&highlight=5,26,51)]

Visualmente, os marcadores/pop-ups de diamante são utilizados para indicar incidentes e as barras/pop-ups coloridos, junto à escala de tempo, indicam as alterações ao estado:

[![Line Charts with Multiple Series Types](media/tutorial-explore-js-client-lib/tcs-line-charts-with-multiple-series-types.png)](media/tutorial-explore-js-client-lib/tcs-line-charts-with-multiple-series-types.png#lightbox) Gráficos de Linhas com Vários Tipos de Série

### <a name="popup-context-menus"></a>Menus de contexto pop-up

Outro exemplo de uma funcionalidade avançada são os menus de contexto personalizados (menus pop-up de clique com o botão direito do rato), que são úteis para permitir ações e passos seguintes lógicos dentro do âmbito da sua aplicação.

Aqui, podemos ver o código subjacente ao HTML, em `// Example 13/14/15`. Inicialmente, este código compõe um gráfico de linhas sob o título "Line Chart with Context Menu to Create Pie/Bar Chart" ("Gráfico de Linhas com Menu de Contexto para Criar Gráfico Circular/de Barras"), vinculado ao elemento `<div>` com o valor de ID `chart13`. Utilizando os menus de contexto, o gráfico de linhas proporciona a capacidade de criar dinamicamente um gráfico circular e de barras, vinculado a elementos `<div>` com os IDs `chart14` e `chart15`. Além disso, tanto o gráfico circular, como o de barras, utilizam também os menus de contexto para permitir as suas próprias funcionalidades: a capacidade de copiar dados do gráfico circular para o de barras e imprimir os dados deste último para a janela da consola do browser, respetivamente.

1. Primeiro, é definida uma série de ações predefinidas. Cada ação contém uma matriz com um ou mais elementos, em que cada qual define um item de menu de contexto individual:
   - `barChartActions`: define o menu de contexto do gráfico circular, que contém um elemento para definir um item individual:
     - `name`: o texto utilizado para o item de menu: "Print parameters to console" ("Imprimir parâmetros para a consola")
     - `action`: a ação associada ao item de menu, que é sempre uma função anónima que recebe três argumentos com base na expressão agregada utilizada para criar o gráfico. Neste caso, são escritos na janela da consola do browser:
       - `ae`: a matriz da expressão agregada
       - `splitBy`: o valor de splitBy
       - `timestamp`: o carimbo de data/hora
   - `pieChartActions`: define o menu de contexto do gráfico de barras, que contém um elemento para definir um item individual. A forma e o esquema são iguais aos de `barChartActions` acima, mas tenha em conta que a função `action` é significativamente diferente, uma vez que instancia e compõe o gráfico de barras. Tenha também em atenção que utiliza o argumento `ae` para especificar a matriz de expressão agregada, que é transmitida no tempo de execução quando o item de menu é apresentado. A função também define a propriedade `ae.contextMenu` com o menu de contexto `barChartActions`.
   - `contextMenuActions`: define o menu de contexto do gráfico de linhas, que contém três elementos para definir três itens de menu. A forma e o esquema de cada elemento são iguais aos dos anteriores. Tal como `barChartActions`, o primeiro item escreve os três argumentos da função na janela da consola do browser. Semelhante a `pieChartActions`, os dois itens seguintes instanciam e compõem os gráficos circular e de barras, respetivamente. Também definem as respetivas propriedades `ae.contextMenu` com os menus de contexto `pieChartActions` e `barChartActions`, respetivamente.

2. Depois, são extraídas duas expressões agregadas para a matriz de expressões agregadas `aes`, especificando a matriz `contextMenuActions` para cada. Estas são utilizadas com o controlo de gráfico de linhas.

3. Por fim, só o gráfico de linhas é inicialmente composto, a partir do qual os gráficos circulares e de barras podem ser compostos no momento de execução.

[!code-javascript[code-sample-context-menus](~/samples-javascript/pages/tutorial/index.html?range=461-540&highlight=7,16,29,61-64,78)]

A captura de ecrã mostra os gráfico, com os menus de contexto pop-up correspondentes. Os gráficos circulares e de barras foram criados dinamicamente mediante a utilização das opções do menu de contexto do gráfico de linhas:

[![Gráfico de Linhas com Menu de Contexto para Criar Gráfico Circular/de Barras](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart.png)](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart.png#lightbox)

### <a name="brushes"></a>Pincéis

Os pincéis podem ser utilizados para limitar um intervalo de tempo para definir ações como ampliar e explorar.

O código utilizado para ilustrar os pincéis também é apresentado no exemplo anterior, "Line Chart with Context Menu to Create Pie/Bar Chart" ("Gráfico de Linhas com Menu de Contexto para Criar Gráfico Circular/de Barras"), que abrange [Menus de contexto pop-up](#popup-context-menus-section).

1. As ações dos pincéis são muito semelhantes às dos menus de contexto, definindo uma série de ações personalizadas para o pincel. Cada ação contém uma matriz com um ou mais elementos, em que cada qual define um item de menu de contexto individual:
   - `name`: o texto utilizado para o item de menu: "Print parameters to console" ("Imprimir parâmetros para a consola")
   - `action`: a ação associada ao item de menu, que é sempre uma função anónima que recebe dois argumentos. Neste caso, são escritos na janela da consola do browser:
      - `fromTime`: o carimbo de data/hora do campo "de" da seleção do pincel
      - `toTime`: o carimbo de data/hora do campo "a" da seleção do pincel

2. As ações do pincel são adicionadas como outra propriedade de opção do gráfico. Repare na propriedade `brushContextMenuActions: brushActions` que está a ser transmitida à chamada `linechart.Render`.

[!code-javascript[code-sample-brushes](~/samples-javascript/pages/tutorial/index.html?range=526-540&highlight=1,13)]

![Line Chart with Context Menu to Create Pie/Bar Chart using brushes (Gráfico de Linhas com Menu de Contexto para Criar Gráfico Circular/de Barras com pincéis)](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart-brushes.png)

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Iniciar sessão e explorar a aplicação de exemplo do TSI e a respetiva origem
> * Utilizar a biblioteca de cliente JavaScript do TSI
> * Utilizar o JavaScript para criar e preencher os controlos dos gráficos com dados do TSI

Conforme foi discutido, a aplicação de exemplo do TSI utiliza um conjunto de dados de demonstração. Para saber mais sobre como pode criar o seu ambiente do TSI e o seu próprio conjunto de dados, avance para o próximo artigo:

> [!div class="nextstepaction"]
> [Plan your Azure Time Series Insights environment](time-series-insights-environment-planning.md) (Planear o seu Ambiente do Azure Time Series Insights)


