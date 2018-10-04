---
title: Deteção de anomalias aplicação do Javascript - serviços cognitivos da Microsoft | Documentos da Microsoft
description: Explore uma aplicação Web de Javascript que usa a API de deteção de anomalias nos serviços cognitivos da Microsoft. Enviar pontos de dados original a API e obter o valor esperado e os pontos de anomalias.
services: cognitive-services
author: wenya
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: wenya
ms.openlocfilehash: 5bb123648a683454597b0561f9f82dffb70eab04
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/03/2018
ms.locfileid: "48248368"
---
# <a name="anomaly-detection-javascript-application"></a>Aplicação do Javascript de deteção de anomalias

[!INCLUDE [PrivatePreviewNote](../../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

Explore uma aplicação Web que utiliza a API de REST de deteção de anomalias para detetar uma anomalia. O exemplo envia os dados de séries de tempo para a API de deteção de anomalias com a sua chave de subscrição, em seguida, obtém todos os pontos de anomalias e o valor esperado para cada ponto de dados a partir da API.

## <a name="prerequisites"></a>Pré-requisitos

### <a name="platform-requirements"></a>Requisitos de plataforma

Este tutorial foi desenvolvido com um editor de texto simples.

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Subscrever a deteção de anomalias e obter uma chave de subscrição 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="get-and-use-the-example"></a>Obtenha e utilize o exemplo

Este tutorial disponibiliza dois cenários para deteção de anomalias de dados de séries de tempo. Vamos começar.

<a name="Step1"></a> 
### <a name="download-the-tutorial-project"></a>Transferir o projeto de tutorial

Clone o [Tutorial do deteção de anomalias de JavaScript do Cognitive Services](https://github.com/MicrosoftAnomalyDetection/javascript-sample), ou transferir o ficheiro. zip e extraí-lo num diretório vazio.

<a name="Step2"></a>
### <a name="run-the-example"></a>Executar o exemplo

Existem dois cenários, pode experimentar o exemplo.
1. Colocar seus **chave de subscrição** no campo de chave de subscrição na função no anomalydetection.html de detetar.
2. Colocar o ponto final de API de deteção de anomalias e certifique-se de que está a utilizar a região correta na região da subscrição.
3. Abra o **anomalydetection.html** arquivo num navegador da Web.

**Dados de séries de tempo semanais do cenário 1 detetar**
1. No campo período, o período de entrada **7**. 
2. Substituir os dados de exemplo com sua semanal tempo série pontos de dados (Json) no campo pontos ou usar diretamente os dados de exemplo.
3. Clique no botão de deteção de anomalias e verificar o resultado na caixa de texto de resposta correta.

**Cenário 2 detetar os dados de séries de tempo sem um período**
1. Deixe o período de vazio num período de campo, partem do princípio de que não sabe o período da série de tempo.
2. Usando os mesmos dados de séries de tempo como o cenário 1.
3. Clique no botão de deteção de anomalias e verifique se o campo período na caixa de texto de resposta correta.

<a name="Step3"></a>
### <a name="read-the-result"></a>O resultado de leitura

[!INCLUDE [diagrams](../includes/diagrams.md)]

<a name="Review"></a>
### <a name="review-and-learn"></a>Rever e aprender

Agora, obter um aplicativo em execução. Vamos rever como a aplicação de exemplo se integra com a tecnologia de serviços cognitivos. Este passo irá facilitar a continuar a criar nesta aplicação ou a desenvolver a sua própria aplicação com deteção de anomalias de Microsoft.
Esta aplicação de exemplo usa a API Restful de deteção de anomalias ponto final.
Rever como a API Restful, é utilizada no aplicativo de exemplo, vamos examinar um trecho de código do anomalydetection.html.
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
