---
title: Reveja o texto Moderator conteúdo do Azure | Microsoft Docs
description: Saiba como consultar texto Moderator conteúdo para ver o respetivo modelo de pontuação e detetado etiquetas. Utilize as informações para detemine se o conteúdo é adequado.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 02/03/2017
ms.author: sajagtap
ms.openlocfilehash: cb8774395b7374677e8de3b80630a2d4abf490f9
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351517"
---
# <a name="review-text"></a>Rever texto

Pode utilizar Moderator conteúdo do Azure para rever o texto utilizando pontuações e etiquetas não detetada. Utilize as informações para determinar se o conteúdo é apropriado. 

## <a name="select-or-enter-the-text-to-review"></a>Selecione ou introduza o texto para rever

No conteúdo Moderator, selecione o **tente** separador. Em seguida, selecione o **texto** opção para aceder ao ecrã de início de moderação interrupção de texto. Introduza a qualquer texto ou submeter o texto de exemplo predefinido de moderação de interrupção de texto automatizada. Pode introduzir um máximo de 1024 caracteres.

## <a name="get-ready-to-review-results"></a>Preparar-se de que reveja os resultados

A ferramenta de revisão primeiro chama a API de moderação interrupção de texto. Em seguida, gera revisões de texto, utilizando as etiquetas detetadas. A ferramenta de revisão corresponda os resultados de pontuação para atenção da equipa.

## <a name="review-text-results"></a>Reveja os resultados de texto

Resultados detalhados são apresentados do windows. Os resultados incluem as etiquetas detetadas e termos de licenciamento que foram devolvidos pela API de moderação interrupção de texto. Para alternar o estado de seleção de uma etiqueta, selecione a etiqueta. Também pode trabalhar com as etiquetas personalizadas que pode ter criado.

![Reveja os resultados de texto](images/3-review-text-2.png)
