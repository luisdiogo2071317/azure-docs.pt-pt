---
title: Exemplo de política do gestão de API do Azure - definir a duração da cache de resposta | Documentos da Microsoft
description: 'Exemplo de política do gestão de API do Azure - demonstra como definir a duração da cache de resposta com o valor: maxAge no cabeçalho Cache-Control enviado pelo back-end....'
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
ms.openlocfilehash: 042fab72da2d4b890314b6ee9c7237241b492fba
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52869233"
---
# <a name="set-response-cache-duration"></a>Definir a duração da cache de resposta

Este artigo mostra um exemplo de política de gestão de API do Azure que demonstra como definir a duração da cache de resposta com o valor: maxAge no cabeçalho Cache-Control enviado pelo back-end. Para definir ou editar um código de política, siga os passos descritos em [conjunto ou editar uma política](../set-edit-policies.md). Para ver outros exemplos, consulte [exemplos de política](../policy-samples.md).

## <a name="policy"></a>Política

Cole o código para o **entrada** bloco.

[!code-xml[Main](../../../api-management-policy-samples/examples/Set cache duration using response cache control header.policy.xml)]

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as políticas APIM:

+ [Políticas de transformação](../api-management-transformation-policies.md)
+ [Exemplos de política](../policy-samples.md)

