---
title: Expressões com nas aplicações de LUIS
titleSuffix: Azure Cognitive Services
description: Expressões com são de entrada do usuário que a sua aplicação precisa para interpretar. Recolha as frases que acredita que os utilizadores introduzirem o. Inclua expressões com o que significa que a mesma coisa, mas são construídos de forma diferente de comprimento de palavra e colocação do word.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: c455296348e3b4378a2912fa5388098b206b3ea4
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/15/2018
ms.locfileid: "45632318"
---
# <a name="utterances-in-luis"></a>Expressões com no LUIS

**Expressões com** são de entrada do usuário que a sua aplicação precisa para interpretar. Para preparar o LUIS para extrair as intenções e entidades dos mesmos, é importante capturar uma variedade de diferentes entradas para cada intenção. Aprendizagem ativa, ou o processo de continuar a dar formação em novas expressões, é essencial para inteligência aprendidas por máquina, que fornece o LUIS.

Recolha as frases que acredita que os utilizadores introduzirem o. Inclua expressões com o que significa que a mesma coisa, mas são construídos de forma diferente de comprimento de palavra e colocação do word. 

## <a name="how-to-choose-varied-utterances"></a>Como escolher expressões com variadas
Quando obter iniciado pela primeira vez [adição de expressões de exemplo](luis-how-to-add-example-utterances.md) para o modelo do LUIS, aqui estão alguns princípios a ter em conta.

### <a name="utterances-arent-always-well-formed"></a>Expressões com não estão sempre bem formados
Pode ser uma frase, como "Reservar quarto-me um pedido para Paris", ou um fragmento de uma frase, como "Reserva" ou "Voo Paris."  Os utilizadores fazem, muitas vezes, os erros de ortografia. Ao planejar a sua aplicação, considere se é ou não verifique a ortografia entrada do usuário antes de o transmitir para LUIS. O [a API de verificação ortográfica do Bing] [ BingSpellCheck] integra-se com os LUIS. Pode associar a aplicação do LUIS com uma chave externa para a API de verificação de ortografia do Bing quando o publicar. Se não escrever expressões de utilizador de verificação, deve preparar o LUIS em expressões que incluem erros de digitação e erros de ortografia.

### <a name="use-the-representative-language-of-the-user"></a>Utilize a linguagem representativa do utilizador
Ao escolher expressões com, lembre-se de que sua opinião é um termo comum ou a frase não pode ser para o usuário típico da sua aplicação de cliente. Talvez não tenham experiência de domínio. Por isso, tenha cuidado ao usar termos ou frases que um usuário apenas diria se fossem um especialista.

### <a name="choose-varied-terminology-as-well-as-phrasing"></a>Escolha a terminologia variada, bem como frases
Descobrirá que, mesmo se tornar os esforços para criar padrões de sentença variadas, ainda será repetido alguns vocabulário.

Siga estas expressões de exemplo:

|Expressões de exemplo|
|--|
|como posso obter um computador?|
|Onde posso obter um computador?|
|Eu quero chegar um computador, como posso sobre isso?|
|Quando tiver um computador?| 

O termo principal aqui, o "computador", não é diversificados. Eles poderiam dizer-se ao computador desktop, laptop, estação de trabalho ou até mesmo apenas máquina. LUIS inteligentemente infere sinónimos do contexto, mas ao criar expressões com formação, é ainda melhor variá-los.

## <a name="example-utterances-in-each-intent"></a>Expressões com de exemplo em cada intenção
Cada intenção tem de ter expressões de exemplo, pelo menos de 10 a 15. Se tiver um objetivo que não tem quaisquer expressões de exemplo, não será capaz de preparar o LUIS. Se tiver um objetivo com um ou muito poucas expressões de exemplo, LUIS não prever com precisão a intenção. 

## <a name="add-small-groups-of-10-15-utterances-for-each-authoring-iteration"></a>Adicionar grupos pequenos de expressões de 10 a 15 para cada iteração de criação
Em cada iteração do modelo, não adicione uma grande quantidade de expressões. Adicione expressões em quantidades de dezenas. [Train](luis-how-to-train.md), [publicar](luis-how-to-publish-app.md), e [testar](luis-interactive-test.md) novamente.  

LUIS baseia-se em vigor modelos com expressões com cuidadosamente selecionadas. Adicionar expressões com demasiados não é importante porque introduz confusão.  

É melhor começar com algumas expressões, em seguida, [rever expressões de ponto final](luis-how-to-review-endoint-utt.md) para extração correta de entidade e de predição de intenção.

## <a name="ignoring-words-and-punctuation"></a>A ignorar a palavras e pontuação
Se pretender ignorar palavras específicas ou pontuação na expressão de exemplo, utilize um [padrão](luis-concept-patterns.md#pattern-syntax) com o _ignorar_ sintaxe. 

## <a name="training-utterances"></a>Expressões de treinamento
O treinamento é determinística: a predição de expressão poderia variam ligeiramente entre versões ou aplicações.

## <a name="testing-utterances"></a>Teste de expressões 

Os desenvolvedores devem começar a testar seus aplicativos de LUIS com tráfego real através do envio expressões com para o ponto final. Estas expressões são utilizados para melhorar o desempenho dos objetivos e entidades com [rever expressões com](luis-how-to-review-endoint-utt.md). Testes submetidos com o site do LUIS painel de teste não são enviados através do ponto final e não contribuem para a aprendizagem ativa. 

## <a name="review-utterances"></a>Expressões de revisão
Depois do modelo estiver preparado, publicada e de recebimento [ponto final](luis-glossary.md#endpoint) consultas, [rever as expressões](luis-how-to-review-endoint-utt.md) sugerido pelo LUIS. LUIS seleciona expressões com ponto final com pontuações de baixas para a intenção ou a entidade. 

## <a name="best-practices"></a>Melhores práticas
Revisão [melhores práticas](luis-concept-best-practices.md) para saber mais.

## <a name="next-steps"></a>Passos Seguintes
Ver [adicionar expressões de exemplo](luis-how-to-add-example-utterances.md) para obter informações sobre uma aplicação LUIS para compreender as expressões de utilizador de treinamento.

[BingSpellCheck]: https://docs.microsoft.com/azure/cognitive-services/bing-spell-check/proof-text