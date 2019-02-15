---
title: Mapeamento de transformação chave de substituição do fluxo de dados de fábrica de dados do Azure
description: Mapeamento de transformação chave de substituição do fluxo de dados de fábrica de dados do Azure
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: 62e9879f6bc75f042669ecb931ca7459d1317cc7
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/14/2019
ms.locfileid: "56272090"
---
# <a name="azure-data-factory-mapping-data-flow-surrogate-key-transformation"></a>Mapeamento de transformação chave de substituição do fluxo de dados de fábrica de dados do Azure

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Utilize a transformação de chave de substituição para adicionar um valor chave arbitrário não comerciais incrementação para o seu conjunto de linhas de fluxo de dados. Isto é útil ao criar tabelas de dimensões de um modelo de dados analíticos de esquema em estrela onde cada membro das tabelas de dimensão tem de ter uma chave exclusiva que é parte fundamental, não comerciais da metodologia Kimball armazém de dados.

![Transformação de chave de substituição](media/data-flow/surrogate.png "transformação de chave de substituição")

"Coluna de chave" é o nome que vai dar para a nova coluna de chave de substituição.

"Começar o valor" é o ponto de início do valor incremental.
