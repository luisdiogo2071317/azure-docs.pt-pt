---
title: Reveja as expressões com ponto final utilizar a aprendizagem ativa na compreensão de idiomas (LUIS) - Azure | Documentos da Microsoft
description: Utilizar a funcionalidade de aprendizagem ativa com o nome 'Rever expressões de ponto final' para melhorar as previsões de desempenho mais rápido.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 06/08/2018
ms.author: v-geberr;
ms.openlocfilehash: e416eec29ff9f4ac96eabf11c87424abeba0c75b
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855722"
---
# <a name="enable-active-learning-by-reviewing-endpoint-utterances"></a>Ativar a aprendizagem ativa, revendo os discursos de ponto final
Aprendizagem ativa é uma das três estratégias para melhorar a exatidão da previsão e o mais fácil de implementar. 

## <a name="what-is-active-learning"></a>O que é a aprendizagem ativa
Aprendizagem ativa é um processo de dois passos. Em primeiro lugar, LUIS seleciona expressões que recebe no ponto final da aplicação que necessitam de validação. O segundo passo é efetuado pelo proprietário da aplicação ou funcionário para validar as expressões selecionadas para [rever](label-suggested-utterances.md), incluindo a intenção correta e todas as entidades na intenção. Depois de rever as expressões, formar e publicar a aplicação novamente. 

## <a name="which-utterances-are-on-the-review-list"></a>As expressões são na lista de revisão
LUIS adiciona expressões com a lista de revisão quando a parte superior disparando intenção tem uma pontuação baixa ou pontuações de principais dois objetivos estão muito próximos. 

## <a name="single-pool-for-utterances-per-app"></a>Conjunto único para expressões com por aplicação
O **rever expressões de ponto final** lista não é alterada com base na versão. Há um único conjunto de expressões para rever, indiferentemente da versão a expressão que está ativamente a editar ou qual é a versão da aplicação foi publicada no ponto final. 

## <a name="where-are-the-utterances-from"></a>Onde estão as expressões de
Expressões de ponto de extremidade são obtidas a partir de consultas de utilizador final no ponto final HTTP do aplicativo. Se a aplicação não está publicada ou não recebeu pedidos com êxito na ainda, não tem quaisquer expressões de com para rever. Se não existem resultados de ponto de extremidade são recebidos para um objetivo específico ou uma entidade, não tem expressões para rever o que os contêm. 

## <a name="schedule-review-periodically"></a>Revisão de agenda periodicamente
Rever expressões sugeridas com não precisa ser feito a todos os dias, mas deve fazer parte da manutenção regular de LUIS. 

## <a name="delete-review-items-programmatically"></a>Eliminar itens de revisão através de programação
Se a aplicação for grande, pode optar por rever algumas expressões de com e elimine as restantes na lista por meio de programação. Isso é feito pela primeira [obtendo](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0a) a lista e, em seguida [eliminar](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58b6f32139e2bb139ce823c9) as expressões por ID.

## <a name="next-steps"></a>Passos Seguintes

* Saiba como [rever](Label-Suggested-Utterances.md) expressões de ponto final
