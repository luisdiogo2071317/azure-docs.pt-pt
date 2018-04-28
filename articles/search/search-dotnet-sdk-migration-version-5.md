---
title: Atualizar para o SDK de .NET de pesquisa do Azure versão 5 | Microsoft Docs
description: Atualizar para o SDK de .NET de pesquisa do Azure versão 5
author: brjohnstmsft
manager: jlembicz
ms.service: search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 04/20/2018
ms.author: brjohnst
ms.openlocfilehash: 018388cd2bd85eb86ad7b62ee247bccd6329e9ac
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="upgrading-to-the-azure-search-net-sdk-version-5"></a>Atualizar para o SDK de .NET de pesquisa do Azure versão 5
Se estiver a utilizar a versão pré-visualização do 4.0 ou mais antiga do [SDK .NET da Azure Search](https://aka.ms/search-sdk), este artigo irá ajudá-lo a atualizar a sua aplicação para utilizar a versão 5.

Para obter instruções mais geral do SDK, incluindo exemplos, consulte [como utilizar o Azure Search a partir de uma aplicação .NET](search-howto-dotnet-sdk.md).

Versão 5 do SDK .NET da Azure Search contém algumas alterações de versões anteriores. São principalmente menores, para que alterar o seu código deverá ser preciso apenas esforço. Consulte [passos para atualizar](#UpgradeSteps) para obter instruções sobre como alterar o código para utilizar a nova versão do SDK.

> [!NOTE]
> Se estiver a utilizar a versão pré-visualização do 2.0 ou mais antiga, deve primeiro atualizar para a versão 3 e, em seguida, atualize para versão 5. Consulte [atualizar para o SDK de .NET de pesquisa do Azure versão 3](search-dotnet-sdk-migration.md) para obter instruções.
>
> A instância de serviço de pesquisa do Azure suporta várias versões de REST API, incluindo o mais recente. Pode continuar a utilizar uma versão quando já não é um mais recente, mas recomendamos que migre código para utilizar a versão mais recente. Quando utilizar a API REST, tem de especificar a versão da API em cada pedido através do parâmetro de versão da api. Ao utilizar o SDK .NET, a versão do SDK está a utilizar determina a versão da REST API correspondente. Se estiver a utilizar um SDK anterior, pode continuar a executar esse código sem alterações, mesmo se o serviço é atualizado para suportar uma versão mais recente da API.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-5"></a>Novidades na versão 5
Versão 5 do SDK .NET da Azure Search destina-se a versão mais recente versão geralmente disponível da API de REST da Azure Search, especificamente 2017-11-11. Isto possibilita a utilizar novas funcionalidades de pesquisa do Azure de uma aplicação .NET, incluindo o seguinte:

* [Sinónimos](search-synonyms.md).
* Agora pode aceder programaticamente avisos no histórico de execução do indexador (consulte o `Warning` propriedade `IndexerExecutionResult` no [referência do .NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexerexecutionresult?view=azure-dotnet) para obter mais detalhes).
* Suporte para .NET Core 2.
* Estrutura do novo pacote suporta a utilização de apenas as partes do SDK que precisa (consulte [interrompendo as alterações na versão 5](#ListOfChanges) para obter detalhes).

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Passos para atualizar
Em primeiro lugar, atualizar a referência do NuGet para `Microsoft.Azure.Search` utilizando a consola de Gestor de pacotes NuGet ou ao clicar no seu referências do projeto e selecionar "Gerir NuGet pacotes..." no Visual Studio.

Depois de NuGet transferiu os novos pacotes e as respetivas dependências, reconstrua o projeto. A menos que estiver a utilizar uma funcionalidade de pré-visualização que não está no SDK GA novo, deve reconstruir com êxito (se não estiver, indique no [GitHub](https://github.com/azure/azure-sdk-for-net/issues)). Se Sim, está pronto para começar!

Tenha em atenção que devido a alterações em empacotamento do SDK .NET da Azure Search, tem de reconstruir a aplicação para utilizar a versão 5. Estas alterações são detalhadas no [interrompendo as alterações na versão 5](#ListOfChanges).

Poderá ver avisos de compilação adicionais relacionados com métodos obsoletos ou propriedades. Os avisos inclui instruções que utilizar em vez da funcionalidade despromovida. Por exemplo, se a sua aplicação utiliza o `IndexingParametersExtensions.DoNotFailOnUnsupportedContentType` método, deve obter um aviso que indica "Este comportamento está agora ativado por predefinição, pelo que já não é necessário chamar este método."

Assim que tiver corrigidos quaisquer avisos de compilação, pode efetuar alterações à sua aplicação para tirar partido das novas funcionalidades, se desejar. Novas funcionalidades no SDK passo são detalhadas no [são as novidades na versão 5](#WhatsNew).

<a name="ListOfChanges"></a>

## <a name="breaking-changes-in-version-5"></a>Eliminar as alterações na versão 5
A alteração inovadora mais significativo na versão 5 é o facto do `Microsoft.Azure.Search` assemblagem e o respetivo conteúdo ter sido dividido em quatro assemblagens separadas que são distribuídas agora como quatro pacotes de NuGet separados:

 - `Microsoft.Azure.Search`: Este é um pacote de metadados que inclui todos os outros pacotes de pesquisa do Azure como dependências. Se estiver a atualizar a partir de uma versão anterior do SDK, basta atualizar este pacote e reconstruir devem ser suficiente para começar a utilizar a nova versão.
 - `Microsoft.Azure.Search.Data`: Utilize este pacote, se estiver a desenvolver uma aplicação .NET através da Azure Search e só precisa de consulta ou documentos na sua índices de atualização. Se também precisa de criar ou atualizar índices, sinónimo maps, ou outros recursos de nível de serviço, utilizam o `Microsoft.Azure.Search` em vez disso, o pacote.
 - `Microsoft.Azure.Search.Service`: Utilize este pacote, se estiver a desenvolver automatização no .NET para gerir os índices de pesquisa do Azure, sinónimo maps, indexadores, origens de dados ou outros recursos de nível de serviço. Se só precisa de consulta ou atualização documentos no seu índices, utilize o `Microsoft.Azure.Search.Data` em vez disso, o pacote. Se precisar de todas as funcionalidades de pesquisa do Azure, utilize o `Microsoft.Azure.Search` em vez disso, o pacote.
- `Microsoft.Azure.Search.Common`: Tipos comuns necessários para as bibliotecas de .NET de pesquisa do Azure. Não deverá necessitar de utilizar este pacote diretamente na sua aplicação; Destina-se apenas a ser utilizado como uma dependência.
 
Esta alteração é tecnicamente interrompendo, uma vez que muitos tipos foram movidos entre as assemblagens. É por isso reconstruir a aplicação é necessário para atualizar a versão 5 do SDK.

Existe um pequeno número de outras alterações na versão 5 que poderão precisar de código altera-se para além de recriar a sua aplicação.

### <a name="removed-obsolete-members"></a>Remover membros obsoletos

Poderá ver erros relacionados com propriedades que foram marcadas como obsoletos em versões anteriores e subsequentemente removidas numa versão 5 ou métodos de compilação. Se encontrar esses erros, eis como resolvê-los:

- Se estava a utilizar o `IndexingParametersExtensions.IndexStorageMetadataOnly` método, utilize `SetBlobExtractionMode(BlobExtractionMode.StorageMetadata)` em vez disso.
- Se estava a utilizar o `IndexingParametersExtensions.SkipContent` método, utilize `SetBlobExtractionMode(BlobExtractionMode.AllMetadata)` em vez disso.

### <a name="removed-preview-features"></a>Funcionalidades de pré-visualização removido

Se estiver a atualizar da versão 4.0-pré-visualização para a versão 5, lembre-se de que matriz JSON e CSV análise suporte para o Blob indexadores foi removido, uma vez que estas funcionalidades ainda estão em pré-visualização. Especificamente, os seguintes métodos do `IndexingParametersExtensions` classe ter sido removido:

- `ParseJsonArrays`
- `ParseDelimitedTextFiles`

Se a aplicação tem uma disco rígida dependência sobre estas funcionalidades, não será possível atualizar para a versão 5 do SDK .NET da Azure Search. Pode continuar a utilizar a versão 4.0-pré-visualização. No entanto,. tenha em atenção que **não é recomendada a utilização de pré-visualização SDKs em aplicações de produção**. Funcionalidades de pré-visualização são apenas de avaliação e podem ser alterados.

## <a name="conclusion"></a>Conclusão
Se precisar de mais detalhes sobre como utilizar o SDK .NET da Azure Search, consulte o [.NET procedimentos](search-howto-dotnet-sdk.md).

Apreciamos os seus comentários sobre o SDK. Se tiver problemas, não hesite e peça-nos para obter ajuda no [fórum MSDN de pesquisa do Azure](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch). Se encontrar um erro, ficheiro de um problema no [repositório do GitHub de SDK do .NET do Azure](https://github.com/Azure/azure-sdk-for-net/issues). Certifique-se para o prefixo do título de problema "[pesquisa do Azure]".

Obrigado por utilizar a pesquisa do Azure!
