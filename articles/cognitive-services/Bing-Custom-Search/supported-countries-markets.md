---
title: Suportado países/regiões e idiomas API para Bing personalizada pesquisa no Azure | Microsoft Docs
description: Descobrir que países/regiões e idiomas são suportados pela API de pesquisa do Bing personalizada.
services: cognitive-services
author: mikedodaro
manager: ronakshah
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 10/19/2017
ms.author: v-gedod
ms.openlocfilehash: 7ff309f9b789662c4ebd791dffaa2bc2e440763e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352891"
---
# <a name="bing-custom-search-countries-and-languages"></a>Países de pesquisa do Bing personalizada e idiomas

A API de pesquisa do Bing personalizada suporta mais do que três dúzia países, muitas com mais de um idioma. 

Embora seja opcional, pedido deve especificar o [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#mkt) parâmetro de consulta, que identifica o mercado onde pretende que os resultados de. Para obter uma lista dos parâmetros de consulta opcionais, consulte [parâmetros de consulta](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#query-parameters)

Pode especificar um país, utilizando o `cc` parâmetro de consulta. Se especificar um país, também tem de especificar um ou mais códigos de idioma a utilizar o `Accept-Language` cabeçalho. Idiomas suportados variam consoante o país; são fornecidos para cada país no **mercados** tabela.

O `Accept-Language` cabeçalho e o `setLang` parâmetro da consulta são mutuamente exclusivos — não especificar ambos. Para obter mais informações, consulte [aceitar idioma](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#acceptlanguage).

## <a name="countries"></a>Países

|País|Código|
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
|China|CN|
|Polónia|LP|
|Portugal|PT|
|Filipinas|PH|
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


## <a name="markets"></a>Mercados

|País|Idioma|Código de colocação no mercado|
|-------|--------|-----------|
|Argentina|Espanhol|es AR|
|Austrália|Português|EN AU|
|Áustria|Alemão|AT da Alemanha|
|Bélgica|Neerlandês|NL-ser|
|Bélgica|Francês|FR-ser|
|Brasil|Português|pt-BR|
|Canadá|Português|en-CA|
|Canadá|Francês|fr-CA|
|Chile|Espanhol|es CL|
|Dinamarca|Dinamarquês|da-DK|
|Finlândia|Finlandês|fi-FI|
|França|Francês|FR-FR|
|Alemanha|Alemão|Alemanha da Alemanha|
|RAE de Hong Kong|Chinês tradicional|zh-HK|
|Índia|Português|EN-IN|
|Indonésia|Português|EN-ID|
|Itália|Italiano|it-IT|
|Japão|Japonês|ja-JP|
|Coreia|Coreano|ko-KR|
|Malásia|Português|Os meus en|
|México|Espanhol|es-MX|
|Países Baixos|Neerlandês|NL-NL|
|Nova Zelândia|Português|EN NZ|
|Noruega|Norueguês|no-NO|
|China|Chinês|zh-CN|
|Polónia|Polaco|pl-PL|
|Portugal|Português|pt-PT|
|Filipinas|Português|EN PH|
|Rússia|Russo|ru-RU|
|Arábia Saudita|Árabe|ar SA|
|África do Sul|Português|EN-ZA|
|Espanha|Espanhol|es-ES|
|Suécia|Sueco|SV-SE|
|Suíça|Francês|FR categoria|
|Suíça|Alemão|Categoria da Alemanha|
|Taiwan|Chinês tradicional|zh-TW|
|Turquia|Turco|tr-TR|
|Reino Unido|Português|en GB|
|Estados Unidos|Português|pt-PT|
|Estados Unidos|Espanhol|es-US|
