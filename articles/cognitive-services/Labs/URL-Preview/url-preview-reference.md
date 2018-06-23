---
title: Referência do pré-visualização do URL - serviços cognitivos Microsoft do projeto | Microsoft Docs
description: Referência para o ponto final de pré-visualização do URL de projeto.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.technology: project-url-preview
ms.topic: article
ms.date: 03/29/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: adc2f83f703e740e40d9ba4fd3ed08ba429e5d97
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354181"
---
# <a name="project-url-preview-v7-reference"></a>Referência de v7 de pré-visualização do URL do projeto

Pré-visualização do URL suporta breves descrições dos recursos Web para publicações no blogue, debates fórum, páginas de pré-visualização, etc.  O URL pode ser qualquer tipo de recurso de Internet: página Web, de notícias, de imagem ou de vídeo. A consulta tem de ser um URL absoluto com um esquema de http ou https; URLs relativos ou outros esquemas como ftp: / / não são suportadas.

As aplicações que utilizam o URL de pré-visualização enviam pedidos Web para o ponto final com um URL para um parâmetro de consulta de pré-visualização.  O pedido tem de incluir o *Ocp-Apim-Subscription-Key* cabeçalho.   

É possível analisar a resposta JSON para obter as informações de pré-visualização: nome, descrição do recurso, *isFamilyFriendly*e ligações que fornecem acesso a uma imagem de representativa e para o recurso completo online.

Tem de utilizar apenas os dados de pré-visualização de URL para apresentar os fragmentos de pré-visualização e imagens em miniatura com hiperligação para os respetivos sites de origem, no URL iniciada pelo utilizador final de partilha no suporte de dados sociais, chat bot ou ofertas de semelhantes. Não deve copiar, armazenar ou colocar em cache todos os dados recebidos da pré-visualização do URL de projeto. Deve honrar quaisquer pedidos para desativar pré-visualizações poderá a receber do Web site ou os proprietários de conteúdo.

## <a name="endpoint"></a>Ponto Final
Para pedir resultados de pré-visualização de URL, envie um pedido para o seguinte ponto final. Utilize os parâmetros de URL e cabeçalhos para definir mais especificações.

Ponto final GET: 
````
https://api.labs.cognitive.microsoft.com/urlpreview/v7.0/search?q=queryURL

````

O pedido tem de utilizar o protocolo HTTPS e incluem o seguinte parâmetro de consulta:

 q - a consulta que identifica o URL para pré-visualização 

As secções seguintes fornecem detalhes técnicos sobre os objetos de resposta, parâmetros de consulta e cabeçalhos que afetam os resultados da pesquisa. 
  
Para obter informações sobre cabeçalhos que devem incluir pedidos, consulte [cabeçalhos](#headers).  
  
Para obter informações sobre os parâmetros de consulta que devem incluir pedidos, consulte [parâmetros de consulta](#query-parameters).  
  
Para obter informações sobre o JSON objetos que a resposta inclui, consulte [objetos de resposta](#response-objects).

O comprimento do URL de consulta máxima é de 2.048 carateres. Para garantir que o comprimento do URL não pode exceder o limite, o comprimento máximo de parâmetros da sua consulta deve ser inferior a 1.500 carateres. Se o URL exceder 2.048 carateres, o servidor devolve 404 não encontrado.  

Para obter informações sobre a utilização permitida e apresentação de resultados, consulte [utilizar e apresentar requisitos](use-display-requirements.md). 

> [!NOTE]
> Alguns cabeçalhos de pedido são significativos para outras APIs de pesquisa não afetam a pré-visualização de URL
> - Pragma – o chamador não tem controlo sobre a pré-visualização de URL utiliza a cache
> - Agente do utilizador – por agora, API de pré-visualização do Url não fornece respostas diferentes para chamadas emanating do Mobile, PC ou portátil.

> Além disso, alguns parâmetros não são atualmente significativos para API de pré-visualização de URL, mas podem ser utilizados no futuro para globalização melhorada. 
 
## <a name="headers"></a>Cabeçalhos  
Seguem-se os cabeçalhos que podem incluir um pedido e resposta.  
  
|Cabeçalho|Descrição|  
|------------|-----------------|   
|<a name="market" />Mercado BingAPIs|Cabeçalho de resposta.<br /><br /> Mercado utilizado por pedido. O formulário é \<languageCode\>-\<indicativo do país\>. Por exemplo, en-US.|  
|<a name="traceid" />BingAPIs TraceId|Cabeçalho de resposta.<br /><br /> O ID da entrada de registo que contém os detalhes do pedido. Quando ocorre um erro, capturar este ID. Se não conseguir determinar e resolver o problema, inclua este ID, juntamente com outras informações que fornecer a equipa de suporte.|  
|<a name="subscriptionkey" />OCP Apim-subscrição-chave|Cabeçalho de pedido necessários.<br /><br /> A chave de subscrição que recebeu quando se inscreveu para este serviço em [serviços cognitivos](https://www.microsoft.com/cognitive-services/).|  
|<a name="clientid" />X-MSEdge-ClientID|Cabeçalho de pedido e resposta opcional.<br /><br /> Bing utiliza este cabeçalho para fornecer aos utilizadores com o comportamento consistente em toda as chamadas de API do Bing. Bing frequentemente flights funcionalidades novas e melhoradas e utiliza o ID de cliente como uma chave para atribuição de tráfego na flights diferentes. Se não utilizar o mesmo ID de cliente para um utilizador em vários pedidos, em seguida, Bing pode atribuir o utilizador a vários flights em conflito. A ser atribuídos a vários flights em conflito podem levar a uma experiência de utilizador inconsistente. Por exemplo, se o segundo pedido possui uma atribuição de voo diferente que o primeiro, a experiência poderá inesperada. Além disso, o Bing pode utilizar o ID de cliente para personalizar os resultados da web para que o cliente IDs de histórico de pesquisa, proporcionando uma experiência mais rica para o utilizador.<br /><br /> Bing também utiliza este cabeçalho para ajudar a melhorar as classificações de resultado ao analisar a atividade gerada por um ID de cliente. As melhorias de relevância ajudá-lo com melhor qualidade dos resultados entregue ao Bing APIs e por sua vez permite superiores através de clique taxas para o consumidor da API.<br /><br />Seguem-se as regras de utilização básica que se aplicam a este cabeçalho.<br /><ul><li>Cada utilizador que utiliza a sua aplicação no dispositivo tem de ter um único, Bing gerado ID de cliente.<br /><br/>Se não incluir este cabeçalho do pedido, o Bing gera um ID e devolve a mesma no cabeçalho de resposta de X-MSEdge-ClientID. O único momento em que não deve incluir este cabeçalho num pedido for a primeira vez que o utilizador utiliza a sua aplicação nesse dispositivo.<br /><br/></li><li>Utilize o ID de cliente para cada pedido de API do Bing que torna a sua aplicação para este utilizador no dispositivo.<br /><br/></li><li>**Atenção:** tem de garantir que este ID de cliente não é linkable para quaisquer informações de conta de utilizador authenticatable.</li><br/><li>Manter o ID de cliente. Para manter o ID de uma aplicação de browser, utilize um cookie HTTP persistente para garantir que o ID é utilizado em todas as sessões. Não utilize um cookie de sessão. Para outras aplicações, tais como as aplicações móveis, utilize o armazenamento persistente do dispositivo para manter o ID.<br /><br/>Da próxima vez que o utilizador utiliza a sua aplicação nesse dispositivo, obter o ID de cliente que manteve.</li></ul><br /> **Nota:** respostas Bing pode ou não pode incluir este cabeçalho. Se a resposta inclui este cabeçalho, o ID de cliente de captura e utilizá-la para todos os pedidos subsequentes do Bing para o utilizador em que o dispositivo.<br /><br /> **Nota:** se incluir o X-MSEdge-ClientID, não pode incluir os cookies no pedido.|  
|<a name="clientip" />X-MSEdge-ClientIP|Cabeçalho de pedido opcional.<br /><br /> O endereço IPv4 ou IPv6 do dispositivo cliente. O endereço IP é utilizado para detetar a localização do utilizador. Bing utiliza as informações de localização para determinar o comportamento de procura seguro.<br /><br />  Não é obfuscate o endereço (por exemplo, alterando o último octeto para 0). Obfuscating os resultados de endereço na localização que não estão a ser em qualquer lugar, quase localização concreta do dispositivo, que poderá resultar no Bing que serve resultados errados.|  
<br /><br /></li></ul>   

## <a name="query-parameters"></a>Parâmetros de consulta  
O pedido pode incluir os seguintes parâmetros de consulta. Consulte a coluna necessária para os parâmetros necessários. Tem de URL codificar os parâmetros de consulta. A consulta tem de ser um URL absoluto com um esquema de http ou https; Não suportamos URLs relativos ou outros esquemas como ftp: / /
  
  
|Nome|Valor|Tipo|Necessário|  
|----------|-----------|----------|--------------|  
|<a name="mkt" />mkt|Mercado onde os resultados provenientes. <br /><br />Para obter uma lista de valores de mercado possíveis, consulte [códigos de mercado](#market-codes).<br /><br /> **Nota:** a API de pré-visualização do URL atualmente só suporta geografia E.U.A. e o idioma inglês.<br /><br />|Cadeia|Sim|  
|<a name="query" />Q|O URL para pré-visualização|Cadeia|Sim|  
|<a name="responseformat" />responseFormat|Escreva o suporte de dados a utilizar para a resposta. Seguem-se os valores possíveis sensível.<br /><ul><li>JSON</li><li>JSONLD</li></ul><br /> A predefinição é JSON. Para obter informações sobre o JSON objetos que contém a resposta, consulte [resposta objetos](#response-objects).<br /><br />  Se especificar JsonLd, o corpo da resposta inclui objetos JSON LD que contêm os resultados da pesquisa. Para obter informações sobre o LD JSON, consulte [JSON LD](http://json-ld.org/).|Cadeia|Não|
|<a name="safesearch"/>safeSearch|Conteúdo para adultos ilegal ou pirated conteúdo, está bloqueado com o código de erro 400 e o *isFamilyFriendly* sinalizador não é devolvido. <p>É o comportamento para o conteúdo para adultos legal, abaixo. Código de estado devolve 200 e o *isFamilyFriendly* sinalizador está definido como false.<ul><li>safeSearch = strict: título, a descrição, URL e imagem não serão devolvidos.</li><li>safeSearch = moderada; Obter o título, o URL e descrição mas não a imagem descritiva.</li><li>safeSearch = desativado; Obter todos os resposta objetos/elementos – título, o URL, descrição e imagem.</li></ul> |Cadeia|Não é necessária. </br> Por predefinição safeSearch = rigorosa.| 

## <a name="response-objects"></a>Objetos de resposta  
O esquema de resposta é a uma [página Web] ou ErrorResponse, como a API de pesquisa de Web. Se falhar o pedido, o objeto de nível superior é o [ErrorResponse](#errorresponse) objeto.


|Object|Descrição|  
|------------|-----------------|  
|[Página Web](#webpage)|Superior ao nível objeto JSON que contém os atributos de pré-visualização.|  

  
### <a name="error"></a>Erro  
Define o erro ocorreu.  
  
|Elemento|Descrição|Tipo|  
|-------------|-----------------|----------|  
|<a name="error-code" />Código|O código de erro que identifica a categoria de erro. Para obter uma lista de códigos possíveis, consulte [códigos de erro](#error-codes).|Cadeia|  
|<a name="error-message" />Mensagem|Uma descrição do erro.|Cadeia|  
|<a name="error-moredetails" />moreDetails|Uma descrição que fornece informações adicionais sobre o erro.|Cadeia|  
|<a name="error-parameter" />Parâmetro|O parâmetro de consulta no pedido que causou o erro.|Cadeia|  
|<a name="error-subcode" />SubCode|O código de erro que identifica o erro. Por exemplo, se `code` é InvalidRequest, `subCode` poderá estar ParameterInvalid ou ParameterInvalidValue. |Cadeia|  
|<a name="error-value" />valor|Valor do parâmetro de consulta que não era válido.|Cadeia|  
  

### <a name="errorresponse"></a>ErrorResponse  
O objecto de nível superior que a resposta inclui quando ocorre uma falha do pedido.  
  
|Nome|Valor|Tipo|  
|----------|-----------|----------|  
|Escreva|Sugestão de tipo.|Cadeia|  
|<a name="errors" />erros|Uma lista de erros que descrevem os motivos por que motivo o pedido falhou.|[Erro](#error)]|   
  

### <a name="webpage"></a>Página Web  
Define as informações sobre uma página Web em pré-visualização.  
  
|Nome|Valor|Tipo|  
|----------|-----------|----------|
|nome|O título da página, não necessariamente o título HTML|Cadeia|
|url|O URL que foi realmente pesquisado (pedido poderá seguiu redirecionamentos)|Cadeia|  
|descrição|Breve descrição da página e do conteúdo|Cadeia|  
|isFamilyFriendly|Mais exatos para itens no índice web; em tempo real fetches efetue esta deteção baseada unicamente no URL e não o conteúdo da página|boolean|
|primaryImageOfPage/contentUrl|O URL para uma imagem representativo para incluir na pré-visualização|Cadeia| 


### <a name="identifiable"></a>Identificação
|Nome|Valor|Tipo|  
|-------------|-----------------|----------|
|ID|Um identificador de recurso|Cadeia|
 

## <a name="error-codes"></a>Códigos de erro

Seguem-se os códigos de estado HTTP possíveis que devolve um pedido.  
  
|Código de Estado|Descrição|  
|-----------------|-----------------|  
|200|Êxito.|  
|400|Um dos parâmetros de consulta está em falta ou não é válido.| 
|400|ServerError, subCode ResourceError: O URL pedido não está acessível|
|400|ServerError, subCode ResourceError: O URL pedido não devolveu um código de êxito (incluindo se devolveu HTTP 404)|
|400|InvalidRequest, subCode bloqueado: O URL pode conter conteúdo para adultos e foi bloqueado| 
|401|A chave de subscrição está em falta ou não é válida.|  
|403|O utilizador é autenticado (por exemplo, estes utilizaram uma chave de subscrição válido), mas não têm permissão para o recurso pedido.<br /><br /> Bing também pode devolver neste estado se o autor da chamada excedeu as respetivas consultas por quota do mês.|  
|410|O pedido utilizado HTTP em vez do protocolo HTTPS. HTTPS é o único protocolo suportado.|  
|429|O autor da chamada excedeu as respetivas consultas por segundo quota.|  
|500|Erro de servidor inesperado.|

Se falhar o pedido, a resposta contém um [ErrorResponse](#errorresponse) objeto, que contém uma lista de [erro](#error) objetos descrevem a causa do erro. Se o erro está relacionado com um parâmetro, a `parameter` campo identifica o parâmetro que é o problema. E se o erro está relacionado com um valor de parâmetro, a `value` campo identifica o valor que não é válido.

```json
{
  "_type": "ErrorResponse", 
  "errors": [
    {
      "code": "InvalidRequest", 
      "subCode": "ParameterMissing", 
      "message": "Required parameter is missing.", 
      "parameter": "q" 
    }
  ]
}

{
  "_type": "ErrorResponse", 
  "errors": [
    {
      "code": "InvalidAuthorization", 
      "subCode": "AuthorizationMissing", 
      "message": "Authorization is required.", 
      "moreDetails": "Subscription key is not recognized."
    }
  ]
}
```

Seguem-se os valores de código de erro secundárias e de código de erro possíveis.

|Código|SubCode|Descrição
|-|-|-
|ServerError|UnexpectedError<br/>ResourceError<br/>NãoImplementado|Código de estado HTTP é 500.
|InvalidRequest|ParameterMissing<br/>ParameterInvalidValue<br/>HttpNotAllowed<br/>Bloqueado|Bing devolve InvalidRequest sempre que qualquer parte do pedido não é válido. Por exemplo, um parâmetro necessário está em falta ou um valor de parâmetro não é válido.<br/><br/>Se o erro é ParameterMissing ou ParameterInvalidValue, o código de estado HTTP é 400.<br/><br/>Se utilizar o protocolo HTTP em vez de HTTPS, Bing devolve HttpNotAllowed, não sendo o código de estado HTTP 410.
|RateLimitExceeded|Não existem códigos secundárias|Bing devolve RateLimitExceeded sempre que excedem o consultas por segundo (QPS) ou consultas por quota do mês (QPM).<br/><br/>Se exceder QPS, Bing devolve o código de estado HTTP 429 e, se for excedido QPM, Bing devolve 403.
|InvalidAuthorization|AuthorizationMissing<br/>AuthorizationRedundancy|Bing devolve InvalidAuthorization quando Bing não é possível autenticar o autor da chamada. Por exemplo, o `Ocp-Apim-Subscription-Key` cabeçalho está em falta ou a chave de subscrição não é válida.<br/><br/>Redundância ocorre se especificar mais do que um método de autenticação.<br/><br/>Se o erro é InvalidAuthorization, o código de estado HTTP é 401.
|InsufficientAuthorization|AuthorizationDisabled<br/>AuthorizationExpired|Bing devolve InsufficientAuthorization quando o autor da chamada não tem permissões para aceder ao recurso. Isto pode ocorrer se a chave de subscrição foi desativada ou não expirou. <br/><br/>Se o erro é InsufficientAuthorization, o código de estado HTTP é 403.

## <a name="next-steps"></a>Passos Seguintes
- [Início rápido c#](csharp.md)
- [Guia de introdução do Java](java-quickstart.md)
- [Início rápido de JavaScript](javascript.md)
- [Guia de introdução do nó](node-quickstart.md)
- [Guia de introdução do Python](python-quickstart.md)

