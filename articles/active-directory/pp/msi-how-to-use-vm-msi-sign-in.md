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
ms.date: 01/05/2018
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: c5f71d27a9e07cc6d6a260b809e91aaa2a50270c
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/05/2018
---
# <a name="sign-in-using-a-vm-user-assigned-managed-service-identity-msi"></a>Inicie sessão com uma VM atribuída utilizador da identidade de serviço geridas (MSI)

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]Este artigo fornece exemplos de script da CLI para início de sessão através de um utilizador atribuído principal de serviço do MSI e orientações tópicos importantes, tais como o processamento de erros.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

Para poder utilizar os exemplos da CLI do Azure neste artigo, não se esqueça de instalar a versão mais recente do [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). 

> [!IMPORTANT]
> - Todos os scripts de exemplo neste artigo assume que o cliente da linha de comandos está em execução numa máquina Virtual ativada de MSI. Utilize a funcionalidade VM "Ligar" no portal do Azure, para ligar remotamente à VM. Para obter mais informações sobre como ativar MSI numa VM, consulte [configurar uma VM geridos serviço de identidade (MSI) utilizando a CLI do Azure](msi-qs-configure-cli-windows-vm.md), ou um dos artigos variantes (utilizando o PowerShell, Portal, um modelo ou um SDK do Azure). 
> - Para evitar erros durante o acesso de início de sessão e de recursos, o MSI deve ser dada, pelo menos, "Leitor" aceder ao âmbito adequado (de VM ou superior) para permitir operações do Azure Resource Manager na VM. Consulte [atribuir acesso uma identidade de serviço geridas (MSI) a um recurso com a CLI do Azure](msi-howto-assign-access-cli.md) para obter mais detalhes.

## <a name="overview"></a>Descrição geral

Fornece um MSI um [principal de serviço](~/articles/active-directory/develop/active-directory-dev-glossary.md#service-principal-object), que é [criadas após ativação MSI](msi-overview.md#how-does-it-work) na VM. O principal de serviço pode ser dado acesso a recursos do Azure e utilizado como uma identidade por clientes de script/comandos-linha para acesso de início de sessão e de recursos. Tradicionalmente, para aceder a recursos protegidos na sua própria identidade, um cliente de script seria necessário:  

   - ser registado e autorizado com o Azure AD como uma aplicação de cliente confidencial/web
   - Inicie sessão no respetivo principal de serviço, utilizando as credenciais da aplicação (que são provável que incorporado no script)

Com MSI, o cliente de script já não é necessário efetuar um, como pode a iniciar sessão no sob o principal de serviço do MSI. 

## <a name="azure-cli"></a>CLI do Azure

O script seguinte demonstra como:

1. Inicie sessão no Azure AD em principal de serviço do MSI utilizador atribuído.  
2. Chamar o Gestor de recursos do Azure e obter a localização da região do Azure para uma VM. CLI encarrega-se da gestão de aquisição token/utilização por si automaticamente. Não se esqueça de substituir o nome VM para `<VM NAME>`e o id de recurso MSI utilizador atribuído para `<MSI ID>`. O id de recurso do MSI é devolvido no `id` propriedade durante a criação de um MSI atribuída de utilizador (consulte [configurar um utilizador atribuído geridos serviço de identidade (MSI) para uma VM, utilizando a CLI do Azure](msi-qs-configure-cli-windows-vm.md) para obter exemplos do `az identity create` comando ).

    ```azurecli
    az login --msi –u <MSI ID>
   
    vmLocation=$(az resource list -n <VM NAME> --query [*].location --out tsv)
    echo The VM region location is $vmLocation
    ```

    Respostas de exemplo:
   
    ```bash
    user@vmLinux:~$ az login --msi -u /subscriptions/80c696ff-5efa-4909-a64d-z1b616f423bl/resourcegroups/rgName/providers/Microsoft.ManagedIdentity/userAssignedIdentities/msiName
    [
      {
        "environmentName": "AzureCloud",
        "id": "90c696ff-5efa-4909-a64d-z1b616f423bl",
        "isDefault": true,
        "name": "MSIResource-/subscriptions/90c696ff-5efa-4909-a64d-z1b616f423bl/resourcegroups/rgName/providers/Microsoft.ManagedIdentity/userAssignedIdentities/msiName@50342",
        "state": "Enabled",
        "tenantId": "933a8f0e-ec41-4e69-8ad8-971zc4b533ll",
        "user": {
          "name": "userAssignedIdentity",
          "type": "servicePrincipal"
        }
      }
    ]  
    user@vmLinux:~$ vmLocation=$(az resource list -n vmLinux --query [*].location --out tsv)
    user@vmLinux:~$ echo The VM region location is $vmLocation
    The VM region location is westcentralus
    ```

## <a name="resource-ids-for-azure-services"></a>IDs de recurso para serviços do Azure

Consulte [que suporte do Azure AD a autenticação de serviços do Azure](msi-overview.md#azure-services-that-support-azure-ad-authentication) para obter uma lista de recursos que suportam o Azure AD e foi testados com MSI e os respetivos IDs de recurso.

## <a name="error-handling-guidance"></a>Orientações de processamento de erros 

As respostas seguintes podem indicar que o MSI não foi corretamente configurado:

- CLI: *MSI: não foi possível obter um token de 'http://localhost:50342 / / token oauth2' com um erro de ' HTTPConnectionPool (anfitrião = 'localhost', porta = 50342)* 

Se receber um destes erros, regressar à VM do Azure no [portal do Azure](https://portal.azure.com) e:

- Vá para o **configuração** página e certifique-se de que "" gerido por uma identidade de serviço está definido como "Yes".
- Vá para o **extensões** página e certifique-se a extensão MSI implementada com êxito.

Se o estiver incorreto, precisa de reatribuir o MSI para o recurso novo ou resolver a falha de implementação. Consulte [configurar uma VM geridos serviço de identidade (MSI) utilizando a CLI do Azure](msi-qs-configure-cli-windows-vm.md) se necessitar de assistência com a configuração de VM.

## <a name="next-steps"></a>Passos Seguintes

- Para ativar o MSI numa VM do Azure, consulte [configurar uma VM geridos serviço de identidade (MSI) utilizando a CLI do Azure](msi-qs-configure-cli-windows-vm.md).

Utilize a seguinte secção de comentários para fornecer comentários e ajudam-nos refinar e formam o nosso conteúdo.








