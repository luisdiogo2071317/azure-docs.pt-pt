---
title: Consola de série de máquina virtual do Azure | Microsoft Docs
description: Consola de série bidirecional para máquinas virtuais do Azure.
services: virtual-machines-linux
documentationcenter: ''
author: harijay
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/05/2018
ms.author: harijay
ms.openlocfilehash: b7d6e48a6f34472bc38947fd70e850b1c3bf6f8a
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
---
# <a name="virtual-machine-serial-console-preview"></a>Consola de série de máquina virtual (pré-visualização) 


A consola de série de máquina virtual no Azure fornece acesso a uma consola baseada em texto para as máquinas virtuais de Linux e Windows. Esta ligação série é COM1 porta série da máquina virtual e fornece acesso à máquina virtual e não estão relacionadas com a rede da máquina virtual / estado do sistema de operativo. Acesso à consola de série para uma máquina virtual pode ser feito atualmente apenas através do portal do Azure e permitido apenas para os utilizadores que tenham contribuinte de VM ou acima acesso à máquina virtual. 

> [!Note] 
> Pré-visualizações ficam disponíveis para si condition que aceita os termos de utilização. Para obter mais informações, consulte [Microsoft suplementares termos de utilização do Azure para a pré-visualizações do Microsoft Azure.] (https://azure.microsoft.com/support/legal/preview-supplemental-terms/) Este serviço está atualmente no **pré-visualização pública** e acesso à consola de série para máquinas virtuais está disponível para global regiões do Azure. Neste momento a consola de série não está disponível na nuvem do Azure Government, Datacenters do Azure e Azure China.


## <a name="prerequisites"></a>Pré-requisitos 

* Máquina virtual tem de ter [diagnóstico de arranque](boot-diagnostics.md) ativada 
* A conta utilizando a consola de série tem de ter [função de contribuinte](../../active-directory/role-based-access-built-in-roles.md) para a VM e o [diagnóstico de arranque](boot-diagnostics.md) conta de armazenamento. 
* Para as definições específicas do Linux distro, consulte [aceder à consola de série para Linux](#accessing-serial-console-for-linux)


## <a name="open-the-serial-console"></a>Abra a consola de série
consola de série para máquinas virtuais só é acessível através de [portal do Azure](https://portal.azure.com). Seguem-se os passos para aceder à consola de série para máquinas virtuais através do portal 

  1. Abra o portal do Azure
  2. No menu à esquerda, selecione as máquinas virtuais.
  3. Clique na VM na lista. Abre a página de descrição geral para a VM.
  4. Desloque para baixo para o suporte + secção de resolução de problemas e clique na opção de consola de série (pré-visualização). Um novo painel com a consola de série irá abrir e iniciar a ligação.

![](../media/virtual-machines-serial-console/virtual-machine-linux-serial-console-connect.gif)


> [!NOTE] 
> Consola de série necessita de um utilizador local com uma palavra-passe configurada. Neste momento, as VMs só configuradas com a chave pública SSH não terá acesso à consola de série. Para criar um utilizador local com palavra-passe, siga [extensão de acesso de VM](https://docs.microsoft.com/azure/virtual-machines/linux/using-vmaccess-extension) e criar utilizador local com a palavra-passe.

### <a name="disable-feature"></a>Desativar a funcionalidade
A funcionalidade de consola de série, pode ser desativada para específicas VMs, desativando a definição de diagnóstico de arranque essa VM.

## <a name="serial-console-security"></a>Segurança da consola de série 

### <a name="access-security"></a>Segurança de acesso 
Acesso à consola de série está limitado a utilizadores que tenham [VM contribuintes](../../active-directory/role-based-access-built-in-roles.md#virtual-machine-contributor) ou posterior acesso para a máquina virtual. Se o inquilino do AAD requer multi-factor Authentication, em seguida, acesso à consola de série também terá de MFA como o acesso é através de [portal do Azure](https://portal.azure.com).

### <a name="channel-security"></a>Segurança de canal
Todos os dados são enviados de volta e estabelecido são encriptados em risco.

### <a name="audit-logs"></a>Registos de auditoria
Todo o acesso à consola de série tem sessão iniciado [diagnóstico de arranque](https://docs.microsoft.com/azure/virtual-machines/linux/boot-diagnostics) registos da máquina virtual. Acesso a estes registos são propriedade e é controlado pelo administrador de máquina virtual do Azure.  

>[!CAUTION] 
Enquanto nenhuma palavra-passe de acesso para a consola sessão iniciada, se executar a consola de comandos contém ou palavras-passe, segredos, os nomes de utilizador ou qualquer outra forma de informação identificativa (PII) de saída, às serão escritos para o diagnóstico de arranque de máquina virtual os registos, juntamente com todos os outro texto visível, como parte da implementação da funcionalidade de scrollback da consola de série. Estes registos são circulares e apenas indivíduos com permissões de leitura para a conta de armazenamento do diagnostics têm acesso aos mesmos, no entanto, recomendamos a seguir a melhor prática utilizando a consola SSH para tudo o que poderá envolver o método segredos e/ou PII. 

### <a name="concurrent-usage"></a>Utilização em simultâneo
Se um utilizador está ligado à consola de série e outro utilizador com êxito os pedidos de acesso a essa mesma máquina virtual, o primeiro utilizador será desligado e o segundo utilizador ligados de forma akin to o primeiro utilizador colocado cópias de segurança e mantendo a consola física e um novo utilizador junto para baixo.

>[!CAUTION] 
Isto significa que o utilizador for desligado não será registado! A capacidade de impor um fim de sessão após desligar (através de SIGHUP ou mecanismo semelhante) ainda está a ser plano. Para o Windows não há um limite de tempo automática ativada no SAC, no entanto para Linux pode configurar definição de tempo limite de terminal. Para fazer isto adicione simplesmente `export TMOUT=600` no seu .bash_profile ou .profile para o utilizador que inicia sessão na consola, para o tempo limite de sessão após 10 minutos.

### <a name="disable-feature"></a>Desativar a funcionalidade
A funcionalidade de consola de série, pode ser desativada para específicas VMs, desativando a definição de diagnóstico de arranque essa VM.

## <a name="common-scenarios-for-accessing-serial-console"></a>Cenários comuns para aceder à consola de série 
Cenário          | Ações na consola de série                |  Aplicabilidade de SO 
:------------------|:-----------------------------------------|:------------------
Ficheiro FSTAB quebrado | A tecla ENTER para continuar e corrigir o ficheiro de fstab utilizando um editor de texto. Consulte [saber como corrigir problemas de fstab](https://support.microsoft.com/help/3206699/azure-linux-vm-cannot-start-because-of-fstab-errors) | Linux 
Regras de firewall incorreta | Aceder à consola de série e corrigir iptables ou as regras de firewall do Windows | Linux/Windows 
Sistema de ficheiros danificados/verificação | Aceder à consola de série e recuperar o sistema de ficheiros | Linux/Windows 
Problemas de configuração de SSH/RDP | Aceder à consola de série e alterar as definições | Linux/Windows 
Bloqueio de rede para baixo do sistema| Consola de série de acesso através do portal para gerir o sistema | Linux/Windows 
Interagir com bootloader | Acesso GRUB/BCD através da consola de série | Linux/Windows 

## <a name="accessing-serial-console-for-linux"></a>Aceder à consola de série para Linux
Por ordem para a consola de série funcionar corretamente, o sistema operativo convidado tem de ser configurado para ler e escrever mensagens de consola para a porta série. A maioria das [aprovadas as distribuições do Linux do Azure](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) tenham a consola de série configurada por predefinição. Apenas clicando no portal na secção de consola de série irá fornecer acesso à consola. 

### <a name="access-for-redhat"></a>Acesso de VM de RedHat 
Imagens de VM de RedHat disponíveis no Azure tem acesso à consola ativado por predefinição. Modo de utilizador único no Red Hat exige raiz utilizador ser ativado, que está desativada por predefinição. Se tiver uma necessidade para ativar o modo de utilizador único, utilize as instruções seguintes:

1. Inicie sessão no sistema do Red Hat através de SSH
2. Ativar a palavra-passe do utilizador raiz 
 * `passwd root` (definido uma palavra-passe de raiz segura)
3. Certifique-se de que utilizador raiz só pode iniciar sessão através do ttyS0
 * `edit /etc/ssh/sshd_config` e certifique-se de que PermitRootLog no está definido para nenhum
 * `edit /etc/securetty file` para permitir apenas o início de sessão através de ttyS0 

Agora se o sistema arranca no modo de utilizador único, pode iniciar sessão através de palavra-passe raiz.

Em alternativa para o modo de utilizador único RHEL 7.4 + ou 6.9 + pode ativar no GRUB pede-lhe, consulte as instruções [aqui](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/5/html/installation_guide/s1-rescuemode-booting-single)

### <a name="access-for-ubuntu"></a>Acesso para Ubuntu 
Ubuntu imagens disponíveis no Azure tem acesso à consola ativado por predefinição. Se o sistema arranca no modo de utilizador único pode aceder sem credenciais adicionais. 

### <a name="access-for-coreos"></a>Acesso para CoreOS
Imagens de CoreOS disponíveis no Azure tem acesso à consola ativado por predefinição. Se o sistema necessário pode ser reiniciado no modo de utilizador único através de alteração GRUB parâmetros e adicionar `coreos.autologin=ttyS0` seria ativar utilizador de núcleos a iniciar sessão e disponível na consola de série. 

### <a name="access-for-suse"></a>Acesso para SUSE
Imagens SLES disponíveis no Azure tem acesso à consola ativado por predefinição. Se estiver a utilizar versões anteriores do SLES no Azure, siga o [artigo BDC](https://www.novell.com/support/kb/doc.php?id=3456486) para ativar a consola de série. Mais recente imagens do SLES 12 SP3 + também permite o acesso através da consola de série no caso do sistema arranca no modo de emergência.

### <a name="access-for-centos"></a>Acesso para CentOS
Imagens de centOS disponíveis no Azure tem acesso à consola ativado por predefinição. Para o modo de utilizador único, seguir instruções semelhantes a Red Hat imagens acima. 

### <a name="access-for-oracle-linux"></a>Acesso para Oracle Linux
Oracle Linux as imagens disponíveis no Azure tem acesso à consola ativado por predefinição. Para o modo de utilizador único, seguir instruções semelhantes a Red Hat imagens acima.

### <a name="access-for-custom-linux-image"></a>Acesso para a imagem personalizada do Linux
Para ativar a consola de série para a imagem de VM com Linux personalizada, ative o acesso à consola no /etc/inittab com um terminal ttyS0. Abaixo está um exemplo para adicionar este no ficheiro inittab 

`S0:12345:respawn:/sbin/agetty -L 115200 console vt102` 

## <a name="errors"></a>Erros
A maioria dos erros são transitórios na natureza e o endereço de ligação de repetir estes. Tabela abaixo mostra uma lista de erros e atenuação 

Erro                            |   Mitigação 
:---------------------------------|:--------------------------------------------|
Não é possível obter as definições de diagnóstico de arranque para '<VMNAME>'. Para utilizar a consola de série, certifique-se de que esse diagnóstico de arranque está ativado para esta VM. | Certifique-se de que a VM possui [diagnóstico de arranque](boot-diagnostics.md) ativada. 
A VM está no estado parado desalocado. Iniciar a VM e repita a ligação da consola de série. | Máquina virtual deve estar num estado iniciado para aceder à consola de série
Não tem as permissões necessárias para utilizar esta consola de série de VM. Certifique-se de que tem, pelo menos, permissões de função de contribuinte de VM.| O acesso à consola de série requer determinada permissão para aceder. Consulte [aceder aos requisitos](#prerequisites) para obter detalhes
Não é possível determinar o grupo de recursos para a conta de armazenamento de diagnóstico de arranque '<STORAGEACCOUNTNAME>'. Certifique-se de que o diagnóstico de arranque está ativado para esta VM e que tem acesso a esta conta de armazenamento. | O acesso à consola de série requer determinada permissão para aceder. Consulte [aceder aos requisitos](#prerequisites) para obter detalhes

## <a name="known-issues"></a>Problemas conhecidos 
Tal como estamos ainda nas fases pré-visualização de acesso à consola de série, iremos estão a funcionar através de alguns problemas conhecidos, segue-se a lista de estas com possíveis soluções 

Problema                           |   Mitigação 
:---------------------------------|:--------------------------------------------|
Não há nenhuma opção com a consola de instância de conjunto de dimensionamento do virtual machine série |  No momento da pré-visualização, o acesso à consola de série de instâncias de conjunto de dimensionamento de máquina virtual não é suportado.
Atingir introduza depois da faixa de ligação não mostra um registo numa linha de comandos | [Atingir introduza faz nada](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md)


## <a name="frequently-asked-questions"></a>Perguntas mais frequentes 
**Q. Como posso enviar comentários?**

A. Fornecer comentários como um problema ao aceder à https://aka.ms/serialconsolefeedback. Em alternativa menos (preferencial) envie comentários através do azserialhelp@microsoft.com ou na categoria de máquina virtual http://feedback.azure.com

**Q.I obtiver um erro "consola existente tem de tipo de SO"Windows"com o tipo de SO pedido do Linux em conflito?**

A. Este é um problema conhecido para corrigir isto, basta abrir [Shell de nuvem do Azure](https://docs.microsoft.com/azure/cloud-shell/overview) no modo de bash e tente novamente.

**Q. Não consigo aceder à consola de série, onde ficheiro um incidente de suporte?**

A. Esta funcionalidade de pré-visualização é abrangida através de termos de pré-visualização do Azure. Suporte para isto é melhor processado através dos canais mencionados acima. 

## <a name="next-steps"></a>Passos Seguintes
* A consola de série do também está disponível para [Windows](../windows/serial-console.md) VMs
* Saiba mais sobre [bootdiagnostics](boot-diagnostics.md)