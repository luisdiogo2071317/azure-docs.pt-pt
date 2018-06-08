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
ms.openlocfilehash: cce2376c440c233d81198d903709867c5fdca7ec
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/07/2018
ms.locfileid: "34839888"
---
# <a name="authorize-requests-using-external-authorizer"></a>Autorizar pedidos utilizando authorizer externo

Este artigo mostra um exemplo de política de gestão de API do Azure que demonstra como proteger o acesso a API utilizando um authorizer externo ao encapsular a lógica de autenticação/autorização personalizada. Para definir ou editar um código de política, siga os passos descritos no [conjunto ou editar uma política](../set-edit-policies.md). Para ver outros exemplos, consulte [amostras de política](../policy-samples.md).

## <a name="policy"></a>Política

Cole o código para o **entrada** bloco.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Authorize requests using external authorizer.policy.xml)]

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as políticas de APIM:

+ [Políticas de restrições de acesso](../api-management-access-restriction-policies.md)
+ [Exemplos de política](../policy-samples.md)