---
title: Descrição geral da API de pesquisa Visual do Bing | Microsoft Docs
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Mostra como obter os detalhes ou informações sobre uma imagem, como imagens semelhantes ou origens de compras.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 04/10/2018
ms.author: scottwhi
ms.openlocfilehash: 95f10d8ea7ebe1d40d45231a8ea40df81543fe8b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354758"
---
# <a name="what-is-bing-visual-search-api"></a>O que é a API de pesquisa do Bing Visual?

API de pesquisa do Bing Visual fornece uma experiência semelhante para os detalhes da imagem apresentados na Bing.com/images. Com a pesquisa Visual pode carregar uma imagem e voltar a informações sobre a imagem, como imagens visualmente semelhantes, compras origens, páginas Web que incluem a imagem e muito mais. Em vez de carregamento de uma imagem, também pode fornecer um token de informações que obtém a partir de uma imagem nos resultados da pesquisa de imagens (consulte [API do Bing imagens](../bing-image-search/overview.md)).

Pesquisa Visual pode identificar celebridades para, monuments e landmarks, elementos artísticos, furnishings inicial, forma, produtos, reconhecimento de caráter (OCR) e mais informações.

Seguem-se as informações que procura Visual permite-lhe detetar.

- Imagens visualmente semelhantes&mdash;uma lista de imagens que são visualmente semelhantes à imagem de entrada
- Produtos semelhantes visualmente&mdash;uma lista de imagens que contêm os produtos que estejam visualmente semelhantes mostrado na imagem de entrada do produto
- Compras origens&mdash;uma lista dos locais onde pode comprar o item mostrado na imagem de entrada
- Relacionadas com pesquisas&mdash;uma lista de relacionados pesquisas efetuadas por outras pessoas ou que se baseiam o conteúdo da imagem
- Páginas Web que inclui a imagem&mdash;uma lista de páginas Web que incluem a imagem de entrada
- Receitas&mdash;uma lista de páginas Web que incluem receitas para efetuar o dish mostrado na imagem de entrada

Para além destes insights, pesquisa Visual também devolve um conjunto diverso de termos (etiquetas) derivado da imagem de entrada. Estas etiquetas permitem aos utilizadores explorar conceitos encontrados na imagem. Por exemplo, se a imagem de entrada de um athlete famosa, uma das etiquetas pode ser o nome do athlete, outra tag foi desporto. Em alternativa, se a imagem de entrada de um gráfico circular apple, as etiquetas pode ser circular Apple, Pies, Desserts, pelo que os utilizadores podem explorar conceitos relacionados.

Os resultados da pesquisa Visual também incluem delimitadora as caixas de regiões de interesse na imagem. Por exemplo, se a imagem contém várias celebridades para, os resultados podem incluir delimitadora caixas para cada um das celebridades para reconhecido na imagem. Em alternativa, se Bing reconhece um produto ou clothing na imagem, o resultado pode incluir uma caixa delimitadora para o produto reconhecido ou item de clothing.

> [!IMPORTANT]
> Se utilizar o imagens/detalhes ponto final para [obter conhecimentos aprofundados de imagem](../bing-image-search/image-insights.md), deve atualizar o seu código para utilizar a procura Visual em vez disso, uma vez que fornece informações mais abrangentes.


## <a name="the-request"></a>O pedido

Seguem-se as opções para obter informações sobre uma imagem. 

- Enviar um token de informações que obtém a partir de uma imagem numa chamada anterior para uma do [API do Bing imagens](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference) pontos finais
- Enviar o URL de uma imagem
- Carregar uma imagem (binário)


Se enviar Visual pesquisa um token de imagem ou o URL, o seguinte mostra o objeto JSON que tem de incluir no corpo do POST. 

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

O `imageInfo` tem de incluir o objeto a `url` e `imageInsightsToken` campo mas não ambos. Definir o `url` campo para o URL de uma imagem de acessível à Internet. O tamanho de imagem suportados máximo é 1 MB.

O `imageInsightsToken` tem de ser definido para um token de informações. Para obter um token de insights, chame a API de imagem do Bing. A resposta contém uma lista de `Image` objetos. Cada `Image` objeto contém uma `imageInsightsToken` campo, o que contém o token.

O `cropArea` campo é opcional. A área de cortar Especifica a parte superior esquerdo canto e na parte inferior, canto direito de uma região de interesse. Especifique os valores no intervalo entre 0,0 e através de 1.0. Os valores são uma percentagem da largura ou altura geral. Por exemplo, o exemplo acima marca o direito metade da imagem como a região de interesse. Incluem-se de que pretende limitar o pedido de informações para a região de interesse.

O `filters` objeto contém um filtro de site (consulte o `site` campo) que pode utilizar para restringir as imagens semelhantes e resultados de produtos semelhantes a um domínio específico. Por exemplo, se a imagem de um Surface Book, pode definir `site` para www.microsoft.com. 

Se pretender obter informações sobre uma cópia local de uma imagem, carregue a imagem como dados binários.

Para obter detalhes sobre estas opções, incluindo a no corpo do POST, consulte [tipos de formulário de conteúdo](#content-form-types).


### <a name="endpoint"></a>Ponto Final

O ponto final da pesquisa de Visual: https:\/\/api.cognitive.microsoft.com/bing/v7.0/images/visualsearch.

É necessário enviar pedidos como apenas pedidos de HTTP POST. 


### <a name="query-parameters"></a>Parâmetros de consulta

Seguem-se os parâmetros de consulta, que deve especificar o seu pedido. No mínimo deve incluir o `mkt` parâmetro de consulta.

|Nome|Valor|Tipo|Necessário|  
|----------|-----------|----------|--------------|  
|<a name="cc" />Cc|Um código de país de 2-caráter do país/região onde os resultados provenientes.<br /><br /> Se definir este parâmetro, também tem de especificar o [aceitar idioma](#acceptlanguage) cabeçalho. Bing utiliza o idioma suportado primeiro localiza da lista de idiomas e combina o idioma com o código de país que especificou para determinar o mercado para devolver resultados a partir do. Se a lista de idiomas não inclui um idioma suportado, o Bing localiza o idioma e o mercado que suporta o pedido mais próximo. Ou pode utilizar um agregado ou predefinida mercado para os resultados em vez dos especificado.<br /><br /> Deve utilizar este parâmetro de consulta e o `Accept-Language` parâmetro de consulta apenas se especificar vários idiomas; caso contrário, deve utilizar o `mkt` e `setLang` parâmetros de consulta.<br /><br /> Este parâmetro e o [mkt](#mkt) parâmetro da consulta são mutuamente exclusivos&mdash;não especificar ambos.|Cadeia|Não|  
|<a name="mkt" />mkt|Mercado onde os resultados provenientes. <br /><br /> **Nota:** é vivamente recomendada sempre especificar o mercado, se conhecido. Especificar o mercado ajuda-o Bing, o pedido de rotas e devolver uma resposta adequada e ideal.<br /><br /> Este parâmetro e o [cc](#cc) parâmetro da consulta são mutuamente exclusivos&mdash;não especificar ambos.|Cadeia|Sim|  
|<a name="safesearch" />safeSearch|Um filtro utilizado para filtrar o conteúdo para adultos. Seguem-se os valores de filtro sensível possíveis.<br /><ul><li>Desativar&mdash;páginas Web com o texto para adultos ou imagens de retorno.<br /><br/></li><li>Moderada&mdash;devolver páginas Web com o texto para adultos, mas não para adultos imagens.<br /><br/></li><li>Strict&mdash;não devolveu a páginas Web com o texto para adultos ou nas imagens.</li></ul><br /> A predefinição é moderada.<br /><br /> **Nota:** se o pedido for proveniente de um mercado política para adultos esse Bing requer que `safeSearch` ser definido como Strict, Bing ignora a `safeSearch` valor e utiliza Strict.<br/><br/>**Nota:** se utilizar o `site:` operador de consulta, há a possibilidade de que a resposta pode conter conteúdo para adultos independentemente do que o `safeSearch` parâmetro de consulta está definido como. Utilize `site:` apenas se estiver em consideração o conteúdo do site e a possibilidade de conteúdo para adultos suporta o seu cenário. |Cadeia|Não|  
|<a name="setlang" />setLang|O idioma a utilizar para cadeias de interface de utilizador. Especifique o idioma utilizando o código de idioma de 2 letra 639-1 ISO. Por exemplo, o código de idioma do inglês é EN. A predefinição é EN (inglês).<br /><br /> Embora seja opcional, deve sempre especificar o idioma. Normalmente, definidas `setLang` para a mesma linguagem especificada pelo `mkt` , a menos que o utilizador pretende que as cadeias de interface de utilizador apresentadas num idioma diferente.<br /><br /> Este parâmetro e o [aceitar idioma](#acceptlanguage) cabeçalho são mutuamente exclusivos&mdash;não especificar ambos.<br /><br /> Uma cadeia de interface de utilizador é uma cadeia que é utilizada como uma etiqueta numa interface de utilizador. Existem algumas cadeias de interface de utilizador nos objetos de resposta de JSON. Além disso, todas as ligações para as propriedades de Bing.com nos objetos de resposta aplicam-se o idioma especificado.|Cadeia|Não| 

### <a name="headers"></a>Cabeçalhos

Seguem-se os cabeçalhos que deve especificar o seu pedido. Os cabeçalhos de tipo de conteúdo e Ocp-Apim-Subscription-Key são os cabeçalhos necessários apenas, mas também devem incluir o agente de utilizador, X-MSEdge-ClientID, X-MSEdge-ClientIP e localização de pesquisa de X.


|Cabeçalho|Descrição|  
|------------|-----------------|  
|<a name="acceptlanguage" />Idioma aceitar|Cabeçalho de pedido opcional.<br /><br /> Uma lista delimitada por vírgulas de idiomas para utilizar cadeias de interface de utilizador. A lista está a ser diminuir a ordem de preferência. Para obter mais informações, incluindo o formato esperado, consulte [RFC2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).<br /><br /> Este cabeçalho e o [setLang](#setlang) parâmetro da consulta são mutuamente exclusivos&mdash;não especificar ambos.<br /><br /> Se definir este cabeçalho, também tem de especificar o [cc](#cc) parâmetro de consulta. Para determinar o mercado para devolver resultados, Bing utiliza o idioma suportado primeiro localiza a partir da lista e combina-o com o `cc` valor do parâmetro. Se a lista inclui um idioma suportado, o Bing localiza o idioma e o mercado que suporta o pedido mais próximo ou utiliza um agregado ou default mercado para os resultados. Para determinar o mercado que o Bing utilizado, consulte o cabeçalho de BingAPIs mercado.<br /><br /> Utilize este cabeçalho e o `cc` parâmetro de consulta apenas se especificar vários idiomas. Caso contrário, utilize o [mkt](#mkt) e [setLang](#setlang) parâmetros de consulta.<br /><br /> Uma cadeia de interface de utilizador é uma cadeia que é utilizada como uma etiqueta numa interface de utilizador. Existem algumas cadeias de interface de utilizador nos objetos de resposta de JSON. Todas as ligações para as propriedades de Bing.com nos objetos de resposta aplicam-se o idioma especificado.|  
|<a name="contenttype" />Tipo de conteúdo|Cabeçalho de pedido necessários.<br /><br />Tem de ser definido como multipart /-dados do formulário e incluir um parâmetro de limite (por exemplo, multipart /-dados do formulário; boundary =\<cadeia limites\>). Para obter mais detalhes, consulte [tipos de formulário de conteúdo](#content-form-types).
|<a name="market" />Mercado BingAPIs|Cabeçalho de resposta.<br /><br /> Mercado utilizado por pedido. O formulário é \<languageCode\>-\<indicativo do país\>. Por exemplo, en-US.|  
|<a name="traceid" />BingAPIs TraceId|Cabeçalho de resposta.<br /><br /> O ID da entrada de registo que contém os detalhes do pedido. Quando ocorre um erro, capturar este ID. Se não conseguir determinar e resolver o problema, inclua este ID, juntamente com outras informações que fornecer a equipa de suporte.|  
|<a name="subscriptionkey" />OCP Apim-subscrição-chave|Cabeçalho de pedido necessários.<br /><br /> A chave de subscrição que recebeu quando se inscreveu para este serviço em [serviços cognitivos](https://www.microsoft.com/cognitive-services/).|  
|<a name="pragma" />Pragma|Cabeçalho de pedido opcional<br /><br /> Por predefinição, o Bing devolve os conteúdos em cache, se disponível. Para impedir que o Bing devolver o conteúdo em cache, defina o cabeçalho de Pragma para no-cache (por exemplo, Pragma: não cache).
|<a name="useragent" />Agente de utilizador|Cabeçalho de pedido opcional.<br /><br /> O agente de utilizador com a origem do pedido. Bing utiliza o agente de utilizador para fornecer os utilizadores móveis com uma experiência otimizada. Embora seja opcional, são encorajados a sempre especifique este cabeçalho.<br /><br /> O agente de utilizador deve ser a mesma cadeia de qualquer browser frequentemente utilizada envia. Para obter informações sobre agentes de utilizador, consulte [RFC 2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).<br /><br /> Seguem-se exemplos de cadeias de agente do utilizador.<br /><ul><li>Windows Phone&mdash;Mozilla/5.0 (compatível; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)<br /><br /></li><li>Android&mdash;Mozilla/5.0 (Linux; U; Android 2.3.5; en-us; SCH-I500 compilação/GINGERBREAD) AppleWebKit/533.1 (KHTML; como Gecko) versão/4.0 Mobile Safari/533.1<br /><br /></li><li>iPhone&mdash;Mozilla/5.0 (iPhone; CPU iPhone SO 6_1 como Mac OS X) AppleWebKit/536.26 (KHTML; como Gecko) Mobile/10B142 iPhone4; 1 BingWeb/3.03.1428.20120423<br /><br /></li><li>PC&mdash;Mozilla/5.0 (do Windows NT 6.3; WOW64; Trident/7.0; Touch; Rv:11.0) como Gecko<br /><br /></li><li>iPad&mdash;Mozilla/5.0 (iPad; SO de CPU 7_0 como Mac OS X) AppleWebKit/537.51.1 (KHTML, como Gecko) versão/7.0 Mobile/11A465 Safari/9537.53</li></ul>|
|<a name="clientid" />X-MSEdge-ClientID|Cabeçalho de pedido e resposta opcional.<br /><br /> Bing utiliza este cabeçalho para fornecer aos utilizadores com o comportamento consistente em toda as chamadas de API do Bing. Bing frequentemente flights funcionalidades novas e melhoradas e utiliza o ID de cliente como uma chave para atribuição de tráfego na flights diferentes. Se não utilizar o mesmo ID de cliente para um utilizador em vários pedidos, em seguida, Bing pode atribuir o utilizador a vários flights em conflito. A ser atribuídos a vários flights em conflito podem levar a uma experiência de utilizador inconsistente. Por exemplo, se o segundo pedido possui uma atribuição de voo diferente que o primeiro, a experiência poderá inesperada. Além disso, o Bing pode utilizar o ID de cliente para personalizar os resultados da web para que o cliente IDs de histórico de pesquisa, proporcionando uma experiência mais rica para o utilizador.<br /><br /> Bing também utiliza este cabeçalho para ajudar a melhorar as classificações de resultado ao analisar a atividade gerada por um ID de cliente. As melhorias de relevância ajudá-lo com melhor qualidade dos resultados entregue ao Bing APIs e por sua vez permite superiores através de clique taxas para o consumidor da API.<br /><br /> **Importante:** embora opcional, deve considerar este cabeçalho obrigado. A persistência do ID de cliente em vários pedidos para o mesmo utilizador final e a combinação de dispositivos permite 1) o consumidor da API receber uma experiência de utilizador consistente e taxas 2) superiores clique-through através de melhor qualidade dos resultados das APIs do Bing.<br /><br /> Seguem-se as regras de utilização básica que se aplicam a este cabeçalho.<br /><ul><li>Cada utilizador que utiliza a sua aplicação no dispositivo tem de ter um único, Bing gerado ID de cliente.<br /><br/>Se não incluir este cabeçalho do pedido, o Bing gera um ID e devolve a mesma no cabeçalho de resposta de X-MSEdge-ClientID. O único momento em que não deve incluir este cabeçalho num pedido for a primeira vez que o utilizador utiliza a sua aplicação nesse dispositivo.<br /><br/></li><li>**Atenção:** tem de garantir que este ID de cliente não é linkable para quaisquer informações de conta de utilizador autenticado.</li><li>Utilize o ID de cliente para cada pedido de API do Bing que torna a sua aplicação para este utilizador no dispositivo.<br /><br/></li><li>Manter o ID de cliente. Para manter o ID de uma aplicação de browser, utilize um cookie HTTP persistente para garantir que o ID é utilizado em todas as sessões. Não utilize um cookie de sessão. Para outras aplicações, tais como as aplicações móveis, utilize o armazenamento persistente do dispositivo para manter o ID.<br /><br/>Da próxima vez que o utilizador utiliza a sua aplicação nesse dispositivo, obter o ID de cliente que manteve.</li></ul><br /> **Nota:** respostas Bing pode ou não pode incluir este cabeçalho. Se a resposta inclui este cabeçalho, o ID de cliente de captura e utilizá-la para todos os pedidos subsequentes do Bing para o utilizador em que o dispositivo.<br /><br /> **Nota:** se incluir o X-MSEdge-ClientID, não pode incluir os cookies no pedido.|  
|<a name="clientip" />X-MSEdge-ClientIP|Cabeçalho de pedido opcional.<br /><br /> O endereço IPv4 ou IPv6 do dispositivo cliente. O endereço IP é utilizado para detetar a localização do utilizador. Bing utiliza as informações de localização para determinar o comportamento de procura seguro.<br /><br /> **Nota:** embora opcional, é encorajados a sempre especifique este cabeçalho e o cabeçalho X-pesquisa de localização.<br /><br /> Não é obfuscate o endereço (por exemplo, alterando o último octeto para 0). Obfuscating os resultados de endereço na localização que não estão a ser em qualquer lugar, quase localização concreta do dispositivo, que poderá resultar no Bing que serve resultados errados.|  
|<a name="location" />Localização de pesquisa X|Cabeçalho de pedido opcional.<br /><br /> Uma lista delimitada por ponto e vírgula dos pares chave/valor que descrevem a localização geográfica do cliente. Bing utiliza as informações de localização para determinar o comportamento de procura seguro e para devolver o conteúdo local relevante. Especifique o par chave/valor como \<chave\>:\<valor\>. Seguem-se as chaves que utiliza para especificar a localização do utilizador.<br /><br /><ul><li>LAT&mdash;necessário. A latitude da localização do cliente, em graus. A latitude tem de ser maior que ou igual a-90.0 e inferior ou igual a +90.0. Os valores negativos indicam latitudes Austrais e valores positivos indicam latitudes Norte.<br /><br /></li><li>muito&mdash;necessário. A longitude da localização do cliente, em graus. A longitude tem de ser maior que ou igual a-180.0 e inferior ou igual a +180.0. Os valores negativos indicam longitudes tsunami e valores positivos indicam longitudes Leste.<br /><br /></li><li>Re&mdash;necessário. O radius, no medidores, que especifica a precisão horizontal das coordenadas. Passe o valor devolvido pelo serviço de localização do dispositivo. Valores típicos podem estar 22m para GPS/Wi-Fi, 380m para triangulation Torre de célula e 18,000m para pesquisa inversa de IP.<br /><br /></li><li>TS&mdash;opcional. O carimbo de UTC UNIX da quando o cliente na localização. (O timestamp UNIX é o número de segundos desde 1 de Janeiro de 1970.)<br /><br /></li><li>HEAD&mdash;opcional. Cabeçalho relativo ou direção do levar do cliente. Especifique a direção da levar como graus de 0 a 360, contando no sentido horário em relação ao norte verdadeiro. Especifique esta chave apenas se o `sp` chave é diferente de zero.<br /><br /></li><li>SP&mdash;opcional. A velocidade horizontal (velocidade), em medidores por segundo, que viaja de dispositivo cliente.<br /><br /></li><li>ALT&mdash;opcional. Altitude do dispositivo cliente, no medidores.<br /><br /></li><li>são&mdash;opcional. O radius, no medidores, que especifica a precisão vertical das coordenadas. Especifique esta chave apenas se especificou o `alt` chave.<br /><br /></li></ul> **Nota:** apesar de muitas das chaves são opcionais, as informações que fornecer, o mais exata os resultados de localização são.<br /><br /> **Nota:** embora opcional, é encorajados a sempre especificar localização geográfica do utilizador. Fornecendo a localização é especialmente importante se o endereço IP do cliente não reflete com precisão a localização do utilizador físico (por exemplo, se o cliente utiliza VPN). Para obter melhores resultados, devem incluir este cabeçalho e o cabeçalho X-MSEdge-ClientIP mas, no mínimo, deve incluir este cabeçalho.|

> [!NOTE] 
> Lembre-se de que os termos de utilização exigir a conformidade com todas as leis aplicáveis, incluindo à utilização destes cabeçalhos. Por exemplo, determinados jurisdictions, tais como Europa, existem requisitos para obter consentimento do utilizador antes de colocar a determinados dispositivos de controlo nos dispositivos de utilizador.


<a name="content-form-types" />

### <a name="content-form-types"></a>Tipos de conteúdo de formulário

Cada pedido tem de incluir o cabeçalho Content-Type. O cabeçalho deve ser definido como: multipart /-dados do formulário; limites =\<cadeia limites\>, onde \<cadeia limites\> é uma cadeia exclusiva, opaco que identifica o limite de dados do formulário. Por exemplo, os limites = boundary_1234 abcd.


Se enviar Visual pesquisa um token de imagem ou o URL, o seguinte mostra os dados do formulário que tem de incluir no corpo do POST. Os dados do formulário têm de incluir o cabeçalho de disposição de conteúdo e a respetiva `name` parâmetro tem de ser definido como "knowledgeRequest". Para obter detalhes sobre o `imageInfo` objeto, consulte [pedido](#the-request).


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

Se carregar uma imagem local, o seguinte mostra os dados do formulário que tem de incluir no corpo do POST. Os dados do formulário têm de incluir o cabeçalho de disposição de conteúdo. O `name` parâmetro tem de ser definido como "de imagem" e o `filename` parâmetro pode ser definido como qualquer cadeia. O cabeçalho de tipo de conteúdo pode ser definido para qualquer tipo de mime da imagem comummente utilizadas. O conteúdo do formulário é o binário da imagem. O tamanho da imagem máximo, pode carregar é de 1 MB. 


```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"
Content-Type: image/jpeg

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

O seguinte mostra como especificar a região de interesse de uma imagem carregada.

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



### <a name="example-request"></a>Exemplo de pedido

O seguinte mostra um pedido de informações de imagem completa que transmite um token de imagem e a região de interesse. Obter o token de informações de uma chamada anterior para /images/search.


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

Se existem informações disponíveis para a imagem, a resposta contém um ou mais `tags` que contêm as informações. O `image` campo contém o token do insights para a imagem de entrada.

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

O `tags` campo contém um nome a apresentar e uma lista de ações (insights). Uma das etiquetas contém um `displayName` campo que está definido como uma cadeia vazia. Esta etiqueta contém as informações de predefinidos, tais como páginas Web que incluem a imagem, imagens visualmente semelhantes e compras origens para itens, encontrados na imagem. Porque é toda a imagem de interesse, a tag de insights predefinido não inclui o delimitadora caixas para as regiões de interesse.


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

Para obter uma lista de informações do predefinido, consulte [predefinido insights](./default-insights-tag.md).



As etiquetas restantes contêm outras informações que podem interessar ao utilizador. Por exemplo, se a imagem contém texto, uma das etiquetas pode incluir um insight TextResults, que contém o texto reconhecido. Em alternativa, se Bing reconhece uma entidade (pessoa, local ou coisa) na imagem, uma das etiquetas pode identificar a entidade. Pesquisa Visual também devolve um conjunto diverso de termos (etiquetas) derivado da imagem de entrada. Estas etiquetas permitem aos utilizadores explorar conceitos encontrados na imagem. Por exemplo, se a imagem de entrada de um athlete famosa, uma das etiquetas pode ser desporto, que contém ligações para as imagens de desporto.

Cada etiqueta inclui um nome de apresentação que pode utilizar para categorizar os conhecimentos delimitadora caixa que identifica a região de interesse aos quais se aplica os conhecimentos, insights próprios e uma miniatura da imagem. Por exemplo, se a imagem de uma pessoa wearing um jersey desporto, uma das etiquetas pode incluir uma caixa delimitadora que bounds o jersey e inclui VisualSearch e ProductVisualSearch insights. E outra tag pode incluir informações sobre ImageResults que contém um URL para um pedido de API /images/search obter imagens topically relacionadas com ou um URL de pesquisa de Bing.com orienta o utilizador para os resultados da pesquisa Bing.com imagem.

Todas as etiquetas que não seja a etiqueta de insights predefinida incluem delimitadora caixas que identificam as regiões de interesse na imagem. Por exemplo, se a imagem inclui várias pessoas reconhecidas, as etiquetas podem incluir delimitadora caixas para cada uma das pessoas ou se a imagem contém itens clothing reconhecido, as etiquetas podem incluir delimitadora caixas para cada item clothing reconhecido. Pode utilizar as caixas de delimitador para criar frequente oportunidades sobre a imagem que quando clicado, fornecem detalhes sobre o conteúdo nessa região da imagem. Não deve incluir oportunidades de frequente numa imagem para caixas de delimitador que identificam toda a imagem.

### <a name="text-recognition"></a>Reconhecimento de texto

Se a imagem contém o texto que o serviço reconhece, uma das etiquetas irá conter um insight TextResults (ação). Os conhecimentos `displayName` contém o texto reconhecido. 

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

Porque a tag `displayName` campo contém ##TextRecognition não utilize-o como uma categoria title no UX. Se ficar para qualquer nome que começa com # #. Em vez disso, utilize o nome a apresentar para a ação.


Reconhecimento de texto também pode reconhece as informações de contactos no cartões de visita, tal como números de telefone e endereços de correio eletrónico. A caixa delimitadora identifica a localização das informações de contactos no cartão. 

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

Se a imagem contém uma entidade reconhecida como uma pessoa, local ou coisa, uma das etiquetas pode incluir uma informação de entidade. Entidades de mensagens em fila também podem incluir trivia conforme mostrado no exemplo seguinte:

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
        },
        {
          "_type" : "ImageModuleAction",
          "actionType" : "Trivia",
          "data" : {
            "value" : [
              {
                "name" : "Where was the cornerstone of the statue of liberty laid",
                "text" : "<the answer>",
                "hostPageUrl" : "http:\/\/contoso.com\/history\/...",
              },
              {
                "name" : "Why Is the Statue of Liberty Green",
                "text" : "<the answer>",
                "hostPageUrl" : "https:\/\/www.contoso.com\/why-statue-of-liberty-is-green",
              },
              {
                "name" : "What is the Statue of Liberty made of",
                "text" : "<the answer>",
                "hostPageUrl" : "https:\/\/www.contoso.com\/art-literature\/statue-liberty-made",
              }
            ]
          }
        }
      ]
    }
```



## <a name="next-steps"></a>Passos Seguintes

Para começar a trabalhar rapidamente com o seu primeiro pedido, consulte os inícios rápidos: [c#](quickstarts/csharp.md) | [Java](quickstarts/java.md) | [node.js](quickstarts/nodejs.md)  |  [Python](quickstarts/python.md).

Experimente a API. Aceda a [consola de teste de API de pesquisa Visual](https://dev.cognitive.microsoft.com/docs/services/878c38e705b84442845e22c7bff8c9ac). 


Familiarize-se com o [Visual referência da API de pesquisa](https://aka.ms/bingvisualsearchreferencedoc). A referência contém a lista de pontos finais, cabeçalhos e os parâmetros de consulta que pretende utilizar para pedir os resultados da pesquisa. Também inclui as definições dos objetos de resposta. 

Não se esqueça de ler [utilizar do Bing e requisitos de apresentação](./use-and-display-requirements.md) , de modo não quebrar qualquer uma das regras sobre como utilizar os resultados da pesquisa.


