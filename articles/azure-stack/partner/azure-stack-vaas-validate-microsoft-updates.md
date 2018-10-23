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
ms.date: 10/19/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 7fcc7d5a1d87fe93d32772dbbb84f1d3c91d5631
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/23/2018
ms.locfileid: "49648790"
---
# <a name="validate-software-updates-from-microsoft"></a>Validar as atualizações de software da Microsoft

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

A Microsoft lançará periodicamente atualizações para o software do Azure Stack. Estas atualizações são fornecidas para parceiros de engenharia conjuntas do Azure Stack antecedência a ser disponibilizadas publicamente para que eles possam validar as atualizações em suas soluções e fornecer comentários à Microsoft.

[!INCLUDE [azure-stack-vaas-workflow-validation-completion](includes/azure-stack-vaas-workflow-validation-completion.md)]

## <a name="apply-monthly-update"></a>Aplicar atualização mensal

[!INCLUDE [azure-stack-vaas-workflow-section_update-azs](includes/azure-stack-vaas-workflow-section_update-azs.md)]

## <a name="create-a-workflow"></a>Criar um fluxo de trabalho

Utilizam o mesmo fluxo de trabalho como validações de atualização **validação do pacote**. Siga as instruções em [criar um fluxo de trabalho de validação do pacote](azure-stack-vaas-validate-oem-package.md#create-a-package-validation-workflow).

## <a name="run-tests"></a>Executar testes

Utilizam o mesmo fluxo de trabalho como validações de atualização **validação do pacote**. Siga as instruções em [testes de validação do pacote executar](azure-stack-vaas-validate-oem-package.md#run-package-validation-tests).

Não é necessário pedir para validações de atualização de assinatura do pacote.

## <a name="next-steps"></a>Passos Seguintes

- [Monitorizar e gerir os testes no portal do VaaS](azure-stack-vaas-monitor-test.md)
