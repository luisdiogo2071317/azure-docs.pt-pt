---
title: Reveja o texto no Azure Content Moderator - Content Moderator
description: Saiba como rever texto no Content Moderator para ver a sua pontuação e detetado etiquetas. Utilize as informações para determinar se o conteúdo é apropriado.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 896956653ed924b7e30e63c1ae5cfa90dd0cc94d
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55219792"
---
# <a name="let-human-reviewers-review-text"></a>Permitir que os revisores humanos, reveja o texto

Pode utilizar o Azure Content Moderator para analisar o texto ao utilizar as pontuações e detetado etiquetas. Utilize as informações para determinar se o conteúdo é adequado. 

## <a name="select-or-enter-the-text-to-review"></a>Selecione ou introduza o texto para rever

No Content Moderator, selecione o **tente** separador. Em seguida, selecione o **texto** opção para passar para o ecrã de início de moderação de texto. Digitar qualquer texto ou submeta o texto de exemplo predefinido para moderação de texto automática. Pode introduzir um máximo de 1024 carateres.

## <a name="get-ready-to-review-results"></a>Prepare-se de que reveja os resultados

A ferramenta de revisão primeiro chama a API de moderação de texto. Em seguida, gera revisões de texto, utilizando as etiquetas detetadas. A ferramenta de revisão coincide com os resultados de pontuação pela atenção de sua equipe.

## <a name="review-text-results"></a>Rever os resultados de texto

Resultados detalhados são apresentados no windows. Os resultados incluem etiquetas detetadas e os termos que foram devolvidos pela API de moderação de texto. Para alternar o estado de seleção de uma etiqueta, selecione a etiqueta. Também pode trabalhar com qualquer etiquetas personalizadas que pode ter criado.

![Captura de ecrã da apresentação da ferramenta de revisão sinalizados texto numa janela do browser Chrome](../images/reviewresults_text.png)
