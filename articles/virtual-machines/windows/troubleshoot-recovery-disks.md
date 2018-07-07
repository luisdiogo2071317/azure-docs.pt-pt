---
title: Utilizar um Windows resolver problemas de VMS com o Azure PowerShell | Documentos da Microsoft
description: Saiba como resolver problemas de VM do Windows no Azure ao ligar o disco do SO a uma VM com o Azure PowerShell de recuperação
services: virtual-machines-windows
documentationCenter: ''
authors: genlin
manager: jeconnoc
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/03/2017
ms.author: genli
ms.openlocfilehash: 1e87704e7d8cf3c7cc21e537d36f95a97265061b
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/07/2018
ms.locfileid: "37903521"
---
# <a name="troubleshoot-a-windows-vm-by-attaching-the-os-disk-to-a-recovery-vm-using-azure-powershell"></a>Resolver problemas de uma VM do Windows ao anexar o disco do SO a uma VM com o Azure PowerShell de recuperação
Se a sua máquina virtual (VM) do Windows no Azure encontra um erro de arranque ou disco, se pretender executar passos de resolução de problemas no disco rígido virtual em si. Um exemplo comum seria uma atualização de aplicação com falhas que impede que a VM que está a ser capazes de arrancar com êxito. Este artigo fornece detalhes sobre como utilizar o Azure PowerShell para ligar o seu disco rígido virtual a outra VM do Windows para corrigir quaisquer erros, em seguida, voltar a criar a VM original.


## <a name="recovery-process-overview"></a>Descrição geral do processo de recuperação
O processo de resolução de problemas é o seguinte:

1. Elimine a VM se deparar com problemas, mantendo os discos rígidos virtuais.
2. Anexe e Monte o disco rígido virtual a outra VM do Windows para fins de resolução de problemas.
3. Ligue à VM da resolução de problemas. Editar ficheiros ou execute quaisquer ferramentas para corrigir problemas no disco rígido virtual original.
4. Desmonte e desanexe o disco rígido virtual da VM de resolução de problemas.
5. Crie uma VM com o disco rígido virtual original.

Para a VM que utiliza o disco gerido, consulte [resolver problemas de uma VM de disco gerido ao anexar um disco de SO novo](#troubleshoot-a-managed-disk-vm-by-attaching-a-new-os-disk).


Certifique-se de que tenha [o Azure PowerShell mais recente](/powershell/azure/overview) instalado e iniciado sessão sua subscrição:

```powershell
Connect-AzureRmAccount
```

Nos exemplos a seguir, substitua os nomes de parâmetros com seus próprios valores. Os nomes de parâmetros de exemplo incluem `myResourceGroup`, `mystorageaccount`, e `myVM`.


## <a name="determine-boot-issues"></a>Determinar os problemas de arranque
Pode ver uma captura de ecrã da sua VM no Azure para ajudar a resolver problemas de arranque. Nesta captura de ecrã pode ajudar a identificar o motivo da falha de uma VM efetuar o arranque. O exemplo seguinte obtém a captura de ecrã da VM Windows com o nome `myVM` no grupo de recursos com o nome `myResourceGroup`:

```powershell
Get-AzureRmVMBootDiagnosticsData -ResourceGroupName myResourceGroup `
    -Name myVM -Windows -LocalPath C:\Users\ops\
```

Reveja a captura de ecrã para determinar por que a VM está a falhar efetuar o arranque. Tenha em atenção que quaisquer mensagens de erro específicas ou códigos de erro fornecidos.


## <a name="view-existing-virtual-hard-disk-details"></a>Ver detalhes do disco rígido virtual existente
Pode anexar o disco rígido virtual a outra VM, terá de identificar o nome do disco rígido virtual (VHD).

O exemplo seguinte obtém informações para a VM com o nome `myVM` no grupo de recursos com o nome `myResourceGroup`:

```powershell
Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
```

Procure `Vhd URI` dentro do `StorageProfile` seção da saída do comando anterior. O seguinte truncados saída de exemplo mostra o `Vhd URI` até o final do bloco de código:

```powershell
RequestId                     : 8a134642-2f01-4e08-bb12-d89b5b81a0a0
StatusCode                    : OK
ResourceGroupName             : myResourceGroup
Id                            : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM
Name                          : myVM
Type                          : Microsoft.Compute/virtualMachines
...
StorageProfile                :
  ImageReference              :
    Publisher                 : MicrosoftWindowsServer
    Offer                     : WindowsServer
    Sku                       : 2016-Datacenter
    Version                   : latest
  OsDisk                      :
    OsType                    : Windows
    Name                      : myVM
    Vhd                       :
      Uri                     : https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
    Caching                   : ReadWrite
    CreateOption              : FromImage
```


## <a name="delete-existing-vm"></a>Eliminar VM existente
Os discos rígidos virtuais e as VMs são dois recursos diferentes do Azure. Um disco rígido virtual é onde o próprio sistema operacional, aplicativos e configurações são armazenadas. A VM propriamente dito é apenas de metadados que definem o tamanho ou a localização e faz referência a recursos como um disco rígido virtual ou uma placa de interface de rede virtual (NIC). Cada disco rígido virtual tem uma concessão atribuída quando anexado a uma VM. Embora os discos de dados possam ser anexados e desanexados mesmo se a VM estiver a ser executada, o disco do SO só pode ser desanexado se o recurso da VM for eliminado. A concessão continua a associar o disco do SO a uma VM, mesmo quando a VM está num estado parado e desalocado.

O primeiro passo para recuperar a sua VM é eliminar o próprio recurso da VM. Com a eliminação, os discos rígidos virtuais permanecem na sua conta de armazenamento. Depois da VM é eliminada, é anexar o disco rígido virtual a outra VM para resolver os erros.

O exemplo seguinte elimina a VM com o nome `myVM` do grupo de recursos com o nome `myResourceGroup`:

```powershell
Remove-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
```

Aguarde até que a VM tiver concluído a eliminação do antes de anexar o disco rígido virtual a outra VM. A concessão no disco rígido virtual que associa-a com a VM precisa ser liberada antes de pode anexar o disco rígido virtual a outra VM.


## <a name="attach-existing-virtual-hard-disk-to-another-vm"></a>Anexar o disco rígido virtual existente a outra VM
Para os passos seguintes, utilize outra VM para fins de resolução de problemas. Anexar o disco rígido virtual existente para esta VM de resolução de problemas para procurar e editar o conteúdo do disco. Este processo permite-lhe corrigir os eventuais erros de configuração ou analisar o aplicativo adicional ou ficheiros de registo do sistema, por exemplo. Escolha ou crie outra VM para utilizar para fins de resolução de problemas.

Ao anexar o disco rígido virtual existente, especificar o URL para o disco de obteve no anterior `Get-AzureRmVM` comando. O exemplo seguinte anexa um disco rígido virtual existente para a VM de resolução de problemas com o nome `myVMRecovery` no grupo de recursos com o nome `myResourceGroup`:

```powershell
$myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMRecovery"
Add-AzureRmVMDataDisk -VM $myVM -CreateOption "Attach" -Name "DataDisk" -DiskSizeInGB $null `
    -VhdUri "https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd"
Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
```

> [!NOTE]
> Adicionar um disco exige que especifique o tamanho do disco. Como podemos anexar um disco existente, o `-DiskSizeInGB` é especificado como `$null`. Este valor garante que o disco de dados está corretamente ligado e sem a necessidade de determinar o verdadeiro tamanho do disco de dados.


## <a name="mount-the-attached-data-disk"></a>Monte o disco de dados anexados

1. RDP para a VM de resolução de problemas usando as credenciais apropriadas. O exemplo seguinte transfere o ficheiro de ligação de RDP para a VM com o nome `myVMRecovery` no grupo de recursos com o nome `myResourceGroup`e transfere-o para `C:\Users\ops\Documents`"

    ```powershell
    Get-AzureRMRemoteDesktopFile -ResourceGroupName "myResourceGroup" -Name "myVMRecovery" `
        -LocalPath "C:\Users\ops\Documents\myVMRecovery.rdp"
    ```

2. O disco de dados é automaticamente detectado e ligado. Ver a lista de volumes anexados para determinar a letra de unidade da seguinte forma:

    ```powershell
    Get-Disk
    ```

    O resultado de exemplo seguinte mostra o disco rígido virtual ligado um disco **2**. (Também pode usar `Get-Volume` para ver a letra de unidade):

    ```powershell
    Number   Friendly Name   Serial Number   HealthStatus   OperationalStatus   Total Size   Partition
                                                                                             Style
    ------   -------------   -------------   ------------   -----------------   ----------   ----------
    0        Virtual HD                                     Healthy             Online       127 GB MBR
    1        Virtual HD                                     Healthy             Online       50 GB MBR
    2        Msft Virtu...                                  Healthy             Online       127 GB MBR
    ```

## <a name="fix-issues-on-original-virtual-hard-disk"></a>Corrigir erros no disco rígido virtual original
Com o disco rígido virtual existente montado, pode agora efetuar qualquer manutenção e passos de resolução de problemas, conforme necessário. Depois de corrigir esses problemas, avance para os passos seguintes.


## <a name="unmount-and-detach-original-virtual-hard-disk"></a>Desmonte e desanexe o disco rígido virtual original
Assim que os erros são resolvidos, desmonte e desanexe o disco rígido virtual existente da sua VM de resolução de problemas. Não é possível utilizar o seu disco rígido virtual com qualquer outra VM até que a anexar o disco rígido virtual para a VM de resolução de problemas de concessão será libertada.

1. Da sua sessão do RDP, desmonte o disco de dados na sua VM de recuperação. Terá do número do disco do anterior `Get-Disk` cmdlet. Em seguida, utilize `Set-Disk` para configurar o disco como offline:

    ```powershell
    Set-Disk -Number 2 -IsOffline $True
    ```

    Confirmar o disco está agora definido como offline usando `Get-Disk` novamente. O resultado de exemplo seguinte mostra que o disco está agora definido como offline:

    ```powershell
    Number   Friendly Name   Serial Number   HealthStatus   OperationalStatus   Total Size   Partition
                                                                                             Style
    ------   -------------   -------------   ------------   -----------------   ----------   ----------
    0        Virtual HD                                     Healthy             Online       127 GB MBR
    1        Virtual HD                                     Healthy             Online       50 GB MBR
    2        Msft Virtu...                                  Healthy             Offline      127 GB MBR
    ```

2. A sua sessão do RDP de saída. Da sua sessão do Azure PowerShell, remova o disco rígido virtual a partir da VM de resolução de problemas.

    ```powershell
    $myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMRecovery"
    Remove-AzureRmVMDataDisk -VM $myVM -Name "DataDisk"
    Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
    ```


## <a name="create-vm-from-original-hard-disk"></a>Criar VM a partir de disco rígido original
Para criar uma VM a partir do seu disco rígido virtual original, utilize [este modelo do Azure Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet). O modelo JSON tenha a seguinte hiperligação:

- https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-specialized-vhd-new-or-existing-vnet/azuredeploy.json

O modelo implementa uma VM numa rede virtual existente, utilizando o URL de VHD do comando anterior. O exemplo seguinte implementa o modelo para o grupo de recursos com o nome `myResourceGroup`:

```powershell
New-AzureRmResourceGroupDeployment -Name myDeployment -ResourceGroupName myResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd-existing-vnet/azuredeploy.json
```

Responda aos pedidos para o modelo, como o nome da VM, o tipo de SO e o tamanho da VM. O `osDiskVhdUri` é o mesmo conforme anteriormente usados ao anexar o disco rígido virtual existente para a VM de resolução de problemas.


## <a name="re-enable-boot-diagnostics"></a>Reativar o diagnóstico de arranque

Quando criar a VM a partir do disco rígido virtual existente, o diagnóstico de arranque pode não ser ativado automaticamente. O exemplo seguinte ativa a extensão de diagnóstico na VM com o nome `myVMDeployed` no grupo de recursos com o nome `myResourceGroup`:

```powershell
$myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMDeployed"
Set-AzureRmVMBootDiagnostics -ResourceGroupName myResourceGroup -VM $myVM -enable
Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
```

## <a name="troubleshoot-a-managed-disk-vm-by-attaching-a-new-os-disk"></a>Resolver problemas de uma VM de disco gerido ao anexar um novo disco de SO
1. Pare a VM resultante do Windows de disco gerido.
2. [Criar um instantâneo do disco gerido](snapshot-copy-managed-disk.md) do disco do SO da VM de disco gerido.
3. [Criar um disco gerido a partir do instantâneo](../scripts/virtual-machines-windows-powershell-sample-create-managed-disk-from-snapshot.md).
4. [Anexar o disco gerido como disco de dados da VM](attach-disk-ps.md).
5. [Alterar o disco de dados do passo 4 para o disco de SO](os-disk-swap.md).

## <a name="next-steps"></a>Passos Seguintes
Se estiver a ter problemas de ligação à sua VM, veja [ligações de resolução de problemas de RDP para uma VM do Azure](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Para problemas com o acesso aos aplicativos em execução na sua VM, consulte [resolver problemas de conectividade de aplicações numa VM do Windows](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Para obter mais informações sobre como utilizar o Resource Manager, consulte [descrição geral do Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
