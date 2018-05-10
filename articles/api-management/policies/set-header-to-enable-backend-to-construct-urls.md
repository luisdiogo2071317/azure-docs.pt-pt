---
title: Exemplo de política de gestão de API do Azure - adicionar um cabeçalho de Forwarded | Microsoft Docs
description: Exemplo de política de gestão de Azure API - demonstra como adicionar um cabeçalho de Forwarded do pedido de entrada para permitir que o back-end da API para construir o URL correto.
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
ms.openlocfilehash: 00c8ac567b476d0591069c83c371d987d651de9d
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="add-a-forwarded-header"></a>Adicionar um cabeçalho de Forwarded

Este artigo mostra um exemplo de política de gestão de API do Azure que demonstra como adicionar um cabeçalho de Forwarded do pedido de entrada para permitir que o back-end da API para construir o URL correto. Para definir ou editar um código de política, siga os passos descritos no [conjunto ou editar uma política](../set-edit-policies.md). Para ver outros exemplos, consulte [amostras de política](../policy-samples.md).

## <a name="code"></a>Código

Cole o código para o **entrada** bloco.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Forward gateway hostname to backend for generating correct urls in responses.policy.xml)]

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as políticas de APIM:

+ [Políticas de transformação](../api-management-transformation-policies.md)
+ [Exemplos de política](../policy-samples.md)
