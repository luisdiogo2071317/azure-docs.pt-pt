---
title: Repor a palavra-passe ou a configuração de ambiente de trabalho remoto numa VM do Windows | Documentos da Microsoft
description: Saiba como repor uma palavra-passe de conta ou dos serviços de ambiente de trabalho remoto numa VM do Windows com o portal do Azure ou o Azure PowerShell.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 45c69812-d3e4-48de-a98d-39a0f5675777
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 03/23/2018
ms.author: cynthn
ms.openlocfilehash: a8db7ef82136bae51c99bcfd2a4743e09ebf5712
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/27/2018
ms.locfileid: "47413825"
---
# <a name="how-to-reset-the-remote-desktop-service-or-its-login-password-in-a-windows-vm"></a>Como repor o serviço de ambiente de trabalho remoto ou a palavra-passe de início de sessão numa VM do Windows
Se não conseguir ligar a uma máquina virtual de Windows (VM), pode repor a palavra-passe de administrador local ou repor a configuração do serviço de ambiente de trabalho remoto (não suportada em controladores de domínio do Windows). Pode utilizar o portal do Azure ou a extensão de acesso à VM do Azure PowerShell para repor a palavra-passe. Assim que tiver sessão iniciada na VM, deverá repor a palavra-passe para esse utilizador.  
Se estiver a utilizar o PowerShell, certifique-se de que tem o [módulo do PowerShell mais recente instalado e configurado](/powershell/azure/overview) e tem sessão iniciada sua subscrição do Azure. Também pode [realizar estes passos para as VMs criadas com o modelo de implementação clássica](https://docs.microsoft.com/azure/virtual-machines/windows/classic/reset-rdp).

## <a name="ways-to-reset-configuration-or-credentials"></a>Formas de repor as credenciais ou de configuração
Pode repor os serviços de ambiente de trabalho remoto e as credenciais de diversas formas, consoante as suas necessidades:

- [Repor no portal do Azure](#azure-portal)
- [Repor com o Azure PowerShell](#vmaccess-extension-and-powershell)

## <a name="azure-portal"></a>Portal do Azure
Para expandir o menu do portal, clique nas três barras no canto superior esquerdo e, em seguida, clique em **máquinas virtuais**:

![Navegue para a sua VM do Azure](./media/reset-rdp/Portal-Select-VM.png)

### <a name="reset-the-local-administrator-account-password"></a>**Repor a palavra-passe da conta de administrador local**

Selecione a sua máquina virtual do Windows, em seguida, clique em **suporte + resolução de problemas** > **Repor palavra-passe**. É apresentado o painel de reposição de palavra-passe:

![Página de reposição de palavra-passe](./media/reset-rdp/Portal-RM-PW-Reset-Windows.png)

Introduza o nome de utilizador e uma nova palavra-passe, em seguida, clique em **atualização**. Tente ligar novamente para a VM.

### <a name="reset-the-remote-desktop-service-configuration"></a>**Repor a configuração do serviço de ambiente de trabalho remoto**

Selecione a sua máquina virtual do Windows, em seguida, clique em **suporte + resolução de problemas** > **Repor palavra-passe**. É apresentado o painel de reposição de palavra-passe. 

![Repor configuração RDP](./media/reset-rdp/Portal-RM-RDP-Reset.png)

Selecione **repor configuração apenas** no menu pendente, em seguida, clique em **atualização**. Tente ligar novamente para a VM.


## <a name="vmaccess-extension-and-powershell"></a>Extensão VMAccess e o PowerShell
Certifique-se de que tem o [módulo do PowerShell mais recente instalado e configurado](/powershell/azure/overview) e tem sessão iniciada sua subscrição do Azure com o `Connect-AzureRmAccount` cmdlet.

### <a name="reset-the-local-administrator-account-password"></a>**Repor a palavra-passe da conta de administrador local**
Repor o nome de utilizador ou palavra-passe de administrador com o [Set-AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) cmdlet do PowerShell. O typeHandlerVersion tem de ser 2.0 ou superior, como versão 1 é preterida. 

```powershell
$SubID = "<SUBSCRIPTION ID>" 
$RgName = "<RESOURCE GROUP NAME>" 
$VmName = "<VM NAME>" 
$Location = "<LOCATION>" 
 
Connect-AzureRmAccount 
Select-AzureRMSubscription -SubscriptionId $SubID 
Set-AzureRmVMAccessExtension -ResourceGroupName $RgName -Location $Location -VMName $VmName -Credential (get-credential) -typeHandlerVersion "2.0" -Name VMAccessAgent 
```

> [!NOTE] 
> Se digitar um nome diferente do que a conta de administrador local atual na sua VM, a extensão VMAccess irá adicionar uma conta de administrador local com esse nome e atribuir a sua palavra-passe especificada para essa conta. Se a conta de administrador local na sua VM existir, ele irá repor a palavra-passe e se a conta estiver desativada, a extensão VMAccess ativa o mesmo.

### <a name="reset-the-remote-desktop-service-configuration"></a>**Repor a configuração do serviço de ambiente de trabalho remoto**
Reponha o acesso remoto à sua VM com o [Set-AzureRmVMAccessExtension](/powershell/module/azurerm.compute/set-azurermvmaccessextension) cmdlet do PowerShell. O exemplo seguinte repõe a extensão de acesso com o nome `myVMAccess` na VM com o nome `myVM` no `myResourceGroup` grupo de recursos:

```powershell
Set-AzureRmVMAccessExtension -ResourceGroupName "myResoureGroup" -VMName "myVM" -Name "myVMAccess" -Location WestUS -typeHandlerVersion "2.0" -ForceRerun
```

> [!TIP]
> Em qualquer momento, uma VM pode ter apenas um agente de acesso VM único. Para definir o acesso à VM propriedades do agente com êxito, o `-ForceRerun` opção pode ser utilizada. Quando utilizar `-ForceRerun`, certifique-se utilizar o mesmo nome para o agente de acesso da VM como utilizado em todos os comandos anteriores.

Se ainda não é possível ligar remotamente à máquina virtual, veja mais passos para experimente [ligações de resolução de problemas de ambiente de trabalho remoto a uma máquina virtual do Azure com base em Windows](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Se perder a ligação para o controlador de domínio do Windows, terá de restaurar a partir de uma cópia de segurança do controlador de domínio.

## <a name="next-steps"></a>Passos Seguintes
Se a extensão de acesso de VM do Azure não responde e não conseguir repor a palavra-passe, pode [redefinir a senha Windows local offline](../windows/reset-local-password-without-agent.md). Esse método é um processo mais avançado e requer que ligue o disco rígido virtual da VM problemática para outra VM. Siga os passos documentados neste artigo, primeiro e tente apenas o método de reposição de palavra-passe offline como último recurso.

[Extensões VM do Azure e funcionalidades](../extensions/features-windows.md)

[Ligar a uma máquina virtual do Azure com RDP ou SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[Resolver problemas de ligações de ambiente de trabalho remoto a uma máquina virtual baseada no Windows Azure](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

