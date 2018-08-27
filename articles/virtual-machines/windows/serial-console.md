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
ms.openlocfilehash: 91c917687edbdfb49fc7a390187a860d9474623a
ms.sourcegitcommit: ebb460ed4f1331feb56052ea84509c2d5e9bd65c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/24/2018
ms.locfileid: "42918929"
---
# <a name="virtual-machine-serial-console-preview"></a>Consola de série de máquina virtual (pré-visualização) 


A consola de série de Máquina Virtual no Azure fornece acesso a um console baseado em texto para máquinas virtuais do Linux e Windows. Essa conexão serial é a porta serial de COM1 da máquina virtual e fornece acesso à máquina virtual e não estão relacionadas à rede da máquina virtual / operativos o estado do sistema. Acesso à consola de série para uma máquina virtual pode ser feito apenas através do portal do Azure atualmente e permitido apenas para os utilizadores que têm Contribuidor de VM ou acima de acesso à máquina virtual. 

Para obter a documentação da consola de série para VMs do Linux [clique aqui](../linux/serial-console.md).

> [!Note] 
> As pré-visualizações são tornadas disponíveis para, a condição de que concorda com os termos de utilização. Para obter mais informações, consulte [Microsoft Azure termos de utilização suplementares para pré-visualizações do Microsoft Azure.] (https://azure.microsoft.com/support/legal/preview-supplemental-terms/) Atualmente este serviço está em **pré-visualização pública** e acesso à consola de série para máquinas virtuais está disponível para regiões globais do Azure. Neste momento a consola de série não está disponível na cloud do Azure Government, Azure Alemanha e China do Azure.

 

## <a name="prerequisites"></a>Pré-requisitos 

* Tem de utilizar o modelo de implementação de gestão de recursos. Implementações clássicas não são suportadas. 
* Máquina virtual tem de ter [diagnósticos de arranque](boot-diagnostics.md) ativada   ![](../media/virtual-machines-serial-console/virtual-machine-serial-console-diagnostics-settings.png)
* A conta a utilizar a consola de série tem de ter [função de contribuinte](../../role-based-access-control/built-in-roles.md) para a VM e o [diagnósticos de arranque](boot-diagnostics.md) conta de armazenamento. 

## <a name="open-the-serial-console"></a>Abra a consola de série
Consola de série para máquinas virtuais só é acessível via [portal do Azure](https://portal.azure.com). Abaixo estão os passos para aceder à consola de série para máquinas virtuais através do portal 

  1. Abra o portal do Azure
  2. No menu da esquerda, selecione as máquinas virtuais.
  3. Clique na VM na lista. Abre a página de descrição geral para a VM.
  4. Desloque para baixo para o suporte + resolução de problemas de seção e clique na opção de consola de série (pré-visualização). Um novo painel com a consola de série abrirá e iniciar a ligação.

![](../media/virtual-machines-serial-console/virtual-machine-windows-serial-console-connect.gif)

## <a name="access-serial-console-for-windows"></a>Acesso à consola de série para Windows 
Imagens do Windows Server mais recente no Azure terá [consola administrativa especial](https://technet.microsoft.com/library/cc787940(v=ws.10).aspx) (SAC) ativada por predefinição. SAC é suportada em versões de servidor do Windows, mas não está disponível nas versões de cliente (por exemplo, Windows 10, Windows 8 ou Windows 7). Para ativar a consola de série para máquinas de virtuais do Windows criada com o uso Feb2018 ou imagens menores utilizam os seguintes passos: 

1. Ligar à máquina virtual Windows através de ambiente de trabalho remoto
2. A partir de uma linha de comandos administrativa, execute os seguintes comandos 
* `bcdedit /ems {current} on`
* `bcdedit /emssettings EMSPORT:1 EMSBAUDRATE:115200`
3. Reinicie o sistema para a consola de SAC seja ativado

![](../media/virtual-machines-serial-console/virtual-machine-windows-serial-console-connect.gif)

Se necessário SAC pode ser ativado offline 

1. Anexe o disco do windows que pretende SAC configurado para como um disco de dados para uma VM existente. 
2. A partir de uma linha de comandos administrativa, execute os seguintes comandos 
* `bcdedit /store <mountedvolume>\boot\bcd /ems {default} on`
* `bcdedit /store <mountedvolume>\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200`

### <a name="how-do-i-know-if-sac-is-enabled-or-not"></a>Como posso saber se SAC está ativado ou não 

Se [SAC] (https://technet.microsoft.com/library/cc787940(v=ws.10).aspx) não está ativada a consola de série não serão apresentados na linha de comandos da SAC. Ele pode mostrar uma informações de estado de funcionamento da VM em alguns casos, ou poderia estar em branco.  

### <a name="enabling-boot-menu-to-show-in-the-serial-console"></a>Ativar menu de inicialização Mostrar na consola de série 

Se precisar de ativar o carregador de inicialização do Windows pede-lhe para mostrar na consola de série, pode adicionar as seguintes opções adicionais para o carregador de inicialização do Windows.

1. Ligar à máquina virtual Windows através de ambiente de trabalho remoto
2. A partir de uma linha de comandos administrativa, execute os seguintes comandos 
* `bcdedit /set {bootmgr} displaybootmenu yes`
* `bcdedit /set {bootmgr} timeout 5`
* `bcdedit /set {bootmgr} bootems yes`
3. Reinicie o sistema para o menu de arranque seja ativado

> [!NOTE] 
> No suporte neste ponto para a função de chaves não está ativada, se necessitar de opções de inicialização avançadas utilizam bcdedit /set {atual} onetimeadvancedoptions no, consulte [bcdedit](https://docs.microsoft.com/windows-hardware/drivers/devtest/bcdedit--set) para obter mais detalhes

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
Embora sem palavras-passe de acesso para a consola são registadas, se os comandos são executados dentro da consola contém ou palavras-passe, segredos, os nomes de utilizador ou qualquer outra forma de informação identificativa (PII) de saída, aqueles serão escritos para o diagnóstico de arranque de máquina virtual funcionalidade de cópia de registos, juntamente com todos os outro texto visível, como parte da implementação de deslocamento da consola de série. Estes registos são circulares e apenas indivíduos com permissões de leitura à conta de armazenamento de diagnóstico tem acesso aos mesmos, no entanto, recomendamos que siga a prática recomendada de usar o ambiente de trabalho remoto para tudo o que pode envolver segredos e/ou PII. 

### <a name="concurrent-usage"></a>Utilização em simultâneo
Se um usuário estiver conectado à consola de série e outro utilizador com êxito pedidos de acesso a essa mesma máquina virtual, o primeiro utilizador será desligado e o segundo utilizador ligado de forma semelhante o primeiro utilizador trabalhando em pé e deixar o console físico e um novo utilizador sentamos.

>[!CAUTION] 
Isso significa que o utilizador que for desligado não terminar a sessão! A capacidade de impor um fim de sessão após a desconexão (via SIGHUP ou mecanismo similar) ainda está no plano. Para Windows, há um tempo limite automático ativado no SAC, no entanto, para Linux, pode configurar a definição de tempo limite de terminal. 

## <a name="using-serial-console-for-nmi-calls-in-windows-vms"></a>Utilizar a consola de série para NMI chama em VMs do Windows
Uma interrupção não maskable (NMI) foi concebida para criar um sinal de que o software numa máquina virtual não será ignorada. Historicamente, NMIs foram utilizadas para monitorizar a existência de problemas de hardware em sistemas que exigem tempos de resposta específica.  Hoje, os programadores e administradores de sistema utilizam frequentemente NMI como um mecanismo para depurar ou resolver problemas de sistemas que estão a ser suspenso.

A consola de série pode ser utilizada para enviar um NMI para uma máquina virtual do Azure com o ícone de teclado na barra de comando mostrada abaixo. Depois do NMI é enviado, a configuração de máquina virtual irá controlar como o sistema responder. Windows podem ser configurado para falhas e criar um despejo de memória, ao receber um NMI.

![](../media/virtual-machines-serial-console/virtual-machine-windows-serial-console-nmi.png) <br>

Para obter informações sobre como configurar o Windows para criar um despejo de falha quando recebe um NMI, consulte: [como gerar um arquivo de despejo de falha completa ou de um arquivo de despejo de falhas de kernel ao utilizar um NMI num sistema baseado em Windows](https://support.microsoft.com/en-us/help/927069/how-to-generate-a-complete-crash-dump-file-or-a-kernel-crash-dump-file)


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

## <a name="known-issues"></a>Problemas conhecidos 
Como estamos ainda nas fases de pré-visualização para acesso à consola de série, vamos trabalhar através de alguns problemas conhecidos, segue-se a lista destas com soluções alternativas possíveis 

Problema                             |   Mitigação 
:---------------------------------|:--------------------------------------------|
Não existe nenhuma opção com a consola de instância de conjunto de dimensionamento do virtual machine serial | Durante a pré-visualização, o acesso à consola de série para instâncias do conjunto de dimensionamento de máquina virtual não é suportado.
Acessando introdução após a faixa de ligação não mostra um registo na linha de comandos | Consulte esta página: [Hitting introduza não faz nada](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md). Isto pode acontecer se estiver a executar uma VM personalizada, alta da aplicação ou configuração de GRUB esse Windows causers não sejam corretamente ligar para a porta serial.
Apenas as informações de estado de funcionamento são mostradas ao ligar a uma VM do Windows| Isso será apresentada se o Console de administração especial não tenha sido ativado para a sua imagem do Windows. Ver [acesso à consola de série para Windows](#access-serial-console-for-windows) para obter instruções sobre como ativar manualmente SAC na sua VM do Windows. Obter mais detalhes podem ser encontrados em [sinais de estado de funcionamento do Windows](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Windows_Health_Info.md).
Não é possível digitar em SAC perguntar se a depuração de kernel está ativada | RDP para a VM e execute `bcdedit /debug {current} off` num prompt de comando elevado. Se não for possível RDP em vez disso, pode anexar o disco do SO a outra VM do Azure e modificá-lo enquanto anexado como um disco de dados com `bcdedit /store <drive letter of data disk>:\boot\bcd /debug <identifier> off`, em seguida, trocar o disco voltar.
Colar no PowerShell nos resultados de SAC num caractere de terceiro, se o conteúdo original tinha um caractere de repetição | É uma solução alternativa remover o módulo de PSReadLine. `Remove-Module PSReadLine` irá remover o módulo de PSReadLine da sessão atual.
Algumas entradas de teclado produzem estranha SAC saída (por exemplo, `[A`, `[3~`) | [VT100](https://aka.ms/vtsequences) seqüências de escape não são suportadas pela linha de comandos da SAC.
Uma resposta de "Proibido" foi encontrada ao aceder à conta de armazenamento do diagnóstico de arranque desta VM. | Certifique-se de que o diagnóstico de arranque não tem uma firewall de conta. Uma conta de armazenamento do diagnóstico de arranque acessível é necessária para a consola de série função.

## <a name="frequently-asked-questions"></a>Perguntas mais frequentes 
**P. Como posso enviar comentários?**

A. Fornecer comentários como um problema ao aceder https://aka.ms/serialconsolefeedback. Em alternativa menos (preferencial) envie comentários através das azserialhelp@microsoft.com ou na categoria de máquina virtual http://feedback.azure.com

**P. Não consigo aceder à consola de série, onde pode enviar um incidente de suporte?**

A. Esta funcionalidade de pré-visualização é abrangida por meio de termos de pré-visualização do Azure. Suporte para isso é melhor processado por meio de canais mencionadas acima. 

## <a name="next-steps"></a>Passos Seguintes
* Para obter um guia detalhado sobre a comandos CMD e do PowerShell, pode utilizar o SAC do Windows, clique em [aqui](serial-console-cmd-ps-commands.md).
* Também está disponível para a consola de série [Linux](../linux/serial-console.md) VMs.
* Saiba mais sobre [diagnósticos de arranque](boot-diagnostics.md).
