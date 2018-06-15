---
title: Política de gestão de API do Azure de exemplo - adicionar capacidades para um serviço de back-end | Microsoft Docs
description: Exemplo de política de gestão de Azure API - demonstra como adicionar capacidades para um serviço de back-end. Por exemplo, aceite um nome do lugar em vez de latitude e longitude numa previsão de Meteorologia API.
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
ms.openlocfilehash: 3ae168473b4fa603afaf3dcf42516687b4dde33e
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
ms.locfileid: "33933304"
---
# <a name="add-capabilities-to-a-backend-service"></a>Adicionar capacidades para um serviço de back-end

Este artigo mostra um exemplo de política de gestão de API do Azure que demonstra como adicionar capacidades para um serviço de back-end. Por exemplo, aceite um nome do lugar em vez de latitude e longitude numa previsão de Meteorologia API. Para definir ou editar um código de política, siga os passos descritos no [conjunto ou editar uma política](../set-edit-policies.md). Para ver outros exemplos, consulte [amostras de política](../policy-samples.md).

## <a name="policy"></a>Política

Cole o código para o **entrada** bloco.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Call out to an HTTP endpoint and cache the response.policy.xml)]

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as políticas de APIM:

+ [Políticas de transformação](../api-management-transformation-policies.md)
+ [Exemplos de política](../policy-samples.md)

