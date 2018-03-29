---
title: Consola de série de máquina virtual do Azure | Microsoft Docs
description: Consola de série bidirecional para máquinas virtuais do Azure.
services: virtual-machines-windows
documentationcenter: ''
author: harijay
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/05/2018
ms.author: harijay
ms.openlocfilehash: 9c16114b4f8d335dc750b1beef1fb6204ae20e0f
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

## <a name="open-the-serial-console"></a>Abra a consola de série
consola de série para máquinas virtuais só é acessível através de [portal do Azure](https://portal.azure.com). Seguem-se os passos para aceder à consola de série para máquinas virtuais através do portal 

  1. Abra o portal do Azure
  2. No menu à esquerda, selecione as máquinas virtuais.
  3. Clique na VM na lista. Abre a página de descrição geral para a VM.
  4. Desloque para baixo para o suporte + secção de resolução de problemas e clique na opção de consola de série (pré-visualização). Um novo painel com a consola de série irá abrir e iniciar a ligação.

![](../media/virtual-machines-serial-console/virtual-machine-windows-serial-console-connect.gif)

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
Enquanto nenhuma palavra-passe de acesso para a consola sessão iniciada, se executar a consola de comandos contém ou palavras-passe, segredos, os nomes de utilizador ou qualquer outra forma de informação identificativa (PII) de saída, às serão escritos para o diagnóstico de arranque de máquina virtual os registos, juntamente com todos os outro texto visível, como parte da implementação da funcionalidade de scrollback da consola de série. Estes registos são circulares e apenas indivíduos com permissões de leitura para a conta de armazenamento do diagnostics têm acesso aos mesmos, no entanto, recomendamos a seguir a melhor prática utilizando o ambiente de trabalho remoto para tudo o que poderá envolver o método segredos e/ou PII. 

### <a name="concurrent-usage"></a>Utilização em simultâneo
Se um utilizador está ligado à consola de série e outro utilizador com êxito os pedidos de acesso a essa mesma máquina virtual, o primeiro utilizador será desligado e o segundo utilizador ligados de forma akin to o primeiro utilizador colocado cópias de segurança e mantendo a consola física e um novo utilizador junto para baixo.

>[!CAUTION] 
Isto significa que o utilizador for desligado não será registado! A capacidade de impor um fim de sessão após desligar (através de SIGHUP ou mecanismo semelhante) ainda está a ser plano. Para o Windows não há um limite de tempo automática ativada no SAC, no entanto para Linux pode configurar definição de tempo limite de terminal. 


## <a name="accessing-serial-console-for-windows"></a>Aceder à consola de série para Windows 
Terão de imagens do Windows Server mais recentes no Azure [especial consola administrativa](https://technet.microsoft.com/library/cc787940(v=ws.10).aspx) (SAC) ativada por predefinição. SAC é suportada em versões de servidor do Windows, mas não está disponível nas versões de cliente (por exemplo, Windows 10, Windows 8 ou Windows 7). Para ativar a consola de série para máquinas virtuais do Windows criada com a utilização Feb2018 ou imagens inferiores, consulte os seguintes passos: 

1. Ligar a sua máquina virtual do Windows através do ambiente de trabalho remoto
2. A partir de uma linha de comandos administrativa, execute os seguintes comandos 
* `bcdedit /ems {current} on`
* `bcdedit /emssettings EMSPORT:1 EMSBAUDRATE:115200`
3. Reiniciar o sistema para a consola de SAC esteja ativado

![](../media/virtual-machines-serial-console/virtual-machine-windows-serial-console-connect.gif)

Se SAC necessárias pode ser ativadas offline bem, 

1. Anexe o disco do windows que pretende SAC configurado para como um disco de dados para a VM existente. 
2. A partir de uma linha de comandos administrativa, execute os seguintes comandos 
* `bcdedit /store <mountedvolume>\boot\bcd /ems {default} on`
* `bcdedit /store <mountedvolume>\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200`

### <a name="how-do-i-know-if-sac-is-enabled-or-not"></a>Como saber se SAC está ativada ou não 

Se [SAC] (https://technet.microsoft.com/library/cc787940(v=ws.10).aspx) não está ativada a consola de série não apresentará uma linha de comandos da SAC. Esta operação pode mostrar informações de um Estado de funcionamento da VM em alguns casos ou seria em branco.  

### <a name="enabling-boot-menu-to-show-in-the-serial-console"></a>Ativar o menu de arranque Mostrar na consola de série 

Se precisar de ativar o carregador de arranque do Windows pede-lhe para mostrar, na consola de série, que pode adicionar as seguintes opções adicionais para o carregador de arranque do Windows.

1. Ligar a sua máquina virtual do Windows através do ambiente de trabalho remoto
2. A partir de uma linha de comandos administrativa, execute os seguintes comandos 
* `bcdedit /set {bootmgr} displaybootmenu yes`
* `bcdedit /set {bootmgr} timeout 5`
* `bcdedit /set {bootmgr} bootems yes`
3. Reiniciar o sistema para o menu de arranque esteja ativado

> [!NOTE] 
> No suporte neste ponto para a função de chaves não está ativado, se necessitar de opções de arranque avançadas utilizam bcdedit /set {atual} onetimeadvancedoptions, consulte [bcdedit](https://docs.microsoft.com/windows-hardware/drivers/devtest/bcdedit--set) para obter mais detalhes

## <a name="common-scenarios-for-accessing-windows-serial-console"></a>Cenários comuns para aceder à consola de série do Windows 
Cenário          | Ações na consola de série                
:------------------|:----------------------------------------
Regras de firewall incorreta | Aceder à consola de série e corrigir iptables ou as regras de firewall do Windows 
Sistema de ficheiros danificados/verificação | Aceder à consola de série e recuperar o sistema de ficheiros após o início de sessão em SAC CMD
Problemas de configuração de RDP | Aceder à consola de série e inicie sessão no canal cmd. Verifique o estado de funcionamento dos serviços de Terminal e reiniciar se for necessário.
Bloqueio de rede para baixo do sistema| Consola de série de acesso e inicie sessão no canal cmd. Verificar o estado de firewall por [netsh](https://docs.microsoft.com/windows-server/networking/technologies/netsh/netsh-contexts) linha de comandos. 

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
Não há nenhuma opção com a consola de instância de conjunto de dimensionamento do virtual machine série | No momento da pré-visualização, o acesso à consola de série de instâncias de conjunto de dimensionamento de máquina virtual não é suportado.
Atingir introduza depois da faixa de ligação não mostra um registo numa linha de comandos | [Atingir introduza faz nada](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md)
Apenas as informações de estado de funcionamento são apresentadas ao ligar a uma VM do Windows| [Sinais de estado de funcionamento do Windows](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Windows_Health_Info.md)

## <a name="frequently-asked-questions"></a>Perguntas mais frequentes 
**Q. Como posso enviar comentários?**

A. Fornecer comentários como um problema ao aceder à https://aka.ms/serialconsolefeedback. Em alternativa menos (preferencial) envie comentários através do azserialhelp@microsoft.com ou na categoria de máquina virtual http://feedback.azure.com 

**Q. Posso obter um erro "consola existente tem de tipo de SO"Windows"com o tipo de SO pedido do Linux em conflito?**

A. Este é um problema conhecido para corrigir isto, basta abrir [Shell de nuvem do Azure](https://docs.microsoft.com/azure/cloud-shell/overview) no modo de bash e tente novamente.

**Q. Não consigo aceder à consola de série, onde ficheiro um incidente de suporte?**

A. Esta funcionalidade de pré-visualização é abrangida através de termos de pré-visualização do Azure. Suporte para isto é melhor processado através dos canais mencionados acima. 

## <a name="next-steps"></a>Passos Seguintes
* Também está disponível para a consola de série [Linux](../linux/serial-console.md) VMs
* Saiba mais sobre [bootdiagnostics](boot-diagnostics.md)
