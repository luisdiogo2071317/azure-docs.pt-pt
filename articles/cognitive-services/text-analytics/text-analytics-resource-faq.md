---
title: Perguntas mais frequentes sobre a análise de texto API - serviços cognitivos do Azure | Microsoft Docs
description: Obtenha respostas a questões recorrentes sobre API de análise de texto do Microsoft cognitivos serviços no Azure.
services: cognitive-services
author: HeidiSteen
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: conceptual
ms.date: 3/07/2018
ms.author: heidist
ms.openlocfilehash: bf82899b4317f0f5ce0f6ca5096dccef7cddd931
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355172"
---
# <a name="frequently-asked-questions-faq-about-the-text-analytics-api"></a>Perguntas mais frequentes (FAQ) sobre a API de análise de texto

 Encontrar respostas a perguntas mais comuns sobre conceitos, código e cenários relacionados com a API de análise de texto cognitivos nos serviços da Microsoft no Azure.

## <a name="can-text-analytics-identify-sarcasm"></a>Análise de texto pode identificar a sarcasm?

Analysis Services é para dados de sentimento do positivos negativo em vez de deteção de mood.

É sempre algum grau de imprecision na análise de dados de sentimento, mas o modelo é mais útil quando não existe nenhum significado oculto ou subtext ao conteúdo. Irony, sarcasm, humor e conteúdo da mesma forma nuanced dependem de contexto cultural e norms a transmitir intenção. Este tipo de conteúdo está entre o mais difícil de analisar. Normalmente, a maior discrepância entre uma pontuação determinada produzida pelo analisador e uma avaliação subjective por um humanos é para conteúdo com significado nuanced.

## <a name="can-i-add-my-own-training-data-or-models"></a>Posso adicionar o meu próprio dados de preparação ou modelos?

Não, os modelos são pretrained. As operações de apenas disponíveis nos dados carregados estão classificação, extração de expressão de chave e deteção de idioma. Alojamos não modelos personalizados. Se pretender criar e alojar modelos de personalizada de machine learning, considere o [do machine learning capacidades no Microsoft R Server](https://docs.microsoft.com/r-server/r/concept-what-is-the-microsoftml-package).

## <a name="can-i-request-additional-languages"></a>Pode pedir idiomas adicionais?

Análise de dados de sentimento e extração frase chave estão disponíveis para um [selecione o número de idiomas](text-analytics-supported-languages.md). Processamento de linguagem natural é complexo e exige substanciais testes antes da nova funcionalidade pode ser libertada. Por este motivo, iremos evitar previamente anunciar suporte para que ninguém demora uma dependência sobre a funcionalidade que necessita de mais tempo para madura. 

Para ajudar-na priorizar os idiomas para funcionar no seguinte, votar em idiomas específicos em [voz do utilizador](https://cognitive.uservoice.com/forums/555922-text-analytics). 

## <a name="why-does-key-phrase-extraction-return-some-words-but-not-others"></a>Por que motivo extração chave frase devolver algumas palavras mas não a outros utilizadores?

Extração de expressão de chave elimina não essencial palavras e adjectives autónomo. Adjective substantivo combinações, tais como "vistas spectacular" ou "Meteorologia foggy" são devolvidas em conjunto.

Geralmente, saída consiste substantivos e objetos do frase. Saída é listada por ordem de importância, com o frase primeiro a ser mais importantes. Importância é medida pelo número de vezes que um determinado conceito é mencionado ou a relação de que o elemento outros elementos de texto.

## <a name="why-does-output-vary-given-identical-inputs"></a>Por que motivo saída variar, idênticas entradas?

Melhoramentos para modelos e algoritmos sejam anunciados se a alteração é principais ou silenciosamente slipstreamed no serviço, se a atualização for secundária. Ao longo do tempo, poderá achar que os mesmos resultados de entrada de texto numa classificação de dados de sentimento diferente ou saída de expressão de chave. Este é um consequence normal e intencional da utilização de recursos de aprendizagem máquina gerido na nuvem.

## <a name="next-steps"></a>Passos Seguintes

É a sua pergunta sobre uma funcionalidade ou funcionalidade em falta? Considere solicitar ou votá-lo nosso [UserVoice web site](https://cognitive.uservoice.com/forums/555922-text-analytics).

## <a name="see-also"></a>Consulte também

 [StackOverflow: Análise de texto API](https://stackoverflow.com/questions/tagged/text-analytics-api)   
 [StackOverflow: Serviços cognitivos](http://stackoverflow.com/questions/tagged/microsoft-cognitive)
