---
title: Utterances LUIS aplicações do Azure | Microsoft Docs
description: Adicione utterances nas aplicações de serviço de inteligente compreensão do idioma (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 02/13/2018
ms.author: v-geberr
ms.openlocfilehash: 7c2cd84df8f1eccbd30a7c8054ec8d06336cf2dd
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264651"
---
# <a name="utterances-in-luis"></a>Utterances no LUIS

**Utterances** são de entrada do utilizador que a aplicação tem interpretar. Para preparar LUIS para extrair as entidades e pendentes dos mesmos, é importante capturar uma variedade de entradas diferentes para cada intenção. Learning Active Directory ou o processo de continuar preparar o novo utterances, é essencial para intelligence adquiridos por máquina que LUIS fornece.

Recolha expressões que pensa que os utilizadores introduzirem. Inclua utterances que significa que a mesma coisa, mas são construídos de forma diferente no comprimento e o posicionamento de palavra. 

## <a name="how-to-choose-varied-utterances"></a>Como escolher utterances variadas
Quando primeiro começar por [adicionar utterances exemplo] [ add-example-utterances] para o seu modelo LUIS, estão aqui alguns princípios a lembrar.

### <a name="utterances-arent-always-well-formed"></a>Utterances não são sempre formados
Poderá ser uma frase, como "Livro-me um pedido para Paris" ou um fragmento de uma frase, como "Booking" ou "Voo Paris."  Os utilizadores, muitas vezes, fizer ortografia. Quando planear a sua aplicação, considere se pretende ou não-verificação ortográfica intervenção do utilizador antes de passá-lo para LUIS. O [a API de verificação ortográfica do Bing] [ BingSpellCheck] integra LUIS. Pode associar a aplicação de LUIS com uma chave externa para a API de verificação ortográfica do Bing quando publica-lo. Se não ortográfica utterances de utilizador de verificação, deve preparar LUIS no utterances que incluem erros de digitação e misspellings.

### <a name="use-the-representative-language-of-the-user"></a>Utilizar a linguagem representativa do utilizador
Ao escolher utterances, lembre-se de que a sua opinião é um termo comum ou o frase de acesso poderá não ser ao utilizador normal da sua aplicação de cliente. Poderá não ter experiência de domínio. Por isso tenha cuidado quando utilizar termos ou expressões que um utilizador seria apenas supor que o se fossem perito.

### <a name="choose-varied-terminology-as-well-as-phrasing"></a>Escolha a terminologia variada, bem como phrasing
Vai descobrir que, mesmo que o se os esforços para criar o frase variadas padrões, ainda será repetir algumas vocabulário.

Siga estes utterances de exemplo:
```
how do I get a computer?
Where do I get a computer?
I want to get a computer, how do I go about it?
When can I have a computer? 
```
Aqui, o termo de núcleos "computador", não é diversificados. Estes foi supor que o computador de secretária, portáteis, estação de trabalho ou mesmo apenas máquina. LUIS inteligentemente infere sinónimos a partir do contexto, mas quando criar utterances para formação, é ainda melhor variá-los.

## <a name="example-utterances-in-each-intent"></a>Utterances de exemplo em cada intenção
Cada intenção tem de ter utterances de exemplo, pelo menos 10 a 15. Se tiver um objetivo que não tem qualquer utterances de exemplo, não será possível preparar LUIS. Se tiver um objetivo com um ou muito poucos utterances de exemplo, LUIS não prever com precisão o objetivo. 

## <a name="add-small-groups-of-10-15-utterances-for-each-authoring-iteration"></a>Adicionar grupos pequenos de 10 a 15 utterances para cada iteração de criação
Em cada iteração do modelo, não adicione uma grande quantidade de utterances. Adicione utterances quantidades de dezenas. [Formação](luis-how-to-train.md), [publicar](publishapp.md), e [testar](interactive-test.md) novamente.  

LUIS baseia-se efetiva modelos com utterances que estão selecionadas cuidadosamente. A adição de utterances demasiados não é útil porque introduz confusões.  

É melhor começar a utilizar alguns utterances, em seguida, [rever utterances de ponto final](label-suggested-utterances.md) para extração correta de predição e a entidade de intenção.

## <a name="training-utterances"></a>Utterances formação
Formação não é determinística: a predição de utterance foi variam ligeiramente entre versões ou aplicações.

## <a name="testing-utterances"></a>Testar utterances 

Os programadores devem começar a testar a respetiva aplicação LUIS com tráfego real enviando utterances para o ponto final. Estes utterances são utilizados para melhorar o desempenho do pendentes e entidades com [rever utterances](label-suggested-utterances.md). Testes submetidos com o Web site LUIS do painel de teste não são enviados através de ponto final e, por isso, não contribuir para learning Active Directory. 

## <a name="review-utterances"></a>Reveja utterances
Depois do seu modelo treinado, publicados e receção [endpoint](luis-glossary.md#endpoint) consultas, [reveja os utterances](label-suggested-utterances.md) sugerida pelo LUIS. LUIS seleciona utterances de ponto final que têm baixas pontuações das classificações para intenção ou entidade. 

## <a name="best-practices"></a>Melhores práticas
Reveja [melhores práticas](luis-concept-best-practices.md) para obter mais informações.

## <a name="next-steps"></a>Passos Seguintes
Consulte [adicionar utterances exemplo] [ add-example-utterances] para obter informações sobre uma aplicação LUIS compreender utterances de utilizador de preparação.

[add-example-utterances]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-how-to-add-example-utterances
[BingSpellCheck]: https://docs.microsoft.com/azure/cognitive-services/bing-spell-check/proof-text