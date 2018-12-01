---
title: Impedir a tradução de conteúdo - API de texto do tradutor
titlesuffix: Azure Cognitive Services
description: Impedir a tradução de conteúdo com a API de texto do Translator.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: conceptual
ms.date: 11/20/2018
ms.author: v-jansko
ms.openlocfilehash: b012201bcb9e0aece3d7e1ddd4f771643088af76
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/30/2018
ms.locfileid: "52683379"
---
# <a name="how-to-prevent-translation-of-content-with-the-translator-text-api"></a>Como impedir a tradução de conteúdo com a API de texto do tradutor

A API de texto do Translator permite-lhe conteúdo de etiqueta, para que ele não está traduzido. Por exemplo, poderá marcar o código, um nome de marca ou uma palavra/expressão que não faz sentido quando localizado. 

## <a name="methods-for-preventing-translation"></a>Métodos para impedir a tradução
1. Uma marca de Twitter de escape @somethingtopassthrough ou #somethingtopassthrough. Anular-escape após a tradução.

2. Etiquetar o conteúdo com `notranslate`.

   Exemplo:

   ```html
   <div class="notranslate">This will not be translated.</div>
   <div>This will be translated. </div>
   ```

3. Utilize o [dicionário dinâmico](dynamic-dictionary.md) para prescrevem uma tradução específica.

4. Não passe a cadeia de caracteres para a API de texto do Translator para a tradução.

5. Microsoft Translator personalizado: Utilizar o uma [dicionário no personalizado Translator](custom-translator/what-is-dictionary.md) recomende uma tradução de uma frase com a probabilidade de 100%.


## <a name="next-steps"></a>Passos Seguintes
> [!div class="nextstepaction"]
> [Evitar a tradução em sua chamada de API do Translator](reference/v3-0-translate.md)
