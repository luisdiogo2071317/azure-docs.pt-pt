---
title: Reveja utterances de ponto final a utilizar learning Active Directory no idioma compreender (LUIS) - Azure | Microsoft Docs
description: Utilizar a funcionalidade de aprendizagem do Active Directory com o nome 'Rever utterances de ponto final' para melhorar as predições de desempenho mais rápido.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 06/08/2018
ms.author: v-geberr;
ms.openlocfilehash: b9672e8e63fb601d4411a342b7f3c00e30f9e002
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35356439"
---
# <a name="enable-active-learning-by-reviewing-endpoint-utterances"></a>Ativar o Active Directory learning revendo utterances de ponto final
Learning Active Directory é uma das três estratégias para melhorar a exatidão da previsão e mais fácil de implementar. 

## <a name="what-is-active-learning"></a>O que é learning Active Directory
Learning Active Directory é um processo de dois passos. Em primeiro lugar, LUIS seleciona utterances recebe ponto final da aplicação que necessitam de validação. O segundo passo é efetuado pelo proprietário da aplicação ou colaborador para validar os utterances selecionados para [rever](label-suggested-utterances.md), incluindo a intenção correta e as entidades dentro de intenção. Depois de rever os utterances, dar formação e publicar novamente a aplicação. 

## <a name="which-utterances-are-on-the-review-list"></a>Que utterances estão na lista de revisão
LUIS adiciona utterances à lista de revisão ao topo acionando intenção tem uma pontuação baixa ou pontuações superior dois pendentes são demasiado fechar. 

## <a name="where-are-the-utterances-from"></a>Onde estão utterances do
Utterances de ponto final são obtidas a partir da consulta o utilizador final do ponto final de HTTP da aplicação. Se a aplicação não está publicada ou não recebeu pedidos ainda, não tem qualquer utterances para rever. Se nenhum ponto final pedidos são recebidos para um objetivo específico ou entidade, não dispõe de utterances para rever que contêm-los. 

## <a name="schedule-review-periodically"></a>Agendar a rever periodicamente
Rever utterances sugeridos não necessita de ser efetuada diariamente, mas deve fazer parte do seu manutenção regular da LUIS. 

## <a name="delete-review-items-programmatically"></a>Eliminar itens de revisão através de programação
Se a aplicação for grande, pode optar por reveja algumas utterances e eliminar o resto da lista através de programação. Isto é feito ao primeiro [obter](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0a) lista e, em seguida, [eliminar](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58b6f32139e2bb139ce823c9) utterances por ID.

## <a name="next-steps"></a>Passos Seguintes

* Saiba como [rever](Label-Suggested-Utterances.md) utterances de ponto final
