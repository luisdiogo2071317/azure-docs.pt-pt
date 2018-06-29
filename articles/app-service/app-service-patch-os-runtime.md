---
title: SO e aplicação de patches de tempo de execução no App Service do Azure | Microsoft Docs
description: Descreve como o App Service do Azure atualizações do SO e tempos de execução, e como pode obter atualizar anúncios.
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/02/2018
ms.author: cephalin
ms.openlocfilehash: 0626b958a9b822569f4d3b6d27f3395bed853174
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37030058"
---
# <a name="os-and-runtime-patching-in-azure-app-service"></a>SO e aplicação de patches de tempo de execução no App Service do Azure

Este artigo mostra como obter determinadas informações de versão sobre o SO ou de software no [do serviço de aplicações](app-service-web-overview.md). 

Serviço de aplicações é um plataforma-como-um-serviço, que significa que o SO e pilha de aplicação são geridos pelo Azure; Gerir apenas a aplicação e os respetivos dados. Mais controlo sobre o SO e a aplicação a pilha está disponível no [Virtual Machines do Azure](https://docs.microsoft.com/azure/virtual-machines/). Com que em mente, contudo é útil para si como um utilizador do serviço de aplicações para saber mais informações, tais como:

-   Como e quando são atualizações do SO aplicadas?
-   Como o serviço de aplicações é aplicado contra vulnerabilidades significativas (tais como o dia zero)?
-   Que versões de SO e tempo de execução são executadas as aplicações?

Por motivos de segurança, determinadas informações específicas das informações de segurança não são publicadas. No entanto, o artigo visa aliviar preocupações, maximizando a transparência do processo e como pode permanecer atualizadas em anúncios relacionadas com segurança ou atualizações de tempo de execução.

## <a name="how-and-when-are-os-updates-applied"></a>Como e quando são atualizações do SO aplicadas?

Azure gere a aplicação de patches de SO em dois níveis, os servidores físicos e as máquinas virtuais de convidado (VMs) com os recursos do serviço de aplicações. Ambos são atualizadas mensalmente, que está alinhada para o mensal [Patch Terça-feira](https://technet.microsoft.com/security/bulletins.aspx) agenda. Estas atualizações são aplicadas automaticamente, de forma que garante que os serviços de SLA do Azure de elevada disponibilidade. 

Para obter informações detalhadas sobre como as atualizações são aplicadas, consulte [Demystifying magic atrás de atualizações do SO do serviço de aplicações](https://blogs.msdn.microsoft.com/appserviceteam/2018/01/18/demystifying-the-magic-behind-app-service-os-updates/).

## <a name="how-does-azure-deal-with-significant-vulnerabilities"></a>Como o Azure lidar com vulnerabilidades significativas?

Quando vulnerabilidades graves precisar de imediato a aplicação de patches, tais como [vulnerabilidades de dia zero](https://wikipedia.org/wiki/Zero-day_(computing)), as atualizações de alta prioridade são processadas de forma caso a caso.

Mantenha-se atualizado com anúncios de segurança críticas no Azure, visitando [blogue de segurança do Azure](https://azure.microsoft.com/blog/topics/security/). 

## <a name="when-are-supported-language-runtimes-updated-added-or-deprecated"></a>Quando são tempos de execução de idiomas suportados atualizados, adicionados ou preteridos?

Novas versões estáveis dos idiomas suportados tempos de execução (principal, secundária ou correção) periodicamente são adicionadas a instâncias de serviço de aplicações. Algumas atualizações substituem a instalação existente, enquanto outros estão instalados side by side with versões existentes. Uma instalação de substituir significa que a aplicação é executada automaticamente no tempo de execução atualizado. Uma instalação lado a lado significa que terá de migrar manualmente a aplicação para tirar partido de uma nova versão do tempo de execução. Para obter mais informações, consulte um das subsecções.

Tempo de execução atualizações e deprecations sejam anunciados aqui:

- https://azure.microsoft.com/updates/?product=app-service 
- https://github.com/Azure/app-service-announcements/issues

> [!NOTE] 
> Aqui a informação que se aplica aos tempos de execução de idioma que são criados numa aplicação do serviço de aplicações. Um tempo de execução personalizado que carregar para o serviço de aplicações, por exemplo, permanece inalterado, a menos que atualizá-lo manualmente.
>
>

### <a name="new-patch-updates"></a>Novas atualizações de correção

Atualizações de correção para .NET, PHP, Java SDK ou versão do Tomcat/Jetty são automaticamente aplicadas ao substituir a instalação existente com a nova versão. NODE.js patch atualizações são instaladas side by side with as versões existentes (semelhantes a versões principais e secundárias na secção seguinte). Versões de patch novo Python podem ser instaladas manualmente através de [site extensões](https://www.siteextensions.net/packages?q=Tags%3A%22python%22)), lado a lado com as instalações de Python incorporadas.

### <a name="new-major-and-minor-versions"></a>Novas versões principais e secundárias

Quando é adicionada uma nova versão de principais ou secundária, é instalada side by side with as versões existentes. Pode atualizar manualmente a aplicação para a nova versão. Se tiver configurado a versão de tempo de execução num ficheiro de configuração (tal como `web.config` e `package.json`), tem de atualizar com o mesmo método. Se utilizou um serviço de aplicações a definição para configurar a sua versão do tempo de execução, pode alterá-la no [portal do Azure](https://portal.azure.com) ou executando um [CLI do Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) no [nuvem Shell](../cloud-shell/overview.md), como mostrado nos exemplos seguintes:

```azurecli-interactive
az webapp config set --net-framework-version v4.7 --resource-group <groupname> --name <appname>
az webapp config set --php-version 7.0 --resource-group <groupname> --name <appname>
az webapp config appsettings set --settings WEBSITE_NODE_DEFAULT_VERSION=8.9.3 --resource-group <groupname> --name <appname>
az webapp config set --python-version 3.4 --resource-group <groupname> --name <appname>
az webapp config set --java-version 1.8 --java-container Tomcat --java-container-version 9.0 --resource-group <groupname> --name <appname>
```

### <a name="deprecated-versions"></a>Versões preteridas  

Quando uma versão mais antiga foi preterida, a data de remoção é anunciada para que possam planear a atualização de versão do tempo de execução em conformidade. 

## <a name="how-can-i-query-os-and-runtime-update-status-on-my-instances"></a>Como pode consultar estado de atualização de SO e tempo de execução no meu instâncias?  

Informações críticas do SO estão bloqueado para baixo de acesso (consulte [funcionalidade do sistema operativo no App Service do Azure](web-sites-available-operating-system-functionality.md)), o [consola Kudu](https://github.com/projectkudu/kudu/wiki/Kudu-console) permite-lhe consultar a instância de serviço de aplicações sobre o SO versão e versões de tempo de execução. 

A tabela seguinte mostra como as versões do Windows e do tempo de execução de idioma que estão a executar as aplicações:

| Informações | Onde encontrá-la | 
|-|-|
| Versão do Windows | Consulte `https://<appname>.scm.azurewebsites.net/Env.cshtml` (em informações do sistema) |
| Versão do .NET | Em `https://<appname>.scm.azurewebsites.net/DebugConsole`, execute o seguinte comando na linha de comandos: <br>`powershell -command "gci 'Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Net Framework Setup\NDP\CDF'"` |
| Versão do .NET core | Em `https://<appname>.scm.azurewebsites.net/DebugConsole`, execute o seguinte comando na linha de comandos: <br> `dotnet --version` |
| Versão de PHP | Em `https://<appname>.scm.azurewebsites.net/DebugConsole`, execute o seguinte comando na linha de comandos: <br> `php --version` |
| Versão do Node.js predefinida | No [nuvem Shell](../cloud-shell/overview.md), execute o seguinte comando: <br> `az webapp config appsettings list --resource-group <groupname> --name <appname> --query "[?name=='WEBSITE_NODE_DEFAULT_VERSION']"` |
| Versão de Python | Em `https://<appname>.scm.azurewebsites.net/DebugConsole`, execute o seguinte comando na linha de comandos: <br> `python --version` |  

> [!NOTE]  
> Acesso à localização de registo `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Component Based Servicing\Packages`, onde obter informações sobre ["KB" patches]((https://docs.microsoft.com/security-updates/SecurityBulletins/securitybulletins)) é armazenada, está a ser bloqueados.
>
>

## <a name="more-resources"></a>Mais recursos

[Centro de confiança: segurança](https://www.microsoft.com/en-us/trustcenter/security)  
[64 bits de núcleo de ASP.NET no App Service do Azure](https://gist.github.com/glennc/e705cd85c9680d6a8f1bdb62099c7ac7)
