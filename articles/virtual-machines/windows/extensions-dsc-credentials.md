---
title: "Introduzir credenciais no Azure através da configuração de estado pretendido | Microsoft Docs"
description: "Saiba como em segurança introduzir credenciais para máquinas virtuais do Azure utilizando o PowerShell pretendido Estado Configuration (DSC)."
services: virtual-machines-windows
documentationcenter: 
author: zjalexander
manager: timlt
editor: 
tags: azure-resource-manager
keywords: dsc
ms.assetid: ea76b7e8-b576-445a-8107-88ea2f3876b9
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 01/17/2018
ms.author: zachal,migreene
ms.openlocfilehash: a0a565c0bb7e17315c7b0475f3213b620a3e2d6c
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
---
# <a name="pass-credentials-to-the-azure-dscextension-handler"></a>Introduzir as credenciais para o processador de DSCExtension do Azure
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Este artigo abrange a extensão de configuração de estado pretendido (DSC) do Azure. Para obter uma descrição geral do processador de extensão de DSC, consulte [introdução para o processador de extensão de configuração de estado pretendido do Azure](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="pass-in-credentials"></a>Transmita as credenciais de início

Como parte do processo de configuração, poderá precisar de configurar contas de utilizador, aceder aos serviços, ou instale um programa num contexto de utilizador. Para fazer tudo isto, terá de fornecer credenciais.

Pode utilizar o DSC definir configurações parametrizadas. Numa configuração parametrizada, as credenciais são transmitidas para a configuração e armazenadas em segurança em ficheiros. MOF. O processador de extensão do Azure simplifica a gestão de credenciais ao fornecer uma gestão automática de certificados.

O seguinte script de configuração de DSC cria uma conta de utilizador local com a palavra-passe especificada:

```powershell
configuration Main
{
    param(
        [Parameter(Mandatory=$true)]
        [ValidateNotNullorEmpty()]
        [PSCredential]
        $Credential
    )
    Node localhost {
        User LocalUserAccount
        {
            Username = $Credential.UserName
            Password = $Credential
            Disabled = $false
            Ensure = "Present"
            FullName = "Local User Account"
            Description = "Local User Account"
            PasswordNeverExpires = $true
        }
    }
}
```

É importante incluir **localhost nó** como parte da configuração. O processador de extensão especificamente procura o **localhost nó** instrução. Se esta instrução está em falta, os seguintes passos não funcionam. Também é importante incluir a typecast **[PsCredential]**. A extensão para encriptar a credencial é acionado deste tipo específico.

Para publicar este script para o armazenamento de Blobs do Azure:

`Publish-AzureVMDscConfiguration -ConfigurationPath .\user_configuration.ps1`

Para definir a extensão de DSC do Azure e forneça a credencial:

```powershell
$configurationName = "Main"
$configurationArguments = @{ Credential = Get-Credential }
$configurationArchive = "user_configuration.ps1.zip"
$vm = Get-AzureVM "example-1"

$vm = Set-AzureVMDSCExtension -VM $vm -ConfigurationArchive $configurationArchive 
-ConfigurationName $configurationName -ConfigurationArgument @configurationArguments

$vm | Update-AzureVM
```

## <a name="how-a-credential-is-secured"></a>Como uma credencial está protegida

Executar este código solicita uma credencial. Depois da credencial for fornecida, que são armazenada na memória. Quando a credencial é publicada utilizando o **conjunto AzureVmDscExtension** cmdlet, a credencial é transmitido através de HTTPS para a VM. Na VM, o Azure armazena as credenciais encriptada no disco, utilizando o certificado da VM local. A credencial é desencriptada brevemente na memória e, em seguida, volte é encriptada transmiti-lo no DSC.

Este processo é diferente do [utilizar configurações seguras sem o processador de extensão](https://msdn.microsoft.com/powershell/dsc/securemof). O ambiente do Azure dá-lhe uma forma para transmitir dados de configuração de forma segura através de certificados. Quando utiliza o processador de extensão de DSC, não precisa de fornecer **$CertificatePath** ou um **$CertificateID**/ **$Thumbprint** entrada no **ConfigurationData**.

## <a name="next-steps"></a>Passos Seguintes

* Obter um [introdução ao processador de extensão de DSC do Azure](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Examine o [modelo Azure Resource Manager para a extensão de DSC](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Para obter mais informações sobre o PowerShell DSC, vá para o [Centro de documentação do PowerShell](https://msdn.microsoft.com/powershell/dsc/overview).
* Para obter mais funcionalidades que pode gerir utilizando o PowerShell DSC e mais recursos de DSC, procure o [galeria do PowerShell](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0).
