---
title: Extensão de VM Windows de instantâneo de cópia de segurança do Azure | Microsoft Docs
description: Efetuar cópia de segurança consistente do aplicação da máquina virtual do Backup do Azure utilizando a extensão de instantâneos VM
services: backup, virtual-machines-windows
documentationcenter: ''
author: trinadhk
manager: jeconnoc
editor: ''
ms.assetid: 57759670-0baa-44db-ae14-8cdc00d3a906
ms.service: backup, virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 3/26/2018
ms.author: trinadhk
ms.openlocfilehash: 8426a2472a28cf287dfe574cb80da56108394ae8
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="vm-snapshot-windows-extension-for-azure-backup"></a>Extensão de VM Windows de instantâneo de cópia de segurança do Azure

## <a name="overview"></a>Descrição geral

Cópia de segurança do Azure fornece suporte para cópia de segurança das cargas de trabalho no local à nuvem e de cópia de segurança de recursos na nuvem para o Cofre dos serviços de recuperação. Cópia de segurança do Azure utiliza a extensão de instantâneos VM para efetuar uma cópia de segurança consistentes de máquina virtual do Azure, sem a necessidade de aplicação para encerrar a VM. Extensão de instantâneo VM é publicado e suportado pela Microsoft como parte do serviço de cópia de segurança do Azure. Cópia de segurança do Azure irá instalar a extensão como parte da primeira agendada cópia de segurança accionadas após ativar a cópia de segurança. Este documento fornece detalhes sobre as plataformas suportadas, configurações e opções de implementação para a extensão de instantâneo VM.

## <a name="prerequisites"></a>Pré-requisitos

### <a name="operating-system"></a>Sistema operativo
Para obter uma lista dos sistemas operativos suportados, consulte [sistemas operativos suportados pela cópia de segurança do Azure](../../backup/backup-azure-arm-vms-prepare.md#supported-operating-systems-for-backup)

### <a name="internet-connectivity"></a>Conectividade Internet

Extensão de instantâneo VM requer que a máquina virtual de destino está ligada à internet quando podemos fazer uma cópia da máquina virtual.

## <a name="extension-schema"></a>Esquema de extensão

O JSON seguinte mostra o esquema para a extensão de instantâneos VM. A extensão requer o ID de tarefa - esta identifica a tarefa cópia de segurança que acionou tire um instantâneo da VM, uri de blob de Estado - onde o estado da operação instantâneo é escrito, agendar hora de início do instantâneo, os registos de blob uri - onde registos correspondente instantâneo tarefas são escritos, objstr-representação dos dados de metadados e os discos VM.  Porque estas definições devem ser tratadas como dados confidenciais, devem ser armazenado numa configuração de definição protegido. Dados da definição de extensão protegido de VM do Azure é encriptados e desencriptados apenas na máquina virtual de destino. Tenha em atenção que estas definições são recomendadas para ser transmitidas do serviço de cópia de segurança do Azure apenas como parte da tarefa de cópia de segurança.

```json
{
  "type": "extensions",
  "name": "VMSnapshot",
  "location":"<myLocation>",
  "properties": {
    "publisher": "Microsoft.RecoveryServices",
    "type": "VMSnapshot",
    "typeHandlerVersion": "1.9",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "locale":"<location>",
      "taskId":"<taskId used by Azure Backup service to communicate with extension>",
      "commandToExecute": "snapshot",
      "commandStartTimeUTCTicks": "<scheduled start time of the snapshot task>",
      "vmType": "microsoft.compute/virtualmachines"
    },
    "protectedSettings": {
      "objectStr": "<blob SAS uri represenattion of VM sent by Azure Backup service to extension>",
      "logsBlobUri": "<blob uri where logs of command execution by extension are written to>",
      "statusBlobUri": "<blob uri where status of the command executed by extension is written>"
    }
  }
}
```

### <a name="property-values"></a>Valores de propriedade

| Nome | Valor / exemplo | Tipo de Dados |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | data |
| taskId | e07354cf-041e-4370-929f-25a319ce8933_1 | cadeia |
| commandStartTimeUTCTicks | 6.36458E + 17 | cadeia |
| Região | pt-PT | cadeia |
| objectStr | Codificação de matriz de uri de sas-"blobSASUri": ["https:\/\/sopattna5365.blob.core.windows.net\/vhds\/vmwin1404ltsc201652903941.vhd? sv = 2014-02-14 & sr = b & sig = % De TywkROXL1zvhXcLujtCut8g3jTpgbE6JpSWRLZxAdtA 3D & st = 2017-11-09T14% 3A23% 3A28Z & zar = 2017-11-09T17% 3A38% 3A28Z & SP2 = rw "," https:\/\/sopattna8461.blob.core.windows.net\/vhds\/vmwin1404ltsc-20160629-122418.vhd? sv = 2014-02-14 & sr = b & sig = 5S0A6YDWvVwqPAkzWXVy % 2BS % 2FqMwzFMbamT5upwx05v8Q % 3D & st = 2017-11-09T14% 3A23% 3A28Z & zar = 2017-11-09T17% 3A38% 3A28Z & SP2 = rw "," https:\/ \/ sopattna8461.blob.Core.Windows.NET\/bootdiagnostics-vmwintu1-deb58392-ed5e-48be-9228-ff681b0cd3ee\/vmubuntu1404ltsc-20160629-122541.vhd? sv = 2014-02-14 & sr = b & sig = % De X0Me2djByksBBMVXMGIUrcycvhQSfjYvqKLeRA7nBD4 3D & st = 2017-11-09T14% 3A23% 3A28Z & zar = 2017-11-09T17% 3A38% 3A28Z & SP2 = rw "," https:\/\/sopattna5365.blob.core.windows.net\/vhds\/vmwin1404ltsc-20160701-163922.vhd? sv = 2014-02-14 & sr = b & sig = oXvtK2IXCNqWv7fpjc7TAzFDpc1GoXtT7r % 2BC % 2BNIAork % 3D & st = 2017-11-09T14% 3A23% 3A28Z & zar = 2017-11-09T17% 3A38% 3A28Z & SP2 = rw "," https:\/ \/ sopattna5365.blob.Core.Windows.NET\/vhds\/vmwin1404ltsc-20170705-124311.vhd? sv = 2014-02-14 & sr = b & sig = ZUM9d28Mvvm % 2FfrhJ71TFZh0Ni90m38bBs3zMl % 2FQ9rs0% 3D & st = 2017-11-09T14% 3A23% 3A28Z & Se = 2017-11-09T17% 3A38% 3A28Z & SP2 = rw "] | cadeia |
| logsBlobUri | https://seapod01coord1exsapk732.blob.core.windows.net/bcdrextensionlogs-d45d8a1c-281e-4bc8-9d30-3b25176f68ea/sopattna-vmubuntu1404ltsc.v2.Logs.txt?sv=2014-02-14&sr=b&sig=DbwYhwfeAC5YJzISgxoKk%2FEWQq2AO1vS1E0rDW%2FlsBw%3D&st=2017-11-09T14%3A33%3A29Z&se=2017-11-09T17%3A38%3A29Z&sp=rw | cadeia |
| statusBlobUri | https://seapod01coord1exsapk732.blob.core.windows.net/bcdrextensionlogs-d45d8a1c-281e-4bc8-9d30-3b25176f68ea/sopattna-vmubuntu1404ltsc.v2.Status.txt?sv=2014-02-14&sr=b&sig=96RZBpTKCjmV7QFeXm5IduB%2FILktwGbLwbWg6Ih96Ao%3D&st=2017-11-09T14%3A33%3A29Z&se=2017-11-09T17%3A38%3A29Z&sp=rw | cadeia |



## <a name="template-deployment"></a>Implementação de modelos

Extensões VM do Azure podem ser implementadas com modelos Azure Resource Manager. No entanto, a forma recomendada de adicionar uma extensão de instantâneos VM a uma máquina virtual é através da cópia de segurança na máquina virtual. Isto pode ser conseguido através de um modelo do Resource Manager.  Um modelo de Gestor de recursos de exemplo que permite a cópia de segurança numa máquina virtual pode ser encontrado no [Galeria de início rápido de Azure](https://azure.microsoft.com/resources/templates/101-recovery-services-backup-vms/).


## <a name="azure-cli-deployment"></a>Implementação da CLI do Azure

Pode ser utilizada a CLI do Azure para ativar a cópia de segurança numa máquina virtual. Cópia de segurança de ativar POST, a primeira tarefa de cópia de segurança agendada instalará a extensão de instantâneos de Vm na VM.

```azurecli
az backup protection enable-for-vm \
    --resource-group myResourceGroup \
    --vault-name myRecoveryServicesVault \
    --vm myVM \
    --policy-name DefaultPolicy
```

## <a name="troubleshoot-and-support"></a>Resolver problemas e suporte

### <a name="troubleshoot"></a>Resolução de problemas

É possível obter dados sobre o estado das implementações de extensão do portal do Azure e utilizando a CLI do Azure. Para ver o estado de implementação das extensões para uma determinada VM, execute o seguinte comando utilizando a CLI do Azure.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Resultado da execução de extensão é registado para o ficheiro seguinte:

```
C:\Packages\Plugins\Microsoft.Azure.RecoveryServices.VMSnapshot
```

### <a name="error-codes-and-their-meanings"></a>Códigos de erro e os respetivos significados

Informações de resolução de problemas pode ser encontrada no [cópia de segurança de VM do Azure guia de resolução de problemas](../../backup/backup-azure-vms-troubleshoot.md).

### <a name="support"></a>Suporte

Se precisar de mais ajuda, a qualquer altura neste artigo, pode contactar as especialistas do Azure no [fóruns do MSDN Azure e Stack Overflow](https://azure.microsoft.com/support/forums/). Em alternativa, pode ficheiro um incidente de suporte do Azure. Vá para o [site de suporte do Azure](https://azure.microsoft.com/support/options/) e selecione o suporte de Get. Para informações sobre como utilizar o suporte do Azure, leia o [suporte do Microsoft Azure FAQ](https://azure.microsoft.com/support/faq/).
