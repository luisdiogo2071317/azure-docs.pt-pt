---
title: Tutorial sobre indexação, consulta e filtragem no Azure Search através do portal | Microsoft Docs
description: Neste tutorial, utilize o portal do Azure e os dados de exemplo predefinidos para gerar um índice no Azure Search. Explore a pesquisa em texto completo, filtros, facetas, a pesquisa difusa, a pesquisa geográfica e muito mais.
author: HeidiSteen
manager: cgronlun
tags: azure-portal
services: search
ms.service: search
ms.topic: tutorial
ms.date: 07/10/2018
ms.author: heidist
ms.openlocfilehash: 124963359d0b2d4050156958de195e47b9331c92
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39007989"
---
# <a name="tutorial-use-built-in-tools-for-azure-search-indexing-and-queries"></a>Tutorial: utilizar as ferramentas incorporadas para indexação e consultas do Azure Search

Numa página do serviço Azure Search no portal do Azure, pode utilizar as ferramentas incorporadas para fins de teste de conceito e uma experiência prática com um mínimo de incremento. As ferramentas do portal não oferecem uma paridade total com APIs .NET e REST, mas para testes rápidos de prova de conceito, os assistentes e editores proporcionam uma assistência fácil. Nesta introdução isenta de código, vai começar com um pequeno conjunto de dados publicados, para que possa escrever consultas interessantes imediatamente. 

> [!div class="checklist"]
> * Comece com os dados de exemplo públicos e gere automaticamente um índice do Azure Search com o assistente para **Importar dados**. 
> * Veja o esquema e os atributos de qualquer índice publicado no Azure Search.
> * Explore a pesquisa em texto completo, filtros, facetas, a pesquisa difusa e a pesquisa geográfica com o **Explorador de pesquisa**.  

As ferramentas do portal não suportam a gama completa de capacidades do Azure Search. Se as ferramentas forem demasiado limitadoras, considere uma [introdução baseada em código à programação do Azure Search no .NET](search-howto-dotnet-sdk.md) ou [ferramentas de teste na Web para fazer chamadas de API REST](search-fiddler.md).

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar. Também pode ver uma demonstração de seis minutos dos passos deste tutorial a partir do terceiro minuto deste [Vídeo de Descrição Geral do Azure Search](https://channel9.msdn.com/Events/Connect/2016/138).

## <a name="prerequisites"></a>Pré-requisitos

[Crie um serviço Azure Search](search-create-service-portal.md) ou localize um serviço existente na sua subscrição atual. 

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Abra o dashboard de serviço do seu serviço da Azure Search. Se não tiver afixado o mosaico do serviço ao dashboard, pode encontrar o seu serviço da seguinte forma: 
   
   * Na Barra de atalhos, clique em **Todos os serviços** no painel de navegação à esquerda.
   * Na caixa de pesquisa, escreva *pesquisa* para obter uma lista dos serviços relacionados com pesquisa da sua subscrição. Clique em **Pesquisar serviços**. O serviço deverá aparecer na lista. 

### <a name="check-for-space"></a>Verificar o espaço
Muitos clientes começam com o serviço gratuito. Esta versão está limitada a três índices, três origens de dados e três indexadores. Certifique-se de que tem espaço para itens adicionais antes de começar. Este tutorial cria um objeto de cada. 

> [!TIP] 
> Os mosaicos no dashboard do serviço mostram quantos índices, indexadores e origens de dados já tem. O mosaico Indexador mostra indicadores de êxito e falha. Clique no mosaico para ver a contagem de indexadores. 
>
> ![Mosaicos para indexadores e origens de dados][1]
>

## <a name="create-index"></a> Criar um índice e carregar dados
As consultas de pesquisa iteram sobre um [*índice*](search-what-is-an-index.md) que contém dados pesquisáveis, metadados e construções utilizadas para otimizar determinados comportamentos de pesquisa.

Para manter esta tarefa baseada no portal, utilizamos um conjunto de dados de exemplo incorporado que pode ser pesquisado com um [*indexador*](search-indexer-overview.md) através do assistente para **Importar dados**. Um indexador é um crawler específico da origem que pode ler metadados e conteúdo de origens de dados suportadas pelo Azure. No código, pode criar e gerir os indexadores como recursos independentes. No portal, os indexadores são expostos através do assistente para **Importar dados**. 

#### <a name="step-1-start-the-import-data-wizard"></a>Passo 1: Iniciar o assistente para Importar dados
1. No seu dashboard do serviço Azure Search, clique em **Importar dados** na barra de comando para iniciar um assistente que cria e preenche um índice.
   
    ![Comando de importação de dados][2]

2. No assistente, clique em **Ligar aos seus dados** > **Exemplos** > **realestate-us-sample**. Esta origem de dados está pré-configurada com um nome, um tipo e informações da ligação. Depois de criada, torna-se uma “origem de dados existente”, que pode ser reutilizada noutras operações de importação.

    ![Selecionar o conjunto de dados de exemplo][9]

3. Clique em **OK** para utilizá-la.

#### <a name="skip-cognitive-skills"></a>Ignorar as Capacidades Cognitivas

**Importar dados** inclui um passo opcional de capacidades cognitivas que adiciona algoritmos de IA à indexação. Esta funcionalidade não é abrangida neste tutorial, pelo que deve avançar diretamente para **Personalizar o índice de destino**. Se estiver curioso sobre a nova funcionalidade de pré-visualização de pesquisa cognitiva no Azure Search, experimente o [início rápido de pesquisa cognitiva](cognitive-search-quickstart-blob.md) ou o [tutorial](cognitive-search-tutorial-blob.md).

   ![Ignorar o passo de capacidades cognitivas][11]

#### <a name="step-2-define-the-index"></a>Passo 2: Definir o índice
Geralmente, a tarefa de criação de um índice é manual e baseada em código, mas o assistente pode gerar um índice para qualquer origem de dados que consiga pesquisar. No mínimo, os índices precisam de um nome e de uma coleção de campos, com um campo marcado como a chave do documento para identificar exclusivamente cada documento.

Os campos têm tipos de dados e atributos. As caixas de verificação na parte superior são *atributos de índice* que controlam a forma como o campo é utilizado. 

* **Recuperável** significa que aparece na lista de resultados da pesquisa. Pode marcar campos individuais como inacessíveis para os resultados da pesquisa ao desmarcar esta caixa de verificação, por exemplo, quando os campos são utilizados apenas em expressões de filtro. 
* **Filtrável**, **Ordenável** e **Facetável** determinam se um campo pode ser utilizado num filtro, uma ordenação ou uma estrutura de navegação de faceta. 
* **Pesquisável** significa que um campo está incluído na pesquisa de texto completo. As cadeias são pesquisáveis. Os campos numéricos e booleanos são frequentemente marcados como não pesquisáveis. 

Por predefinição, o assistente verifica a origem de dados relativamente a identificadores exclusivos como a base do campo de chaves. As cadeias são atribuídas como recuperáveis e pesquisáveis. Os números inteiros são atribuídos como recuperáveis, filtráveis, ordenáveis e facetáveis.

  ![Índice de realestate gerado][3]

Clique em **OK** para criar o índice.

#### <a name="step-3-define-the-indexer"></a>Passo 3: Definir o indexador
Ainda no assistente para **Importar dados**, clique em **Indexador** > **Nome** e escreva um nome para o indexador. 

Este objeto define um processo executável. Pode colocá-lo na agenda recorrente, mas, por agora, utilize a opção predefinida para executar o indexador uma vez, de imediato, quando clicar em **OK**.  

  ![indexador de realestate][8]

## <a name="check-progress"></a>Verificar progresso
Para monitorizar a importação de dados, regresse ao dashboard do serviço, desloque-se para baixo e faça duplo clique no mosaico **Indexadores**, para abrir a lista de indexadores. Deverá ver o indexador que acabou de criar na lista, com o estado a indicar “em curso” ou “êxito”, juntamente com o número de documentos indexados.

   ![Mensagem de indexador em curso][4]

## <a name="view-the-index"></a>Ver o índice

Os mosaicos no dashboard de serviço fornecem as informações de resumo, bem como acesso a informações detalhadas. Por exemplo, no mosaico **Índices**, deverá ver uma lista de índices existentes, incluindo o índice *realestate-us-sample* que acabou de criar no passo anterior.

Agora, clique no índice *realestate-us-sample* para ver as opções do portal para a respetiva definição. Uma opção para **Adicionar/Editar Campos** permite-lhe criar e atribuir novos campos completamente. Os campos existentes têm uma representação física no Azure Search e são, portanto, não modificáveis, nem mesmo no código. Para alterar radicalmente um campo existente, crie um campo novo e remova o original. 

   ![definição de índice de exemplo][10]

Outras construções, como a classificação de perfis e as opções de CORS, podem ser adicionadas a qualquer momento. 

Para compreender claramente o que pode e não pode editar durante o design do índice, dispense um minuto para ver as opções de definição do índice. As opções desativadas são um indicador de que um valor não pode ser alterado nem eliminado.

## <a name="query-index"></a> Consultar o índice
Mais adiante, já deve ter um índice de pesquisa pronto para consultar com a página de consulta [**Explorador de procura**](search-explorer.md) incorporada. Fornece uma caixa de pesquisa para que possa testar as cadeias de consulta arbitrária. 

> [!TIP]
> No [vídeo de Descrição Geral do Azure Search](https://channel9.msdn.com/Events/Connect/2016/138), os passos seguintes são demonstrados a partir dos 6m08 segundos.
>

1. Clique em **Explorador de pesquisa** na barra de comando.

   ![Comando do Explorador de pesquisa][5]

2. Clique em **Alterar índice**, na barra de comandos, para mudar para *realestate-us-sample*. Clique em **Definir versão da API**, na barra de comandos, para ver que APIs REST estão disponíveis. Para as consultas abaixo, utilize a versão disponível geralmente (2017-11-11). 

   ![Comandos de índice e de API][6]

3. Na barra de pesquisa, introduza as cadeias de consulta abaixo e clique em **Pesquisar**.

    > [!NOTE]
    > O **Explorador de procura** só está preparado para processar [pedidos de API REST](https://docs.microsoft.com/rest/api/searchservice/search-documents). Aceita sintaxes de [sintaxe de consultas simples](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) e [ analisador de consultas de Lucene completo](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search), bem como todos os parâmetros de pesquisa disponíveis em operações de [Pesquisar no Documento](https://docs.microsoft.com/rest/api/searchservice/search-documents).
    > 


#### <a name="example-string-searchseattle"></a>Exemplo (cadeia): `search=seattle`

+ O parâmetro **search** é utilizado para introduzir uma pesquisa de palavras-chave para pesquisa em texto completo; neste caso, devolve imóveis em King County, no estado norte-americano de Washington, que contenham *Seattle* em qualquer campo pesquisável do documento. 

+ O **Explorador de pesquisa** devolve resultados em JSON, que é verboso e difícil de ler se os documentos tiverem uma estrutura densa. Isto é intencional; a visibilidade de todo o documento é um caso de utilização importante, especialmente durante os testes. Para uma melhor experiência de utilizador, terá de escrever código que [processa resultados de pesquisa](search-pagination-page-layout.md) para reproduzir elementos importantes.

+ Os documentos são compostos por todos os campos marcados como "recuperáveis" no índice. Para ver os atributos de índice no portal, clique em *realestate-us-sample* no mosaico **Índices**.

#### <a name="example-parameterized-searchseattlecounttruetop100"></a>Exemplo (parametrizado): `search=seattle&$count=true&$top=100`

+ O símbolo **&** é utilizado para acrescentar os parâmetros da pesquisa, que podem ser especificados por qualquer ordem. 

+  O parâmetro **$count=true** devolve uma contagem para a soma de todos os documentos devolvidos. Este valor é apresentado junto à parte superior dos resultados da pesquisa. Pode verificar as consultas de filtro através da monitorização das alterações comunicadas por **$count=true**. As contagens mais pequenas indicam que o filtro está a funcionar.

+ **$top=100** devolve os cem documentos com a melhor classificação de entre o total. Por predefinição, o Azure Search devolve as 50 melhores correspondências. Pode aumentar ou diminuir a quantidade através de **$top**.

## <a name="filter-query"></a>Filtrar a consulta

Os filtros são incluídos nos pedidos de pesquisa se acrescentar o parâmetro **$filter**. 

#### <a name="example-filtered-searchseattlefilterbeds-gt-3"></a>Exemplo (filtrado): `search=seattle&$filter=beds gt 3`

+ O parâmetro **$filter** devolve resultados que correspondem aos critérios que indicou. Neste caso, quartos acima de 3. 

+ A sintaxe do filtro é uma construção OData. Para obter mais informações, veja [Filter OData syntax (Sintaxe de Filtros OData)](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search).

## <a name="facet-query"></a> Facetar a consulta

Os filtros de faceta são incluídos nos resultados da pesquisa. Pode utilizar o parâmetro de faceta para devolver uma contagem agregada dos documentos que correspondam a um valor de faceta que indicar. 

#### <a name="example-faceted-with-scope-reduction-searchfacetcitytop2"></a>Exemplo (por facetas com redução de âmbito): `search=*&facet=city&$top=2`

+ **search=*** é uma consulta vazia. As pesquisas em branco pesquisam em tudo. Uma das razões para submeter uma consulta em branco é para filtrar ou especificar facetas no conjunto completo de documentos. Por exemplo, pretende que uma estrutura de navegação por facetas seja constituída por todas as cidades no índice.

+  **facet** devolve uma estrutura de navegação que pode ser transmitida a um controlo de IU. Devolve categorias e uma contagem. Neste caso, as categorias baseiam-se no número de cidades. Não existe agregação no Azure Search, mas pode aproximar a agregação através de `facet`, que dá uma contagem de documentos em cada categoria.

+ **$top=2** devolve dois documentos, o que exemplifica que pode utilizar `top` para reduzir ou aumentar os resultados.

#### <a name="example-facet-on-numeric-values-searchseattlefacetbeds"></a>Exemplo (faceta em valores numéricos): `search=seattle&facet=beds`**

+ Esta consulta é a faceta para «beds», numa pesquisa de texto por *Seattle*. O termo *beds* pode ser especificado como faceta, porque o campo está marcado como campo recuperável, filtrável e facetável no índice e os valores nele contidos (numéricos, 1 a 5) são adequados para categorizar imóveis em grupos (listas com três quartos, quatro quartos). 

+ Apenas os campos filtráveis podem ser facetados. Apenas os campos recuperáveis podem ser devolvidos nos resultados.

## <a name="highlight-query"></a> Adicionar detetor de ocorrências

O detetor de ocorrências refere-se à formatação no texto que corresponde à palavra-chave, tendo em conta que as correspondências são encontradas num determinado campo. Se o termo da sua pesquisa estiver embrenhado numa descrição, pode adicionar o detetor de ocorrências para que seja mais fácil encontrá-lo. 

#### <a name="example-highlighter-searchgranite-countertopshighlightdescription"></a>Exemplo (marcador): `search=granite countertops&highlight=description`

+ Neste exemplo, é mais fácil encontrar a expressão formatada *granite countertops* (bancadas em granito) no campo da descrição.

#### <a name="example-linguistic-analysis-searchmicehighlightdescription"></a>Exemplo (análise linguística): `search=mice&highlight=description`

+ A pesquisa em texto completo localiza formas de palavras com uma semântica parecida. Neste caso, os resultados da pesquisa contêm texto realçado para “rato”, para casas que tenham uma praga de ratos, em resposta a uma pesquisa de palavras-chave sobre “rato”. Podem aparecer nos resultados diferentes formas da mesma palavra, devido à análise linguística. 

+ O Azure Search suporta 56 analisadores do Lucene e da Microsoft. Por predefinição, o Azure Search utiliza o analisador padrão do Lucene. 

## <a name="fuzzy-search"></a> Experimentar a pesquisa difusa

Por predefinição, os termos de consulta com erros ortográficos, como *samamish*para o planalto Samammish, na área de Seattle, não conseguem devolver correspondências em pesquisas típicas. O exemplo seguinte não devolve nenhum resultado.

#### <a name="example-misspelled-term-unhandled-searchsamamish"></a>Exemplo (termo com erros ortográficos, não processado): `search=samamish`

Para processar erros ortográficos, pode utilizar a pesquisa difusa. A pesquisa difusa é ativada quando utiliza a sintaxe de consulta Lucene completa, que ocorre quando faz duas coisas: define **queryType=full** na consulta e anexa **~** à cadeia de pesquisa. 

#### <a name="example-misspelled-term-handled-searchsamamishquerytypefull"></a>Exemplo (termo com erros ortográficos, processado): `search=samamish~&queryType=full`

Este exemplo devolve agora documentos que incluem correspondências de "Sammamish".

Quando **queryType** não é especificado, é utilizado o analisador de consultas simples predefinido. O analisador de consultas simples é mais rápido, mas se precisar de pesquisa difusa, de expressões regulares, da pesquisa de proximidade ou de outros tipos de consultas avançadas, será necessária a sintaxe completa. 

A pesquisa difusa e a pesquisa com carateres universais têm implicações no resultado da pesquisa. A análise linguística não é realizada nestes formatos de consulta. Antes de utilizar a pesquisa difusa e a pesquisa com carateres universais, reveja [Como funciona a pesquisa em texto completo no Azure Search](search-lucene-query-architecture.md#stage-2-lexical-analysis) e procure a secção sobre as exceções à análise lexical.

Para obter mais informações sobre os cenários de consultas que o analisador de consultas completo permite, veja [Lucene query syntax in Azure Search (Sintaxe de consultas do Lucene no Azure Search)](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search).

## <a name="geo-search"></a> Experimentar a pesquisa geoespacial

A pesquisa geoespacial é suportada através do [tipo de dados edm.GeographyPoint](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) em campos que contenham coordenadas. A pesquisa geográfica é um tipo de filtros especificado em [Filter OData syntax (Sintaxe de Filtros OData)](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search). 

#### <a name="example-geo-coordinate-filters-searchcounttruefiltergeodistancelocationgeographypoint-122121513-47673988-le-5"></a>Exemplo (filtros de coordenadas geográficas): `search=*&$count=true&$filter=geo.distance(location,geography'POINT(-122.121513 47.673988)') le 5`

A consulta de exemplo filtra todos os resultados relativamente a dados posicionais, em que os resultados estejam a menos de cinco quilómetros de distância de um determinado ponto (especificado como coordenadas de latitude e longitude). Ao adicionar **$count**, pode ver quantos resultados são devolvidos quando alterar a distância ou as coordenadas. 

A pesquisa geoespacial é útil se a sua aplicação de pesquisa tiver uma funcionalidade “encontrar perto de mim” ou se utilizar a navegação por mapa. Contudo, não é uma pesquisa em texto completo. Se os seus utilizadores tiverem de pesquisar por nome de cidade ou país, adicione campos que contenham nomes de cidades ou países, para além de coordenadas.

## <a name="takeaways"></a>Conclusões

Este tutorial demonstra os passos básicos para utilizar o assistente para **Importar dados** e o **Explorador de procura** no portal do Azure.

Como a força motriz nos bastidores do assistente para Importar dados, aprendeu sobre [indexadores](search-indexer-overview.md), bem como o fluxo de trabalho básico para design do índice, incluindo [modificações suportadas num índice publicado](ttps://docs.microsoft.com/rest/api/searchservice/update-index). 

Aprendeu a sintaxe de consulta através de exemplos práticos que demonstram as capacidades principais, como filtros, detetor de ocorrências, pesquisa difusa e pesquisa geográfica.

Por fim, aprendeu a obter informações ao clicar em mosaicos no dashboard de qualquer índice, indexador ou origem de dados que criar para a sua subscrição. Mais tarde, ao trabalhar com os seus índices ou índices criados por colegas, pode utilizar o portal para verificar rapidamente uma definição de origem de dados ou a construção de uma coleção de campos, sem ter de procurar através de código que não lhe é familiar.

## <a name="clean-up-resources"></a>Limpar recursos

A forma mais rápida de os limpar no final do tutorial passa por eliminar o grupo de recursos que contém o serviço Azure Search. Pode eliminar o grupo de recursos agora, para eliminar permanentemente tudo no mesmo. No portal, o nome do grupo de recursos está na página Descrição Geral do serviço Azure Search.

## <a name="next-steps"></a>Passos seguintes

Para exploração adicional baseada em ferramentas do Azure Search, considere a utilização de uma ferramenta de teste REST, como o Postman ou o Fiddler:

> [!div class="nextstepaction"]
> [Ferramentas de teste na Web para chamar APIs REST do Azure Search](search-fiddler.md)


<!--Image references-->
[1]: ./media/search-get-started-portal/tiles-indexers-datasources2.png
[2]: ./media/search-get-started-portal/import-data-cmd2.png
[3]: ./media/search-get-started-portal/realestateindex2.png
[4]: ./media/search-get-started-portal/indexers-inprogress2.png
[5]: ./media/search-get-started-portal/search-explorer-cmd2.png
[6]: ./media/search-get-started-portal/search-explorer-changeindex-se2.png
[7]: ./media/search-get-started-portal/search-explorer-query2.png
[8]: ./media/search-get-started-portal/realestate-indexer2.png
[9]: ./media/search-get-started-portal/import-datasource-sample2.png
[10]: ./media/search-get-started-portal/sample-index-def.png
[11]: ./media/search-get-started-portal/skip-cog-skill-step.png