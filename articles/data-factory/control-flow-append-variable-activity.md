---
title: Acrescentar atividade variável na fábrica de dados do Azure | Documentos da Microsoft
description: Saiba como configurar a atividade de acrescentar variável para adicionar um valor para uma variável de matriz existente definida no pipeline do Data Factory
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/09/2018
ms.author: douglasl
ms.openlocfilehash: e904075908fe7108c0566856b25fe03be0b7fd86
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/04/2019
ms.locfileid: "54023812"
---
# <a name="append-variable-activity-in-azure-data-factory"></a>Acrescentar atividade variável na fábrica de dados do Azure

Utilize a atividade de acrescentar variável para adicionar um valor para uma variável de matriz existente definida no pipeline do Data Factory.

## <a name="type-properties"></a>Propriedades do tipo

Propriedade | Descrição | Necessário
-------- | ----------- | --------
nome | Nome da atividade no pipeline | Sim
descrição | Texto que descreve o que faz a atividade | não
tipo | Tipo de atividade é AppendVariable | sim
valor | Cadeia de caracteres literal ou expressão de valor de objeto utilizado para acrescentar numa variável especificada | sim
Nomedavariável | Nome da variável que será modificada pela atividade, a variável tem de ser do tipo "Array" | sim

## <a name="next-steps"></a>Passos Seguintes
Saiba mais sobre uma atividade de fluxo de controle relacionados suportada pelo Data Factory: 

- [Atividade de variável de conjunto](control-flow-set-variable-activity.md)
