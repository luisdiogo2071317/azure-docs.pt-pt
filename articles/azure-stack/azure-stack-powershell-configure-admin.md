---
title: Ligar ao Azure Stack com o PowerShell como um operador | Documentos da Microsoft
description: Saiba como ligar ao Azure Stack com o PowerShell como um operador
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 11/08/2018
ms.author: mabrigg
ms.reviewer: thoroet
ms.openlocfilehash: b961fac00ba43eb1b44acc46c6f60fa0f3a10877
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2018
ms.locfileid: "52957083"
---
# <a name="connect-to-azure-stack-with-powershell-as-an-operator"></a>Ligar ao Azure Stack com o PowerShell como um operador

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Pode configurar o Azure Stack para utilizar o PowerShell para gerir os recursos como a criação de ofertas, planos, quotas e alertas. Este tópico ajuda-o a configurar o ambiente de operador.

## <a name="prerequisites"></a>Pré-requisitos

Execute os seguintes pré-requisitos a partir do [development kit do](./asdk/asdk-connect.md#connect-with-rdp) ou a partir de um baseado em Windows cliente externo se estiver [ligado ao ASDK através de VPN](./asdk/asdk-connect.md#connect-with-vpn). 

 - Instale [módulos do Azure Stack compatível com o Azure PowerShell](azure-stack-powershell-install.md).  
 - No desenvolvimento do [Azure Stack Kit, deve ser o blobEndpoint](azure-stack-powershell-download.md) .  

## <a name="configure-the-operator-environment-and-sign-in-to-azure-stack"></a>Configurar o ambiente de operador e inicie sessão no Azure Stack

Configure o ambiente de operador do Azure Stack com o PowerShell. Execute um dos seguintes scripts: substituir o tenantName do Azure AD, ponto final de GraphAudience e valores de ArmEndpoint com a configuração do seu próprio ambiente.

````PowerShell  
    # For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external.
    # To get this value for Azure Stack integrated systems, contact your service provider.
    $ArmEndpoint = "<Admin Resource Manager endpoint for your environment>"

    $AuthEndpoint = (Get-AzureRmEnvironment -Name "AzureStackAdmin").ActiveDirectoryAuthority.TrimEnd('/')
    $TenantId = (invoke-restmethod "$($AuthEndpoint)/$($AADTenantName)/.well-known/openid-configuration").issuer.TrimEnd('/').Split('/')[-1]

    $TenantID = Get-AzsDirectoryTenantId `
      -AADTenantName "<myDirectoryTenantName>.onmicrosoft.com" `
      -EnvironmentName AzureStackAdmin

    # After signing in to your environment, Azure Stack cmdlets
    # can be easily targeted at your Azure Stack instance.
    Add-AzureRmAccount -EnvironmentName "AzureStackAdmin" -TenantId $tenantId
````

## <a name="test-the-connectivity"></a>Testar a conectividade

Agora que tem tudo o que configurar, utilizar o PowerShell para criar recursos no Azure Stack. Por exemplo, pode criar um grupo de recursos para uma aplicação e adicionar uma máquina virtual. Utilize o seguinte comando para criar um grupo de recursos chamado **MyResourceGroup**.

```powershell
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>Passos Seguintes

 - [Desenvolver modelos para o Azure Stack](user/azure-stack-develop-templates.md)
 - [Implementar modelos com o PowerShell](user/azure-stack-deploy-template-powershell.md)