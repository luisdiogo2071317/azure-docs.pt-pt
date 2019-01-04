---
title: Definir variável atividade na fábrica de dados do Azure | Documentos da Microsoft
description: Saiba como utilizar a atividade definir variável para definir o valor de uma variável existente definido no pipeline do Data Factory
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: douglasl
ms.openlocfilehash: cc573028779bcd6b77394bbeefbea58f714b835c
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/04/2019
ms.locfileid: "54017349"
---
# <a name="set-variable-activity-in-azure-data-factory"></a>Atividade de variável de conjunto de Azure Data Factory

Utilize a atividade definir variável para definir o valor de uma variável existente do tipo cadeia, booleano ou matriz definidos no pipeline do Data Factory.

## <a name="type-properties"></a>Propriedades do tipo

Propriedade | Descrição | Necessário
-------- | ----------- | --------
nome | Nome da atividade no pipeline | Sim
descrição | Texto que descreve o que faz a atividade | não
tipo | Tipo de atividade é SetVariable | sim
valor | Cadeia de caracteres literal ou expressão de valor de objeto usado para definir a variável especificada | sim
Nomedavariável | Nome da variável que esta atividade definirá | sim


## <a name="next-steps"></a>Passos Seguintes
Saiba mais sobre uma atividade de fluxo de controle relacionados suportada pelo Data Factory: 

- [Acrescentar atividade variável](control-flow-append-variable-activity.md)
