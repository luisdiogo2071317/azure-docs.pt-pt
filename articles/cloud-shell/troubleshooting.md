---
title: Resolução de problemas do Cloud Shell do Azure | Documentos da Microsoft
description: Resolução de problemas do Azure Cloud Shell
services: azure
documentationcenter: ''
author: maertendMSFT
manager: angelc
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/03/2018
ms.author: damaerte
ms.openlocfilehash: 21bc0633a9cc607325b48998791cb12631ecd0d7
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37856492"
---
# <a name="troubleshooting--limitations-of-azure-cloud-shell"></a>Resolução de problemas & limitações do Azure Cloud Shell

Resoluções conhecidas para a resolução de problemas no Azure Cloud Shell incluem:

## <a name="general-troubleshooting"></a>Resolução de problemas

### <a name="early-timeouts-in-firefox"></a>Tempos limite inicial no FireFox
- **Detalhes**: Cloud Shell utiliza um websocket aberto para passar a entrada/saída ao seu navegador. FireFox tem políticas predefinidas que podem fechar o websocket prematuramente esgotando antecipada no Cloud Shell.
- **Resolução**: FireFox aberto e navegue para "sobre: config" na caixa URL. Procure "network.websocket.timeout.ping.request" e altere o valor de 0 a 10.

### <a name="storage-dialog---error-403-requestdisallowedbypolicy"></a>Diálogo de armazenamento - erro: RequestDisallowedByPolicy 403
- **Detalhes**: ao criar uma conta de armazenamento através do Cloud Shell, é sem êxito devido a uma política do Azure colocada pelo seu administrador. Mensagem de erro irá incluir: `The resource action 'Microsoft.Storage/storageAccounts/write' is disallowed by one or more policies.`
- **Resolução**: contacte o administrador do Azure para remover ou atualizar a política do Azure negar a criação de armazenamento.

### <a name="storage-dialog---error-400-disallowedoperation"></a>Diálogo de armazenamento - erro: DisallowedOperation 400
 - **Detalhes**: ao utilizar uma subscrição do Azure Active Directory, não é possível criar o armazenamento.
 - **Resolução**: utilizar uma subscrição do Azure com capacidade de criação de recursos de armazenamento. Subscrições do Azure AD não são possível criar recursos do Azure.

### <a name="terminal-output---error-failed-to-connect-terminal-websocket-cannot-be-established-press-enter-to-reconnect"></a>Terminal de saída - erro: Falha ao ligar o terminal: não é possível estabelecer o websocket. Prima `Enter` restabelecer a ligação.
 - **Detalhes**: Cloud Shell requer a capacidade de estabelecer uma conexão websocket à infraestrutura de Cloud Shell.
 - **Resolução**: Verifique que configurou as definições de rede para permitir o envio de pedidos de https e pedidos de websocket para domínios em *. console.azure.com.

## <a name="bash-troubleshooting"></a>Resolução de problemas de bash

### <a name="cannot-run-the-docker-daemon"></a>Não é possível executar o daemon do docker

- **Detalhes**: Cloud Shell utiliza um contentor para alojar o seu ambiente de shell, assim que executa o daemon não é permitida.
- **Resolução**: Utilize [docker-machine](https://docs.docker.com/machine/overview/), que é instalada por predefinição, para gerir os contentores do docker a partir de um anfitrião do Docker remoto.

## <a name="powershell-troubleshooting"></a>Resolução de problemas do PowerShell

### <a name="gui-applications-are-not-supported"></a>Aplicativos de GUI não são suportados

- **Detalhes**: se um usuário inicia um aplicativo de GUI, o pedido não devolve. Por exemplo, quando um utilizador clona um repositório privado do GitHub que é a autenticação de dois fatores ativada, é apresentada uma caixa de diálogo para concluir a autenticação de dois fatores.  
- **Resolução**: Feche e reabra o shell.

### <a name="get-help--online-does-not-open-the-help-page"></a>Get-Help - online não abrir a página de ajuda

- **Detalhes**: se um usuário digita `Get-Help Find-Module -online`, um vê como uma mensagem de erro: `Starting a browser to display online Help failed. No program or browser is associated to open the URI http://go.microsoft.com/fwlink/?LinkID=398574.`
- **Resolução**: Copie o url e abri-lo manualmente no seu navegador.

### <a name="troubleshooting-remote-management-of-azure-vms"></a>Resolução de problemas de gestão remota de VMs do Azure

- **Detalhes**: devido às predefinições de Firewall do Windows para o WinRM, o utilizador pode ver o erro seguinte: `Ensure the WinRM service is running. Remote Desktop into the VM for the first time and ensure it can be discovered.`
- **Resolução**: execute `Enable-AzureRmVMPSRemoting` para permitir que todos os aspetos da comunicação remota do PowerShell no computador de destino.
 

### <a name="dir-caches-the-result-in-azure-drive"></a>`dir` coloca em cache o resultado na unidade do Azure

- **Detalhes**: O resultado da `dir` é colocado em cache na unidade do Azure.
- **Resolução**: depois de criar ou remover um recurso na vista de unidade do Azure, executar `dir -force` para atualizar.

## <a name="general-limitations"></a>Limitações gerais
O Azure Cloud Shell tem as seguintes limitações conhecidas:

### <a name="system-state-and-persistence"></a>Estado do sistema e persistência

A máquina que fornece a sua sessão do Cloud Shell é temporária e ele é reciclado, depois da sessão está inativa durante 20 minutos. Cloud Shell requer uma partilha de ficheiros do Azure de ser montado. Como resultado, a sua subscrição tem de ser capaz de configurar recursos de armazenamento para aceder ao Cloud Shell. Outras considerações incluem:

* Com o armazenamento montado, apenas modificações do `clouddrive` diretório são mantidas. No Bash, sua `$Home` diretório também é mantido.
* Partilhas de ficheiros do Azure podem ser montadas apenas a partir do seu [atribuídos região](persisting-shell-storage.md#mount-a-new-clouddrive).
  * No Bash, execute `env` para encontrar a sua região definida como `ACC_LOCATION`.
* Ficheiros do Azure suportam o armazenamento apenas localmente redundante e contas de armazenamento georredundante.

### <a name="browser-support"></a>Suporte do browser

Cloud Shell suporta as versões mais recentes do Microsoft Edge, o Microsoft Internet Explorer, o Google Chrome, o Mozilla Firefox e o Apple Safari. Não é suportado o Safari no modo privado.

### <a name="copy-and-paste"></a>Copiar e colar

[!include [copy-paste](../../includes/cloud-shell-copy-paste.md)]

### <a name="for-a-given-user-only-one-shell-can-be-active"></a>Para um determinado usuário, apenas um shell pode estar ativa

Os utilizadores podem apenas iniciar um tipo de shell por vez, optar por **Bash** ou **PowerShell**. No entanto, pode ter várias instâncias de Bash ou do PowerShell em execução em simultâneo. A troca entre o Bash ou o PowerShell causas Cloud Shell para reiniciar, que termina sessões existentes.

### <a name="usage-limits"></a>Limites de utilização

Cloud Shell destina-se para casos de uso interativo. Como resultado, qualquer sessões de não-interativa de execução longa são terminadas sem aviso.

## <a name="bash-limitations"></a>Limitações de bash

### <a name="user-permissions"></a>Permissões de utilizador

As permissões estão definidas como usuários regulares sem acesso de sudo. Qualquer instalação fora de sua `$Home` diretório não é persistente.

### <a name="editing-bashrc"></a>Editar .bashrc

Tenha cuidado de edição .bashrc, isso pode causar erros inesperados no Cloud Shell.

## <a name="powershell-limitations"></a>Limitações do PowerShell

### <a name="azuread-module-name"></a>`AzureAD` nome do módulo

O `AzureAD` nome do módulo está atualmente `AzureAD.Standard.Preview`, o módulo fornece a mesma funcionalidade.

### <a name="sqlserver-module-functionality"></a>`SqlServer` funcionalidade do módulo

O `SqlServer` módulo incluído no Cloud Shell tem suporte apenas de pré-lançamento para o PowerShell Core. Em particular, `Invoke-SqlCmd` ainda não está disponível.

### <a name="default-file-location-when-created-from-azure-drive"></a>Localização do ficheiro predefinido quando criado a partir do disco do Azure:

Utilizar cmdlets do PowerShell, os utilizadores não é podem criar arquivos sob a unidade do Azure. Quando os utilizadores criam novos ficheiros com outras ferramentas, como vim ou nano, os ficheiros são guardados para a `$HOME` por predefinição. 

### <a name="gui-applications-are-not-supported"></a>Aplicativos de GUI não são suportados

Se o usuário executa um comando que seria criar uma caixa de diálogo do Windows, tal como `Connect-AzureAD` ou `Connect-AzureRmAccount`, um vê como uma mensagem de erro: `Unable to load DLL 'IEFRAME.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)`.

### <a name="tab-completion-crashes-psreadline"></a>Conclusão de tabulação falha PSReadline

Se EditMode o utilizador no PSReadline estiver definido como Emacs, o utilizador tenta exibir todas as possibilidades por meio de conclusão de tabulação e o tamanho da janela é demasiado pequeno para exibir todas as possibilidades, PSReadline falhará.

### <a name="large-gap-after-displaying-progress-bar"></a>Grande lacuna depois de exibir a barra de progresso

Se o utilizador efetua uma ação que exibe uma barra, tal um separador de progresso conclusão ao mesmo tempo no `Azure:` unidade, em seguida, é possível que o cursor não está corretamente definido e uma lacuna aparece em que a barra de progresso era anteriormente.

### <a name="random-characters-appear-inline"></a>Caracteres aleatórios são apresentadas inline

A sequência de posição do cursor códigos, por exemplo `5;13R`, pode aparecer na entrada do usuário.  Os caracteres podem ser removidos manualmente.

## <a name="personal-data-in-cloud-shell"></a>Dados pessoais no Cloud Shell

O Azure Cloud Shell leva os seus dados pessoais sério, os dados capturados e armazenados pelo serviço do Azure Cloud Shell são utilizados para fornecer predefinições para a sua experiência, como o shell que utilizou mais recentemente, tamanho da fonte preferencial, o tipo de letra preferencial e o arquivo partilharem detalhes que volta a nuvem unidade. Caso queira exportar ou eliminar esses dados, incluímos as seguintes instruções.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

### <a name="export"></a>Exportar
Para **exportar** as definições de utilizador, o Cloud Shell poupa-se para como preferencial shell, o tamanho da fonte e o tipo de letra, execute os seguintes comandos.

1. Inicie o Bash no Cloud Shell
2. Execute os seguintes comandos:
```
user@Azure:~$ token="Bearer $(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s | jq -r ".access_token")"
user@Azure:~$ curl https://management.azure.com/providers/Microsoft.Portal/usersettings/cloudconsole?api-version=2017-12-01-preview -H Authorization:"$token" -s | jq
```

### <a name="delete"></a>Eliminar
Para **eliminar** as definições de utilizador, o Cloud Shell poupa-se para como preferencial shell, o tamanho da fonte e o tipo de letra, execute os seguintes comandos. Da próxima vez que inicia o Cloud Shell será solicitado para carregar uma partilha de ficheiros novamente. 

Ficheiros do Azure real partilha não será eliminada se eliminar as definições de utilizador, aceda a ficheiros do Azure para concluir essa ação.

1. Inicie o Bash no Cloud Shell
2. Execute os seguintes comandos:
```
user@Azure:~$ token="Bearer $(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s | jq -r ".access_token")"
user@Azure:~$ curl -X DELETE https://management.azure.com/providers/Microsoft.Portal/usersettings/cloudconsole?api-version=2017-12-01-preview -H Authorization:"$token"
```
