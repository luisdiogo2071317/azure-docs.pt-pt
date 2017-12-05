---
title: "Exemplo de política de gestão de API do Azure - adicionar um cabeçalho de Forwarded | Microsoft Docs"
description: "Exemplo de política de gestão de Azure API - demonstra como adicionar um cabeçalho de Forwarded do pedido de entrada para permitir que o back-end da API para construir o URL correto."
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
ms.openlocfilehash: cc2df914532b6cda37c951b65b243e90b63d57cb
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2017
---
# <a name="add-a-forwarded-header"></a>Adicionar um cabeçalho de Forwarded

Este artigo mostra um exemplo de política de gestão de API do Azure que demonstra como adicionar um cabeçalho de Forwarded do pedido de entrada para permitir que o back-end da API para construir o URL correto. Para definir ou editar um código de política, siga os passos descritos no [conjunto ou editar uma política](../set-edit-policies.md). Para ver outros exemplos, consulte [amostras de política](../policy-samples.md).

## <a name="code"></a>Código

Cole o código para o **entrada** bloco.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Forward gateway hostname to backend for generating correct urls in responses.policy.xml)]

## <a name="next-steps"></a>Passos seguintes

Saiba mais sobre as políticas de APIM:

+ [Políticas de transformação](../api-management-transformation-policies.md)
+ [Exemplos de política](../policy-samples.md)
