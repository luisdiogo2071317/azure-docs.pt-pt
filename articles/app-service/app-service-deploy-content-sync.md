---
title: "Conteúdo de sincronização a partir de uma pasta de cloud App Service do Azure"
description: "Saiba como implementar a aplicação no App Service do Azure através de sincronização de conteúdo da pasta de nuvem."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: 88d3a670-303a-4fa2-9de9-715cc904acec
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2016
ms.author: dariagrigoriu
ms.openlocfilehash: 8e132e4d4a65588d57e3cfb969e785f5a164206c
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2018
---
# <a name="sync-content-from-a-cloud-folder-to-azure-app-service"></a>Conteúdo de sincronização a partir de uma pasta de cloud App Service do Azure
Este tutorial mostra como a sincronizar o seu conteúdo [App Service do Azure](http://go.microsoft.com/fwlink/?LinkId=529714) nuvem populares dos serviços de armazenamento, como o Dropbox e OneDrive. 

## <a name="overview"></a>Descrição geral da implementação de conteúdo de sincronização
Utiliza a tecnologia de implementação de sincronização de conteúdo a pedido a [motor de implementação do Kudu](https://github.com/projectkudu/kudu/wiki) integrado com o serviço de aplicações. No [portal do Azure](https://portal.azure.com), pode designar uma pasta no seu armazenamento na nuvem, trabalhar com o código da aplicação e conteúdo nessa pasta e sincronizar para o App Service com a, clique em de um botão. O processo do Kudu de compilação e implementação utiliza a sincronização de conteúdo. 

## <a name="contentsync"></a>Como ativar a implementação de conteúdo de sincronização
Para ativar a sincronização de conteúdo do [portal do Azure](https://portal.azure.com), siga estes passos:

1. Na página da sua aplicação no portal do Azure, clique em **definições** > **origem de implementação**. Clique em **Escolher origem**, em seguida, selecione **OneDrive** ou **Dropbox** como origem para a implementação. 
   
    ![Sincronização de conteúdo](./media/app-service-deploy-content-sync/deployment_source.png)
   
   > [!NOTE]
   > Devido às diferenças subjacentes nas APIs, **OneDrive para empresas** não é suportado neste momento. 
   > 
   > 
2. O fluxo de trabalho de autorização para ativar o serviço de aplicações aceder a um caminho predefinido de designado específico para o OneDrive ou Dropbox, onde todos os serviço de aplicações de conteúdo concluída é armazenado. Após a autorização, a plataforma de serviço de aplicações dá-lhe a opção para criar uma pasta de conteúdo no caminho de conteúdo do designado ou escolher uma pasta de conteúdo existente sob este caminho de conteúdo designado. Os caminhos de conteúdo designados sob as contas de armazenamento em nuvem utilizados para sincronização do serviço de aplicações são os seguintes:  
   
   * **OneDrive**: `Apps\Azure Web Apps` 
   * **Dropbox**:`Dropbox\Apps\Azure`
3. Após a sincronização de conteúdo inicial, a sincronização de conteúdo pode ser iniciada a pedido do portal do Azure. Histórico de implementação está disponível na **implementações** página.
   
    ![Histórico de implementação](./media/app-service-deploy-content-sync/onedrive_sync.png)

Obter mais informações para a implementação da Dropbox estão disponíveis em [implementar da Dropbox] (https://azure.microsoft.com/en-in/blog/new-deploy-to-windows-azure-web-sites-from-dropbox/).

