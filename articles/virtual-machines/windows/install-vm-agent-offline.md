---
title: Instalar o agente da VM do Azure no modo offline | Microsoft Docs
description: Saiba como instalar o agente da VM do Azure no modo offline.
services: virtual-machines-windows
documentationcenter: ''
author: genlin
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: genli
ms.openlocfilehash: 8b9d1ea3e4b5bf959484a737453d7f6f1f63d1e6
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="install-the-azure-virtual-machine-agent-in-offline-mode"></a>Instalar o agente da Máquina Virtual do Azure no modo offline 

O agente da Máquina Virtual do Azure (agente da VM) fornece funcionalidades úteis, tais como a reposição de palavra-passe de administrador local e scripts atribuição. Este artigo mostra como instalar o agente da VM para uma máquina de virtual offline do Windows (VM). 

## <a name="when-to-use-the-vm-agent-in-offline-mode"></a>Quando utilizar o agente da VM no modo offline

Instale o agente da VM no modo offline nos seguintes cenários:

- VM do Azure implementadas não tem instalado o agente de VM ou o agente não está a funcionar.
- Se esqueceu a palavra-passe de administrador para a VM ou não é possível aceder a VM.

## <a name="how-to-install-the-vm-agent-in-offline-mode"></a>Como instalar o agente da VM no modo offline

Utilize os seguintes passos para instalar o agente da VM no modo offline.

### <a name="step-1-attach-the-os-disk-of-the-vm-to-another-vm-as-a-data-disk"></a>Passo 1: Anexar o disco do SO da VM para outra VM, como um disco de dados

1.  Elimine a VM. Certifique-se de selecionar o **manter os discos** opção ao eliminar a VM.

2.  Anexar o disco de SO como um disco de dados para outra VM (conhecido como um _troubleshooter_ VM). Para obter mais informações, consulte [anexar um disco de dados para uma VM do Windows no portal do Azure](attach-managed-disk-portal.md).

3.  Ligar para o troubleshooter VM. Abra **gestão de computadores** > **disco gestão**. Certifique-se de que o disco do SO está online e de que as letras de unidade são atribuídas para as partições de disco.

### <a name="step-2-modify-the-os-disk-to-install-the-azure-vm-agent"></a>Passo 2: Modificar o disco de SO para instalar o agente da VM do Azure

1.  Efetue uma ligação de ambiente de trabalho remoto a troubleshooter VM.

2.  O disco de SO que anexou, procure a pasta de \windows\system32\config. Copie todos os ficheiros nesta pasta como uma cópia de segurança, no caso de uma reversão não é necessária.

3.  Iniciar o **Editor de registo** (regedit.exe).

4.  Selecione o **HKEY_LOCAL_MACHINE** chave. No menu, selecione **ficheiro** > **carga Hive**:

    ![Carregar o ramo de registo](./media/install-vm-agent-offline/load-hive.png)

5.  Procure a pasta de \windows\system32\config\SYSTEM no disco do SO que anexou. Para o nome do ramo de registo, introduza **BROKENSYSTEM**. O novo ramo de registo é apresentado sob o **HKEY_LOCAL_MACHINE** chave.

6.  Procure a pasta de \windows\system32\config\SOFTWARE no disco do SO que anexou. Para o nome do software do hive, introduza **BROKENSOFTWARE**.

7.  Se o agente da VM não está a funcionar, cópia de segurança a configuração atual.

    >[!NOTE]
    >Se a VM não tem o agente instalado, avance para o passo 8. 
      
    1. Mudar o nome da pasta \windowsazure para \windowsazure.old.

    2. Exporte os seguintes registos:
        - HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet001\Services\WindowsAzureGuestAgent
        - HKEY_LOCAL_MACHINE\BROKENSYSTEM\\ControlSet001\Services\WindowsAzureTelemetryService
        - HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet001\Services\RdAgent

8.  Utilize os ficheiros existentes no troubleshooter VM como um repositório para a instalação do agente de VM. Conclua os seguintes passos:

    1. De troubleshooter VM, exporte as subchaves seguintes no formato de registo (. reg): 
        - HKEY_LOCAL_MACHINE  \SYSTEM\ControlSet001\Services\WindowsAzureGuestAgent
        - HKEY_LOCAL_MACHINE  \SYSTEM\ControlSet001\Services\WindowsAzureTelemetryService
        - HKEY_LOCAL_MACHINE  \SYSTEM\ControlSet001\Services\RdAgent

        ![Exportar as subchaves de registo](./media/install-vm-agent-offline/backup-reg.png)

    2. Edite os ficheiros de registo. Cada ficheiro, altere o valor de entrada **sistema** para **BROKENSYSTEM** (conforme ilustrado nas seguintes imagens) e guarde o ficheiro.

        ![Alterar os valores de subchave de registo](./media/install-vm-agent-offline/change-reg.png)

    3. Importe os ficheiros de registo para o repositório fazendo duplo clique em cada ficheiro de registo.

    4. Confirme se as três subchaves seguintes são importadas com êxito para o **BROKENSYSTEM** ramo de registo:
        - WindowsAzureGuestAgent
        - WindowsAzureTelemetryService
        - RdAgent

9.  Copie a pasta de agente da VM do C:\windowsazure\packages para o &lt;disco do SO que anexou&gt;: \windowsazure\packages.

    ![Copie os ficheiros do agente da VM para o disco do SO](./media/install-vm-agent-offline/copy-package.png)
      
    >[!NOTE]
    >Não copie o **registos** pasta. Depois de iniciado o serviço, são gerados novos registos.

10.  Selecione **BROKENSYSTEM**. No menu, selecione **ficheiro** > **descarregamento do Hive**.

11.  Selecione **BROKENSOFTWARE**. No menu, selecione **ficheiro** > **descarregamento do Hive**.

12.  Desligue o disco de SO e, em seguida, recrie a VM utilizando o disco do SO.

13.  Aceda à VM. Tenha em atenção que o RdAgent está em execução e os registos estão a ser gerados.

Se criou a VM utilizando o modelo de implementação clássica, terminar.


### <a name="use-the-provisionguestagent-property-for-vms-created-with-azure-resource-manager"></a>Utilize a propriedade de ProvisionGuestAgent para VMs criadas com o Azure Resource Manager

Se criou a VM utilizando o modelo de implementação Resource Manager, utilize o módulo Azure PowerShell para atualizar o **ProvisionGuestAgent** propriedade. A propriedade informa a Azure que a VM tem o agente da VM instalado.

Para definir o **ProvisionGuestAgent** propriedade, execute os seguintes comandos do Azure PowerShell:

   ```powershell
   $vm = Get-AzureVM –ServiceName <cloud service name> –Name <VM name>
   $vm.VM.ProvisionGuestAgent = $true
   Update-AzureVM –Name <VM name> –VM $vm.VM –ServiceName <cloud service name>
   ```

Em seguida, execute o `Get-AzureVM` comando. Tenha em atenção que o **GuestAgentStatus** propriedade agora é preenchida com dados:

   ```powershell
   Get-AzureVM –ServiceName <cloud service name> –Name <VM name>
   GuestAgentStatus:Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVMModel.GuestAgentStatus
   ```

## <a name="next-steps"></a>Passos Seguintes

- [Descrição geral do agente da Máquina Virtual do Azure](agent-user-guide.md)
- [Extensões de máquina virtual e funcionalidades para o Windows](extensions-features.md)
