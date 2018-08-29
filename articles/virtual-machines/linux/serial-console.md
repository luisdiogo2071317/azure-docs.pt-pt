---
title: Consola de série de Máquina Virtual do Azure | Documentos da Microsoft
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
ms.date: 08/07/2018
ms.author: harijay
ms.openlocfilehash: d4ca44268740f48702594d9c87aa568d4f8eecb6
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/28/2018
ms.locfileid: "43122410"
---
# <a name="virtual-machine-serial-console-preview"></a>Consola de série de máquina virtual (pré-visualização) 


A consola de série de Máquina Virtual no Azure fornece acesso a um console baseado em texto para máquinas virtuais do Linux e Windows. Essa conexão serial é a porta serial de COM1 da máquina virtual e fornece acesso à máquina virtual e não estão relacionadas à rede da máquina virtual / operativos o estado do sistema. Acesso à consola de série para uma máquina virtual pode ser feito apenas através do portal do Azure atualmente e permitido apenas para os utilizadores que têm Contribuidor de VM ou acima de acesso à máquina virtual. 

Para obter a documentação da consola de série para VMs do Windows, [clique aqui](../windows/serial-console.md).

> [!Note] 
> As pré-visualizações são tornadas disponíveis para, a condição de que concorda com os termos de utilização. Para obter mais informações, consulte [Microsoft Azure termos de utilização suplementares para pré-visualizações do Microsoft Azure.] (https://azure.microsoft.com/support/legal/preview-supplemental-terms/) Atualmente este serviço está em **pré-visualização pública** e acesso à consola de série para máquinas virtuais está disponível para regiões globais do Azure. Neste momento a consola de série não está disponível na cloud do Azure Government, Azure Alemanha e China do Azure.


## <a name="prerequisites"></a>Pré-requisitos 

* Tem de utilizar o modelo de implementação de gestão de recursos. Implementações clássicas não são suportadas. 
* A máquina virtual tem de ter [diagnósticos de arranque](boot-diagnostics.md) habilitado - veja a captura de ecrã abaixo.

    ![](../media/virtual-machines-serial-console/virtual-machine-serial-console-diagnostics-settings.png)
    
* A conta do Azure através da consola de série tem de ter [função de contribuinte](../../role-based-access-control/built-in-roles.md) para a VM e o [diagnósticos de arranque](boot-diagnostics.md) conta de armazenamento. 
* A máquina virtual para o qual é a consola de série acessing também tem de ter uma conta baseada em palavra-passe. Pode criar uma com o [Repor palavra-passe](https://docs.microsoft.com/azure/virtual-machines/extensions/vmaccess#reset-password) funcionalidade de extensão de acesso da VM - veja a captura de ecrã abaixo.

    ![](../media/virtual-machines-serial-console/virtual-machine-serial-console-reset-password.png)

* Para configurações específicas de distribuições Linux, consulte [aceder à consola de série para Linux](#access-serial-console-for-linux)



## <a name="get-started-with-serial-console"></a>Introdução à consola de série
Consola de série para máquinas virtuais só é acessível via [portal do Azure](https://portal.azure.com). Abaixo estão os passos para aceder à consola de série para máquinas virtuais através do portal 

  1. Abra o portal do Azure
  2. No menu da esquerda, selecione as máquinas virtuais.
  3. Clique na VM na lista. Abre a página de descrição geral para a VM.
  4. Desloque para baixo para o suporte + resolução de problemas de seção e clique na opção de consola de série (pré-visualização). Um novo painel com a consola de série abrirá e iniciar a ligação.

![](../media/virtual-machines-serial-console/virtual-machine-linux-serial-console-connect.gif)


> [!NOTE] 
> Consola de série requer um utilizador local com uma palavra-passe configurada. Neste momento, as VMs configuradas apenas com uma chave pública SSH não terá acesso à consola de série. Para criar um utilizador local com a palavra-passe, utilize o [extensão de acesso de VM](https://docs.microsoft.com/azure/virtual-machines/linux/using-vmaccess-extension) (também disponível no portal clicando em "Repor palavra-passe") e criar um utilizador local com uma palavra-passe.

## <a name="disable-serial-console"></a>Desativar a consola de série
Por predefinição, todas as subscrições têm acesso de consola de série ativado para todas as VMs. Pode desativar a consola de série no nível de assinatura ou o nível VM.

### <a name="subscription-level-disable"></a>Desativar o nível de assinatura
Consola de série pode ser desabilitada para uma subscrição completa por através da [chamada à API de REST de consola desativar](https://aka.ms/disableserialconsoleapi). Pode usar a funcionalidade "Experimente-lo" disponível na página de documentação da API desativar e ativar a consola de série para uma subscrição. Introduza o seu `subscriptionId`, "padrão" no `default` campo e clique em executar. Os comandos da CLI do Azure ainda não estão disponíveis e vão deparar-se numa data posterior. [Experimente a chamada de API do REST aqui](https://aka.ms/disableserialconsoleapi).

![](../media/virtual-machines-serial-console/virtual-machine-serial-console-rest-api-try-it.png)

Em alternativa, pode utilizar o conjunto de comandos abaixo no Cloud Shell (comandos de bash mostrados) para desativar, ativar e ver o estado disbled da consola de série para uma subscrição. 

* Para obter o estado desativado da consola de série de uma subscrição:
    ```azurecli-interactive
    $ export ACCESSTOKEN=($(az account get-access-token --output=json | jq .accessToken | tr -d '"')) 

    $ export SUBSCRIPTION_ID=$(az account show --output=json | jq .id -r)

    $ curl "https://management.azure.com/subscriptions/$SUBSCRIPTION_ID/providers/Microsoft.SerialConsole/consoleServices/default?api-version=2018-05-01" -H "Authorization: Bearer $ACCESSTOKEN" -H "Content-Type: application/json" -H "Accept: application/json" -s | jq .properties
    ```
* Para desativar a consola de série para uma subscrição:
    ```azurecli-interactive
    $ export ACCESSTOKEN=($(az account get-access-token --output=json | jq .accessToken | tr -d '"')) 

    $ export SUBSCRIPTION_ID=$(az account show --output=json | jq .id -r)

    $ curl -X POST "https://management.azure.com/subscriptions/$SUBSCRIPTION_ID/providers/Microsoft.SerialConsole/consoleServices/default/disableConsole?api-version=2018-05-01" -H "Authorization: Bearer $ACCESSTOKEN" -H "Content-Type: application/json" -H "Accept: application/json" -s -H "Content-Length: 0"
    ```
* Para ativar a consola de série para uma subscrição:
    ```azurecli-interactive
    $ export ACCESSTOKEN=($(az account get-access-token --output=json | jq .accessToken | tr -d '"')) 

    $ export SUBSCRIPTION_ID=$(az account show --output=json | jq .id -r)

    $ curl -X POST "https://management.azure.com/subscriptions/$SUBSCRIPTION_ID/providers/Microsoft.SerialConsole/consoleServices/default/enableConsole?api-version=2018-05-01" -H "Authorization: Bearer $ACCESSTOKEN" -H "Content-Type: application/json" -H "Accept: application/json" -s -H "Content-Length: 0"
    ```

### <a name="vm-level-disable"></a>Desativar ao nível da VM
Consola de série pode ser desativada para VMs específicas ao desativar a definição de diagnóstico de arranque essa VM. Basta desativar o diagnóstico de arranque a partir do portal do Azure e a consola de série será desativada para a VM.

## <a name="serial-console-security"></a>Segurança da consola de série 

### <a name="access-security"></a>Segurança de acesso 
Acesso à consola de série está limitado a utilizadores que têm [contribuidores de VM](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) ou acima de acesso à máquina virtual. Se o inquilino do AAD requer o multi-factor Authentication, em seguida, acesso à consola de série também terá de MFA como o acesso é através de [portal do Azure](https://portal.azure.com).

### <a name="channel-security"></a>Segurança de canal
Todos os dados que são enviados e volta são encriptados na conexão.

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
Ficheiro FSTAB quebrado | `Enter` chave para continuar e corrigir o ficheiro fstab com um editor de texto. Terá de estar no modo de utilizador único para isso. Ver [como corrigir problemas de fstab](https://support.microsoft.com/help/3206699/azure-linux-vm-cannot-start-because-of-fstab-errors) e [consola de série a utilizar para aceder a GRUB e modo de utilizador único](serial-console-grub-single-user-mode.md) para começar a utilizar. | Linux 
Regras de firewall incorreta | Aceder à consola de série e corrigir iptables ou regras de firewall do Windows. | Linux/Windows 
Danos/verificação de sistema de ficheiros | Aceder à consola de série e recuperar o sistema de ficheiros. | Linux/Windows 
Problemas de configuração de SSH/RDP | Aceder à consola de série e alterar as definições. | Linux/Windows 
Bloqueio de rede para baixo do sistema| Consola de série de acesso através do portal para gerir o sistema. | Linux/Windows 
Interagir com o carregador de inicialização | Acesso GRUB/BCD através da consola de série. Aceda a [consola de série a utilizar para aceder a GRUB e modo de utilizador único](serial-console-grub-single-user-mode.md) para começar a utilizar. | Linux/Windows 

## <a name="access-serial-console-for-linux"></a>Consola de série de acesso para Linux
Por ordem para a consola de série funcionar corretamente, o sistema operativo convidado tem de ser configurado para ler e gravar mensagens de consola para a porta serial. A maioria dos [distribuições do Linux apoiadas pelo Azure](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) tenham a consola de série configurada por predefinição. Clicar simplesmente a secção de consola de série no portal do Azure irá fornecer acesso à consola. 

Distro      | Acesso de consola de série
:-----------|:---------------------
Red Hat Enterprise Linux    | Red Hat Enterprise Linux imagens disponíveis no Azure tem acesso à consola ativado por predefinição. 
CentOS      | As imagens de centOS disponíveis no Azure têm acesso à consola ativado por predefinição. 
Ubuntu      | Imagens do Ubuntu disponíveis no Azure têm acesso à consola ativado por predefinição.
CoreOS      | As imagens de CoreOS disponíveis no Azure têm acesso à consola ativado por predefinição.
SUSE        | Mais recente SLES as imagens disponíveis no Azure têm acesso à consola ativado por predefinição. Se estiver a utilizar as versões mais antigas (10 ou abaixo) de SLES no Azure, siga os [artigo BDC](https://www.novell.com/support/kb/doc.php?id=3456486) para ativar a consola de série. 
Oracle Linux        | Imagens do Oracle Linux disponíveis no Azure tem acesso à consola ativado por predefinição.
Imagens do Linux personalizadas     | Para ativar a consola de série para a sua imagem de VM do Linux personalizada, permitem o acesso de consola no /etc/inittab para executar um terminal em ttyS0. Eis um exemplo para adicionar isso no arquivo inittab: `S0:12345:respawn:/sbin/agetty -L 115200 console vt102`. Para obter mais informações sobre como criar adequadamente imagens personalizadas, consulte [criar e carregar um VHD do Linux no Azure](https://aka.ms/createuploadvhd).

## <a name="errors"></a>Erros
A maioria dos erros são de natureza transitórios e tentar novamente, muitas vezes, a ligação da consola de série aborda estas. A tabela abaixo mostra uma lista de erros e mitigações

Erro                            |   Mitigação 
:---------------------------------|:--------------------------------------------|
Não é possível obter as definições de diagnóstico de arranque para "<VMNAME>'. Para utilizar a consola de série, certifique-se de que o diagnóstico de arranque está ativado para esta VM. | Certifique-se de que a VM tem [diagnósticos de arranque](boot-diagnostics.md) ativada. 
A VM está num estado parado desalocado. Iniciar a VM e repita a ligação da consola de série. | Máquina virtual deve estar num estado iniciado para aceder à consola de série
Não tem as permissões necessárias para utilizar esta VM da consola de série. Certifique-se de que tem, pelo menos, permissões de função de Contribuidor de VM.| O acesso de consola de série requer determinada permissão para aceder. Ver [os requisitos de acesso](#prerequisites) para obter detalhes
Não é possível determinar o grupo de recursos para a conta de armazenamento do diagnóstico de arranque '<STORAGEACCOUNTNAME>'. Certifique-se de que o diagnóstico de arranque está ativado para esta VM e que tem acesso a esta conta de armazenamento. | O acesso de consola de série requer determinada permissão para aceder. Ver [os requisitos de acesso](#prerequisites) para obter detalhes
Web socket foi fechado ou não foi possível abrir. | Poderá ter de lista branca `*.console.azure.com`. Um mais detalhado for mas a abordagem mais é à lista de permissões a [intervalos de IP de Datacenter do Microsoft Azure](https://www.microsoft.com/en-us/download/details.aspx?id=41653), que alterar bastante regularmente.
## <a name="known-issues"></a>Problemas conhecidos 
Como estamos ainda nas fases de pré-visualização para acesso à consola de série, estamos a trabalhar por meio de alguns problemas conhecidos. Segue-se a lista destas com soluções possíveis:

Problema                           |   Mitigação 
:---------------------------------|:--------------------------------------------|
Não existe nenhuma opção com a consola de instância de conjunto de dimensionamento do virtual machine serial |  Durante a pré-visualização, o acesso à consola de série para instâncias do conjunto de dimensionamento de máquina virtual não é suportado.
Acessando introdução após a faixa de ligação não mostra um registo na linha de comandos | Consulte esta página: [Hitting introduza não faz nada](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md). Isto pode acontecer se estiver a executar uma VM personalizada, a aplicação protegida ou a configuração GRUB que faz com que o Linux não sejam corretamente ligar para a porta serial.
Uma resposta de "Proibido" foi encontrada ao aceder à conta de armazenamento do diagnóstico de arranque desta VM. | Certifique-se de que o diagnóstico de arranque não tem uma firewall de conta. Uma conta de armazenamento do diagnóstico de arranque acessível é necessária para a consola de série função.


## <a name="frequently-asked-questions"></a>Perguntas mais frequentes 
**P. Como posso enviar comentários?**

A. Fornecer comentários como um problema ao aceder https://aka.ms/serialconsolefeedback. Em alternativa menos (preferencial) envie comentários através das azserialhelp@microsoft.com ou na categoria de máquina virtual http://feedback.azure.com

**P. Não consigo aceder à consola de série, onde pode enviar um incidente de suporte?**

A. Esta funcionalidade de pré-visualização é abrangida por meio de termos de pré-visualização do Azure. Suporte para isso é melhor processado por meio de canais mencionadas acima. 

## <a name="next-steps"></a>Passos Seguintes
* Utilizar a consola de série para [arrancar GRUB e introduza o modo de utilizador único](serial-console-grub-single-user-mode.md)
* Utilizar a consola de série para [chamadas NMI e SysRq](serial-console-nmi-sysrq.md)
* Também está disponível para a consola de série [Windows](../windows/serial-console.md) VMs
* Saiba mais sobre [diagnósticos de arranque](boot-diagnostics.md)