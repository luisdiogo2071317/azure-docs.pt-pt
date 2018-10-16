---
title: Consola de série de Máquina Virtual do Azure | Documentos da Microsoft
description: Consola de série de bidirecional para máquinas virtuais do Azure.
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
ms.date: 08/07/2018
ms.author: harijay
ms.openlocfilehash: ee42c279abaf9282b6f37e2b00050d33ebd093d2
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49318270"
---
# <a name="virtual-machine-serial-console"></a>Consola de série de máquina virtual


A consola de série de Máquina Virtual no Azure fornece acesso a um console baseado em texto para as máquinas virtuais do Windows. Esta conexão serial é a porta serial COM1 da máquina virtual, fornecendo acesso para a máquina virtual que é independente da rede ou o estado do sistema operativo de uma máquina virtual. Aceder à consola de série para uma máquina virtual pode atualmente apenas ser feito através do portal do Azure e é permitida apenas para os utilizadores que têm Contribuidor de VM ou acima de acesso à máquina virtual. 

Para obter a documentação da consola de série para VMs do Linux [clique aqui](serial-console-linux.md).

> [!NOTE] 
> Consola de série para máquinas virtuais está disponível em geral em regiões globais do Azure. Neste momento consola de série ainda não está disponível em clouds do Azure Government ou Azure China.

 

## <a name="prerequisites"></a>Pré-requisitos 

* Tem de utilizar o modelo de implementação de gestão de recursos. Implementações clássicas não são suportadas. 
* Máquina virtual tem de ter [diagnósticos de arranque](boot-diagnostics.md) ativada 

    ![](../media/virtual-machines-serial-console/virtual-machine-serial-console-diagnostics-settings.png)

* A conta a utilizar a consola de série tem de ter [função de contribuinte](../../role-based-access-control/built-in-roles.md) para a VM e o [diagnósticos de arranque](boot-diagnostics.md) conta de armazenamento. 
* A máquina virtual para o qual é a consola de série acessing também tem de ter uma conta baseada em palavra-passe. Pode criar uma com o [Repor palavra-passe](https://docs.microsoft.com/azure/virtual-machines/extensions/vmaccess#reset-password) funcionalidade de extensão de acesso da VM - veja a captura de ecrã abaixo.

    ![](../media/virtual-machines-serial-console/virtual-machine-serial-console-reset-password.png)

## <a name="get-started-with-serial-console"></a>Introdução à consola de série
Consola de série para máquinas virtuais só é acessível via [portal do Azure](https://portal.azure.com). Seguem-se os passos para aceder à consola de série para máquinas virtuais através do portal.

  1. Abra o portal do Azure
  2. No menu da esquerda, selecione as máquinas virtuais.
  3. Clique na VM na lista. Abre a página de descrição geral para a VM.
  4. Desloque para baixo para o suporte + resolução de problemas de seção e clique na opção "Consola de série". Um novo painel com a consola de série abrirá e iniciar a ligação.


## <a name="enable-serial-console-in-custom-or-older-images"></a>Ativar a consola de série em imagens personalizadas ou mais antigas
Imagens do Windows Server mais recente no Azure terá [consola administrativa especial](https://technet.microsoft.com/library/cc787940(v=ws.10).aspx) (SAC) ativada por predefinição. SAC é suportada em versões de servidor do Windows, mas não está disponível nas versões de cliente (por exemplo, Windows 10, Windows 8 ou Windows 7). Para ativar a consola de série para máquinas de virtuais do Windows criada antes de Fevereiro de 2018, utilize os seguintes passos: 

1. Ligar à máquina virtual Windows através de ambiente de trabalho remoto
2. A partir de uma linha de comandos administrativa, execute os seguintes comandos 
* `bcdedit /ems {current} on`
* `bcdedit /emssettings EMSPORT:1 EMSBAUDRATE:115200`
3. Reinicie o sistema para a consola de SAC seja ativado

![](/media/virtual-machines-serial-console/virtual-machine-windows-serial-console-connect.gif)

Se for necessário, o SAC pode ser ativadas offline também:

1. Anexe o disco do windows que pretende SAC configurado para como um disco de dados para uma VM existente. 
2. A partir de uma linha de comandos administrativa, execute os seguintes comandos 
* `bcdedit /store <mountedvolume>\boot\bcd /ems {default} on`
* `bcdedit /store <mountedvolume>\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200`

### <a name="how-do-i-know-if-sac-is-enabled"></a>Como posso saber se SAC está ativado?

Se [SAC] (https://technet.microsoft.com/library/cc787940(v=ws.10).aspx) não está ativada a consola de série não serão apresentados na linha de comandos da SAC. Em alguns casos, serão apresentadas informações de estado de funcionamento da VM e, em outros casos estará em branco. Se estiver a utilizar uma imagem do Windows Server criada antes de Fevereiro de 2018, SAC provavelmente não será ativado.

## <a name="enable-the-windows-boot-menu-in-serial-console"></a>Habilitar o Menu de arranque do Windows na consola de série 

Se precisar de ativar o carregador de inicialização do Windows pede-lhe para mostrar na consola de série, pode adicionar as seguintes opções adicionais aos seus dados de configuração de arranque. Ver [bcdedit](https://docs.microsoft.com/windows-hardware/drivers/devtest/bcdedit--set) para obter mais detalhes

1. Ligar à máquina virtual Windows através de ambiente de trabalho remoto
2. A partir de uma linha de comandos administrativa, execute os seguintes comandos 
* `bcdedit /set {bootmgr} displaybootmenu yes`
* `bcdedit /set {bootmgr} timeout 10`
* `bcdedit /set {bootmgr} bootems yes`
3. Reinicie o sistema para o menu de arranque seja ativado

> [!NOTE] 
> O tempo limite que definiu para o menu de Gestor de arranque a aparecer irá afetar o tempo de arranque de sistema operacional no futuro. Embora possa ser aceitável para algumas para adicionar o limite de tempo segundo 10 para garantir que o Gerenciador de inicialização é visível através da consola de série, outras pessoas poderá limite de tempo mais curto ou longo. Defina o valor de tempo limite como um valor que se sente confortável.

## <a name="use-serial-console-for-nmi-calls-in-windows-vms"></a>Utilizar a consola de série para chamadas de NMI em VMs do Windows
Uma interrupção não maskable (NMI) foi concebida para criar um sinal de que o software numa máquina virtual não será ignorada. Historicamente, NMIs foram utilizadas para monitorizar a existência de problemas de hardware em sistemas que exigem tempos de resposta específica.  Hoje, os programadores e administradores de sistema utilizam frequentemente NMI como um mecanismo para depurar ou resolver problemas de sistemas que estão a ser suspenso.

A consola de série pode ser utilizada para enviar um NMI para uma máquina virtual do Azure com o ícone de teclado na barra de comando mostrada abaixo. Depois do NMI é enviado, a configuração de máquina virtual irá controlar como o sistema responde. Windows podem ser configurado para falhas e criar um despejo de memória, ao receber um NMI.

![](../media/virtual-machines-serial-console/virtual-machine-windows-serial-console-nmi.png) <br>

Para obter informações sobre como configurar o Windows para criar um despejo de falha quando recebe um NMI, consulte: [como gerar um arquivo de despejo de falha completa ou de um arquivo de despejo de falhas de kernel ao utilizar um NMI num sistema baseado em Windows](https://support.microsoft.com/en-us/help/927069/how-to-generate-a-complete-crash-dump-file-or-a-kernel-crash-dump-file)

## <a name="disable-serial-console"></a>Desativar a consola de série
Por predefinição, todas as subscrições têm acesso de consola de série ativado para todas as VMs. Pode desativar a consola de série no nível de assinatura ou o nível VM.

> [!NOTE]       
> Para ativar ou desativar a consola de série para uma subscrição, tem de ter permissões de escrita para a subscrição. Isto inclui, mas não se limita às funções de administrador ou proprietário. Funções personalizadas também podem ter permissões de escrita.

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
Embora sem palavras-passe de acesso para a consola são registadas, se os comandos são executados dentro da consola contém ou palavras-passe, segredos, os nomes de utilizador ou qualquer outra forma de informação identificativa (PII) de saída, aqueles serão escritos para o diagnóstico de arranque de máquina virtual funcionalidade de cópia de registos, juntamente com todos os outro texto visível, como parte da implementação de deslocamento da consola de série. Estes registos são circulares e apenas indivíduos com permissões de leitura à conta de armazenamento de diagnóstico tem acesso aos mesmos, no entanto, recomendamos que siga a prática recomendada de usar o ambiente de trabalho remoto para tudo o que pode envolver segredos e/ou PII. 

### <a name="concurrent-usage"></a>Utilização em simultâneo
Se um usuário estiver conectado à consola de série e outro utilizador com êxito pedidos de acesso a essa mesma máquina virtual, o primeiro utilizador será desligado e o segundo utilizador ligado de forma semelhante o primeiro utilizador trabalhando em pé e deixar o console físico e um novo utilizador sentamos.

>[!CAUTION] 
Isso significa que o utilizador que for desligado não terminar a sessão! A capacidade de impor um fim de sessão após a desconexão (via SIGHUP ou mecanismo similar) ainda está no plano. Para Windows, há um tempo limite automático ativado no SAC, no entanto, para Linux, pode configurar a definição de tempo limite de terminal. 

## <a name="common-scenarios-for-accessing-serial-console"></a>Cenários comuns para aceder à consola de série 
Cenário          | Ações na consola de série                
:------------------|:-----------------------------------------
Regras de firewall incorreta | Aceda a seriais regras de firewall de Windows de consola e correção. 
Danos/verificação de sistema de ficheiros | Aceder à consola de série e recuperar o sistema de ficheiros. 
Problemas de configuração de RDP | Aceder à consola de série e alterar as definições. Vá para o [documentação de RDP](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access) para começar a utilizar.
Bloqueio de rede para baixo do sistema| Consola de série de acesso através do portal para gerir o sistema. Alguns comandos de rede estão listados na [documentação Serial consola CMD e do PowerShell](serial-console-cmd-ps-commands.md). 
Interagir com o carregador de inicialização | Acesso BCD através da consola de série. Aceda a [menu de inicialização de ativação para mostrar na consola de série do](#enabling-boot-menu-to-show-in-the-serial-console) para começar a utilizar. 

## <a name="accessibility"></a>Acessibilidade
A acessibilidade é um foco principal para a consola de série do Azure. Para esse fim, vamos certificar-se de que a consola de série está acessível para aqueles com visual e auditivas, bem como as pessoas que podem não ser capazes de usar um mouse.

### <a name="keyboard-navigation"></a>Navegação do teclado
Utilize o `tab` chave no teclado para navegar pela interface de consola de série no portal do Azure. Sua localização será realçada na tela. Para deixar o foco do painel de consola de série, prima `Ctrl + F6` no teclado.

### <a name="use-serial-console-with-a-screen-reader"></a>Utilizar a consola de série com um leitor de ecrã
Consola de série é fornecido com suporte de leitor de ecrã incorporado. Navegação com um leitor de ecrã ativado, permitirá que o texto alternativo para o botão selecionado atualmente a ser lido em voz alta pelo leitor de ecrã.

## <a name="errors"></a>Erros
A maioria dos erros são transitórios na natureza e o endereço de ligação de repetir estes. A tabela abaixo mostra uma lista de erros e mitigações

Erro                            |   Mitigação 
:---------------------------------|:--------------------------------------------|
Não é possível obter as definições de diagnóstico de arranque para "<VMNAME>'. Para utilizar a consola de série, certifique-se de que o diagnóstico de arranque está ativado para esta VM. | Certifique-se de que a VM tem [diagnósticos de arranque](boot-diagnostics.md) ativada. 
A VM está num estado parado desalocado. Iniciar a VM e repita a ligação da consola de série. | Máquina virtual deve estar num estado iniciado para aceder à consola de série
Não tem as permissões necessárias para utilizar esta consola de série de VM. Certifique-se de que tem, pelo menos, permissões de função de Contribuidor de VM.| O acesso de consola de série requer determinada permissão para aceder. Ver [os requisitos de acesso](#prerequisites) para obter detalhes
Não é possível determinar o grupo de recursos para a conta de armazenamento do diagnóstico de arranque '<STORAGEACCOUNTNAME>'. Certifique-se de que o diagnóstico de arranque está ativado para esta VM e que tem acesso a esta conta de armazenamento. | O acesso de consola de série requer determinada permissão para aceder. Ver [os requisitos de acesso](#prerequisites) para obter detalhes
Uma resposta de "Proibido" foi encontrada ao aceder à conta de armazenamento do diagnóstico de arranque desta VM. | Certifique-se de que o diagnóstico de arranque não tem uma firewall de conta. Uma conta de armazenamento do diagnóstico de arranque acessível é necessária para a consola de série função.
Web socket foi fechado ou não foi possível abrir. | Poderá ter de lista branca `*.console.azure.com`. Um mais detalhado for mas a abordagem mais é à lista de permissões a [intervalos de IP de Datacenter do Microsoft Azure](https://www.microsoft.com/en-us/download/details.aspx?id=41653), que alterar bastante regularmente.
Apenas as informações de estado de funcionamento são mostradas ao ligar a uma VM do Windows| Isso será apresentada se o Console de administração especial não tenha sido ativado para a sua imagem do Windows. Ver [acesso à consola de série para Windows](#access-serial-console-for-windows) para obter instruções sobre como ativar manualmente SAC na sua VM do Windows. Obter mais detalhes podem ser encontrados em [sinais de estado de funcionamento do Windows](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Windows_Health_Info.md).

## <a name="known-issues"></a>Problemas conhecidos 
Estamos cientes de alguns problemas com a consola de série. Aqui está uma lista desses problemas e os passos para a mitigação.

Problema                             |   Mitigação 
:---------------------------------|:--------------------------------------------|
Acessando introdução após a faixa de ligação não mostra um registo na linha de comandos | Consulte esta página: [Hitting introduza não faz nada](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md). Isto pode acontecer se estiver a executar uma VM personalizada, a aplicação protegida ou uma configuração de arranque que Windows causers não sejam corretamente ligar para a porta serial.
Não é possível digitar em SAC perguntar se a depuração de kernel está ativada | RDP para a VM e execute `bcdedit /debug {current} off` num prompt de comando elevado. Se não for possível RDP em vez disso, pode anexar o disco do SO a outra VM do Azure e modificá-lo enquanto anexado como um disco de dados com `bcdedit /store <drive letter of data disk>:\boot\bcd /debug <identifier> off`, em seguida, trocar o disco voltar.
Colar no PowerShell nos resultados de SAC num caractere de terceiro, se o conteúdo original tinha um caractere de repetição | Uma solução alternativa é passado descarregar o módulo de PSReadLine da sessão atual. Executar `Remove-Module PSReadLine` descarregar o módulo de PSReadLine da sessão atual - isto não irá eliminar ou desinstalar o módulo.
Algumas entradas de teclado produzem estranha SAC saída (por exemplo, `[A`, `[3~`) | [VT100](https://aka.ms/vtsequences) seqüências de escape não são suportadas pela linha de comandos da SAC.
Colar muito longas seqüências de caracteres não funciona | Consola de série limita o comprimento de cadeias de caracteres colado no terminal para 2048 carateres. Isso é para evitar sobrecarregar a largura de banda da porta serial.

## <a name="frequently-asked-questions"></a>Perguntas mais frequentes 

**P. Como posso enviar comentários?**

R. Fornecer comentários como um problema ao aceder https://aka.ms/serialconsolefeedback. Em alternativa menos (preferencial) envie comentários através das azserialhelp@microsoft.com ou na categoria de máquina virtual http://feedback.azure.com

**P. Consola de série suporta copiar/colar?**

R. Sim faz. Utilize Ctrl + Shift + C e Ctrl + Shift + V para copiar e colar no terminal.

**P. Quem pode ativar ou desativar a consola de série para a minha subscrição?**

R. Para ativar ou desativar a consola de série a um nível de toda a subscrição, tem de ter permissões de escrita para a subscrição. Funções com permissão de escrita incluem, mas não sejam limitam às funções de administrador ou proprietário. Funções personalizadas também podem ter permissões de escrita.

**P. Quem pode aceder a consola de série para a minha VM?**

R. Tem de ter acesso de nível de Contribuidor ou superior para uma VM para aceder à consola de série da VM. 

**P. Meu consola de série não está visível qualquer coisa, o que fazer?**

R. A imagem é provavelmente mal configurada para acesso à consola de série. Ver [consola de série ativar em imagens personalizadas ou mais antigas](#enable-serial-console-in-custom-or-older-images) para obter detalhes sobre como configurar a sua imagem para ativar a consola de série.

**P. Consola de série está disponível para conjuntos de dimensionamento de máquinas virtuais?**

R. Neste momento, o acesso à consola de série para instâncias do conjunto de dimensionamento de máquina virtual não é suportado.

## <a name="next-steps"></a>Passos Seguintes
* Para obter um guia detalhado sobre a comandos CMD e do PowerShell, pode utilizar o SAC do Windows, clique em [aqui](serial-console-cmd-ps-commands.md).
* Também está disponível para a consola de série [Linux](serial-console-linux.md) VMs.
* Saiba mais sobre [diagnósticos de arranque](boot-diagnostics.md).