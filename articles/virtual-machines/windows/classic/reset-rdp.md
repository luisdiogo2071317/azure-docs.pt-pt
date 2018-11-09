---
title: Repor a palavra-passe ou a configuração de ambiente de trabalho remoto numa VM do Windows no Azure | Documentos da Microsoft
description: Saiba como repor uma palavra-passe de conta ou dos serviços de ambiente de trabalho remoto numa VM do Windows criada com o modelo de implementação clássica com o portal do Azure ou o Azure PowerShell.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: cynthn
ms.openlocfilehash: e91f75299dddf0ac173499a531ea959292082a9f
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51246553"
---
# <a name="how-to-reset-the-remote-desktop-service-or-its-login-password-in-a-windows-vm-created-using-the-classic-deployment-model"></a>Como repor o serviço de ambiente de trabalho remoto ou a palavra-passe de início de sessão numa VM do Windows criada com o modelo de implementação clássico
> [!IMPORTANT]
> O Azure tem dois modelos de implementação para criar e trabalhar com recursos: [Resource Manager e Clássico](../../../resource-manager-deployment-model.md). Este artigo explica como utilizar o modelo de implementação clássica. A Microsoft recomenda que as implementações mais novas utilizem o modelo Resource Manager. Também pode [realizar estes passos para as VMs criadas com o modelo de implementação do Resource Manager](../reset-rdp.md).
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]


Se não conseguir ligar a uma máquina virtual de Windows (VM), pode repor a palavra-passe de administrador local ou repor a configuração do serviço de ambiente de trabalho remoto. Pode utilizar o portal do Azure ou a extensão de acesso à VM do Azure PowerShell para repor a palavra-passe.

## <a name="ways-to-reset-configuration-or-credentials"></a>Formas de repor as credenciais ou de configuração
Pode repor os serviços de ambiente de trabalho remoto e as credenciais de diversas formas, consoante as suas necessidades:

- [Repor no portal do Azure](#azure-portal)
- [Repor com o Azure PowerShell](#vmaccess-extension-and-powershell)

## <a name="azure-portal"></a>Portal do Azure
Pode utilizar o [portal do Azure](https://portal.azure.com) para repor o serviço de ambiente de trabalho remoto. Para expandir o menu do portal, clique nas três barras no canto superior esquerdo e, em seguida, clique em **máquinas virtuais (clássicas)**:

![Navegue para a sua VM do Azure](./media/reset-rdp/Portal-Select-Classic-VM.png)

Selecione a sua máquina virtual do Windows e, em seguida, clique em **reposição remota...** . A caixa de diálogo seguinte for apresentada a reposição da configuração do ambiente de trabalho remoto:

![Página de configuração de RDP de reposição](./media/reset-rdp/Portal-RDP-Reset-Windows.png)

Também pode repor o nome de utilizador e palavra-passe da conta de administrador local. A partir da sua VM, clique em **suporte + resolução de problemas** > **Repor palavra-passe**. É apresentado o painel de reposição de palavra-passe:

![Página de reposição de palavra-passe](./media/reset-rdp/Portal-PW-Reset-Windows.png)

Depois de introduzir o novo nome de utilizador e palavra-passe, clique em **guardar**.

## <a name="vmaccess-extension-and-powershell"></a>Extensão VMAccess e o PowerShell
Certificar-se de que o agente da VM está instalado na máquina virtual. A extensão VMAccess não precisa ser instalado antes de poder utilizá-lo, desde que o agente da VM está disponível. Certifique-se de que o agente da VM já está instalado utilizando o seguinte comando. (Substitua "myCloudService" e "myVM" pelos nomes do seu serviço de nuvem e a sua VM, respectivamente. Pode obter esses nomes ao executar `Get-AzureVM` sem quaisquer parâmetros.)

```powershell
$vm = Get-AzureVM -ServiceName "myCloudService" -Name "myVM"
write-host $vm.VM.ProvisionGuestAgent
```

Se o **write-host** comando apresenta **verdadeiro**, o agente da VM está instalado. Se for exibido **False**, consulte as instruções e um link para download na [agente da VM e extensões – parte 2](https://go.microsoft.com/fwlink/p/?linkid=403947&clcid=0x409) mensagem de blogue do Azure.

Se criou a máquina virtual com o portal, verifique se `$vm.GetInstance().ProvisionGuestAgent` devolve **True**. Caso contrário, pode configurá-lo ao utilizar este comando:

```powershell
$vm.GetInstance().ProvisionGuestAgent = $true
```

Este comando impede que o seguinte erro ao executar o **Set-AzureVMExtension** comando nos passos seguintes: "Aprovisionar o agente convidado tem de estar ativado no objeto da VM antes de definir a extensão de acesso de VM de IaaS."

### <a name="reset-the-local-administrator-account-password"></a>**Repor a palavra-passe da conta de administrador local**
Criar uma credencial de início de sessão com o nome de conta de administrador local atual e uma nova palavra-passe e, em seguida, execute o `Set-AzureVMAccessExtension` da seguinte forma.

```powershell
$cred=Get-Credential
Set-AzureVMAccessExtension –vm $vm -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password  | Update-AzureVM
```

Se digitar um nome diferente da conta atual, a extensão VMAccess muda o nome de conta de administrador local, atribui a palavra-passe para essa conta e emite um ambiente de trabalho remoto fim de sessão. Se a conta de administrador local é desativada, a extensão VMAccess ativa o mesmo.

Estes comandos também repõem a configuração do serviço de ambiente de trabalho remoto.

### <a name="reset-the-remote-desktop-service-configuration"></a>**Repor a configuração do serviço de ambiente de trabalho remoto**
Para repor a configuração do serviço de ambiente de trabalho remoto, execute o seguinte comando:

```powershell
Set-AzureVMAccessExtension –vm $vm | Update-AzureVM
```

A extensão VMAccess executa dois comandos na máquina virtual:

```powershell
netsh advfirewall firewall set rule group="Remote Desktop" new enable=Yes
```

Este comando activa o grupo de Firewall do Windows incorporado que permite receber tráfego de ambiente de trabalho remoto, que utiliza a porta TCP 3389.

```powershell
Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -Value 0
```

Este comando define o fDenyTSConnections o valor de registo para 0, permitindo que as ligações de ambiente de trabalho remoto.

## <a name="next-steps"></a>Passos Seguintes
Se a extensão de acesso de VM do Azure não responde e não conseguir repor a palavra-passe, pode [redefinir a senha Windows local offline](../reset-local-password-without-agent.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Esse método é um processo mais avançado e requer que ligue o disco rígido virtual da VM problemática para outra VM. Siga os passos documentados neste artigo, primeiro e tente apenas o método de reposição de palavra-passe offline como último recurso.

[Extensões VM do Azure e funcionalidades](../extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

[Ligar a uma máquina virtual do Azure com RDP ou SSH](https://msdn.microsoft.com/library/azure/dn535788.aspx)

[Resolver problemas de ligações de ambiente de trabalho remoto a uma máquina virtual baseada no Windows Azure](../troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

