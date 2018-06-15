---
title: Azure API política de amostra de gestão - conteúdo de resposta do filtro | Microsoft Docs
description: Exemplo de política de gestão de Azure API - demonstra como elementos de dados do payload de resposta com base no produto associado ao pedido de filtro.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2017
ms.author: apimpm
ms.openlocfilehash: f6475b272239e9352211a80985e1b46da9c6f8e0
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
ms.locfileid: "33933433"
---
# <a name="filter-response-content"></a>Filtrar o conteúdo da resposta

Este artigo mostra um exemplo de política de gestão de API do Azure que demonstra como elementos de dados do payload de resposta com base no produto associado ao pedido de filtro. Para definir ou editar um código de política, siga os passos descritos no [conjunto ou editar uma política](../set-edit-policies.md). Para ver outros exemplos, consulte [amostras de política](../policy-samples.md).

## <a name="policy"></a>Política

Cole o código para o **saída** bloco.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Filter response content based on product name.policy.xml)]

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as políticas de APIM:

+ [Políticas de transformação](../api-management-transformation-policies.md)
+ [Exemplos de política](../policy-samples.md)

