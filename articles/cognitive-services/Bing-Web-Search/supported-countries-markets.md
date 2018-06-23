---
title: Suportado países/regiões e idiomas Bing Web da API de pesquisa no Azure | Microsoft Docs
description: Descobrir que países/regiões e idiomas são suportados pela API de pesquisa do Bing Web.
services: cognitive-services
author: v-jerkin
manager: jhubbard
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 10/06/2017
ms.author: v-jerkin
ms.openlocfilehash: 7b62c4a4feb7144662a8fe4d692f11f1efe5db1b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351806"
---
# <a name="bing-web-search-countries-and-languages"></a>Países de pesquisa na Web Bing e idiomas

A API de pesquisa do Bing Web suporta mais do que três dúzia países, muitas com mais de um idioma. A especificação de um país com uma consulta serve principalmente para refinar os resultados da pesquisa com base nos interesses desse país. Além disso, os resultados podem conter ligações para o Bing, e estas ligações podem localizar a experiência de utilizador do Bing, de acordo com o país especificado ou o idioma.

Pode especificar um país, utilizando o `cc` parâmetro de consulta. Se especificar um país, também tem de especificar um ou mais códigos de idioma a utilizar o `Accept-Language` cabeçalho de HTTP. Idiomas suportados variam consoante o país; são fornecidos para cada país na tabela de mercados.

Em alternativa, pode especificar o mercado utilizando o `mkt` consultar parâmetros e um código do **mercados** tabela. Especificar um mercado em simultâneo Especifica um país e um idioma preferencial. O `setLang` parâmetro de consulta pode ser definido como um código de idioma neste caso; normalmente, este é o mesmo idioma especificado pelo `mkt` , a menos que o utilizador prefers ver o Bing noutro idioma.

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
