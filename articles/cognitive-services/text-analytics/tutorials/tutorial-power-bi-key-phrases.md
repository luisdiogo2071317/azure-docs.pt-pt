---
title: Análise de Texto com o Power BI - Serviços Cognitivos do Azure | Microsoft Docs
description: Saiba como utilizar a Análise de Texto para extrair expressões-chave de textos armazenados no Power BI.
services: cognitive-services
author: luiscabrer
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: tutorial
ms.date: 3/07/2018
ms.author: luisca
ms.openlocfilehash: 2cdb93d44218627efdcb0360d8cf4a4eeeca177a
ms.sourcegitcommit: 7b845d3b9a5a4487d5df89906cc5d5bbdb0507c8
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/14/2018
ms.locfileid: "42889265"
---
# <a name="text-analytics-with-power-bi"></a>Análise de Texto com o Power BI

O Microsoft Power BI mostra os dados das organizações em apelativos relatórios que podem ser distribuídos pela sua organização, de modo a proporcionar informações mais rápidas e aprofundadas. Parte dos Serviços Cognitivos do Microsoft Azure, o serviço Análise de Texto consegue extrair as expressões mais importantes dos textos através da API Key Phrases. Em conjunto, estas ferramentas podem ajudá-lo a ver rapidamente aquilo de que os seus clientes estão a falar e como se sentem em relação a esse assunto.

Neste tutorial, pode aprender a integrar o Power BI Desktop e a API Key Phrases para extrair as expressões mais importantes dos comentários dos clientes mediante a utilização de uma função do Power Query. Também vamos criar um Word Cloud a partir dessas expressões.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, precisa:

> [!div class="checklist"]
> * Do Microsoft Power BI Desktop. [Transfira sem custos](https://powerbi.microsoft.com/get-started/).
> * De uma conta do Microsoft Azure. [Inicie uma avaliação gratuita](https://azure.microsoft.com/free/) ou [inicie sessão](https://portal.azure.com/).
> * De uma chave de acesso para a Análise de Texto. [Inscreva-se](../../cognitive-services-apis-create-account.md) e [obtenha a sua chave](../how-tos/text-analytics-how-to-access-key.md).
> * De comentários de clientes. [Obtenha os nossos dados de exemplo](https://aka.ms/cogsvc/ta) ou utilize os seus próprios dados.

## <a name="loading-customer-data"></a>Carregar dados de clientes

Para começar, abra o Power BI Desktop e carregue o ficheiro de valores separados por vírgula (CSV) **FabrikamComments.csv**. Este ficheiro representa um dia de atividades hipotéticas num fórum de suporte fictício de uma empresa.

> [!NOTE]
> O Power BI pode utilizar dados de várias origens, como o Facebook ou uma base de dados SQL. Saiba mais em [Integração do Facebook no Power BI](https://powerbi.microsoft.com/integrations/facebook/) e em [Integração do SQL Server no Power BI](https://powerbi.microsoft.com/integrations/sql-server/).

Na janela principal do Power BI Desktop, localize o grupo External Data (Dados Externos) do friso Base. Escolha **Text/CSV** (Texto/CSV) no menu pendente **Get Data** (Obter Dados) deste grupo.

![[Botão Get Data (Obter Dados)]](../media/tutorials/power-bi/get-data-button.png)

É apresentada a caixa de diálogo Open (Abrir). Navegue para a pasta Downloads (Transferências) ou para a pasta que contém o ficheiro de dados de exemplo. Clique em `FabrikamComments.csv` e, depois, no botão **Open** (Abrir). É apresentada a caixa de diálogo de importação do CSV.

![[Caixa de diálogo Importar CSV]](../media/tutorials/power-bi/csv-import.png)

A caixa de diálogo de importação do CSV permite-lhe verificar se o Power BI Desktop detetou corretamente o conjunto de carateres, o delimitador, as linhas de cabeçalho e os tipos de colunas. Estas informações estão todas corretas, pelo que clicamos em **Carregar**.

Para ver os dados carregados, utilize o botão Data View (Vista de Dados), na extremidade esquerda da área de trabalho do Power BI, para mudar para a vista Data (Dados). É aberta uma tabela que contém os nossos dados, como no Microsoft Excel.

![[Vista inicial dos dados importados]](../media/tutorials/power-bi/initial-data-view.png)

## <a name="preparing-the-data"></a>Preparar os dados

Poderá ser necessário transformar os seus dados no Power BI Desktop antes que os mesmos estejam prontos para serem processados pelo serviço Key Phrases.

Os nossos dados, por exemplo, incluem o campo `subject` e o campo `comment`. Quando extraímos expressões-chave, devemos considerar texto em ambos os campos, não apenas em `comment`. A função Merge Columns (Unir Colunas) no Power BI Desktop facilita a execução desta tarefa.

Clique em **Edit Queries** (Editar Consultas), no grupo External Data (Dados Externos), no friso Base, para abrir o Query Editor (Editor de Consultas) na janela principal do Power BI Desktop. 

![[Grupo External Data no friso Base]](../media/tutorials/power-bi/edit-queries.png)

Selecione "FabrikamComments" na lista Queries (Consultas), no lado esquerdo da janela, caso ainda não esteja selecionado.

Agora, selecione as colunas `subject` e `comment` na tabela. Poderá ter de se deslocar na horizontal para ver estas colunas. Primeiro, clique no cabeçalho de coluna `subject`, mantenha premida a tecla Control e clique no cabeçalho de coluna `comment`.

![[Selecionar os campos que vão ser unidos]](../media/tutorials/power-bi/select-columns.png)

No grupo Text Columns (Colunas de Texto) do friso Transform (Transformar), clique em **Merge Columns** (Unir Colunas). É apresentada a caixa de diálogo Merge Columns (Unir Colunas).

![[Unir campos com a caixa de diálogo Merge Columns]](../media/tutorials/power-bi/merge-columns.png)

Na caixa de diálogo Merge Columns (Unir Colunas), escolha Tab (Tabulação) como o separador e clique em **OK.** E já está!

Também pode considerar filtrar e deixar de fora as mensagens em branco ao utilizar o filtro Remove Empty (Remover em Branco) ou remover os carateres que não podem ser impressos com a transformação Clean (Limpar). Se os seus dados tiverem uma coluna como a coluna `spamscore` no ficheiro de exemplo, pode utilizar um filtro Number (Número) para ignorar os comentários de “spam”.

## <a name="understanding-the-api"></a>Compreender a API

A API Key Phrases consegue processar até mil documentos de texto por pedido HTTP. Contudo, o Power BI prefere lidar com registos um de cada vez, pelo que as nossas chamadas à API vão conter apenas um documento. [A API](//westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) precisa dos seguintes campos para cada documento que está a ser processado.

| | |
| - | - |
| `id`  | Um identificador exclusivo para este documento dentro do pedido. A resposta também contém este campo. Desta forma, se processar vários documentos, pode associar facilmente as expressões-chave extraídas ao documento do qual provêm. Como estamos a processar um documento por pedido, já sabemos a que documento é que a resposta está associada, pelo que `id` pode ser igual em cada pedido.|
| `text`  | O texto que vai ser processado. É obtido na coluna `Merged` que criámos, que contém a linha do assunto e o texto do comentário combinados. Para a API Key Phrases, estes dados não podem ter mais de 5000 carateres.|
| `language` | O código que representa o idioma em que o documento está escrito. As nossas mensagens estão todas em inglês, pelo que podemos codificar o idioma `en` na nossa consulta.|

Temos de combinar estes campos num documento JSON (JavaScript Object Notation), para ser submetido para a API Key Phrases. A resposta deste pedido também é um documento JSON e temos de analisá-lo e obter as expressões-chave.

## <a name="creating-a-custom-function"></a>Criar uma função personalizada

Agora, estamos prontos para criar a função personalizada que vai integrar o Power BI e a Análise de Texto. O Power BI Desktop invoca a função para cada linha da tabela e cria uma coluna nova com os resultados.

A função recebe o texto que vai ser processado como parâmetro. Converte os dados de e para o JavaScript Object Notation (JSON) necessário e faz o pedido HTTP para o ponto final da API Key Phrases. Depois de analisar a resposta, a função devolve uma cadeia que contém uma lista separada por vírgulas das expressões-chave extraídas.

> [!NOTE]
> As funções personalizadas do Power BI Desktop são escritas na [linguagem de fórmulas M do Power Query](https://msdn.microsoft.com/library/mt211003.aspx), ou apenas “M”, para abreviar. M é uma linguagem de programação funcional com base em [F#](https://docs.microsoft.com/dotnet/fsharp/). No entanto, não é preciso ser programador para concluir este tutorial. O código necessário está incluído abaixo.

Ainda na janela Query Editor (Editor de Consultas), no friso Base, clique em **New Source** (Nova Origem) (no grupo New Query [Nova Consulta]) e escolha **Blank Query** (Consulta em branco), no menu pendente. 

É apresentada uma consulta nova, inicialmente denominada Query1, na lista Queries (Consultas). Faça duplo clique nesta entrada e dê-lhe o nome `KeyPhrases`.

Agora, clique em **Advanced Editor** (Editor Avançado), no grupo Query (Consulta) do friso Base, para abrir a janela deste editor. Elimine o código que já aparece na janela e cole o código seguinte. 

> [!NOTE]
> Nos exemplos abaixo, presumimos que o ponto final está em https://westus.api.cognitive.microsoft.com.  A Análise de Texto suporta a criação de subscrições em 13 regiões diferentes. Se se tiver inscrito no serviço noutra região, confirme que utiliza o ponto final da região que selecionou. A região deve aparecer na página Descrição Geral do portal do Azure quando selecionar a sua subscrição da Análise de Texto.

```fsharp
// Returns key phrases from the text in a comma-separated list
(text) => let
    apikey      = "YOUR_API_KEY_HERE",
    endpoint    = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases",
    jsontext    = Text.FromBinary(Json.FromValue(Text.Start(Text.Trim(text), 5000))),
    jsonbody    = "{ documents: [ { language: ""en"", id: ""0"", text: " & jsontext & " } ] }",
    bytesbody   = Text.ToBinary(jsonbody),
    headers     = [#"Ocp-Apim-Subscription-Key" = apikey],
    bytesresp   = Web.Contents(endpoint, [Headers=headers, Content=bytesbody]),
    jsonresp    = Json.Document(bytesresp),
    keyphrases  = Text.Lower(Text.Combine(jsonresp[documents]{0}[keyPhrases], ", "))
in  keyphrases
```

Cole também a chave da API de Análise de Texto, obtida no dashboard do Microsoft Azure, na linha que começa por `apikey`. (Certifique-se de que mantém as aspas antes e depois da chave.) Em seguida, clique em **Done** (Concluído).

## <a name="using-the-function"></a>Utilizar a função

Agora, podemos utilizar a função personalizada para obter as expressões-chave presentes em cada comentário dos nossos clientes e armazená-las numa coluna nova na tabela. 

Ainda no Query Editor (Editor de Consultas), mude para a consulta FabrikamComments, mude para o friso Add Column (Adicionar Coluna) e clique no botão **Invoke Custom Function** (Invocar Função Personalizada), no grupo General (Geral).

![[Botão Invoke Custom Function]](../media/tutorials/power-bi/invoke-custom-function-button.png)<br><br>

Na caixa de diálogo Invoke Custom Function (Invocar Função Personalizada), introduza `keyphrases` como o nome da coluna nova. Escolha a função personalizada, `KeyPhrases`, como Function Query (Consulta de Função). 

É apresentado um campo novo na caixa de diálogo, que pergunta que campo queremos transmitir à função como o parâmetro `text`. Escolha `Merged` (a coluna que criámos anteriormente ao unir os campos de assunto e de mensagem), no menu pendente.

![[Invocar uma função personalizada]](../media/tutorials/power-bi/invoke-custom-function.png)

Por fim, clique em **OK.**

Se estiver tudo pronto, o Power BI chama a nossa função uma vez para cada linha na tabela, executando as consultas de expressões-chave e adicionando a coluna nova à tabela. Mas antes que isso aconteça, poderá ter de especificar as definições de autenticação e privacidade.

## <a name="authentication-and-privacy"></a>Autenticação e privacidade

Depois de fechar a caixa de diálogo Invoke Custom Function (Invocar Função Personalizada), poderá surgir uma faixa que lhe pede que especifique como vai ser feita a ligação ao ponto final de Key Phrases.

![[faixa de credenciais]](../media/tutorials/power-bi/credentials-banner.png)

Clique em **Edit Credentials** (Editar Credenciais), confirme que está selecionado Anonymous (Anónimo) na caixa de diálogo, e clique em **Connect** (Ligar). 

> [!NOTE]
> Porquê anónimo? O serviço Análise de Texto utiliza a chave de API para o autenticar, pelo que o Power BI não tem de fornecer credenciais para o próprio pedido HTTP.

![[definir a autenticação como anónima]](../media/tutorials/power-bi/access-web-content.png)

Se vir a faixa Edit Credentials (Editar Credenciais) mesmo depois de escolher o acesso anónimo, poderá ter-se esquecido de colar a chave de API. Verifique a função personalizada `KeyPhrases` no Advanced Editor (Editor Avançado) para confirmar que está aí.

De seguida, poderá aparecer uma faixa que pede que indique informações sobre a privacidade das origens de dados. 

![[faixa de privacidade]](../media/tutorials/power-bi/privacy-banner.png)

Clique em **Continue** (Continuar) e escolha Public (Pública) para cada origem de dados na caixa de diálogo. Em seguida, clique em **Save** (Guardar).

![[definir a privacidade da origem de dados]](../media/tutorials/power-bi/privacy-dialog.png)

## <a name="creating-the-word-cloud"></a>Criar o balão de palavras

Depois de resolver as várias faixas que apareceram, clique em **Close & Apply** (Fechar e Aplicar), no friso Base, para fechar o Query Editor (Editor de Consultas).

O Power BI Desktop demora alguns momentos a fazer os pedidos HTTP necessários. Em cada linha na tabela, a coluna `keyphrases` nova contém as expressões-chave que a API Key Phrases detetou no texto. 

Vamos utilizar esta coluna para gerar um balão de palavras. Para começar, clique no botão Report (Relatório), na janela principal do Power BI Desktop, no lado esquerdo da área de trabalho.

> [!NOTE]
> Porquê utilizar expressões-chave extraídas para gerar um balão de palavras em vez do texto completo de todos os comentários? As expressões-chave dão-nos as palavras *importantes* dos comentários dos clientes, não apenas as palavras *mais comuns*. Além disso, o dimensionamento das palavras no balão resultante não é distorcido pela utilização frequente de uma determinada palavra num número relativamente pequeno de comentários.

Se ainda não tiver o elemento visual Word Cloud instado, instale-o. No painel Visualizations, no lado direito da área de trabalho, clique nas reticências (**...**) e escolha **Import From Store** (Importar do Arquivo). Em seguida, procure “cloud” e clique no botão **Add** (Adicionar), junto ao elemento visual Word Cloud. O Power BI instala o elemento visual Word Cloud e diz-lhe se o instalou com êxito.

![[adicionar um elemento visual]](../media/tutorials/power-bi/add-custom-visuals.png)<br><br>

Agora, vamos criar o nosso balão de palavras!

![[Ícone do Word Cloud no painel visualizations]](../media/tutorials/power-bi/visualizations-panel.png)

Primeiro, clique no ícone Word Cloud, no painel Visualizations (Visualizações). É apresentado um relatório novo na área de trabalho. Arraste o campo `keyphrases` do painel Fields (Campos) para o campo Category (Categoria), no painel Visualizations (Visualizações). O balão de palavras aparece dentro do relatório.

Agora, mude para a página Formato do painel Visualizations (Visualizações). Na categoria Stop Words (Parar Palavras), ative **Default Stop Words** (Palavras de Paragem Predefinidas) para eliminar palavras pequenas e comuns, como “de”, no balão. 

![[ativar palavras de paragem predefinidas]](../media/tutorials/power-bi/default-stop-words.png)

Um pouco mais abaixo no painel, desative **Rotate Text** (Rodar Texto) e **Title** (Título).

![[ativar o modo de detalhe]](../media/tutorials/power-bi/word-cloud-focus-mode.png)

Clique na ferramenta Modo de Detalhe para ver melhor o balão de palavras. A ferramenta expande o balão de palavras de modo a que preencha toda a área de trabalho, conforme mostrado abaixo.

![[Balão de Palavras]](../media/tutorials/power-bi/word-cloud.png)

## <a name="more-text-analytics-services"></a>Mais serviços da Análise de Texto

O serviço Análise de Texto, um dos Serviços Cognitivos que o Microsoft Azure oferece, também disponibiliza análise de sentimentos e deteção de idioma. A deteção de idioma, em particular, é útil se nem todos os comentários dos seus clientes estiverem em inglês.

Estas duas APIs são muito semelhantes à API Key Phrases. Assim, é possível utilizar funções personalizadas quase iguais para integrá-las no Power BI Desktop. Basta criar uma consulta em branco e colar o código adequado abaixo, no Advanced Editor (Editor Avançado), como fez anteriormente. (Não se esqueça da sua chave de acesso!) Depois, tal como antes, utilize a função para adicionar uma coluna nova à tabela.

A função Sentiment Analysis, abaixo, devolve uma classificação que indica até que ponto é positivo o sentimento expresso no texto.

```fsharp
// Returns the sentiment score of the text, from 0.0 (least favorable) to 1.0 (most favorable)
(text) => let
    apikey      = "YOUR_API_KEY_HERE",
    endpoint    = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/sentiment",
    jsontext    = Text.FromBinary(Json.FromValue(Text.Start(Text.Trim(text), 5000))),
    jsonbody    = "{ documents: [ { language: ""en"", id: ""0"", text: " & jsontext & " } ] }",
    bytesbody   = Text.ToBinary(jsonbody),
    headers     = [#"Ocp-Apim-Subscription-Key" = apikey],
    bytesresp   = Web.Contents(endpoint, [Headers=headers, Content=bytesbody]),
    jsonresp    = Json.Document(bytesresp),
    sentiment   = jsonresp[documents]{0}[score]
in  sentiment
```

Eis duas versões de uma função Language Detection. A primeira devolve o código de idioma ISO (por exemplo, `en` para inglês), ao passo que a segunda devolve o nome “amigável” (por exemplo, `English`). Poderá reparar que apenas a última linha do corpo difere entre ambas as versões.

```fsharp
// Returns the two-letter language code (e.g. en for English) of the text
(text) => let
    apikey      = "YOUR_API_KEY_HERE",
    endpoint    = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/languages",
    jsontext    = Text.FromBinary(Json.FromValue(Text.Start(Text.Trim(text), 5000))),
    jsonbody    = "{ documents: [ { id: ""0"", text: " & jsontext & " } ] }",
    bytesbody   = Text.ToBinary(jsonbody),
    headers     = [#"Ocp-Apim-Subscription-Key" = apikey],
    bytesresp   = Web.Contents(endpoint, [Headers=headers, Content=bytesbody]),
    jsonresp    = Json.Document(bytesresp),
    language    = jsonresp[documents]{0}[detectedLanguages]{0}[iso6391Name]
in  language
```
```fsharp
// Returns the name (e.g. English) of the language in which the text is written
(text) => let
    apikey      = "YOUR_API_KEY_HERE",
    endpoint    = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/languages",
    jsontext    = Text.FromBinary(Json.FromValue(Text.Start(Text.Trim(text), 5000))),
    jsonbody    = "{ documents: [ { id: ""0"", text: " & jsontext & " } ] }",
    bytesbody   = Text.ToBinary(jsonbody),
    headers     = [#"Ocp-Apim-Subscription-Key" = apikey],
    bytesresp   = Web.Contents(endpoint, [Headers=headers, Content=bytesbody]),
    jsonresp    = Json.Document(bytesresp),
    language    = jsonresp[documents]{0}[detectedLanguages]{0}[name]
in  language
```

Por fim, segue-se uma variação da função Key Phrases já apresentada que devolve as expressões como um objeto de lista em vez de uma cadeia única de expressões separadas por vírgulas. 

> [!NOTE]
> A devolução de uma única cadeia simplificou o nosso exemplo de balão de palavras. As listas, por sua vez, são um formato mais flexível para trabalhar com as expressões devolvidas no Power BI. Pode utilizar o grupo Structured Column (Coluna Estruturada), no frio Transform (Transformar) do Query Editor (Editor de Consultas), para manipular os objetos de lista.

```fsharp
// Returns key phrases from the text as a list object
(text) => let
    apikey      = "YOUR_API_KEY_HERE",
    endpoint    = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases",
    jsontext    = Text.FromBinary(Json.FromValue(Text.Start(Text.Trim(text), 5000))),
    jsonbody    = "{ documents: [ { language: ""en"", id: ""0"", text: " & jsontext & " } ] }",
    bytesbody   = Text.ToBinary(jsonbody),
    headers     = [#"Ocp-Apim-Subscription-Key" = apikey],
    bytesresp   = Web.Contents(endpoint, [Headers=headers, Content=bytesbody]),
    jsonresp    = Json.Document(bytesresp),
    keyphrases  = jsonresp[documents]{0}[keyPhrases]
in  keyphrases
```

## <a name="next-steps"></a>Passos seguintes

Saiba mais sobre o serviço Análise de Texto, a linguagem de fórmula M no Power Query ou o Power BI.

> [!div class="nextstepaction"]
> [Text Analytics API reference](//westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) (Referência à API de Análise de Texto)

> [!div class="nextstepaction"]
> [Power Query M reference](//msdn.microsoft.com/library/mt211003.aspx) (Referência à linguagem M do Power Query)

> [!div class="nextstepaction"]
> [Documentação do Power BI](//powerbi.microsoft.com/documentation/powerbi-landing-page/)
