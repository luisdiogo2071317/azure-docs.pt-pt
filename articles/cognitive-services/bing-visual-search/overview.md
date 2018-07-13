---
title: Descrição geral da API de pesquisa Visual do Bing | Documentos da Microsoft
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Mostra como obter os detalhes ou informações sobre uma imagem, como imagens semelhantes ou fontes de compras.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 04/10/2018
ms.author: scottwhi
ms.openlocfilehash: aa563d89b1834f5be952f13c31a2451d809709b1
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39006534"
---
# <a name="what-is-bing-visual-search-api"></a>O que é a API de Pesquisa Visual do Bing?

API de pesquisa Visual do Bing fornece uma experiência semelhante à apresentada na Bing.com/images os detalhes da imagem. Com a pesquisa Visual, pode carregar uma imagem e obter informações sobre a imagem, como imagens visualmente similares, fontes de compras, páginas da Web que incluem a imagem e muito mais. Em vez de carregar uma imagem, também pode fornecer um token de informações que obtém a partir de uma imagem nos resultados da pesquisa de imagens (veja [a API de imagens do Bing](../bing-image-search/overview.md)).

Pesquisa Visual pode identificar celebridades, monumentos e pontos de referência, obras de arte, furnishings domésticas, moda, produtos, reconhecimento de carateres (OCR) e muito mais.

Seguem-se as informações que pesquisa Visual permite que descubra.

- Imagens visualmente similares&mdash;uma lista de imagens visualmente semelhantes à imagem de entrada
- Produtos visualmente semelhantes&mdash;uma lista de imagens que contenham produtos visualmente semelhantes para o produto mostrado na imagem de entrada
- Origens de compras&mdash;uma lista de locais onde pode adquirir o item mostrado na imagem de entrada
- Relacionados com pesquisas&mdash;uma lista de pesquisas relacionadas feitas por outras pessoas ou que são baseadas no conteúdo da imagem
- Páginas da Web que incluem a imagem&mdash;uma lista de páginas da Web que incluem a imagem de entrada
- Receitas&mdash;uma lista de páginas da Web que incluem receitas para tornar o dish mostrado na imagem de entrada

Além dessas informações, pesquisa Visual também devolve um conjunto variado de termos (etiquetas) derivado a partir da imagem de entrada. Essas marcas permitem aos utilizadores explorar os conceitos da imagem. Por exemplo, se a imagem de entrada for de um famoso famoso, uma das etiquetas pode ser o nome do famoso, outra etiqueta pode ser desporto. Em alternativa, se a imagem de entrada for de uma pizza da apple, as etiquetas podem ser as sobremesas de pizza da Apple, tortas, para que os utilizadores podem explorar os conceitos relacionados.

Os resultados da pesquisa Visual também incluem delimitação de caixas de regiões de interesse na imagem. Por exemplo, se a imagem contém vários celebridades, os resultados podem incluir delimitação de caixas para cada um das celebridades reconhecidas na imagem. Em alternativa, se Bing reconhece um produto ou vestuário na imagem, o resultado pode incluir uma caixa delimitadora para o produto reconhecido ou item de vestuário.

> [!IMPORTANT]
> Se usar o/imagens/detalhes ponto final para [Obtenha informações de imagem](../bing-image-search/image-insights.md), deve atualizar o código para usar pesquisa Visual em vez disso, uma vez que ele fornece informações mais abrangentes.


## <a name="the-request"></a>O pedido

Seguem-se as opções para obter informações aprofundadas sobre uma imagem. 

- Enviar um token de informações que obtém a partir de uma imagem numa chamada anterior para um da [a API de imagens do Bing](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference) pontos finais
- Enviar o URL de uma imagem
- Carregar uma imagem (binário)


Se enviar pesquisa Visual, um token de imagem ou o URL, o seguinte mostra o objeto JSON que devem ser incluídos no corpo da POSTAGEM. 

```json
{
    "imageInfo" : {
        "url" : "",
        "imageInsightsToken" : "",
        "cropArea" : {
            "top" : 0.1,
            "left" : 0.5,
            "right" : 0.9,
            "bottom" : 0.9
        }
    },
    "knowledgeRequest" : {
      "filters" : {
        "site" : ""
      }
    }
}
```

O `imageInfo` objeto tem de incluir qualquer um de `url` ou `imageInsightsToken` campo, mas não ambos. Definir o `url` campo para o URL de uma imagem acessível da Internet. O tamanho de imagem suportados máximo é 1 MB.

O `imageInsightsToken` tem de ser definido como um token de informações. Para obter um token de insights, chame a API de imagens do Bing. A resposta contém uma lista de `Image` objetos. Cada `Image` objeto contém um `imageInsightsToken` campo, que contém o token.

O `cropArea` campo é opcional. A área de recorte Especifica a parte superior, esquerdo canto e na parte inferior, canto direito de uma região de interesse. Especifique os valores no intervalo 0,0 através de 1.0. Os valores são uma percentagem da largura ou altura geral. Por exemplo, o exemplo acima marca o direito metade da imagem como a região de interesse. Incluí-lo se pretender limitar o pedido de informações para a região de interesse.

O `filters` objeto contém um filtro de site (consulte a `site` campo) que pode utilizar para restringir as imagens semelhantes e os resultados de produtos semelhantes a um domínio específico. Por exemplo, se a imagem é de um Surface Book, pode definir `site` para www.microsoft.com. 

Se quiser obter informações sobre uma cópia local de uma imagem, carregue a imagem como dados binários.

Para obter detalhes sobre estas opções, incluindo no corpo da POSTAGEM, consulte [formulário de tipos de conteúdo](#content-form-types).


### <a name="endpoint"></a>Ponto Final

O ponto final de pesquisa Visual é: https:\/\/api.cognitive.microsoft.com/bing/v7.0/images/visualsearch.

Pedidos têm de ser enviados como apenas pedidos de HTTP POST. 


### <a name="query-parameters"></a>Parâmetros de consulta

Seguem-se os parâmetros de consulta que deve especificar o seu pedido. No mínimo, deve incluir o `mkt` parâmetro de consulta.

|Nome|Valor|Tipo|Necessário|  
|----------|-----------|----------|--------------|  
|<a name="cc" />Cc|Um código de país de 2 carateres do país onde vêm os resultados.<br /><br /> Se definir este parâmetro, também tem de especificar o [Accept-Language](#acceptlanguage) cabeçalho. O Bing utiliza o primeiro idioma com suporte que se encontra na lista de idiomas e combina a linguagem com o código de país que especificou para determinar o mercado para devolver resultados da. Se a lista de idiomas não inclui um idioma suportado, o Bing localiza o idioma e o mercado que oferece suporte a solicitação mais próximo. Ou pode utilizar um agregado ou padrão de mercado para os resultados em vez do especificado.<br /><br /> Deve utilizar este parâmetro de consulta e o `Accept-Language` parâmetro de consulta apenas se especificar vários idiomas; caso contrário, deve usar o `mkt` e `setLang` parâmetros de consulta.<br /><br /> Este parâmetro e o [mkt](#mkt) parâmetro de consulta são mutuamente exclusivas&mdash;não especificar ambos.|Cadeia|Não|  
|<a name="mkt" />mkt|O mercado de onde vêm os resultados. <br /><br /> **Nota:** é encorajado a sempre especificar o mercado, se conhecidos. Especificar o mercado de ajuda do Bing encaminhar a solicitação e devolver uma resposta adequada e ideal.<br /><br /> Este parâmetro e o [cc](#cc) parâmetro de consulta são mutuamente exclusivas&mdash;não especificar ambos.|Cadeia|Sim|  
|<a name="safesearch" />pesquisa segura|Um filtro utilizado para filtrar o conteúdo para adultos. Seguem-se os valores de filtro de maiúsculas e minúsculas possíveis.<br /><ul><li>Desativar&mdash;devolver páginas da Web com imagens ou de texto para adultos.<br /><br/></li><li>Moderado&mdash;devolver páginas da Web com o texto para adultos, mas não para adultos imagens.<br /><br/></li><li>Strict&mdash;não retornam páginas da Web com imagens ou de texto para adultos.</li></ul><br /> A predefinição é moderado.<br /><br /> **Nota:** se a solicitação é proveniente de um mercado de política de adultos do Bing que requer que `safeSearch` estar definida para Strict, Bing ignora o `safeSearch` valor e utiliza Strict.<br/><br/>**Nota:** se utilizar o `site:` operador de consulta, é provável que a resposta pode conter o conteúdo para adultos, independentemente do que o `safeSearch` parâmetro de consulta está definido como. Utilize `site:` apenas se está atento o conteúdo do site e o seu cenário suporta a possibilidade de conteúdo para adultos. |Cadeia|Não|  
|<a name="setlang" />setLang|O idioma a utilizar para cadeias de caracteres de interface de usuário. Especifica o idioma usando o código de idioma de 2 letras 639-1 ISO. Por exemplo, o código de idioma para inglês é EN. A predefinição é EN (em inglês).<br /><br /> Embora seja opcional, deve sempre especificar o idioma. Normalmente, definido `setLang` para o mesmo idioma especificado pelo `mkt` , a menos que o usuário deseja as cadeias de caracteres de interface do usuário exibidas num idioma diferente.<br /><br /> Este parâmetro e o [Accept-Language](#acceptlanguage) cabeçalho são mutuamente exclusivas&mdash;não especificar ambos.<br /><br /> Uma cadeia de caracteres de interface do usuário é uma cadeia que é utilizada como etiqueta numa interface de utilizador. Existem algumas cadeias de caracteres de interface de utilizador nos objetos de resposta de JSON. Além disso, todas as ligações às propriedades de Bing.com nos objetos de resposta aplicam-se o idioma especificado.|Cadeia|Não| 

### <a name="headers"></a>Cabeçalhos

Seguem-se os cabeçalhos que deve especificar o seu pedido. Os cabeçalhos de tipo de conteúdo e Ocp-Apim-Subscription-Key são os cabeçalhos necessários únicos, mas também deve incluir o agente do usuário, X-MSEdge-ClientID, X-MSEdge-ClientIP e localização de pesquisa de X.


|Cabeçalho|Descrição|  
|------------|-----------------|  
|<a name="acceptlanguage" />Aceite-Language|Cabeçalho do pedido opcional.<br /><br /> Uma lista delimitada por vírgulas de idiomas a utilizar para cadeias de caracteres de interface de usuário. A lista está por ordem de preferência decrescente. Para obter mais informações, incluindo o formato esperado, consulte [RFC2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).<br /><br /> Este cabeçalho e o [setLang](#setlang) parâmetro de consulta são mutuamente exclusivas&mdash;não especificar ambos.<br /><br /> Se definir este cabeçalho, também tem de especificar o [cc](#cc) parâmetro de consulta. Para determinar o mercado para devolver resultados, o Bing utiliza o primeiro idioma suportado que encontra na lista e combina-o com o `cc` valor do parâmetro. Se a lista não inclui um idioma suportado, o Bing localiza o idioma e o mercado que oferece suporte a solicitação mais próximo ou ele utiliza um agregado ou padrão de mercado para os resultados. Para determinar o mercado que Bing utilizado, consulte o cabeçalho de BingAPIs de colocação no mercado.<br /><br /> Utilize este cabeçalho e o `cc` parâmetro de consulta apenas se especificar vários idiomas. Caso contrário, utilize o [mkt](#mkt) e [setLang](#setlang) parâmetros de consulta.<br /><br /> Uma cadeia de caracteres de interface do usuário é uma cadeia que é utilizada como etiqueta numa interface de utilizador. Existem algumas cadeias de caracteres de interface de utilizador nos objetos de resposta de JSON. Todas as ligações às propriedades de Bing.com nos objetos de resposta aplicam-se o idioma especificado.|  
|<a name="contenttype" />Tipo de conteúdo|Cabeçalho do pedido necessário.<br /><br />Tem de ser definido como multipart/form-data e incluir um parâmetro de limite (por exemplo, multipart/form-data; limite =\<cadeia de caracteres de limite\>). Para obter mais informações, consulte [formulário de tipos de conteúdo](#content-form-types).
|<a name="market" />Mercado de BingAPIs|Cabeçalho de resposta.<br /><br /> O mercado utilizado pelo pedido. O formulário é \<languageCode\>-\<indicativo do país\>. Por exemplo, en-US.|  
|<a name="traceid" />BingAPIs TraceId|Cabeçalho de resposta.<br /><br /> O ID da entrada de log que contém os detalhes do pedido. Quando ocorre um erro, capturar este ID. Se não for capaz de determinar e resolver o problema, inclua essa identificação, juntamente com as outras informações que fornecer a equipa de suporte.|  
|<a name="subscriptionkey" />OCP-Apim-Subscription-Key|Cabeçalho do pedido necessário.<br /><br /> A chave de subscrição que recebeu quando se inscreveu para este serviço no [dos serviços cognitivos](https://www.microsoft.com/cognitive-services/).|  
|<a name="pragma" />Pragma|Cabeçalho do pedido opcional<br /><br /> Por predefinição, o Bing devolve os conteúdos em cache, se disponível. Para impedir que o Bing retornando conteúdo em cache, defina o cabeçalho de Pragma para no cache (por exemplo, Pragma: não-cache).
|<a name="useragent" />Agente de utilizador|Cabeçalho do pedido opcional.<br /><br /> O agente de utilizador com a origem do pedido. Bing utiliza o agente de utilizador para fornecer aos usuários móveis com uma experiência otimizada. Embora seja opcional, é encorajado a sempre especificar este cabeçalho.<br /><br /> O agente de utilizador deve ser a mesma cadeia de qualquer navegador usada envia. Para obter informações sobre agentes de utilizador, consulte [RFC 2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).<br /><br /> Seguem-se exemplos de cadeias de caracteres do agente do usuário.<br /><ul><li>Windows Phone&mdash;Mozilla/5.0 (compatível; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Toque; NOKIA; Lumia 822)<br /><br /></li><li>Android&mdash;Mozilla/5.0 (Linux; U; Android 2.3.5; en-us; SCH-I500 compilação/GINGERBREAD) AppleWebKit/533.1 (KHTML; como Gecko) versão/4.0 Mobile Safari/533.1<br /><br /></li><li>iPhone&mdash;Mozilla/5.0 (iPhone; CPU 6_1 de iPhone OS como Mac OS X) AppleWebKit/536.26 (KHTML; como Gecko) Mobile/10B142 iPhone4; 1 BingWeb/3.03.1428.20120423<br /><br /></li><li>PC&mdash;Mozilla/5.0 (Windows NT 6.3; WOW64; Trident/7.0; Toque; Rv:11.0) como Gecko<br /><br /></li><li>iPad&mdash;Mozilla/5.0 (iPad; 7_0 de SO de CPU, como o Mac OS X) AppleWebKit/537.51.1 (KHTML, como Gecko) versão/7.0 Mobile/11A465 Safari/9537.53</li></ul>|
|<a name="clientid" />X-MSEdge-ID de cliente|Cabeçalho de solicitação e resposta opcionais.<br /><br /> O Bing utiliza este cabeçalho para fornecer aos utilizadores um comportamento consistente em chamadas de API do Bing. Bing, muitas vezes, por vários voos novos recursos e aprimoramentos, e utiliza o ID de cliente como uma chave para atribuir o tráfego de voos diferentes. Se não utilizar o mesmo ID de cliente para um utilizador em vários pedidos, Bing pode atribuir o utilizador a vários voos em conflito. A ser atribuído a vários voos em conflito pode levar a uma experiência de usuário inconsistente. Por exemplo, se a segunda solicitação tiver uma atribuição de voo diferentes que o primeiro, a experiência poderão ser inesperada. Além disso, o Bing pode utilizar o ID de cliente para personalizar os resultados da web para que o cliente IDs de histórico de pesquisas, fornecendo uma experiência mais rica para o utilizador.<br /><br /> O Bing utiliza também este cabeçalho para ajudar a melhorar as classificações de resultado ao analisar a atividade gerada por um ID de cliente. Ajudam os aprimoramentos de relevância de melhor qualidade dos resultados fornecidos pelas APIs do Bing e, em seguida permite maior taxas de cliques para o consumidor de API.<br /><br /> **Importante:** embora seja opcional, deve considerar este cabeçalho obrigado. Manter o ID de cliente em vários pedidos para o mesmo utilizador final e a combinação de dispositivo permite que 1) o consumidor de API receber uma experiência de usuário consistente e 2) mais elevadas taxas de cliques por meio de melhor qualidade dos resultados das APIs do Bing.<br /><br /> Seguem-se as regras de utilização básica que se aplicam a este cabeçalho.<br /><ul><li>Cada utilizador que utiliza a sua aplicação no dispositivo tem de ter um exclusivo, Bing gerados pelo ID de cliente.<br /><br/>Se não incluir este cabeçalho no pedido, o Bing gera um ID e devolve a mesma no cabeçalho de resposta de X-MSEdge-ID de cliente. O único momento em que não deve incluir esse cabeçalho num pedido é a primeira vez que o utilizador utiliza a sua aplicação nesse dispositivo.<br /><br/></li><li>**Atenção:** tem de garantir que este ID de cliente não é vinculação para quaisquer informações de conta de utilizador autenticado.</li><li>Utilize o ID de cliente para cada pedido de API do Bing que torna a sua aplicação para este utilizador no dispositivo.<br /><br/></li><li>Manter o ID de cliente. Para manter o ID de uma aplicação de browser, utilize um cookie HTTP persistente para garantir que o ID é utilizado em todas as sessões. Não utilize um cookie de sessão. Para outras aplicações, como as aplicações móveis, utilize o armazenamento persistente do dispositivo para manter o ID.<br /><br/>Da próxima vez que o utilizador utiliza a sua aplicação nesse dispositivo, obtenha o ID de cliente que manteve.</li></ul><br /> **Nota:** respostas Bing pode ou não pode incluir esse cabeçalho. Se a resposta inclui esse cabeçalho, capture o ID de cliente e usá-la para todos os pedidos subsequentes do Bing para o utilizador nesse dispositivo.<br /><br /> **Nota:** se incluir o ClientID de MSEdge X, não pode incluir os cookies no pedido.|  
|<a name="clientip" />X-MSEdge-ClientIP|Cabeçalho do pedido opcional.<br /><br /> O endereço IPv4 ou IPv6 do dispositivo cliente. O endereço IP é utilizado para detetar a localização do utilizador. O Bing utiliza as informações de localização para determinar o comportamento de pesquisa segura.<br /><br /> **Nota:** embora seja opcional, é encorajado a sempre especificar esse cabeçalho e o cabeçalho X-pesquisa-Location.<br /><br /> Não é ofuscar o endereço (por exemplo, alterando o último octeto para 0). Ofuscar os resultados de endereço no local não estão sendo em qualquer lugar junto à localização do dispositivo real, que poderá resultar no Bing que serve os resultados de errado.|  
|<a name="location" />Localização de pesquisa de X|Cabeçalho do pedido opcional.<br /><br /> Uma lista delimitada por ponto e vírgula de pares de chave/valor que descrevem a localização geográfica do cliente. O Bing utiliza as informações de localização para determinar o comportamento de pesquisa segura e retornar o conteúdo local relevante. Especificar o par chave/valor como \<chave\>:\<valor\>. Seguem-se as chaves que utilizar para especificar a localização do utilizador.<br /><br /><ul><li>LAT&mdash;necessário. A latitude da localização do cliente, em graus. A latitude tem de ser maior ou igual a -90,0 e menor ou igual a + 90,0. Valores negativos indicam as latitudes Austrais e valores positivos indicam as latitudes Norte.<br /><br /></li><li>longa&mdash;necessário. A longitude da localização do cliente, em graus. A longitude tem de ser maior ou igual a -180,0 e menor ou igual a + 180.0. Valores negativos indicam as longitudes ocidentais e leste longitudes de indicar valores positivos.<br /><br /></li><li>Re&mdash;necessário. O radius, em metros, que especifica a precisão horizontal das coordenadas. Passe o valor devolvido pelo serviço de localização do dispositivo. Valores típicos podem estar 22m para GPS/Wi-Fi, os 380m para triangulação de Torre de célula e 18,000m para pesquisa IP inversa.<br /><br /></li><li>TS&mdash;opcional. O carimbo de hora UNIX de UTC de quando o cliente estava no local. (O carimbo de hora UNIX é o número de segundos desde 1 de Janeiro de 1970.)<br /><br /></li><li>HEAD&mdash;opcional. O cliente rumo relativo ou direção de viagem. Especifica a direção de transporte como graus de 0 a 360, contagem para a direita em relação ao norte verdadeiro. Especifique esta chave apenas se o `sp` chave é diferente de zero.<br /><br /></li><li>SP&mdash;opcional. A velocidade horizontal (velocidade), em metros por segundo, o que o dispositivo cliente está em uma viagem.<br /><br /></li><li>ALT&mdash;opcional. A altitude do dispositivo cliente, em metros.<br /><br /></li><li>são&mdash;opcional. O radius, em metros, que especifica a precisão vertical das coordenadas. Especifique esta chave apenas se especificou o `alt` chave.<br /><br /></li></ul> **Nota:** embora muitas das chaves são opcionais, a obter mais informações fornecidas por si, são mais precisas dos resultados de localização.<br /><br /> **Nota:** embora seja opcional, é encorajado a sempre especificar a localização geográfica do utilizador. Fornecendo a localização é especialmente importante se o endereço IP do cliente não reflete com precisão local físico do usuário (por exemplo, se o cliente utiliza VPN). Para obter melhores resultados, deve incluir esse cabeçalho e o cabeçalho X-MSEdge-ClientIP mas, no mínimo, deve incluir esse cabeçalho.|

> [!NOTE] 
> Lembre-se de que os termos de utilização exigir a conformidade com todas as leis aplicáveis, incluindo relativamente à utilização desses cabeçalhos. Por exemplo, em determinadas jurisdições, como a Europa, existem requisitos para obter o consentimento do utilizador antes de colocar a determinados dispositivos de controle em dispositivos de utilizador.


<a name="content-form-types" />

### <a name="content-form-types"></a>Tipos de conteúdo do formulário

Cada pedido tem de incluir o cabeçalho Content-Type. O cabeçalho deve ser definido como: multipart/form-data; limite =\<cadeia de caracteres de limites\>, onde \<cadeia de caracteres de limite\> é uma cadeia de identificador exclusiva e opaca que identifica o limite dos dados do formulário. Por exemplo, os limites = boundary_1234 abcd.


Se enviar pesquisa Visual, um token de imagem ou o URL, o código a seguir mostra os dados do formulário que tem de incluir no corpo da POSTAGEM. Os dados do formulário tem de incluir o cabeçalho Content-Disposition e a respetiva `name` parâmetro tem de ser definido como "knowledgeRequest." Para obter detalhes sobre o `imageInfo` objeto, consulte [o pedido](#the-request).


```
--boundary_1234-abcd
Content-Disposition: form-data; name="knowledgeRequest"

{
    "imageInfo" : {
        "url" : "https://contoso.com/2018/05/fashion/red.jpg"
    }
}

--boundary_1234-abcd--
```

Se carregar uma imagem do local, o código a seguir mostra os dados do formulário que tem de incluir no corpo da POSTAGEM. Os dados do formulário tem de incluir o cabeçalho Content-Disposition. Seus `name` parâmetro tem de ser definido como "imagem" e o `filename` parâmetro pode ser definido como qualquer cadeia de caracteres. É possível definir o cabeçalho Content-Type para qualquer tipo de mime da imagem comumente utilizados. O conteúdo do formulário é o binário da imagem. O tamanho da imagem máximo que pode carregar é de 1 MB. O maior da largura ou altura deve ser 1.500 pixéis ou menos.


```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"
Content-Type: image/jpeg

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

O código a seguir mostra como especificar a região de interesse de uma imagem carregada.

```
--boundary_1234-abcd
Content-Disposition: form-data; name="knowledgeRequest"

{
    "imageInfo" : {
        "cropArea" : {
            "top" : 0.2,
            "left" : 0.3,
            "bottom" : 0.7,
            "right" : 0.6
        }
    }
}

--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="image"
Content-Type: image/jpeg


ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```



### <a name="example-request"></a>Pedido de exemplo

O código a seguir mostra um pedido de informações de imagem completa que passa um token de imagem e a região de interesse. Obtenha o token de informações a partir de uma chamada anterior para /images/search.


```  
POST https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch?mkt=en-us HTTP/1.1  
Content-Type: multipart/form-data; boundary=boundary_1234-abcd
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com 

--boundary_1234-abcd
Content-Disposition: form-data; name="knowledgeRequest"

{
    "imageInfo" : {
        "imageInsightsToken" : "mid_D6426898706EC7..."
        "cropArea" : {
            "top" : 0.1,
            "left" : 0.2,
            "bottom" : 0.7,
            "right" : 0.5
        }
    }
}

--boundary_1234-abcd--
```


## <a name="the-response"></a>A resposta

Se existem informações disponíveis para a imagem, a resposta contém um ou mais `tags` que contêm as informações. O `image` campo contém o token de informações para a imagem de entrada.

```json
{
  "_type" : "ImageKnowledge",
  "tags" : [
    {...},
    {...},
    {...},
    {...},
    {...}
  ],
  "image" : {
    "imageInsightsToken" : "bcid_AF8C9CA409421B..."
  }
}
```

O `tags` campo contém um nome a apresentar e uma lista de ações (informações). Uma das etiquetas contém um `displayName` campo que está definido como uma cadeia vazia. Esta etiqueta contém as informações padrão, como páginas da Web que incluem a imagem, imagens visualmente similares e fontes de compras para itens encontrados na imagem. Como a imagem inteira é de interesse, a marca de informações padrão não inclui de delimitação de caixas para as regiões de interesse.


```json
{
  "_type" : "ImageKnowledge",
  "tags" : [
    {
      "displayName" : "",
      "actions" : [
        {...},
        {...},
        {...},
        {...}
      ]
    },
    {...},
    {...},
    {...},
    {...}
  ],
  "image" : {
    "imageInsightsToken" : "bcid_AF8C9CA409421B..."
  }
}
```

Para obter uma lista das informações padrão, consulte [predefinido de insights](./default-insights-tag.md).



A restante contêm outras informações que podem ser interessantes para o usuário. Por exemplo, se a imagem contém texto, uma das etiquetas pode incluir uma resposta de TextResults, que contém o texto reconhecido. Em alternativa, se Bing reconhece uma entidade (pessoa, local ou coisa) na imagem, uma das etiquetas pode identificar a entidade. Pesquisa Visual também devolve um conjunto variado de termos (etiquetas) derivado a partir da imagem de entrada. Essas marcas permitem aos utilizadores explorar os conceitos da imagem. Por exemplo, se a imagem de entrada for de um famoso famoso, uma das etiquetas pode ser desporto, que contém ligações para imagens do desporto.

Cada etiqueta inclui um nome de apresentação que pode utilizar para categorizar o insight, delimitação que identifica a região de interesse que se aplica a informação, as informações propriamente ditas e uma miniatura da imagem. Por exemplo, se a imagem de uma pessoa rei ou uma de manga comprida com desporto, uma das etiquetas pode incluir uma caixa delimitadora que ressalta a manga comprida e inclui informações VisualSearch e ProductVisualSearch. E outra etiqueta pode incluir uma informação de ImageResults que contém um URL para um pedido de API de /images/search para obter imagens topically relacionadas ou um URL de pesquisa de Bing.com que direciona o utilizador para os resultados de pesquisa de imagem Bing.com.

Todas as etiquetas que não seja a marca de informações padrão incluem delimitação de caixas que identificam as regiões de interesse na imagem. Por exemplo, se a imagem inclui várias pessoas reconhecidas, as etiquetas podem incluir delimitação de caixas para cada uma das pessoas, ou se a imagem contém itens de vestuário reconhecido, as etiquetas podem incluir delimitação de caixas para cada item de vestuário reconhecido. Pode usar as caixas de delimitadora para criar pontos de acesso em toda a imagem que quando clicado, fornecem detalhes sobre o conteúdo nessa região da imagem. Não deve incluir pontos de acesso numa imagem para caixas de delimitadora que identificam a imagem inteira.

### <a name="text-recognition"></a>Reconhecimento de texto

Se a imagem contém o texto que o serviço reconhece, uma das etiquetas conterá um insight TextResults (ação). A informação `displayName` contém o texto reconhecido. 

```json
    {
        "image" : {
            "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?q=%23%23Text..."
        },
        "displayName" : "##TextRecognition",
        "boundingBox" : {
            "queryRectangle" : {
                "topLeft" : {"x" : 0, "y" : 0},
                "topRight" : {"x" : 1, "y" : 0},
                "bottomRight" : {"x" : 1, "y" : 1},
                "bottomLeft" : {"x" : 0, "y" : 1}
            },
            "displayRectangle" : {
                "topLeft" : {"x" : 0, "y" : 0},
                "topRight" : {"x" : 1, "y" : 0},
                "bottomRight" : {"x" : 1, "y" : 1},
                "bottomLeft" : {"x" : 0, "y" : 1}
            }
        },
        "actions" : [{
            "displayName" : "WALK BIKE ACROSS BRIDGE",
            "actionType" : "TextResults"
        }],
        "sources" : ["OCR"]
    }
```

Uma vez que a marca `displayName` campo contém ##TextRecognition, não usá-lo como um cargo de categoria a experiência do usuário. Que vai para qualquer nome que começa com # #. Em vez disso, utilize o nome a apresentar da ação.


Reconhecimento de texto também pode reconhecer as informações de contacto nos cartões de visita, como números de telefone e endereços de e-mail. A caixa delimitadora identifica a localização das informações de contato no cartão. 

```json
    {
      "image" : {
        "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?q=%23%23TextRecognition..."
      },
      "displayName" : "##TextRecognition",
      "boundingBox" : {
        "queryRectangle" : {
          "topLeft" : {"x" : 0.635, "y" : 0},
          "topRight" : {"x" : 0.77, "y" : 0},
          "bottomRight" : {"x" : 0.77, "y" : 0.4873333},
          "bottomLeft" : {"x" : 0.635, "y" : 0.4873333}
        },
        "displayRectangle" : {
          "topLeft" : {"x" : 0.635, "y" : 0},
          "topRight" : {"x" : 0.77, "y" : 0},
          "bottomRight" : {"x" : 0.77, "y" : 0.4873333},
          "bottomLeft" : {"x" : 0.635, "y" : 0.4873333}
        }
      },
      "actions" : [
        {
          "url" : "tel:888%20555%201212",
          "actionType" : "Uri"
        }
      ],
      "sources" : ["OCR"]
    },
    {
      "image" : {
        "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?q=%23%23TextRecognition..."
      },
      "displayName" : "##TextRecognition",
      "boundingBox" : {
        "queryRectangle" : {
          "topLeft" : {"x" : 0.63, "y" : 0},
          "topRight" : {"x" : 0.866, "y" : 0},
          "bottomRight" : {"x" : 0.866, "y" : 0.5553334},
          "bottomLeft" : {"x" : 0.63, "y" : 0.5553334}
        },
        "displayRectangle" : {
          "topLeft" : {"x" : 0.63, "y" : 0},
          "topRight" : {"x" : 0.866, "y" : 0},
          "bottomRight" : {"x" : 0.866, "y" : 0.5553334},
          "bottomLeft" : {"x" : 0.63, "y" : 0.5553334}
        }
      },
      "actions" : [
        {
          "url" : "mailto:someone@outlook.com",
          "actionType" : "Uri"
        }
      ],
      "sources" : ["OCR"]
    },
    {
      "image" : {
        "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?q=%23%23TextRecognition..."
      },
      "displayName" : "##TextRecognition",
      "boundingBox" : {
        "queryRectangle" : {
          "topLeft" : {"x" : 0, "y" : 0},
          "topRight" : {"x" : 1, "y" : 0},
          "bottomRight" : {"x" : 1, "y" : 1},
          "bottomLeft" : {"x" : 0, "y" : 1}
        },
        "displayRectangle" : {
          "topLeft" : {"x" : 0, "y" : 0},
          "topRight" : {"x" : 1, "y" : 0},
          "bottomRight" : {"x" : 1, "y" : 1},
          "bottomLeft" : {"x" : 0, "y" : 1}
        }
      },
      "actions" : [
        {
          "displayName" : "CHARLENE WHITNEY Graphic Designer 888 555 1212 someone@outlook.com www.contoso.com",
          "actionType" : "TextResults"
        }
      ],
      "sources" : ["OCR"]
    }
```

Se a imagem contém uma entidade reconhecida como uma pessoa, local ou coisa, uma das etiquetas pode incluir uma informação de entidade. 

```json
    {
      "image" : {
        "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?q=Statue+of+Liberty..."
      },
      "displayName" : "Statue of Liberty",
      "boundingBox" : {
        "queryRectangle" : {
          "topLeft" : {"x" : 0.40625, "y" : 0.1757813},
          "topRight" : {"x" : 0.6171875, "y" : 0.1757813},
          "bottomRight" : {"x" : 0.6171875, "y" : 0.3867188},
          "bottomLeft" : {"x" : 0.40625, "y" : 0.3867188}
        },
        "displayRectangle" : {
          "topLeft" : {"x" : 0.40625, "y" : 0.1757813},
          "topRight" : {"x" : 0.6171875, "y" : 0.1757813},
          "bottomRight" : {"x" : 0.6171875, "y" : 0.3867188},
          "bottomLeft" : {"x" : 0.40625, "y" : 0.3867188}
        }
      },
      "actions" : [
        {
          "_type" : "ImageEntityAction",
          "webSearchUrl" : "https:\/\/www.bing.com\/search?q=Statue+of+Liberty",
          "displayName" : "Statue of Liberty",
          "actionType" : "Entity",
        }
      ]
    }
```



## <a name="next-steps"></a>Passos Seguintes

Para começar rapidamente com o primeiro pedido, veja os inícios rápidos: [c#](quickstarts/csharp.md) | [Java](quickstarts/java.md) | [node. js](quickstarts/nodejs.md)  |  [Python](quickstarts/python.md).

Experimente a API. Aceda a [consola de teste de API de pesquisa Visual](https://dev.cognitive.microsoft.com/docs/services/878c38e705b84442845e22c7bff8c9ac). 


Familiarize-se com o [referência de API de pesquisa Visual](https://aka.ms/bingvisualsearchreferencedoc). A referência contém a lista de pontos finais, cabeçalhos e parâmetros de consulta que teria de utilizar para solicitar os resultados da pesquisa. Também inclui as definições dos objetos de resposta. 

Não se esqueça de ler o artigo [Requisitos de Utilização e Apresentação do Bing](./use-and-display-requirements.md) para que não infrinja nenhuma das regras relativas à utilização dos resultados da pesquisa.


