---
title: Suportado países/regiões e idiomas API para Bing notícias pesquisa no Azure | Microsoft Docs
description: Descobrir que países/regiões e idiomas são suportados pela API de pesquisa do Bing imagem.
services: cognitive-services
author: MikeDodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: article
ms.date: 12/20/2017
ms.author: v-gedod
ms.openlocfilehash: 80326d66e509b64efd5d386fe793bc9942b29ae3
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351901"
---
# <a name="bing-news-search-countries-and-languages"></a>Países de pesquisa do Bing notícias e idiomas

A API de pesquisa do Bing notícias suporta vários países, muitas com mais de um idioma. A especificação de um país com uma consulta serve principalmente para refinar os resultados da pesquisa com base nos interesses desse país. Além disso, os resultados podem conter ligações para o Bing, e estas ligações podem localizar a experiência de utilizador do Bing, de acordo com o país especificado ou o idioma.

Pode especificar um país, utilizando o `cc` parâmetro de consulta. Se especificar um país, também tem de especificar um ou mais códigos de idioma a utilizar o `Accept-Language` cabeçalho de HTTP. Idiomas suportados variam consoante o país; são fornecidos para cada país na tabela de mercados.

Em alternativa, pode especificar o mercado utilizando o `mkt` consultar parâmetros e um código do **mercados** tabela. Especificar um mercado em simultâneo Especifica um país e um idioma preferencial. O `setLang` parâmetro de consulta pode ser definido como um código de idioma neste caso; normalmente, este é o mesmo idioma especificado pelo `mkt` , a menos que o utilizador prefers ver o Bing noutro idioma.

## <a name="supported-markets-for-news-search-endpoint"></a>Suportado mercados para o ponto final de pesquisa de notícias de última hora

Para o `/news/search` ponto final, a tabela seguinte lista os valores de código de mercado que pode utilizar para especificar o `mkt` parâmetro de consulta. Bing devolve o conteúdo para apenas estes mercados. A lista está sujeita a alterações.  
  
Para obter uma lista de país códigos que pode especificar no `cc` parâmetro de consulta, consulte [indicativos de país](#countrycodes).  
  
|País/região|Idioma|Código de colocação no mercado|  
|---------------------|--------------|-----------------| 
|Dinamarca|Dinamarquês|da-DK|
|Áustria|Alemão|AT da Alemanha| 
|Suíça|Alemão|Categoria da Alemanha|
|Alemanha|Alemão|Alemanha da Alemanha|
|Austrália|Português|EN AU|
|Canadá|Português|en-CA|
|Reino Unido|Português|en GB|
|Indonésia|Português|EN-ID|
|Irlanda|Português|EN-i/e|
|Índia|Português|EN-IN|
|Malásia|Português|Os meus en|
|Nova Zelândia|Português|EN NZ|
|República das Filipinas|Português|EN PH|
|Singapura|Português|EN-SG|
|Estados Unidos|Português|pt-PT|
|Português|Geral|EN WW|
|Português|Geral|EN XA|
|África do Sul|Português|EN-ZA|
|Argentina|Espanhol|es AR|
|Chile|Espanhol|es CL|
|Espanha|Espanhol|es-ES|
|México|Espanhol|es-MX|
|Estados Unidos|Espanhol|es-US| 
|Espanhol|Geral|es XL|
|Finlândia|Finlandês|fi-FI|  
|França|Francês|FR-ser|
|Canadá|Francês|fr-CA| 
|Bélgica|Neerlandês|NL-ser|
|Suíça|Francês|FR categoria|
|França|Francês|FR-FR|  
|Itália|Italiano|it-IT| 
|R.A.E. Hong Kong|Chinês tradicional|zh-HK|  
|Taiwan|Chinês tradicional|zh-TW|
|Japão|Japonês|ja-JP|  
|Coreia|Coreano|ko-KR|  
|Países Baixos|Neerlandês|NL-NL|  
|República Popular da China|Chinês|zh-CN|  
|Brasil|Português|pt-BR|
|Rússia|Russo|ru-RU|  
|Suécia|Sueco|SV-SE|  
|Turquia|Turco|tr-TR|  

## <a name="supported-markets-for-news-endpoint"></a>Mercados suportados para o ponto final de notícias de última hora
Para o `/news` ponto final, a tabela seguinte lista os valores de código de mercado que pode utilizar para especificar o `mkt` parâmetro de consulta. Bing devolve o conteúdo para apenas estes mercados. A lista está sujeita a alterações.  
  
Para obter uma lista de país códigos que pode especificar no `cc` parâmetro de consulta, consulte [indicativos de país](#countrycodes).  
  
|País/região|Idioma|Código de colocação no mercado|  
|---------------------|--------------|-----------------| 
|Dinamarca|Dinamarquês|da-DK|
|Alemanha|Alemão|Alemanha da Alemanha|
|Austrália|Português|EN AU|
|Reino Unido|Português|en GB|
|Estados Unidos|Português|pt-PT|
|Português|Geral|EN WW|
|Chile|Espanhol|es CL|
|México|Espanhol|es-MX|
|Estados Unidos|Espanhol|es-US| 
|Finlândia|Finlandês|fi-FI|  
|Canadá|Francês|fr-CA|
|França|Francês|FR-FR|  
|Itália|Italiano|it-IT| 
|Brasil|Português|pt-BR|
|República Popular da China|Chinês|zh-CN|

## <a name="supported-markets-for-news-trending-endpoint"></a>Suportado mercados para o ponto final de tendências de notícias de última hora
Para o `/news/trendingtopics` ponto final, a tabela seguinte lista os valores de código de mercado que pode utilizar para especificar o `mkt` parâmetro de consulta. Bing devolve o conteúdo para apenas estes mercados. A lista está sujeita a alterações.  
  
Para obter uma lista de país códigos que pode especificar no `cc` parâmetro de consulta, consulte [indicativos de país](#countrycodes).  
  
|País/região|Idioma|Código de colocação no mercado|  
|---------------------|--------------|-----------------| 
|Alemanha|Alemão|Alemanha da Alemanha|
|Austrália|Português|EN AU|
|Reino Unido|Português|en GB|
|Estados Unidos|Português|pt-PT|
|Canadá|Português|en-CA|
|Índia|Português|EN-IN|
|França|Francês|FR-FR|
|Canadá|Francês|fr-CA|
|Brasil|Português|pt-BR|
|República Popular da China|Chinês|zh-CN|


<a name="countrycodes"></a>   
### <a name="country-codes"></a>Códigos de país  

Seguem-se a indicativos de país que especificou no `cc` parâmetro de consulta. A lista está sujeita a alterações.  
  
|País/região|Indicativo do país|  
|---------------------|------------------|  
|Argentina|AR|  
|Austrália|AU|  
|Áustria|AT|  
|Bélgica|SER|  
|Brasil|BR|  
|Canadá|AC|  
|Chile|CL|  
|Dinamarca|DK|  
|Finlândia|FI|  
|França|FR|  
|Alemanha|ALEMANHA|  
|R.A.E. Hong Kong|HK|  
|Índia|EM|  
|Indonésia|ID|  
|Itália|TI|  
|Japão|JP|  
|Coreia|KR|  
|Malásia|A MINHA|  
|México|MX|  
|Países Baixos|NL|  
|Nova Zelândia|NZ|  
|Noruega|NÃO|  
|República Popular da China|CN|  
|Polónia|LP|  
|Portugal|PT|  
|República das Filipinas|PH|  
|Rússia|RU|  
|Arábia Saudita|SA|  
|África do Sul|ZA|  
|Espanha|ES|  
|Suécia|SE|  
|Suíça|CATEGORIA|  
|Taiwan|TW|  
|Turquia|TR|  
|Reino Unido|GB|  
|Estados Unidos|EUA|

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações sobre os pontos finais de pesquisa do Bing notícias de última hora, consulte [a referência da API de pesquisa de notícias v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference).