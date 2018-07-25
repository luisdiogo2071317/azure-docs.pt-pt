---
title: Preparar a sua aplicação LUIS – Azure | Documentos da Microsoft
description: Utilize a compreensão de idiomas (LUIS) para preparar o seu modelo.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/14/2018
ms.author: diberry
ms.openlocfilehash: e947df20141b0b9870f318f410488aea23bafcf5
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/24/2018
ms.locfileid: "39223189"
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

    ![Botão de preparação](./media/luis-how-to-train/train-button.png)

3. Quando o treinamento estiver concluído, uma barra de notificação de verde é apresentada na parte superior do navegador.

    ![Treinar e testar a aplicação de página](./media/luis-how-to-train/train-success.png)

<!-- The following note refers to what might cause the error message "Training failed: FewLabels for model: <ModelName>" -->

>[!NOTE]
>Se tiver uma ou mais objetivos na sua aplicação que não contêm as expressões de exemplo, não é possível preparar a sua aplicação. Adicione expressões para todos os seus objetivos. Para obter mais informações, consulte [adicionar expressões de exemplo](luis-how-to-add-example-utterances.md).

## <a name="next-steps"></a>Passos Seguintes

* [Identifique expressões sugeridas com os LUIS](luis-how-to-review-endoint-utt.md) 
* [Utilizar as funcionalidades para melhorar o desempenho da sua aplicação LUIS](luis-how-to-add-features.md) 