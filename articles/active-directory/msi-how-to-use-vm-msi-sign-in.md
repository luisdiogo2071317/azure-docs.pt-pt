---
title: "Como utilizar uma identidade de serviço gerida do Azure VM para início de sessão"
description: "Passo a passo instruções e exemplos de utilizar um principal de serviço do MSI de VM do Azure para o cliente de script iniciar sessão e de recursos de acesso."
services: active-directory
documentationcenter: 
author: bryanla
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/01/2017
ms.author: bryanla
ms.openlocfilehash: 2e4da8cd02a1d07a3225a0c1fda4c60928dba8a4
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-use-an-azure-vm-managed-service-identity-msi-for-sign-in"></a>Como utilizar um Azure VM geridos serviço de identidade (MSI) para início de sessão 

[!INCLUDE [preview-notice](../../includes/active-directory-msi-preview-notice.md)]  
Este artigo fornece exemplos de script do PowerShell e da CLI para início de sessão com um principal de serviço do MSI e orientações tópicos importantes, tais como o processamento de erros.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

Se planeia utilizar os Azure PowerShell ou a CLI do Azure exemplos neste artigo, não se esqueça de instalar a versão mais recente do [Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM) ou [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). 

> [!IMPORTANT]
> - Todos os scripts de exemplo neste artigo assume que o cliente da linha de comandos está em execução numa máquina Virtual ativada de MSI. Utilize a funcionalidade VM "Ligar" no portal do Azure, para ligar remotamente à VM. Para obter mais informações sobre como ativar MSI numa VM, consulte [configurar uma VM geridos serviço de identidade (MSI) no portal do Azure](msi-qs-configure-portal-windows-vm.md), ou um dos artigos variantes (utilizando o PowerShell, CLI, um modelo ou um SDK do Azure). 
> - Para evitar erros durante o acesso a recursos, MSI da VM tem de ser fornecido, pelo menos, "Leitor" aceder ao âmbito adequado (de VM ou superior) para permitir operações do Azure Resource Manager na VM. Consulte [atribuir acesso uma identidade de serviço geridas (MSI) a um recurso no portal do Azure](msi-howto-assign-access-portal.md) para obter mais detalhes.

## <a name="overview"></a>Descrição geral

Fornece um MSI um [principal de serviço](develop/active-directory-dev-glossary.md#service-principal-object), que é [criadas após ativação MSI](msi-overview.md#how-does-it-work) na VM. O principal de serviço pode ser dado acesso a recursos do Azure e utilizado como uma identidade por clientes de script/comandos-linha para início de sessão e acesso a recursos. Tradicionalmente, para aceder a recursos protegidos na sua própria identidade, um cliente de script seria necessário:  

   - ser registado e autorizado com o Azure AD como uma aplicação de cliente confidencial/web
   - Inicie sessão no respetivo principal de serviço, utilizando as credenciais da aplicação (que são provável que incorporado no script)

Com MSI, o cliente de script já não é necessário efetuar um, como pode a iniciar sessão no sob o principal de serviço do MSI. 

## <a name="azure-cli"></a>CLI do Azure

O script seguinte demonstra como:

1. iniciar sessão Azure AD ao abrigo do serviço MSI da VM principal  
2. Chamar o Gestor de recursos do Azure e obter ID de principal de serviço a VM CLI encarrega-se da gestão de aquisição token/utilização por si automaticamente. Não se esqueça de substituir o nome de máquina virtual para `<VM-NAME>`.  

   ```azurecli
   az login --msi
   
   spID=$(az resource list -n <VM-NAME> --query [*].identity.principalId --out tsv)
   echo The MSI service principal ID is $spID
   ```

## <a name="azure-powershell"></a>Azure PowerShell

O script seguinte demonstra como:

1. Adquirir um token de acesso do MSI da VM.  
2. Utilize o token de acesso para iniciar sessão Azure AD, sob o principal de serviço correspondente do MSI.   
3. Chame um cmdlet do Azure Resource Manager para obter informações sobre a VM. PowerShell encarrega-se de gerir a utilização de token para si automaticamente.  

   ```azurepowershell
   # Get an access token for the MSI
   $response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token `
                                 -Method GET -Body @{resource="https://management.azure.com/"} -Headers @{Metadata="true"}
   $content =$response.Content | ConvertFrom-Json
   $access_token = $content.access_token
   echo "The MSI access token is $access_token"

   # Use the access token to sign in under the MSI service principal. -AccountID can be any string to identify the session.
   Login-AzureRmAccount -AccessToken $access_token -AccountId "MSI@50342"

   # Call Azure Resource Manager to get the service principal ID for the VM's MSI. 
   $vmInfoPs = Get-AzureRMVM -ResourceGroupName <RESOURCE-GROUP> -Name <VM-NAME>
   $spID = $vmInfoPs.Identity.PrincipalId
   echo "The MSI service principal ID is $spID"
   ```

## <a name="resource-ids-for-azure-services"></a>IDs de recurso para serviços do Azure

Consulte [que suporte do Azure AD a autenticação de serviços do Azure](msi-overview.md#azure-services-that-support-azure-ad-authentication) para obter uma lista de recursos que suportam o Azure AD e foi testados com MSI e os respetivos IDs de recurso.

## <a name="error-handling-guidance"></a>Orientações de processamento de erros 

Respostas como as seguintes podem indicar que a VM MSI não foi corretamente configurada:

- PowerShell: *Invoke-WebRequest: não é possível ligar ao servidor remoto*
- CLI: *MSI: não foi possível obter um token de 'http://localhost:50342 / / token oauth2' com um erro de ' HTTPConnectionPool (anfitrião = 'localhost', porta = 50342)* 

Se receber um destes erros, regressar à VM do Azure no [portal do Azure](https://portal.azure.com) e:

- Vá para o **configuração** página e certifique-se de que "" gerido por uma identidade de serviço está definido como "Yes".
- Vá para o **extensões** página e certifique-se a extensão MSI implementada com êxito.

Se o estiver incorreto, terá de voltar a implementar o MSI no seu recurso ou resolver a falha de implementação. Consulte [configurar uma VM geridos serviço de identidade (MSI) no portal do Azure](msi-qs-configure-portal-windows-vm.md) se necessitar de assistência com a configuração de VM.

## <a name="related-content"></a>Conteúdo relacionado

- Para ativar o MSI numa VM do Azure, consulte [configurar uma VM geridos serviço de identidade (MSI) com o PowerShell](msi-qs-configure-powershell-windows-vm.md), ou [configurar uma VM geridos serviço de identidade (MSI) utilizando a CLI do Azure](msi-qs-configure-cli-windows-vm.md)

Utilize a seguinte secção de comentários para fornecer comentários e ajudam-nos refinar e formam o nosso conteúdo.








