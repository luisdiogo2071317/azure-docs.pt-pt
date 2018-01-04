---
title: "Executar uma imagem personalizada do Docker Hub na Aplicação Web do Azure para Contentores | Microsoft Docs"
description: "Como utilizar uma imagem personalizada do Docker para as Aplicações Web do Azure para Contentores."
keywords: "serviço de aplicações do azure, aplicação web, linux, docker, contentor"
services: app-service
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: b97bd4e6-dff0-4976-ac20-d5c109a559a8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 11/02/2017
ms.author: cephalin;wesmc
ms.custom: mvc
ms.openlocfilehash: a95a8435e4ecef201ad0f6d9ecda68e94f06ea80
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/14/2017
---
# <a name="run-a-custom-docker-hub-image-in-azure-web-app-for-containers"></a>Executar uma imagem personalizada do Docker Hub na Aplicação Web do Azure para Contentores

O Serviço de Aplicações dispõe de pilhas de aplicação previamente definidas no Linux com suporte para versões específicas, como PHP 7.0 e Node.js 4.5. Também pode utilizar uma imagem personalizada do Docker para executar a aplicação Web numa pilha de aplicação que ainda não esteja definida no Azure. Este guia de introdução mostra como criar uma aplicação Web e implementar a [imagem oficial do Nginx Docker](https://hub.docker.com/r/_/nginx/) no mesmo. Irá criar a aplicação Web com a [CLI do Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).

![Aplicação de exemplo em execução no Azure](media/quickstart-custom-docker-image/hello-world-in-browser.png)

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user.md)]

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group.md)]

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux.md)]

## <a name="create-a-web-app-for-container"></a>Criar uma Aplicação Web para Contentor

Crie uma [aplicação Web](../app-service-web-overview.md) no plano do `myAppServicePlan`Serviço de Aplicações com o comando[az webapp create](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create). Não se esqueça de substituir `<app name>` por um nome de aplicação único.

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app name> --deployment-container-image-name nginx
```

No comando anterior, `--deployment-container-image-name` aponta para a imagem pública do Hub do Docker [https://hub.docker.com/r/_/nginx/](https://hub.docker.com/r/_/nginx/).

Quando a aplicação Web tiver sido criada, a CLI do Azure mostra informações semelhantes ao seguinte exemplo:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app name>.scm.azurewebsites.net/<app name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

## <a name="browse-to-the-app"></a>Navegar para a aplicação

Procure o seguinte URL com o browser.

```bash
http://<app_name>.azurewebsites.net
```

![Aplicação de exemplo em execução no Azure](media/quickstart-custom-docker-image/hello-world-in-browser.png)

**Parabéns!** Implementou uma imagem personalizada do Docker para as Aplicações Web para Contentores.

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Utilizar uma imagem personalizada do Docker](tutorial-custom-docker-image.md)
