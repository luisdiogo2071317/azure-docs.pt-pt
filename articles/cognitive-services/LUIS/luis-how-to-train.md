---
title: Preparar a sua aplicação LUIS
titleSuffix: Azure Cognitive Services
description: Treinamento é o processo de ensinar a sua aplicação de compreensão de idiomas (LUIS) para aprimorar sua compreensão de linguagem natural. Treine a sua aplicação LUIS após as atualizações para o modelo de como adicionar, editar, etiquetagem ou a eliminação de entidades, intenções ou expressões com.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/06/2018
ms.author: diberry
ms.openlocfilehash: 42cff3dd8237598da5aa71ed1a4d6462c5b4c25d
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44049136"
---
# <a name="train-your-luis-app"></a>Preparar a sua aplicação LUIS

Treinamento é o processo de ensinar a sua aplicação de compreensão de idiomas (LUIS) para aprimorar sua compreensão de linguagem natural. Treine a sua aplicação LUIS após as atualizações para o modelo de como adicionar, editar, etiquetagem ou a eliminação de entidades, intenções ou expressões com. 

<!--
When you train a LUIS app by example, LUIS generalizes from the examples you have labeled, and it learns to recognize the relevant intents and entities. This teaches LUIS to improve classification accuracy in the future. -->

Treinamento e [teste](luis-concept-test.md) uma aplicação é um processo iterativo. Depois de preparar a aplicação do LUIS, testá-la com expressões de exemplo para ver se as intenções e entidades são reconhecidas corretamente. Se não estiverem, fazer atualizações para a aplicação LUIS, formação e teste novamente. 

## <a name="train-your-app"></a>Preparar a sua aplicação
Para iniciar o processo iterativo, primeiro tem de preparar a sua aplicação LUIS, pelo menos, uma vez. Certifique-se que cada intenção tem, pelo menos, uma expressão antes de treinamento.

1. Aceder à sua aplicação, selecionando o respetivo nome na **as minhas aplicações** página. 

2. Na sua aplicação, selecione **Train** no painel superior. 

3. Quando o treinamento estiver concluído, uma barra de notificação de verde é apresentada na parte superior do navegador.

<!-- The following note refers to what might cause the error message "Training failed: FewLabels for model: <ModelName>" -->

>[!NOTE]
>Se tiver uma ou mais objetivos na sua aplicação que não contêm as expressões de exemplo, não é possível preparar a sua aplicação. Adicione expressões para todos os seus objetivos. Para obter mais informações, consulte [adicionar expressões de exemplo](luis-how-to-add-example-utterances.md).

## <a name="next-steps"></a>Passos Seguintes

* [Identifique expressões sugeridas com os LUIS](luis-how-to-review-endoint-utt.md) 
* [Utilizar as funcionalidades para melhorar o desempenho da sua aplicação LUIS](luis-how-to-add-features.md) 