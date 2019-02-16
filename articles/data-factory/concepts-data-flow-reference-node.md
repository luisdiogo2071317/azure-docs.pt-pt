---
title: Nó de referência de fluxo de dados de mapeamento de fábrica de dados do Azure
description: Fluxo de dados de fábrica de dados, irá adicionar um nó de referência para associações, pesquisas, uniões
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/31/2019
ms.openlocfilehash: 2e318210d96822b13f65eadeef79798b1b4595d1
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/16/2019
ms.locfileid: "56325902"
---
# <a name="mapping-data-flow-reference-node"></a>Nó de referência de fluxo de dados de mapeamento

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Nó de referência](media/data-flow/referencenode.png "nó de referência")

Um nó de referência é automaticamente adicionado à tela para indicar que o nó que está ligado a faz referência a outro nó existente na tela. Pense num nó de referência como um ponteiro ou uma referência a outra transformação de fluxo de dados.

Por exemplo: Quando participar ou união mais de um fluxo de dados, a tela de fluxo de dados pode adicionar um nó de referência que reflete o nome e as definições do fluxo de entrada não principal.

O nó de referência não pode ser movido ou eliminado. No entanto, pode clicar para o nó para modificar as definições de transformação de origem.

As regras de interface do Usuário que regem quando o fluxo de dados adiciona o nó de referência baseiam-se no espaço disponível e espaçamento vertical entre as linhas.
