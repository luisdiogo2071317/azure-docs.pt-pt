---
title: Conteúdo de sincronização a partir de uma pasta de cloud App Service do Azure
description: Saiba como implementar a aplicação no App Service do Azure através de sincronização de conteúdo da pasta de nuvem.
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
ms.assetid: 88d3a670-303a-4fa2-9de9-715cc904acec
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2018
ms.author: cephalin;dariagrigoriu
ms.openlocfilehash: 3781010c74daa51c92813db85ee03eaa4c02a4cf
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35233592"
---
# <a name="sync-content-from-a-cloud-folder-to-azure-app-service"></a>Conteúdo de sincronização a partir de uma pasta de cloud App Service do Azure
Este artigo mostra-lhe como sincronizar o seu conteúdo [App Service do Azure](http://go.microsoft.com/fwlink/?LinkId=529714) da Dropbox e OneDrive. 

Tem a alimentação ligada a implementação de sincronização de conteúdo a pedido pelo App Service [motor de implementação do Kudu](https://github.com/projectkudu/kudu/wiki). Pode, trabalhar com o código da aplicação e o conteúdo numa pasta designada de nuvem e, em seguida, sincronizar para o serviço de aplicações com clique do botão para um. Sincronização conteúda utiliza o servidor de compilação do Kudu. 

## <a name="enable-content-sync-deployment"></a>Ativar a implementação de conteúdo de sincronização

Para ativar a sincronização de conteúdo, navegue para a página da aplicação do serviço de aplicações no [portal do Azure](https://portal.azure.com).

No menu à esquerda, clique em **Centro de implementação** > **OneDrive** ou **Dropbox** > **autorizar**. Siga as instruções de autorização. 

![](media/app-service-deploy-content-sync/choose-source.png)

Basta autorizar com o OneDrive ou Dropbox uma vez. Se já estiver autorizado, basta clicar **continuar**. Pode alterar a conta do OneDrive ou Dropbox autorizada clicando **alterar conta**.

![](media/app-service-deploy-content-sync/continue.png)

No **configurar** página, selecione a pasta que pretende sincronizar. Esta pasta é criada no seguinte caminho de conteúdo designado no OneDrive ou Dropbox. 
   
* **OneDrive**: `Apps\Azure Web Apps`
* **Dropbox**: `Apps\Azure`

Quando terminar, clique em **continuar**.

No **resumo** página, verifique as suas opções e clique em **concluir**.

## <a name="synchronize-content"></a>Sincronizar o conteúdo

Quando pretender sincronizar o conteúdo na sua pasta de nuvem com o serviço de aplicações, volte atrás para o **Centro de implementação** página e clique em **sincronização**.

![](media/app-service-deploy-content-sync/synchronize.png)
   
   > [!NOTE]
   > Devido às diferenças subjacentes nas APIs, **OneDrive para empresas** não é suportado neste momento. 
   > 
   > 

## <a name="disable-content-sync-deployment"></a>Desativar a implementação de conteúdo de sincronização

Para desativar a sincronização de conteúdo, navegue para a página da aplicação do serviço de aplicações no [portal do Azure](https://portal.azure.com).

No menu à esquerda, clique em **Centro de implementação** > **OneDrive** ou **Dropbox** > **desligar**.

![](media/app-service-deploy-content-sync/disable.png)

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Implementar a partir do repositório de Git local](app-service-deploy-local-git.md)
