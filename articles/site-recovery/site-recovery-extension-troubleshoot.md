---
title: 'Resolver problemas de falha de agente do Azure Site Recovery: Estado do agente convidado indisponível | Microsoft Docs'
description: Sintomas, causas e soluções de falhas do Azure Site Recovery relacionados com o agente e extensão
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.date: 05/02/2018
ms.author: asgang
ms.openlocfilehash: 9bfe181b2271f4e8af6f43e1728167712dade8ee
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
ms.locfileid: "33777604"
---
# <a name="troubleshoot-azure-site-recovery-extension-failures-issues-with-the-agent-or-extension"></a>Resolver problemas de falhas de extensão do Azure Site Recovery: problemas com o agente ou a extensão

Este artigo fornece os passos de resolução de problemas que podem ajudar a resolver erros de Azure Site Recovery relacionadas com a extensão e agente da VM.


## <a name="azure-site-recovery-extension-time-out"></a>Limite de tempo de extensão do Azure Site Recovery  

Mensagem de erro: "a execução da tarefa foi excedido durante o controlo para a operação de extensão ser iniciado"<br>
Código de erro: "151076"

 O Azure Site Recovery instalar uma extensão na máquina virtual como parte da tarefa de ativação da proteção. Qualquer uma das seguintes condições poderá impedir a proteção de que está a ser acionada e a tarefa falhar. Conclua os seguintes passos de resolução de problemas e, em seguida, repita a operação:

**Causa 1: [o agente está instalado na VM, mas do responder (para VMs do Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**    
**Causa 2: [o agente instalado na VM está desatualizado (para VMs com Linux)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**Causa 3: [a extensão de recuperação de Site não consegue atualizar ou carregar](#the-site-recovery-extension-fails-to-update-or-load)**  

Mensagem de erro: "operação de extensão de recuperação de site anterior está a demorar mais tempo do que o esperado."<br>
Código de erro: "150066"<br>

**Causa 1: [o agente está instalado na VM, mas do responder (para VMs do Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**    
**Causa 2: [o agente instalado na VM está desatualizado (para VMs com Linux)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**Causa 3: [o estado da extensão de recuperação de sites está incorreto](#the-site-recovery-extension-fails-to-update-or-load)**  

## <a name="protection-fails-because-the-vm-agent-is-unresponsive"></a>A proteção falha porque o agente VM está a responder

Mensagem de erro: "a execução da tarefa foi excedido durante o controlo para a operação de extensão ser iniciadas."<br>
Código de erro: "151099"<br>

Este erro pode ocorrer se o agente convidado do Azure na máquina virtual não está no estado pronto.
Pode verificar o estado do agente convidado do Azure no [portal do Azure](https://portal.azure.com/). Aceda à máquina virtual está a tentar proteger e verificar o estado "VM > Definições > propriedades > Estado do agente". Na maioria das vezes o estado do agente fique pronto depois de reiniciar a máquina virtual. No entanto, se o reinício não for uma opção de possível ou ainda estão a enfrentar o problema, em seguida, conclua os seguintes passos de resolução de problemas.

**Causa 1: [o agente está instalado na VM, mas do responder (para VMs do Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**    
**Causa 2: [o agente instalado na VM está desatualizado (para VMs com Linux)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  


Mensagem de erro: "a execução da tarefa foi excedido durante o controlo para a operação de extensão ser iniciadas."<br>
Código de erro: "151095"<br>

Esta situação ocorrer quando a versão do agente no computador Linux é antiga. Execute o seguinte passo de resolução de problemas.<br>
  **Causa 1: [o agente instalado na VM está desatualizado (para VMs com Linux)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
## <a name="causes-and-solutions"></a>Causas e soluções

### <a name="the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>O agente está instalado na VM, mas é responder (para VMs do Windows)

#### <a name="solution"></a>Solução
O agente VM poderá ter sido danificado ou o serviço poderá foram parado. Reinstalar o agente da VM ajuda a obter a versão mais recente. Também ajuda a reiniciar a comunicação com o serviço.

1. Determinar se o "Windows serviço de agente convidado do Azure" está em execução nos serviços de VM (services.msc). Tente reiniciar o "Windows serviço de agente convidado do Azure".    
2. Se o serviço de agente de convidados do Windows Azure não está visível nos serviços, no painel de controlo, aceda a **programas e funcionalidades** para determinar se o serviço de agente de convidados do Windows está instalado.
4. Se o agente de convidados do Windows Azure é apresentado no **programas e funcionalidades**, desinstale o agente convidado do Windows.
5. Transfira e instale o [versão mais recente do agente do MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Tem de ter direitos de administrador para concluir a instalação.
6. Certifique-se de que os serviços de agente de convidados do Windows Azure aparece nos serviços.
7. Reinicie a tarefa de proteção.

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
4. Ative a proteção da máquina virtual.



### <a name="the-site-recovery-extension-fails-to-update-or-load"></a>A extensão de recuperação de Site não consegue atualizar ou carregar
Se o estado das extensões é "vazio ', 'NotReady' ou transitar.

#### <a name="solution"></a>Solução

Desinstalar a extensão e reinicie a operação de novo.

Para desinstalar a extensão:

1. No [portal do Azure](https://portal.azure.com/), vá para a VM que estão a ocorrer falhas de cópia de segurança.
2. Selecione **definições**.
3. Selecione **extensões**.
4. Selecione **extensão de recuperação de sites**.
5. Selecione **desinstalar**.

Para a VM com Linux, se a extensão de VMSnapshot não for apresentada no portal do Azure, [atualizar o agente Linux do Azure](../virtual-machines/linux/update-agent.md), e, em seguida, execute a proteção. 

Concluir estes passos, faz com que a extensão de ser reinstalado durante a proteção.


