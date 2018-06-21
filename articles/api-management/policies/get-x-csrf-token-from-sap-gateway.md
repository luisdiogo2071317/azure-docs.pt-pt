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
ms.openlocfilehash: 2e9f51cc2afadf70c02a23a85a4c2a866f3fd149
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36285475"
---
# <a name="implement-x-csrf-pattern"></a>Padrão de implementar X-CSRF

Este artigo mostra um exemplo de política de gestão de API do Azure que demonstra como implementar o padrão de X CSRF utilizado por muitas APIs. Este exemplo é específico do Gateway SAP. Para definir ou editar um código de política, siga os passos descritos no [conjunto ou editar uma política](../set-edit-policies.md). Para ver outros exemplos, consulte [amostras de política](../policy-samples.md).

## <a name="policy"></a>Política

Cole o código para o **entrada** bloco.

[!code-xml[Main](../../../api-management-policy-samples/examples/Get X-CSRF token from SAP gateway using send request policy.xml)]

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as políticas de APIM:

+ [Políticas de transformação](../api-management-transformation-policies.md)
+ [Exemplos de política](../policy-samples.md)

