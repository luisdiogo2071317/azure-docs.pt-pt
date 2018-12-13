---
title: Exemplo de Azure política de gestão do API - erros de envio para o Stackify para registro em log | Documentos da Microsoft
description: Exemplo de política do gestão de API do Azure - demonstra como adicionar uma política de registo de erro a enviar erros ao Stackify para registo....
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
ms.openlocfilehash: 07cc83830fe2d467c611622bb66dfbb8c9429c2d
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52864881"
---
# <a name="send-errors-to-stackify-for-logging"></a>Enviar erros ao Stackify para registo

Este artigo mostra um exemplo de política de gestão de API do Azure que demonstra como adicionar uma política de registo de erro a enviar erros ao Stackify para registo. Para definir ou editar um código de política, siga os passos descritos em [conjunto ou editar uma política](../set-edit-policies.md). Para ver outros exemplos, consulte [exemplos de política](../policy-samples.md).

## <a name="policy"></a>Política

Cole o código para o **no erro** bloco.

[!code-xml[Main](../../../api-management-policy-samples/examples/Log errors to Stackify.policy.xml)]

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as políticas APIM:

+ [Políticas de transformação](../api-management-transformation-policies.md)
+ [Exemplos de política](../policy-samples.md)

