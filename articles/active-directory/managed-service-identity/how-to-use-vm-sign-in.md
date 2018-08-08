---
title: Como utilizar uma identidade de serviço gerido do Azure VM para início de sessão
description: Instruções passo a passo instruções e exemplos para utilizar um principal de serviço do MSI de VM do Azure para o cliente de script iniciar sessão e de recursos de acesso.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/01/2017
ms.author: daveba
ms.openlocfilehash: 205938bbf615face0768028717a333c13c1fafa1
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/07/2018
ms.locfileid: "39590318"
---
# <a name="how-to-use-an-azure-vm-managed-service-identity-msi-for-sign-in"></a>Como utilizar um Azure VM Managed Service Identity (MSI) para início de sessão 

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]  
Este artigo fornece exemplos de script do PowerShell e a CLI para início de sessão com um principal de serviço do MSI e documentação de orientação sobre tópicos importantes, como o tratamento de erros.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

Se planeja usar os exemplos do Azure PowerShell ou a CLI do Azure neste artigo, certifique-se de que instala a versão mais recente do [do Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM) ou [CLI 2.0 do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli). 

> [!IMPORTANT]
> - Todos os script de exemplo neste artigo supõe que o cliente de linha de comando está em execução numa máquina Virtual ativado de MSI. Utilize a funcionalidade de "Ligar" de VM no portal do Azure, para ligar remotamente à VM. Para obter detalhes sobre como ativar o MSI numa VM, consulte [configurar uma VM Managed Service Identity (MSI) com o portal do Azure](qs-configure-portal-windows-vm.md), ou um dos artigos variantes (usando o PowerShell, CLI, um modelo ou um SDK do Azure). 
> - Para evitar erros durante o acesso a recursos, MSI da VM tem de obter, pelo menos, "Leitor de" acesso no âmbito do apropriado (a VM ou superior) para permitir operações do Azure Resource Manager na VM. Ver [atribuir um acesso de identidade de serviço gerida (MSI) a um recurso no portal do Azure](howto-assign-access-portal.md) para obter detalhes.

## <a name="overview"></a>Descrição geral

Fornece um MSI uma [objeto principal do serviço](../develop/developer-glossary.md#service-principal-object) , que é [criados após ativar o MSI](overview.md#how-does-it-work) na VM. O principal de serviço pode ser concedido acesso aos recursos do Azure e utilizado como uma identidade por clientes de script/linha de comandos para início de sessão e acesso a recursos. Tradicionalmente, para aceder a recursos protegidos na sua própria identidade do utilizador, um cliente de script precisaria:  

   - ser registrado e dar o seu consentimento no Azure AD como uma aplicação de cliente confidencial/web
   - Inicie sessão em seu principal de serviço, com as credenciais da aplicação (o que são provável que incorporado no script)

Com o MSI, o cliente de script já não precisa fazer qualquer um, como ele pode iniciar sessão sob o principal de serviço do MSI. 

## <a name="azure-cli"></a>CLI do Azure

O script a seguir demonstra como:

1. Inicie sessão no Azure AD em principal de serviço MSI da VM  
2. Chamar o Azure Resource Manager e obter o ID principal de serviço. da VM CLI cuida do gerenciamento de aquisição/utilizar token para automaticamente. Certifique-se de que substitua o nome de máquina virtual de `<VM-NAME>`.  

   ```azurecli
   az login --identity
   
   spID=$(az resource list -n <VM-NAME> --query [*].identity.principalId --out tsv)
   echo The MSI service principal ID is $spID
   ```

## <a name="azure-powershell"></a>Azure PowerShell

O script a seguir demonstra como:

1. Inicie sessão no Azure AD em principal de serviço MSI da VM  
2. Chame um cmdlet do Azure Resource Manager para obter informações sobre a VM. PowerShell se encarrega de gerir a utilização de token para automaticamente.  

   ```azurepowershell
   Add-AzureRmAccount -identity

   # Call Azure Resource Manager to get the service principal ID for the VM's MSI. 
   $vmInfoPs = Get-AzureRMVM -ResourceGroupName <RESOURCE-GROUP> -Name <VM-NAME>
   $spID = $vmInfoPs.Identity.PrincipalId
   echo "The MSI service principal ID is $spID"
   ```

## <a name="resource-ids-for-azure-services"></a>IDs de recurso para serviços do Azure

Ver [que o suporte do Azure AD a autenticação dos serviços Azure](services-support-msi.md#azure-services-that-support-azure-ad-authentication) para obter uma lista de recursos que suportam o Azure AD e foram testados com o MSI e seus respectivos IDs de recurso.

## <a name="error-handling-guidance"></a>Documentação de orientação de manipulação de erro 

Respostas, como a seguir podem indicar que MSI da VM não foi corretamente configurada:

- PowerShell: *Invoke-WebRequest: não é possível ligar ao servidor remoto*
- CLI: *MSI: não foi possível obter um token da "http://localhost:50342/oauth2/token" com um erro de "HTTPConnectionPool (host = 'localhost', porta = 50342)* 

Se receber um desses erros, regresse à VM do Azure no [portal do Azure](https://portal.azure.com) e:

- Vá para o **configuração** página e certifique-se de que "Identidade de serviço gerida" está definida como "Sim".
- Vá para o **extensões** página e certifique-se a extensão MSI implementada com êxito.

Se uma for incorreta, terá de voltar a implementar o MSI no seu recurso, ou a falha de implementação de resolução de problemas. Ver [configurar uma VM Managed Service Identity (MSI) com o portal do Azure](qs-configure-portal-windows-vm.md) se precisar de assistência com a configuração da VM.

## <a name="related-content"></a>Conteúdo relacionado

- Para ativar o MSI numa VM do Azure, consulte [configurar uma VM Managed Service Identity (MSI) com o PowerShell](qs-configure-powershell-windows-vm.md), ou [configurar uma VM Managed Service Identity (MSI) com a CLI do Azure](qs-configure-cli-windows-vm.md)

Utilize a seguinte secção de comentários para fornecer comentários e ajude-na refinar e moldar o nosso conteúdo.








