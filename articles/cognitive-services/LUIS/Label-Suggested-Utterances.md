---
title: Etiqueta utterances sugeridos com LUIS | Microsoft Docs
description: Utilize a compreensão de idiomas (LUIS) para etiquetar utterances sugeridas e ajudar a aprendizagem de Active Directory aumento.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/08/2017
ms.author: v-geberr
ms.openlocfilehash: 91a2fe738743d359677392bfb79aac885702b440
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35356431"
---
# <a name="review-endpoint-utterances"></a>Reveja utterances de ponto final

A funcionalidade de breakthrough do LUIS é o [conceito](luis-concept-review-endpoint-utterances.md) de aprendizagem do Active Directory. Depois do LUIS tem consultas de ponto final, LUIS utiliza o Active Directory de aprendizagem para melhorar a qualidade dos resultados. No processo de aprendizagem do Active Directory, LUIS examina todas as utterances de ponto final e seleciona utterances que se não souber de. Se Etiquetar estes utterances, formação e publicar, em seguida, LUIS identifica utterances com mais exatidão. 

## <a name="filter-utterances"></a>Filtrar utterances
1. Abra a aplicação (por exemplo, TravelAgent), selecionando o respetivo nome nos **aplicações My** página, em seguida, selecione **criar** na barra superior.

2. Sob o **melhorar o desempenho da aplicação**, selecione **rever utterances de ponto final**.

    ![Reveja utterances](./media/label-suggested-utterances/review.png)

3. No **rever utterances de ponto final** página, selecione no **lista de filtro por intenção ou entidade** caixa de texto. Esta lista pendente inclui todos os pendentes em **pendentes** e todas as entidades em **entidades**.

    ![Filtro de utterances](./media/label-suggested-utterances/filter.png)

4. Selecione uma categoria (pendentes ou entidades) na lista pendente e reveja as utterances.

    ![Utterances intenção](./media/label-suggested-utterances/intent-utterances.png)

## <a name="label-entities"></a>Entidades de etiqueta
LUIS substitui tokens de entidade (palavras) com os nomes das entidades realçados azul. Se um utterance tem sem etiqueta entidades, etiqueta-los no utterance. 

1. Selecione em word(s) no utterance. 

2. Selecione uma entidade na lista.

    ![Entidade de etiqueta](./media/label-suggested-utterances/label-entity.png)

## <a name="align-single-utterance"></a>Alinhar utterance único

Cada utterance tem um objetivo sugerido apresentado no **alinhado intenção** coluna. 

1. Se concordar com esse sugestão, selecione na marca de verificação.

    ![Manter a intenção alinhada](./media/label-suggested-utterances/align-intent-check.png)

2. Se disagree com a sugestão, selecione o objetivo correto da lista pendente intenção aligned, em seguida, selecione na marca de verificação para a direita da intenção aligned. 

    ![Alinhar intenção](./media/label-suggested-utterances/align-intent.png)

3. Depois de selecionar na marca de verificação, o utterance é removido da lista. 

## <a name="align-several-utterances"></a>Alinhar utterances várias

Para alinhar utterances várias, selecione a caixa para a esquerda dos utterances, em seguida, selecione no **adicionar selecionado** botão. 

![Alinhar vários](./media/label-suggested-utterances/add-selected.png)

## <a name="verify-aligned-intent"></a>Certifique-se a intenção alinhada
Pode verificar o utterance foi alinhada com a tentativa de correta, acedendo ao **pendentes** página, selecione o nome da intenção e rever os utterances. Utterance de **rever utterances de ponto final** está na lista.

## <a name="delete-utterance"></a>Eliminar utterance
Cada utterance pode ser eliminado da lista de revisão. Depois de eliminar, não irá aparecer na lista de novo. Isto acontece mesmo que o utilizador introduz o mesmo utterance do ponto final. 

Se não souber se deve eliminar o utterance, movê-la para a intenção, None ou crie um novo objetivo, tais como "diversas" e mova o utterance para esse objetivo. 

## <a name="delete-several-utterances"></a>Eliminar vários utterances
Para eliminar vários utterances, selecione cada item e seleciona na Reciclagem de lixo à direita do **adicionar selecionado** botão.

![Eliminar vários](./media/label-suggested-utterances/delete-several.png)

## <a name="next-steps"></a>Passos Seguintes

Para testar como melhora o desempenho depois de etiqueta utterances sugeridos, pode aceder a consola de teste, selecionando **testar** no painel superior. Para obter instruções sobre como testar a aplicação utilizando a consola de teste, consulte [formação e testar a aplicação](Train-Test.md).
