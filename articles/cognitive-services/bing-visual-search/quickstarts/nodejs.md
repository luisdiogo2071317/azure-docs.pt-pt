---
title: Início rápido de JavaScript para a API de pesquisa Visual do Bing | Documentos da Microsoft
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Mostra como carregar uma imagem para a API de pesquisa Visual do Bing e obter informações sobre a imagem.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: 60b1dc9b8ea9eda258e9776b8967df38c97d964e
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39071708"
---
# <a name="your-first-bing-visual-search-query-in-javascript"></a>Sua primeira consulta de pesquisa Visual do Bing em JavaScript

API de pesquisa Visual do Bing devolve informações sobre uma imagem que fornece. Pode fornecer a imagem utilizando o URL da imagem, um insights token, ou ao carregar uma imagem. Para obter informações sobre estas opções, consulte [o que é a API de pesquisa Visual do Bing?](../overview.md) Este artigo demonstra a carregar uma imagem. Carregar uma imagem pode ser útil em cenários móveis onde tirar uma fotografia de um ponto de referência bem conhecido e obter informações sobre ele. Por exemplo, as informações podem incluir trivia sobre o ponto de referência. 

Se carregar uma imagem do local, o código a seguir mostra os dados do formulário que tem de incluir no corpo da POSTAGEM. Os dados do formulário tem de incluir o cabeçalho Content-Disposition. Seus `name` parâmetro tem de ser definido como "imagem" e o `filename` parâmetro pode ser definido como qualquer cadeia de caracteres. O conteúdo do formulário é o binário da imagem. O tamanho da imagem máximo que pode carregar é de 1 MB. 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

Este artigo inclui uma aplicação de consola simples que envia um pedido de API de pesquisa Visual do Bing e exibe os resultados da pesquisa JSON. Embora esse aplicativo é escrito em JavaScript, a API é um serviço RESTful Web compatível com qualquer linguagem de programação que pode fazer solicitações HTTP e analisar JSON. 

## <a name="prerequisites"></a>Pré-requisitos

Precisa [node. js 6](https://nodejs.org/en/download/) executar esse código.

Neste início rápido, pode utilizar um [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) chave de subscrição ou uma chave de subscrição paga.

## <a name="running-the-application"></a>Executar a aplicação

O código a seguir mostra como enviar a mensagem com FormData no node. js.

Para executar esta aplicação, siga estes passos:

1. Crie uma pasta para o seu projeto (ou utilize o seu IDE ou editor favorito).
2. A partir de uma linha de comandos ou terminal, navegue para a pasta que acabou de criar.
3. Instale os módulos do pedido:  
  ```  
  npm install request  
  ```  
3. Instale os módulos de dados do formulário:  
  ```  
  npm install form-data  
  ```  
4. Crie um ficheiro denominado GetVisualInsights.js e adicione o seguinte código ao mesmo.
5. Substitua o `subscriptionKey` valor com a sua chave de subscrição.
6. Substitua o `imagePath` valor com o caminho da imagem para carregar.
7. Execute o programa.  
  ```
  node GetVisualInsights.js
  ```

```javascript
var request = require('request');
var FormData = require('form-data');
var fs = require('fs');

var baseUri = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch';
var subscriptionKey = '<yoursubscriptionkeygoeshere>';
var imagePath = "<pathtoyourimagegoeshere>";

var form = new FormData();
form.append("image", fs.createReadStream(imagePath));

form.getLength(function(err, length){
  if (err) {
    return requestCallback(err);
  }

  var r = request.post(baseUri, requestCallback);
  r._form = form; 
  r.setHeader('Ocp-Apim-Subscription-Key', subscriptionKey);
});

function requestCallback(err, res, body) {
    console.log(JSON.stringify(JSON.parse(body), null, '  '))
}
```


## <a name="next-steps"></a>Passos Seguintes

[Obtenha informações sobre uma imagem com um token de insights](../use-insights-token.md)  
[Tutorial de carregamento de imagem de pesquisa Visual do Bing](../tutorial-visual-search-image-upload.md)
[tutorial de aplicação de página única de pesquisa Visual do Bing](../tutorial-bing-visual-search-single-page-app.md)  
[Descrição geral de pesquisa Visual do Bing](../overview.md)  
[Experimente-o](https://aka.ms/bingvisualsearchtryforfree)  
[Obter uma chave de acesso de avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Referência da API de pesquisa Visual do Bing](https://aka.ms/bingvisualsearchreferencedoc)
