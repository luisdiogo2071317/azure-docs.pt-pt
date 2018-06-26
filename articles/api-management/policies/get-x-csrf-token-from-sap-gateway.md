---
title: Azure API política de amostra de gestão - padrão de implementar X-CSRF | Microsoft Docs
description: Exemplo de política de gestão de Azure API - demonstra como implementar o padrão de X CSRF utilizado por muitas APIs. Este exemplo é específico do Gateway SAP.
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
ms.openlocfilehash: 3a2067836a1488d117dced96f3935f2d1f8b1b48
ms.sourcegitcommit: e34afd967d66aea62e34d912a040c4622a737acb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/26/2018
ms.locfileid: "36946028"
---
# <a name="implement-x-csrf-pattern"></a>Padrão de implementar X-CSRF

Este artigo mostra um exemplo de política de gestão de API do Azure que demonstra como implementar o padrão de X CSRF utilizado por muitas APIs. Este exemplo é específico do Gateway SAP. Para definir ou editar um código de política, siga os passos descritos no [conjunto ou editar uma política](../set-edit-policies.md). Para ver outros exemplos, consulte [amostras de política](../policy-samples.md).

## <a name="policy"></a>Política

Cole o código para o **entrada** bloco.

[!code-xml[Main](../../../api-management-policy-samples/examples/Get X-CSRF token from SAP gateway using send request.policy.xml)]

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as políticas de APIM:

+ [Políticas de transformação](../api-management-transformation-policies.md)
+ [Exemplos de política](../policy-samples.md)

