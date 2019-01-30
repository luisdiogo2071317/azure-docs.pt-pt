---
title: Práticas recomendadas de validação do Azure Stack. | Microsoft Docs
description: Este artigo tem as práticas recomendadas para a validação como um serviço.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/26/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 11/26/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 2fa0912d120760ddba3b3d21f718e214e9d7c26b
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/30/2019
ms.locfileid: "55238929"
---
# <a name="best-practices-for-validation-as-a-service"></a>Melhores práticas para a validação como um serviço

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Este artigo abrange as melhores práticas para gerir recursos na validação como um serviço (VaaS). Para uma descrição geral dos recursos de VaaS, consulte [validação como um conceito de principais de serviço](azure-stack-vaas-key-concepts.md).

## <a name="solution-management"></a>Gerenciamento de solução

### <a name="naming-convention-for-vaas-solutions"></a>Convenção de nomenclatura para soluções de VaaS

Utilize uma convenção de nomenclatura consistente para todas as soluções registadas no VaaS. Por exemplo, o nome da solução pode ser criado a partir as propriedades de hardware abaixo da seguinte forma:

|Nome do Produto | Elemento de Hardware exclusivos 1 | Elemento de Hardware exclusivos 2 | Nome da solução
|---|---|---|---|
Minha solução XYZ |  Todos os Flash | Meu comutador X01 | MySolutionXYZ_AllFlash_MySwitchX01

### <a name="when-to-create-a-new-vaas-solution"></a>Quando criar uma nova solução de VaaS

Utilize a mesma solução de VaaS na execução de fluxos de trabalho no mesmo hardware SKU. Uma nova solução de VaaS deve ser criada apenas quando existe uma alteração para o SKU de hardware.

## <a name="workflow-management"></a>Gerenciamento de fluxo de trabalho

### <a name="naming-convention-for-vaas-workflows"></a>Convenção de nomenclatura para VaaS fluxos de trabalho

Utilize uma convenção de nomenclatura consistente para todas as execuções de fluxo de trabalho de VaaS. Por exemplo, a construção de um fluxo de trabalho o nome das propriedades de compilação abaixo da seguinte forma:

|(Principal) do número de compilação | Date | Tamanho de solução | Nome do Fluxo de Trabalho
|---|---|---| ---|
1808 | 081518 | 4NODE | 1808_081518_4NODE

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre [validação como um conceito de principais de serviço](azure-stack-vaas-key-concepts.md)
