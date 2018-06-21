---
title: Política de gestão de API do Azure de exemplo - autorizar a pedido utilizando o authorizer externo | Microsoft Docs
description: Exemplo de política de gestão de Azure API - demonstra como autorizar pedidos utilizando authorizer externo ao encapsular uma lógica de autenticação/autorização personalizado ou legado.
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
ms.date: 06/06/2018
ms.author: apimpm
ms.openlocfilehash: 7d172a40f2aad65b595026fc656634060a1f7193
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36284877"
---
# <a name="authorize-requests-using-external-authorizer"></a>Autorizar pedidos utilizando authorizer externo

Este artigo mostra um exemplo de política de gestão de API do Azure que demonstra como proteger o acesso a API utilizando um authorizer externo ao encapsular a lógica de autenticação/autorização personalizada. Para definir ou editar um código de política, siga os passos descritos no [conjunto ou editar uma política](../set-edit-policies.md). Para ver outros exemplos, consulte [amostras de política](../policy-samples.md).

## <a name="policy"></a>Política

Cole o código para o **entrada** bloco.

[!code-xml[Main](../../../api-management-policy-samples/examples/Authorize requests using external authorizer.policy.xml)]

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as políticas de APIM:

+ [Políticas de restrições de acesso](../api-management-access-restriction-policies.md)
+ [Exemplos de política](../policy-samples.md)