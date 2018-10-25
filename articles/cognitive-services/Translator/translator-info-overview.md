---
title: O que é a API de Texto do Microsoft Translator?
titlesuffix: Azure Cognitive Services
description: Integre a API de Texto do Microsoft Translator nas suas aplicações, sites, ferramentas e outras soluções para oferecer experiências de utilizador de vários idiomas.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: overview
ms.date: 05/10/2018
ms.author: erhopf
ms.openlocfilehash: 6c89ff41531b130843eb288b98ffe7def1d8915e
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/23/2018
ms.locfileid: "49645060"
---
# <a name="what-is-translator-text-api"></a>O que é a API de Texto do Microsoft Translator?

A API de Texto do Microsoft Translator pode ser integrada diretamente nas suas aplicações, sites, ferramentas ou outras soluções para oferecer experiências de utilizador em [mais de 60 idiomas](languages.md). Ela pode ser utilizada em qualquer plataforma de hardware e com qualquer sistema operativo para fazer tradução de idiomas de texto para texto.

A API de Texto do Microsoft Translator faz parte da coleção de [APIs dos Serviços Cognitivos](https://docs.microsoft.com/azure/#pivot=products&panel=ai) do Azure de aprendizagem automática e algoritmos de IA na cloud, prontamente consumível nos seus projetos de desenvolvimento.

## <a name="about-microsoft-translator"></a>Sobre o Microsoft Translator

O Microsoft Translator é um serviço de tradução automática com base na cloud. No núcleo deste serviço estão a API de Texto do Translator e a [API de Voz do Translator](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-translation) que potencializam vários produtos e serviços da Microsoft e são utilizados por milhares de empresas em todo o mundo nas suas aplicações e fluxos de trabalho, permitindo que o conteúdo alcance o público no mundo inteiro.

A tradução de voz também está disponível através da [pré-visualização de Voz dos Serviços Cognitivos](https://docs.microsoft.com/azure/cognitive-services/speech-service/), que combina a API de Voz do Translator existente, a API de Voz do Bing e o Serviço de Voz Personalizada (pré-visualização) num serviço unificado e totalmente personalizável.  

Saiba mais sobre o [serviço do Microsoft Translator](https://www.microsoft.com/en-us/translator/home.aspx)

## <a name="language-customization"></a>Personalização de idioma

Pode ser utilizada uma extensão do principal serviço do Microsoft Translator, o Tradutor Personalizado, em conjunto com a API de Texto do Translator para ajudar a personalizar o sistema de tradução neural e melhorar a tradução para a sua terminologia e estilo específicos.

Com o Tradutor Personalizado, pode criar sistemas de tradução que suportam a terminologia utilizada na sua própria empresa ou setor. O sistema de tradução personalizada será, em seguida, integrado facilmente nas suas aplicações existentes, fluxos de trabalho e sites nos vários tipos de dispositivos, através da API Texto do Microsoft Translator normal, com o parâmetro de categoria.

Saiba mais sobre a [personalização de idiomas](customization.md)

## <a name="microsoft-translator-neural-machine-translation"></a>Tradução Automática Neural do Microsoft Translator

A Tradução Automática Neural (NMT) é o novo padrão para traduções automáticas com tecnologia de IA de alta qualidade. Ela substitui a tecnologia de Tradução Automática de Estatísticas (SMT) legada que atingiu um patamar de qualidade em meados de 2010.

A NMT oferece melhores traduções que a SMT, não apenas do ponto de vista da qualidade de tradução em bruto, mas também porque soam mais fluentes e humanas. O motivo principal desta fluidez deve-se à NMT utilizar o contexto completo de uma frase para traduzir palavras. A SMT utilizou apenas o contexto imediato de algumas palavras antes e depois de cada palavra.

Os modelos da NMT são o núcleo da API e não estão visíveis para os utilizadores finais. A única diferença notável é a melhoria da qualidade da tradução, especialmente em idiomas como chinês, japonês e árabe.

Saiba mais sobre [como funciona a NMT](https://www.microsoft.com/en-us/translator/mt.aspx#nnt)

## <a name="next-steps"></a>Passos seguintes

- Leia sobre os [detalhes dos preços](https://azure.microsoft.com/pricing/details/cognitive-services/translator-text-api/).

- [Inscrever-se](translator-text-how-to-signup.md) para obter uma chave de acesso.

- O [Início Rápido](quickstarts/csharp.md) é uma descrição das chamadas à API REST escritas em C#. Aprenda a traduzir texto de um idioma para outro com pouco código.

- A [documentação de referência de API](https://docs.microsoft.com/azure/cognitive-services/Translator/reference/v3-0-reference) oferece a documentação técnica para as APIs.

## <a name="see-also"></a>Consulte também

- [Página de Documentação dos Serviços Cognitivos](https://docs.microsoft.com/azure/#pivot=products&panel=ai)
- [Página de Produto dos Serviços Cognitivos](https://azure.microsoft.com/services/cognitive-services/)
- [Informações sobre soluções e preços](https://www.microsoft.com/en-us/translator/default.aspx)
