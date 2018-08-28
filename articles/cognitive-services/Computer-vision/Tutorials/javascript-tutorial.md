---
title: Tutorial de visão API JavaScript do computador | Microsoft Docs
description: Explore uma aplicação JavaScript básica que utiliza a API de visão de computador nos serviços cognitivos da Microsoft. Efetuar OCR, criar miniaturas e trabalhar com funcionalidades visual numa imagem.
services: cognitive-services
author: KellyDF
manager: corncar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 09/19/2017
ms.author: kefre
ms.openlocfilehash: 89bdc0524e07c1cb6a1473e0a52791fe20271e06
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351913"
---
# <a name="computer-vision-api-javascript-tutorial"></a>Tutorial de visão API JavaScript do computador

Este tutorial mostra as funcionalidades da Microsoft cognitivos serviços computador visão API REST.

Explore uma aplicação do JavaScript que utiliza a API de REST de visão do computador para executar optical caráter reconhecimento (OCR), criar miniaturas smart-recortada, plus detetar, categorizar etiqueta e descrevem visual funcionalidades, incluindo faces, numa imagem. Este exemplo permite-lhe um URL de imagem para análise ou processamento de envio. Pode utilizar este exemplo de código aberto como um modelo para criar uma aplicação em JavaScript para utilizar a API de REST de visão do computador.

A aplicação de forma JavaScript já foi escrita, mas não tem nenhuma funcionalidade visão do computador. Neste tutorial, adicione o código específico para a API de REST de visão de computador para concluir a funcionalidade da aplicação.

## <a name="prerequisites"></a>Pré-requisitos

### <a name="platform-requirements"></a>Requisitos de plataforma

Este tutorial tem foram desenvolvido utilizando um editor de texto simples.

### <a name="subscribe-to-computer-vision-api-and-get-a-subscription-key"></a>Subscrever a API de visão do computador e obter uma chave de subscrição 

Antes de criar o exemplo, tem de subscrever API de visão do computador que faz parte dos serviços cognitivos da Microsoft. Para detalhes de gestão de chaves e da subscrição, consulte [subscrições](https://azure.microsoft.com/try/cognitive-services/). As chaves primárias e secundárias são válidas para utilizar neste tutorial. 

## <a name="download-the-tutorial-project"></a>Transferir o projeto tutorial

Clone o [cognitivos serviços JavaScript computador visão Tutorial](https://github.com/Azure-Samples/cognitive-services-javascript-computer-vision-tutorial), ou transfira o ficheiro. zip e extraia-o para um diretório vazio.

Se preferir utilizar o tutorial foi terminado com o código do tutorial todos os adicionado, pode utilizar os ficheiros no **concluído** pasta.

## <a name="add-the-tutorial-code"></a>Adicione o código do tutorial

A aplicação do JavaScript é configurada com seis .HTML os ficheiros, um para cada funcionalidade. Cada ficheiro demonstra uma função diferente da visão de computador (analisar OCR, etc.). As secções tutorial seis não dispõe de interdependencies, pelo que pode adicionar o código do tutorial para um ficheiro, todos os ficheiros de seis ou apenas alguns dos ficheiros. E pode adicionar o código do tutorial para os ficheiros em qualquer ordem.

Vamos começar.

## <a name="analyze-an-image"></a>Analisar uma imagem

A funcionalidade de análise do computador visão analisa uma imagem para mais de 2.000 objetos reconhecível, beings de maior duração, scenery e ações. Assim que a análise estiver concluída, analisar devolve um objeto JSON que descreve a imagem com etiquetas descritivas, análise de cor, legendas e muito mais.

Para concluir a funcionalidade de análise do tutorial da aplicação, execute os seguintes passos:

### <a name="analyze-step-1-add-the-event-handler-code-for-the-form-button"></a>Analisar o passo 1: Adicione o código de processador de eventos para o botão de formulário

Abra o **analyze.html** ficheiro num editor de texto e localize o **analyzeButtonClick** função perto do fim do ficheiro.

O **analyzeButtonClick** função de processador de eventos limpa o formulário, apresenta a imagem especificada no URL, em seguida, chama o **AnalyzeImage** função para analisar a imagem.

Copie e cole o seguinte código para o **analyzeButtonClick** função.

```javascript
function analyzeButtonClick() {

    // Clear the display fields.
    $("#sourceImage").attr("src", "#");
    $("#responseTextArea").val("");
    $("#captionSpan").text("");
    
    // Display the image.
    var sourceImageUrl = $("#inputImage").val();
    $("#sourceImage").attr("src", sourceImageUrl);
    
    AnalyzeImage(sourceImageUrl, $("#responseTextArea"), $("#captionSpan"));
}
```

### <a name="analyze-step-2-add-the-wrapper-for-the-rest-api-call"></a>Analisar o passo 2: adicionar o wrapper para a chamada de REST API

O **AnalyzeImage** função encapsula num wrapper a chamada de REST API para analisar uma imagem. Após um retorno com êxito, a análise JSON formatada será apresentada no textarea especificado e a legenda será apresentada no intervalo especificado.

Copie e cole o **AnalyzeImage** funcionar código para apenas por baixo do **analyzeButtonClick** função.

```javascript
/* Analyze the image at the specified URL by using Microsoft Cognitive Services Analyze Image API.
 * @param {string} sourceImageUrl - The URL to the image to analyze.
 * @param {<textarea> element} responseTextArea - The text area to display the JSON string returned
 *                             from the REST API call, or to display the error message if there was 
 *                             an error.
 * @param {<span> element} captionSpan - The span to display the image caption.
 */
function AnalyzeImage(sourceImageUrl, responseTextArea, captionSpan) {
    // Request parameters.
    var params = {
        "visualFeatures": "Categories,Description,Color",
        "details": "",
        "language": "en",
    };
    
    // Perform the REST API call.
    $.ajax({
        url: common.uriBasePreRegion + 
             $("#subscriptionRegionSelect").val() + 
             common.uriBasePostRegion + 
             common.uriBaseAnalyze +
             "?" + 
             $.param(params),
                    
        // Request headers.
        beforeSend: function(jqXHR){
            jqXHR.setRequestHeader("Content-Type","application/json");
            jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key", 
                encodeURIComponent($("#subscriptionKeyInput").val()));
        },
        
        type: "POST",
        
        // Request body.
        data: '{"url": ' + '"' + sourceImageUrl + '"}',
    })
    
    .done(function(data) {
        // Show formatted JSON on webpage.
        responseTextArea.val(JSON.stringify(data, null, 2));
        
        // Extract and display the caption and confidence from the first caption in the description object.
        if (data.description && data.description.captions) {
            var caption = data.description.captions[0];
            
            if (caption.text && caption.confidence) {
                captionSpan.text("Caption: " + caption.text +
                    " (confidence: " + caption.confidence + ").");
            }
        }
    })
    
    .fail(function(jqXHR, textStatus, errorThrown) {
        // Prepare the error string.
        var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
        errorString += (jqXHR.responseText === "") ? "" : (jQuery.parseJSON(jqXHR.responseText).message) ? 
            jQuery.parseJSON(jqXHR.responseText).message : jQuery.parseJSON(jqXHR.responseText).error.message;
        
        // Put the error JSON in the response textarea.
        responseTextArea.val(JSON.stringify(jqXHR, null, 2));
        
        // Show the error message.
        alert(errorString);
    });
}
```

### <a name="analyze-step-3-run-the-application"></a>Analisar o passo 3: executar a aplicação

Guardar o **analyze.html** de ficheiros e abra-o num browser. Colocar a sua chave de subscrição para o **chave de subscrição** campo e certifique-se de que está a utilizar a região correta no **subscrição região**. Introduza um URL numa imagem para analisar, em seguida, clique em de **analisar imagem** botão para analisar uma imagem e ver o resultado.

## <a name="recognize-a-landmark"></a>Reconhecer um landmark

A funcionalidade de Landmark do computador visão analisa uma imagem para landmarks naturais e artificial, tais como mountains ou edifícios famosa. Assim que a análise estiver concluída, Landmark devolve um objeto JSON que identifica os landmarks encontrados na imagem.

Para concluir a funcionalidade de Landmark do tutorial da aplicação, execute os seguintes passos:

### <a name="landmark-step-1-add-the-event-handler-code-for-the-form-button"></a>Passo 1 da landmark: Adicione o código de processador de eventos para o botão de formulário

Abra o **landmark.html** ficheiro num editor de texto e localize o **landmarkButtonClick** função perto do fim do ficheiro.

O **landmarkButtonClick** função de processador de eventos limpa o formulário, apresenta a imagem especificada no URL, em seguida, chama o **IdentifyLandmarks** função para analisar a imagem.

Copie e cole o seguinte código para o **landmarkButtonClick** função.

```javascript
function landmarkButtonClick() {

    // Clear the display fields.
    $("#sourceImage").attr("src", "#");
    $("#responseTextArea").val("");
    $("#captionSpan").text("");
    
    // Display the image.
    var sourceImageUrl = $("#inputImage").val();
    $("#sourceImage").attr("src", sourceImageUrl);
    
    IdentifyLandmarks(sourceImageUrl, $("#responseTextArea"), $("#captionSpan"));
}
```

### <a name="landmark-step-2-add-the-wrapper-for-the-rest-api-call"></a>Passo landmark 2: adicionar o wrapper para a chamada de REST API

O **IdentifyLandmarks** função encapsula num wrapper a chamada de REST API para analisar uma imagem. Após um retorno com êxito, a análise JSON formatada será apresentada no textarea especificado e a legenda será apresentada no intervalo especificado.

Copie e cole o **IdentifyLandmarks** funcionar código para apenas por baixo do **landmarkButtonClick** função.

```javascript
/* Identify landmarks in the image at the specified URL by using Microsoft Cognitive Services 
 * Landmarks API.
 * @param {string} sourceImageUrl - The URL to the image to analyze for landmarks.
 * @param {<textarea> element} responseTextArea - The text area to display the JSON string returned
 *                             from the REST API call, or to display the error message if there was 
 *                             an error.
 * @param {<span> element} captionSpan - The span to display the image caption.
 */
function IdentifyLandmarks(sourceImageUrl, responseTextArea, captionSpan) {
    // Request parameters.
    var params = {
        "model": "landmarks"
    };
    
    // Perform the REST API call.
    $.ajax({
        url: common.uriBasePreRegion + 
             $("#subscriptionRegionSelect").val() + 
             common.uriBasePostRegion + 
             common.uriBaseLandmark +
             "?" + 
             $.param(params),
                    
        // Request headers.
        beforeSend: function(jqXHR){
            jqXHR.setRequestHeader("Content-Type","application/json");
            jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key", 
                encodeURIComponent($("#subscriptionKeyInput").val()));
        },
        
        type: "POST",
        
        // Request body.
        data: '{"url": ' + '"' + sourceImageUrl + '"}',
    })
    
    .done(function(data) {
        // Show formatted JSON on webpage.
        responseTextArea.val(JSON.stringify(data, null, 2));
        
        // Extract and display the caption and confidence from the first caption in the description object.
        if (data.result && data.result.landmarks) {
            var landmark = data.result.landmarks[0];
            
            if (landmark.name && landmark.confidence) {
                captionSpan.text("Landmark: " + landmark.name +
                    " (confidence: " + landmark.confidence + ").");
            }
        }
    })
    
    .fail(function(jqXHR, textStatus, errorThrown) {
        // Prepare the error string.
        var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
        errorString += (jqXHR.responseText === "") ? "" : (jQuery.parseJSON(jqXHR.responseText).message) ? 
            jQuery.parseJSON(jqXHR.responseText).message : jQuery.parseJSON(jqXHR.responseText).error.message;
        
        // Put the error JSON in the response textarea.
        responseTextArea.val(JSON.stringify(jqXHR, null, 2));
        
        // Show the error message.
        alert(errorString);
    });
}
```

### <a name="landmark-step-3-run-the-application"></a>Passo landmark 3: executar a aplicação

Guardar o **landmark.html** de ficheiros e abra-o num browser. Colocar a sua chave de subscrição para o **chave de subscrição** campo e certifique-se de que está a utilizar a região correta no **subscrição região**. Introduza um URL numa imagem para analisar, em seguida, clique em de **analisar imagem** botão para analisar uma imagem e ver o resultado.

## <a name="recognize-celebrities"></a>Reconhecer celebridades

A funcionalidade de celebridades para do computador visão analisa uma imagem para famosa pessoas. Assim que a análise estiver concluída, celebridades para devolve um objeto JSON que identifica as celebridades para encontrar na imagem.

Para concluir a funcionalidade de celebridades para do tutorial da aplicação, execute os seguintes passos:

### <a name="celebrities-step-1-add-the-event-handler-code-for-the-form-button"></a>Celebridades para passo 1: Adicione o código de processador de eventos para o botão de formulário

Abra o **celebrities.html** ficheiro num editor de texto e localize o **celebritiesButtonClick** função perto do fim do ficheiro.

O **celebritiesButtonClick** função de processador de eventos limpa o formulário, apresenta a imagem especificada no URL, em seguida, chama o **IdentifyCelebrities** função para analisar a imagem.

Copie e cole o seguinte código para o **celebritiesButtonClick** função.

```javascript
function celebritiesButtonClick() {

    // Clear the display fields.
    $("#sourceImage").attr("src", "#");
    $("#responseTextArea").val("");
    $("#captionSpan").text("");
    
    // Display the image.
    var sourceImageUrl = $("#inputImage").val();
    $("#sourceImage").attr("src", sourceImageUrl);
    
    IdentifyCelebrities(sourceImageUrl, $("#responseTextArea"), $("#captionSpan"));
}
```

### <a name="celebrities-step-2-add-the-wrapper-for-the-rest-api-call"></a>Celebridades para passo 2: adicionar o wrapper para a chamada de REST API

```javascript
/* Identify celebrities in the image at the specified URL by using Microsoft Cognitive Services 
 * Celebrities API.
 * @param {string} sourceImageUrl - The URL to the image to analyze for celebrities.
 * @param {<textarea> element} responseTextArea - The text area to display the JSON string returned
 *                             from the REST API call, or to display the error message if there was 
 *                             an error.
 * @param {<span> element} captionSpan - The span to display the image caption.
 */
function IdentifyCelebrities(sourceImageUrl, responseTextArea, captionSpan) {
    // Request parameters.
    var params = {
        "model": "celebrities"
    };
    
    // Perform the REST API call.
    $.ajax({
        url: common.uriBasePreRegion + 
             $("#subscriptionRegionSelect").val() + 
             common.uriBasePostRegion + 
             common.uriBaseCelebrities +
             "?" + 
             $.param(params),
                    
        // Request headers.
        beforeSend: function(jqXHR){
            jqXHR.setRequestHeader("Content-Type","application/json");
            jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key", 
                encodeURIComponent($("#subscriptionKeyInput").val()));
        },
        
        type: "POST",
        
        // Request body.
        data: '{"url": ' + '"' + sourceImageUrl + '"}',
    })
    
    .done(function(data) {
        // Show formatted JSON on webpage.
        responseTextArea.val(JSON.stringify(data, null, 2));
        
        // Extract and display the caption and confidence from the first caption in the description object.
        if (data.result && data.result.celebrities) {
            var celebrity = data.result.celebrities[0];
            
            if (celebrity.name && celebrity.confidence) {
                captionSpan.text("Celebrity name: " + celebrity.name +
                    " (confidence: " + celebrity.confidence + ").");
            }
        }
    })
    
    .fail(function(jqXHR, textStatus, errorThrown) {
        // Prepare the error string.
        var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
        errorString += (jqXHR.responseText === "") ? "" : (jQuery.parseJSON(jqXHR.responseText).message) ? 
            jQuery.parseJSON(jqXHR.responseText).message : jQuery.parseJSON(jqXHR.responseText).error.message;
        
        // Put the error JSON in the response textarea.
        responseTextArea.val(JSON.stringify(jqXHR, null, 2));
        
        // Show the error message.
        alert(errorString);
    });
}
```

### <a name="celebrities-step-3-run-the-application"></a>Celebridades para passo 3: executar a aplicação

Guardar o **celebrities.html** de ficheiros e abra-o num browser. Colocar a sua chave de subscrição para o **chave de subscrição** campo e certifique-se de que está a utilizar a região correta no **subscrição região**. Introduza um URL numa imagem para analisar, em seguida, clique em de **analisar imagem** botão para analisar uma imagem e ver o resultado.

## <a name="intelligently-generate-a-thumbnail"></a>Inteligentemente gerar uma miniatura

A funcionalidade de miniatura da visão de computador de gera uma miniatura a partir de uma imagem. Utilizando o **cortar inteligente** funcionalidade, a funcionalidade de miniatura identificará a área de interesse numa imagem e o Centro de miniatura nesta área, para gerar mais aesthetically pleasing imagens em miniatura.

Para concluir a funcionalidade de miniatura do tutorial da aplicação, execute os seguintes passos:

### <a name="thumbnail-step-1-add-the-event-handler-code-for-the-form-button"></a>Em miniatura passo 1: Adicione o código de processador de eventos para o botão de formulário

Abra o **thumbnail.html** ficheiro num editor de texto e localize o **thumbnailButtonClick** função perto do fim do ficheiro.

O **thumbnailButtonClick** função de processador de eventos limpa o formulário, apresenta a imagem especificada no URL, em seguida, chama o **getThumbnail** função duas vezes para criar dois miniaturas, um smart recortada e um sem cortar inteligente.

Copie e cole o seguinte código para o **thumbnailButtonClick** função.

```javascript
function thumbnailButtonClick() {

    // Clear the display fields.
    document.getElementById("sourceImage").src = "#";
    document.getElementById("thumbnailImageSmartCrop").src = "#";
    document.getElementById("thumbnailImageNonSmartCrop").src = "#";
    document.getElementById("responseTextArea").value = "";
    document.getElementById("captionSpan").text = "";
    
    // Display the image.
    var sourceImageUrl = document.getElementById("inputImage").value;
    document.getElementById("sourceImage").src = sourceImageUrl;
    
    // Get a smart cropped thumbnail.
    getThumbnail (sourceImageUrl, true, document.getElementById("thumbnailImageSmartCrop"), 
        document.getElementById("responseTextArea"));
    
    // Get a non-smart-cropped thumbnail.
    getThumbnail (sourceImageUrl, false, document.getElementById("thumbnailImageNonSmartCrop"),
        document.getElementById("responseTextArea"));
}
```

### <a name="thumbnail-step-2-add-the-wrapper-for-the-rest-api-call"></a>Em miniatura passo 2: adicionar o wrapper para a chamada de REST API

O **getThumbnail** função encapsula num wrapper a chamada de REST API para analisar uma imagem. Após um retorno com êxito, será apresentada a miniatura no elemento img especificado.

Copie e cole o seguinte **getThumbnail** função para apenas por baixo do **thumbnailButtonClick** função.

```javascript
/* Get a thumbnail of the image at the specified URL by using Microsoft Cognitive Services
 * Thumbnail API.
 * @param {string} sourceImageUrl URL to image.
 * @param {boolean} smartCropping Set to true to use the smart cropping feature which crops to the
 *                                more interesting area of an image; false to crop for the center
 *                                of the image.
 * @param {<img> element} imageElement The img element in the DOM which will display the thumnail image.
 * @param {<textarea> element} responseTextArea - The text area to display the Response Headers returned
 *                             from the REST API call, or to display the error message if there was 
 *                             an error.
 */
function getThumbnail (sourceImageUrl, smartCropping, imageElement, responseTextArea) {
    // Create the HTTP Request object.
    var xhr = new XMLHttpRequest();
    
    // Request parameters.
    var params = "width=100&height=150&smartCropping=" + smartCropping.toString();

    // Build the full URI.
    var fullUri = common.uriBasePreRegion + 
                  document.getElementById("subscriptionRegionSelect").value + 
                  common.uriBasePostRegion + 
                  common.uriBaseThumbnail +
                  "?" + 
                  params;
    
    // Identify the request as a POST, with the URI and parameters.
    xhr.open("POST", fullUri);
    
    // Add the request headers.
    xhr.setRequestHeader("Content-Type","application/json");
    xhr.setRequestHeader("Ocp-Apim-Subscription-Key", 
        encodeURIComponent(document.getElementById("subscriptionKeyInput").value));
    
    // Set the response type to "blob" for the thumbnail image data.
    xhr.responseType = "blob";
    
    // Process the result of the REST API call.
    xhr.onreadystatechange = function(e) {
        if(xhr.readyState === XMLHttpRequest.DONE) {
            
            // Thumbnail successfully created.
            if (xhr.status === 200) {
                // Show response headers.
                var s = JSON.stringify(xhr.getAllResponseHeaders(), null, 2);
                responseTextArea.value = JSON.stringify(xhr.getAllResponseHeaders(), null, 2);
                
                // Show thumbnail image.
                var urlCreator = window.URL || window.webkitURL;
                var imageUrl = urlCreator.createObjectURL(this.response);
                imageElement.src = imageUrl;
            } else {
                // Display the error message. The error message is the response body as a JSON string. 
                // The code in this code block extracts the JSON string from the blob response.
                var reader = new FileReader();
                
                // This event fires after the blob has been read.
                reader.addEventListener('loadend', (e) => {
                    responseTextArea.value = JSON.stringify(JSON.parse(e.srcElement.result), null, 2);
                });
                
                // Start reading the blob as text.
                reader.readAsText(xhr.response);
            }
        }
    }
    
    // Execute the REST API call.
    xhr.send('{"url": ' + '"' + sourceImageUrl + '"}');
}
```

### <a name="thumbnail-step-3-run-the-application"></a>Em miniatura passo 3: executar a aplicação

Guardar o **thumbnail.html** de ficheiros e abra-o num browser. Colocar a sua chave de subscrição para o **chave de subscrição** campo e certifique-se de que está a utilizar a região correta no **subscrição região**. Introduza um URL numa imagem para analisar, em seguida, clique em de **gerar miniaturas** botão para analisar uma imagem e ver o resultado.

## <a name="read-printed-text-ocr"></a>Leia o texto impressos (OCR)

A funcionalidade de reconhecimento de caráter Optical (OCR) do computador visão analisa uma imagem de texto impressos. Depois da análise estiver concluída, OCR devolve um objeto JSON que contém o texto e a localização do texto na imagem.

Para concluir a funcionalidade de OCR do tutorial da aplicação, execute os seguintes passos:

### <a name="ocr-step-1-add-the-event-handler-code-for-the-form-button"></a>Passo 1 da OCR: Adicione o código de processador de eventos para o botão de formulário

Abra o **ocr.html** ficheiro num editor de texto e localize o **ocrButtonClick** função perto do fim do ficheiro.

O **ocrButtonClick** função de processador de eventos limpa o formulário, apresenta a imagem especificada no URL, em seguida, chama o **ReadOcrImage** função para analisar a imagem.

Copie e cole o seguinte código para o **ocrButtonClick** função.

```javascript
function ocrButtonClick() {

    // Clear the display fields.
    $("#sourceImage").attr("src", "#");
    $("#responseTextArea").val("");
    $("#captionSpan").text("");
    
    // Display the image.
    var sourceImageUrl = $("#inputImage").val();
    $("#sourceImage").attr("src", sourceImageUrl);
    
    ReadOcrImage(sourceImageUrl, $("#responseTextArea"));
}
```

### <a name="ocr-step-2-add-the-wrapper-for-the-rest-api-call"></a>Passo OCR 2: adicionar o wrapper para a chamada de REST API

O **ReadOcrImage** função encapsula num wrapper a chamada de REST API para analisar uma imagem. Após um bem-sucedida voltar, o formato JSON que descrevem o texto e o textarea especificado será apresentada a localização do texto.

Copie e cole o seguinte **ReadOcrImage** função para apenas por baixo do **ocrButtonClick** função.

```javascript
/* Recognize and read printed text in an image at the specified URL by using Microsoft Cognitive 
 * Services OCR API.
 * @param {string} sourceImageUrl - The URL to the image to analyze for printed text.
 * @param {<textarea> element} responseTextArea - The text area to display the JSON string returned
 *                             from the REST API call, or to display the error message if there was 
 *                             an error.
 */
function ReadOcrImage(sourceImageUrl, responseTextArea) {
    // Request parameters.
    var params = {
        "language": "unk",
        "detectOrientation ": "true",
    };

    // Perform the REST API call.
    $.ajax({
        url: common.uriBasePreRegion + 
             $("#subscriptionRegionSelect").val() + 
             common.uriBasePostRegion + 
             common.uriBaseOcr +
             "?" + 
             $.param(params),
        
        // Request headers.
        beforeSend: function(jqXHR){
            jqXHR.setRequestHeader("Content-Type","application/json");
            jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key", 
                encodeURIComponent($("#subscriptionKeyInput").val()));
        },
        
        type: "POST",
        
        // Request body.
        data: '{"url": ' + '"' + sourceImageUrl + '"}',
    })
    
    .done(function(data) {
        // Show formatted JSON on webpage.
        responseTextArea.val(JSON.stringify(data, null, 2));
    })
    
    .fail(function(jqXHR, textStatus, errorThrown) {
        // Put the JSON description into the text area.
        responseTextArea.val(JSON.stringify(jqXHR, null, 2));
        
        // Display error message.
        var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
        errorString += (jqXHR.responseText === "") ? "" : (jQuery.parseJSON(jqXHR.responseText).message) ? 
            jQuery.parseJSON(jqXHR.responseText).message : jQuery.parseJSON(jqXHR.responseText).error.message;
        alert(errorString);
    });
}
```

### <a name="ocr-step-3-run-the-application"></a>Passo OCR 3: executar a aplicação

Guardar o **ocr.html** de ficheiros e abra-o num browser. Colocar a sua chave de subscrição para o **chave de subscrição** campo e certifique-se de que está a utilizar a região correta no **subscrição região**. Introduza um URL numa imagem do texto para ler, em seguida, clique em de **leitura imagem** botão para analisar uma imagem e ver o resultado.

## <a name="read-handwritten-text-handwriting-recognition"></a>Leia o texto handwritten (reconhecimento de escrita manual)

A funcionalidade de reconhecimento de escrita manual do computador visão analisa uma imagem de texto handwritten. Depois da análise estiver concluída, o reconhecimento de escrita manual devolve um objeto JSON que contém o texto e a localização do texto na imagem.

Para concluir a funcionalidade de reconhecimento de escrita manual do tutorial da aplicação, execute os seguintes passos:

### <a name="handwriting-recognition-step-1-add-the-event-handler-code-for-the-form-button"></a>O passo de reconhecimento de escrita 1: Adicione o código de processador de eventos para o botão de formulário

Abra o **handwriting.html** ficheiro num editor de texto e localize o **handwritingButtonClick** função perto do fim do ficheiro.

O **handwritingButtonClick** função de processador de eventos limpa o formulário, apresenta a imagem especificada no URL, em seguida, chama o **HandwritingImage** função para analisar a imagem.

Copie e cole o seguinte código para o **handwritingButtonClick** função.

```javascript
function handwritingButtonClick() {

    // Clear the display fields.
    $("#sourceImage").attr("src", "#");
    $("#responseTextArea").val("");
    
    // Display the image.
    var sourceImageUrl = $("#inputImage").val();
    $("#sourceImage").attr("src", sourceImageUrl);
    
    ReadHandwrittenImage(sourceImageUrl, $("#responseTextArea"));
}
```

### <a name="handwriting-recognition-step-2-add-the-wrapper-for-the-rest-api-call"></a>O passo de reconhecimento de escrita 2: adicionar o wrapper para a chamada de REST API

O **ReadHandwrittenImage** função encapsula num wrapper as dois chamadas da REST API necessárias para analisar uma imagem. Porque o reconhecimento de escrita manual é um processo moroso, é utilizado um processo de dois passos. A primeira chamada submete a imagem para o processamento; a segunda chamada obtém o texto detetado quando o processamento é concluído.

Depois do texto é obtido, o formato JSON que descrevem o texto e a localização do texto será apresentado no textarea especificado.

Copie e cole o seguinte **ReadHandwrittenImage** função para apenas por baixo do **handwritingButtonClick** função.

```javascript
/* Recognize and read text from an image of handwriting at the specified URL by using Microsoft 
 * Cognitive Services Recognize Handwritten Text API.
 * @param {string} sourceImageUrl - The URL to the image to analyze for handwriting.
 * @param {<textarea> element} responseTextArea - The text area to display the JSON string returned
 *                             from the REST API call, or to display the error message if there was 
 *                             an error.
 */
function ReadHandwrittenImage(sourceImageUrl, responseTextArea) {
    // Request parameters.
    var params = {
        "handwriting": "true",
    };

    // This operation requrires two REST API calls. One to submit the image for processing,
    // the other to retrieve the text found in the image. 
    //
    // Perform the first REST API call to submit the image for processing.
    $.ajax({
        url: common.uriBasePreRegion + 
             $("#subscriptionRegionSelect").val() + 
             common.uriBasePostRegion + 
             common.uriBaseHandwriting +
             "?" + 
             $.param(params),
        
        // Request headers.
        beforeSend: function(jqXHR){
            jqXHR.setRequestHeader("Content-Type","application/json");
            jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key", 
                encodeURIComponent($("#subscriptionKeyInput").val()));
        },
        
        type: "POST",
        
        // Request body.
        data: '{"url": ' + '"' + sourceImageUrl + '"}',
    })
    
    .done(function(data, textStatus, jqXHR) {
        // Show progress.
        responseTextArea.val("Handwritten image submitted.");
        
        // Note: The response may not be immediately available. Handwriting Recognition is an
        // async operation that can take a variable amount of time depending on the length
        // of the text you want to recognize. You may need to wait or retry this GET operation.
        //
        // Try once per second for up to ten seconds to receive the result.
        var tries = 10;
        var waitTime = 100;
        var taskCompleted = false;
        
        var timeoutID = setInterval(function () { 
            // Limit the number of calls.
            if (--tries <= 0) {
                window.clearTimeout(timeoutID);
                responseTextArea.val("The response was not available in the time allowed.");
                return;
            }

            // The "Operation-Location" in the response contains the URI to retrieve the recognized text.
            var operationLocation = jqXHR.getResponseHeader("Operation-Location");
            
            // Perform the second REST API call and get the response.
            $.ajax({
                url: operationLocation,
                
                // Request headers.
                beforeSend: function(jqXHR){
                    jqXHR.setRequestHeader("Content-Type","application/json");
                    jqXHR.setRequestHeader("Ocp-Apim-Subscription-Key",
                        encodeURIComponent($("#subscriptionKeyInput").val()));
                },
                
                type: "GET",
            })
            
            .done(function(data) {
                // If the result is not yet available, return.
                if (data.status && (data.status === "NotStarted" || data.status === "Running")) {
                    return;
                }
                
                // Show formatted JSON on webpage.
                responseTextArea.val(JSON.stringify(data, null, 2));
                
                // Indicate the task is complete and clear the timer.
                taskCompleted = true;
                window.clearTimeout(timeoutID);
            })
            
            .fail(function(jqXHR, textStatus, errorThrown) {
                // Indicate the task is complete and clear the timer.
                taskCompleted = true;
                window.clearTimeout(timeoutID);
                
                // Display error message.
                var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
                errorString += (jqXHR.responseText === "") ? "" : (jQuery.parseJSON(jqXHR.responseText).message) ? 
                    jQuery.parseJSON(jqXHR.responseText).message : jQuery.parseJSON(jqXHR.responseText).error.message;
                alert(errorString);
            });
        }, waitTime);
    })
    
    .fail(function(jqXHR, textStatus, errorThrown) {
        // Put the JSON description into the text area.
        responseTextArea.val(JSON.stringify(jqXHR, null, 2));
        
        // Display error message.
        var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
        errorString += (jqXHR.responseText === "") ? "" : (jQuery.parseJSON(jqXHR.responseText).message) ? 
            jQuery.parseJSON(jqXHR.responseText).message : jQuery.parseJSON(jqXHR.responseText).error.message;
        alert(errorString);
    });
}
```

### <a name="handwriting-recognition-step-3-run-the-application"></a>O passo de reconhecimento de escrita 3: executar a aplicação

Guardar o **handwriting.html** de ficheiros e abra-o num browser. Colocar a sua chave de subscrição para o **chave de subscrição** campo e certifique-se de que está a utilizar a região correta no **subscrição região**. Introduza um URL numa imagem do texto para ler, em seguida, clique em de **leitura imagem** botão para analisar uma imagem e ver o resultado.

## <a name="next-steps"></a>Passos Seguintes

- [Computador visão API C&#35; Tutorial](CSharpTutorial.md)
- [Tutorial de Python de API de visão do computador](PythonTutorial.md)
