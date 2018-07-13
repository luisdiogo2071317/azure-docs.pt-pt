---
title: Países/regiões e idiomas suportadas para as API de pesquisa do Bing personalizado no Azure | Documentos da Microsoft
description: Descubra que países/regiões e idiomas são compatíveis com a API de pesquisa personalizada do Bing.
services: cognitive-services
author: mikedodaro
manager: ronakshah
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 10/19/2017
ms.author: v-gedod
ms.openlocfilehash: 7009991ddd0bc8fd9fc68eaab57585b752db1fc1
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39006500"
---
# <a name="bing-custom-search-countriesregions-and-languages"></a>Pesquisa personalizada do Bing países/regiões e idiomas

A API de pesquisa personalizada do Bing suporta mais de três doze países/regiões, muitos com mais de um idioma. 

Embora seja opcional, a solicitação deve especificar o [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#mkt) parâmetro de consulta, que identifica onde pretende que os resultados provenientes de mercado. Para obter uma lista de parâmetros de consulta opcionais, consulte [parâmetros de consulta](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#query-parameters)

Pode especificar um país/região a utilizar o `cc` parâmetro de consulta. Se especificar um país/região, também tem de especificar um ou mais códigos de idioma a utilizar o `Accept-Language` cabeçalho. Os idiomas com suporte variam consoante o país/região. são fornecidos para cada país/região no **mercados** tabela.

O `Accept-Language` cabeçalho e o `setLang` parâmetro de consulta são mutuamente exclusivos, não especifique ambos. Para obter detalhes, consulte [Accept-Language](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#acceptlanguage).

## <a name="countries"></a>Países

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
