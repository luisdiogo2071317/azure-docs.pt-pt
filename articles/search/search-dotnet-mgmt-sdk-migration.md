---
title: "Atualizar para versão 2 do SDK de gestão de .NET Dazure Search | Microsoft Docs"
description: "Atualizar para o SDK de gestão de .NET do Azure Search versão 2"
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
ms.openlocfilehash: ade32dcb4e0885c251c17fad46fb753b6134d027
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2018
---
# <a name="upgrading-to-the-azure-search-net-management-sdk-version-2"></a>Atualizar para o SDK de gestão de .NET do Azure Search versão 2
Se estiver a utilizar a versão 1.0.2 ou mais antiga do [SDK de gestão de .NET de pesquisa do Azure](https://aka.ms/search-mgmt-sdk), este artigo irá ajudá-lo a atualizar a sua aplicação para utilizar a versão 2.

Versão 2 do SDK de gestão do Azure Search .NET contém algumas alterações de versões anteriores. São principalmente menores, para que alterar o seu código deverá ser preciso apenas esforço. Consulte [passos para atualizar](#UpgradeSteps) para obter instruções sobre como alterar o código para utilizar a nova versão do SDK.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-2"></a>Novidades na versão 2
Versão 2 do SDK de gestão do Azure Search .NET destina-se a mesma versão geralmente disponível da API de REST de gestão de pesquisa do Azure como versões anteriores do SDK, especificamente 2015-08-19. As alterações ao SDK estão estritamente do lado do cliente de alterações para melhorar a facilidade de utilização do SDK em si. Estas alterações incluem o seguinte:

* `Services.CreateOrUpdate`e respetivas versões assíncronas agora automaticamente consultam o aprovisionamento `SearchService` e devolve até que o serviço aprovisionamento estiver concluído. Isto poupa tem de ter de escrever desse código, consulta por si.
* Se pretender continuar a consultar manualmente o aprovisionamento de serviço, pode utilizar o novo `Services.BeginCreateOrUpdate` método ou uma das respetivas versões assíncronas.
* Novos métodos `Services.Update` e respetivas versões assíncronas foram adicionadas ao SDK. Estes métodos utilizam HTTP aplicar o PATCH para suportar uma atualização incremental de um serviço. Por exemplo, pode agora dimensionar um serviço transferindo uma `SearchService` instância destes métodos que contém apenas o pretendido `partitionCount` e `replicaCount` propriedades. A forma de chamar antiga `Services.Get`, modificar o devolvido `SearchService`e passou-a para `Services.CreateOrUpdate` ainda é suportada, mas já não é necessário. 

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Passos para atualizar
Em primeiro lugar, atualizar a referência do NuGet para `Microsoft.Azure.Management.Search` utilizando a consola de Gestor de pacotes NuGet ou ao clicar no seu referências do projeto e selecionar "Gerir NuGet pacotes..." no Visual Studio.

Depois de NuGet transferiu os novos pacotes e as respetivas dependências, reconstrua o projeto. Dependendo de como o seu código está estruturado,-reconstrua poderá com êxito. Se Sim, está pronto para começar!

Se a compilação falhar, é possível porque já implementado algumas das interfaces SDK (por exemplo, para efeitos de teste de unidade), que foram alterados. Para resolver este problema, terá de implementar os novos métodos tais como `BeginCreateOrUpdateWithHttpMessagesAsync`.

Depois de ter corrigido os erros de compilação, pode efetuar alterações à sua aplicação para tirar partido das novas funcionalidades, se desejar. Novas funcionalidades no SDK passo são detalhadas no [são as novidades na versão 2](#WhatsNew).

## <a name="conclusion"></a>Conclusão
Apreciamos os seus comentários sobre o SDK. Se tiver problemas, não hesite e peça-nos para obter ajuda no [fórum MSDN de pesquisa do Azure](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch). Se encontrar um erro, ficheiro de um problema no [repositório do GitHub de SDK do .NET do Azure](https://github.com/Azure/azure-sdk-for-net/issues). Certifique-se para o prefixo do título de problema "[pesquisa do Azure]".

Obrigado por utilizar a pesquisa do Azure!
