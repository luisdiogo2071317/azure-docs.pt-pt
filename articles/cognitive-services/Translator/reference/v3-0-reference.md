---
title: Referência V3.0 de API de texto do tradutor
titlesuffix: Azure Cognitive Services
description: Documentação de referência para o V3.0 de API de texto do Translator.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: reference
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: 6f679536d69f700fd6678eb3bbbb869e42439cde
ms.sourcegitcommit: 7804131dbe9599f7f7afa59cacc2babd19e1e4b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/17/2018
ms.locfileid: "51853358"
---
# <a name="translator-text-api-v30"></a>V3.0 de API de texto do tradutor

## <a name="whats-new"></a>Novidades

A versão 3 da API de texto do Translator fornece uma API da Web baseados em JSON modernos. Melhora a capacidade de utilização e desempenho, a consolidação de recursos existentes em menos operações e fornece novos recursos.

 * Transliteração converter texto num idioma de um script para outro script.
 * Tradução para vários idiomas numa solicitação.
 * Deteção de idioma, tradução e Transliteração numa solicitação.
 * Dicionário para traduções alternativas de pesquisa de um período, para encontrar back traduções e exemplos que mostram os termos utilizados no contexto.
 * Resultados de deteção de idioma mais informativos.

## <a name="base-urls"></a>Base URLs

Texto API v3.0 está disponível na nuvem seguinte:

| Descrição | Região | URL Base                                        |
|-------------|--------|-------------------------------------------------|
| Azure       | Global | API.cognitive.microsofttranslator.com           |


## <a name="authentication"></a>Autenticação

Subscrever a API de texto do Translator nos serviços cognitivos da Microsoft e utilizar a sua chave de subscrição (disponível no portal do Azure) para autenticar. 

A forma mais simples é passar a chave secreta do Azure para o serviço de Microsoft Translator usando o cabeçalho do pedido `Ocp-Apim-Subscription-Key`.

Uma alternativa é usar a chave secreta para obter um token de autorização do serviço de tokens de. Em seguida, passa o token de autorização para o serviço Microsoft Translator, com o `Authorization` cabeçalho do pedido. Para obter um token de autorização, fazer um `POST` pedido para o URL seguinte:

| Ambiente     | URL do serviço de autenticação                                |
|-----------------|-----------------------------------------------------------|
| Azure           | `https://api.cognitive.microsoft.com/sts/v1.0/issueToken` |

Seguem-se os pedidos de exemplo para obter um token dado uma chave secreta:

```
// Pass secret key using header
curl --header 'Ocp-Apim-Subscription-Key: <your-key>' --data "" 'https://api.cognitive.microsoft.com/sts/v1.0/issueToken'
// Pass secret key using query string parameter
curl --data "" 'https://api.cognitive.microsoft.com/sts/v1.0/issueToken?Subscription-Key=<your-key>'
```

Um pedido com êxito devolve o token de acesso codificada como texto simples no corpo da resposta. O token válido é passado para o serviço Microsoft Translator, como um token de portador a autorização.

```
Authorization: Bearer <Base64-access_token>
```

Um token de autenticação é válido durante 10 minutos. O token deve ser reutilizado ao fazer várias chamadas para as APIs de Microsoft Translator. No entanto, se o seu programa faz solicitações para a API do Translator ao longo de um longo período de tempo, em seguida, seu programa tem de pedir um novo token de acesso em intervalos regulares (por exemplo, a cada 8 minutos).

Para resumir, um pedido de cliente para a API do Translator irá incluir um cabeçalho de autorização obtido da tabela a seguir:

<table width="100%">
  <th width="30%">Cabeçalhos</th>
  <th>Descrição</th>
  <tr>
    <td>OCP-Apim-Subscription-Key</td>
    <td>*Utilizar com a subscrição de serviços cognitivos, se estiver passando a chave secreta*.<br/>O valor é a chave secreta do Azure para a sua subscrição para a API de texto do Translator.</td>
  </tr>
  <tr>
    <td>Autorização</td>
    <td>*Utilizar com subscrição dos serviços cognitivos, se estiver passando um token de autenticação.*<br/>O valor é o token de portador: "portador <token>'.</td>
  </tr>
</table> 

## <a name="errors"></a>Erros

Uma resposta de erro padrão é um objeto JSON com o par nome/valor com o nome `error`. O valor também é um objeto JSON com propriedades:

  * `code`: Um código de erro definidas pelo servidor.

  * `message`: Uma cadeia de caracteres fornecendo uma representação legível do erro.

Por exemplo, um cliente com uma subscrição de avaliação gratuita iria receber o erro seguinte assim que a quota gratuita está esgotada:

```
{
  "error": {
    "code":403001,
    "message":"The operation is not allowed because the subscription has exceeded its free quota."
    }
}
```
O código de erro é uma combinação de número de 6 dígitos o código de estado HTTP de 3 dígitos seguido por uma série de 3 dígitos para ainda mais categorizar o erro. Códigos de erro comuns são:

| Código | Descrição |
|:----|:-----|
| 400000| Uma das entradas de pedido não é válida.|
| 400001| O parâmetro de "escopo" é inválido.|
| 400002| O parâmetro de "category" é inválido.|
| 400003| Um especificador de idioma está em falta ou é inválido.|
| 400004| Um especificador de script de destino ("para o script") está em falta ou é inválido.|
| 400005| Um texto de entrada está em falta ou é inválido.|
| 400006| A combinação de idioma e o script não é válida.|
| 400018| Um especificador de script de origem ("de script") está em falta ou é inválido.|
| 400019| Um do idioma especificado não é suportado.|
| 400020| Um dos elementos na matriz de texto de entrada não é válido.|
| 400021| O parâmetro de versão de API está em falta ou é inválido.|
| 400023| Um do par de idioma especificado não é válido.|
| 400035| O idioma de origem ("de" campo) não é válido.|
| 400036| O idioma de destino ("para" campo) está em falta ou é inválido.|
| 400042| Uma das opções ("Opções" campo especificado) não é válida.|
| 400043| O ID de rastreio de cliente (campo ClientTraceId ou cabeçalho X-ClientTranceId) está em falta ou é inválido.|
| 400050| O texto de entrada é demasiado longo.|
| 400064| O parâmetro de "tradução" está em falta ou é inválido.|
| 400070| O número de scripts de destino (parâmetro ToScript) não corresponde ao número de idiomas de destino (para o parâmetro).|
| 400071| O valor não é válido para TextType.|
| 400072| A matriz de texto de entrada tem demasiados elementos.|
| 400073| O parâmetro de script não é válido.|
| 400074| O corpo do pedido não é um JSON válido.|
| 400075| A combinação de par e categoria de idioma não é válida.|
| 400077| Foi excedido o tamanho máximo do pedido.|
| 400079| A pedido para a tradução entre de e para o idioma personalizadas do sistema não existem.|
| 401000| O pedido não está autorizado porque as credenciais estão em falta ou inválido.|
| 401015| "As credenciais fornecidas são para a API de voz. Este pedido requer credenciais para a API de texto. Utilize uma subscrição para a API de texto do Translator."|
| 403000| A operação não é permitida.|
| 403001| A operação não é permitida porque a subscrição excedeu a quota gratuita.|
| 405000| O método de pedido não é suportado para o recurso pedido.|
| 408001| O sistema de tradução personalizadas solicitado ainda não está disponível. Tente novamente dentro de alguns minutos.|
| 415000| O cabeçalho Content-Type está em falta ou é inválido.|
| 429000, 429001, 429002| O servidor rejeitou o pedido porque o cliente está a enviar demasiados pedidos. Reduza a frequência de pedidos para evitar a limitação.|
| 500000| Ocorreu um erro inesperado. Se o erro persistir, relatá-lo com a data/hora do erro, solicitar o identificador do cabeçalho de resposta X-RequestId e o identificador de cliente a partir do cabeçalho de pedido X-ClientTraceId.|
| 503000| Serviço está temporariamente indisponível. Tente novamente. Se o erro persistir, relatá-lo com a data/hora do erro, solicitar o identificador do cabeçalho de resposta X-RequestId e o identificador de cliente a partir do cabeçalho de pedido X-ClientTraceId.|

