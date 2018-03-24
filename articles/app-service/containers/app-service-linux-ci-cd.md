---
title: Implementação contínua provém de um registo de contentor do Docker com a aplicação Web para contentores - Azure | Microsoft Docs
description: Como configurar a implementação contínua provém de um registo de contentor do Docker na aplicação Web para contentores.
keywords: serviço de aplicações do Azure, linux, docker, acr, oss
services: app-service
documentationcenter: ''
author: ahmedelnably
manager: cfowler
editor: ''
ms.assetid: a47fb43a-bbbd-4751-bdc1-cd382eae49f8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: aelnably;msangapu
ms.openlocfilehash: ac35dbd041de50ab8aae1a0fb4c00fe3917a7297
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="continuous-deployment-with-web-app-for-containers"></a>Implementação contínua com a aplicação Web para contentores

Neste tutorial, configura a implementação contínua para uma imagem personalizada do contentor de geridos [registo de contentor do Azure](https://azure.microsoft.com/services/container-registry/) repositórios ou [Docker Hub](https://hub.docker.com).

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no [portal do Azure](https://portal.azure.com).

## <a name="enable-the-continuous-deployment-feature"></a>Ativar a funcionalidade de implementação contínua

Ativar a funcionalidade de implementação contínua utilizando [CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) e executando o seguinte comando:

```azurecli-interactive
az webapp deployment container config --name name --resource-group myResourceGroup --enable-cd true
```

No [portal do Azure](https://portal.azure.com/), selecione o **do serviço de aplicações** opção no lado esquerdo da página.

Selecione o nome da aplicação para o qual pretende configurar a implementação contínua do Hub de Docker.

No **contentor de Docker** página, selecione **no**e, em seguida, selecione **guardar** para ativar a implementação contínua.

![Captura de ecrã da definição de aplicação](./media/app-service-webapp-service-linux-ci-cd/step2.png)

## <a name="prepare-the-webhook-url"></a>Preparar o URL do webhook

Obter o URL do webhook utilizando [CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) e executando o seguinte comando:

```azurecli-interactive
az webapp deployment container show-cd-url --name sname1 --resource-group rgname
```

Para o URL do webhook, é necessário o seguinte ponto final: `https://<publishingusername>:<publishingpwd>@<sitename>.scm.azurewebsites.net/docker/hook`.

Pode obter o `publishingusername` e `publishingpwd` , transferindo-a aplicação web publicar perfis com o portal do Azure.

![Captura de ecrã da adição de webhook 2](./media/app-service-webapp-service-linux-ci-cd/step3-3.png)

## <a name="add-a-webhook"></a>Adicionar um webhook

### <a name="azure-container-registry"></a>Registo de Contentores do Azure

1. Na sua página de portal de registo, selecione **Webhooks**.
2. Para criar um novo webhook, selecione **adicionar**. 
3. No **criar o webhook** painel, dê um nome do webhook. Para o webhook URI, forneça o URL que obteve na secção anterior.

Certifique-se que definir o âmbito como repositório que contém a imagem de contentor.

![Captura de ecrã do webhook](./media/app-service-webapp-service-linux-ci-cd/step3ACRWebhook-1.png)

Quando atualizar a imagem, a aplicação web é atualizada automaticamente com a nova imagem.

### <a name="docker-hub"></a>Hub de docker

Na página do seu Hub de Docker, selecione **Webhooks**e, em seguida, **WEBHOOK de A criar**.

![Captura de ecrã da adição de webhook 1](./media/app-service-webapp-service-linux-ci-cd/step3-1.png)

Para o URL do webhook, forneça o URL que obteve anteriormente.

![Captura de ecrã da adição de webhook 2](./media/app-service-webapp-service-linux-ci-cd/step3-2.png)

Quando atualizar a imagem, a aplicação web é atualizada automaticamente com a nova imagem.

## <a name="next-steps"></a>Passos Seguintes

* [Introdução ao App Service do Azure no Linux](./app-service-linux-intro.md)
* [Registo de contentor do Azure](https://azure.microsoft.com/services/container-registry/)
* [Criar uma aplicação Web .NET Core no Serviço de Aplicações no Linux](quickstart-dotnetcore.md)
* [Criar uma aplicação Ruby web no App Service no Linux](quickstart-ruby.md)
* [Implementar uma aplicação web de Docker/aceda na aplicação Web para contentores](quickstart-docker-go.md)
* [FAQ do Serviço de Aplicações do Azure no Linux](./app-service-linux-faq.md)
* [Gerir a aplicação Web para contentores utilizando a CLI do Azure](./app-service-linux-cli.md)
