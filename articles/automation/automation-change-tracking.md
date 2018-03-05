---
title: "Controlar as alterações com a automatização do Azure | Microsoft Docs"
description: "A solução de controlo de alterações ajuda-o a identificar o software e as alterações de serviço do Windows que ocorrem no seu ambiente."
services: automation
documentationcenter: 
author: georgewallace
manager: carmonm
editor: 
ms.assetid: f8040d5d-3c89-4f0c-8520-751c00251cb7
ms.service: automation
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2018
ms.author: gwallace
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 79c5f354c3e63856474e46e2b6928af829604e15
ms.sourcegitcommit: 83ea7c4e12fc47b83978a1e9391f8bb808b41f97
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/28/2018
---
# <a name="track-software-changes-in-your-environment-with-the-change-tracking-solution"></a>Controlar as alterações de software no seu ambiente com a solução de controlo de alterações

Este artigo ajuda-o a utilizar a solução de controlo de alterações para identificar facilmente as alterações no seu ambiente. A solução controla as alterações ao software do Windows e Linux, os ficheiros do Windows e Linux, chaves de registo do Windows, serviços do Windows e Linux daemons. Identificar as alterações de configuração pode ajudar a identificar problemas operacionais.

As alterações ao software instalado, os serviços do Windows, registo do Windows e de ficheiros e aplicações daemons do Linux em servidores monitorizados são enviadas para o serviço de análise de registos na nuvem para processamento. Lógica é aplicada para os dados recebidos e o serviço em nuvem regista os dados. Ao utilizar as informações no dashboard do controlo de alterações, pode facilmente ver as alterações efetuadas na sua infraestrutura de servidor.

## <a name="enable-change-tracking-and-inventory"></a>Ativar o registo de alterações e inventário


Para começar a controlar as alterações, tem de ativar a solução de controlo de alterações e de inventário para a sua conta de automatização.

1. No portal do Azure, navegue até à sua conta de automatização
1. Selecione **alterações** em **configuração**.
2. Selecione uma área de trabalho de análise do registo existente ou **criar nova área de trabalho** e clique em **ativar**.

Isto permite que a solução para a sua conta de automatização. A solução pode demorar até 15 minutos a ativar. A faixa azul notifica-o quando a solução está ativada. Navegue de volta para o **alterações** página para gerir a solução.

## <a name="configuring-change-tracking-and-inventory"></a>Configurar o registo de alterações e inventário

Para saber como para os computadores integrar a solução visite: [soluções de integração automatização](automation-onboard-solutions-from-automation-account.md). Quando ativar um novo ficheiro ou a chave de registo para acompanhar, está ativado para registo de alterações e inventário.

### <a name="configure-linux-files-to-track"></a>Configurar ficheiros de Linux para controlar

Utilize os seguintes passos para configurar o controlo de ficheiro em computadores Linux:

1. Na sua conta de automatização, selecione **alterações** em **gestão de configuração**. Clique em **editar definições de** (symbol engrenagem).
2. No **alterações** página, selecione **Linux ficheiros**, em seguida, clique em **+ adicionar** para adicionar um novo ficheiro para controlar.
3. No **adicionar ficheiros de Linux para o controlo de alterações**, introduza as informações para o ficheiro ou diretório para controlar e clique em **guardar**.

|Propriedade  |Descrição  |
|---------|---------|
|Ativado     | Determina se a definição é aplicada.        |
|Nome do item     | Nome amigável do ficheiro a ser monitorizados.        |
|Grupo     | Um nome de grupo para agrupar logicamente os ficheiros.        |
|Introduza o caminho     | O caminho para verificar se o ficheiro. Por exemplo: "/etc/*.conf"       |
|Tipo de caminho     | Tipo de item para ser controlados valores possíveis são de ficheiros e diretórios.        |
|Recursão     | Determina se recursão é utilizada durante a pesquisa para o item ser monitorizados.        |
|Utilize o Sudo     | Esta definição determina se o sudo é utilizado quando a verificação do item.         |
|Ligações     | Esta definição determina como simbólicas resolvidas quando atravessar diretórios.<br> **Ignorar** - ignora as ligações simbólicas e não inclui os ficheiros/diretórios referenciados.<br>**Siga** - segue-se as ligações simbólicas durante recursão e também inclui os ficheiros/diretórios referenciados.<br>**Gerir** - segue-se as ligações simbólicas e permite a alteração de conteúdo devolvido.     |

> [!NOTE]
> A opção de ligações de "Gerir" não é recomendada. Obtenção de conteúdo do ficheiro não é suportada.

### <a name="configure-windows-files-to-track"></a>Configurar ficheiros do Windows para controlar

Utilize os seguintes passos para configurar ficheiros de registo em computadores Windows:

1. Na sua conta de automatização, selecione **alterações** em **gestão de configuração**. Clique em **editar definições de** (symbol engrenagem).
2. No **alterações** página, selecione **ficheiros do Windows**, em seguida, clique em **+ adicionar** para adicionar um novo ficheiro para controlar.
3. No **adicionar ficheiros do Windows para controlo de alterações**, introduza as informações para o ficheiro controlar e clique em **guardar**.

|Propriedade  |Descrição  |
|---------|---------|
|Ativado     | Determina se a definição é aplicada.        |
|Nome do item     | Nome amigável do ficheiro a ser monitorizados.        |
|Grupo     | Um nome de grupo para agrupar logicamente os ficheiros.        |
|Introduza o caminho     | O caminho para verificar o ficheiro por exemplo: "c:\temp\myfile.txt"       |

### <a name="configure-windows-registry-keys-to-track"></a>Configurar as chaves de registo do Windows para controlar

Utilize os seguintes passos para configurar o controlo de chave de registo em computadores Windows:

1. Na sua conta de automatização, selecione **alterações** em **gestão de configuração**. Clique em **editar definições de** (symbol engrenagem).
2. No **alterações** página, selecione **registo do Windows**, em seguida, clique em **+ adicionar** para adicionar uma nova chave de registo para acompanhar.
3. No **adicionar registo do Windows para controlo de alterações**, introduza as informações da chave controlar e clique em **guardar**.

|Propriedade  |Descrição  |
|---------|---------|
|Ativado     | Determina se a definição é aplicada.        |
|Nome do item     | Nome amigável do ficheiro a ser monitorizados.        |
|Grupo     | Um nome de grupo para agrupar logicamente os ficheiros.        |
|Chave de registo do Windows   | O caminho para verificar se o ficheiro. Por exemplo: "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders\Common arranque"      |

## <a name="limitations"></a>Limitações

A solução de controlo de alterações não suporta atualmente os seguintes itens:

* Pastas (diretórios) para o ficheiro de controlo do Windows
* Recursão para o ficheiro de controlo do Windows
* Os carateres universais para o ficheiro de controlo do Windows
* Variáveis de caminho
* Sistemas de ficheiros de rede
* Conteúdo do ficheiro

Outras limitações:

* O **tamanho de ficheiro máximo** e valores de coluna são não utilizados na implementação atual.
* Se recolher ficheiros mais de 2500 no ciclo de coleção de 30 minutos, o desempenho da solução pode ser degradado.
* Quando o tráfego de rede é elevado, registos de alteração poderão demorar até seis horas para apresentar.
* Se modificar a configuração enquanto um computador é encerrado, o computador poderá publicar as alterações que pertenciam a configuração anterior.

## <a name="known-issues"></a>Problemas Conhecidos

A solução de controlo de alterações atualmente estão a ocorrer os seguintes problemas:
* Atualizações de correção não são recolhidas para Windows 10 criadores Update e no Windows Server 2016 Core RS3 máquinas.

## <a name="change-tracking-data-collection-details"></a>Alterar os detalhes de recolha de dados de controlo

A tabela seguinte mostra a frequência de recolha de dados para os tipos de alterações. Para cada tipo, o instantâneo dos dados do estado atual também é atualizado, pelo menos, a cada 24 horas:

| **Tipo de alteração** | **Frequência** |
| --- | --- |
| Registo do Windows | minutos de 50 | 
| Ficheiro do Windows | 30 minutos | 
| Ficheiro do Linux | 15 minutos | 
| Serviços Windows | 30 minutos | 
| Aplicações daemons do Linux | 5 minutos |
| Software do Windows | 30 minutos | 
| Software do Linux | 5 minutos | 

### <a name="registry-key-change-tracking"></a>Alteração de chave de registo de controlo

O objetivo de monitorização de alterações a chaves de registo é identificar pontos de extensibilidade onde podem ativar o código de terceiros e de software maligno. A lista seguinte mostra a lista de chaves de registo previamente configurada. Estas chaves são configuradas, mas não ativadas. Para monitorizar estas chaves de registo, tem de ativar cada um deles.

> [!div class="mx-tdBreakAll"]
> |  |
> |---------|
> |**HKEY\_LOCAL\_MACHINE\Software\Classes\Directory\ShellEx\ContextMenuHandlers**     |
|&nbsp;&nbsp;&nbsp;&nbsp;Monitores comuns autostart entradas que ligue diretamente no Explorador do Windows e, normalmente, execução no processo com Explorer.exe.    |
> |**HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Startup**     |
|&nbsp;&nbsp;&nbsp;&nbsp;Scripts de monitores que são executados no arranque.     |
> |**HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Shutdown**    |
|&nbsp;&nbsp;&nbsp;&nbsp;Scripts de monitores que são executados no encerramento.     |
> |**HKEY\_LOCAL\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Run**     |
|&nbsp;&nbsp;&nbsp;&nbsp;Monitoriza as chaves que são carregadas antes do utilizador iniciar a sessão na respetiva conta do Windows. A chave é utilizada para programas de 32 bits em execução nos computadores de 64 bits.    |
> |**HKEY\_LOCAL\_MACHINE\SOFTWARE\Microsoft\Active Setup\Installed componentes**     |
|&nbsp;&nbsp;&nbsp;&nbsp;Monitores alterações das definições de aplicação.     |
> |**HKEY\_LOCAL\_MACHINE\Software\Classes\Directory\ShellEx\ContextMenuHandlers**|
|&nbsp;&nbsp;&nbsp;&nbsp;Monitores comuns autostart entradas que ligue diretamente no Explorador do Windows e, normalmente, execução no processo com Explorer.exe.|
> |**HKEY\_LOCAL\_MACHINE\Software\Classes\Directory\Shellex\CopyHookHandlers**|
|&nbsp;&nbsp;&nbsp;&nbsp;Monitores comuns autostart entradas que ligue diretamente no Explorador do Windows e, normalmente, execução no processo com Explorer.exe.|
> |**HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers**|
|&nbsp;&nbsp;&nbsp;&nbsp;Monitores para o ícone de sobreposição de registo do processador.|
|**HKEY\_LOCAL\_MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers**|
|&nbsp;&nbsp;&nbsp;&nbsp;Monitores para o ícone de sobreposição de registo de processador para programas de 32 bits em execução nos computadores de 64 bits.|
> |**HKEY\_LOCAL\_objetos de programa auxiliar de MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\Browser**|
|&nbsp;&nbsp;&nbsp;&nbsp;Monitores para o novo browser auxiliar objeto plug-ins para o Internet Explorer. Utilizado para aceder a documentos Object Model (DOM) da página atual e para controlar a navegação.|
> |**HKEY\_LOCAL\_objetos de programa auxiliar de MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\Browser**|
|&nbsp;&nbsp;&nbsp;&nbsp;Monitores para o novo browser auxiliar objeto plug-ins para o Internet Explorer. Utilizado para aceder a documentos Object Model (DOM) da página atual e para controlar a navegação para programas de 32 bits em execução nos computadores de 64 bits.|
> |**HKEY\_LOCAL\_MACHINE\Software\Microsoft\Internet Explorer\Extensions**|
|&nbsp;&nbsp;&nbsp;&nbsp;Monitores para novas extensões de Internet Explorer, tais como menus de ferramenta personalizada e botões da barra de ferramentas personalizadas.|
> |**HKEY\_LOCAL\_MACHINE\Software\Wow6432Node\Microsoft\Internet Explorer\Extensions**|
|&nbsp;&nbsp;&nbsp;&nbsp;Monitores para novas extensões de Internet Explorer, tais como menus de ferramenta personalizada e botões da barra de ferramentas personalizadas para os programas de 32 bits em execução nos computadores de 64 bits.|
> |**HKEY\_LOCAL\_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Drivers32**|
|&nbsp;&nbsp;&nbsp;&nbsp;Monitoriza os controladores de 32 bits associados wavemapper, wave1 e wave2, msacm.imaadpcm, .msadpcm, .msgsm610 e vidc. É semelhante à secção [controladores] no sistema. Ficheiro INI.|
> |**HKEY\_LOCAL\_MACHINE\Software\Wow6432Node\Microsoft\Windows NT\CurrentVersion\Drivers32**|
|&nbsp;&nbsp;&nbsp;&nbsp;Monitores para os controladores de 32 bits associados wavemapper, wave1 e wave2, msacm.imaadpcm, .msadpcm, .msgsm610 e vidc para programas de 32 bits em execução nos computadores de 64 bits. É semelhante à secção [controladores] no sistema. Ficheiro INI.|
> |**HKEY\_LOCAL\_MACHINE\System\CurrentControlSet\Control\Session Manager\KnownDlls**|
|&nbsp;&nbsp;&nbsp;&nbsp;Monitoriza a lista de sistema conhecido ou frequentemente utilizada DLLs; Este sistema impede que pessoas explorá permissões do diretório de aplicação fracos, arrastando em versões de Trojan horse de DLLs do sistema.|
> |**HKEY\_LOCAL\_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\Notify**|
|&nbsp;&nbsp;&nbsp;&nbsp;Monitoriza a lista de pacotes capazes de receber notificações de eventos do Winlogon, o modelo de suporte de início de sessão interativo para o sistema operativo Windows.|

## <a name="use-change-tracking"></a>Utilizar o registo de alterações

Depois da solução estiver ativada, pode ver o resumo de alterações para os seus computadores monitorizados, selecionando **alterações** em **gestão de configuração** na sua conta de automatização.

Pode ver as alterações aos seus computadores e, em seguida, desagregação em detalhes para cada evento. Largar pendentes estão disponíveis na parte superior do gráfico para limitar o gráfico e informações detalhadas com base nos intervalos de tipo e a hora de alteração. Também pode clique e arraste o gráfico para selecionar um intervalo de tempo personalizado.

![imagem do dashboard do controlo de alterações](./media/automation-change-tracking/change-tracking-dash01.png)

Clicar de alterar ou eventos apresenta as informações detalhadas sobre essa alteração. Como pode ver do exemplo, o tipo de arranque do serviço foi alterado de Manual como Auto.

![imagem de detalhes de registo de alterações](./media/automation-change-tracking/change-tracking-details.png)

## <a name="search-logs"></a>Registos de pesquisa

Além dos detalhes que são fornecidos no portal, as pesquisas podem ser feitas contra os registos. Com o **alterações** clique aberto, da página **Log Analytics**, esta ação abre o **pesquisa de registo** página.

### <a name="sample-queries"></a>Consultas de exemplo

A tabela seguinte fornece pesquisas de registo de exemplo para alterar registos recolhidos por esta solução:

|Consulta  |Descrição  |
|---------|---------|
|ConfigurationData<br>&#124; where   ConfigDataType == "WindowsServices" and SvcStartupType == "Auto"<br>&#124; where SvcState == "Stopped"<br>&#124; resumir arg_max(TimeGenerated, *) por SoftwareName, o computador         | Mostra os registos de inventário mais recentes dos serviços do Windows que foi definido como Auto, mas foram reportados como a ser parado<br>Os resultados estão limitados para o registo mais recente do que SoftwareName e de computador      |
|ConfigurationChange<br>&#124; where ConfigChangeType == "Software" and ChangeCategory == "Removed"<br>&#124; ordem pelo TimeGenerated desc|Mostra os registos de alteração de software removido|

## <a name="next-steps"></a>Passos Seguintes

Visite o tutorial num registo de alterações para obter mais informações sobre como utilizar a solução:

> [!div class="nextstepaction"]
> [Resolver problemas de alterações no seu ambiente](automation-tutorial-troubleshoot-changes.md)

* Utilize [pesquisas de registo na análise de registos](../log-analytics/log-analytics-log-searches.md) para ver dados de registo de alterações de detalhado.
