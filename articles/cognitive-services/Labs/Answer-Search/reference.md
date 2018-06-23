---
title: Referência de pesquisa de resposta - serviços cognitivos Microsoft do projeto | Microsoft Docs
description: Referência para o ponto final de pesquisa de resposta de projeto.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.technology: project-answer-search
ms.topic: article
ms.date: 04/13/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 8c95fac0c031ec62a9d98d6c3278bd3b3f345140
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354164"
---
# <a name="project-answer-search-v7-reference"></a>Referência de v7 de pesquisa de resposta do projeto

SearchAPI de resposta do Bing assume um parâmetro de consulta e devolve um `searchResponse` com `answerType`: `facts` ou `entities`. 

As aplicações que utilizam a API de pesquisa de resposta enviam pedidos para o ponto final com um URL para um parâmetro de consulta de pré-visualização.  O pedido tem de incluir o `q=searchTerm` parâmetro e *Ocp-Apim-Subscription-Key* cabeçalho.   

É possível analisar a resposta JSON de factos e entidades que contém detalhes sobre o objeto da pesquisa.

## <a name="endpoint"></a>Ponto Final
Para pedir os resultados da pesquisa de resposta, envie um pedido para o seguinte ponto final. Utilize os parâmetros de URL e cabeçalhos para definir mais especificações.

Ponto final GET: 
````
https://api.labs.cognitive.microsoft.com/answerSearch/v7.0/search?q=<searchTerm>&subscription-key=0123456789ABCDEF&mkt=en-us

````

O pedido tem de utilizar o protocolo HTTPS e incluem o seguinte parâmetro de consulta:
-  q =<URL> -a consulta que identifica o objeto de pesquisa

Para obter exemplos mostram como fazer pedidos, consulte [c# início rápido](c-sharp-quickstart.md) ou [início rápido do Java](java-quickstart.md). 

As secções seguintes fornecem detalhes técnicos sobre os objetos de resposta, parâmetros de consulta e cabeçalhos que afetam os resultados da pesquisa. 
  
Para obter informações sobre cabeçalhos que devem incluir pedidos, consulte [cabeçalhos](#headers).  
  
Para obter informações sobre os parâmetros de consulta que devem incluir pedidos, consulte [parâmetros de consulta](#query-parameters).  
  
Para obter informações sobre o JSON objetos que a resposta inclui, consulte [objetos de resposta](#response-objects).

O comprimento do URL de consulta máxima é de 2.048 carateres. Para garantir que o comprimento do URL não pode exceder o limite, o comprimento máximo de parâmetros da sua consulta deve ser inferior a 1.500 carateres. Se o URL exceder 2.048 carateres, o servidor devolve 404 não encontrado.  

Para obter informações sobre a utilização permitida e apresentação de resultados, consulte [utilizar e apresentar requisitos](use-display-requirements.md). 

> [!NOTE]
> Alguns cabeçalhos de pedido são significativos para outras APIs de pesquisa não afetam a pré-visualização de URL
> - Pragma – o chamador não tem controlo sobre a pré-visualização de URL utiliza a cache
> - Cache-Control – o autor da chamada não têm controlo sobre se o URL de pré-visualização utiliza a cache
> - Agente de utilizador

> Além disso, alguns parâmetros não são atualmente significativos para API de pré-visualização de URL, mas podem ser utilizados no futuro para globalização melhorada. 
 
## <a name="headers"></a>Cabeçalhos  
Seguem-se os cabeçalhos que podem incluir um pedido e resposta.  
  
|Cabeçalho|Descrição|  
|------------|-----------------|  
|Aceitar|Cabeçalho de pedido opcional.<br /><br /> O tipo de suporte de dados predefinido é aplicação/json. Para especificar que utilizam a resposta [JSON LD](http://json-ld.org/), defina o cabeçalho Accept para aplicação/ld + json.|  
|<a name="acceptlanguage" />Idioma aceitar|Cabeçalho de pedido opcional.<br /><br /> Uma lista delimitada por vírgulas de idiomas para utilizar cadeias de interface de utilizador. A lista está a ser diminuir a ordem de preferência. Para obter mais informações, incluindo o formato esperado, consulte [RFC2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).<br /><br /> Este cabeçalho e o [setLang](#setlang) parâmetro da consulta são mutuamente exclusivos&mdash;não especificar ambos.<br /><br /> Se definir este cabeçalho, também tem de especificar o [cc](#cc) parâmetro de consulta. Para determinar o mercado para devolver resultados, Bing utiliza o idioma suportado primeiro localiza a partir da lista e combina-o com o `cc` valor do parâmetro. Se a lista inclui um idioma suportado, o Bing localiza o idioma e o mercado que suporta o pedido mais próximo ou utiliza um agregado ou default mercado para os resultados. Para determinar o mercado que o Bing utilizado, consulte o cabeçalho de BingAPIs mercado.<br /><br /> Utilize este cabeçalho e o `cc` parâmetro de consulta apenas se especificar vários idiomas. Caso contrário, utilize o [mkt](#mkt) e [setLang](#setlang) parâmetros de consulta.<br /><br /> Uma cadeia de interface de utilizador é uma cadeia que é utilizada como uma etiqueta numa interface de utilizador. Existem algumas cadeias de interface de utilizador nos objetos de resposta de JSON. Todas as ligações para as propriedades de Bing.com nos objetos de resposta aplicam-se o idioma especificado.|  
|<a name="market" />Mercado BingAPIs|Cabeçalho de resposta.<br /><br /> Mercado utilizado por pedido. O formulário é \<languageCode\>-\<indicativo do país\>. Por exemplo, en-US.|  
|<a name="traceid" />BingAPIs TraceId|Cabeçalho de resposta.<br /><br /> O ID da entrada de registo que contém os detalhes do pedido. Quando ocorre um erro, capturar este ID. Se não conseguir determinar e resolver o problema, inclua este ID, juntamente com outras informações que fornecer a equipa de suporte.|  
|<a name="subscriptionkey" />OCP Apim-subscrição-chave|Cabeçalho de pedido necessários.<br /><br /> A chave de subscrição que recebeu quando se inscreveu para este serviço em [serviços cognitivos](https://www.microsoft.com/cognitive-services/).|  
|<a name="pragma" />Pragma|Cabeçalho de pedido opcional<br /><br /> Por predefinição, o Bing devolve os conteúdos em cache, se disponível. Para impedir que o Bing devolver o conteúdo em cache, defina o cabeçalho de Pragma para no-cache (por exemplo, Pragma: não cache).
|<a name="useragent" />Agente de utilizador|Cabeçalho de pedido opcional.<br /><br /> O agente de utilizador com a origem do pedido. Bing utiliza o agente de utilizador para fornecer os utilizadores móveis com uma experiência otimizada. Embora seja opcional, são encorajados a sempre especifique este cabeçalho.<br /><br /> O agente de utilizador deve ser a mesma cadeia de qualquer browser frequentemente utilizada envia. Para obter informações sobre agentes de utilizador, consulte [RFC 2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).<br /><br /> Seguem-se exemplos de cadeias de agente do utilizador.<br /><ul><li>Windows Phone&mdash;Mozilla/5.0 (compatível; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)<br /><br /></li><li>Android&mdash;Mozilla/5.0 (Linux; U; Android 2.3.5; en-us; SCH-I500 compilação/GINGERBREAD) AppleWebKit/533.1 (KHTML; como Gecko) versão/4.0 Mobile Safari/533.1<br /><br /></li><li>iPhone&mdash;Mozilla/5.0 (iPhone; CPU iPhone SO 6_1 como Mac OS X) AppleWebKit/536.26 (KHTML; como Gecko) Mobile/10B142 iPhone4; 1 BingWeb/3.03.1428.20120423<br /><br /></li><li>PC&mdash;Mozilla/5.0 (do Windows NT 6.3; WOW64; Trident/7.0; Touch; Rv:11.0) como Gecko<br /><br /></li><li>iPad&mdash;Mozilla/5.0 (iPad; SO de CPU 7_0 como Mac OS X) AppleWebKit/537.51.1 (KHTML, como Gecko) versão/7.0 Mobile/11A465 Safari/9537.53</li></ul>|
|<a name="clientid" />X-MSEdge-ClientID|Cabeçalho de pedido e resposta opcional.<br /><br /> Bing utiliza este cabeçalho para fornecer aos utilizadores com o comportamento consistente em toda as chamadas de API do Bing. Bing frequentemente flights funcionalidades novas e melhoradas e utiliza o ID de cliente como uma chave para atribuição de tráfego na flights diferentes. Se não utilizar o mesmo ID de cliente para um utilizador em vários pedidos, em seguida, Bing pode atribuir o utilizador a vários flights em conflito. A ser atribuídos a vários flights em conflito podem levar a uma experiência de utilizador inconsistente. Por exemplo, se o segundo pedido possui uma atribuição de voo diferente que o primeiro, a experiência poderá inesperada. Além disso, o Bing pode utilizar o ID de cliente para personalizar os resultados da web para que o cliente IDs de histórico de pesquisa, proporcionando uma experiência mais rica para o utilizador.<br /><br /> Bing também utiliza este cabeçalho para ajudar a melhorar as classificações de resultado ao analisar a atividade gerada por um ID de cliente. As melhorias de relevância ajudá-lo com melhor qualidade dos resultados entregue ao Bing APIs e por sua vez permite superiores através de clique taxas para o consumidor da API.<br /><br /> **Importante:** embora opcional, deve considerar este cabeçalho obrigado. A persistência do ID de cliente em vários pedidos para o mesmo utilizador final e a combinação de dispositivos permite 1) o consumidor da API receber uma experiência de utilizador consistente e taxas 2) superiores clique-through através de melhor qualidade dos resultados das APIs do Bing.<br /><br /> Seguem-se as regras de utilização básica que se aplicam a este cabeçalho.<br /><ul><li>Cada utilizador que utiliza a sua aplicação no dispositivo tem de ter um único, Bing gerado ID de cliente.<br /><br/>Se não incluir este cabeçalho do pedido, o Bing gera um ID e devolve a mesma no cabeçalho de resposta de X-MSEdge-ClientID. O único momento em que não deve incluir este cabeçalho num pedido for a primeira vez que o utilizador utiliza a sua aplicação nesse dispositivo.<br /><br/></li><li>Utilize o ID de cliente para cada pedido de API do Bing que torna a sua aplicação para este utilizador no dispositivo.<br /><br/></li><li>**Atenção:** tem de garantir que este ID de cliente não é linkable para quaisquer informações de conta de utilizador authenticatable.</li><br/><li>Manter o ID de cliente. Para manter o ID de uma aplicação de browser, utilize um cookie HTTP persistente para garantir que o ID é utilizado em todas as sessões. Não utilize um cookie de sessão. Para outras aplicações, tais como as aplicações móveis, utilize o armazenamento persistente do dispositivo para manter o ID.<br /><br/>Da próxima vez que o utilizador utiliza a sua aplicação nesse dispositivo, obter o ID de cliente que manteve.</li></ul><br /> **Nota:** respostas Bing pode ou não pode incluir este cabeçalho. Se a resposta inclui este cabeçalho, o ID de cliente de captura e utilizá-la para todos os pedidos subsequentes do Bing para o utilizador em que o dispositivo.<br /><br /> **Nota:** se incluir o X-MSEdge-ClientID, não pode incluir os cookies no pedido.|  
|<a name="clientip" />X-MSEdge-ClientIP|Cabeçalho de pedido opcional.<br /><br /> O endereço IPv4 ou IPv6 do dispositivo cliente. O endereço IP é utilizado para detetar a localização do utilizador. Bing utiliza as informações de localização para determinar o comportamento de procura seguro.<br /><br /> **Nota:** embora opcional, é encorajados a sempre especifique este cabeçalho e o cabeçalho X-pesquisa de localização.<br /><br /> Não é obfuscate o endereço (por exemplo, alterando o último octeto para 0). Obfuscating os resultados de endereço na localização que não estão a ser em qualquer lugar, quase localização concreta do dispositivo, que poderá resultar no Bing que serve resultados errados.|  
|<a name="location" />Localização de pesquisa X|Cabeçalho de pedido opcional.<br /><br /> Uma lista delimitada por ponto e vírgula dos pares chave/valor que descrevem a localização geográfica do cliente. Bing utiliza as informações de localização para determinar o comportamento de procura seguro e para devolver o conteúdo local relevante. Especifique o par chave/valor como \<chave\>:\<valor\>. Seguem-se as chaves que utiliza para especificar a localização do utilizador.<br /><br /><ul><li>LAT&mdash;a latitude da localização do cliente, em graus. A latitude tem de ser maior que ou igual a-90.0 e inferior ou igual a +90.0. Os valores negativos indicam latitudes Austrais e valores positivos indicam latitudes Norte.<br /><br /></li><li>muito&mdash;a longitude da localização do cliente, em graus. A longitude tem de ser maior que ou igual a-180.0 e inferior ou igual a +180.0. Os valores negativos indicam longitudes tsunami e valores positivos indicam longitudes Leste.<br /><br /></li><li>Re&mdash; raio, no medidores, que especifica a precisão horizontal das coordenadas. Passe o valor devolvido pelo serviço de localização do dispositivo. Valores típicos podem estar 22m para GPS/Wi-Fi, 380m para triangulation Torre de célula e 18,000m para pesquisa inversa de IP.<br /><br /></li><li>TS&mdash; o UTC UNIX timestamp da quando o cliente na localização. (O timestamp UNIX é o número de segundos desde 1 de Janeiro de 1970.)<br /><br /></li><li>HEAD&mdash;opcional. Cabeçalho relativo ou direção do levar do cliente. Especifique a direção da levar como graus de 0 a 360, contando no sentido horário em relação ao norte verdadeiro. Especifique esta chave apenas se o `sp` chave é diferente de zero.<br /><br /></li><li>SP&mdash; a velocidade de horizontal (velocidade), em medidores por segundo, que viaja de dispositivo cliente.<br /><br /></li><li>ALT&mdash; altitude do dispositivo cliente, no medidores.<br /><br /></li><li>são&mdash;opcional. O radius, no medidores, que especifica a precisão vertical das coordenadas. RADIUS assume a predefinição 50 Kilometers. Especifique esta chave apenas se especificou o `alt` chave.<br /><br /></li></ul> **Nota:** apesar destas chaves são opcionais, as informações que fornecer, o mais exata os resultados de localização são.<br /><br /> **Nota:** é encorajados a sempre especificar localização geográfica do utilizador. Fornecendo a localização é especialmente importante se o endereço IP do cliente não reflete com precisão a localização do utilizador físico (por exemplo, se o cliente utiliza VPN). Para obter melhores resultados, devem incluir este cabeçalho e o cabeçalho X-MSEdge-ClientIP mas, no mínimo, deve incluir este cabeçalho.|

> [!NOTE] 
> Lembre-se de que os termos de utilização exigir a conformidade com todas as leis aplicáveis, incluindo à utilização destes cabeçalhos. Por exemplo, determinados jurisdictions, tais como Europa, existem requisitos para obter consentimento do utilizador antes de colocar a determinados dispositivos de controlo nos dispositivos de utilizador.
  

## <a name="query-parameters"></a>Parâmetros de consulta  
O pedido pode incluir os seguintes parâmetros de consulta. Consulte a coluna necessária para os parâmetros necessários. Tem de URL codificar os parâmetros de consulta.  
  
  
|Nome|Valor|Tipo|Necessário|  
|----------|-----------|----------|--------------|  
|<a name="mkt" />mkt|Mercado onde os resultados provenientes. <br /><br />Para obter uma lista de valores de mercado possíveis, consulte [códigos de mercado](#market-codes).<br /><br /> **Nota:** a API de pré-visualização do URL atualmente suporta apenas en-us mercado e de idioma.<br /><br />|Cadeia|Sim|  
|<a name="query" />Q|O URL para pré-visualização|Cadeia|Sim|  
|<a name="responseformat" />responseFormat|Escreva o suporte de dados a utilizar para a resposta. Seguem-se os valores possíveis sensível.<br /><ul><li>JSON</li><li>JSONLD</li></ul><br /> A predefinição é JSON. Para obter informações sobre o JSON objetos que contém a resposta, consulte [resposta objetos](#response-objects).<br /><br />  Se especificar JsonLd, o corpo da resposta inclui objetos JSON LD que contêm os resultados da pesquisa. Para obter informações sobre o LD JSON, consulte [JSON LD](http://json-ld.org/).|Cadeia|Não|  
|<a name="safesearch" />safeSearch|Um filtro utilizado para filtrar o conteúdo para adultos. Seguem-se os valores de filtro sensível possíveis.<br /><ul><li>Desativar&mdash;devolver páginas Web para adultos texto, imagens ou vídeos.<br /><br/></li><li>Moderada&mdash;devolver páginas Web com o texto para adultos, mas não para adultos imagens ou vídeos.<br /><br/></li><li>Strict&mdash;não devolveu a páginas Web para adultos texto, imagens ou vídeos.</li></ul><br /> A predefinição é moderada.<br /><br /> **Nota:** se o pedido for proveniente de um mercado política para adultos esse Bing requer que `safeSearch` está definido para Strict, Bing ignora a `safeSearch` valor e utiliza Strict.<br/><br/>**Nota:** se utilizar o `site:` operador de consulta, há a possibilidade de que a resposta pode conter conteúdo para adultos independentemente do que o `safeSearch` parâmetro de consulta está definido como. Utilize `site:` apenas se estiver em consideração o conteúdo do site e a possibilidade de conteúdo para adultos suporta o seu cenário. |Cadeia|Não|  
|<a name="setlang" />setLang|O idioma a utilizar para cadeias de interface de utilizador. Especifique o idioma utilizando o código de idioma de 2 letra 639-1 ISO. Por exemplo, o código de idioma do inglês é EN. A predefinição é EN (inglês).<br /><br /> Embora seja opcional, deve sempre especificar o idioma. Normalmente, definidas `setLang` para a mesma linguagem especificada pelo `mkt` , a menos que o utilizador pretende que as cadeias de interface de utilizador apresentadas num idioma diferente.<br /><br /> Este parâmetro e o [aceitar idioma](#acceptlanguage) cabeçalho são mutuamente exclusivos&mdash;não especificar ambos.<br /><br /> Uma cadeia de interface de utilizador é uma cadeia que é utilizada como uma etiqueta numa interface de utilizador. Existem algumas cadeias de interface de utilizador nos objetos de resposta de JSON. Além disso, todas as ligações para as propriedades de Bing.com nos objetos de resposta aplicam-se o idioma especificado.|Cadeia|Não| 


## <a name="response-objects"></a>Objetos de resposta  
O esquema de resposta é a uma [página Web] ou ErrorResponse, como a API de pesquisa de Web. Se falhar o pedido, o objeto de nível superior é o [ErrorResponse](#errorresponse) objeto.


|Object|Descrição|  
|------------|-----------------|  
|A página [Web]|Superior ao nível objeto JSON que contém os atributos de pré-visualização.|  
|[Facto]|Superior ao nível objeto JSON que contém factos.| 
|[Entidades|Superior ao nível objeto JSON que contém detalhes de entidade.| 

  
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

  
  
### <a name="license"></a>Licença  
Define a licença sob a qual o texto ou fotografia pode ser utilizada.  
  
|Nome|Valor|Tipo|  
|----------|-----------|----------|  
|nome|O nome da licença.|Cadeia|  
|url|O URL para um Web site onde o utilizador pode obter mais informações sobre a licença.<br /><br /> Utilize o nome e o URL para criar uma hiperligação.|Cadeia|  
  

### <a name="licenseattribution"></a>LicenseAttribution  
Define uma regra contractual para atribuição de licença.  
  
|Nome|Valor|Tipo|  
|----------|-----------|----------|  
|Escreva|Uma sugestão de tipo, que está definida para LicenseAttribution.|Cadeia|  
|licença|A licença sob a qual o conteúdo pode ser utilizado.|[Licença](#license)|  
|licenseNotice|A licença para apresentar junto ao campo de destino. Por exemplo, "Text com licença de CC por SA".<br /><br /> Utilizar o nome e o URL na licença do `license` campo para criar uma hiperligação para o Web site que descreve os detalhes da licença. Em seguida, substitua o nome de licença no `licenseNotice` cadeia (por exemplo, CC-por-SA) com a hiperligação que acabou de criar.|Cadeia|  
|mustBeCloseToContent|Um valor booleano que determina se o conteúdo da regra tem de ser colocado numa fechar proximidade para o campo que se aplica a regra. Se **verdadeiro**, tem de ser colocado o conteúdo próximos. Se **falso**, ou não existe neste campo, o conteúdo pode ser colocado no critério da função invocadora.|Booleano|  
|targetPropertyName|O nome do campo que se aplica a regra.|Cadeia|  
  

### <a name="link"></a>Ligação  
Define os componentes de um hyperlink.  
  
|Nome|Valor|Tipo|  
|----------|-----------|----------|  
|Escreva|Sugestão de tipo.|Cadeia|  
|texto|O texto a apresentar.|Cadeia|  
|url|UM URL. Utilizar o URL e apresentar o texto para criar uma hiperligação.|Cadeia|  
  

### <a name="linkattribution"></a>LinkAttribution  
Define uma regra contractual para atribuição de ligação.  
  
|Nome|Valor|Tipo|  
|----------|-----------|----------|  
|Escreva|Uma sugestão de tipo, que está definida para LinkAttribution.|Cadeia|  
|mustBeCloseToContent|Um valor booleano que determina se o conteúdo da regra tem de ser colocado numa fechar proximidade para o campo que se aplica a regra. Se **verdadeiro**, tem de ser colocado o conteúdo próximos. Se **falso**, ou não existe neste campo, o conteúdo pode ser colocado no critério da função invocadora.|Booleano|  
|targetPropertyName|O nome do campo que se aplica a regra.<br /><br /> Se não for especificado um destino, a atribuição aplica-se à entidade como um todo e deve ser apresentada imediatamente a seguir a apresentação de entidade. Se existirem várias regras atribuição de texto e a ligação que não especificam um destino, deve utilizar para concatená-los e apresentá-los utilizando um "dados a partir de:" etiqueta. Por exemplo, "dados a partir de < fornecedor Nome1\> &#124; < fornecedor name2\>".|Cadeia|  
|texto|O texto de atribuição.|Cadeia|  
|url|O URL para o site do fornecedor. Utilize `text` e o URL para criar da hiperligação.|Cadeia|  
  
  
### <a name="mediaattribution"></a>MediaAttribution  
Define uma regra contractual para atribuição de suporte de dados.  
  
|Nome|Valor|Tipo|  
|----------|-----------|----------|  
|Escreva|Uma sugestão de tipo, que está definida para MediaAttribution.|Cadeia|  
|mustBeCloseToContent|Um valor booleano que determina se o conteúdo da regra tem de ser colocado numa fechar proximidade para o campo que se aplica a regra. Se **verdadeiro**, tem de ser colocado o conteúdo próximos. Se **falso**, ou não existe neste campo, o conteúdo pode ser colocado no critério da função invocadora.|Booleano|  
|targetPropertyName|O nome do campo que se aplica a regra.|Cadeia|  
|url|O URL que utilizou para criar da hiperligação do conteúdo do suporte de dados. Por exemplo, se o destino é uma imagem, utilizaria o URL para tornar a imagem clicável.|Cadeia|  
  
  
  
### <a name="organization"></a>Organização  
Define um publicador.  
  
Tenha em atenção que um fabricante poderá fornecer respetivo nome ou o seu Web site ou ambos.  
  
|Nome|Valor|Tipo|  
|----------|-----------|----------|  
|nome|O nome do Editor.|Cadeia|  
|url|O URL para o site do fabricante.<br /><br /> Tenha em atenção que o publicador não fornecer um Web site.|Cadeia|  
  
  

### <a name="webpage"></a>Página Web  
Define as informações sobre uma página Web em pré-visualização.  
  
|Nome|Valor|Tipo|  
|----------|-----------|----------|
|nome|O título da página, não necessariamente o título HTML|Cadeia|
|url|O URL que foi realmente pesquisado (pedido poderá seguiu redirecionamentos)|Cadeia|  
|descrição|Breve descrição da página e do conteúdo|Cadeia|  
|isFamilyFriendly|Mais exatos para itens no índice web; em tempo real fetches efetue esta deteção baseada unicamente no URL e não o conteúdo da página|boolean|
|primaryImageOfPage/contentUrl|O URL para uma imagem representativo para incluir na pré-visualização|Cadeia| 
  
  
### <a name="querycontext"></a>QueryContext  
Define o contexto de consulta que o Bing utilizado no pedido.  
  
|Elemento|Descrição|Tipo|  
|-------------|-----------------|----------|  
|adultIntent|Um valor booleano que indica se a consulta especificada tem intenção para adultos. O valor é **verdadeiro** se a consulta tem intenção para adultos; caso contrário, **falso**.|Booleano|  
|alterationOverrideQuery|A cadeia de consulta a utilizar para forçar o Bing para utilizar a cadeia original. Por exemplo, se a cadeia de consulta é *saling downwind*, a cadeia de consulta de substituição será *+ saling downwind*. Lembre-se codificar a cadeia de consulta que resulta em *% 2Bsaling + downwind*.<br /><br /> Este campo está incluído apenas se a cadeia de consulta original contém um erro de ortografia.|Cadeia|  
|alteredQuery|A cadeia de consulta utilizada pelo Bing para executar a consulta. Bing utiliza a cadeia de consulta alterada se a cadeia de consulta original continha prende ortografia. Por exemplo, se a cadeia de consulta é `saling downwind`, a cadeia de consulta alterada será `sailing downwind`.<br /><br /> Este campo está incluído apenas se a cadeia de consulta original contém um erro de ortografia.|Cadeia|  
|askUserForLocation|Um valor booleano que indica se Bing necessita que a localização do utilizador para fornecer resultados precisos. Se especificar a localização do utilizador utilizando o [X-MSEdge-ClientIP](#clientip) e [X-pesquisa-Location](#location) cabeçalhos, pode ignorar este campo.<br /><br /> Para a localização em atenção as consultas, tais como "Meteorologia hoje" ou "restaurants contra quase-me" que têm a localização do utilizador para fornecer resultados precisos, este campo está definido como **verdadeiro**.<br /><br /> Para a localização em atenção as consultas que incluem a localização (por exemplo, "meteorologia da Seattle"), este campo está definido como **falso**. Este campo também está definido como **falso** para consultas que não são localização tenha em atenção, tais como o "melhor sellers".|Booleano|  
|originalQuery|A cadeia de consulta conforme especificado no pedido.|Cadeia|  

### <a name="identifiable"></a>Identificação
|Nome|Valor|Tipo|  
|-------------|-----------------|----------|
|ID|Um identificador de recurso|Cadeia|
 
### <a name="rankinggroup"></a>RankingGroup
Define uma pesquisa resulta de grupo, tais como mainline.
|Nome|Valor|Tipo|  
|-------------|-----------------|----------|
|itens|Uma lista de resultados de pesquisa para apresentar no grupo.|RankingItem|

### <a name="rankingitem"></a>RankingItem
Define um item de resultado de pesquisa para apresentar.
|Nome|Valor|Tipo|  
|-------------|-----------------|----------|
|resultIndex|Um índice baseado em zero do item de resposta para apresentar. Se o item não incluir este campo, apresenta todos os itens na resposta. Por exemplo, apresentam todos os artigos de notícias a resposta de notícias de última hora.|Número inteiro|
|answerType|A resposta que contém o item para apresentar. Por exemplo, notícias de última hora.<br /><br />Utilize o tipo para encontrar a resposta no objeto SearchResponse. O tipo é o nome de um campo SearchResponse.<br /><br /> No entanto, utilizar o tipo de resposta só se este objeto inclui o campo de valor; caso contrário, ignorá-lo.|Cadeia|
|textualIndex|O índice da resposta na textualAnswers para apresentar.| Número inteiro não assinado|
|valor|O ID que identifica uma resposta para apresentar ou um item de uma resposta para apresentar. Se o ID de identifica uma resposta, apresenta todos os itens da resposta.|Identificação|

### <a name="rankingresponse"></a>RankingResponse  
Define onde na procura deve ser colocado o conteúdo da página de resultados e a ordem pela qual.  
  
|Nome|Valor|  
|----------|-----------|  
|<a name="ranking-mainline" />mainline|Os resultados da pesquisa para apresentar o mainline.|  
|<a name="ranking-pole" />ao pólo|Os resultados da pesquisa que devem ser garantidos o tratamento mais visível (por exemplo, é apresentado o mainline acima e barra lateral).|  
|<a name="ranking-sidebar" />barra lateral|Os resultados da pesquisa a apresentar na barra lateral.| 


### <a name="searchresponse"></a>SearchResponse  
Define o objeto de nível superior de resposta inclui quando o pedido seja bem-sucedido.  
  
Tenha em atenção que, se o serviço suspeita um ataque de recusa de serviço, o pedido terá êxito (código de estado HTTP é 200 OK); No entanto, o corpo da resposta estará vazio.  
  
|Nome|Valor|Tipo|  
|----------|-----------|----------|  
|Escreva|Sugestão de tipo, que está definido para SearchResponse.|Cadeia|  
|Página Web|Um objeto JSON que define a pré-visualização|cadeia|  
  
  
### <a name="textattribution"></a>TextAttribution  
Define uma regra contractual para atribuição de texto simples.  
  
|Nome|Valor|Tipo|  
|----------|-----------|----------|  
|Escreva|Uma sugestão de tipo, que está definida para TextAttribution.|Cadeia|  
|texto|O texto de atribuição.<br /><br /> Atribuição de texto aplica-se à entidade como um todo e deve ser apresentada imediatamente a seguir a apresentação de entidade. Se existirem várias regras atribuição de texto ou de ligação que não especificam um destino, deve utilizar para concatená-los e apresentá-los utilizando um "dados a partir de:" etiqueta.|Cadeia| 


## <a name="error-codes"></a>Códigos de erro

Seguem-se os códigos de estado HTTP possíveis que devolve um pedido.  
  
|Código de Estado|Descrição|  
|-----------------|-----------------|  
|200|Êxito.|  
|400|Um dos parâmetros de consulta está em falta ou não é válido.|  
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
- [Início rápido c#](c-sharp-quickstart.md)
- [Guia de introdução do Java](java-quickstart.md)
- [Guia de introdução do nó](node-quickstart.md)
- [Guia de introdução do Python](python-quickstart.md)

