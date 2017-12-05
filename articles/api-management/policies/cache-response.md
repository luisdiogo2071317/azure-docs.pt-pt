---
title: "Política de gestão de API do Azure de exemplo - adicionar capacidades para um serviço de back-end | Microsoft Docs"
description: "Exemplo de política de gestão de Azure API - demonstra como adicionar capacidades para um serviço de back-end. Por exemplo, aceite um nome do lugar em vez de latitude e longitude numa previsão de Meteorologia API."
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
ms.openlocfilehash: d500df0f0e48134ac9c1397795d65706d2e56ff9
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2017
---
# <a name="add-capabilities-to-a-backend-service"></a>Adicionar capacidades para um serviço de back-end

Este artigo mostra um exemplo de política de gestão de API do Azure que demonstra como adicionar capacidades para um serviço de back-end. Por exemplo, aceite um nome do lugar em vez de latitude e longitude numa previsão de Meteorologia API. Para definir ou editar um código de política, siga os passos descritos no [conjunto ou editar uma política](../set-edit-policies.md). Para ver outros exemplos, consulte [amostras de política](../policy-samples.md).

## <a name="policy"></a>Política

Cole o código para o **entrada** bloco.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Call out to an HTTP endpoint and cache the response.policy.xml)]

## <a name="next-steps"></a>Passos seguintes

Saiba mais sobre as políticas de APIM:

+ [Políticas de transformação](../api-management-transformation-policies.md)
+ [Exemplos de política](../policy-samples.md)

