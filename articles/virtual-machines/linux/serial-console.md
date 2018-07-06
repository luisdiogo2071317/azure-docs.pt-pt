---
title: Consola de série de máquina virtual do Azure | Documentos da Microsoft
description: Consola de série de bidirecional para máquinas virtuais do Azure.
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
ms.openlocfilehash: 00a776131321500386b507f45ea84817b08147f7
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37867866"
---
# <a name="virtual-machine-serial-console-preview"></a>Consola de série de máquina virtual (pré-visualização) 


A consola de série de máquina virtual no Azure fornece acesso a um console baseado em texto para máquinas virtuais do Linux e Windows. Essa conexão serial é a porta serial de COM1 da máquina virtual e fornece acesso à máquina virtual e não estão relacionadas à rede da máquina virtual / operativos o estado do sistema. Acesso à consola de série para uma máquina virtual pode ser feito apenas através do portal do Azure atualmente e permitido apenas para os utilizadores que têm Contribuidor de VM ou acima de acesso à máquina virtual. 

> [!Note] 
> As pré-visualizações são tornadas disponíveis para, a condição de que concorda com os termos de utilização. Para obter mais informações, consulte [Microsoft Azure termos de utilização suplementares para pré-visualizações do Microsoft Azure.] (https://azure.microsoft.com/support/legal/preview-supplemental-terms/) Atualmente este serviço está em **pré-visualização pública** e acesso à consola de série para máquinas virtuais está disponível para regiões globais do Azure. Neste momento a consola de série não está disponível na cloud do Azure Government, Azure Alemanha e China do Azure.


## <a name="prerequisites"></a>Pré-requisitos 

* Tem de utilizar o modelo de implementação de gestão de recursos. Implementações clássicas não são suportadas. 
* Máquina virtual tem de ter [diagnósticos de arranque](boot-diagnostics.md) ativada 
* A conta a utilizar a consola de série tem de ter [função de contribuinte](../../role-based-access-control/built-in-roles.md) para a VM e o [diagnósticos de arranque](boot-diagnostics.md) conta de armazenamento. 
* Para as definições específicas para a distribuição Linux, consulte [aceder à consola de série para Linux](#accessing-serial-console-for-linux)


## <a name="open-the-serial-console"></a>Abra a consola de série
consola de série para máquinas virtuais só é acessível via [portal do Azure](https://portal.azure.com). Abaixo estão os passos para aceder à consola de série para máquinas virtuais através do portal 

  1. Abra o portal do Azure
  2. No menu da esquerda, selecione as máquinas virtuais.
  3. Clique na VM na lista. Abre a página de descrição geral para a VM.
  4. Desloque para baixo para o suporte + resolução de problemas de seção e clique na opção de consola de série (pré-visualização). Um novo painel com a consola de série abrirá e iniciar a ligação.

![](../media/virtual-machines-serial-console/virtual-machine-linux-serial-console-connect.gif)


> [!NOTE] 
> Consola de série requer um utilizador local com uma palavra-passe configurada. Neste momento, as VMs configuradas apenas com a chave pública SSH não terá acesso à consola de série. Para criar um utilizador local com a palavra-passe, siga [extensão de acesso de VM](https://docs.microsoft.com/azure/virtual-machines/linux/using-vmaccess-extension) e criar o utilizador local com a palavra-passe.

### <a name="disable-feature"></a>Desativar funcionalidade
A funcionalidade de consola de série pode ser desativada para VMs específicas ao desativar a definição de diagnóstico de arranque essa VM.

## <a name="serial-console-security"></a>Segurança da consola de série 

### <a name="access-security"></a>Segurança de acesso 
Acesso à consola de série está limitado a utilizadores que têm [contribuidores de VM](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) ou acima de acesso à máquina virtual. Se o inquilino do AAD requer o multi-factor Authentication, em seguida, acesso à consola de série também terá de MFA como o acesso é através de [portal do Azure](https://portal.azure.com).

### <a name="channel-security"></a>Segurança de canal
Todos os dados são enviados de volta e para trás, são encriptados na conexão.

### <a name="audit-logs"></a>Registos de auditoria
Todo o acesso à consola de série é iniciado a [diagnósticos de arranque](https://docs.microsoft.com/azure/virtual-machines/linux/boot-diagnostics) registos da máquina virtual. Acesso a estes registos são propriedade e controlado pelo administrador de máquina virtual do Azure.  

>[!CAUTION] 
Embora sem palavras-passe de acesso para a consola são registadas, se os comandos são executados dentro da consola contém ou palavras-passe, segredos, os nomes de utilizador ou qualquer outra forma de informação identificativa (PII) de saída, aqueles serão escritos para o diagnóstico de arranque de máquina virtual registos, juntamente com todos os outro texto visível, como parte da implementação da funcionalidade de scrollback da consola de série. Estes registos são circulares e apenas indivíduos com permissões de leitura à conta de armazenamento de diagnóstico tem acesso aos mesmos, no entanto, recomendamos que siga a prática recomendada de utilizar a consola SSH para tudo o que pode envolver segredos e/ou PII. 

### <a name="concurrent-usage"></a>Utilização em simultâneo
Se um usuário estiver conectado à consola de série e outro utilizador com êxito pedidos de acesso a essa mesma máquina virtual, o primeiro utilizador será desligado e o segundo utilizador ligado de forma semelhante o primeiro utilizador trabalhando em pé e deixar o console físico e um novo utilizador sentamos.

>[!CAUTION] 
Isso significa que o utilizador que for desligado não terminar a sessão! A capacidade de impor um fim de sessão após a desconexão (via SIGHUP ou mecanismo similar) ainda está no plano. Para o Windows há um tempo limite automático ativado no SAC, no entanto para Linux pode configurar a definição de tempo limite de terminal. Para fazer isso simplesmente de adicionar `export TMOUT=600` no seu. bash_profile ou .profile para o usuário fizer logon na consola com, para o tempo limite da sessão após 10 minutos.

### <a name="disable-feature"></a>Desativar funcionalidade
A funcionalidade de consola de série pode ser desativada para VMs específicas ao desativar a definição de diagnóstico de arranque essa VM.

## <a name="common-scenarios-for-accessing-serial-console"></a>Cenários comuns para aceder à consola de série 
Cenário          | Ações na consola de série                |  Aplicabilidade de SO 
:------------------|:-----------------------------------------|:------------------
Ficheiro FSTAB quebrado | A tecla ENTER para continuar e corrigir o ficheiro fstab com um editor de texto. Consulte [como corrigir problemas de fstab](https://support.microsoft.com/help/3206699/azure-linux-vm-cannot-start-because-of-fstab-errors) | Linux 
Regras de firewall incorreta | Aceder à consola de série e corrigir iptables ou regras de firewall do Windows | Linux/Windows 
Danos/verificação de sistema de ficheiros | Aceder à consola de série e recuperar o sistema de ficheiros | Linux/Windows 
Problemas de configuração de SSH/RDP | Aceder à consola de série e alterar as definições | Linux/Windows 
Bloqueio de rede para baixo do sistema| Consola de série de acesso através do portal para gerir o sistema | Linux/Windows 
Interagir com o carregador de inicialização | Acesso GRUB/BCD através da consola de série | Linux/Windows 

## <a name="accessing-serial-console-for-linux"></a>Aceder à consola de série para Linux
Por ordem para a consola de série funcionar corretamente, o sistema operativo convidado tem de ser configurado para ler e gravar mensagens de consola para a porta serial. A maioria dos [distribuições do Linux apoiadas pelo Azure](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) tenham a consola de série configurada por predefinição. Basta clicar no portal na secção de consola de série irá fornecer acesso à consola. 

### <a name="access-for-red-hat"></a>Acesso para Red Hat 
Imagens da Red Hat disponíveis no Azure tem acesso à consola ativado por predefinição. Modo de utilizador único no Red Hat requer o utilizador de raiz ser ativada, o que está desativada por predefinição. Se tiver uma necessidade para ativar o modo de utilizador único, utilize as instruções seguintes:

1. Iniciar sessão sistema de Red Hat através de SSH
2. Ativar a palavra-passe de utilizador de raiz 
 * `passwd root` (definido uma palavra-passe de raiz fortes)
3. Certifique-se de que o utilizador de raiz pode apenas iniciar sessão através de ttyS0
 * `edit /etc/ssh/sshd_config` e certifique-se de que PermitRootLog no está definido ou nenhum
 * `edit /etc/securetty file` para permitir apenas o início de sessão por meio de ttyS0 

Agora se o sistema for inicializado no modo de utilizador único pode iniciar sessão através de palavra-passe de raiz.

Como alternativa para o modo de utilizador único RHEL 7.4 + ou 6.9 + pode ativar no GRUB pede-lhe, veja as instruções [aqui](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/5/html/installation_guide/s1-rescuemode-booting-single)

### <a name="access-for-ubuntu"></a>Acesso para Ubuntu 
Imagens do Ubuntu disponíveis no Azure têm acesso à consola ativado por predefinição. Se o sistema for inicializado no modo de utilizador único pode aceder sem credenciais adicionais. 

### <a name="access-for-coreos"></a>Acesso para CoreOS
As imagens de CoreOS disponíveis no Azure têm acesso à consola ativado por predefinição. Se o sistema necessário pode ser inicializado em modo de utilizador único por meio de alterar os parâmetros GRUB e adicionar `coreos.autologin=ttyS0` permitiria o utilizador de núcleos a iniciar sessão e disponível na consola de série. 

### <a name="access-for-suse"></a>Acesso para SUSE
As imagens SLES disponíveis no Azure têm acesso à consola ativado por predefinição. Se estiver a utilizar versões mais antigas do SLES no Azure, siga os [artigo BDC](https://www.novell.com/support/kb/doc.php?id=3456486) para ativar a consola de série. Mais recente imagens de SLES 12 SP3 + também permite o acesso através da consola de série no caso do sistema for inicializado no modo de emergência.

### <a name="access-for-centos"></a>Acesso para CentOS
As imagens de centOS disponíveis no Azure têm acesso à consola ativado por predefinição. Modo de utilizador único, seguir instruções semelhantes para imagens do Red Hat acima. 

### <a name="access-for-oracle-linux"></a>Acesso para Oracle Linux
Imagens do Oracle Linux disponíveis no Azure tem acesso à consola ativado por predefinição. Modo de utilizador único, seguir instruções semelhantes para imagens do Red Hat acima.

### <a name="access-for-custom-linux-image"></a>Acesso para a imagem personalizada do Linux
Para ativar a consola de série para a sua imagem de VM do Linux personalizada, permitem o acesso de consola no /etc/inittab para executar um terminal em ttyS0. Segue-se um exemplo para adicionar isso no arquivo inittab 

`S0:12345:respawn:/sbin/agetty -L 115200 console vt102` 

## <a name="errors"></a>Erros
A maioria dos erros são transitórios na natureza e o endereço de ligação de repetir estes. Tabela a seguir mostra uma lista de erros e atenuação 

Erro                            |   Mitigação 
:---------------------------------|:--------------------------------------------|
Não é possível obter as definições de diagnóstico de arranque para "<VMNAME>'. Para utilizar a consola de série, certifique-se de que o diagnóstico de arranque está ativado para esta VM. | Certifique-se de que a VM tem [diagnósticos de arranque](boot-diagnostics.md) ativada. 
A VM está num estado parado desalocado. Iniciar a VM e repita a ligação da consola de série. | Máquina virtual deve estar num estado iniciado para aceder à consola de série
Não tem as permissões necessárias para utilizar esta VM da consola de série. Certifique-se de que tem, pelo menos, permissões de função de Contribuidor de VM.| O acesso de consola de série requer determinada permissão para aceder. Ver [os requisitos de acesso](#prerequisites) para obter detalhes
Não é possível determinar o grupo de recursos para a conta de armazenamento do diagnóstico de arranque '<STORAGEACCOUNTNAME>'. Certifique-se de que o diagnóstico de arranque está ativado para esta VM e que tem acesso a esta conta de armazenamento. | O acesso de consola de série requer determinada permissão para aceder. Ver [os requisitos de acesso](#prerequisites) para obter detalhes

## <a name="known-issues"></a>Problemas conhecidos 
Como estamos ainda nas fases de pré-visualização para acesso à consola de série, vamos trabalhar através de alguns problemas conhecidos, segue-se a lista destas com soluções alternativas possíveis 

Problema                           |   Mitigação 
:---------------------------------|:--------------------------------------------|
Não existe nenhuma opção com a consola de instância de conjunto de dimensionamento do virtual machine serial |  Durante a pré-visualização, o acesso à consola de série para instâncias do conjunto de dimensionamento de máquina virtual não é suportado.
Acessando introdução após a faixa de ligação não mostra um registo na linha de comandos | [Acessando introduza não faz nada](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md)


## <a name="frequently-asked-questions"></a>Perguntas mais frequentes 
**P. Como posso enviar comentários?**

A. Fornecer comentários como um problema ao aceder https://aka.ms/serialconsolefeedback. Em alternativa menos (preferencial) envie comentários através das azserialhelp@microsoft.com ou na categoria de máquina virtual http://feedback.azure.com

**Q.I obtiver um erro "o console de existente tem o tipo de SO"Windows"com o tipo de SO solicitado do Linux em conflito?**

A. Este é um problema conhecido para corrigir isso, basta abrir [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) no modo de bash e tente novamente.

**P. Não consigo aceder à consola de série, onde pode enviar um incidente de suporte?**

A. Esta funcionalidade de pré-visualização é abrangida por meio de termos de pré-visualização do Azure. Suporte para isso é melhor processado por meio de canais mencionadas acima. 

## <a name="next-steps"></a>Passos Seguintes
* Também está disponível para a consola de série [Windows](../windows/serial-console.md) VMs
* Saiba mais sobre [bootdiagnostics](boot-diagnostics.md)