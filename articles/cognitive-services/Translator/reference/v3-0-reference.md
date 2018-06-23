---
title: Referência do Microsoft tradutor texto API v 3.0 | Microsoft Docs
description: Documentação de referência para a API de texto do conversor de Microsoft v 3.0.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: microsoft translator
ms.topic: article
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: cfaa9584e833b137b417d9074fbfcf606eb21388
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352700"
---
#<a name="translator-text-api-v30"></a>V 3.0 de texto API tradutor

## <a name="whats-new"></a>Novidades

A versão 3 da API de texto de tradutor Microsoft fornece uma API de Web baseados em JSON moderna. Melhora a capacidade de utilização e desempenho através da consolidação funcionalidades existentes numa menos operações e fornece novas funcionalidades.

 * Transliteration para converter o texto um idioma de um script outro script.
 * Conversão para vários idiomas num pedido.
 * Deteção de idioma, tradução e transliteration num pedido.
 * Dicionário para traduções alternativo de pesquisa de um termo, para localizar back traduções e exemplos de termos utilizados no contexto.
 * Mais informativos resultados de deteção de idioma.

## <a name="base-urls"></a>Base URLs

V 3.0 de API de texto está disponível na nuvem seguinte:

| Descrição | Região | URL Base                                        |
|-------------|--------|-------------------------------------------------|
| Azure       | Global | API.cognitive.microsofttranslator.com           |


## <a name="authentication"></a>Autenticação

Subscrever tradutor texto API nos serviços cognitivos da Microsoft e utilizar a sua chave de subscrição (disponível no portal do Azure) para se autenticar. 

É a forma mais simples para passar a chave secreta do Azure para o serviço de tradutor utilizando o cabeçalho do pedido `Ocp-Apim-Subscription-Key`.

Uma alternativa consiste em utilizar a sua chave secreta para obter um token de autorização do serviço de token. Em seguida, passa o token de autorização para o serviço de tradutor utilizando o `Authorization` cabeçalho do pedido. Para obter um token de autorização, efetue um `POST` pedido para o seguinte URL:

| Ambiente     | URL do serviço de autenticação                                |
|-----------------|-----------------------------------------------------------|
| Azure           | `https://api.cognitive.microsoft.com/sts/v1.0/issueToken` |

Seguem-se os pedidos de exemplo para obter um token fornecido uma chave secreta:

```
// Pass secret key using header
curl --header 'Ocp-Apim-Subscription-Key: <your-key>' --data "" 'https://api.cognitive.microsoft.com/sts/v1.0/issueToken'
// Pass secret key using query string parameter
curl --data "" 'https://api.cognitive.microsoft.com/sts/v1.0/issueToken?Subscription-Key=<your-key>'
```

Um pedido com êxito devolve o token de acesso codificado como texto simples no corpo da resposta. O token válido é transmitido para o serviço de tradutor como um token de portador a autorização.

```
Authorization: Bearer <Base64-access_token>
```

Um token de autenticação é válido durante 10 minutos. O token deve ser reutilizado quando efetuar várias chamadas para as APIs de conversor. No entanto, se o seu programa torna pedidos para a API de tradutor por um longo período de tempo, em seguida, o programa tem de pedir um novo token de acesso em intervalos regulares (por exemplo, a cada 8 minutos).

Para resumir, um pedido de cliente para a API de tradutor irá incluir um cabeçalho de autorização obtido a partir da tabela seguinte:

<table width="100%">
  <th width="30%">Cabeçalhos</th>
  <th>Descrição</th>
  <tr>
    <td>OCP Apim-subscrição-chave</td>
    <td>*Utilizar com a subscrição de serviços cognitivos se estão a ser transmitidos a chave secreta*.<br/>O valor é a chave secreta do Azure para a sua subscrição a API de texto de conversor.</td>
  </tr>
  <tr>
    <td>Autorização</td>
    <td>*Utilize com a subscrição de serviços cognitivos se está a transmitir um token de autenticação.*<br/>O valor é o token de portador: ' portador <token>'.</td>
  </tr>
</table> 

## <a name="errors"></a>Erros

Uma resposta de erro padrão é um objeto JSON com o par nome/valor com o nome `error`. O valor também é um objeto JSON com propriedades:

  * `code`: Um código de erro definidas pelo servidor.

  * `message`: Uma cadeia de fornecer uma representação legível por humanos do erro.

Por exemplo, um cliente com uma subscrição de avaliação gratuita seria receber o erro seguinte assim que a quota livre se esgota:

```
{
  "error": {
    "code":403000,
    "message":"The subscription has exceeded its free quota."
    }
}
```
