---
title: Implementar a aplicação no serviço de aplicações do Azure através de FTP/S | Microsoft Docs
description: Saiba como implementar a aplicação no serviço de aplicações do Azure através de FTP ou FTPS.
services: app-service
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
ms.assetid: ae78b410-1bc0-4d72-8fc4-ac69801247ae
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2018
ms.author: cephalin;dariac
ms.openlocfilehash: 7e05e06a5abd02dd67f58a8e01bb246e318f51de
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/07/2018
ms.locfileid: "34850241"
---
# <a name="deploy-your-app-to-azure-app-service-using-ftps"></a>Implementar a aplicação no serviço de aplicações do Azure através de FTP/S

Este artigo mostra como utilizar o FTP ou FTPS para implementar a sua aplicação web, o back-end da aplicação móvel ou a aplicação API para [App Service do Azure](http://go.microsoft.com/fwlink/?LinkId=529714).

O ponto final FTP/S para a sua aplicação já está ativo. É necessária ativar a implementação de FTP/S nenhuma configuração.

## <a name="open-ftp-dashboard"></a>Abra dashboard FTP

No [portal do Azure](https://portal.azure.com), abra a aplicação [página recursos](../azure-resource-manager/resource-group-portal.md#manage-resources).

Para abrir o dashboard FTP, clique em **entrega contínua (pré-visualização)** > **FTP** > **Dashboard**.

![Abra dashboard FTP](./media/app-service-deploy-ftp/open-dashboard.png)

## <a name="get-ftp-connection-information"></a>Obter informações de ligação de FTP

No dashboard do FTP, clique em **cópia** para copiar as credenciais de ponto final e aplicação FTPS.

![Copiar as informações de FTP](./media/app-service-deploy-ftp/ftp-dashboard.png)

É recomendado que utilize **aplicação credenciais** para implementar na sua aplicação, porque é exclusivo para cada aplicação. No entanto, se clicar em **credenciais de utilizador**, pode definir as credenciais de nível de utilizador que pode utilizar para início de sessão FTP/S para todas as aplicações de serviço de aplicações na sua subscrição.

## <a name="deploy-files-to-azure"></a>Implementar ficheiros para o Azure

1. A partir do seu cliente FTP (por exemplo, [Visual Studio](https://www.visualstudio.com/vs/community/) ou [FileZilla](https://filezilla-project.org/download.php?type=client)), utilize as informações de ligação que recolheu para ligar à sua aplicação.
3. Copiar os ficheiros e a respetiva estrutura de diretórios correspondentes para o [ **/site/wwwroot** diretório](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) no Azure (ou o **/site/wwwroot/App_Data/tarefas/** diretório para WebJobs).
4. Navegue até ao URL da sua aplicação para verificar que a aplicação está a funcionar corretamente. 

> [!NOTE] 
> Ao contrário [implementações baseadas em Git](app-service-deploy-local-git.md), implementação de FTP não suporta as automatizações de implementação seguintes: 
>
> - dependência restaura (por exemplo, automatizações NuGet, NPM, PIP e compositor)
> - compilação de binários de .NET
> - a geração da Web. config (aqui está uma [exemplo de Node.js](https://github.com/projectkudu/kudu/wiki/Using-a-custom-web.config-for-Node-apps))
> 
> Gerar estes ficheiros necessários manualmente no seu computador local e, em seguida, implementá-las em conjunto com a sua aplicação.
>
>

## <a name="enforce-ftps"></a>Impor FTPS

Para maior segurança, deve permitir FTP por SSL apenas. Também pode desativar FTP e FTPS se não utilizar a implementação de FTP.

Na página de recursos da sua aplicação no [portal do Azure](https://portal.azure.com), selecione **as definições de aplicação** no painel de navegação esquerdo.

Para desativar o FTP não encriptada, selecione **FTPS apenas**. Para desativar completamente o FTP e FTPS, selecione **desativar**. Quando terminar, clique em **guardar**.

![Desativar FTP/S](./media/app-service-deploy-ftp/disable-ftp.png)

## <a name="automate-with-scripts"></a>Automatizar com scripts

Para a implementação de FTP utilizando [CLI do Azure](/cli/azure), consulte [criar uma aplicação web e implementar os ficheiros com FTP (CLI do Azure)](./scripts/app-service-cli-deploy-ftp.md).

Para a implementação de FTP utilizando [Azure PowerShell](/cli/azure), consulte [carregar ficheiros para uma aplicação web utilizando o FTP (PowerShell)](./scripts/app-service-powershell-deploy-ftp.md).

## <a name="troubleshoot-ftp-deployment"></a>Resolver problemas de implementação de FTP

- [Como posso resolver problemas de implementação de FTP?](#how-can-i-troubleshoot-ftp-deployment)
- [Posso não estou capaz de FTP e publicar o meu código. Como posso resolver o problema?](#im-not-able-to-ftp-and-publish-my-code-how-can-i-resolve-the-issue)
- [Como ligar para FTP no App Service do Azure através de modo passivo?](#how-can-i-connect-to-ftp-in-azure-app-service-via-passive-mode)

### <a name="how-can-i-troubleshoot-ftp-deployment"></a>Como posso resolver problemas de implementação de FTP?

O primeiro passo para resolução de problemas de implementação de FTP é isolar um problema de implementação de um problema de aplicação de tempo de execução.

Um problema de implementação normalmente resulta em nenhum ficheiros ou ficheiros errados implementados para a sua aplicação. Pode resolver por investigar a implementação de FTP ou selecionar um caminho de implementação alternativos (por exemplo, o controlo de origem).

Um problema de aplicação de tempo de execução normalmente resulta no conjunto correto de ficheiros implementado a sua aplicação mas o comportamento da aplicação incorreto. Pode resolver concentrar-se no comportamento de código em tempo de execução e investigar caminhos falha específica.

Para determinar um problema de implementação ou o tempo de execução, consulte [implementação vs. problemas de tempo de execução](https://github.com/projectkudu/kudu/wiki/Deployment-vs-runtime-issues).

### <a name="im-not-able-to-ftp-and-publish-my-code-how-can-i-resolve-the-issue"></a>Posso não estou capaz de FTP e publicar o meu código. Como posso resolver o problema?
Verifique se introduziu o nome de anfitrião correto e [credenciais](#step-1--set-deployment-credentials). Verifique também se as seguintes portas FTP no seu computador não são bloqueadas por uma firewall:

- Porta de ligação de controlo de FTP: 21
- Porta de ligação de dados FTP: 989, 10001 10300
 
### <a name="how-can-i-connect-to-ftp-in-azure-app-service-via-passive-mode"></a>Como ligar para FTP no App Service do Azure através de modo passivo?
App Service do Azure suporta a ligação ocorra através de modo ativo e passivo. Modo passivo é preferencial porque as máquinas de implementação são, normalmente, protegidos por uma firewall (no sistema operativo ou como parte de uma casa ou de rede de negócio). Consulte uma [exemplo na documentação do WinSCP](https://winscp.net/docs/ui_login_connection). 

## <a name="next-steps"></a>Passos Seguintes

Para cenários de implementação mais avançados, tente [implementar no Azure com o Git](app-service-deploy-local-git.md). Implementação baseada em Git para o Azure permite o controlo de versão, o restauro de pacote, MSBuild e muito mais.

## <a name="more-resources"></a>Mais Recursos

* [Credenciais de implementação do App Service do Azure](app-service-deploy-ftp.md)
