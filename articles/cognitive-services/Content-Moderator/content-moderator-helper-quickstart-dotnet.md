---
title: 'Guia de Início Rápido: SDK do Content Moderator para o método auxiliar .NET'
titlesuffix: Azure Cognitive Services
description: Como devolver um cliente do Content Moderator com o SDK do Azure Content Moderator para .NET
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: quickstart
ms.date: 01/04/2018
ms.author: sajagtap
ms.openlocfilehash: 0c167de3af8aea6820962e7cc8d0c21f5ba61807
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47223275"
---
# <a name="quickstart-helper-code-to-return-a-content-moderator-client"></a>Guia de Início Rápido: código auxiliar para devolver um cliente do Content Moderator

Este artigo apresenta informações e exemplos de código para ajudá-lo a começar a utilizar o SDK do Content Moderator para .NET para criar um cliente do Content Moderator para a sua subscrição.

A biblioteca é utilizada por outros inícios rápidos nesta secção.

Este artigo pressupõe que já está familiarizado com o Visual Studio e a linguagem C#.

> [!IMPORTANT]
> Esta biblioteca de classes contém código apenas para fins de demonstração.
> Se adaptar este código para utilização em produção, utilize um método seguro de armazenamento e utilização da sua chave de subscrição do Content Moderator.

## <a name="sign-up-for-content-moderator-services"></a>Inscrever-se nos serviços do Content Moderator

Antes de poder utilizar os serviços do Content Moderator através da API REST ou do SDK, precisa de uma chave de subscrição.
Veja o [Guia de Início Rápido](quick-start.md) para saber como pode obter a chave.

## <a name="create-your-visual-studio-project"></a>Criar o projeto do Visual Studio

1. Crie um novo projeto **Biblioteca de Classes (.NET Framework)**.

   No código de exemplo, o projeto tem o nome **ModeratorHelper**.

1. Adicione uma referência à assemblagem do .NET Framework **System.Configuration**.

### <a name="install-required-packages"></a>Instalar pacotes necessários

Instale os seguintes pacotes NuGet:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Newtonsoft.Json

### <a name="create-the-content-moderator-client"></a>Criar o cliente do Content Moderator

Substitua os conteúdos do ficheiro ModeratorHelper.cs pelo seguinte código:

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
> Atualize os campos **AzureRegion** e **CMSubscriptionKey** com os valores do identificador da região e da chave de subscrição.

Tem agora uma forma rápida de criar um cliente do Content Moderator para a sua subscrição.

## <a name="next-steps"></a>Passos seguintes

[Transfira a solução Visual Studio](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) para este e outros inícios rápidos do Content Moderator para .NET e comece a trabalhar na sua integração.
