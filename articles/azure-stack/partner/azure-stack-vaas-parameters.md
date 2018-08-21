---
title: Parâmetros comuns do fluxo de trabalho na validação do Azure Stack como serviço | Documentos da Microsoft
description: Parâmetros comuns do fluxo de trabalho para a validação do Azure Stack como serviço
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
ms.openlocfilehash: 81a7be973739cfd6eb3f8fb8dc7a0723623c2b8e
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/20/2018
ms.locfileid: "40235052"
---
# <a name="workflow-common-parameters-for-azure-stack-validation-as-a-service"></a>Parâmetros comuns do fluxo de trabalho para a validação do Azure Stack como serviço

[!INCLUDE[Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Parâmetros comuns incluem valores como variáveis de ambiente e o utilizador as credenciais necessárias por todos os testes de validação como um serviço (VaaS). É possível definir esses valores ao nível do fluxo de trabalho. Guarde os valores quando criar ou modificar um fluxo de trabalho. Durante a agenda, o fluxo de trabalho, carrega os valores para o teste. 

## <a name="environment-parameters"></a>Parâmetros do ambiente

Parâmetros do ambiente descrevem o ambiente do Azure Stack em teste. Estes valores devem ser fornecidos pelo gerar e carregar o ficheiro de configuração de carimbo de data / `&lt;link&gt;. [How to get the stamp info link].`

| Nome do parâmetro | Necessário | Tipo | Descrição |
|----------------------------------|----------|------|---------------------------------------------------------------------------------------------------------------------------------|
| Compilação de pilha do Azure | Necessário |  | Número da implementação do Azure Stack (por exemplo, 1.0.170330.9) de compilação |
| Versão do OEM | Sim |  | Número da versão do pacote de OEM utilizado durante a implementação do Azure Stack. |
| Assinatura de OEM | Sim |  | Assinatura do pacote de OEM utilizado durante a implementação do Azure Stack. |
| ID de inquilino do AAD | Necessário |  | Inquilino do Azure Active Directory GUID especificado durante a implementação do Azure Stack.|
| Região | Necessário |  | Região de implementação do Azure Stack. |
| Ponto final do Gestor de recursos de inquilino | Necessário |  | Ponto final para operações do Azure Resource Manager de inquilino (por exemplo, https://management.<ExternalFqdn>) |
| Ponto final do Gestor de recursos de administração | Sim |  | Ponto final para operações de inquilino do Azure Resource Manager (por exemplo, https://adminmanagement.<ExternalFqdn>) |
| FQDN externo | Sim |  | Externo utilizado como o sufixo para pontos finais de nome de domínio completamente qualificado. (por exemplo, local.azurestack.external ou redmond.contoso.com). |
| Número de nós | Sim |  | Número de nós na implementação. |

## <a name="test-parameters"></a>Parâmetros de teste

Parâmetros comuns de teste incluem informações confidenciais que não é possível armazenadas nos arquivos de configuração e tem de ser fornecidos manualmente.

| Nome do parâmetro | Necessário | Tipo | Descrição |
|--------------------------------|------------------------------------------------------------------------------|------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Nome de utilizador do inquilino | Necessário |  | Administrador de inquilino do Active Directory seja aprovisionada já ou do Azure tem de ser aprovisionadas pelo administrador de serviço no diretório do AAD. Para obter detalhes sobre o aprovisionamento da conta de inquilino, consulte [começar com o Azure AD](https://docs.microsoft.com/azure/active-directory/get-started-azure-ad). Este valor é utilizado pelo teste de execução de operações ao nível do inquilino como a implementação de modelos para aprovisionar os recursos (VMs, contas de armazenamento etc.) e executar cargas de trabalho. Este valor é utilizado pelo teste de execução de operações ao nível do inquilino como a implementação de modelos para aprovisionar os recursos (VMs, contas de armazenamento etc.) e executar cargas de trabalho. |
| Palavra-passe do inquilino | Necessário |  | Palavra-passe para o utilizador do inquilino. |
| Nome de utilizador do administrador de serviços | Necessária: Validação de solução, validação do pacote<br>Não é necessário: aprovação de teste |  | Administrador do Active Directory do Azure do inquilino do AAD especificada durante a implementação do Azure Stack. |
| Palavra-passe de administrador de serviços | Necessária: Validação de solução, validação do pacote<br>Não é necessário: aprovação de teste |  | Palavra-passe para o utilizador de administrador de serviços. |
| Nome de utilizador do administrador da nuvem | Necessário |  | Conta do Azure Stack domínio administrador (por exemplo, contoso\cloudadmin). Procurar por função de utilizador = "CloudAdmin" no ficheiro de configuração e selecione o valor na etiqueta de nome de utilizador no ficheiro de configuração. |
| Palavra-passe de administrador da nuvem | Necessário |  | Palavra-passe para o utilizador de administrador da nuvem. |
| Cadeia de ligação de diagnóstico | Necessário |  | Um URI de SAS para uma conta de armazenamento do Azure para que o diagnóstico registos serão copiados durante a execução de teste. Instruções para gerar o URI de SAS estão localizadas [configurar uma conta de armazenamento de BLOBs](azure-stack-vaas-set-up-account.md). |


## <a name="next-steps"></a>Passos Seguintes

- Para saber mais sobre [validação de Azure Stack como um serviço](https://docs.microsoft.com/azure/azure-stack/partner).
