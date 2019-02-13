---
title: Nó de referência de fluxo de dados de mapeamento de fábrica de dados do Azure
description: Fluxo de dados de fábrica de dados, irá adicionar um nó de referência para associações, pesquisas, uniões
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/31/2019
ms.openlocfilehash: 641375c2b848957ffc0f5ad092a28460d91b6690
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56213566"
---
# <a name="azure-data-factory-mapping-data-flow-reference-node"></a>Nó de referência de fluxo de dados de mapeamento de fábrica de dados do Azure

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Nó de referência](media/data-flow/referencenode.png "nó de referência")

Um nó de referência é automaticamente adicionado à tela para indicar que o nó que está ligado a faz referência a outro nó existente na tela. Pense num nó de referência como um ponteiro ou uma referência a outra transformação de fluxo de dados.

Por exemplo: Quando participar ou união mais de um fluxo de dados, a tela de fluxo de dados pode adicionar um nó de referência que reflete o nome e as definições do fluxo de entrada não principal.

O nó de referência não pode ser movido ou eliminado. No entanto, pode clicar para o nó para modificar as definições de transformação de origem.

As regras de interface do Usuário que regem quando o fluxo de dados adiciona o nó de referência baseiam-se no espaço disponível e espaçamento vertical entre as linhas.
