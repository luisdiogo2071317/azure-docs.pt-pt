---
title: "Implementação contínua provém de um registo de contentor do Docker com a aplicação Web para contentores - Azure | Microsoft Docs"
description: "Como configurar a implementação contínua provém de um registo de contentor do Docker na aplicação Web para contentores."
keywords: "serviço de aplicações do Azure, linux, docker, acr, oss"
services: app-service
documentationcenter: 
author: ahmedelnably
manager: cfowler
editor: 
ms.assetid: a47fb43a-bbbd-4751-bdc1-cd382eae49f8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: aelnably;wesmc
ms.openlocfilehash: 84bc344438d5542cc6409fa50aa6e55eb547c6a8
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/23/2018
---
# <a name="continuous-deployment-with-web-app-for-containers"></a>Implementação contínua com a aplicação Web para contentores

Neste tutorial, configura a implementação contínua para uma imagem personalizada contentor de gerido [registo de contentor do Azure](https://azure.microsoft.com/services/container-registry/) repositórios ou [Docker Hub](https://hub.docker.com).

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Iniciar sessão para o [portal do Azure](https://portal.azure.com)

## <a name="enable-container-continuous-deployment-feature"></a>Ativar funcionalidade de implementação contínua do contentor

Pode ativar a funcionalidade de implementação contínua utilizando [CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) e executando o seguinte comando

```azurecli-interactive
az webapp deployment container config -n sname -g rgname -e true
```

No  **[portal do Azure](https://portal.azure.com/)**, clique em de **do serviço de aplicações** opção no lado esquerdo da página.

Clique no nome da sua aplicação que pretende configurar a implementação contínua do Docker Hub para.

No **as definições de aplicação**, adicionar uma aplicação definição chamado `DOCKER_ENABLE_CI` com o valor `true`.

![Inserir uma imagem de definição de aplicação](./media/app-service-webapp-service-linux-ci-cd/step2.png)

## <a name="prepare-webhook-url"></a>Preparar o URL do Webhook

Pode obter o URL do Webhook utilizando [CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) e executando o seguinte comando

```azurecli-interactive
az webapp deployment container show-cd-url -n sname1 -g rgname
```

Para o URL do Webhook, tem de ter o seguinte ponto final: `https://<publishingusername>:<publishingpwd>@<sitename>.scm.azurewebsites.net/docker/hook`.

Pode obter o `publishingusername` e `publishingpwd` , transferindo-a aplicação web publicar perfis com o portal do Azure.

![Inserir uma imagem de adição de webhook 2](./media/app-service-webapp-service-linux-ci-cd/step3-3.png)

## <a name="add-a-web-hook"></a>Adicionar um hook de web

### <a name="azure-container-registry"></a>Registo de Contentores do Azure

No seu painel do portal de registo, clique em **Webhooks**, criar um novo webhook clicando **adicionar**. No **criar o webhook** painel, dê um nome do webhook. Para o URI de Webhook tem de fornecer o URL obtido **passo 3**

Certifique-se de que é possível definir o âmbito como repositório que contém a imagem de contentor.

![Inserir uma imagem do webhook](./media/app-service-webapp-service-linux-ci-cd/step3ACRWebhook-1.png)

Quando a imagem for atualizada, a aplicação web for atualizado automaticamente com a nova imagem.

### <a name="docker-hub"></a>Hub de docker

Na página do seu Hub de Docker, clique em **Webhooks**, em seguida, **WEBHOOK de A criar**.

![Inserir uma imagem de adição de webhook 1](./media/app-service-webapp-service-linux-ci-cd/step3-1.png)

Para o URL do Webhook, tem de fornecer o URL obtido **passo 3**

![Inserir uma imagem de adição de webhook 2](./media/app-service-webapp-service-linux-ci-cd/step3-2.png)

Quando a imagem for atualizada, a aplicação web for atualizado automaticamente com a nova imagem.

## <a name="next-steps"></a>Passos Seguintes

* [O que é o serviço de aplicações do Azure no Linux?](./app-service-linux-intro.md)
* [Registo de contentor do Azure](https://azure.microsoft.com/services/container-registry/)
* [Utilizar o .NET Core no Serviço de Aplicações do Azure no Linux](quickstart-dotnetcore.md)
* [Utilizar o Ruby no Serviço de Aplicações do Azure no Linux](quickstart-ruby.md)
* [Como utilizar uma imagem personalizada do Docker para as Aplicações Web para Contentores](quickstart-docker-go.md)
* [FAQ sobre a Aplicação Web do Serviço de Aplicações do Azure para Contentores](./app-service-linux-faq.md)
* [Gerir a aplicação Web para contentores utilizando o Azure CLI 2.0](./app-service-linux-cli.md)