---
title: Filtragem de palavras ofensivas - API de texto do tradutor
titlesuffix: Azure Cognitive Services
description: Utilize linguagem inapropriada filtragem na API de texto do Translator.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: conceptual
ms.date: 12/14/2017
ms.author: v-jansko
ms.openlocfilehash: 4154950cf8d8b6ec2e47a9f8100cb7983ac127bf
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/23/2018
ms.locfileid: "49648042"
---
# <a name="add-profanity-filtering-with-the-translator-text-api"></a>Adicionar palavras ofensivas filtragem com a API de texto do tradutor

Normalmente, o serviço Microsoft Translator retém linguagem inapropriada que está presente na origem na tradução. O grau de linguagem inapropriada e contexto que torna as palavras impróprias diferem entre culturas. Como resultado, o grau de linguagem inapropriada no idioma de destino pode ser Sim os amplificou ou reduzido.

Se quiser evitar ver linguagem inapropriada na tradução, mesmo que a linguagem inapropriada está presente no texto de origem, utilize a linguagem inapropriada opção disponível no método Translate() de filtragem. Esta opção permite-lhe escolher se pretende ver linguagem inapropriada eliminada, marcado com etiquetas adequadas, ou não tomar nenhuma ação executada.

O método de Translate() usa o parâmetro de "Opções", que contém o novo elemento "ProfanityAction". Os valores aceites do ProfanityAction são "NoAction", "Assinalado" e "Eliminado".

## <a name="accepted-values-of-profanityaction-and-examples"></a>Valores aceites de ProfanityAction e exemplos
|Valor de ProfanityAction | Ação | Exemplo: Origem-japonês | Exemplo: Destino-inglês|
| :---|:---|:---|:---|
| NoAction | Predefinição. Mesmo que não definir a opção. Linguagem inapropriada passa de origem para destino. | 彼は変態です。 | Ele é idiota. |
| Marcado | Palavras profanas são cercadas por marcas XML \<linguagem inapropriada >... \</profanity >. | 彼は変態です。 | Ele é um \<linguagem inapropriada > puxão\</profanity >. |
| Eliminada | Palavras profanas são removidas da saída sem substituição. | 彼は。 | Ele é um. |

## <a name="next-steps"></a>Passos Seguintes
> [!div class="nextstepaction"]
> [Aplicam-se a linguagem inapropriada filtragem da chamada de API do Translator](reference/v3-0-translate.md)
