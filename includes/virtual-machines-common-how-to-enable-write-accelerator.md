---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines
author: msraiye
ms.service: virtual-machines
ms.topic: include
ms.date: 04/30/2018
ms.author: raiye
ms.custom: include file
ms.openlocfilehash: 54faa5a50b3fe965bc7f95fc0da0fdda9388412f
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/01/2018
---
# <a name="write-accelerator"></a>Escrever o acelerador
Escreva que acelerador é uma capacidade de disco para a série M máquinas virtuais (VMs) no armazenamento Premium com discos gerida do Azure exclusivamente. Como o nome de Estados, o objetivo da funcionalidade é melhorar a latência de e/s de escritas contra o Premium Storage do Azure. Escreva que acelerador Idealmente é adequada em que as atualizações de ficheiro de registo são necessárias para manter o disco de uma forma de performant elevada disponibilidade para bases de dados modernas.

Escreva que acelerador estiver geralmente disponível para VMs de série M na nuvem pública.

## <a name="planning-for-using-write-accelerator"></a>Planeamento de utilização escrever acelerador
Escreva que acelerador deve ser utilizada para os volumes que contêm o registo de transações ou Refazer registos de um DBMS. Não se recomenda utilizar escrever acelerador para os volumes de dados de um DBMS como a funcionalidade foi otimizada para ser utilizada em discos de registo.

Escrever acelerador só funciona em conjunto com [discos geridos pelo Azure](https://azure.microsoft.com/services/managed-disks/). 


> [!IMPORTANT]
> Se pretender ativar ou desativar a escrita acelerador para um volume existente que está incorporado fora vários discos de armazenamento do Azure Premium e repartidos a utilização de gestores de volume ou disco do Windows, espaços de armazenamento do Windows, Windows Escalamento horizontal ficheiro server (SOFS), Linux LVM ou MDADM, todos os discos de criar o volume tem de ser ativados ou desativados para escrever acelerador nos passos separados. **Antes de ativar ou desativar a escrita acelerador em tal configuração de, encerre a VM do Azure**. 


> [!IMPORTANT]
> Para ativar a escrever acelerador para um disco do Azure existente que não faz parte de uma compilação de volume fora vários discos com os gestores de volume ou disco do Windows, Windows os espaços de armazenamento, o servidor de ficheiros de escalamento horizontal do Windows (SOFS), LVM do Linux ou MDADM, a carga de trabalho ao aceder ao disco do Azure tem de ser encerrada. Aplicações de base de dados utilizando o disco do Azure tem de ser encerradas.

> [!IMPORTANT]
> Ativar a escrever o acelerador para o disco de sistema operativo da VM irá reiniciar a VM. 

Ativar o acelerador escrita para discos de SO não devem ser necessários para SAP relacionadas com as configurações de VM

### <a name="restrictions-when-using-write-accelerator"></a>Restrições ao utilizar escrever acelerador
Quando utilizar escrever acelerador para um disco/VHD do Azure, estas restrições aplicam-se:

- O disco de Premium colocação em cache tem de ser definido como 'None' ou 'Read Only'. Todos os outros modos de colocação em cache não são suportados.
- Instantâneo no disco acelerador de escrita ativado ainda não é suportado. Esta restrição bloqueia a capacidade do serviço de cópia de segurança do Azure para efetuar um instantâneo consistente da aplicação de todos os discos da máquina virtual.
- Apenas os tamanhos de e/s mais pequenos são colocar o caminho na melhoria. Numa carga de trabalho situações em que está a obter em massa dados carregado ou onde memórias intermédias de registo de transação do DBMS diferente estão preenchidas para um maior grau antes de obter persistente para o armazenamento, de possibilidades são de que a e/s escritos no disco não está a ser o caminho na melhoria.

Existem limites de VHDs de armazenamento do Azure Premium por VM que pode ser suportado pelo escrever acelerador. Os limites atuais são:

| SKU DE VM | Número de discos de escrever o acelerador | Escrever acelerador disco IOPS por VM |
| --- | --- | --- |
| M128ms | 16 | 8000 |
| M128s | 16 | 8000 |
| M64ms | 8 | 4000 |
| M64s | 8 | 4000 | 

## <a name="enabling-write-accelerator-on-a-specific-disk"></a>Ativar o acelerador escrever num disco específico
As secções seguintes alguns descrevem como escrever acelerador pode ser ativada em VHDs de armazenamento do Azure Premium.


### <a name="prerequisites"></a>Pré-requisitos
Os seguintes pré-requisitos aplicam-se à utilização do escrever acelerador neste ponto no tempo:

- Os discos que pretende aplicar Azure escrever acelerador contra têm de ser [discos geridos pelo Azure](https://azure.microsoft.com/services/managed-disks/) no armazenamento Premium.
- Tem de utilizar uma VM de série M

### <a name="enabling-through-power-shell"></a>Ativação através de Power Shell
O módulo de Power Shell do Azure da versão 5.5.0 incluem as alterações para os cmdlets relevantes para ativar ou desativar a escrita acelerador para discos Premium Storage do Azure específicos.
Para ativar ou implementar discos suportados pela escrita acelerador, os seguintes comandos de Power Shell foi alterados e expandidos para aceitar um parâmetro para escrever acelerador.

Um novo parâmetro do comutador, "WriteAccelerator" foi adicionado para os seguintes cmdlets: 

- Set-AzureRmVMOsDisk
- Add-AzureRmVMDataDisk
- Set-AzureRmVMDataDisk
- Add-AzureRmVmssDataDisk

Não fornecer o parâmetro define a propriedade como false e irá implementar os discos que não têm suporte por escrever acelerador.

Um novo parâmetro do comutador, "OsDiskWriteAccelerator" foi adicionado para os seguintes cmdlets: 

- Set-AzureRmVmssStorageProfile

Não lhe confere a paramenter define a propriedade como false e fornecerá discos que não a tirar partido de escrita acelerador.

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

#### <a name="adding--new-disk-supported-by-write-accelerator"></a>Adicionar novo disco suportado pelo escrever acelerador
Pode utilizar este script para adicionar um novo disco à VM. O disco criado com este script vai utilizar escrever acelerador.

```

# Specify your VM Name
$vmName="myVM"
#Specify your Resource Group
$rgName = "myWAVMs"
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
$vmName="myVM"
#Specify your Resource Group
$rgName = "myWAVMs"
#data disk name
$datadiskname = "test-log001" 
#new Write Accelerator status ($true for enabled, $false for disabled) 
$newstatus = $true
#Pulls the VM info for later
$vm=Get-AzurermVM -ResourceGroupName $rgname -Name $vmname
#add a new VM data disk
Set-AzureRmVMDataDisk -VM $vm -Name $datadiskname -Caching None -WriteAccelerator:$newstatus
#Updates the VM with the disk config - does not require a reboot
Update-AzureRmVM -ResourceGroupName $rgname -VM $vm

```

Terá de adaptar os nomes de VM, o disco e o grupo de recursos. O script acima adiciona escrever acelerador para um disco existente, em que o valor para $newstatus está definido como '$true'. Utilizar o valor '$false' desativará acelerador escrever num disco indicado.

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

Passo seguinte é atualizar o ficheiro JSON e ativar o acelerador escrita no disco chamado 'log1'. Este passo pode ser conseguido ao adicionar este atributo para o ficheiro JSON após a entrada de cache do disco. 

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

 
