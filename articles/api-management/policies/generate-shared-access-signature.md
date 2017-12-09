---
title: "Azure API política de amostra de gestão - gerar assinatura de acesso partilhado | Microsoft Docs"
description: "Exemplo de política de gestão de Azure API - demonstra como gerar a assinatura de acesso partilhado utilizando expressões e reencaminhar o pedido para o armazenamento do Azure com a política de uri de reescrever..."
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
ms.openlocfilehash: 9b0d37e4f7930389d3399e51de905db2b2ce8c27
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2017
---
# <a name="generate-shared-access-signature"></a>Gerar a assinatura de acesso partilhado

Este artigo mostra um exemplo de política de gestão de API do Azure que demonstra como gerar [assinatura de acesso partilhado](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) utilizando expressões e reencaminhá-los o pedido para o armazenamento do Azure com a política de reescrever-uri. Para definir ou editar um código de política, siga os passos descritos no [conjunto ou editar uma política](../set-edit-policies.md). Para ver outros exemplos, consulte [amostras de política](../policy-samples.md).

## <a name="policy"></a>Política

Cole o código para o **entrada** bloco.

[!code-xml[Main](../../../api-management-policy-samples/Snippets/Generate Shared Access Signature and forward request to Azure storage.policy.xml)]

## <a name="next-steps"></a>Passos seguintes

Saiba mais sobre as políticas de APIM:

+ [Políticas de transformação](../api-management-transformation-policies.md)
+ [Exemplos de política](../policy-samples.md)

