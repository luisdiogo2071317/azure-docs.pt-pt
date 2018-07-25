---
title: Tutorial para adicionar a conclusão automática à caixa de pesquisa com o Azure Search | Microsoft Docs
description: Exemplos de como melhorar a experiência do utilizador final das suas aplicações centradas em dados com a conclusão automática e as APIs de sugestões do Azure Search.
manager: pablocas
author: mrcarter8
services: search
ms.service: search
ms.devlang: NA
ms.topic: tutorial
ms.date: 07/11/2018
ms.author: mcarter
ms.openlocfilehash: 7120080bfdc188c150c7065e1c0639ab8c04f173
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38989691"
---
# <a name="tutorial-add-auto-complete-to-your-search-box-using-azure-search"></a>Tutorial: Adicionar a conclusão automática à caixa de pesquisa com o Azure Search

Neste tutorial, irá aprender a utilizar [sugestões](https://docs.microsoft.com/rest/api/searchservice/suggestions), a [conclusão automática ](https://docs.microsoft.com/en-us/rest/api/searchservice/autocomplete) e [facetas](search-faceted-navigation.md) na [API REST do Azure Search](https://docs.microsoft.com/rest/api/searchservice/) e no [SDK do .NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.documentsoperationsextensions?view=azure-dotnet) para criar uma caixa de pesquisa poderosa. As *sugestões* fornecem recomendações de resultados reais com base no que o utilizador escreveu até ao momento. A *conclusão automática*, [uma nova funcionalidade de pré-visualização](search-api-preview.md) do Azure Search, fornece termos a partir do índice para concluir o que o utilizador está a escrever atualmente. Vamos comparar várias técnicas para melhorar a produtividade dos utilizadores e encontrar rápida e facilmente aquilo que procuram ao colocar a riqueza da pesquisa diretamente no utilizador à medida que escreve.

Este tutorial guia-o através de uma aplicação baseada em ASP.NET MVC que utiliza C# para chamar as [bibliotecas de cliente .NET do Azure Search](https://aka.ms/search-sdk) e JavaScript para chamar diretamente a API REST do Azure Search. A aplicação para este tutorial utiliza um índice que preencheu dados de exemplo [NYCJobs](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs). Pode utilizar o índice já configurado na demonstração de Empregos em Nova Iorque ou preencher o seu próprio índice através de um carregador de dados na solução de exemplo NYCJobs. Este exemplo utiliza bibliotecas DE JavaScript [jQuery UI](https://jqueryui.com/autocomplete/) e [XDSoft](https://xdsoft.net/jqplugins/autocomplete/) para criar uma caixa de pesquisa que suporte conclusão automática. Ao utilizar estes componentes, juntamente com o Azure Search, verá vários exemplos de como suportar a conclusão automática com antecipação na sua caixa de pesquisa.

Efetuará as seguintes tarefas:

> [!div class="checklist"]
> * Transferir e configurar a solução
> * Adicionar informações do serviço de pesquisa para as definições da aplicação
> * Implementar uma caixa de entrada de pesquisa
> * Adicionar suporte para uma lista de conclusão automática que solicita conteúdo de uma origem remota 
> * Obter sugestões e conclusão automática com o SDK do .NET e a API REST
> * Suportar a colocação em cache do lado do cliente para melhorar o desempenho 

## <a name="prerequisites"></a>Pré-requisitos

* O Visual Studio 2017. Pode utilizar a [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/) gratuita. 

* Transfira o [código fonte](https://github.com/azure-samples/search-dotnet-getting-started) de exemplo para o tutorial.

* (Opcional) Utilize uma conta do Azure Active Directory e um serviço do Azure Search. Se não tiver uma conta do Azure, pode inscrever-se para obter uma [avaliação gratuita](https://azure.microsoft.com/free/). Para obter ajuda com o aprovisionamento do serviço, veja [Criar um serviço de pesquisa](search-create-service-portal.md). A conta e o serviço são opcionais porque este tutorial pode ser concluído com um índice NYCJobs alojado já em utilização numa demonstração diferente.

* (Opcional) Transfira o código de exemplo [NYCJobs](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs) para importar os dados de NYCJobs para um índice no seu próprio serviço do Azure Search.

> [!Note]
> Se estiver a utilizar o serviço do Azure Search gratuito, está limitado a três índices. O carregador de dados do NYCJobs cria dois índices. Confirme que o seu serviço tem espaço para aceitar os índices novos.

### <a name="set-up-azure-search-optional"></a>Configurar o Azure Search (Opcional)

Siga os passos nesta secção se quiser importar os dados da aplicação de exemplo NYCJobs para o seu próprio índice. Este passo é opcional.  Se quiser utilizar o índice de exemplo fornecido, avance para a secção seguinte e execute o exemplo.

1. Na pasta DataLoader NYCJobs do código de exemplo do NYCJobs, abra o ficheiro da solução DataLoader.sln no Visual Studio.

1. Atualize as informações de ligação para o seu serviço do Azure Search.  Abra o ficheiro App.config no projeto DataLoader e altere as appSettings de TargetSearchServiceName e TargetSearchServiceApiKey para refletir o seu serviço do Azure Search e a chave de API do Serviço do Azure Search.  Podem ser encontrados no portal do Azure.

1. Prima F5 para iniciar a aplicação.  Isto irá criar 2 índices e importar os dados de exemplo de NYCJob.

1. No código de exemplo do tutorial, abra o ficheiro de solução AutocompleteTutorial.sln no Visual Studio.  Abra o ficheiro Web.config no projeto AutocompleteTutorial e altere os valores de SearchServiceName e SearchServiceApiKey para o mesmo que acima.

### <a name="running-the-sample"></a>Executar o exemplo

Pode agora executar a aplicação de exemplo do tutorial.  Abra o ficheiro de solução AutocompleteTutorial.sln no Visual Studio para executar o tutorial.  A solução contém um projeto ASP.NET MVC.  Prima F5 para executar o projeto e carregar a página no browser da sua opção.  Na parte superior, verá uma opção para selecionar C# ou JavaScript.  A opção C# chama o HomeController a partir do browser e utiliza o SDK do .NET do Azure Search para obter os resultados.  A opção JavaScript chama a API REST do Azure Search diretamente a partir do browser.  Normalmente, esta opção terá um melhor desempenho, uma vez que utiliza o controlador a partir do fluxo.  Pode escolher a opção adequada às suas necessidades e preferências de linguagem.  Existem vários exemplos de conclusão automática na página com algumas orientações para cada um.  Cada exemplo tem um texto recomendado que pode experimentar.  Tente escrever algumas letras em cada caixa de pesquisa para ver o que acontece.

## <a name="how-this-works-in-code"></a>Como funciona no código

Agora que já viu os exemplos no browser, vamos ver detalhadamente o primeiro exemplo para rever os componentes envolvidos e como funcionam.

### <a name="search-box"></a>Caixa de pesquisa

Para a escolha da linguagem, a caixa de pesquisa é exatamente a mesma.  Abra o ficheiro Index.cshtml na pasta \Views\Home. A caixa de pesquisa em si é simples:

```html
<input class="searchBox" type="text" id="example1a" placeholder="search">
```

Trata-se de uma caixa de texto de entrada simples com uma classe para definir o estilo, um ID para ser referenciado pelo JavaScript e texto de marcador de posição.  A magia está no javascript.

### <a name="javascript-code-c"></a>Código de JavaScript (C#)

O exemplo de linguagem C# utiliza o JavaScript em Index.cshtml para tirar partido da biblioteca de conclusão automática jQuery UI.  Esta biblioteca adiciona a experiência de conclusão automática à caixa de pesquisa ao efetuar chamadas assíncronas para o controlador MVC para obter recomendações.  Vamos analisar o código JavaScript do primeiro exemplo:

```javascript
$(function () {
    $("#example1a").autocomplete({
        source: "/home/suggest?highlights=false&fuzzy=false&",
        minLength: 3,
        position: {
            my: "left top",
            at: "left-23 bottom+10"
        }
    });
});
```

Este código é executado no browser durante o carregamento da página para configurar a conclusão automática para a caixa de entrada "example1a".  `minLength: 3` garante que as recomendações apenas serão apresentadas quando existe, pelo menos, três carateres na caixa de pesquisa.  O valor de origem é importante:

```javascript
source: "/home/suggest?highlights=false&fuzzy=false&",
```

Esta linha informa a API de conclusão automática onde obter a lista de itens a mostrar na caixa de pesquisa.  Como se trata de um projeto MVC, chama a função Suggest em HomeController.cs.  Vamos ver mais detalhes na próxima secção.  Também transmite alguns parâmetros aos detetores de controlo, correspondência difusa e termo.  A API de JavaScript de conclusão automática adiciona o parâmetro do termo.

#### <a name="extending-the-sample-to-support-fuzzy-matching"></a>Expandir o exemplo para suportar a correspondência difusa

A pesquisa difusa permite-lhe obter os resultados com base em correspondências aproximadas, mesmo que o utilizador escreva uma palavra incorretamente na caixa de pesquisa.  Vamos experimentar isto ao alterar a linha de origem para ativar a correspondência difusa.

Altere a seguinte linha de:

```javascript
source: "/home/suggest?highlights=false&fuzzy=false&",
```

para este:

```javascript
source: "/home/suggest?highlights=false&fuzzy=true&",
```

Inicie a aplicação ao premir F5.

Experimente escrever algo como "execativo" e observe como são devolvidos resultados para "executivo", mesmo que não sendo uma correspondência exata das letras que escreveu.

### <a name="homecontrollercs-c"></a>HomeController.cs (C#)

Agora que revimos o código JavaScript do exemplo, vamos analisar o código do controlador C# que obtém realmente as recomendações através do SDK do .NET do Azure Search.

1. Abra o ficheiro HomeController.cs no diretório Controllers. 

1. A primeira coisa em que irá reparar é num método na parte superior da classe denominado InitSearch.  Esta ação cria um cliente de índice HTTP autenticado para o serviço do Azure Search.  Se quiser saber mais sobre como isto funciona, visite o seguinte tutorial: [Como utilizar o Azure Search a partir de uma Aplicação .NET](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk)

1. Mova para a função Suggest.

```csharp
public ActionResult Suggest(bool highlights, bool fuzzy, string term)
{
    InitSearch();

    // Call suggest API and return results
    SuggestParameters sp = new SuggestParameters()
    {
        UseFuzzyMatching = fuzzy,
        Top = 5
    };

    if (highlights)
    {
        sp.HighlightPreTag = "<b>";
        sp.HighlightPostTag = "</b>";
    }

    DocumentSuggestResult resp = _indexClient.Documents.Suggest(term, "sg", sp);

    // Convert the suggest query results to a list that can be displayed in the client.
    List<string> suggestions = resp.Results.Select(x => x.Text).ToList();
    return new JsonResult
    {
        JsonRequestBehavior = JsonRequestBehavior.AllowGet,
        Data = suggestions
    };
}
```

A função Suggest aceita dois parâmetros que determinam se são devolvidos detetores de ocorrências ou é utilizada a correspondência difusa além da introdução do termo de pesquisa.  O método cria um objeto SuggestParameters, que é então transmitido à API Suggest. Em seguida, o resultado é convertido em JSON, para que possa ser mostrado no cliente.
(Opcional) Adicione um ponto de interrupção à função Suggest e percorra o código.  Tenha em atenção a resposta devolvida pelo SDK e como é convertida no resultado devolvido a partir do método.

Os outros exemplos na página seguem o mesmo padrão para adicionar o detetor de ocorrências, antecipação para recomendações de conclusão automática e facetas para suportar a colocação em cache do lado do cliente dos resultados de conclusão automática.  Reveja cada um deles para compreender como funcionam e como aproveitá-los na sua experiência de pesquisa.

### <a name="javascript-language-example"></a>Exemplo de linguagem JavaScript

No exemplo de linguagem JavaScript, o código JavaScript na página IndexJavaScript.cshtml aproveita a conclusão automática de jQuery UI.  Trata-se de uma biblioteca que faz a maior parte do trabalho pesado ao apresentar uma caixa de pesquisa agradável e facilita as chamadas assíncronas para o Azure Search para obter recomendações.  Vamos analisar o código JavaScript do primeiro exemplo:

```javascript
$(function () {
    $("#example1a").autocomplete({
        source: function (request, response) {
        $.ajax({
            type: "POST",
            url: suggestUri,
            dataType: "json",
            headers: {
                "api-key": searchServiceApiKey,
                "Content-Type": "application/json"
            },
            data: JSON.stringify({
                top: 5,
                fuzzy: false,
                suggesterName: "sg",
                search: request.term
            }),
                success: function (data) {
                    if (data.value && data.value.length > 0) {
                        response(data.value.map(x => x["@@search.text"]));
                    }
                }
            });
        },
        minLength: 3,
        position: {
            my: "left top",
            at: "left-23 bottom+10"
        }
    });
});
```

Se comparar isto ao exemplo acima que chama o controlador Home, verá várias semelhanças.  A configuração de conclusão automática para `minLength` e `position` é exatamente igual.  A alteração significativa aqui é a origem.  Em vez de chamar o método Suggest no controlador Home, é criado um pedido REST numa função de JavaScript e executado através de ajax.  em seguida, a resposta é processada em "êxito" e utilizada como origem.

## <a name="takeaways"></a>Conclusões

Este tutorial demonstra os passos básicos para a criação de uma caixa de pesquisa que suporta conclusão automática e sugestões.  Viu como pode criar uma aplicação ASP.NET MVC e utilizar o SDK do .NET ou a API REST do Azure Search para obter sugestões.

## <a name="next-steps"></a>Passos seguintes

Integre sugestões e conclusão automática na sua experiência de pesquisa.  Considere a forma como a utilização direta do SDK do .NET ou da API REST pode ajudar a levar o poder do Azure Search aos seus utilizadores à medida que escrevem para os tornar mais produtivos.

> [!div class="nextstepaction"]
> [API REST de Conclusão Automática](https://docs.microsoft.com/en-us/rest/api/searchservice/autocomplete)
> [API REST de Sugestões](https://docs.microsoft.com/en-us/rest/api/searchservice/suggestions)
> [Atributo do índice de facetas numa API REST de Criar Índice](https://docs.microsoft.com/en-us/rest/api/searchservice/create-index)

