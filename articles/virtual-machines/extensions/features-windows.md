---
title: Extensões VM do Azure e de funcionalidades do Windows | Microsoft Docs
description: Saiba que extensões estão disponíveis para máquinas virtuais do Azure, agrupadas por que forneça ou melhorar.
services: virtual-machines-windows
documentationcenter: ''
author: danielsollondon
manager: jeconnoc
editor: ''
tags: azure-service-management,azure-resource-manager
ms.assetid: 999d63ee-890e-432e-9391-25b3fc6cde28
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/30/2018
ms.author: danis
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e9e147e2cbe5ff42562d6fcfab62460df48f3d65
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/06/2018
ms.locfileid: "34809731"
---
# <a name="virtual-machine-extensions-and-features-for-windows"></a>Extensões de máquina virtual e funcionalidades para o Windows

Extensões de máquina virtual do Azure (VM) são aplicações de pequenas a indicar tarefas de configuração e a automatização de pós-implementação em VMs do Azure. Por exemplo, se uma máquina virtual requer a instalação de software, proteção de software antivírus, ou para executar um script dentro do mesmo, uma extensão da VM pode ser utilizada. Extensões VM do Azure podem ser executadas com a CLI do Azure, PowerShell, os modelos Azure Resource Manager e o portal do Azure. Extensões podem ser incluídas com uma nova implementação de VM ou executar qualquer sistema existente.

Este artigo fornece uma descrição geral das extensões de VM, pré-requisitos para utilizar extensões de VM do Azure, e orientações sobre como detetar, gerir e remover as extensões de VM. Este artigo fornece informações generalizadas porque há várias extensões VM, cada um com uma configuração potencialmente exclusiva. Extensão específicos podem ser encontrados detalhes cada documento específicos para a extensão individuais.

## <a name="use-cases-and-samples"></a>Casos de utilização e amostras

Estão disponíveis várias extensões de VM do Azure diferentes, cada um com específico utilizar maiúsculas e minúsculas. Alguns exemplos incluem:

- Aplicar as configurações de PowerShell Desired estado para uma VM com a extensão de DSC para Windows. Para obter mais informações, consulte [extensão de configuração do Azure Desired estado](dsc-overview.md).
- Configure a monitorização de uma VM com a extensão de VM de agente de monitorização da Microsoft. Para obter mais informações, consulte [ligar as VMs do Azure ao Log Analytics](../../log-analytics/log-analytics-azure-vm-extension.md).
- Configure uma VM do Azure utilizando Chef. Para obter mais informações, consulte [implementação da VM do Azure de automatização com Chef](../windows/chef-automation.md).
- Configure a monitorização da infraestrutura do Azure com a extensão de Datadog. Para obter mais informações, consulte o [Datadog blogue](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).


Para além das extensões de específicos de processos, uma extensão de Script personalizado está disponível para máquinas virtuais Windows e Linux. A extensão de Script personalizado para o Windows permite que qualquer script do PowerShell para ser executado numa VM. Scripts personalizados são úteis para estruturar implementações do Azure que necessitam de configuração para além dos quais as nativa ferramentas do Azure podem fornecer. Para obter mais informações, consulte [a extensão de Script do Windows VM personalizada](custom-script-windows.md).

## <a name="prerequisites"></a>Pré-requisitos

Para lidar com a extensão da VM, terá do agente Linux do Azure instalados. Algumas extensões individuais têm pré-requisitos, tais como o acesso a recursos ou dependências.

### <a name="azure-vm-agent"></a>Agente da VM do Azure

O agente da VM do Azure gere as interações entre uma VM do Azure e o controlador de recursos de infraestrutura do Azure. O agente da VM é responsável por muitos aspetos funcionais de implementar e gerir as VMs do Azure, incluindo a executar as extensões VM. O agente da VM do Azure está pré-instalado imagens do Azure Marketplace e pode ser instalado manualmente nos sistemas operativos suportados. O Azure VM agente para o Windows é conhecido como o agente convidado do Windows.

Para obter informações sobre sistemas operativos suportados e instruções de instalação, consulte [agente da máquina virtual do Azure](agent-windows.md).

#### <a name="supported-agent-versions"></a>Versões do agente suportado

Para fornecer a melhor experiência possível, existem versões mínimas do agente. Para obter mais informações, consulte [este artigo](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support).

#### <a name="supported-oses"></a>Sos suportados

O agente convidado do Windows é executado em vários sos, no entanto, a arquitetura de extensões tem um limite para os sos que extensões. Para obter mais informações, consulte [neste artigo] (https://support.microsoft.com/en-us/help/4078134/azure-extension-supported-operating-systems ).

Algumas extensões não são suportadas em todos os sos e pode emitir *51 de código de erro, 'SO não suportada'*. Consulte a documentação de extensões individuais para suporte.

#### <a name="network-access"></a>Acesso à rede

Pacotes de extensão são transferidos do repositório de extensões de armazenamento do Azure e carregamentos de estado de extensão são publicados no armazenamento do Azure. Se utilizar [suportado](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support) versão dos agentes, não é necessário permitir o acesso ao Storage do Azure na região de VM, como pode utilizar o agente para redirecionar a comunicação com o controlador de recursos de infraestrutura do Azure para as comunicações do agente. Se tiver uma versão não suportada do agente, terá de permitir o acesso de saída para o armazenamento do Azure nessa região da VM.

> [!IMPORTANT]
> Se tiver bloqueou o acesso à *168.63.129.1* utilizando a firewall do convidado, em seguida, extensões falharem independentemente acima.

Agentes só podem ser utilizados para transferir os pacotes de extensão e comunicar o estado. Por exemplo, se uma instalação da extensão tem de transferir um script a partir do GitHub (de Script personalizado) ou necessita de acesso ao Storage do Azure (cópia de segurança do Azure), em seguida, adicionais firewall/rede segurança portas de grupo tem de ser aberta. As diferentes extensões têm requisitos diferentes, uma vez que estes são aplicações na sua própria direito. Para as extensões que necessitam de acesso ao Storage do Azure, pode permitir o acesso utilizando as etiquetas de serviço do Azure NSG para [armazenamento](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags).

O agente convidado do Windows não tem suporte para lhe redirecionar pedidos de tráfego através do agente de servidor de proxy.

## <a name="discover-vm-extensions"></a>Detetar as extensões de VM

Várias extensões VM diferentes estão disponíveis para utilização com as VMs do Azure. Para ver uma lista completa, utilize [Get-AzureRmVMExtensionImage](/powershell/module/azurerm.compute/get-azurermvmextensionimage). O exemplo seguinte apresenta uma lista de todas as extensões disponíveis no *WestUS* localização:

```powershell
Get-AzureRmVmImagePublisher -Location "WestUS" | `
Get-AzureRmVMExtensionImageType | `
Get-AzureRmVMExtensionImage | Select Type, Version
```

## <a name="run-vm-extensions"></a>Executar as extensões VM

Extensões VM do Azure é executada em VMs existentes, que é útil quando for necessário efetuar alterações de configuração ou recuperar conectividade numa VM já implementada. Extensões VM também podem ser incluídas com implementações de modelos Azure Resource Manager. Ao utilizar extensões com modelos do Resource Manager, VMs do Azure pode ser implementadas e configuradas sem a intervenção de pós-implementação.

Os métodos seguintes podem ser utilizados para executar uma extensão contra uma VM existente.

### <a name="powershell"></a>PowerShell

Existem vários comandos do PowerShell para executar as extensões individuais. Para ver uma lista, utilize [Get-Command](/powershell/module/microsoft.powershell.core/get-command) e filtre *extensão*:

```powershell
Get-Command Set-AzureRM*Extension* -Module AzureRM.Compute
```

Isto fornece o resultado semelhante ao seguinte:

```powershell
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Set-AzureRmVMAccessExtension                       4.5.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMADDomainExtension                     4.5.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMAEMExtension                          4.5.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMBackupExtension                       4.5.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMBginfoExtension                       4.5.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMChefExtension                         4.5.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMCustomScriptExtension                 4.5.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMDiagnosticsExtension                  4.5.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMDiskEncryptionExtension               4.5.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMDscExtension                          4.5.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMExtension                             4.5.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMSqlServerExtension                    4.5.0      AzureRM.Compute
Cmdlet          Set-AzureRmVmssDiskEncryptionExtension             4.5.0      AzureRM.Compute
```

O exemplo seguinte utiliza a extensão de Script personalizado para transferir um script a partir de um repositório do GitHub para a máquina virtual de destino e, em seguida, execute o script. Para obter mais informações sobre a extensão de Script personalizado, consulte [descrição geral de extensão de Script personalizado](custom-script-windows.md).

```powershell
Set-AzureRmVMCustomScriptExtension -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" -Name "myCustomScript" `
    -FileUri "https://raw.githubusercontent.com/neilpeterson/nepeters-azure-templates/master/windows-custom-script-simple/support-scripts/Create-File.ps1" `
    -Run "Create-File.ps1" -Location "West US"
```

No exemplo seguinte, a extensão de acesso de VM é utilizada para repor a palavra-passe administrativa de uma VM do Windows para uma palavra-passe temporária. Para obter mais informações sobre a extensão de acesso de VM, consulte [serviço de reposição de ambiente de trabalho remoto numa Windows VM](../windows/reset-rdp.md). Assim que tiver executado isto, devem repor a palavra-passe no primeiro início de sessão:

```powershell
$cred=Get-Credential

Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" -VMName "myVM" -Name "myVMAccess" `
    -Location WestUS -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"
```

O `Set-AzureRmVMExtension` comando pode ser utilizado para iniciar de qualquer extensão VM. Para obter mais informações, consulte o [referência de conjunto AzureRmVMExtension](https://msdn.microsoft.com/library/mt603745.aspx).


### <a name="azure-portal"></a>Portal do Azure

Extensões VM podem ser aplicadas a uma VM existente através do portal do Azure. Selecione a VM no portal, escolha **extensões**, em seguida, selecione **adicionar**. Escolha a extensão que pretende na lista de extensões disponíveis e siga as instruções no assistente.

O exemplo seguinte mostra a instalação da extensão de Antimalware da Microsoft do portal do Azure:

![Instalar a extensão de antimalware](./media/features-windows/installantimalwareextension.png)

### <a name="azure-resource-manager-templates"></a>Modelos do Azure Resource Manager

Extensões VM podem ser adicionadas a um modelo Azure Resource Manager e executadas com a implementação do modelo. Quando implementa uma extensão com um modelo, pode criar totalmente configuradas implementações do Azure. Por exemplo, o seguinte JSON é executado de um Gestor de recursos do modelo implementa um conjunto de VMs com balanceamento de carga e uma base de dados SQL do Azure, em seguida, instala uma aplicação .NET Core em cada VM. A extensão da VM encarrega-se de que a instalação de software.

Para obter mais informações, consulte o [completa modelo do Resource Manager](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
    ],
    "tags": {
    "displayName": "config-app"
    },
    "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.9",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
        ]
    },
    "protectedSettings": {
        "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
    }
}
```

Para obter mais informações sobre como criar modelos do Resource Manager, consulte [Authoring Azure Resource Manager modelos com extensões de VM do Windows](../windows/template-description.md#extensions).

## <a name="secure-vm-extension-data"></a>Proteger os dados de extensão VM

Quando executa uma extensão VM, poderá ser necessário incluir informações confidenciais, tais como as credenciais, os nomes das contas de armazenamento e chaves de acesso de conta de armazenamento. Várias extensões VM incluem uma configuração protegida que encripta os dados e apenas desencripta a mesma dentro da VM de destino. Cada extensão com um esquema de configuração protegido específica, e cada está detalhada na documentação de extensão específica.

O exemplo seguinte mostra uma instância da extensão de Script personalizado para o Windows. O comando a executar inclui um conjunto de credenciais. Neste exemplo, o comando a executar não está encriptado:

```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
    ],
    "tags": {
    "displayName": "config-app"
    },
    "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.9",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
        ],
        "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
    }
}
```

Mover o **comando a executar** propriedade para o **protegidos** configuração protege a cadeia de execução, conforme mostrado no exemplo seguinte:

```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
    ],
    "tags": {
    "displayName": "config-app"
    },
    "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.9",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
        ]
    },
    "protectedSettings": {
        "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
    }
}
```

### <a name="how-do-agents-and-extensions-get-updated"></a>Como agentes e as extensões for atualizadas?

Os agentes e as extensões de partilham o mesmo mecanismo de atualização. Algumas atualizações não necessitam de regras de firewall adicional.

Quando estiver disponível uma atualização, está instalada na VM apenas quando há uma alteração às extensões e outras alterações de modelo de VM, tais como:

- Discos de dados
- Extensões
- Contentor de diagnóstico de arranque
- Segredos de SO convidado
- Tamanho da VM
- Perfil de rede

Os publicadores disponibilizar atualizações para regiões de em alturas diferentes, pelo que é possível que pode ter VMs em regiões diferentes em diferentes versões.

#### <a name="listing-extensions-deployed-to-a-vm"></a>Extensões de listagem implementadas para uma VM

```powershell
$vm = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -VMName "myVM"
$vm.Extensions | select Publisher, VirtualMachineExtensionType, TypeHandlerVersion
```

```powershell
Publisher             VirtualMachineExtensionType          TypeHandlerVersion
---------             ---------------------------          ------------------
Microsoft.Compute     CustomScriptExtension                1.9
```

#### <a name="agent-updates"></a>Atualizações de agente

O agente convidado do Windows contém apenas *código de processamento de extensão*, a *código de aprovisionamento do Windows* separado. Pode desinstalar o agente convidado do Windows. Não é possível desativar a atualização automática do agente convidado janela.

O *código de processamento de extensão* é responsável para comunicar com os recursos de infraestrutura do Azure e processar, tais como as operações de extensões VM instala, comunicar o estado, atualizar as extensões individuais e removê-los. Atualizações contêm correções de segurança, correções de erros e melhoramentos para o *código de processamento de extensão*.

Para verificar a versão que está a executar, consulte [Detecting instalado o agente convidado Windows](agent-windows.md#detect-the-vm-agent).

#### <a name="extension-updates"></a>Atualizações de extensão

Quando estiver disponível uma atualização da extensão, o agente de convidados Windows transfere e atualiza a extensão. Atualizações automáticas de extensão são *secundárias* ou *correção*. Pode optar ativamente por participar no ou par excluir extensões *secundárias* atualizações quando Aprovisiona a extensão. O exemplo seguinte mostra como atualizar automaticamente as versões de secundárias num modelo do Resource Manager com *autoUpgradeMinorVersion ": true,'*:

```json
    "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.9",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
        ]
    },
```

Para obter as correções de erros de versão secundária mais recentes, recomendamos que selecione sempre atualização automática das implementações de extensão. Atualizações de correção que transportar as correções de erros de segurança ou de chave não podem ser excluídas.

### <a name="how-to-identify-extension-updates"></a>Como identificar as atualizações de extensão

#### <a name="identifying-if-the-extension-is-set-with-autoupgrademinorversion-on-a-vm"></a>Identificar se a extensão está definida com a etiqueta autoUpgradeMinorVersion numa VM

Pode ver o modelo de VM se a extensão foi aprovisionada com 'autoUpgradeMinorVersion'. Para verificar, utilize [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm) e forneça o nome do grupo de recursos e VM da seguinte forma:

```powerShell
 $vm = Get-AzureRmVm -ResourceGroupName "myResourceGroup" -VMName "myVM"
 $vm.Extensions
```

O seguinte resultado de exemplo mostra que *autoUpgradeMinorVersion* está definido como *verdadeiro*:

```powershell
ForceUpdateTag              :
Publisher                   : Microsoft.Compute
VirtualMachineExtensionType : CustomScriptExtension
TypeHandlerVersion          : 1.9
AutoUpgradeMinorVersion     : True
```

#### <a name="identifying-when-an-autoupgrademinorversion-occurred"></a>Identificar quando ocorreu uma etiqueta autoUpgradeMinorVersion

Para ver quando ocorreu uma atualização para a extensão, reveja o agente de registos na VM em *C:\WindowsAzure\Logs\WaAppAgent.log*

No exemplo seguinte, a VM tinha *Microsoft.Compute.CustomScriptExtension 1.8* instalado. Uma correção estava disponível para a versão *1.9*:

```powershell
[INFO]  Getting plugin locations for plugin 'Microsoft.Compute.CustomScriptExtension'. Current Version: '1.8', Requested Version: '1.9'
[INFO]  Auto-Upgrade mode. Highest public version for plugin 'Microsoft.Compute.CustomScriptExtension' with requested version: '1.9', is: '1.9'
```

## <a name="agent-permissions"></a>Permissões de agente

Para efetuar as respetivas tarefas, o agente tem de ser executado como *Sistema Local*.

## <a name="troubleshoot-vm-extensions"></a>Resolver problemas de extensões VM

Cada extensão VM poderá ter passos específicos para a extensão de resolução de problemas. Por exemplo, quando utiliza a extensão de Script personalizado, detalhes de execução do script podem ser encontrados localmente na VM em que a extensão foi executada. Quaisquer passos de resolução de problemas específicos da extensão estão descritos na documentação de extensão específica.

Os passos de resolução de problemas seguintes aplicam-se a todas as extensões VM.

1. Para verificar o registo do agente convidado Windows, observe a atividade quando a extensão estava a ser aprovisionado no *C:\WindowsAzure\Logs\WaAppAgent.txt*

2. Verifique os registos de extensão real para obter mais detalhes no *C:\WindowsAzure\Logs\Plugins\<extensionName >*

3. Consulte as secções de resolução de problemas de documentação específico de extensão para códigos de erro, etc de problemas conhecidos.

4. Consulte os registos de sistema. Verificação das outras operações que pode ter interfered com a extensão, tal como uma instalação de execução demorada de outra aplicação que é necessário acesso de Gestor de pacote exclusivo.

### <a name="common-reasons-for-extension-failures"></a>Os motivos mais comuns de falhas de extensão

1. Extensões têm de 20 minutos para executar (exceções são o CustomScript extensões, Chef e DSC com 90 minutos). Se a implementação exceder neste momento, está marcado como um tempo limite. A causa desta situação pode dever-se recursos baixo VMs, outro VM configurações/arranque tarefas consumir elevadas quantidades de recursos, embora a extensão está a tentar aprovisionar.

2. Pré-requisitos mínimos não foram cumpridos. Algumas extensões terem dependências de SKUs de VM, tais como imagens HPC. As extensões podem exigir determinados requisitos de acesso redes, tais como a comunicação com o Storage do Azure ou serviços de público. Outros exemplos podem ser acesso para os repositórios de pacote, a ficar sem espaço em disco ou restrições de segurança.

3. Acesso do Gestor de pacote exclusivo. Em alguns casos, poderá encontrar uma execução demorada de VM de configuração e instalação da extensão em conflito, em que ambos os necessitam de acesso exclusivo para o Gestor de pacotes.

### <a name="view-extension-status"></a>Ver o estado de extensão

Depois de uma extensão VM tiver sido executada contra uma VM, utilize [Get-AzureRmVM ](/powershell/module/azurerm.compute/get-azurermvm) para devolver o estado da extensão. *Substatuses [0]* mostra que o aprovisionamento de extensão concluída com êxito, que significa que com êxito as TI implementado para a VM, mas ocorreu uma falha na execução da extensão dentro da VM, *Substatuses [1]*.

```powershell
Get-AzureRmVM -ResourceGroupName "myResourceGroup" -VMName "myVM" -Status
```

O resultado será semelhante ao seguinte exemplo de saída:

```powershell
Extensions[0]           :
  Name                  : CustomScriptExtension
  Type                  : Microsoft.Compute.CustomScriptExtension
  TypeHandlerVersion    : 1.9
  Substatuses[0]        :
    Code                : ComponentStatus/StdOut/succeeded
    Level               : Info
    DisplayStatus       : Provisioning succeeded
    Message             : Windows PowerShell \nCopyright (C) Microsoft Corporation. All rights reserved.\n
  Substatuses[1]        :
    Code                : ComponentStatus/StdErr/succeeded
    Level               : Info
    DisplayStatus       : Provisioning succeeded
    Message             : The argument 'cseTest%20Scriptparam1.ps1' to the -File parameter does not exist. Provide the path to an existing '.ps1' file as an argument to the

-File parameter.
  Statuses[0]           :
    Code                : ProvisioningState/failed/-196608
    Level               : Error
    DisplayStatus       : Provisioning failed
    Message             : Finished executing command
```

Também é possível encontrar o estado de execução da extensão no portal do Azure. Para ver o estado de uma extensão, selecione a VM, escolha **extensões**, em seguida, selecione a extensão pretendida.

### <a name="rerun-vm-extensions"></a>Volte a executar as extensões de VM

Pode haver casos em que uma extensão VM tem de ser executada novamente. Pode executar novamente uma extensão, removendo-a e, em seguida, execute novamente a extensão com um método de execução da sua preferência. Para remover uma extensão, utilize [remover AzureRmVMExtension](/powershell/module/AzureRM.Compute/Remove-AzureRmVMExtension) da seguinte forma:

```powershell
Remove-AzureRmVMExtension -ResourceGroupName "myResourceGroup" -VMName "myVM" -Name "myExtensionName"
```

Pode também remover uma extensão no portal do Azure da seguinte forma:

1. Selecione uma VM.
2. Escolha **extensões**.
3. Selecione a extensão pretendida.
4. Escolha **desinstalar**.

## <a name="common-vm-extensions-reference"></a>Referência de extensões VM comuns
| Nome de extensão | Descrição | Mais informações |
| --- | --- | --- |
| Extensão de Script personalizado para o Windows |Executar scripts em relação a uma máquina virtual do Azure |[Extensão de Script personalizado para o Windows](custom-script-windows.md) |
| Extensão de DSC para Windows |Extensão do PowerShell DSC (configuração do estado pretendido) |[Extensão de DSC para Windows](dsc-overview.md) |
| Extensão do Diagnóstico do Azure |Gerir o diagnóstico do Azure |[Extensão do Diagnóstico do Azure](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
| Extensão de acesso VM do Azure |Gerir utilizadores e as credenciais |[Extensão de acesso VM para Linux](https://azure.microsoft.com/blog/using-vmaccess-extension-to-reset-login-credentials-for-linux-vm/) |

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre as extensões VM, consulte [descrição geral de funcionalidades e as extensões de máquina virtual do Azure](overview.md).