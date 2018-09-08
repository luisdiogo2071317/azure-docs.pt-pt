---
title: Uma visão geral de validação como um serviço para o Azure Stack | Documentos da Microsoft
description: Uma descrição geral da validação do Azure Stack como um serviço, problemas conhecidos.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 56251245a23df031f3bc8fe3d36de43e194fbcc7
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44159678"
---
# <a name="what-is-validation-as-a-service-for-azure-stack"></a>O que é a validação como um serviço para o Azure Stack?

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Validação como um serviço (VaaS) é um serviço do Azure nativo concebido para os parceiros de solução que são de engenharia conjuntas ofertas do Azure Stack com a Microsoft. Parceiros de solução podem utilizar o serviço para verificar se as suas soluções de cumprem os requisitos da Microsoft e a funcionar conforme esperado com o Azure Stack.

Usada principalmente para VaaS é:

- Validar novas soluções do Azure Stack
- Validar as alterações ao software do Azure Stack
- Obter parceiro de solução assinado digitalmente utilizados durante a implementação de pacotes
- Materiais de validação do pré-visualização do Azure Stack

## <a name="validate-new-azure-stack-solution"></a>Validar a nova solução do Azure Stack

Parceiros de utilizam o fluxo de trabalho de validação de solução para verificar novas soluções do Azure Stack. A solução tem de passar os testes de componente de testes requeridos do Hardware Lab Kit (HKL) do Azure Stack. Só precisa de executar o fluxo de trabalho duas vezes para cada nova solução: uma vez para a configuração mínima e máxima.

Para obter mais informações, consulte [validar uma nova solução do Azure Stack](azure-stack-vaas-validate-solution-new.md).

## <a name="validate-changes-to-the-azure-stack-software"></a>Validar as alterações ao software do Azure Stack

Parceiros de utilizam o fluxo de trabalho de validação do pacote para verificar se a sua solução funciona com a recente atualização de software do Azure Stack. No mínimo, o fluxo de trabalho de validação do pacote deve ser executado no ambiente de hardware recomendada pela Microsoft e numa solução em que o patch e de atualização (P & U) foram utilizados para aplicar a atualização. Deve executar a validação do pacote na compilação de linha de base.

Para obter mais informações, consulte [validar as atualizações de software da Microsoft](azure-stack-vaas-validate-microsoft-updates.md).

## <a name="get-digitally-signed-solution-partner-packages"></a>Obter solução assinada digitalmente pacotes de parceiro

Além de validação de atualizações do Azure Stack, pode utilizar o fluxo de trabalho de validação do pacote para verificar atualizações para pacotes de personalização OEM, que incluem drivers de parceiros específicos do Azure Stack, firmware e outro software utilizado durante a implementação da pilha do Azure software. Implemente o pacote que está a verificar a versão atual do Azure Stack através um, pelo menos, a mínima tamanho solução que serão suportadas. O pacote atualizado tem de ser carregado para o serviço antes de iniciar o teste. Se os testes forem bem sucedidas, notificar vaashelp@microsoft.com. Informe o parceiro do Azure Stack que o pacote foi concluída de teste e deve ser assinado digitalmente com a assinatura digital do Azure Stack. A Microsoft assina o pacote e notifica o parceiro do Azure Stack que o pacote está disponível para download no portal.

Para obter mais informações, consulte [pacotes de validar OEM](azure-stack-vaas-validate-oem-package.md).

## <a name="preview-azure-stack-validation-collateral"></a>Materiais de validação do pré-visualização do Azure Stack

Microsoft regularmente disponibiliza novas funcionalidades no Azure Stack. Como parte do processo de desenvolvimento para o fornecimento desses recursos no mercado, novo acessórios de teste é disponibilizado no fluxo de trabalho de aprovação de teste. O fluxo de trabalho de aprovação de teste inclui acessórios de teste dos outros fluxos de trabalho para permitir a execução de teste não oficial. Não utilize o fluxo de trabalho de aprovação de teste para enviar resultados para aprovação. Use o fluxo de validações de validação e o pacote da solução para obter aprovação oficial para a sua solução.

## <a name="next-steps"></a>Passos Seguintes

- Começar a utilizar, e [configurar sua validação como uma conta de serviço](azure-stack-vaas-validate-solution-new.md)
