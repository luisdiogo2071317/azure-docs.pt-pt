---
title: 'Resolver problemas de falhas de cópia de segurança do Azure: Estado do agente convidado indisponível'
description: Os sintomas, causas e resoluções de falhas de cópia de segurança do Azure relacionados com o agente de extensão e discos.
services: backup
author: genlin
manager: cshepard
keywords: Cópia de segurança do Azure; Agente da VM; Conectividade de rede
ms.service: backup
ms.topic: troubleshooting
ms.date: 10/30/2018
ms.author: genli
ms.openlocfilehash: 5c37e2e3cabb81ed123146f283c7d568cc58816d
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50242633"
---
# <a name="troubleshoot-azure-backup-failure-issues-with-the-agent-or-extension"></a>Resolver problemas de falhas de cópia de segurança do Azure: problemas com o agente ou a extensão

Este artigo fornece passos de resolução de problemas que podem ajudar a resolverem erros de cópia de segurança do Azure relacionados à comunicação com o agente da VM e a extensão.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="vm-agent-unable-to-communicate-with-azure-backup"></a>Agente VM não é possível comunicar com o Azure Backup

Mensagem de erro: "Agente VM não é possível comunicar com o Backup do Azure"<br>
Código de erro: "UserErrorGuestAgentStatusUnavailable"

Depois de registar e agendar uma VM para o serviço de cópia de segurança, cópia de segurança inicia a tarefa através da comunicação com o agente da VM para tirar um instantâneo de ponto no tempo. Qualquer uma das seguintes condições poderá impedir que o instantâneo que está a ser acionado. Quando um instantâneo não é acionado, a cópia de segurança poderá falhar. Conclua os seguintes passos de resolução de problemas na ordem listada e, em seguida, repita a operação:

**Causa 1: [a VM não tem acesso à internet](#the-vm-has-no-internet-access)**  
**Causa 2: [o agente está instalado na VM, mas está sem resposta (para VMs do Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**    
**Causa 3: [o agente instalado na VM está desatualizado (para VMs do Linux)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**Causa 4: [não é possível obter o estado do instantâneo ou não pode ser criado um instantâneo](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**    
**Causa 5: [a extensão de cópia de segurança não consegue atualizar ou de carga](#the-backup-extension-fails-to-update-or-load)**  

## <a name="snapshot-operation-failed-due-to-no-network-connectivity-on-the-virtual-machine"></a>A operação de instantâneo falha porque a máquina virtual não está ligada à rede

Mensagem de erro: "A operação falhou devido a nenhuma conectividade de rede na máquina virtual do instantâneo"<br>
Código de erro: "ExtensionSnapshotFailedNoNetwork"

Depois de registar e agendar uma VM para o serviço de cópia de segurança do Azure, a cópia de segurança inicia a tarefa através da comunicação com a extensão de cópia de segurança de VM para tirar um instantâneo de ponto no tempo. Qualquer uma das seguintes condições poderá impedir que o instantâneo que está a ser acionado. Se o instantâneo não é acionado, pode ocorrer uma falha de cópia de segurança. Conclua os seguintes passos de resolução de problemas na ordem listada e, em seguida, repita a operação:    
**Causa 1: [a VM não tem acesso à internet](#the-vm-has-no-internet-access)**  
**Causa 2: [não é possível obter o estado do instantâneo ou não pode ser criado um instantâneo](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**Causa 3: [a extensão de cópia de segurança não consegue atualizar ou de carga](#the-backup-extension-fails-to-update-or-load)**  

## <a name="vmsnapshot-extension-operation-failed"></a>Operação de extensão de VMSnapshot falha

Mensagem de erro: "a operação de extensão de VMSnapshot falhou"<br>
Código de erro: "ExtentionOperationFailed"

Depois de registar e agendar uma VM para o serviço de cópia de segurança do Azure, a cópia de segurança inicia a tarefa através da comunicação com a extensão de cópia de segurança de VM para tirar um instantâneo de ponto no tempo. Qualquer uma das seguintes condições poderá impedir que o instantâneo que está a ser acionado. Se o instantâneo não é acionado, pode ocorrer uma falha de cópia de segurança. Conclua os seguintes passos de resolução de problemas na ordem listada e, em seguida, repita a operação:  
**Causa 1: [não é possível obter o estado do instantâneo ou não pode ser criado um instantâneo](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**Causa 2: [a extensão de cópia de segurança não consegue atualizar ou de carga](#the-backup-extension-fails-to-update-or-load)**  
**Causa 3: [o agente está instalado na VM, mas está sem resposta (para VMs do Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**Causa 4: [o agente instalado na VM está desatualizado (para VMs do Linux)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**

## <a name="backup-fails-because-the-vm-agent-is-unresponsive"></a>Cópia de segurança falha porque o agente da VM não está a responder

Mensagem de erro: "Não foi possível comunicar com o agente VM para o estado do instantâneo" <br>
Código de erro: "GuestAgentSnapshotTaskStatusError"

Depois de registar e agendar uma VM para o serviço de cópia de segurança do Azure, a cópia de segurança inicia a tarefa através da comunicação com a extensão de cópia de segurança de VM para tirar um instantâneo de ponto no tempo. Qualquer uma das seguintes condições poderá impedir que o instantâneo que está a ser acionado. Se o instantâneo não é acionado, pode ocorrer uma falha de cópia de segurança. Conclua os seguintes passos de resolução de problemas na ordem listada e, em seguida, repita a operação:  
**Causa 1: [o agente está instalado na VM, mas está sem resposta (para VMs do Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**Causa 2: [o agente instalado na VM está desatualizado (para VMs do Linux)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**Causa 3: [a VM não tem acesso à internet](#the-vm-has-no-internet-access)**  

## <a name="backup-fails-with-an-internal-error"></a>Falha de cópia de segurança, com um erro interno

Mensagem de erro: "cópia de segurança falhou com um erro interno - repita a operação dentro de alguns minutos" <br>
Código de erro: "BackUpOperationFailed" / "BackUpOperationFailedV2"

Depois de registar e agendar uma VM para o serviço de cópia de segurança do Azure, a cópia de segurança inicia a tarefa através da comunicação com a extensão de cópia de segurança de VM para tirar um instantâneo de ponto no tempo. Qualquer uma das seguintes condições poderá impedir que o instantâneo que está a ser acionado. Se o instantâneo não é acionado, pode ocorrer uma falha de cópia de segurança. Conclua os seguintes passos de resolução de problemas na ordem listada e, em seguida, repita a operação:  
**Causa 1: [a VM não tem acesso à internet](#the-vm-has-no-internet-access)**  
**Causa 2: [o agente instalado na VM, mas não está a responder (para VMs do Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**Causa 3: [o agente instalado na VM está desatualizado (para VMs do Linux)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**Causa 4: [não é possível obter o estado do instantâneo ou não pode ser criado um instantâneo](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**Causa 5: [a extensão de cópia de segurança não consegue atualizar ou de carga](#the-backup-extension-fails-to-update-or-load)**  
**Causa 6: [serviço de cópia de segurança não tem permissão para eliminar os pontos de restauração antigos devido a um bloqueio de grupo de recursos](#backup-service-does-not-have-permission-to-delete-the-old-restore-points-due-to-resource-group-lock)**

## <a name="causes-and-solutions"></a>Causas e soluções

### <a name="the-vm-has-no-internet-access"></a>A VM não tem acesso à internet
Pelo requisito de implementação, a VM não tem acesso à internet. Em alternativa, poderá ter restrições que impedem o acesso para a infraestrutura do Azure.

Para funcionar corretamente, a extensão de cópia de segurança precisa de conectividade para os endereços IP públicos do Azure. A extensão envia comandos para um ponto final de armazenamento do Azure (HTTPs URL) de gerir os instantâneos da VM. Se a extensão não tiver acesso à internet pública, cópia de segurança, eventualmente, falha.

É possível implementar um servidor proxy para encaminhar o tráfego VM.
##### <a name="create-a-path-for-https-traffic"></a>Criar um caminho para o tráfego de HTTPs

1. Se tiver restrições de rede no local (por exemplo, um grupo de segurança de rede), implementa um servidor de proxy de HTTPs para encaminhar o tráfego.
2. Para permitir o acesso à internet do servidor de proxy de HTTPs, adicione regras para o grupo de segurança de rede, se tiver uma.

Para saber como configurar um proxy de HTTPs para cópias de segurança VM, veja [preparar o ambiente para fazer cópias de segurança de máquinas virtuais do Azure](backup-azure-arm-vms-prepare.md#establish-network-connectivity).

A cópia de segurança de VM ou o servidor de proxy por meio do qual o tráfego é encaminhado requer acesso a endereços IP públicos do Azure

####  <a name="solution"></a>Solução
Para resolver o problema, experimente um dos seguintes métodos:

##### <a name="allow-access-to-azure-storage-that-corresponds-to-the-region"></a>Permitir o acesso ao armazenamento do Azure, que corresponde à região

Pode usar [etiquetas de serviço](../virtual-network/security-overview.md#service-tags) para permitir ligações para o armazenamento da região específica. Certifique-se de que a regra que permite o acesso à conta de armazenamento tem uma prioridade mais alta do que a regra que bloqueia o acesso de internet. 

![Grupo de segurança de rede com marcas de armazenamento para uma região](./media/backup-azure-arm-vms-prepare/storage-tags-with-nsg.png)

Para compreender o procedimento passo a passo para configurar as etiquetas de serviço, veja [este vídeo](https://youtu.be/1EjLQtbKm1M).

> [!WARNING]
> Etiquetas de serviço de armazenamento estão em pré-visualização. Eles estão disponíveis apenas nas regiões específicas. Para obter uma lista de regiões, consulte [etiquetas para o armazenamento de serviço](../virtual-network/security-overview.md#service-tags).

Se utilizar Managed Disks do Azure, poderá ter de abertura de portas adicionais (porta 8443) nas firewalls.

Além disso, se a sub-rede não tiver uma rota para tráfego de saída da internet, terá de adicionar um ponto final de serviço com a etiqueta de serviço "Microsoft. Storage" à sua sub-rede. 

### <a name="the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>O agente está instalado na VM, mas ele não está a responder (para VMs do Windows)

#### <a name="solution"></a>Solução
O agente da VM pode ter sido corrompido ou o serviço pode ter sido parado. Reinstalar o agente da VM ajuda a obter a versão mais recente. Ele também ajuda a reiniciar a comunicação com o serviço.

1. Determine se o serviço de agente de convidado do Windows está em execução nos serviços de VM (Services. msc). Tente reiniciar o serviço de agente de convidado do Windows e iniciar a cópia de segurança.    
2. Se o serviço de agente de convidado do Windows não está visível nos serviços, no painel de controlo, aceda a **programas e funcionalidades** para determinar se o serviço de agente de convidado do Windows está instalado.
4. Se o agente convidado do Windows é apresentada no **programas e funcionalidades**, desinstale o agente convidado do Windows.
5. Transferir e instalar o [mais recente versão do agente do MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Tem de ter direitos de administrador para concluir a instalação.
6. Certifique-se de que os serviços do agente de convidado do Windows são apresentadas nos serviços.
7. Execute uma cópia de segurança a pedido: 
    * No portal, selecione **cópia de segurança agora**.

Além disso, certifique-se de que [Microsoft .NET 4.5 está instalado](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) na VM. .NET 4.5 é necessário para o agente da VM comunicar com o serviço.

### <a name="the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>O agente instalado na VM está desatualizado (para VMs do Linux)

#### <a name="solution"></a>Solução
Mais relacionados com o agente ou relacionados com a extensão de falhas para VMs do Linux são causadas por problemas que afetam um agente VM desatualizado. Para resolver este problema, siga estas Diretrizes gerais:

1. Siga as instruções para [atualizar o agente da VM do Linux](../virtual-machines/linux/update-agent.md).

 > [!NOTE]
 > Estamos *altamente recomendável* que Atualize o agente apenas por meio de um repositório de distribuição. Não é recomendável baixar o código do agente diretamente a partir do GitHub e a atualizá-los. Se o agente mais recente para a sua distribuição não é suportada de distribuição disponíveis, contacte para obter instruções sobre como instalá-lo. Para verificar se o agente mais recente, vá para o [agente do Windows Azure Linux](https://github.com/Azure/WALinuxAgent/releases) página no repositório do GitHub.

2. Certifique-se de que o agente do Azure está em execução a VM ao executar o seguinte comando: `ps -e`

 Se o processo não está em execução, reinicie-o usando os seguintes comandos:

 * Para o Ubuntu: `service walinuxagent start`
 * Para outras distribuições: `service waagent start`

3. [Configurar o agente de reinício automático](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).
4. Execute uma nova cópia de segurança de teste. Se a falha persistir, recolha os seguintes registos da VM:

   * /var/lib/waagent/*.xml
   * /var/log/waagent.log
   * / var/iniciar/azure / *

Se é necessário o registo verboso para waagent, siga estes passos:

1. No ficheiro /etc/waagent.Conf., localize a seguinte linha: **ativar o registo verboso (y | n)**
2. Alteração da **Logs.Verbose** partir da *n* para *y*.
3. Guardar a alteração e, em seguida, reinicie waagent, concluindo os passos descritos anteriormente nesta secção.

###  <a name="the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>Não é possível obter o estado do instantâneo ou não pode ser criado um instantâneo
A cópia de segurança VM depende de emitir um comando de instantâneo para a conta de armazenamento subjacente. Cópia de segurança pode falhar porque ele não tem acesso à conta de armazenamento ou porque a execução da tarefa de instantâneo é atrasada.

#### <a name="solution"></a>Solução
As seguintes condições poderão provocar a falha na tarefa de instantâneo:

| Causa | Solução |
| --- | --- |
| O estado da VM é informado incorretamente, uma vez que a VM é encerrada no protocolo RDP (Remote Desktop). | Se encerrar a VM na RDP, consulte o portal para determinar se o estado da VM está correto. Se não estiver correto, encerre a VM no portal, utilizando o **encerramento** opção no dashboard de VM. |
| A VM não é possível obter o endereço de anfitrião ou de recursos de infraestrutura do DHCP. | DHCP tem de estar ativado no computador convidado para a cópia de segurança de VM de IaaS para trabalhar. Se a VM não é possível obter o endereço de anfitrião ou de recursos de infraestrutura da resposta DHCP 245, não é possível transferir ou executar quaisquer extensões. Se precisar de um IP privado estático, configurá-lo através da plataforma. A opção de DHCP no interior da VM deve ser ativada à esquerda. Para obter mais informações, consulte [definir um IP estático de privada interno](../virtual-network/virtual-networks-reserved-private-ip.md). |

### <a name="the-backup-extension-fails-to-update-or-load"></a>A extensão de cópia de segurança não consegue atualizar ou de carga
Se não é possível carregar extensões, a cópia de segurança falha porque não pode ser criado um instantâneo.

#### <a name="solution"></a>Solução

Desinstale a extensão para forçar a extensão de VMSnapshot para recarregar. Da próxima tentativa de cópia de segurança recarrega a extensão.

Para desinstalar a extensão:

1. Na [portal do Azure](https://portal.azure.com/), vá para a VM que está com falhas de cópia de segurança.
2. Selecione **definições**.
3. Selecione **Extensions** (Extensões).
4. Selecione **extensão de Vmsnapshot**.
5. Selecione **desinstalar**.

Para VM do Linux, se a extensão do VMSnapshot não mostra no portal do Azure, [atualizar o agente do Linux do Azure](../virtual-machines/linux/update-agent.md), e, em seguida, execute a cópia de segurança. 

Concluir estes passos, faz com que a extensão de ser reinstalados durante a próxima cópia de segurança.

### <a name="backup-service-does-not-have-permission-to-delete-the-old-restore-points-due-to-resource-group-lock"></a>O serviço de cópia de segurança não tem permissão para eliminar os pontos de restauração antigos devido a um bloqueio de grupo de recursos
Este problema é específico para VMs gerenciadas em que o usuário bloqueia o grupo de recursos. Neste caso, o serviço de cópia de segurança não é possível eliminar os pontos de restauro mais antigos. Como há um limite de 18 pontos de restauro, novas cópias de segurança começam a falhar.

#### <a name="solution"></a>Solução

Para resolver o problema, remova o bloqueio do grupo de recursos e conclua os seguintes passos para remover a coleção de ponto de restauro: 
 
1. Remova o bloqueio no grupo de recursos em que a VM se encontra. 
2. Instale ARMClient com o Chocolatey: <br>
   https://github.com/projectkudu/ARMClient
3. Inicie sessão no ARMClient: <br>
    `.\armclient.exe login`
4. Obter a coleção de ponto de restauro que corresponde à VM: <br>
    `.\armclient.exe get https://management.azure.com/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Compute/restorepointcollections/AzureBackup_<VM-Name>?api-version=2017-03-30`

    Exemplo: `.\armclient.exe get https://management.azure.com/subscriptions/f2edfd5d-5496-4683-b94f-b3588c579006/resourceGroups/winvaultrg/providers/Microsoft.Compute/restorepointcollections/AzureBackup_winmanagedvm?api-version=2017-03-30`
5. Elimine a coleção de ponto de restauro: <br>
    `.\armclient.exe delete https://management.azure.com/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Compute/restorepointcollections/AzureBackup_<VM-Name>?api-version=2017-03-30` 
6. O próximo backup agendado cria automaticamente uma coleção de ponto de restauro e novos pontos de restauro.

Quando tiver terminado, pode novamente colocar novamente o bloqueio no grupo de recursos VM. 
