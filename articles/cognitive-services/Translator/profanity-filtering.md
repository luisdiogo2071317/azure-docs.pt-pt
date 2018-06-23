---
title: Profanity filtragem com o Microsoft tradutor texto API | Microsoft Docs
description: Utilize profanity filtragem na API de texto do conversor de Microsoft.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 12/14/2017
ms.author: v-jansko
ms.openlocfilehash: a7172e1e8aa336c011fb06e93fc5c4b54d26a3cd
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352591"
---
# <a name="how-to-add-profanity-filtering-with-the-microsoft-translator-text-api"></a>Como adicionar profanity filtragem com a API do Microsoft tradutor texto

Normalmente, o serviço de tradutor mantém profanity que está presente na origem na tradução. O grau de profanity e o contexto que torna palavras profanas diferem entre culturas. Como resultado o grau de profanity na linguagem de destino pode ser amplified ou reduzido.

Se quiser evitar ver profanity na tradução (independentemente da presença de profanity no texto de origem), pode utilizar a opção de filtragem de profanity no método Translate(). A opção permite-lhe escolher se pretende ver profanity eliminado ou marcado com etiquetas adequadas ou nenhuma ação executada.

O método de Translate() assume um parâmetro de "Opções", que contém o novo elemento de "ProfanityAction". Os valores aceites de ProfanityAction são "NoAction", "Assinalado" e "Eliminado".

## <a name="accepted-values-of-profanityaction-and-examples"></a>Valores aceites de ProfanityAction e exemplos
|Valor de ProfanityAction | Ação | Exemplo: Origem - japonês | Exemplo: Destino - inglês|
| :---|:---|:---|:---|
| NoAction | Predefinição. Mesmo que não definir a opção. Profanity transmite a partir da origem para destino. | 彼は変態です。 | Ele é um jerk. |
| Marcada | Palavras profanas são rodeadas por etiquetas XML \<profanity >... \</Profanity >. | 彼は変態です。 | Ele é um \<profanity > jerk\</profanity >. |
| Eliminada | Palavras profanas são removidas da saída sem substituição. | 彼は。 | Ele é um. |

## <a name="next-steps"></a>Passos Seguintes
> [!div class="nextstepaction"]
> [Aplicar profanity filtragem com a chamada de API de conversor](reference/v3-0-translate.md)

