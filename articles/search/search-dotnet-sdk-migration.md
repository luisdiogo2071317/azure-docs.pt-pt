---
title: "Atualizar para o SDK de .NET de pesquisa do Azure versão 3 | Microsoft Docs"
description: "Atualizar para o SDK de .NET de pesquisa do Azure versão 3"
services: search
documentationcenter: 
author: brjohnstmsft
manager: pablocas
editor: 
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 01/15/2018
ms.author: brjohnst
ms.openlocfilehash: 48238788e06057ccaba41d1d3f500b5c10c93cb7
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2018
---
# <a name="upgrading-to-the-azure-search-net-sdk-version-3"></a>Atualizar para o SDK de .NET de pesquisa do Azure versão 3
Se estiver a utilizar a versão pré-visualização do 2.0 ou mais antiga do [SDK .NET da Azure Search](https://aka.ms/search-sdk), este artigo irá ajudá-lo a atualizar a sua aplicação para utilizar a versão 3.

Para obter instruções mais geral do SDK, incluindo exemplos, consulte [como utilizar o Azure Search a partir de uma aplicação .NET](search-howto-dotnet-sdk.md).

A versão 3 do SDK .NET da Azure Search contém algumas alterações de versões anteriores. São principalmente menores, para que alterar o seu código deverá ser preciso apenas esforço. Consulte [passos para atualizar](#UpgradeSteps) para obter instruções sobre como alterar o código para utilizar a nova versão do SDK.

> [!NOTE]
> Se estiver a utilizar a versão 1.0.2-preview ou anterior, deverá atualizar para versão 1.1 e, em seguida, atualize para versão 3. Consulte [atualizar para o SDK de .NET do Azure Search versão 1.1](search-dotnet-sdk-migration-version-1.md) para obter instruções.
>
> A instância de serviço de pesquisa do Azure suporta várias versões de REST API, incluindo o mais recente. Pode continuar a utilizar uma versão quando já não é um mais recente, mas recomendamos que migre código para utilizar a versão mais recente. Quando utilizar a API REST, tem de especificar a versão da API em cada pedido através do parâmetro de versão da api. Ao utilizar o SDK .NET, a versão do SDK está a utilizar determina a versão da REST API correspondente. Se estiver a utilizar um SDK anterior, pode continuar a executar esse código sem alterações, mesmo se o serviço é atualizado para suportar uma versão mais recente da API.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-3"></a>Novidades na versão 3
A versão 3 do SDK .NET da Azure Search destina-se a versão mais recente versão geralmente disponível da API de REST da Azure Search, especificamente 2016-09-01. Isto torna possível utilizar muitas funcionalidades novas da Azure Search a partir de uma aplicação .NET, incluindo o seguinte:

* [Analisadores personalizados](https://aka.ms/customanalyzers)
* [Armazenamento de Blobs do Azure](search-howto-indexing-azure-blob-storage.md) e [Table Storage do Azure](search-howto-indexing-azure-tables.md) suporte do indexador
* Personalização de indexador através de [mapeamentos de campo](search-indexer-field-mappings.md)
* O suporte de ETags para ativar a atualização em simultâneo seguro de definições de índice, indexadores e origens de dados
* Suporte para criar definições de campo de índice de forma declarativa decorating a classe de modelo e utilizando a nova `FieldBuilder` classe.
* Suporte para o .NET Core e o perfil do .NET portátil 111

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Passos para atualizar
Em primeiro lugar, atualizar a referência do NuGet para `Microsoft.Azure.Search` utilizando a consola de Gestor de pacotes NuGet ou ao clicar no seu referências do projeto e selecionar "Gerir NuGet pacotes..." no Visual Studio.

Depois de NuGet transferiu os novos pacotes e as respetivas dependências, reconstrua o projeto. Dependendo de como o seu código está estruturado,-reconstrua poderá com êxito. Se Sim, está pronto para começar!

Se a compilação falhar, deverá ver um erro de compilação como o seguinte:

    Program.cs(31,45,31,86): error CS0266: Cannot implicitly convert type 'Microsoft.Azure.Search.ISearchIndexClient' to 'Microsoft.Azure.Search.SearchIndexClient'. An explicit conversion exists (are you missing a cast?)

O passo seguinte é para corrigir este erro de compilação. Consulte [interrompendo as alterações na versão 3](#ListOfChanges) para obter detalhes sobre o que faz com que o erro e como o corrigir.

Poderá ver avisos de compilação adicionais relacionados com métodos obsoletos ou propriedades. Os avisos inclui instruções que utilizar em vez da funcionalidade despromovida. Por exemplo, se a sua aplicação utiliza o `IndexingParameters.Base64EncodeKeys` propriedade, deve obter um aviso que indica que`"This property is obsolete. Please create a field mapping using 'FieldMapping.Base64Encode' instead."`

Depois de ter corrigido os erros de compilação, pode efetuar alterações à sua aplicação para tirar partido das novas funcionalidades, se desejar. Novas funcionalidades no SDK passo são detalhadas no [são as novidades na versão 3](#WhatsNew).

<a name="ListOfChanges"></a>

## <a name="breaking-changes-in-version-3"></a>Eliminar as alterações na versão 3
Existe um pequeno número de interrompendo as alterações na versão 3, que pode necessitar de código altera-se para além de recriar a sua aplicação.

### <a name="indexesgetclient-return-type"></a>Tipo de retorno Indexes.GetClient
O `Indexes.GetClient` método tem um novo tipo de retorno. Anteriormente, devolveu `SearchIndexClient`, mas isto foi alterado para `ISearchIndexClient` na versão 2.0-preview e que alteração acarreta a versão 3. Isto é para suportar clientes que pretendem mock o `GetClient` método para testes de unidade ao devolver uma implementação de mock `ISearchIndexClient`.

#### <a name="example"></a>Exemplo
Se o seu código tem o seguinte aspeto:

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

Pode alterá-la para esta opção para corrigir os erros de compilação:

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

### <a name="analyzername-datatype-and-others-are-no-longer-implicitly-convertible-to-strings"></a>Já não são implicitamente pode ser convertidos em cadeias AnalyzerName, tipo de dados e outros
Existem muitos tipos no SDK .NET da Azure Search que derivem de `ExtensibleEnum`. Anteriormente foram todos os seguintes tipos implicitamente pode ser convertido no tipo `string`. No entanto, um erro foi descoberto no `Object.Equals` implementação para estas classes e corrigir os erros necessário desativar esta conversão implícita. A conversão explícita para `string` ainda tem permissão.

#### <a name="example"></a>Exemplo
Se o seu código tem o seguinte aspeto:

```csharp
var customTokenizerName = TokenizerName.Create("my_tokenizer"); 
var customTokenFilterName = TokenFilterName.Create("my_tokenfilter"); 
var customCharFilterName = CharFilterName.Create("my_charfilter"); 
 
var index = new Index();
index.Analyzers = new Analyzer[] 
{ 
    new CustomAnalyzer( 
        "my_analyzer",  
        customTokenizerName,  
        new[] { customTokenFilterName },  
        new[] { customCharFilterName }), 
}; 
```

Pode alterá-la para esta opção para corrigir os erros de compilação:

```csharp
const string CustomTokenizerName = "my_tokenizer"; 
const string CustomTokenFilterName = "my_tokenfilter"; 
const string CustomCharFilterName = "my_charfilter"; 
 
var index = new Index();
index.Analyzers = new Analyzer[] 
{ 
    new CustomAnalyzer( 
        "my_analyzer",  
        CustomTokenizerName,  
        new TokenFilterName[] { CustomTokenFilterName },  
        new CharFilterName[] { CustomCharFilterName })
}; 
```

### <a name="removed-obsolete-members"></a>Remover membros obsoletos

Poderá ver erros de compilação relacionados com propriedades que foram identificadas como obsoletos na versão 2.0 preview e posteriormente removidas versão 3 ou métodos. Se encontrar esses erros, eis como resolvê-los:

- Se estava a utilizar este construtor: `ScoringParameter(string name, string value)`, em alternativa, utilize este:`ScoringParameter(string name, IEnumerable<string> values)`
- Se estava a utilizar o `ScoringParameter.Value` propriedade, utilize o `ScoringParameter.Values` propriedade ou o `ToString` método em vez disso.
- Se estava a utilizar o `SearchRequestOptions.RequestId` propriedade, utilize o `ClientRequestId` propriedade em vez disso.

### <a name="removed-preview-features"></a>Funcionalidades de pré-visualização removido

Se estiver a atualizar da versão 2.0-pré-visualização para a versão 3, lembre-se de que JSON e CSV análise suporte para o Blob indexadores foi removido, uma vez que estas funcionalidades ainda estão em pré-visualização. Especificamente, os seguintes métodos do `IndexingParametersExtensions` classe ter sido removido:

- `ParseJson`
- `ParseJsonArrays`
- `ParseDelimitedTextFiles`

Se a aplicação tem uma disco rígida dependência sobre estas funcionalidades, não será possível atualizar a versão 3 do SDK .NET da Azure Search. Pode continuar a utilizar a pré-visualização-versão 2.0. No entanto,. tenha em atenção que **não é recomendada a utilização de pré-visualização SDKs em aplicações de produção**. Funcionalidades de pré-visualização são apenas de avaliação e podem ser alterados.

## <a name="conclusion"></a>Conclusão
Se precisar de mais detalhes sobre como utilizar o SDK .NET da Azure Search, consulte o [.NET procedimentos](search-howto-dotnet-sdk.md).

Apreciamos os seus comentários sobre o SDK. Se tiver problemas, não hesite e peça-nos para obter ajuda no [fórum MSDN de pesquisa do Azure](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch). Se encontrar um erro, ficheiro de um problema no [repositório do GitHub de SDK do .NET do Azure](https://github.com/Azure/azure-sdk-for-net/issues). Certifique-se para o prefixo do título de problema "[pesquisa do Azure]".

Obrigado por utilizar a pesquisa do Azure!
