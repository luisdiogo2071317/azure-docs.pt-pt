---
title: Suportado países/regiões e idiomas API para Bing imagem pesquisa no Azure | Microsoft Docs
description: Descobrir que países/regiões e idiomas são suportados pela API de pesquisa do Bing imagem.
services: cognitive-services
author: v-jerkin
manager: jhubbard
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 10/06/2017
ms.author: v-jerkin
ms.openlocfilehash: d0d33ee714ba5cd1ce4e846b96c04f755933bee1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354500"
---
# <a name="bing-image-search-countries-and-languages"></a>Países de pesquisa do Bing imagem e idiomas

A API de pesquisa do Bing imagem suporta mais do que três dúzia países, muitas com mais de um idioma. A especificação de um país com uma consulta serve principalmente para refinar os resultados da pesquisa com base nos interesses desse país. Além disso, os resultados podem conter ligações para o Bing, e estas ligações podem localizar a experiência de utilizador do Bing, de acordo com o país especificado ou o idioma.

Para especificar o país e de idioma, defina o `mkt` parâmetro de consulta (marcados) para um código a partir de **mercados** tabela abaixo. Mercado Especifica um país e o idioma. Se o utilizador prefers ver apresentar texto num idioma diferente, defina `setLang` consultar parâmetro para o código de idioma apropriado.

Em alternativa, pode especificar o país utilizando o `cc` parâmetro de consulta. Se especificar um país, também tem de especificar um ou mais códigos de idioma a utilizar o `Accept-Language` cabeçalho de HTTP. Idiomas suportados variam consoante o país; são fornecidos para cada país na tabela de mercados.

> [!NOTE]
> A API de imagens tendências atualmente suporta apenas os mercados seguintes:
> - EN-US (inglês, Estados Unidos) 
> - en-CA (inglês, Canadá) 
> - EN-AU (inglês, Austrália) 
> - zh-CN (chinês, China)

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

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações sobre os pontos finais de pesquisa do Bing notícias de última hora, consulte [a referência da API de pesquisa de imagem de notícias v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference).
