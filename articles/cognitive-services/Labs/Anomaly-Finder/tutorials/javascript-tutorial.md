---
title: Deteção de anomalias aplicação Javascript - serviços cognitivos Microsoft | Microsoft Docs
description: Explore uma aplicação Web de Javascript que utiliza a API de deteção de anomalias nos serviços cognitivos da Microsoft. Enviar pontos de dados original para a API e obter os pontos de anomalias e o valor esperado.
services: cognitive-services
author: wenya
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: wenya
ms.openlocfilehash: 42c3941a05efe8b74f818cd99f3606b3073892a9
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353336"
---
# <a name="anomaly-detection-javascript-application"></a>Aplicação do Javascript de deteção de anomalias

Explore uma aplicação Web que utiliza a API de REST de deteção de anomalias para detetar uma anomalias. O exemplo envia os dados de séries de tempo para a API de deteção de anomalias com a sua chave de subscrição, em seguida, obtém todos os pontos de anomalias e o valor esperado para cada ponto de dados a partir da API de.

## <a name="prerequisites"></a>Pré-requisitos

### <a name="platform-requirements"></a>Requisitos de plataforma

Este tutorial tem foram desenvolvido utilizando um editor de texto simples.

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Subscrever a deteção de anomalias e obter uma chave de subscrição 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="get-and-use-the-example"></a>Obtenha e utilize o exemplo

Este tutorial fornece dois cenários de deteção de anomalias de dados de séries de tempo. Vamos começar.

<a name="Step1"></a> 
### <a name="download-the-tutorial-project"></a>Transferir o projeto tutorial

Clone o [cognitivos Tutorial de deteção de anomalias do serviços de JavaScript](https://github.com/MicrosoftAnomalyDetection/javascript-sample), ou transfira o ficheiro. zip e extraia-o para um diretório vazio.

<a name="Step2"></a>
### <a name="run-the-example"></a>Executar o exemplo

Existem dois cenários, pode experimentar o exemplo.
1. Coloque o **chave de subscrição** para o campo de chave de subscrição no detetar funcionam em anomalydetection.html.
2. Colocar o ponto final de API de deteção de anomalias e certifique-se de que está a utilizar a região correta na região de subscrição.
3. Abra o **anomalydetection.html** ficheiro num Web browser.

**Dados de séries de tempo semanais do cenário 1 detetar**
1. No campo período, o período de entrada **7**. 
2. Substituir os dados de exemplo com os semanal tempo série pontos de dados (Json) no campo pontos ou utilizar os dados de exemplo diretamente.
3. Clique no botão de deteção de anomalias e verificar o resultado na caixa de texto de resposta à direita.

**Cenário 2 detetar os dados de séries de tempo sem um período**
1. Deixe o período de vazio no período registado, parta do princípio de que não souber o período de série de tempo.
2. Utilizando os mesmos dados de séries de tempo que o cenário 1.
3. Clique no botão de deteção de anomalias e verifique se o campo de período na caixa de texto de resposta à direita.

<a name="Step3"></a>
### <a name="read-the-result"></a>O resultado de leitura

[!INCLUDE [diagrams](../includes/diagrams.md)]

<a name="Review"></a>
### <a name="review-and-learn"></a>Reveja e saiba mais

Agora, obter uma aplicação em execução. Vamos rever como a aplicação de exemplo integra-se com a tecnologia de serviços cognitivos. Este passo tornarão mais fácil continuar a criar esta aplicação ou para desenvolver as suas próprias aplicações com deteção de anomalias da Microsoft.
Esta aplicação de exemplo permite utilizar a API Restful da deteção de anomalias ponto final.
Rever como a API Restful obtém utilizada na aplicação de exemplo, vamos ver um fragmento de código do anomalydetection.html.
```JavaScript
function anomalyDetection(url, subscriptionKey, points, period) {
    var obj = new Object();
    obj.Points = JSON.parse(points); // this points are read from text box.
    obj.Period = parseInt(period);//period=7 weekly period
    var tsData = JSON.stringify(obj);
    // Perform the REST API call.
    $.ajax({
        url: url, //Anomaly Detection API endpoint
        // Request headers.
        beforeSend: function (xhrObj) {
            xhrObj.setRequestHeader("Content-Type", "application/json");
            xhrObj.setRequestHeader("Ocp-Apim-Subscription-Key", subscriptionKey); // Replace your subscription key
        },
        type: "POST",
        // Request body.
        data: tsData, // json format
        })
        .done(function (data) {
            // Show formatted JSON on webpage.
            $("#responseTextArea").val(JSON.stringify(data, null, 2));
        })
        .fail(function (jqXHR, textStatus, errorThrown) {
            // Display error message.
            var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
            errorString += (jqXHR.responseText === "") ? "" : jQuery.parseJSON(jqXHR.responseText).message;
            $("#responseTextArea").val(errorString);           
        });
}

```

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Referência da API REST](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
