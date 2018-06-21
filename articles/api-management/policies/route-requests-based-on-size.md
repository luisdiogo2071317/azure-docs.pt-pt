---
title: Azure API política de amostra de gestão - rota o pedido com base no tamanho do respetivo corpo | Microsoft Docs
description: Exemplo de política de gestão de Azure API - demonstra como encaminhar pedidos com base no tamanho do respetivos corpos.
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
ms.openlocfilehash: a93e1d9fecea59ebb68c512b96c8381b5b1a9346
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36284765"
---
# <a name="route-the-request-based-on-the-size-of-its-body"></a>O pedido com base no tamanho do respetivo corpo de rotas

Este artigo mostra um exemplo de política de gestão de API do Azure que demonstra como encaminhar pedidos com base no tamanho do respetivos corpos de. Para definir ou editar um código de política, siga os passos descritos no [conjunto ou editar uma política](../set-edit-policies.md). Para ver outros exemplos, consulte [amostras de política](../policy-samples.md).

## <a name="policy"></a>Política

Cole o código para o **entrada** bloco.

[!code-xml[Main](../../../api-management-policy-samples/examples/Route requests based on size.policy.xml)]

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as políticas de APIM:

+ [Políticas de transformação](../api-management-transformation-policies.md)
+ [Exemplos de política](../policy-samples.md)

