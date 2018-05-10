---
title: Exemplo de política de gestão de API do Azure - adicionar um cabeçalho que contém um id de correlação | Microsoft Docs
description: Exemplo de política de gestão de Azure API - demonstra como adicionar um cabeçalho que contém um id de correlação para o pedido de entrada.
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
ms.openlocfilehash: 7d339ba08720d482f622aa9b708d3a6e057eaaa8
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="add-a-header-containing-a-correlation-id"></a>Adicionar um cabeçalho que contém um id de correlação

Este artigo mostra um exemplo de política de gestão de API do Azure que demonstra como adicionar um cabeçalho que contém um id de correlação para o pedido de entrada. Para definir ou editar um código de política, siga os passos descritos no [conjunto ou editar uma política](../set-edit-policies.md). Para ver outros exemplos, consulte [amostras de política](../policy-samples.md).

## <a name="policy"></a>Política

Cole o código para o **entrada** bloco.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Add correlation id to inbound request.policy.xml)]

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as políticas de APIM:

+ [Políticas de transformação](../api-management-transformation-policies.md)
+ [Exemplos de política](../policy-samples.md)

