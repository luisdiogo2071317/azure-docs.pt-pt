---
title: Azure SDK de Moderator conteúdo para o método de programa auxiliar do .NET | Microsoft Docs
description: Como devolver um cliente de Moderator conteúdo utilizando o SDK Moderator conteúdo do Azure para .NET
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/04/2018
ms.author: sajagtap
ms.openlocfilehash: 36f2124708731f78f34849d8210ed39ea8f59140
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351469"
---
# <a name="helper-code-to-return-a-content-moderator-client"></a>Códigos de programa auxiliar para devolver um cliente Moderator conteúdo

Este artigo fornece informações e exemplos de código para o ajudar a começar a utilizar o SDK Moderator conteúdo para .NET para criar um cliente Moderator conteúdo para a sua subscrição.

A biblioteca é utilizada por outros inícios rápidos nesta secção.

Este artigo pressupõe que já estiver familiarizado com o Visual Studio e c#.

> [!IMPORTANT]
> Esta biblioteca de classe contém código destinado apenas a fins de demonstração.
> Se adaptar este código para utilização em produção, utilize um método de armazenar e utilizando a chave de subscrição de conteúdo Moderator seguro.

## <a name="sign-up-for-content-moderator-services"></a>Inscreva-se os serviços de Moderator conteúdo

Antes de poder utilizar os serviços de Moderator conteúdo através da API REST ou o SDK, precisa de uma chave de subscrição.
Consulte o [início rápido](quick-start.md) para saber como pode obter a chave.

## <a name="create-your-visual-studio-project"></a>Criar o projeto do Visual Studio

1. Crie um novo **biblioteca de classes (.NET Framework)** projeto.

   O código de exemplo, posso com o nome do projeto **ModeratorHelper**.

1. Adicione uma referência para o **System** assemblagem Framework.

### <a name="install-required-packages"></a>Instalar pacotes necessários

Instale os pacotes de NuGet seguintes:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Newtonsoft

### <a name="create-the-content-moderator-client"></a>Criar o cliente Moderator conteúdo

Substitua o conteúdo do ficheiro ModeratorHelper.cs com o seguinte código:

    using Microsoft.CognitiveServices.ContentModerator;

    namespace ModeratorHelper
    {
    /// <summary>
    /// Wraps the creation and configuration of a Content Moderator client.
    /// </summary>
    /// <remarks>This class library contains insecure code. If you adapt this 
    /// code for use in production, use a secure method of storing and using
    /// your Content Moderator subscription key.</remarks>
    public static class Clients
    {
        /// <summary>
        /// The region/location for your Content Moderator account, 
        /// for example, westus.
        /// </summary>
        private static readonly string AzureRegion = "myRegion";

        /// <summary>
        /// The base URL fragment for Content Moderator calls.
        /// </summary>
        private static readonly string AzureBaseURL =
            $"{AzureRegion}.api.cognitive.microsoft.com";

        /// <summary>
        /// Your Content Moderator subscription key.
        /// </summary>
        private static readonly string CMSubscriptionKey = "myKey";

        /// <summary>
        /// Returns a new Content Moderator client for your subscription.
        /// </summary>
        /// <returns>The new client.</returns>
        /// <remarks>The <see cref="ContentModeratorClient"/> is disposable.
        /// When you have finished using the client,
        /// you should dispose of it either directly or indirectly. </remarks>
        public static ContentModeratorClient NewClient()
        {
            // Create and initialize an instance of the Content Moderator API wrapper.
            ContentModeratorClient client = new ContentModeratorClient(new ApiKeyServiceClientCredentials(CMSubscriptionKey));

            client.BaseUrl = AzureBaseURL;
            return client;
        }
    }
    }


> [!IMPORTANT]
> Atualização do **AzureRegion** e **CMSubscriptionKey** campos com os valores da sua chave de subscrições e identificador de região.

Tem agora uma forma rápida de criar um cliente Moderator conteúdo para a sua subscrição.

## <a name="next-steps"></a>Passos Seguintes

[Transferir a solução do Visual Studio](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) para esta e outros inícios rápidos de Moderator conteúdo para o .NET e começar a utilizar a integração.
