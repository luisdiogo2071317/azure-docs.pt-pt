---
title: 'Início Rápido: Criar uma consulta de pesquisa visual, Node.js - Pesquisa Visual do Bing'
titleSuffix: Azure Cognitive Services
description: Como carregar uma imagem para a API de Pesquisa Visual do Bing e obter informações sobre a imagem.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: quickstart
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: 3490f7722ca0c1331ccea26cd18398cff1317aee
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/09/2018
ms.locfileid: "48887418"
---
# <a name="quickstart-your-first-bing-visual-search-query-in-javascript"></a>Início Rápido: a sua primeira consulta de Pesquisa Visual do Bing em JavaScript

A API de Pesquisa Visual do Bing devolve informações sobre a imagem fornecida. Pode fornecer a imagem através do URL da mesma, de um token de informações ou do carregamento da mesma. Para obter informações sobre estas opções, veja [What is Bing Visual Search API?](../overview.md) (O que é a API de Pesquisa Visual do Bing?). Este artigo demonstra como carregar uma imagem. Carregar uma imagem pode ser útil em cenários móveis onde tira uma fotografia de um ponto de referência bem conhecido e obtém informações sobre o mesmo. Por exemplo, as informações podem incluir curiosidades sobre esse ponto de referência. 

Se carregar uma imagem local, o seguinte mostra os dados de formato que tem de incluir no corpo de POST. Os dados de formulário têm de incluir o cabeçalho Content-Disposition. O respetivo parâmetro `name` tem de ser definido como “image” e o parâmetro `filename` pode ser definido como qualquer cadeia. Os conteúdos do formulário são o binário da imagem. O tamanho máximo de imagem que pode carregar é 1 MB. 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

Este artigo inclui uma aplicação de consola simples que envia um pedido da API de Pesquisa Visual do Bing e apresenta os resultados da pesquisa JSON. Embora esta aplicação seja escrita em JavaScript, a API é um serviço Web RESTful compatível com qualquer linguagem de programação que consiga fazer pedidos HTTP e analisar JSON. 

## <a name="prerequisites"></a>Pré-requisitos

Irá precisar do [Node.js 6](https://nodejs.org/en/download/) para executar este código.

Neste guia de início rápido, pode utilizar uma chave de subscrição de uma [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) ou uma chave de subscrição paga.

## <a name="running-the-application"></a>Executar a aplicação

A seguir, é-lhe mostrado como enviar a mensagem com FormData no Node.js.

Para executar esta aplicação, siga estes passos:

1. Crie uma pasta para o seu projeto (ou utilize o seu IDE ou editor favorito).
2. Numa linha de comandos ou terminal, navegue para a pasta que acabou de criar.
3. Instale os módulos do pedido:  
  ```  
  npm install request  
  ```  
3. Instale os módulos de dados de formulário:  
  ```  
  npm install form-data  
  ```  
4. Crie um ficheiro com o nome GetVisualInsights.js e adicione-lhe o seguinte código.
5. Substitua o valor `subscriptionKey` pela sua chave de subscrição.
6. Substitua o valor `imagePath` pelo caminho da imagem a carregar.
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


## <a name="next-steps"></a>Passos seguintes

[Get insights about an image using an insights token](../use-insights-token.md) (Utilizar um token de informações para obter informações de uma imagem)  
[Tutorial de carregamento de imagens da Pesquisa Visual do Bing](../tutorial-visual-search-image-upload.md)
[Tutorial de aplicação de página única da Pesquisa Visual do Bing](../tutorial-bing-visual-search-single-page-app.md)  
[Descrição geral da Pesquisa Visual do Bing](../overview.md)  
[Experimente](https://aka.ms/bingvisualsearchtryforfree)  
[Obter uma chave de acesso de avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Bing Visual Search API reference](https://aka.ms/bingvisualsearchreferencedoc) (Referência da API de Pesquisa Visual do Bing)
