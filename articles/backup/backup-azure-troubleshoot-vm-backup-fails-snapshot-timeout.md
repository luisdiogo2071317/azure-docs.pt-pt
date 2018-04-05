---
title: 'Resolver problemas de falhas de cópia de segurança do Azure: Estado do agente convidado indisponível | Microsoft Docs'
description: Sintomas, causas e soluções de falhas de cópia de segurança do Azure relacionados com o agente, a extensão e discos.
services: backup
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
keywords: Cópia de segurança do Azure; Agente da VM; Conectividade de rede
ms.assetid: 4b02ffa4-c48e-45f6-8363-73d536be4639
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 01/09/2018
ms.author: genli;markgal;sogup;
ms.openlocfilehash: 81678f6a8659ffb763ebfe418098e510c73f6ae0
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="troubleshoot-azure-backup-failure-issues-with-the-agent-or-extension"></a>Resolver problemas de falhas de cópia de segurança do Azure: problemas com o agente ou a extensão

Este artigo fornece os passos de resolução de problemas que podem ajudar a resolver erros de cópia de segurança do Azure relacionados com a comunicação com o agente da VM e a extensão.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="vm-agent-unable-to-communicate-with-azure-backup"></a>Agente VM não é possível comunicar com o Backup do Azure

Mensagem de erro: "Agente da VM não é possível comunicar com o Backup do Azure"

Depois de registar e agendar uma VM para o serviço de cópia de segurança, a cópia de segurança inicia a tarefa ao comunicar com o agente VM para criar um instantâneo de ponto no tempo. Qualquer uma das seguintes condições poderá impedir que o instantâneo a ser acionado. Quando um instantâneo não está activado, a cópia de segurança poderá falhar. Conclua os seguintes passos de resolução de problemas, pela ordem listada a e, em seguida, repita a operação:

**Causa 1: [a VM não tem acesso à internet](#the-vm-has-no-internet-access)**  
**Causa 2: [o agente está instalado na VM, mas do responder (para VMs do Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**    
**Causa 3: [o agente instalado na VM está desatualizado (para VMs com Linux)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**Causa 4: [não é possível obter o estado de instantâneo ou não pode ser obtido um instantâneo](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**    
**Causa 5: [a extensão de cópia de segurança não consegue atualizar ou carregar](#the-backup-extension-fails-to-update-or-load)**  

## <a name="snapshot-operation-failed-due-to-no-network-connectivity-on-the-virtual-machine"></a>Falha na operação de instantâneo porque a máquina virtual não está ligada à rede

Mensagem de erro: "A operação falhou devido a sem conectividade de rede na máquina virtual de instantâneos"

Depois de registar e agendar uma VM para o serviço de cópia de segurança do Azure, a cópia de segurança inicia a tarefa ao comunicar com a extensão de cópia de segurança de VM para criar um instantâneo de ponto no tempo. Qualquer uma das seguintes condições poderá impedir que o instantâneo a ser acionado. Se o instantâneo não é acionado, poderão ocorrer uma falha de cópia de segurança. Conclua os seguintes passos de resolução de problemas, pela ordem listada a e, em seguida, repita a operação:    
**Causa 1: [a VM não tem acesso à internet](#the-vm-has-no-internet-access)**  
**Causa 2: [não é possível obter o estado de instantâneo ou não pode ser obtido um instantâneo](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**Causa 3: [a extensão de cópia de segurança não consegue atualizar ou carregar](#the-backup-extension-fails-to-update-or-load)**  

## <a name="vmsnapshot-extension-operation-failed"></a>Falha da operação de extensão VMSnapshot

Mensagem de erro: "a operação de extensão de VMSnapshot falhou"

Depois de registar e agendar uma VM para o serviço de cópia de segurança do Azure, a cópia de segurança inicia a tarefa ao comunicar com a extensão de cópia de segurança de VM para criar um instantâneo de ponto no tempo. Qualquer uma das seguintes condições poderá impedir que o instantâneo a ser acionado. Se o instantâneo não é acionado, poderão ocorrer uma falha de cópia de segurança. Conclua os seguintes passos de resolução de problemas, pela ordem listada a e, em seguida, repita a operação:  
**Causa 1: [não é possível obter o estado de instantâneo ou não pode ser obtido um instantâneo](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**Causa 2: [a extensão de cópia de segurança não consegue atualizar ou carregar](#the-backup-extension-fails-to-update-or-load)**  
**Causa 3: [o agente está instalado na VM, mas do responder (para VMs do Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**Causa 4: [o agente instalado na VM está desatualizado (para VMs com Linux)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**

## <a name="backup-fails-because-the-vm-agent-is-unresponsive"></a>Cópia de segurança falha porque o agente VM está a responder

Messagae de erro: "Não é possível efetuar a operação dado que o agente da VM não está a responder"

Depois de registar e agendar uma VM para o serviço de cópia de segurança do Azure, a cópia de segurança inicia a tarefa ao comunicar com a extensão de cópia de segurança de VM para criar um instantâneo de ponto no tempo. Qualquer uma das seguintes condições poderá impedir que o instantâneo a ser acionado. Se o instantâneo não é acionado, poderão ocorrer uma falha de cópia de segurança. Conclua os seguintes passos de resolução de problemas, pela ordem listada a e, em seguida, repita a operação:  
**Causa 1: [o agente está instalado na VM, mas do responder (para VMs do Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**Causa 2: [o agente instalado na VM está desatualizado (para VMs com Linux)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**Causa 3: [a VM não tem acesso à internet](#the-vm-has-no-internet-access)**  

## <a name="backup-fails-with-an-internal-error"></a>Falha de cópia de segurança, com um erro interno

Mensagem de erro: "cópia de segurança falhou com um erro interno -. Repita a operação dentro de alguns minutos"

Depois de registar e agendar uma VM para o serviço de cópia de segurança do Azure, a cópia de segurança inicia a tarefa ao comunicar com a extensão de cópia de segurança de VM para criar um instantâneo de ponto no tempo. Qualquer uma das seguintes condições poderá impedir que o instantâneo a ser acionado. Se o instantâneo não é acionado, poderão ocorrer uma falha de cópia de segurança. Conclua os seguintes passos de resolução de problemas, pela ordem listada a e, em seguida, repita a operação:  
**Causa 1: [a VM não tem acesso à internet](#the-vm-has-no-internet-access)**  
**Causa 2: [o agente instalado na VM, mas não está a responder (para VMs do Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**Causa 3: [o agente instalado na VM está desatualizado (para VMs com Linux)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**Causa 4: [não é possível obter o estado de instantâneo ou não pode ser obtido um instantâneo](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**Causa 5: [a extensão de cópia de segurança não consegue atualizar ou carregar](#the-backup-extension-fails-to-update-or-load)**  
**Causa 6: [serviço de cópia de segurança não tem permissão para eliminar os pontos de restauro antigos devido a um bloqueio de grupo de recursos](#backup-service-does-not-have-permission-to-delete-the-old-restore-points-due-to-resource-group-lock)**

## <a name="causes-and-solutions"></a>Causas e soluções

### <a name="the-vm-has-no-internet-access"></a>A VM não tem acesso à internet
Pelo requisito de implementação, a VM não tem acesso à internet. Em alternativa, poderá ter restrições que impedem o acesso à infraestrutura do Azure.

Para funcionar corretamente, a extensão de cópia de segurança necessita de conectividade para os endereços IP públicos do Azure. A extensão envia comandos para um ponto final de armazenamento do Azure (HTTP URL) de gerir os instantâneos da VM. Se a extensão não tiver acesso à internet pública, cópia de segurança eventualmente falha.

####  <a name="solution"></a>Solução
Para resolver o problema, experimente um dos seguintes métodos:

##### <a name="allow-access-to-azure-storage-that-corresponds-to-the-region"></a>Permitir o acesso ao armazenamento do Azure que corresponde à região

Pode utilizar [etiquetas de serviço](../virtual-network/security-overview.md#service-tags) para permitir ligações ao armazenamento da região específica. Certifique-se de que a regra que permite o acesso à conta de armazenamento tem prioridade mais alta do que a regra que bloqueia o acesso à internet. 

![Grupo de segurança de rede com etiquetas de armazenamento para uma região](./media/backup-azure-arm-vms-prepare/storage-tags-with-nsg.png)

> [!WARNING]
> As etiquetas de serviço de armazenamento estão na pré-visualização. Estão disponíveis apenas em regiões específicas. Para obter uma lista de regiões, consulte [etiquetas para armazenamento de serviço](../virtual-network/security-overview.md#service-tags).

##### <a name="create-a-path-for-http-traffic"></a>Criar um caminho para o tráfego HTTP

1. Se tiver de restrições de rede no local (por exemplo, um grupo de segurança de rede), implemente um servidor de proxy HTTP para encaminhar o tráfego.
2. Para permitir o acesso à internet do servidor de proxy de HTTP, adicione regras para o grupo de segurança de rede, se tiver uma.

Para saber como configurar um proxy HTTP para cópias de segurança VM, consulte [preparar o ambiente para fazer cópias de segurança de máquinas virtuais do Azure](backup-azure-arm-vms-prepare.md#establish-network-connectivity).

Se utilizar discos gerida do Azure, poderá ser necessário uma abertura de portas adicionais (porta 8443) nas firewalls.

### <a name="the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>O agente está instalado na VM, mas é responder (para VMs do Windows)

#### <a name="solution"></a>Solução
O agente VM poderá ter sido danificado ou o serviço poderá foram parado. Reinstalar o agente da VM ajuda a obter a versão mais recente. Também ajuda a reiniciar a comunicação com o serviço.

1. Determine se o agente de convidados do Windows está em execução nos serviços de VM (services.msc). Tente reiniciar o serviço de agente de convidados do Windows e iniciar a cópia de segurança.    
2. Se o serviço de agente de convidados do Windows não está visível nos serviços, no painel de controlo, aceda a **programas e funcionalidades** para determinar se o serviço de agente de convidados do Windows está instalado.
4. Se o agente convidado do Windows é apresentado no **programas e funcionalidades**, desinstale o agente convidado do Windows.
5. Transfira e instale o [versão mais recente do agente do MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Tem de ter direitos de administrador para concluir a instalação.
6. Certifique-se de que os serviços de agente do Windows convidado aparece nos serviços.
7. Execute uma cópia de segurança a pedido: 
    * No portal, selecione **cópia de segurança agora**.

Além disso, certifique-se de que [está instalado o Microsoft .NET 4.5](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) na VM. O .NET 4.5 é necessário para o agente da VM comunicar com o serviço.

### <a name="the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>O agente instalado na VM está desatualizado (para VMs com Linux)

#### <a name="solution"></a>Solução
Mais relacionados com o agente relacionadas com a extensão de falhas de ou para VMs com Linux são causadas por problemas que afetam um agente VM desatualizado. Para resolver este problema, siga estas Diretrizes gerais:

1. Siga as instruções para [atualizar o agente da VM com Linux](../virtual-machines/linux/update-agent.md).

 > [!NOTE]
 > Iremos *vivamente recomendável* que Atualize o agente apenas através de um repositório de distribuição. Não é recomendada a transferir o código de agente diretamente a partir do GitHub e atualizá-la. Se o agente mais recente para a distribuição não suporte de distribuição disponíveis, contacte para obter instruções sobre como o instalar. Para verificar se o agente mais recente, vá para o [agente Linux do Windows Azure](https://github.com/Azure/WALinuxAgent/releases) página no repositório GitHub.

2. Certifique-se de que o agente do Azure está em execução a VM, executando o seguinte comando: `ps -e`

 Se o processo não está em execução, reinicie-o utilizando os seguintes comandos:

 * Para Ubuntu: `service walinuxagent start`
 * Para outras distribuições: `service waagent start`

3. [Configurar o agente de reinício automático](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).
4. Execute uma nova cópia de segurança de teste. Se a falha persistir, recolha os seguintes registos de VM:

   * /var/lib/waagent/*.xml
   * /var/log/waagent.log
   * /var/log/azure/*

Se é necessário o registo verboso para waagent, siga estes passos:

1. No ficheiro /etc/waagent.conf, localize a seguinte linha: **ativar registo verboso (y | n)**
2. Alterar o **Logs.Verbose** valor a partir da *n* para *y*.
3. Guardar a alteração e, em seguida, reinicie waagent, efetuando os passos descritos anteriormente nesta secção.

###  <a name="the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>Não é possível obter o estado de instantâneo ou não pode ser obtido um instantâneo
A cópia de segurança VM depende de emitir um comando de instantâneo para a conta de armazenamento subjacente. Pode efetuar uma cópia de segurança porque este não tem acesso à conta de armazenamento ou porque a execução da tarefa de instantâneo está atrasada.

#### <a name="solution"></a>Solução
As seguintes condições poderão fazer com que a tarefa de instantâneo falhar:

| Causa | Solução |
| --- | --- |
| O estado VM é comunicado incorretamente porque a VM seja encerrada no protocolo de ambiente de trabalho remoto (RDP). | Se desligar a VM com RDP, consulte o portal para determinar se o estado da VM está correto. Se não estiver correto, encerre a VM no portal utilizando o **encerramento** opção no dashboard VM. |
| A VM não é possível obter o endereço do anfitrião ou de recursos de infraestrutura do DHCP. | DHCP deve ser ativado no computador convidado para a cópia de segurança de VM do IaaS funcione. Se a VM não é possível obter o endereço do anfitrião ou de recursos de infraestrutura da resposta DHCP 245, não é possível transferir ou executar quaisquer extensões. Se precisar de um IP privado estático, configurá-lo através da plataforma. A opção de DHCP dentro da VM deve ser ativada à esquerda. Para obter mais informações, consulte [definir um IP estático de privada interno](../virtual-network/virtual-networks-reserved-private-ip.md). |

### <a name="the-backup-extension-fails-to-update-or-load"></a>A extensão de cópia de segurança não consegue atualizar ou carregar
Se não é possível carregar as extensões, a cópia de segurança falha porque não pode ser obtido um instantâneo.

#### <a name="solution"></a>Solução

Desinstale a extensão para forçar a extensão de VMSnapshot para recarregar. A tentativa de cópia de segurança seguinte reloads a extensão.

Para desinstalar a extensão:

1. No [portal do Azure](https://portal.azure.com/), vá para a VM que estão a ocorrer falhas de cópia de segurança.
2. Selecione **definições**.
3. Selecione **extensões**.
4. Selecione **Vmsnapshot extensão**.
5. Selecione **desinstalar**.

Concluir estes passos, faz com que a extensão de ser reinstalado durante a próxima cópia de segurança.

### <a name="backup-service-does-not-have-permission-to-delete-the-old-restore-points-due-to-resource-group-lock"></a>O serviço de cópia de segurança não tem permissão para eliminar os pontos de restauro antigos devido a um bloqueio de grupo de recursos
Este problema é específico para VMs gerido em que o utilizador bloqueia o grupo de recursos. Neste caso, o serviço de cópia de segurança não consegue eliminar pontos de restauro mais antigos. Porque não existe um limite de 18 pontos de restauro, novas cópias de segurança começarem a falhar.

#### <a name="solution"></a>Solução

Para resolver o problema, execute os seguintes passos para remover a recolha de ponto de restauro: <br>
 
1. Remova o bloqueio no grupo de recursos em que a VM se encontra. 
2. Instale ARMClient utilizando Chocolatey: <br>
   https://github.com/projectkudu/ARMClient
3. Inicie sessão no ARMClient: <br>
    `.\armclient.exe login`
4. Obter a coleção de ponto de restauro que corresponde à VM: <br>
    `.\armclient.exe get https://management.azure.com/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Compute/restorepointcollections/AzureBackup_<VM-Name>?api-version=2017-03-30`

    Exemplo: `.\armclient.exe get https://management.azure.com/subscriptions/f2edfd5d-5496-4683-b94f-b3588c579006/resourceGroups/winvaultrg/providers/Microsoft.Compute/restorepointcollections/AzureBackup_winmanagedvm?api-version=2017-03-30`
5. Elimine a coleção de ponto de restauro: <br>
    `.\armclient.exe delete https://management.azure.com/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Compute/restorepointcollections/AzureBackup_<VM-Name>?api-version=2017-03-30` 
6. A cópia de segurança agendada seguinte cria automaticamente uma coleção de ponto de restauro e novos pontos de restauro.

 
O problema será acionador se bloquear o grupo de recursos novamente. 

