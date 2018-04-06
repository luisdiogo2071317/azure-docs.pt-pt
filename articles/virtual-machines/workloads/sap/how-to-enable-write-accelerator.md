---
title: Acelerador de escrita de mensagens em fila do Azure para implementações de SAP | Microsoft Docs
description: Manual de operações para sistemas de SAP HANA que são implementados em máquinas virtuais do Azure.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 177bc05eea3aa05231c71a42950fa622b68afc53
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/05/2018
---
# <a name="azure-write-accelerator-for-sap-deployments"></a>Acelerador de escrita de mensagens em fila do Azure para implementações de SAP
Acelerador de escrita do Azure é uma funcionalidade que está a obter implementada para VMs de série M exclusivamente. O acelerador de escrita do Azure não está disponível com quaisquer outro-série da VM no Azure, exceto a série M. Como o nome de Estados, o objetivo da funcionalidade é melhorar a latência de e/s de escritas contra o Premium Storage do Azure. 

>[!NOTE]
> Neste momento, o acelerador de escrita do Azure está em pré-visualização pública e necessita de listagem de branco do seu ID de subscrição do Azure

A funcionalidade de acelerador de escrita do Azure está disponível para implementação de série M como pré-visualização pública em:

- US2 oeste
- Europa ocidental
- Sudeste Asiático

## <a name="planning-for-using-azure-write-accelerator"></a>Planeamento de utilização do Azure escrever acelerador
Acelerador do Azure escrever deve ser utilizada para os volumes que contêm o registo de transações ou Refazer registos de um DBMS. Não se recomenda utilizar Azure escrever acelerador para os volumes de dados de um DBMS. Motivo para esta restrição é que o Azure escrever acelerador requer do Azure Premium Storage VHDs seja montado sem a adicionais leitura colocação em cache que está disponível para armazenamento Premium. Vantagens maiores com este tipo de colocação em cache podem ser respeitadas com bases de dados tradicionais. Uma vez que escrever acelerador só está a afetar as atividades de escrita e não acelerar leituras, a estrutura suportada para SAP é utilizar acelerador escrita contra o registo de transações ou Refazer unidades de registo de bases de dados SAP suportado. 

Acelerador do Azure escrever só funciona em conjunto com [discos geridos pelo Azure](https://azure.microsoft.com/services/managed-disks/). Por conseguinte, terá de planear em conformidade. 

>[!NOTE]
> Como a funcionalidade do Azure escrever acelerador ainda está em pré-visualização pública, sem implementações de cenário de produção são suportadas na funcionalidade ainda.

Existem limites de VHDs de armazenamento do Azure Premium por VM que pode ser suportado pelo Azure escrever acelerador. Os limites atuais são:

- 16 VHDs para um M128xx VM
- 8 VHDs para um M64xx VM

> [!IMPORTANT]
> Se pretender ativar ou desativar o Azure escrever acelerador para um volume existente que está incorporado fora vários discos de armazenamento do Azure Premium e repartidos a utilização de gestores de volume ou disco do Windows, espaços de armazenamento do Windows, Windows Escalamento horizontal servidor (SOFS), Linux LVM de ficheiros ou MDADM, todos os discos para criar o volume tem de ser ativado ou desativado para escrever acelerador nos passos separados. **Antes de ativar ou desativar a escrita acelerador em tal configuração de, encerre a VM do Azure**. 


> [!IMPORTANT]
> Para ativar o Azure escrever acelerador para um disco do Azure existente que não faça parte de uma compilação de volume fora vários discos com o disco do Windows ou gestores de volume, os espaços de armazenamento do Windows, Windows Escalamento horizontal (SOFS) do servidor de ficheiros, Linux LVM ou MDADM, a carga de trabalho a aceder a Disco do Azure tem de ser encerrada. Aplicações de base de dados utilizando o disco do Azure tem de ser encerradas.

> [!IMPORTANT]
> Ativar a escrever o acelerador para o disco do sistema de operativo Azure da VM irá reiniciar a VM. 

Ativar o Azure escrever acelerador para funcionar discos não deve ser necessário para SAP relacionadas com as configurações de VM

### <a name="restrictions-when-using-azure-write-accelerator"></a>Restrições ao utilizar o Azure escrever acelerador
Quando utilizar o Azure escrever acelerador para um disco/VHD do Azure, estas restrições aplicam-se:

- O disco Premium colocação em cache tem de ser definido como 'None'. Todos os outros modos de colocação em cache não são suportados.
- Instantâneo no disco acelerador de escrita ativado ainda não é suportado. Esta restrição bloqueia a capacidade do serviço de cópia de segurança do Azure para efetuar um instantâneo consistente da aplicação de todos os discos da máquina virtual.


## <a name="enabling-write-accelerator-on-a-specific-disk"></a>Ativar o acelerador escrever num disco específico
As secções seguintes alguns descrevem como acelerador de escrita do Azure pode ser ativada em VHDs de armazenamento do Azure Premium.

Neste ponto no tempo, a ativação escrever acelerador através da API Rest do Azure e o Power Shell é os métodos apenas. Outros métodos até suporte no Azure portal seguirá durante as próximas algumas semanas.

### <a name="prerequisites"></a>Pré-requisitos
Os pré-requisitos seguintes são aplicáveis à utilização do Azure escrever acelerador neste ponto no tempo:

- O ID de subscrição que foi utilizado para implementar a VM e armazenamento para a VM tem de ser listado em branco. Contacte o seu CSA Microsoft, GBB, ou listadas no Gestor de contas para obter o ID de subscrição em branco. 
- Os discos que pretende aplicar Azure escrever acelerador contra têm de ser [discos geridos pelo Azure](https://azure.microsoft.com/services/managed-disks/).

### <a name="enabling-through-power-shell"></a>Ativação através de Power Shell
O módulo de Power Shell do Azure da versão 5.5.0 incluem as alterações para os cmdlets relevantes para ativar ou desativar o Azure acelerador de escrita para discos Premium Storage do Azure específicos.
Para ativar ou implementar discos suportados pela escrita acelerador, os seguintes comandos de Power Shell foi alterados e expandidos para aceitar um parâmetro para escrever acelerador.

Um novo parâmetro do comutador, "WriteAccelerator" foi adicionado para os seguintes cmdlets: 

- Set-AzureRmVMOsDisk
- Add-AzureRmVMDataDisk
- Set-AzureRmVMDataDisk
- Add-AzureRmVmssDataDisk

Não fornecer o parâmetro define a propriedade como false e irá implementar os discos que não têm suporte pelo Azure escrever acelerador.

Um novo parâmetro do comutador, "OsDiskWriteAccelerator" foi adicionado para os seguintes cmdlets: 

- Set-AzureRmVmssStorageProfile

Não dar os conjuntos a propriedade como false e fornecerá discos que não a tirar partido do Azure escrever acelerador.

Um novo booleano (sem valores nulos) é o parâmetro opcional, "OsDiskWriteAccelerator" foi adicionado para os seguintes cmdlets: 

- Update-AzureRmVM
- Update-AzureRmVmss

Especifique $true ou $false para controlar o suporte do Azure escrever acelerador com os discos.

Exemplos de comandos podem ter o seguinte aspeto:

```

New-AzureRmVMConfig | Set-AzureRmVMOsDisk | Add-AzureRmVMDataDisk -Name "datadisk1" | Add-AzureRmVMDataDisk -Name "logdisk1" -WriteAccelerator | New-AzureRmVM

Get-AzureRmVM | Update-AzureRmVM -OsDiskWriteAccelerator $true

New-AzureRmVmssConfig | Set-AzureRmVmssStorageProfile -OsDiskWriteAccelerator | Add-AzureRmVmssDataDisk -Name "datadisk1" -WriteAccelerator:$false | Add-AzureRmVmssDataDisk -Name "logdisk1" -WriteAccelerator | New-AzureRmVmss

Get-AzureRmVmss | Update-AzureRmVmss -OsDiskWriteAccelerator:$false 

```

Dois cenários principais podem ser convertidos em script conforme mostrado nas seguintes secções.

#### <a name="adding--new-disk-supported-by-azure-write-accelerator"></a>Adicionar novo disco suportado pelo Azure escrever acelerador
Pode utilizar este script para adicionar um novo disco à VM. O disco criado com este script vai utilizar o Azure escrever acelerador.

```

# Specify your VM Name
$vmName="mysapVM"
#Specify your Resource Group
$rgName = "mysap"
#data disk name
$datadiskname = "log001"
#LUN Id
$lunid=8
#size
$size=1023
#Pulls the VM info for later
$vm=Get-AzurermVM -ResourceGroupName $rgname -Name $vmname
#add a new VM data disk
Add-AzureRmVMDataDisk -CreateOption empty -DiskSizeInGB $size -Name $vmname-$datadiskname -VM $vm -Caching None -WriteAccelerator:$true -lun $lunid
#Updates the VM with the disk config - does not require a reboot
Update-AzureRmVM -ResourceGroupName $rgname -VM $vm

```
Terá de adaptar os nomes de VM, o disco, o grupo de recursos, tamanho do disco LunID do disco para a sua implementação específica.


#### <a name="enabling-azure-write-accelerator-on-an-existing-azure-disk"></a>Ativar o Azure escrever acelerador um disco existente do Azure
Se precisar de ativar o acelerador escrever num disco existente, pode utilizar este script para executar a tarefa:

```

#Specify your VM Name
$vmName="mysapVM"
#Specify your Resource Group
$rgName = "mysap"
#data disk name
$datadiskname = "testsap-log001" 
#new Write Accelerator status ($true for enabled, $false for disabled) 
$newstatus = $true
#Pulls the VM info for later
$vm=Get-AzurermVM -ResourceGroupName $rgname -Name $vmname
#add a new VM data disk
Set-AzureRmVMDataDisk -VM $vm -Name $datadiskname -Caching None -WriteAccelerator:$newstatus
#Updates the VM with the disk config - does not require a reboot
Update-AzureRmVM -ResourceGroupName $rgname -VM $vm

```

Terá de adaptar os nomes de VM, o disco e o grupo de recursos. O script acima adiciona que escrever acelerador para um disco existente é que o valor para $newstatus estiver definido como '$true'. Utilizar o valor '$false' desativará acelerador escrever num disco indicado.

> [!Note]
> Executar o script acima serão desanexar o disco especificado, ativar o acelerador escrita no disco e, em seguida, anexar o disco novo




### <a name="enabling-through-rest-apis"></a>A ativação através de Rest APIs
Para implementar através da API Rest do Azure, terá de instalar o armclient do Azure

#### <a name="install-armclient"></a>Instalar armclient

Para executar armclient, tem de instalá-lo através de Chocolatey. Pode instalá-lo através de cmd.exe ou powershell. Utilize direitos elevados para estes comandos ("Executar como administrador").

Utilizar cmd.exe execute o seguinte comando:

```
@"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"
```

Utilizando a Shell de energia tem de utilizar:

```
Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

Agora pode instalar o armclient com o comando abaixo no cmd.exe ou Power Shell

```
choco install armclient
```

#### <a name="getting-your-current-vm-configuration"></a>Obter a configuração atual da VM
Para alterar os atributos da sua configuração de disco, terá primeiro de obter a configuração atual de um ficheiro JSON. Pode obter a configuração atual, executando o seguinte comando:

```
armclient GET /subscriptions/<<subscription-ID<</resourceGroups/<<ResourceGroup>>/providers/Microsoft.Compute/virtualMachines/<<virtualmachinename>>?api-version=2017-12-01 > <<filename.json>>
```
Substitua os termos de <> <>com os seus dados, incluindo o nome de ficheiro que deve ter o ficheiro JSON.

O resultado pode ter o seguinte aspeto:

```
{
  "properties": {
    "vmId": "2444c93e-f8bb-4a20-af2d-1658d9dbbbcb",
    "hardwareProfile": {
      "vmSize": "Standard_M64s"
    },
    "storageProfile": {
      "imageReference": {
        "publisher": "SUSE",
        "offer": "SLES-SAP",
        "sku": "12-SP3",
        "version": "latest"
      },
      "osDisk": {
        "osType": "Linux",
        "name": "mylittlesap_OsDisk_1_754a1b8bb390468e9b4c429b81cc5f5a",
        "createOption": "FromImage",
        "caching": "ReadWrite",
        "managedDisk": {
          "storageAccountType": "Premium_LRS",
          "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/disks/mylittlesap_OsDisk_1_754a1b8bb390468e9b4c429b81cc5f5a"
        },
        "diskSizeGB": 30
      },
      "dataDisks": [
        {
          "lun": 0,
          "name": "data1",
          "createOption": "Attach",
          "caching": "None",
          "managedDisk": {
            "storageAccountType": "Premium_LRS",
            "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/disks/data1"
          },
          "diskSizeGB": 1023
        },
        {
          "lun": 1,
          "name": "log1",
          "createOption": "Attach",
          "caching": "None",
          "managedDisk": {
            "storageAccountType": "Premium_LRS",
            "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/disks/data2"
          },
          "diskSizeGB": 1023
        }
      ]
    },
    "osProfile": {
      "computerName": "mylittlesapVM",
      "adminUsername": "pl",
      "linuxConfiguration": {
        "disablePasswordAuthentication": false
      },
      "secrets": []
    },
    "networkProfile": {
      "networkInterfaces": [
        {
          "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Network/networkInterfaces/mylittlesap518"
        }
      ]
    },
    "diagnosticsProfile": {
      "bootDiagnostics": {
        "enabled": true,
        "storageUri": "https://mylittlesapdiag895.blob.core.windows.net/"
      }
    },
    "provisioningState": "Succeeded"
  },
  "type": "Microsoft.Compute/virtualMachines",
  "location": "westeurope",
  "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/virtualMachines/mylittlesapVM",
  "name": "mylittlesapVM"

```

Passo seguinte é atualizar o ficheiro JSON e ativar o acelerador escrita no disco chamado 'log1'. Pode fazê-lo ao adicionar este atributo para o ficheiro JSON após a entrada de cache do disco. 

```
        {
          "lun": 1,
          "name": "log1",
          "createOption": "Attach",
          "caching": "None",
          **"writeAcceleratorEnabled": true,**
          "managedDisk": {
            "storageAccountType": "Premium_LRS",
            "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/disks/data2"
          },
          "diskSizeGB": 1023
        }
```

Em seguida, atualize a implementação existente com este comando:

```
armclient PUT /subscriptions/<<subscription-ID<</resourceGroups/<<ResourceGroup>>/providers/Microsoft.Compute/virtualMachines/<<virtualmachinename>>?api-version=2017-12-01 @<<filename.json>>

```

O resultado deverá ser semelhante o um abaixo. Pode ver o que é acelerador de escrita de mensagens em fila ativada para um disco.

```
{
  "properties": {
    "vmId": "2444c93e-f8bb-4a20-af2d-1658d9dbbbcb",
    "hardwareProfile": {
      "vmSize": "Standard_M64s"
    },
    "storageProfile": {
      "imageReference": {
        "publisher": "SUSE",
        "offer": "SLES-SAP",
        "sku": "12-SP3",
        "version": "latest"
      },
      "osDisk": {
        "osType": "Linux",
        "name": "mylittlesap_OsDisk_1_754a1b8bb390468e9b4c429b81cc5f5a",
        "createOption": "FromImage",
        "caching": "ReadWrite",
        "managedDisk": {
          "storageAccountType": "Premium_LRS",
          "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/disks/mylittlesap_OsDisk_1_754a1b8bb390468e9b4c429b81cc5f5a"
        },
        "diskSizeGB": 30
      },
      "dataDisks": [
        {
          "lun": 0,
          "name": "data1",
          "createOption": "Attach",
          "caching": "None",
          "managedDisk": {
            "storageAccountType": "Premium_LRS",
            "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/disks/data1"
          },
          "diskSizeGB": 1023
        },
        {
          "lun": 1,
          "name": "log1",
          "createOption": "Attach",
          "caching": "None",
          **"writeAcceleratorEnabled": true,**
          "managedDisk": {
            "storageAccountType": "Premium_LRS",
            "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/disks/data2"
          },
          "diskSizeGB": 1023
        }
      ]
    },
    "osProfile": {
      "computerName": "mylittlesapVM",
      "adminUsername": "pl",
      "linuxConfiguration": {
        "disablePasswordAuthentication": false
      },
      "secrets": []
    },
    "networkProfile": {
      "networkInterfaces": [
        {
          "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Network/networkInterfaces/mylittlesap518"
        }
      ]
    },
    "diagnosticsProfile": {
      "bootDiagnostics": {
        "enabled": true,
        "storageUri": "https://mylittlesapdiag895.blob.core.windows.net/"
      }
    },
    "provisioningState": "Succeeded"
  },
  "type": "Microsoft.Compute/virtualMachines",
  "location": "westeurope",
  "id": "/subscriptions/XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX/resourceGroups/mylittlesap/providers/Microsoft.Compute/virtualMachines/mylittlesapVM",
  "name": "mylittlesapVM"

```

Do ponto da alteração no, a unidade deve ser suportada pela escrita acelerador.

 