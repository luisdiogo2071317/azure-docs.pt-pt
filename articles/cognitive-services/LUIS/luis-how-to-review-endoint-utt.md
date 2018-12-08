---
title: Expressões com de utilizador de revisão
titleSuffix: Language Understanding - Azure Cognitive Services
description: O recurso de vanguarda através do LUIS é o conceito de aprendizagem ativa. Depois que o LUIS tiver consultas de ponto de extremidade, aprendizagem ativa melhora a qualidade dos resultados por expressões seleciona com que ela seja verificá-lo. Se coloca essas expressões com uma etiqueta, formar e publicar, em seguida, LUIS identifica expressões com mais precisão.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/06/2018
ms.author: diberry
ms.openlocfilehash: 7175463001eac118d2939d3479e61a01b80ba86b
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53106554"
---
# <a name="how-to-review-endpoint-utterances-in-luis-portal"></a>Como rever os discursos de ponto final no portal do LUIS

O recurso de vanguarda através do LUIS é o [conceito](luis-concept-review-endpoint-utterances.md) de aprendizagem ativa. Depois que o LUIS tiver consultas de ponto de extremidade, LUIS utiliza o Active Directory de aprendizagem para aprimorar a qualidade dos resultados. O processo de aprendizagem ativa, LUIS examina todas as expressões de ponto final e seleciona expressões com que ela seja verificá-lo. Se coloca essas expressões com uma etiqueta, formar e publicar, em seguida, LUIS identifica expressões com mais precisão. 

## <a name="filter-utterances"></a>Expressões de filtro
1. Abra a sua aplicação (por exemplo, TravelAgent) ao selecionar o respetivo nome na **as minhas aplicações** página, em seguida, selecione **criar** na barra superior.

2. Sob o **melhorar o desempenho da aplicação**, selecione **rever expressões de ponto final**.

3. Na **rever expressões de ponto final** página, selecione no **lista de filtros de intenção ou entidade** caixa de texto. Esta lista pendente inclui todas as intenções sob **INTENÇÕES** e todas as entidades sob **entidades**.

    ![Filtro de expressões](./media/label-suggested-utterances/filter.png)

4. Na lista pendente, selecione uma categoria (intenções ou entidades) e reveja as expressões.

    ![Expressões com intenção](./media/label-suggested-utterances/intent-utterances.png)

## <a name="label-entities"></a>Entidades de etiqueta
LUIS substitui tokens de entidade (palavras) com os nomes de entidades realçados em azul. Se uma expressão tem sem etiqueta entidades, da etiqueta-los na expressão. 

1. Selecione o word(s) na expressão. 

2. Selecione uma entidade a partir da lista.

    ![Etiqueta de entidade](./media/label-suggested-utterances/label-entity.png)

## <a name="align-single-utterance"></a>Alinhar expressão única

Cada ocorrência de pronunciação tem uma intenção sugerida zobrazené a **alinhadas com a intenção** coluna. 

1. Se concordar com essa sugestão, selecione a marca de verificação.

    ![Manter a intenção alinhada](./media/label-suggested-utterances/align-intent-check.png)

2. Se discordar com a sugestão, selecione a intenção correta da lista pendente intenção alinhada, em seguida, selecione na marca de verificação para a direita da intenção alinhada. 

    ![Alinhar intenção](./media/label-suggested-utterances/align-intent.png)

3. Depois de selecionar a marca de verificação, a expressão é removido da lista. 

## <a name="align-several-utterances"></a>Alinhar expressões com vários

Alinhar expressões com vários, marque a caixa à esquerda das expressões, em seguida, selecione sobre o **adicionar selecionado** botão. 

![Alinhar vários](./media/label-suggested-utterances/add-selected.png)

## <a name="verify-aligned-intent"></a>Certifique-se a intenção alinhada
Pode verificar a expressão foi alinhado com a intenção correta ao aceder a **intenções** página, selecione o nome de intenção e rever as expressões. A expressão da **rever expressões de ponto final** está na lista.

## <a name="delete-utterance"></a>Eliminar expressão
Cada expressão pode ser eliminado da lista de revisão. Depois de eliminado, não aparecerá na lista novamente. Isso vale mesmo que o usuário insere a expressão mesmo do ponto final. 

Se tiver a certeza se deve excluir a expressão, mova-o para a intenção, nenhuma ou criar uma intenção de novo, como "diversos" e mova a expressão para esse propósito. 

## <a name="delete-several-utterances"></a>Eliminar várias expressões
Para eliminar várias expressões, selecione cada item e selecione do contentor de caixote do lixo à direita dos **adicionar selecionado** botão.

![Eliminar vários](./media/label-suggested-utterances/delete-several.png)

## <a name="next-steps"></a>Passos Seguintes

Para testar como o desempenho aumenta depois coloca expressões sugeridas com uma etiqueta, pode aceder à consola de teste, selecionando **testar** no painel superior. Para obter instruções sobre como testar a sua aplicação utilizando a consola de teste, consulte [treinar e testar a aplicação](luis-interactive-test.md).
