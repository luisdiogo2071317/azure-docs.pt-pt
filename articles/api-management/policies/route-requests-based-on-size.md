---
title: "Azure API política de amostra de gestão - rota o pedido com base no tamanho do respetivo corpo | Microsoft Docs"
description: "Exemplo de política de gestão de Azure API - demonstra como encaminhar pedidos com base no tamanho do respetivos corpos."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/13/2017
ms.author: apimpm
ms.openlocfilehash: 0558f82a31015af91df135b6a0d4c4050049d11d
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2017
---
# <a name="route-the-request-based-on-the-size-of-its-body"></a>O pedido com base no tamanho do respetivo corpo de rotas

Este artigo mostra um exemplo de política de gestão de API do Azure que demonstra como encaminhar pedidos com base no tamanho do respetivos corpos de. Para definir ou editar um código de política, siga os passos descritos no [conjunto ou editar uma política](../set-edit-policies.md). Para ver outros exemplos, consulte [amostras de política](../policy-samples.md).

## <a name="policy"></a>Política

Cole o código para o **entrada** bloco.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Route requests based on size.policy.xml)]

## <a name="next-steps"></a>Passos seguintes

Saiba mais sobre as políticas de APIM:

+ [Políticas de transformação](../api-management-transformation-policies.md)
+ [Exemplos de política](../policy-samples.md)

