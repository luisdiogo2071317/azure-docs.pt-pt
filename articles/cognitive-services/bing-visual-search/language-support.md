---
title: Suporte de idiomas - API de pesquisa Visual do Bing
titleSuffix: Azure Cognitive Services
description: Uma lista de idiomas, países e regiões suportados pela API de pesquisa Visual do Bing. A API de pesquisa Visual do Bing suporta mais de três doze países/regiões, muitos com mais de um idioma.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: article
ms.date: 09/25/2018
ms.author: scottwhi
ms.openlocfilehash: 1dd434bbf71e482f5d0a52b3c84e46382079b43a
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/09/2018
ms.locfileid: "48886404"
---
# <a name="language-and-region-support-for-the-bing-visual-search-api"></a>Suporte de idioma e região para a API de pesquisa Visual do Bing

API de pesquisa Visual do Bing suporta mais de três doze países/regiões, muitos com mais de um idioma. Cada pedido deve incluir país/região e idioma à escolha do utilizador. Saber o mercado do usuário ajuda o Bing devolver resultados apropriados. Se não especificar um país/região e idioma, o Bing faz um melhor esforço para determinar o país/região e idioma do utilizador. Uma vez que os resultados podem conter hiperligações para o Bing, saber o país/região e idioma pode fornecer um Bing localizada preferencial experiência do usuário se o usuário clicar em links do Bing.

Para especificar o país/região e idioma, defina o `mkt` parâmetro de consulta (mercado) para um código a partir de **mercados** tabela abaixo. O mercado Especifica um país/região e idioma. Se o utilizador prefere ver exibir texto num idioma diferente, defina `setLang` parâmetro para o código de idioma apropriado de consulta.

Em alternativa, pode especificar o país/região a utilizar o `cc` parâmetro de consulta. Se especificar um país/região, também tem de especificar um ou mais códigos de idioma a utilizar o `Accept-Language` cabeçalho de HTTP. Os idiomas com suporte variam consoante o país/região. são fornecidos para cada país na tabela de mercados.



> [!NOTE]
> As seguintes restrições de mercado aplicam-se:
>
> - Anotações de reconhecimento de imagem só estão disponíveis em inglês.
> - Receita, compras e páginas, incluindo informações estão disponíveis no mercado en-US apenas.


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
