---
title: Mapeamento de transformação de pesquisa de fluxo de dados de fábrica de dados do Azure
description: Mapeamento de transformação de pesquisa de fluxo de dados de fábrica de dados do Azure
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: 28c8f2b0005641934f7fcd9549f1cf004b206d80
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/14/2019
ms.locfileid: "56271995"
---
# <a name="azure-data-factory-mapping-data-flow-lookup-transformation"></a>Mapeamento de transformação de pesquisa de fluxo de dados de fábrica de dados do Azure

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Utilize Lookup para adicionar dados de referência de outra origem para o seu fluxo de dados. A transformação de pesquisa requer uma origem definida que aponta para a tabela de referência e faz a correspondência em campos de chave.

![Transformação lookup](media/data-flow/lookup1.png "pesquisa")

Selecione os campos de chave que deseja correspondência com entre os campos de fluxo de entrada e os campos da origem de referência. Primeiro tem de criar uma nova origem da tela de design de fluxo de dados para utilizar como à direita para a pesquisa.

Quando forem encontradas correspondências, resultante de linhas e colunas da origem de referência serão adicionadas ao seu fluxo de dados. Pode escolher quais os campos de interesse que deseja incluir no seu coletor no final do seu fluxo de dados.
