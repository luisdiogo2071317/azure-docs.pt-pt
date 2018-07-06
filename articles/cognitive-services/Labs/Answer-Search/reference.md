---
title: Referência de pesquisa de resposta - serviços cognitivos da Microsoft do projeto | Documentos da Microsoft
description: Referência para o ponto final de pesquisa de resposta de projeto.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.technology: project-answer-search
ms.topic: article
ms.date: 04/13/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: a12761c2d913cd7ffaa2cbc2cd42576c6bc96434
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37866989"
---
# <a name="project-answer-search-v7-reference"></a>Referência de v7 de pesquisa de resposta do projeto

Bing resposta SearchAPI assume um parâmetro de consulta e retorna um `searchResponse` com `answerType`: `facts` ou `entities`. 

Aplicativos que usam a API de pesquisa de resposta enviam pedidos para o ponto final com um URL para um parâmetro de consulta de pré-visualização.  O pedido tem de incluir o `q=searchTerm` parâmetro e *Ocp-Apim-Subscription-Key* cabeçalho.   

A resposta JSON pode ser analisada para factos e entidades que contêm detalhes sobre o objeto de pesquisa.

## <a name="endpoint"></a>Ponto Final
Para pedir os resultados da pesquisa de resposta, envie um pedido para o seguinte ponto de extremidade. Utilize os cabeçalhos e os parâmetros de URL para definir ainda mais especificações.

Ponto final GET: 
````
https://api.labs.cognitive.microsoft.com/answerSearch/v7.0/search?q=<searchTerm>&subscription-key=0123456789ABCDEF&mkt=en-us

````

O pedido tem de utilizar o protocolo HTTPS e incluir o parâmetro de consulta a seguir:
-  p =<URL> -a consulta que identifica o objeto de pesquisa

Para obter exemplos que mostram como fazer pedidos, consulte [c# início rápido](c-sharp-quickstart.md) ou [início rápido de Java](java-quickstart.md). 

As secções seguintes fornecem detalhes técnicos sobre os objetos de resposta, parâmetros de consulta e cabeçalhos que afetam os resultados da pesquisa. 
  
Para obter informações sobre cabeçalhos de pedidos devem incluir, consulte [cabeçalhos](#headers).  
  
Para obter informações sobre parâmetros de consulta que devem incluir pedidos, consulte [parâmetros de consulta](#query-parameters).  
  
Para obter informações sobre o JSON de objetos que inclui a resposta, consulte [objetos de resposta](#response-objects).

O comprimento do URL de consulta máxima é de 2048 carateres. Para garantir que o comprimento do URL não excede o limite, o comprimento máximo de seus parâmetros de consulta deve ser inferior a 1 500 carateres. Se o URL exceder 2048 carateres, o servidor devolve 404 não encontrado.  

Para obter informações sobre o uso permitido e a exibição de resultados, consulte [utilizar e apresentam os requisitos](use-display-requirements.md). 

> [!NOTE]
> Alguns cabeçalhos de pedido com significado para outras APIs de pesquisa não afetam a pré-visualização do URL
> - Pragma – o chamador não tem controlo sobre se o URL de visualização utiliza a cache
> - Cache-Control – o chamador não tem controlo sobre se o URL de visualização utiliza a cache
> - Agente de utilizador

> Além disso, alguns parâmetros não são atualmente significativos para a API de pré-visualização do URL, mas podem ser utilizados no futuro para a globalização melhorada. 
 
## <a name="headers"></a>Cabeçalhos  
Seguem-se os cabeçalhos que podem incluir uma solicitação e resposta.  
  
|Cabeçalho|Descrição|  
|------------|-----------------|  
|Aceitar|Cabeçalho do pedido opcional.<br /><br /> O tipo de suporte de dados predefinido é application/json. Para especificar que a resposta é usar [JSON LD](http://json-ld.org/), defina o cabeçalho Accept como application/ld + json.|  
|<a name="acceptlanguage" />Aceite-Language|Cabeçalho do pedido opcional.<br /><br /> Uma lista delimitada por vírgulas de idiomas a utilizar para cadeias de caracteres de interface de usuário. A lista está por ordem de preferência decrescente. Para obter mais informações, incluindo o formato esperado, consulte [RFC2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).<br /><br /> Este cabeçalho e o [setLang](#setlang) parâmetro de consulta são mutuamente exclusivas&mdash;não especificar ambos.<br /><br /> Se definir este cabeçalho, também tem de especificar o [cc](#cc) parâmetro de consulta. Para determinar o mercado para devolver resultados, o Bing utiliza o primeiro idioma suportado que encontra na lista e combina-o com o `cc` valor do parâmetro. Se a lista não inclui um idioma suportado, o Bing localiza o idioma e o mercado que oferece suporte a solicitação mais próximo ou ele utiliza um agregado ou padrão de mercado para os resultados. Para determinar o mercado que Bing utilizado, consulte o cabeçalho de BingAPIs de colocação no mercado.<br /><br /> Utilize este cabeçalho e o `cc` parâmetro de consulta apenas se especificar vários idiomas. Caso contrário, utilize o [mkt](#mkt) e [setLang](#setlang) parâmetros de consulta.<br /><br /> Uma cadeia de caracteres de interface do usuário é uma cadeia que é utilizada como etiqueta numa interface de utilizador. Existem algumas cadeias de caracteres de interface de utilizador nos objetos de resposta de JSON. Todas as ligações às propriedades de Bing.com nos objetos de resposta aplicam-se o idioma especificado.|  
|<a name="market" />Mercado de BingAPIs|Cabeçalho de resposta.<br /><br /> O mercado utilizado pelo pedido. O formulário é \<languageCode\>-\<indicativo do país\>. Por exemplo, en-US.|  
|<a name="traceid" />BingAPIs TraceId|Cabeçalho de resposta.<br /><br /> O ID da entrada de log que contém os detalhes do pedido. Quando ocorre um erro, capturar este ID. Se não for capaz de determinar e resolver o problema, inclua essa identificação, juntamente com as outras informações que fornecer a equipa de suporte.|  
|<a name="subscriptionkey" />OCP-Apim-Subscription-Key|Cabeçalho do pedido necessário.<br /><br /> A chave de subscrição que recebeu quando se inscreveu para este serviço no [dos serviços cognitivos](https://www.microsoft.com/cognitive-services/).|  
|<a name="pragma" />Pragma|Cabeçalho do pedido opcional<br /><br /> Por predefinição, o Bing devolve os conteúdos em cache, se disponível. Para impedir que o Bing retornando conteúdo em cache, defina o cabeçalho de Pragma para no cache (por exemplo, Pragma: não-cache).
|<a name="useragent" />Agente de utilizador|Cabeçalho do pedido opcional.<br /><br /> O agente de utilizador com a origem do pedido. Bing utiliza o agente de utilizador para fornecer aos usuários móveis com uma experiência otimizada. Embora seja opcional, é encorajado a sempre especificar este cabeçalho.<br /><br /> O agente de utilizador deve ser a mesma cadeia de qualquer navegador usada envia. Para obter informações sobre agentes de utilizador, consulte [RFC 2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).<br /><br /> Seguem-se exemplos de cadeias de caracteres do agente do usuário.<br /><ul><li>Windows Phone&mdash;Mozilla/5.0 (compatível; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Toque; NOKIA; Lumia 822)<br /><br /></li><li>Android&mdash;Mozilla/5.0 (Linux; U; Android 2.3.5; en-us; SCH-I500 compilação/GINGERBREAD) AppleWebKit/533.1 (KHTML; como Gecko) versão/4.0 Mobile Safari/533.1<br /><br /></li><li>iPhone&mdash;Mozilla/5.0 (iPhone; CPU 6_1 de iPhone OS como Mac OS X) AppleWebKit/536.26 (KHTML; como Gecko) Mobile/10B142 iPhone4; 1 BingWeb/3.03.1428.20120423<br /><br /></li><li>PC&mdash;Mozilla/5.0 (Windows NT 6.3; WOW64; Trident/7.0; Toque; Rv:11.0) como Gecko<br /><br /></li><li>iPad&mdash;Mozilla/5.0 (iPad; 7_0 de SO de CPU, como o Mac OS X) AppleWebKit/537.51.1 (KHTML, como Gecko) versão/7.0 Mobile/11A465 Safari/9537.53</li></ul>|
|<a name="clientid" />X-MSEdge-ID de cliente|Cabeçalho de solicitação e resposta opcionais.<br /><br /> O Bing utiliza este cabeçalho para fornecer aos utilizadores um comportamento consistente em chamadas de API do Bing. Bing, muitas vezes, por vários voos novos recursos e aprimoramentos, e utiliza o ID de cliente como uma chave para atribuir o tráfego de voos diferentes. Se não utilizar o mesmo ID de cliente para um utilizador em vários pedidos, Bing pode atribuir o utilizador a vários voos em conflito. A ser atribuído a vários voos em conflito pode levar a uma experiência de usuário inconsistente. Por exemplo, se a segunda solicitação tiver uma atribuição de voo diferentes que o primeiro, a experiência poderão ser inesperada. Além disso, o Bing pode utilizar o ID de cliente para personalizar os resultados da web para que o cliente IDs de histórico de pesquisas, fornecendo uma experiência mais rica para o utilizador.<br /><br /> O Bing utiliza também este cabeçalho para ajudar a melhorar as classificações de resultado ao analisar a atividade gerada por um ID de cliente. Ajudam os aprimoramentos de relevância de melhor qualidade dos resultados fornecidos pelas APIs do Bing e, em seguida permite maior taxas de cliques para o consumidor de API.<br /><br /> **Importante:** embora seja opcional, deve considerar este cabeçalho obrigado. Manter o ID de cliente em vários pedidos para o mesmo utilizador final e a combinação de dispositivo permite que 1) o consumidor de API receber uma experiência de usuário consistente e 2) mais elevadas taxas de cliques por meio de melhor qualidade dos resultados das APIs do Bing.<br /><br /> Seguem-se as regras de utilização básica que se aplicam a este cabeçalho.<br /><ul><li>Cada utilizador que utiliza a sua aplicação no dispositivo tem de ter um exclusivo, Bing gerados pelo ID de cliente.<br /><br/>Se não incluir este cabeçalho no pedido, o Bing gera um ID e devolve a mesma no cabeçalho de resposta de X-MSEdge-ID de cliente. O único momento em que não deve incluir esse cabeçalho num pedido é a primeira vez que o utilizador utiliza a sua aplicação nesse dispositivo.<br /><br/></li><li>Utilize o ID de cliente para cada pedido de API do Bing que torna a sua aplicação para este utilizador no dispositivo.<br /><br/></li><li>**Atenção:** tem de garantir que este ID de cliente não é vinculação para quaisquer informações de conta de utilizador authenticatable.</li><br/><li>Manter o ID de cliente. Para manter o ID de uma aplicação de browser, utilize um cookie HTTP persistente para garantir que o ID é utilizado em todas as sessões. Não utilize um cookie de sessão. Para outras aplicações, como as aplicações móveis, utilize o armazenamento persistente do dispositivo para manter o ID.<br /><br/>Da próxima vez que o utilizador utiliza a sua aplicação nesse dispositivo, obtenha o ID de cliente que manteve.</li></ul><br /> **Nota:** respostas Bing pode ou não pode incluir esse cabeçalho. Se a resposta inclui esse cabeçalho, capture o ID de cliente e usá-la para todos os pedidos subsequentes do Bing para o utilizador nesse dispositivo.<br /><br /> **Nota:** se incluir o ClientID de MSEdge X, não pode incluir os cookies no pedido.|  
|<a name="clientip" />X-MSEdge-ClientIP|Cabeçalho do pedido opcional.<br /><br /> O endereço IPv4 ou IPv6 do dispositivo cliente. O endereço IP é utilizado para detetar a localização do utilizador. O Bing utiliza as informações de localização para determinar o comportamento de pesquisa segura.<br /><br /> **Nota:** embora seja opcional, é encorajado a sempre especificar esse cabeçalho e o cabeçalho X-pesquisa-Location.<br /><br /> Não é ofuscar o endereço (por exemplo, alterando o último octeto para 0). Ofuscar os resultados de endereço no local não estão sendo em qualquer lugar junto à localização do dispositivo real, que poderá resultar no Bing que serve os resultados de errado.|  
|<a name="location" />Localização de pesquisa de X|Cabeçalho do pedido opcional.<br /><br /> Uma lista delimitada por ponto e vírgula de pares de chave/valor que descrevem a localização geográfica do cliente. O Bing utiliza as informações de localização para determinar o comportamento de pesquisa segura e retornar o conteúdo local relevante. Especificar o par chave/valor como \<chave\>:\<valor\>. Seguem-se as chaves que utilizar para especificar a localização do utilizador.<br /><br /><ul><li>LAT&mdash;a latitude da localização do cliente, em graus. A latitude tem de ser maior ou igual a -90,0 e menor ou igual a + 90,0. Valores negativos indicam as latitudes Austrais e valores positivos indicam as latitudes Norte.<br /><br /></li><li>longa&mdash;a longitude da localização do cliente, em graus. A longitude tem de ser maior ou igual a -180,0 e menor ou igual a + 180.0. Valores negativos indicam as longitudes ocidentais e leste longitudes de indicar valores positivos.<br /><br /></li><li>Re&mdash; raio, em metros, que especifica a precisão horizontal das coordenadas. Passe o valor devolvido pelo serviço de localização do dispositivo. Valores típicos podem estar 22m para GPS/Wi-Fi, os 380m para triangulação de Torre de célula e 18,000m para pesquisa IP inversa.<br /><br /></li><li>TS&mdash; timestamp o UNIX de UTC de quando o cliente estava no local. (O carimbo de hora UNIX é o número de segundos desde 1 de Janeiro de 1970.)<br /><br /></li><li>HEAD&mdash;opcional. O cliente rumo relativo ou direção de viagem. Especifica a direção de transporte como graus de 0 a 360, contagem para a direita em relação ao norte verdadeiro. Especifique esta chave apenas se o `sp` chave é diferente de zero.<br /><br /></li><li>SP&mdash; a velocidade horizontal (velocidade), em metros por segundo, o que o dispositivo cliente está em uma viagem.<br /><br /></li><li>ALT&mdash; a altitude do dispositivo cliente, em metros.<br /><br /></li><li>são&mdash;opcional. O radius, em metros, que especifica a precisão vertical das coordenadas. Predefinições de RADIUS para 50 Quilômetros. Especifique esta chave apenas se especificou o `alt` chave.<br /><br /></li></ul> **Nota:** apesar destas chaves são opcionais, a obter mais informações fornecidas por si, são mais precisas dos resultados de localização.<br /><br /> **Nota:** é encorajado a sempre especificar a localização geográfica do utilizador. Fornecendo a localização é especialmente importante se o endereço IP do cliente não reflete com precisão local físico do usuário (por exemplo, se o cliente utiliza VPN). Para obter melhores resultados, deve incluir esse cabeçalho e o cabeçalho X-MSEdge-ClientIP mas, no mínimo, deve incluir esse cabeçalho.|

> [!NOTE] 
> Lembre-se de que os termos de utilização exigir a conformidade com todas as leis aplicáveis, incluindo relativamente à utilização desses cabeçalhos. Por exemplo, em determinadas jurisdições, como a Europa, existem requisitos para obter o consentimento do utilizador antes de colocar a determinados dispositivos de controle em dispositivos de utilizador.
  

## <a name="query-parameters"></a>Parâmetros de consulta  
O pedido pode incluir os seguintes parâmetros de consulta. Consulte a coluna necessária para parâmetros obrigatórios. Tem o URL de codificar os parâmetros de consulta.  
  
  
|Nome|Valor|Tipo|Necessário|  
|----------|-----------|----------|--------------|  
|<a name="mkt" />mkt|O mercado de onde vêm os resultados. <br /><br />Para obter uma lista de possíveis valores de mercado, consulte [códigos de mercado](#market-codes).<br /><br /> **Nota:** a API de pré-visualização do URL atualmente suporta apenas en--nos mercado e a linguagem.<br /><br />|Cadeia|Sim|  
|<a name="query" />p|O URL para a pré-visualização|Cadeia|Sim|  
|<a name="responseformat" />responseFormat|O tipo de mídia a utilizar para a resposta. Seguem-se os valores possíveis de maiúsculas e minúsculas.<br /><ul><li>JSON</li><li>JSONLD</li></ul><br /> A predefinição é JSON. Para obter informações sobre o JSON de objetos que contém a resposta, consulte [objetos de resposta](#response-objects).<br /><br />  Se especificar JsonLd, o corpo da resposta inclui os objetos de JSON LD que contêm os resultados da pesquisa. Para obter informações sobre o JSON-LD, consulte [JSON LD](http://json-ld.org/).|Cadeia|Não|  
|<a name="safesearch" />pesquisa segura|Um filtro utilizado para filtrar o conteúdo para adultos. Seguem-se os valores de filtro de maiúsculas e minúsculas possíveis.<br /><ul><li>Desativar&mdash;devolver páginas da Web com o texto para adultos, imagens ou vídeos.<br /><br/></li><li>Moderado&mdash;devolver páginas da Web com o texto para adultos, mas não para adultos imagens ou vídeos.<br /><br/></li><li>Strict&mdash;não retornam páginas da Web com o texto para adultos, imagens ou vídeos.</li></ul><br /> A predefinição é moderado.<br /><br /> **Nota:** se a solicitação é proveniente de um mercado de política de adultos do Bing que requer que `safeSearch` está definido para Strict, Bing ignora o `safeSearch` valor e utiliza Strict.<br/><br/>**Nota:** se utilizar o `site:` operador de consulta, é provável que a resposta pode conter o conteúdo para adultos, independentemente do que o `safeSearch` parâmetro de consulta está definido como. Utilize `site:` apenas se está atento o conteúdo do site e o seu cenário suporta a possibilidade de conteúdo para adultos. |Cadeia|Não|  
|<a name="setlang" />setLang|O idioma a utilizar para cadeias de caracteres de interface de usuário. Especifica o idioma usando o código de idioma de 2 letras 639-1 ISO. Por exemplo, o código de idioma para inglês é EN. A predefinição é EN (em inglês).<br /><br /> Embora seja opcional, deve sempre especificar o idioma. Normalmente, definido `setLang` para o mesmo idioma especificado pelo `mkt` , a menos que o usuário deseja as cadeias de caracteres de interface do usuário exibidas num idioma diferente.<br /><br /> Este parâmetro e o [Accept-Language](#acceptlanguage) cabeçalho são mutuamente exclusivas&mdash;não especificar ambos.<br /><br /> Uma cadeia de caracteres de interface do usuário é uma cadeia que é utilizada como etiqueta numa interface de utilizador. Existem algumas cadeias de caracteres de interface de utilizador nos objetos de resposta de JSON. Além disso, todas as ligações às propriedades de Bing.com nos objetos de resposta aplicam-se o idioma especificado.|Cadeia|Não| 


## <a name="response-objects"></a>Objetos de resposta  
O esquema de resposta é qualquer um de uma [página Web] ou byl vrácen Prvek, tal como a API de pesquisa na Web. Se o pedido falhar, o objeto de nível superior é o [byl vrácen Prvek](#errorresponse) objeto.


|Object|Descrição|  
|------------|-----------------|  
|Página [Web]|Principal ao nível objeto JSON que contém os atributos de pré-visualização.|  
|[Fato]|Principal ao nível objeto JSON que contém os fatos.| 
|[Entidades|Principal ao nível objeto JSON que contém os detalhes da entidade.| 

  
### <a name="error"></a>Erro  
Define o erro ocorrido.  
  
|Elemento|Descrição|Tipo|  
|-------------|-----------------|----------|  
|<a name="error-code" />Código|O código de erro que identifica a categoria de erro. Para obter uma lista de códigos possíveis, consulte [códigos de erro](#error-codes).|Cadeia|  
|<a name="error-message" />mensagem|Uma descrição do erro.|Cadeia|  
|<a name="error-moredetails" />moreDetails|Uma descrição que fornece informações adicionais sobre o erro.|Cadeia|  
|<a name="error-parameter" />parâmetro|O parâmetro de consulta no pedido que causou o erro.|Cadeia|  
|<a name="error-subcode" />subCode|O código de erro que identifica o erro. Por exemplo, se `code` é InvalidRequest, `subCode` pode ser ParameterInvalid ou ParameterInvalidValue. |Cadeia|  
|<a name="error-value" />valor|Valor do parâmetro de consulta que não era válido.|Cadeia|  
  

### <a name="errorresponse"></a>Byl vrácen Prvek  
O objeto de nível superior que a resposta inclui quando o pedido falhar.  
  
|Nome|Valor|Tipo|  
|----------|-----------|----------|  
|tipo|Dica de tipo.|Cadeia|  
|<a name="errors" />erros|Uma lista de erros que descrevem os motivos por que o pedido falhou.|[Erro](#error)]|  

  
  
### <a name="license"></a>Licença  
Define a licença sob a qual podem ser utilizada o texto ou fotos.  
  
|Nome|Valor|Tipo|  
|----------|-----------|----------|  
|nome|O nome da licença.|Cadeia|  
|url|O URL para um Web site em que o utilizador pode obter mais informações sobre a licença.<br /><br /> Utilize o nome e o URL para criar uma hiperligação.|Cadeia|  
  

### <a name="licenseattribution"></a>LicenseAttribution  
Define uma regra contratual para atribuição de licença.  
  
|Nome|Valor|Tipo|  
|----------|-----------|----------|  
|tipo|Uma dica de tipo, que é definida como LicenseAttribution.|Cadeia|  
|licença|A licença sob a qual o conteúdo pode ser utilizado.|[Licença](#license)|  
|licenseNotice|A licença para apresentar junto ao campo de destino. Por exemplo, "Text sob a licença de CC por SA".<br /><br /> Utilizar o nome e a URL na licença o `license` campo para criar uma hiperligação para o Web site que descreve os detalhes da licença. Em seguida, substitua o nome de licença no `licenseNotice` cadeia de caracteres (por exemplo, CC-por-SA) com o hiperlink que acabou de criar.|Cadeia|  
|mustBeCloseToContent|Um valor booleano que determina se o conteúdo da regra tem de ser colocado em Fechar proximidade para o campo que se aplica a regra. Se **true**, tem de ser colocado o conteúdo próximos. Se **false**, ou este campo não existe, o conteúdo pode ser colocado a critério do chamador.|Booleano|  
|targetPropertyName|O nome do campo que se aplica a regra.|Cadeia|  
  

### <a name="link"></a>Ligação  
Define os componentes de um hiperlink.  
  
|Nome|Valor|Tipo|  
|----------|-----------|----------|  
|tipo|Dica de tipo.|Cadeia|  
|texto|O texto de exibição.|Cadeia|  
|url|UM URL. Utilize o URL e apresentar o texto para criar uma hiperligação.|Cadeia|  
  

### <a name="linkattribution"></a>LinkAttribution  
Define uma regra contratual para atribuição de ligação.  
  
|Nome|Valor|Tipo|  
|----------|-----------|----------|  
|tipo|Uma dica de tipo, que é definida como LinkAttribution.|Cadeia|  
|mustBeCloseToContent|Um valor booleano que determina se o conteúdo da regra tem de ser colocado em Fechar proximidade para o campo que se aplica a regra. Se **true**, tem de ser colocado o conteúdo próximos. Se **false**, ou este campo não existe, o conteúdo pode ser colocado a critério do chamador.|Booleano|  
|targetPropertyName|O nome do campo que se aplica a regra.<br /><br /> Se não for especificado um destino, a atribuição aplica-se para a entidade como um todo e deve ser apresentada imediatamente após a apresentação de entidade. Se existirem várias regras atribuição de texto e ligação que não especifique um destino, deve utilizar para concatená-los e exibi-los usando um "dados a partir de:" etiqueta. Por exemplo, "dados a partir de < fornecedor name1\> &#124; < name2 de fornecedor\>".|Cadeia|  
|texto|O texto de atribuição.|Cadeia|  
|url|O URL para o site do fornecedor. Utilize `text` e o URL para criar de hiperligação.|Cadeia|  
  
  
### <a name="mediaattribution"></a>MediaAttribution  
Define uma regra contratual para atribuição de suporte de dados.  
  
|Nome|Valor|Tipo|  
|----------|-----------|----------|  
|tipo|Uma dica de tipo, que é definida como MediaAttribution.|Cadeia|  
|mustBeCloseToContent|Um valor booleano que determina se o conteúdo da regra tem de ser colocado em Fechar proximidade para o campo que se aplica a regra. Se **true**, tem de ser colocado o conteúdo próximos. Se **false**, ou este campo não existe, o conteúdo pode ser colocado a critério do chamador.|Booleano|  
|targetPropertyName|O nome do campo que se aplica a regra.|Cadeia|  
|url|O URL que utilizar para criar de hiperlink do conteúdo do suporte de dados. Por exemplo, se o destino for uma imagem, usaria o URL para criar a imagem clicável.|Cadeia|  
  
  
  
### <a name="organization"></a>Organização  
Define um publicador.  
  
Tenha em atenção que um fabricante poderá fornecer seu nome ou o site ou ambos.  
  
|Nome|Valor|Tipo|  
|----------|-----------|----------|  
|nome|O nome do Editor.|Cadeia|  
|url|O URL para o site do publicador.<br /><br /> Tenha em atenção que o publicador poderão não fornecer um Web site.|Cadeia|  
  
  

### <a name="webpage"></a>Página Web  
Define as informações sobre uma página da Web em pré-visualização.  
  
|Nome|Valor|Tipo|  
|----------|-----------|----------|
|nome|O título da página, não necessariamente o título do HTML|Cadeia|
|url|O URL que, na verdade, foi pesquisado (pedido pode seguiu redirecionamentos)|Cadeia|  
|descrição|Breve descrição da página e conteúdo|Cadeia|  
|isFamilyFriendly|Mais precisos para itens no índice web; buscas em tempo real fazer esta deteção com base apenas no URL e não o conteúdo da página|boolean|
|primaryImageOfPage/contentUrl|O URL para uma imagem representativo para incluir na pré-visualização|Cadeia| 
  
  
### <a name="querycontext"></a>QueryContext  
Define o contexto de consulta que Bing utilizado para o pedido.  
  
|Elemento|Descrição|Tipo|  
|-------------|-----------------|----------|  
|adultIntent|Um valor booleano que indica se a consulta especificada tem intencional adulto. O valor é **true** se a consulta tem intencional adulto; caso contrário, **falso**.|Booleano|  
|alterationOverrideQuery|A cadeia de consulta a utilizar para forçar o Bing para utilizar a cadeia de caracteres original. Por exemplo, se a cadeia de consulta é *saling downwind*, a cadeia de caracteres de consulta de substituição será *+ saling downwind*. Lembre-se codificar a cadeia de consulta que resulta em *% 2Bsaling + downwind*.<br /><br /> Este campo está incluído apenas se a cadeia de consulta original contém um erro de ortografia.|Cadeia|  
|alteredQuery|A cadeia de consulta utilizada pelo Bing para realizar a consulta. O Bing utiliza a cadeia de consulta alterada se a cadeia de consulta original contido erros de ortografia. Por exemplo, se a cadeia de consulta é `saling downwind`, a cadeia de consulta alterada será `sailing downwind`.<br /><br /> Este campo está incluído apenas se a cadeia de consulta original contém um erro de ortografia.|Cadeia|  
|askUserForLocation|Um valor booleano que indica se Bing necessita que a localização do utilizador para fornecer resultados precisos. Se tiver especificado na localização do utilizador utilizando o [MSEdge-X-ClientIP](#clientip) e [X-pesquisa-Location](#location) cabeçalhos, pode ignorar este campo.<br /><br /> Para consultas com suporte para localização, como "meteorológica de hoje" ou "restaurantes contra ao meu redor" que precisam de localização do utilizador para fornecer resultados precisos, este campo é definido como **true**.<br /><br /> Para localização com suporte para consultas que incluem a localização (por exemplo, "tempo da Seattle"), este campo é definido como **false**. Este campo também é definido como **false** para consultas que não são localização com suporte para, por exemplo, "mais vendidos".|Booleano|  
|originalQuery|A cadeia de consulta, conforme especificado no pedido.|Cadeia|  

### <a name="identifiable"></a>Identificação
|Nome|Valor|Tipo|  
|-------------|-----------------|----------|
|ID|Um identificador de recurso|Cadeia|
 
### <a name="rankinggroup"></a>RankingGroup
Define uma pesquisa resulta de grupo, tal como mainline.
|Nome|Valor|Tipo|  
|-------------|-----------------|----------|
|itens|Uma lista de resultados de pesquisa para ver o grupo.|RankingItem|

### <a name="rankingitem"></a>RankingItem
Define um item de resultado de pesquisa para apresentar.
|Nome|Valor|Tipo|  
|-------------|-----------------|----------|
|resultIndex|Um índice baseado em zero do item na resposta a apresentar. Se o item não incluir este campo, apresenta todos os itens na resposta. Por exemplo, exiba todos os artigos de notícias na resposta de notícias.|Número inteiro|
|answerType|A resposta que contém o item para apresentar. Por exemplo, notícias.<br /><br />Utilize o tipo para encontrar a resposta no objeto SearchResponse. O tipo é o nome de um campo de SearchResponse.<br /><br /> No entanto, utilizar o tipo de resposta apenas se este objeto inclui o campo de valor; caso contrário, ignorá-lo.|Cadeia|
|textualIndex|O índice da resposta sob textualAnswers para apresentar.| Número inteiro não assinado|
|valor|O ID identifica uma resposta para apresentar ou um item de uma resposta para apresentar. Se o ID identifica uma resposta, exiba todos os itens da resposta.|Identificação|

### <a name="rankingresponse"></a>RankingResponse  
Define onde na pesquisa deve ser colocado o conteúdo da página de resultados e por que ordem.  
  
|Nome|Valor|  
|----------|-----------|  
|<a name="ranking-mainline" />mainline|Os resultados da pesquisa para apresentar os principais no.|  
|<a name="ranking-pole" />Polo|Os resultados da pesquisa que devem ser proporcionados o tratamento mais visível (por exemplo, são apresentados acima o principal e barra lateral).|  
|<a name="ranking-sidebar" />barra lateral|Os resultados da pesquisa para apresentar na barra lateral.| 


### <a name="searchresponse"></a>SearchResponse  
Define o objeto de nível superior que a resposta inclui quando o pedido for concluída com êxito.  
  
Tenha em atenção que, se o serviço suspeita de um ataque de negação de serviço, o pedido terá êxito (código de estado HTTP é 200 OK); No entanto, o corpo da resposta estará vazio.  
  
|Nome|Valor|Tipo|  
|----------|-----------|----------|  
|tipo|Dica de tipo, que é definida como SearchResponse.|Cadeia|  
|Página Web|Um objeto JSON que define a pré-visualização|cadeia|  
  
  
### <a name="textattribution"></a>TextAttribution  
Define uma regra contratual para atribuição de texto sem formatação.  
  
|Nome|Valor|Tipo|  
|----------|-----------|----------|  
|tipo|Uma dica de tipo, que é definida como TextAttribution.|Cadeia|  
|texto|O texto de atribuição.<br /><br /> Atribuição de texto aplica-se para a entidade como um todo e deve ser apresentada imediatamente após a apresentação de entidade. Se existirem várias regras atribuição de texto ou de ligações que não especifique um destino, deve utilizar para concatená-los e exibi-los usando um "dados a partir de:" etiqueta.|Cadeia| 


## <a name="error-codes"></a>Códigos de erro

Seguem-se os possíveis códigos de estado HTTP que retorna um pedido.  
  
|Código de Estado|Descrição|  
|-----------------|-----------------|  
|200|Êxito.|  
|400|Um dos parâmetros de consulta está em falta ou não é válido.|  
|401|A chave de subscrição está em falta ou não é válida.|  
|403|O utilizador é autenticado (por exemplo, eles utilizaram uma chave de subscrição válido), mas que não tem permissão para o recurso solicitado.<br /><br /> Bing também pode devolver este estado se o chamador excedido suas consultas por quota do mês.|  
|410|O pedido de utilizado HTTP em vez do protocolo HTTPS. O HTTPS é o único protocolo suportado.|  
|429|O autor da chamada excedeu as consultas por segundo quota.|  
|500|Erro de servidor inesperada.|

Se o pedido falhar, a resposta contém um [byl vrácen Prvek](#errorresponse) objeto, que contém uma lista de [erro](#error) objetos que descrevem o que causou de erro. Se o erro está relacionado com um parâmetro, o `parameter` campo identifica o parâmetro que é o problema. E se o erro está relacionado com um valor de parâmetro, o `value` campo identifica o valor que não é válido.

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
|InvalidRequest|ParameterMissing<br/>ParameterInvalidValue<br/>HttpNotAllowed<br/>Bloqueado|O Bing devolve InvalidRequest sempre que qualquer parte do pedido não é válido. Por exemplo, um parâmetro necessário está em falta ou um valor de parâmetro não é válido.<br/><br/>Se o erro é ParameterMissing ou ParameterInvalidValue, o código de estado HTTP é 400.<br/><br/>Se utilizar o protocolo HTTP em vez de HTTPS, o Bing devolve HttpNotAllowed e o código de estado HTTP é 410.
|RateLimitExceeded|Sem códigos secundárias|O Bing devolve RateLimitExceeded sempre que excedem suas consultas por segundo (QPS) ou consultas por quota do mês (QPM).<br/><br/>Se ultrapassar QPS, o Bing devolve o código de estado HTTP 429 e, se exceder QPM, o Bing devolve 403.
|InvalidAuthorization|AuthorizationMissing<br/>AuthorizationRedundancy|O Bing devolve InvalidAuthorization ao Bing não é possível autenticar o chamador. Por exemplo, o `Ocp-Apim-Subscription-Key` cabeçalho está em falta ou a chave de subscrição não é válida.<br/><br/>Redundância ocorre se especificar mais do que um método de autenticação.<br/><br/>Se o erro for InvalidAuthorization, o código de estado HTTP é 401.
|InsufficientAuthorization|AuthorizationDisabled<br/>AuthorizationExpired|O Bing devolve InsufficientAuthorization quando o chamador não tem permissões para aceder ao recurso. Isto pode ocorrer se a chave de subscrição foi desativada ou expirou. <br/><br/>Se o erro for InsufficientAuthorization, o código de estado HTTP é 403.

## <a name="next-steps"></a>Passos Seguintes
- [Início rápido em C#](c-sharp-quickstart.md)
- [Início rápido de Java](java-quickstart.md)
- [Guia de introdução do nó](node-quickstart.md)
- [Guia de introdução do Python](python-quickstart.md)

