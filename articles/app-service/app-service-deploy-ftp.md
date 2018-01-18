---
title: "Implementar a aplicação no serviço de aplicações do Azure através de FTP/S | Microsoft Docs"
description: "Saiba como implementar a aplicação no serviço de aplicações do Azure através de FTP ou FTPS."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: ae78b410-1bc0-4d72-8fc4-ac69801247ae
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2016
ms.author: cephalin;dariac
ms.openlocfilehash: fcd079306a8968505349bb3f4a805f203a5c9999
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2018
---
# <a name="deploy-your-app-to-azure-app-service-using-ftps"></a>Implementar a aplicação no serviço de aplicações do Azure através de FTP/S

Este artigo mostra como utilizar o FTP ou FTPS para implementar a sua aplicação web, o back-end da aplicação móvel ou a aplicação API para [App Service do Azure](http://go.microsoft.com/fwlink/?LinkId=529714).

O ponto final FTP/S para a sua aplicação já está ativo. É necessária ativar a implementação de FTP/S nenhuma configuração.

<a name="step1"></a>
## <a name="step-1-set-deployment-credentials"></a>Passo 1: Definir credenciais de implementação

Para aceder ao servidor FTP para a sua aplicação, primeiro precisa de credenciais de implementação. 

Para definir ou repor as credenciais de implementação, consulte o artigo [credenciais de implementação de serviço de aplicações do Azure](app-service-deployment-credentials.md). Este tutorial demonstra a utilização de credenciais de nível de utilizador.

## <a name="step-2-get-ftp-connection-information"></a>Passo 2: Obter informações de ligação de FTP

1. No [portal do Azure](https://portal.azure.com), abra a aplicação [página recursos](../azure-resource-manager/resource-group-portal.md#manage-resources).
2. Selecione **descrição geral** no menu à esquerda, em seguida, anote os valores para **utilizador FTP/implementação**, **nome de anfitrião do FTP**, e **nome de anfitrião FTPS**. 

    ![Informações de ligação de FTP](./media/app-service-deploy-ftp/FTP-Connection-Info.PNG)

    > [!NOTE]
    > Para fornecer contexto adequado para o servidor FTP, a **utilizador FTP/implementação** valor apresentado pelo portal do Azure inclui o nome da aplicação.
    > Pode encontrar as informações do mesmas quando selecionar **propriedades** no menu esquerdo. 
    >
    > Além disso, a palavra-passe de implementação nunca é apresentada. Se se esquecer da sua palavra-passe de implementação, volte atrás para [passo 1](#step1) e repor a palavra-passe de implementação.
    >
    >

## <a name="step-3-deploy-files-to-azure"></a>Passo 3: Implementação de ficheiros para o Azure

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

## <a name="next-steps"></a>Passos Seguintes

Para cenários de implementação mais avançados, tente [implementar no Azure com o Git](app-service-deploy-local-git.md). Implementação baseada em Git para o Azure permite o controlo de versão, o restauro de pacote, MSBuild e muito mais.

## <a name="more-resources"></a>Mais Recursos

* [Credenciais de implementação do App Service do Azure](app-service-deploy-ftp.md)
