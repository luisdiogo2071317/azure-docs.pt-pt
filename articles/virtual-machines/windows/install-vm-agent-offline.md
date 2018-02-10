---
title: Instalar o agente da VM do Azure no modo Offline | Microsoft Docs
description: Saiba instalar o agente da VM do Azure no modo Offline.
services: virtual-machines-windows
documentationcenter: 
author: genlin
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: genli
ms.openlocfilehash: b38bfaffad3e214f3b854715e4d8030cde432bae
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
---
# <a name="install-the-vm-agent-in-offline-mode-in-an-azure-windows-vm"></a>Instalar o agente da VM no modo offline numa VM do Windows Azure 

Agente VM fornece funcionalidades úteis, como a reposição de palavra-passe de administrador local, push e de script. Este artigo mostra como instalar o agente da VM para uma VM que está offline. 

## <a name="when-to-use-offline-mode"></a>Quando utilizar o modo offline

Terá de instalar o agente da VM no modo offline no seguinte cenário:

- Implementar uma VM do Azure que não tem o agente da VM instalado ou o agente da VM não está a funcionar.
- Não se lembra a palavra-passe do administrador ou não é possível aceder a VM.

Neste cenário, terá de instalar o agente da VM no modo offline. 



## <a name="detailed-steps"></a>Passos detalhados

**Passo 1: Anexar o disco do SO da VM para outra VM, como um disco de dados**

1.  Elimine a VM. Certifique-se de que seleciona o **manter os discos** opção ao fazê-lo.

2.  Anexe o disco de SO como um disco de dados para outra VM (um troubleshooter VM). Para obter mais informações, veja o artigo [Como anexar um disco de dados a uma VM do Windows no portal do Azure](attach-managed-disk-portal.md).

3.  Ligue à VM da resolução de problemas. Abra **gestão de computadores** > **disco gestão**. Certifique-se de que o disco do SO está online e que as respetivas partições têm letras de unidade atribuídas.

**Passo 2: Modificar o disco de SO para instalar o agente da VM**

1.  Estabelecer uma ligação de ambiente de trabalho remota para a VM de resolução de problemas.

2.  O disco de SO anexou, navegue para **\windows\system32\config**. Copie todos os ficheiros de cópia de segurança no caso de uma reversão não é necessária.

3.  Inicie o Editor de registo (regedit.exe).

4.  Clique em de **HKEY_LOCAL_MACHINE** da chave e, em seguida, selecione **ficheiro** > **carga Hive** no menu.

    ![Ramo de registo de carga](./media/install-vm-agent-offline/load-hive.png)

5.  Navegue para **\windows\system32\config\SYSTEM** no sistema operativo de disco que é anexado e, em seguida, escreva BROKENSYSTEM como o nome do ramo de registo. Depois de fazê-lo, irá ver o registo do hive em **HKEY_LOCAL_MACHINE**.

6.  Navegue para **\windows\system32\config\SOFTWARE** no disco do SO ligado, escreva BROKENSOFTWARE como o nome para o ramo de registo.

7.  Se tiver uma versão atual do agente que não está a funcionar, efetue uma cópia de segurança da configuração atual. Se a VM não tem o agente da VM instalado, avance para o passo seguinte.  
      
    1. Mude o nome de pasta \windowsazure para \windowsazure.old

    2. Exportar registos seguintes
        - HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet001\Services\WindowsAzureGuestAgent
        - HKEY_LOCAL_MACHINE \BROKENSYSTEM\\ControlSet001\Services\WindowsAzureTelemetryService
        - HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet001\Services\RdAgent

8.  Utilize os ficheiros existentes na VM resolução de problemas como um repositório para a instalação do agente VM: 

    1. De troubleshooter VM, exporte as subchaves seguintes no formato de registo (. reg): 

        - HKEY_LOCAL_MACHINE  \SYSTEM\ControlSet001\Services\WindowsAzureGuestAgent
        - HKEY_LOCAL_MACHINE  \SYSTEM\ControlSet001\Services\WindowsAzureTelemetryService
        - HKEY_LOCAL_MACHINE  \SYSTEM\ControlSet001\Services\RdAgent

        ![A imagem sobre chaves de registo de exportação](./media/install-vm-agent-offline/backup-reg.png)

    2. Editar estes ficheiros de três registo, altere o **sistema** chave da entrada **BROKENSYSTEM**e, em seguida, guardar os ficheiros.
        ![A imagem sobre chaves de registo de alteração](./media/install-vm-agent-offline/change-reg.png)
    3. Fazendo duplo clique em ficheiros de registo para importá-los.
    4. Certifique-se de que as seguintes chaves são importadas para o ramo do BROKENSYSTEM com êxito: WindowsAzureGuestAgent, WindowsAzureTelemetryService e RdAgent.

9.  Copie a pasta de agente VM de C:\windowsazure\packages para o &lt;disco do SO que anexou&gt;: \windowsazure\packages.
    ![A imagem sobre ficheiros de cópia](./media/install-vm-agent-offline/copy-package.png)
      
    **Tenha em atenção** não copie a pasta de registos. Depois de iniciado o serviço, serão gerados novos registos.

10.  Clique em BROKENSYSTEM e selecione **ficheiro** > **descarregamento do Hive** no menu.

11.  Clique em BROKENSOFTWARE e selecione **ficheiro** > **descarregamento do Hive** no menu.

12.  Desligue o disco e, em seguida, recrie a VM utilizando o disco do SO.

13.  Se aceder a VM, agora verá RDAgent em execução e os registos de introdução criados.

14. Se se tratar de uma VM criada utilizando o modelo de implementação clássica, terminar. No entanto, se se tratar de uma VM criada utilizando o modelo de implementação Resource Manager, também tem de utilizar Azure PowerShell para atualizar a propriedade ProvisionGuestAgent para que esse Azure sabe que a VM tenha o agente instalado. Para tal, execute os seguintes comandos do Azure PowerShell:

        $vm = Get-AzureVM –ServiceName <cloud service name> –Name <VM name>
        $vm.VM.ProvisionGuestAgent = $true
        Update-AzureVM –Name <VM name> –VM $vm.VM –ServiceName <cloud service name>

    Se executar **Get-AzureVM** depois de seguir estes passos, o **GuestAgentStatus** propriedade deve ser preenchida em vez de uma página em branco que está a ser apresentada:

        Get-AzureVM –ServiceName <cloud service name> –Name <VM name>
        GuestAgentStatus:Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVMModel.GuestAgentStatus

## <a name="next-steps"></a>Passos Seguintes

- [Descrição geral do agente da Máquina Virtual do Azure](agent-user-guide.md)
- [Extensões de máquina virtual e funcionalidades para o Windows](extensions-features.md)