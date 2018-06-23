---
title: Suportado países/regiões e idiomas API para Bing Visual pesquisa | Microsoft Docs
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Descobrir que países/regiões e idiomas são suportados pela API Visual de pesquisa do Bing.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 04/10/2018
ms.author: scottwhi
ms.openlocfilehash: 4723d028cc22caf8be3eb294b52506ec112cbab5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354140"
---
# <a name="bing-visual-search-countries-and-languages"></a>Países de pesquisa de Visual Bing e idiomas

API de pesquisa do Bing Visual suporta mais do que três dúzia países, muitas com mais de um idioma. Cada pedido deve incluir país e idioma à escolha do utilizador. Saber mercado do utilizador ajuda-o Bing devolver resultados adequados. Se não especificar uma linguagem e país, o Bing faz um melhor esforço para determinar o país e de idioma do utilizador. Porque os resultados podem conter ligações para o Bing, saber o país e o idioma pode fornecer um Bing localizada preferencial experiência de utilizador se o utilizador clica as ligações do Bing.

Para especificar o país e de idioma, defina o `mkt` parâmetro de consulta (marcados) para um código a partir de **mercados** tabela abaixo. Mercado Especifica um país e o idioma. Se o utilizador prefers ver apresentar texto num idioma diferente, defina `setLang` consultar parâmetro para o código de idioma apropriado.

Em alternativa, pode especificar o país utilizando o `cc` parâmetro de consulta. Se especificar um país, também tem de especificar um ou mais códigos de idioma a utilizar o `Accept-Language` cabeçalho de HTTP. Idiomas suportados variam consoante o país; são fornecidos para cada país na tabela de mercados.



> [!NOTE]
> As seguintes restrições de mercado aplicam-se:
> 
> - Anotações de reconhecimento de imagem só estão disponíveis em inglês. 
> - Receitas, compras e páginas incluindo insights estão disponíveis no mercado en-US apenas. 


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
