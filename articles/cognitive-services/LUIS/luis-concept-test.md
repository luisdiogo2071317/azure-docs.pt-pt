---
title: Testar a aplicação de LUIS - Azure | Microsoft Docs
description: Utilize a compreensão de idiomas (LUIS) para trabalhar continuamente na sua aplicação para refiná-lo e melhorar a compreensão de idiomas.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/14/2018
ms.author: v-geberr
ms.openlocfilehash: 8c702d2adbadd2736eed05c7580e8aabf69affbf
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266333"
---
# <a name="testing-in-luis"></a>Teste no LUIS

Os testes são o processo de fornecer utterances de exemplo para LUIS e obter uma resposta de reconhecido por LUIS pendentes e entidades. 

Pode [testar](interactive-test.md) LUIS interativamente, um utterance cada vez, ou forneça um [batch](luis-concept-batch-test.md) de utterances. Com os testes, comparar atual [Active Directory](luis-concept-version.md#active-version) modelo para o modelo publicado. 

<a name="A-test-score"></a>
<a name="Score-all-intents"></a>
<a name="E-(exponent)-notation"></a>
## <a name="what-is-a-score-in-testing"></a>O que é uma pontuação no teste?
Consulte [pontuação de predição](luis-concept-prediction-score.md) conceitos para saber mais sobre as pontuações de predição.

## <a name="interactive-testing"></a>Testar interativa
Testar interativo é feita do **teste** painel do Web site. Pode introduzir um utterance para ver como entidades e pendentes são identificadas e classificadas. Se LUIS não se encontra prever as entidades e pendentes como esperado num utterance no painel de teste, copie-o para o **intenção** página como uma nova utterance. Em seguida, etiqueta as partes do que utterance e formação LUIS. 

## <a name="batch-testing"></a>Teste do batch
Consulte [batch testar](luis-concept-batch-test.md) se estiver a testar utterance mais do que um cada vez.

## <a name="endpoint-testing"></a>Teste de ponto final
Pode testar a utilizar o [endpoint](luis-glossary.md#endpoint) com um máximo de duas versões da sua aplicação. Com a versão principal ou em direto da sua aplicação definida como o **produção** ponto final, adicionar uma segunda versão para o **transição** ponto final. Esta abordagem fornece três versões de um utterance: o modelo atual no painel de teste de [LUIS] [ LUIS] Web site e as versões de dois em dois pontos finais diferentes. 

Todas as endpoint teste contagens para a sua quota de utilização. 

## <a name="do-not-log-tests"></a>Não sessão testes
Se testar em relação a um ponto final e não quiser utterance com sessão iniciada, não se esqueça de utilizar o `logging=false` configuração de cadeia de consulta.

## <a name="where-to-find-utterances"></a>Onde encontrar utterances
LUIS armazena utterances todos os registados no registo de consulta, disponível para transferência no [LUIS] [ LUIS] Web site **aplicações** página lista, bem como o LUIS [APIs de criação ](https://aka.ms/luis-authoring-apis). 

Qualquer utterances LUIS é não souber de constam o **[rever utterances de ponto final](label-suggested-utterances.md)** página do [LUIS] [ LUIS] Web site. 

![Reveja utterances de ponto final](./media/luis-concept-test/review-endpoint-utterances.png)
 
## <a name="remember-to-train"></a>Não se esqueça de formação
Não se esqueça de [preparar](luis-how-to-train.md) LUIS depois de efetuar alterações ao modelo. Alterações à aplicação LUIS não são visualizadas no teste até que a aplicação está preparada. 

## <a name="best-practices"></a>Melhores práticas
Saiba [melhores práticas](luis-concept-best-practices.md).

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre [testar](interactive-test.md) sua utterances.

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions