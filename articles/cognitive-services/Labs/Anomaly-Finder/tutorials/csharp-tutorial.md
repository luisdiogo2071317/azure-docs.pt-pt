---
title: Aplicação de deteção c# anomalias - serviços cognitivos da Microsoft | Documentos da Microsoft
description: Explore uma aplicação de c# que utiliza a API de deteção de anomalias nos serviços cognitivos da Microsoft. Enviar pontos de dados original a API e obter o valor esperado e os pontos de anomalias.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: c6d33de70fd5b7c03b7af6b85e8ebc567dd2f925
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/03/2018
ms.locfileid: "39502513"
---
# <a name="anomaly-detection-c-application"></a>Aplicação de anomalias deteção c#

Explore uma aplicação básica do Windows que utiliza a API de deteção de anomalias para detetar anomalias da entrada. O exemplo envia os dados de séries de tempo para a API de deteção de anomalias com a sua chave de subscrição, em seguida, obtém todos os pontos de anomalias e valor esperado para cada ponto de dados a partir da API.

## <a name="prerequisites"></a>Pré-requisitos

### <a name="platform-requirements"></a>Requisitos de plataforma

O exemplo foi desenvolvido para o .NET Framework usando [Visual Studio 2017, edição de Comunidade](https://www.visualstudio.com/products/visual-studio-community-vs). 

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Subscrever a deteção de anomalias e obter uma chave de subscrição 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="get-and-use-the-example"></a>Obtenha e utilize o exemplo

Pode clonar a aplicação de exemplo de deteção de anomalias para o computador a partir [Github](https://github.com/MicrosoftAnomalyDetection/csharp-sample.git). 
<a name="Step1"></a>
### <a name="install-the-example"></a>Instalar o exemplo

No seu ambiente de trabalho do GitHub, abra Sample\AnomalyDetectionSample.sln.

<a name="Step2"></a>
### <a name="build-the-example"></a>Criar o exemplo

Prima Ctrl + Shift + B, ou clique em Build no menu da faixa de opções, em seguida, selecione compilar solução.

<a name="Step3"></a>
### <a name="run-the-example"></a>Executar o exemplo

1. Depois de concluída a compilação, prima **F5** ou clique em **iniciar** no menu da faixa de opções para executar o exemplo.
2. Localize a janela de interface de utilizador de deteção de anomalias com a caixa de edição de texto ler "{your_subscription_key}".
3. Substitua o ficheiro de request.json, que contém os dados de exemplo, com os seus dados, em seguida, clique em "Enviar". Microsoft recebe os dados carregar e usá-los para detetar quaisquer pontos de anomalias entre, em seguida. Os dados que carregar não serão incluídos no servidor da Microsoft. Para detetar o ponto de anomalias novamente, precisa carregar os dados mais uma vez.
4. Se os dados são boas, encontrará o resultado da deteção de anomalias no campo de "Resposta". Se ocorrer algum erro, as informações de erro serão exibidas no campo de resposta.

<a name="Review"></a>
### <a name="read-the-result"></a>O resultado de leitura

[!INCLUDE [diagrams](../includes/diagrams.md)]

<a name="Review"></a>
### <a name="review-and-learn"></a>Rever e aprender

Agora que tem um aplicativo em execução, vamos rever como a aplicação de exemplo se integra com a tecnologia de serviços cognitivos. Este passo irá facilitar a continuar a criar para esta aplicação ou a desenvolver a sua própria aplicação com deteção de anomalias da Microsoft.

Esta aplicação de exemplo usa a API Restful de deteção de anomalias ponto final.

Rever como a API Restful, é utilizada no aplicativo de exemplo, vamos examinar um trecho de código do **AnomalyDetectionClient.cs**. O ficheiro contém comentários sobre o código que indica "Chave de exemplo de código COMEÇA aqui" e "Chave de exemplo de código TERMINA aqui" para o ajudar a localizar o código reproduzidos de trechos de código abaixo.

```csharp
            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE STARTS HERE
            // Set http request header
            // ---------------------------------------------------------------------- 
            client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);
            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE ENDS HERE 
            // ----------------------------------------------------------------------

```
**Request(...)**  Abaixo código o trecho de código mostra como utilizar o HttlClient submeter a sua subscrição chave e dados pontos para o ponto final da API de deteção de anomalias.

```csharp
    public async Task<string> Request(string baseAddress, string endpoint, string subscriptionKey, string requestData)
    {
        using (HttpClient client = new HttpClient { BaseAddress = new Uri(baseAddress) })
        {
            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE STARTS HERE
            // Set http request header
            // ---------------------------------------------------------------------- 
            client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);
            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE ENDS HERE 
            // ----------------------------------------------------------------------

            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE STARTS HERE
            // Build the request content
            // ---------------------------------------------------------------------- 
            var content = new StringContent(requestData, Encoding.UTF8, "application/json");
            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE ENDS HERE 
            // ----------------------------------------------------------------------

            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE STARTS HERE
            // Send the request content with POST method.
            // ---------------------------------------------------------------------- 
            var res = await client.PostAsync(endpoint, content);
            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE ENDS HERE 
            // ----------------------------------------------------------------------
            if (res.IsSuccessStatusCode)
            {
                return await res.Content.ReadAsStringAsync();
            }
            else
            {
                return $"ErrorCode: {res.StatusCode}";
            }
        }
    }
```

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Referência da API REST](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
