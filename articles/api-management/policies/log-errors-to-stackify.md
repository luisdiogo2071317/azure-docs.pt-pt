---
title: Azure API política de amostra de gestão - erros de envio para Stackify para registo | Microsoft Docs
description: Exemplo de política de gestão de Azure API - demonstra como adicionar uma política de registo de erro a enviar erros ao Stackify para registo...
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
ms.openlocfilehash: 5daf21cb55289c874d56910b1240e1433f3d55d0
ms.sourcegitcommit: e34afd967d66aea62e34d912a040c4622a737acb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/26/2018
ms.locfileid: "36945960"
---
# <a name="send-errors-to-stackify-for-logging"></a>Enviar erros ao Stackify para registo

Este artigo mostra um exemplo de política de gestão de API do Azure que demonstra como adicionar uma política de registo de erro a enviar erros ao Stackify para registo. Para definir ou editar um código de política, siga os passos descritos no [conjunto ou editar uma política](../set-edit-policies.md). Para ver outros exemplos, consulte [amostras de política](../policy-samples.md).

## <a name="policy"></a>Política

Cole o código para o **no erro** bloco.

[!code-xml[Main](../../../api-management-policy-samples/examples/Log errors to Stackify.policy.xml)]

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as políticas de APIM:

+ [Políticas de transformação](../api-management-transformation-policies.md)
+ [Exemplos de política](../policy-samples.md)

