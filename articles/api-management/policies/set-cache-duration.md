---
title: Exemplo de política de gestão de Azure API - definir a duração da cache de resposta | Microsoft Docs
description: Exemplo de política de gestão de Azure API - demonstra como definir a duração da cache de resposta utilizando o valor de maxAge no cabeçalho de Cache-Control enviado pelo back-end...
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
ms.openlocfilehash: 8640668ae51c113cc467501b44dbd03b257325c3
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
ms.locfileid: "33937288"
---
# <a name="set-response-cache-duration"></a>Definir a duração da cache de resposta

Este artigo mostra um exemplo de política de gestão de API do Azure que demonstra como definir a duração da cache de resposta utilizando o valor de maxAge no cabeçalho de Cache-Control enviado pelo back-end. Para definir ou editar um código de política, siga os passos descritos no [conjunto ou editar uma política](../set-edit-policies.md). Para ver outros exemplos, consulte [amostras de política](../policy-samples.md).

## <a name="policy"></a>Política

Cole o código para o **entrada** bloco.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Set cache duration using response cache control header.policy.xml)]

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as políticas de APIM:

+ [Políticas de transformação](../api-management-transformation-policies.md)
+ [Exemplos de política](../policy-samples.md)

