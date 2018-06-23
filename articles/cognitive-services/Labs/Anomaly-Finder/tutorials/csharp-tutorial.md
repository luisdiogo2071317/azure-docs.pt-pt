---
title: Aplicação de deteção c# anomalias - serviços cognitivos Microsoft | Microsoft Docs
description: Explore uma aplicação de c# que utiliza a API de deteção de anomalias nos serviços cognitivos da Microsoft. Enviar pontos de dados original para a API e obter os pontos de anomalias e o valor esperado.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: 2e4100fd7d8e85a6b103c31000176aaaeb3d7151
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353360"
---
# <a name="anomaly-detection-c-application"></a>Aplicação de anomalias deteção c#

Explore uma aplicação básica do Windows que utiliza a API de deteção de anomalias para detetar anomalias da entrada. O exemplo envia os dados de séries de tempo para a API de deteção de anomalias com a sua chave de subscrição, em seguida, obtém todos os pontos de anomalias e valor esperado para cada ponto de dados a partir da API de.

## <a name="prerequisites"></a>Pré-requisitos

### <a name="platform-requirements"></a>Requisitos de plataforma

O exemplo tem foram desenvolvido para o .NET Framework utilizando [2017 do Visual Studio, edição Community](https://www.visualstudio.com/products/visual-studio-community-vs). 

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Subscrever a deteção de anomalias e obter uma chave de subscrição 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="get-and-use-the-example"></a>Obtenha e utilize o exemplo

Pode clonar a aplicação de exemplo de deteção de anomalias para o seu computador de [Github](https://github.com/MicrosoftAnomalyDetection/csharp-sample.git). 
<a name="Step1"></a>
### <a name="install-the-example"></a>Instalar o exemplo

No seu ambiente de trabalho do GitHub, abra Sample\AnomalyDetectionSample.sln.

<a name="Step2"></a>
### <a name="build-the-example"></a>O exemplo de compilação

Prima Ctrl + Shift + B, ou clique em compilação no menu do Friso e, em seguida, selecione compilar solução.

<a name="Step3"></a>
### <a name="run-the-example"></a>Executar o exemplo

1. Uma vez concluída a compilação, prima **F5** ou clique em **iniciar** no menu do friso para executar o exemplo.
2. Localize a janela de interface de utilizador de deteção de anomalias com a caixa de edição de texto ler "{your_subscription_key}".
3. Substituir o ficheiro de request.json, que contém os dados de exemplo, com os seus dados e, em seguida, clique no botão "Enviar". Microsoft recebe dados carregar e utilizá-los para detetar pontos de anomalias entre, em seguida. Os dados que atualizado não irão ser persistente no servidor da Microsoft. Para detetar o ponto de anomalias novamente, tem de carregar os dados novamente.
4. Se os dados são boas, irá encontrar o resultado de deteção de anomalias no campo "Resposta". Se qualquer erro ocorrer, as informações de erro serão apresentadas no campo de resposta.

<a name="Review"></a>
### <a name="read-the-result"></a>O resultado de leitura

[!INCLUDE [diagrams](../includes/diagrams.md)]

<a name="Review"></a>
### <a name="review-and-learn"></a>Reveja e saiba mais

Agora que tem uma aplicação em execução, vamos analisar como a aplicação de exemplo integra-se com a tecnologia de serviços cognitivos. Este passo tornarão mais fácil continuar edifício para esta aplicação ou para desenvolver as suas próprias aplicações com deteção de anomalias da Microsoft.

Esta aplicação de exemplo permite utilizar a API Restful da deteção de anomalias ponto final.

Rever como a API Restful obtém utilizada na aplicação de exemplo, vamos ver um fragmento de código do **AnomalyDetectionClient.cs**. O ficheiro contém a comentários de código que indica "Chave de exemplo código INICIA aqui" e "Chave de exemplo código TERMINA aqui" para o ajudar a localizar o código de fragmentos reproduzidos abaixo.

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
**Request(...)**  Abaixo código fragmento mostra como utilizar o HttlClient submeter a sua subscrição chave e os dados pontos para o ponto final da API de deteção de anomalias.

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
