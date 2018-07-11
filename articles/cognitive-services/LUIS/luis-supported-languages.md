---
title: Suporta a localização com LUIS aplicações no Azure | Microsoft Docs
description: Saiba mais sobre os idiomas que LUIS suporta.
services: cognitive-services
author: cahann
manager: hsalama
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/04/2017
ms.author: cahann
ms.openlocfilehash: 1eabc01ee07f8791680738a156471e3efe2c44ff
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "35356112"
---
# <a name="culture-specific-understanding-in-luis-apps"></a>Compreender específicas do idioma LUIS aplicações

Uma aplicação LUIS é cultura específica e não pode ser alterada depois de estar definida. 

## <a name="multi-language-luis-apps"></a>Multilingues LUIS aplicações
Se precisar de uma aplicação de cliente de LUIS multilingues, tais como um chatbot, tem algumas opções. Se LUIS suporta todos os idiomas, desenvolver uma aplicação LUIS para cada idioma. Cada aplicação LUIS tem um ID exclusivo da aplicação e o registo do ponto final. Se tiver de fornecer o idioma compreender para um idioma LUIS não suporta, pode utilizar [Microsoft tradutor API](../Translator/translator-info-overview.md) traduzir utterance para um idioma suportado, submeter utterance para o ponto final LUIS e receber o pontuações resultantes.

## <a name="languages-supported"></a>Idiomas suportados
LUIS compreende utterances nos seguintes idiomas:


| Idioma |Região  |  Domínio prebuilt | Entidade prebuilt | Sugestões de expressão | **[Análise de texto](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages) | 
|--|--|:--:|:--:|:--:|:--:|
| Inglês americano |`en-US` | ✔ | ✔  |✔|✔|
| Francês Canadá |`fr-CA` |-|   -   |-|✔|
| *[Chinês](#chinese-support-notes) |`zh-CN` | ✔ | ✔ |✔|-|
| Neerlandês |`nl-NL` |-|  -   |-|✔|
| Francês (França) |`fr-FR` |-| ✔ |✔ |✔|
| Alemão |`de-DE` |-| ✔ |✔ |✔|
| Italiano |`it-IT` |-| ✔ |✔|✔|
| *[Japonês](#japanese-support-notes) |`ja-JP` |-| ✔ |✔|Apenas o frase chave|
| Coreano |`ko-KR` |-|   -   |-|Apenas o frase chave|
| Português (Brasil) |`pt-BR` |-| ✔ |✔ |nem todas as culturas secundárias|
| Espanhol (Espanha) |`es-ES` |-| ✔ |✔|✔|
| Espanhol (México)|`es-MX` |-|  -   |✔|✔|


Varia consoante o suporte de idiomas [entidades prebuilt](luis-reference-prebuilt-entities.md) e [domínios prebuilt](luis-reference-prebuilt-domains.md). 

### <a name="chinese-support-notes"></a>* Notas de suporte chinês

 - No `zh-cn` cultura, LUIS espera o caráter de chinês simplificado, em vez do conjunto de carateres tradicional.
 - Os nomes das expressões regulares, funcionalidades, as entidades e pendentes poderão estar em chinês ou Roman carateres.
 - Consulte o [referência de domínios prebuilt ](luis-reference-prebuilt-domains.md) para obter informações sobre que prebuilt domínios são suportados no `zh-cn` cultura.
<!--- When writing regular expressions in Chinese, do not insert whitespace between Chinese characters.-->

### <a name="japanese-support-notes"></a>* Notas de suporte japonês

 - Uma vez LUIS não fornece diferenças sintáticas Analysis Services e não irá compreender a diferença entre Keigo e japonês informal, tem de incorporar os diferentes níveis de formality como exemplos de formação para as suas aplicações. 
     - でございます não é o mesmo que です. 
     - です não é o mesmo que だ. 

### <a name="text-analytics-support-notes"></a>* * Notas de suportar a análise de texto
Português só é suportada para subcultures: `pt-PT` e `pt-BR`. Todos os outras culturas são suportadas ao nível do idioma principal. Saiba mais sobre a análise de texto [idiomas suportados](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages). 

### <a name="speech-api-supported-languages"></a>Idiomas suportado de API de reconhecimento de voz
Consulte o reconhecimento de voz [idiomas suportados](https://docs.microsoft.com/azure/cognitive-services/Speech/api-reference-rest/supportedlanguages##interactive-and-dictation-mode) para idiomas de modo de ditado de reconhecimento de voz.

### <a name="bing-spell-check-supported-languages"></a>Idiomas ortográfica do Bing verifique suportados
Consulte a verificação de ortográfica do Bing [idiomas suportados](https://docs.microsoft.com/azure/cognitive-services/bing-spell-check/bing-spell-check-supported-languages) para uma lista de idiomas suportados e o estado.

## <a name="rare-or-foreign-words-in-an-application"></a>Palavras raras ou externas numa aplicação
No `en-us` cultura, LUIS aprende distinguir palavras mais inglês, incluindo slang. No `zh-cn` cultura, LUIS aprende a distinguir a maior parte dos carateres chineses. Se utilizar uma palavra raro no `en-us` ou caráter no `zh-cn`, e verá que LUIS parece que não é possível distinguir essa palavra ou um caráter, pode adicionar esse word ou caráter para um [funcionalidade frase-lista](luis-how-to-add-features.md). Por exemplo, palavras fora o idioma da aplicação - ou seja, externas palavras – devem ser adicionadas para uma funcionalidade da lista de expressão. Esta lista de expressão deve ser marcada não-permutáveis, para indicar que o conjunto de palavras raros uma classe que deve saber LUIS para reconhecer de formulários, mas não são automaticamente sinónimos ou permutáveis entre si.

### <a name="hybrid-languages"></a>Idiomas híbrida
Idiomas híbrida combinam palavras de dois culturas como inglês e chinês. Nestes idiomas não são suportados no LUIS porque uma aplicação é baseada numa cultura único.

## <a name="tokenization"></a>Atomização
Para efetuar a aprendizagem, LUIS interrompe um utterance [tokens](luis-glossary.md#token) com base na cultura. 

|Idioma|  cada espaço ou um caráter especial | nível de carateres|palavras compostas|[Entidade com token devolvida](luis-concept-data-extraction.md#tokenized-entity-returned)
|--|:--:|:--:|:--:|:--:|
|Chinês||✔||✔|
|Neerlandês|||✔|✔|
|Inglês (en-us)|✔ ||||
|Francês (fr-FR)|✔||||
|Francês (fr AC)|✔||||
|Alemão|||✔|✔|
|Italiano|✔||||
|Japonês||||✔|
|Coreano||✔||✔|
|Português (Brasil)|✔||||
|Espanhol (es-ES)|✔||||
|Espanhol (es MX)|✔||||

 