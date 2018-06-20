---
title: Compreender a aplicação iterativa LUIS design - Azure | Microsoft Docs
description: As aplicações LUIS necessitam de iterações de design para preparar LUIS para obter a melhor extração de dados.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 02/12/2018
ms.author: v-geberr
ms.openlocfilehash: b7f8dd46dc8289322726934f330761b0f1ab94bd
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265942"
---
# <a name="authoring-cycle"></a>Ciclo de criação
LUIS Aprende melhor num ciclo iterativo de alterações de modelo, exemplos de utterance, publicação e a recolha de dados de consultas de ponto final. 

![Ciclo de criação](./media/luis-concept-app-iteration/iteration.png)

## <a name="building-a-luis-model"></a>Criar um modelo de LUIS
Objetivo do modelo é descobrir o que o utilizador é solicitando (a sua intenção ou intenção) e as partes da pergunta fornecem detalhes (entidades) que o ajudam a determinar a resposta. 

O modelo tem de ser específico para o domínio de aplicação para determinar palavras e expressões que é relevante, bem como típico word ordenação. 

O modelo inclui intenção, entidades. 

## <a name="add-training-examples"></a>Adicionar exemplos de formação
LUIS tem utterances de exemplo nos pendentes. Os exemplos tem suficiente variação de opção do word e ordem de word ser capaz de determinar que objetivo a utterance destinam-se. Cada utterance de exemplo tem de ter todos os dados necessários identificados como entidades. 

Instruir LUIS para ignorar utterances que não são relevantes para o domínio da sua aplicação através da atribuição utterance para o **nenhum** intenção. Qualquer palavras ou expressões que não precisa de solicitados fora de um utterance não tem de ser de etiqueta. Não há qualquer etiqueta para palavras ou frases reconhecíveis para ignorar. 
<!--
## Not just yet
Do not add features such as a [phrase list](luis-concept-feature.md) feature in your first cycle. Phrase lists are phrases that would be specific to your app's subject area.  
-->
## <a name="train-and-publish-the-app"></a>Dar formação e publicar a aplicação
Assim que tiver utterances diferentes de 10 a 15 em cada intenção, com as entidades necessárias com a etiqueta, a formação LUIS, em seguida, publicar ao obter pontos finais da sua. Certifique-se criar a sua aplicação e publicar a aplicação para que o se encontra disponível na [regiões de ponto final](luis-reference-regions.md) precisa. 

## <a name="https-endpoint-testing"></a>Teste de ponto final HTTPS
Pode testar a sua aplicação de LUIS de ponto final de HTTPS listada no **[publicar](publishapp.md)** página. Teste do ponto final permite LUIS escolher qualquer utterances com confiança de baixa para revisão.  

## <a name="recycle"></a>Reciclagem
Quando tiver terminado com um ciclo de criação, pode começar novamente. Comece por ler utterances de ponto final que Luis marcado com confiança baixa. Verifique estes utterances para intenção e entidade. Depois de rever utterances, a lista de revisão deve estar vazia.  

## <a name="batch-testing"></a>Teste do batch
Teste do batch é uma forma de ver quantas utterances de exemplo são classificadas por LUIS. Os exemplos devem estar familiarizados com LUIS e devem ser corretamente com a etiqueta com o objetivo e entidades que pretender LUIS localizar. Os resultados do teste indicam o quão bem LUIS executará no conjunto de utterances. 

## <a name="next-steps"></a>Passos Seguintes

Conheça os conceitos [colaboração](luis-concept-collaborator.md).

[luis-reference-prebuilt-domains]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-prebuilt-domains