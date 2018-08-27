---
title: Países, regiões e idiomas suportados pela API de pesquisa Web do Bing
description: Descubra que países/regiões e idiomas são compatíveis com a API de pesquisa Web Bing.
services: cognitive-services
author: v-jerkin
manager: jhubbard
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 8/16/2018
ms.author: erhopf
ms.openlocfilehash: 87384a8aeabfe0c4a178344235debcc586cdf3de
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/24/2018
ms.locfileid: "42888476"
---
# <a name="countries-regions-and-languages-supported-by-the-bing-web-search-api"></a>Países, regiões e idiomas suportados pela API de pesquisa Web do Bing

A API de pesquisa Web Bing suporta mais de três doze países ou regiões, muitos com mais de um idioma. Especificação de um país ou região com uma consulta ajuda a refinar os resultados da pesquisa com base nos interesses desse país ou regiões. Os resultados podem incluir ligações para o Bing e estas ligações podem localizar a experiência do usuário do Bing, de acordo com o idioma ou país/região especificado.

Pode especificar um país ou região a utilizar o `cc` parâmetro de consulta. Quando é especificado um país ou região, tem de especificar um ou mais códigos de idioma com o [ `Accept-Language` cabeçalho](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#headers). Utilize o [tabela de mercados](#Markets) para obter uma lista de linguagens suportadas em cada mercado.

Em alternativa, pode especificar o mercado com o `mkt` parâmetro de consulta e um código a partir de **mercados** tabela. Especificar um mercado em simultâneo Especifica um país ou região e um idioma preferencial. Pode definir explicitamente a linguagem com o `setLang` parâmetro de consulta.

## <a name="countriesregions"></a>Países/regiões

|País/região|Código|
|-------|----|
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
|RAE de Hong Kong|HK|
|Índia|ÍNDIA|
|Indonésia|ID|
|Itália|TI|
|Japão|JP|
|Coreia|COREIA|
|Malásia|MEU|
|México|MX|
|Países Baixos|NL|
|Nova Zelândia|NZ|
|Noruega|NÃO|
|China|CN|
|Polónia|PL|
|Portugal|HORA DO PACÍFICO|
|Filipinas|PH|
|Rússia|RU|
|Arábia Saudita|SA|
|África do Sul|ZA|
|Espanha|ES|
|Suécia|SE|
|Suíça|CH|
|Taiwan|TW|
|Turquia|TR|
|Reino Unido|GB|
|Estados Unidos|EUA|

## <a name="markets"></a>Mercados

|País/região|Idioma|Código do mercado|
|-------|--------|-----------|
|Argentina|Espanhol|es-AR|
|Austrália|Português|EN-AU|
|Áustria|Alemão|Alemanha-AT|
|Bélgica|Neerlandês|NL-ser|
|Bélgica|Francês|FR-ser|
|Brasil|Português|pt-BR|
|Canadá|Português|en-CA|
|Canadá|Francês|fr-CA|
|Chile|Espanhol|es-CL|
|Dinamarca|Dinamarquês|da-DK|
|Finlândia|Finlandês|fi-FI|
|França|Francês|FR-FR|
|Alemanha|Alemão|de-DE|
|RAE de Hong Kong|Chinês tradicional|zh-HK|
|Índia|Português|EN-IN|
|Indonésia|Português|ID de en|
|Itália|Italiano|it-IT|
|Japão|Japonês|ja-JP|
|Coreia|Coreano|ko-KR|
|Malásia|Português|EN MY|
|México|Espanhol|es-MX|
|Países Baixos|Neerlandês|NL-NL|
|Nova Zelândia|Português|EN-NZ|
|Noruega|Norueguês|no-NO|
|China|Chinês|zh-CN|
|Polónia|Polaco|pl-PL|
|Portugal|Português|pt-PT|
|Filipinas|Português|EN-PH|
|Rússia|Russo|ru-RU|
|Arábia Saudita|Árabe|ar SA|
|África do Sul|Português|EN ZA|
|Espanha|Espanhol|es-ES|
|Suécia|Sueco|SV-SE|
|Suíça|Francês|FR-CH|
|Suíça|Alemão|Alemanha-CH|
|Taiwan|Chinês tradicional|zh-TW|
|Turquia|Turco|tr-TR|
|Reino Unido|Português|en-GB|
|Estados Unidos|Português|pt-PT|
|Estados Unidos|Espanhol|es-US|
