---
title: Validar as atualizações de software da Microsoft na validação do Azure Stack como serviço | Documentos da Microsoft
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
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: a9e1bb2f134e0e51803266f5e7f94c05ab6966f2
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/20/2018
ms.locfileid: "40235511"
---
# <a name="validate-software-updates-from-microsoft"></a>Validar as atualizações de software da Microsoft

[!INCLUDE[Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

A Microsoft lançará periodicamente atualizações para o software do Azure Stack. Estas atualizações são fornecidas para parceiros de engenharia conjuntas do Azure Stack antecedência a ser disponibilizadas publicamente para que eles possam validar as atualizações em suas soluções e fornecer comentários à Microsoft.

## <a name="test-an-existing-solution"></a>Testar uma solução existente

1. Inicie sessão para o [portal de validação](https://azurestackvalidation.com).

2. Selecione uma solução existente em que a atualização da Microsoft foi implementada e selecione **começar** sobre o **validação do pacote** mosaico.

    ![Validação do pacote](media/image3.png)

3. Introduza o nome de validação.

4. Introduza o URL para o pacote de OEM que foi instalado na solução no momento da implementação. Utilize o URL para o pacote armazenado no serviço de Blobs do Azure. Para obter mais informações, consulte [criar um blob de armazenamento do Azure para armazenar registos](azure-stack-vaas-set-up-account.md#create-an-azure-storage-blob-to-store-logs).

5. Selecione **carregar** ao adicionar o ficheiro de configuração de implementação. Consulte a [validação de uma nova solução de pilha do Azure](azure-stack-vaas-validate-solution-new.md) para obter informações sobre a carregar o ficheiro de configuração de implementação.

6. O ficheiro de configuração de implementação, em seguida, tem de ser personalizados com o ficheiro de parâmetros de ambiente correto, consulte [parâmetros do ambiente](azure-stack-vaas-parameters.md#environment-parameters) para obter mais detalhes.

    > [!Note]   
    > O ficheiro de configuração de implementação pode ser personalizado ao adicionar parâmetros comuns de teste. Para obter mais informações, consulte [parâmetros comuns do fluxo de trabalho para a validação do Azure Stack como serviço](azure-stack-vaas-parameters.md)

7. O nome de utilizador e palavra-passe para o utilizador do inquilino, o administrador de serviço e o administrador da nuvem tem de ser introduzidos manualmente.

8. Forneça o URL para o blob de armazenamento do Azure para armazenar os registos de diagnóstico. Para obter mais informações, consulte [criar um blob de armazenamento do Azure para armazenar registos](azure-stack-vaas-set-up-account.md#create-an-azure-storage-blob-to-store-logs).

    > [!Note]  
    > Marcas descritivas podem ser introduzidas para etiquetar o fluxo de trabalho.

10. Selecione **submeter** para guardar o fluxo de trabalho.

O fluxo de trabalho de solução é executada durante cerca de 24 horas. Adicione uma ligação para ou a instrução no agendamento os testes. Clear na ferramenta.

Encontrar mais informações sobre como monitorizar o progresso de uma validação executar, veja [monitorar um teste ](azure-stack-vaas-monitor-test.md).

## <a name="next-steps"></a>Passos Seguintes

- Para saber mais sobre [validação de Azure Stack como um serviço](https://docs.microsoft.com/azure/azure-stack/partner).
