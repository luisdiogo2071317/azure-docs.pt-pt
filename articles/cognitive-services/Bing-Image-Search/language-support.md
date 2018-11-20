---
title: Suporte de idiomas - API de pesquisa de imagens do Bing
titleSuffix: Azure Cognitive Services
description: Descubra que países/regiões e idiomas são compatíveis com a API de pesquisa de imagens do Bing.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 10/06/2017
ms.author: aahi
ms.openlocfilehash: e5c9a4291501c657a94509aec2edd90d00ab795d
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/20/2018
ms.locfileid: "52160432"
---
# <a name="language-and-region-support-for-the-bing-image-search-api"></a>Suporte de idioma e região para a API de pesquisa de imagens do Bing

A API de pesquisa de imagens do Bing suporta mais de três doze países/regiões, muitos com mais de um idioma. Especificação de um país/região com uma consulta serve principalmente para refinar os resultados da pesquisa com base nos interesses desse país/região. Além disso, os resultados podem conter hiperligações para do Bing e estas ligações podem localizar a experiência do usuário do Bing, de acordo com o país/regiões especificados ou o idioma.

Para especificar o país/região e idioma, defina o `mkt` parâmetro de consulta (mercado) para um código a partir de **mercados** tabela abaixo. O mercado Especifica um país/região e idioma. Se o utilizador prefere ver exibir texto num idioma diferente, defina `setLang` parâmetro para o código de idioma apropriado de consulta.

Em alternativa, pode especificar o país/região a utilizar o `cc` parâmetro de consulta. Se especificar um país/região, também tem de especificar um ou mais códigos de idioma a utilizar o `Accept-Language` cabeçalho de HTTP. Os idiomas com suporte variam consoante o país/região. são fornecidos para cada país/região na tabela de mercados.

> [!NOTE]
> A API de imagens populares atualmente suporta apenas os mercados seguintes:
> - EN-US (inglês, Estados Unidos)
> - en-CA (inglês, Canadá)
> - EN-AU (inglês, Austrália)
> - zh-CN (chinês, China)

## <a name="countries"></a>Países

|País/região|Código|
|-------|----|
|Argentina|AR|
|Austrália|AU|
|Áustria|AT|
|Bélgica|BE|
|Brasil|BR|
|Canadá|CA|
|Chile|CL|
|Dinamarca|DK|
|Finlândia|FI|
|França|FR|
|Alemanha|DE|
|RAE de Hong Kong|HK|
|Índia|IN|
|Indonésia|ID|
|Itália|TI|
|Japão|JP|
|Coreia|KR|
|Malásia|MY|
|México|MX|
|Países Baixos|NL|
|Nova Zelândia|NZ|
|Noruega|NO|
|China|CN|
|Polónia|PL|
|Portugal|PT|
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

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações sobre os pontos de extremidade de pesquisa do Bing notícias, consulte [referência da API de pesquisa de imagens de notícias v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference).
