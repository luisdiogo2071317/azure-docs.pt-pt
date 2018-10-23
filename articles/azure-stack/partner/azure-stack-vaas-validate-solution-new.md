---
title: Validar uma nova solução do Azure Stack | Documentos da Microsoft
description: Saiba como validar uma nova solução do Azure Stack com validação como um serviço.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 10/19/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 777609b89bc08cd61489d2c3a3669ec07ccbc372
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/23/2018
ms.locfileid: "49647019"
---
# <a name="validate-a-new-azure-stack-solution"></a>Validar uma nova solução do Azure Stack

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Saiba como utilizar o **validação de solução** fluxo de trabalho para certificar novas soluções do Azure Stack.

Uma solução do Azure Stack é uma lista de hardware de materiais (LM) que tem sido em conjunto, faz um acordo entre a Microsoft e o parceiro depois de cumprir os requisitos de certificação de logótipo do Windows Server. Uma solução tem de ser recertified quando tiver ocorrido uma alteração ao hardware BoM. Para fazer outras perguntas sobre quando voltar a certificar o soluções, contacte a equipa em [ vaashelp@microsoft.com ](mailto:vaashelp@microsoft.com).

Para certificar a sua solução, executada duas vezes o fluxo de trabalho de validação de solução. Executá-lo uma vez para o *minimamente* suportado de configuração. Executá-lo uma segunda vez para o *maximally* suportado de configuração. Microsoft certifica-se que a solução se ambas as configurações de passar em todos os testes.

[!INCLUDE [azure-stack-vaas-workflow-validation-completion](includes/azure-stack-vaas-workflow-validation-completion.md)]

## <a name="create-a-solution-validation-workflow"></a>Criar um fluxo de trabalho de validação de solução

1. [!INCLUDE [azure-stack-vaas-workflow-step_select-solution](includes/azure-stack-vaas-workflow-step_select-solution.md)]
2. Selecione **começar** sobre o **validações de solução** mosaico.

    ![Mosaico de fluxo de trabalho de validações de solução](media/tile_validation-solution.png)

3. [!INCLUDE [azure-stack-vaas-workflow-step_naming](includes/azure-stack-vaas-workflow-step_naming.md)]
4. Selecione o **configuração da solução**.
    - **Mínimo**: a solução está configurada com o número mínimo suportado de nós.
    - **Máximo**: a solução está configurada com o número máximo suportado de nós.
5. [!INCLUDE [azure-stack-vaas-workflow-step_upload-stampinfo](includes/azure-stack-vaas-workflow-step_upload-stampinfo.md)]

    ![Informações de validação de solução](media/workflow_validation-solution_info.png)

6. [!INCLUDE [azure-stack-vaas-workflow-step_test-params](includes/azure-stack-vaas-workflow-step_test-params.md)]

    > [!NOTE]
    > Parâmetros do ambiente não podem ser modificados depois de criar um fluxo de trabalho.

7. [!INCLUDE [azure-stack-vaas-workflow-step_tags](includes/azure-stack-vaas-workflow-step_tags.md)]
8. [!INCLUDE [azure-stack-vaas-workflow-step_submit](includes/azure-stack-vaas-workflow-step_submit.md)]
    Será redirecionado para a página de resumo de testes.

## <a name="execute-solution-validation-tests"></a>Executar testes de validação de solução

Na **resumo de testes de validação de solução** página, verá uma lista de testes necessários para concluir a validação.

[!INCLUDE [azure-stack-vaas-workflow-validation-section_schedule](includes/azure-stack-vaas-workflow-validation-section_schedule.md)]

![Teste de validação de solução de agendamento](media/workflow_validation-solution_schedule-test.png)

## <a name="next-steps"></a>Passos Seguintes

- [Monitorizar e gerir os testes no portal do VaaS](azure-stack-vaas-monitor-test.md)