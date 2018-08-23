---
title: incluir ficheiro
description: incluir ficheiro
services: service-bus-relay
author: clemensv
ms.service: service-bus-relay
ms.topic: include
ms.date: 08/16/2018
ms.author: clemensv
ms.custom: include file
ms.openlocfilehash: e07d82b8a3aea4f0db0f5a071d78ea360cd611ab
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/16/2018
ms.locfileid: "40177258"
---
### <a name="create-a-console-application"></a>Criar uma aplicação de consola

Se tiver desativado a opção "Requer Autorização do Cliente" ao criar o Reencaminhamento, pode enviar os pedidos para o URL de Ligações Híbridas com qualquer browser. Para aceder a pontos finais protegidos, tem de criar e transmitir um token no cabeçalho `ServiceBusAuthorization`, mostrado aqui.

No Visual Studio, crie um novo projeto de **Aplicação de Consola (.NET Framework)**.

### <a name="add-the-relay-nuget-package"></a>Adicionar o pacote NuGet de Reencaminhamento

1. Clique com o botão direito do rato no projeto recém-criado e, em seguida, selecione **Gerir Pacotes NuGet**.
2. Selecione **Procurar** e, em seguida, procure **Microsoft.Azure.Relay**. Nos resultados da pesquisa, selecione **Reencaminhamento do Microsoft Azure**. 
3. Selecione **Instalar** para concluir a instalação. Feche a caixa de diálogo.

### <a name="write-code-to-send-requests"></a>Escrever código para enviar pedidos

1. Na parte superior do ficheiro Program.cs, substitua as instruções `using` existentes pelas seguintes instruções `using`:
   
    ```csharp
    using System;
    using System.IO;
    using System.Threading;
    using System.Threading.Tasks;
    using System.Net.Http;
    using Microsoft.Azure.Relay;
    ```
2. Adicione constantes à classe `Program`, para obter os detalhes da ligação híbrida. Substitua os marcadores de posição entre parênteses retos pelos valores que obteve quando criou a ligação híbrida. Certifique-se de que utiliza o nome de espaço de nomes totalmente qualificado.
   
    ```csharp
    private const string RelayNamespace = "{RelayNamespace}.servicebus.windows.net";
    private const string ConnectionName = "{HybridConnectionName}";
    private const string KeyName = "{SASKeyName}";
    private const string Key = "{SASKey}";
    ```
3. Adicione o seguinte método à classe `Program`:
   
    ```csharp
    private static async Task RunAsync()
    {
        var tokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                KeyName, Key);
        var uri = new Uri(string.Format("https://{0}/{1}", RelayNamespace, ConnectionName));
        var token = (await tokenProvider.GetTokenAsync(uri.AbsoluteUri, TimeSpan.FromHours(1))).TokenString;
        var client = new HttpClient();
        var request = new HttpRequestMessage()
        {
            RequestUri = uri,
            Method = HttpMethod.Get,
        };
        request.Headers.Add("ServiceBusAuthorization", token);
        var response = await client.SendAsync(request);
        Console.WriteLine(await response.Content.ReadAsStringAsync());        Console.ReadLine();
    }
    ```
4. Adicione a seguinte linha de código ao método `Main` na classe `Program`.
   
    ```csharp
    RunAsync().GetAwaiter().GetResult();
    ```
   
    O ficheiro Program.cs deve ter o seguinte aspeto:
   
    ```csharp
    using System;
    using System.IO;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.Relay;
   
    namespace Client
    {
        class Program
        {
            private const string RelayNamespace = "{RelayNamespace}.servicebus.windows.net";
            private const string ConnectionName = "{HybridConnectionName}";
            private const string KeyName = "{SASKeyName}";
            private const string Key = "{SASKey}";
   
            static void Main(string[] args)
            {
                RunAsync().GetAwaiter().GetResult();
            }
   
            private static async Task RunAsync()
            {
               var tokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                KeyName, Key);
                var uri = new Uri(string.Format("https://{0}/{1}", RelayNamespace, ConnectionName));
                var token = (await tokenProvider.GetTokenAsync(uri.AbsoluteUri, TimeSpan.FromHours(1))).TokenString;
                var client = new HttpClient();
                var request = new HttpRequestMessage()
                {
                    RequestUri = uri,
                    Method = HttpMethod.Get,
                };
                request.Headers.Add("ServiceBusAuthorization", token);
                var response = await client.SendAsync(request);
                Console.WriteLine(await response.Content.ReadAsStringAsync());
            }
        }
    }
    ```

