---
title: Validar as atualizações de software da Microsoft no Azure Stack validação como um serviço | Documentos da Microsoft
description: Aprenda a validar as atualizações de software da Microsoft com a validação como um serviço.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 1/07/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: c90b81cdb6df5477d658d53dc93d0c3bbc47c1ff
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2019
ms.locfileid: "54106448"
---
# <a name="validate-software-updates-from-microsoft"></a>Validar as atualizações de software da Microsoft

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

A Microsoft lançará periodicamente atualizações para o software do Azure Stack. Estas atualizações são fornecidas para o Azure Stack coengineering parceiros. As atualizações são fornecidas com antecedência de publicamente disponíveis. Pode verificar as atualizações em relação a sua solução e fornecer comentários à Microsoft.

[!INCLUDE [azure-stack-vaas-workflow-validation-completion](includes/azure-stack-vaas-workflow-validation-completion.md)]

## <a name="apply-monthly-update"></a>Aplicar atualização mensal

[!INCLUDE [azure-stack-vaas-workflow-section_update-azs](includes/azure-stack-vaas-workflow-section_update-azs.md)]

## <a name="create-a-workflow"></a>Criar um fluxo de trabalho

Utilizam o mesmo fluxo de trabalho como validações de atualização **validação de solução**.

## <a name="run-tests"></a>Executar testes

1. Utilizam o mesmo fluxo de trabalho como validações de atualização **validação de solução**. 

2. Siga as instruções em [testes de validação de solução executar](azure-stack-vaas-validate-oem-package.md#run-package-validation-tests). Selecione os seguintes testes em vez disso:
    - Verificação de atualização mensal do Azure Stack
    - Mecanismo de simulação de cloud

Não precisa de pedido de assinatura para validações de atualização do pacote.

## <a name="next-steps"></a>Passos Seguintes

- [Monitorizar e gerir os testes no portal do VaaS](azure-stack-vaas-monitor-test.md)