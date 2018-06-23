---
title: Preparar a sua aplicação LUIS - Azure | Microsoft Docs
description: Utilize a compreensão de idiomas (LUIS) para preparar o seu modelo.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/14/2018
ms.author: v-geberr
ms.openlocfilehash: 4593954e9e0a60beaa5ee86df848f908b23c6b20
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355430"
---
# <a name="train-your-luis-app"></a>Preparar a sua aplicação LUIS

Formação é o processo de teaching a sua aplicação de compreensão de idiomas (LUIS) para melhorar a compreensão de linguagem natural. Preparar a sua aplicação LUIS após as atualizações ao modelo de como adicionar, editar, etiquetas ou eliminar entidades, pendentes ou utterances. 

<!--
When you train a LUIS app by example, LUIS generalizes from the examples you have labeled, and it learns to recognize the relevant intents and entities. This teaches LUIS to improve classification accuracy in the future. -->

Formação e [testar](luis-concept-test.md) uma aplicação é um processo iterativo. Depois de preparar a sua aplicação LUIS, testá-lo com utterances de exemplo para ver se as entidades e pendentes são reconhecidas corretamente. Se não estiver, efetue as atualizações da aplicação de LUIS, formação e teste novamente. 

## <a name="train-your-app"></a>Preparar a sua aplicação
Para iniciar o processo iterativo, primeiro terá de preparar a sua aplicação LUIS, pelo menos, uma vez. Certifique-se que cada intenção tem pelo menos um utterance antes de formação.

1. Aceder à sua aplicação, selecionando o respetivo nome na **aplicações My** página. 

2. Na sua aplicação, selecione **formação** no painel superior. 

    ![Botão de formação](./media/luis-how-to-train/train-button.png)

3. Quando a formação estiver concluída, é apresentada uma barra de notificação verde na parte superior do browser.

    ![Página de aplicação de teste e formação](./media/luis-how-to-train/train-success.png)

<!-- The following note refers to what might cause the error message "Training failed: FewLabels for model: <ModelName>" -->

>[!NOTE]
>Se tiver uma ou mais pendentes na sua aplicação que não contêm utterances de exemplo, não é possível preparar a sua aplicação. Adicione utterances para todos os seus pendentes. Para obter mais informações, consulte [adicionar utterances exemplo](luis-how-to-add-example-utterances.md).

## <a name="next-steps"></a>Passos Seguintes

* [Etiqueta utterances sugeridos com LUIS](Label-Suggested-Utterances.md) 
* [Utilize as funcionalidades para melhorar o desempenho da sua aplicação LUIS](luis-how-to-add-features.md) 