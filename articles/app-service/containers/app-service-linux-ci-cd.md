---
title: Implementação contínua a partir de um registo de contentor do Docker com a aplicação Web para contentores - Azure | Documentos da Microsoft
description: Como configurar a implementação contínua a partir de um registo de contentor do Docker na aplicação Web para contentores.
keywords: serviço de aplicações do Azure, linux, docker, acr, oss
services: app-service
documentationcenter: ''
author: msangapu
manager: jeconnoc
editor: ''
ms.assetid: a47fb43a-bbbd-4751-bdc1-cd382eae49f8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2018
ms.author: msangapu
ms.openlocfilehash: 20ca63b7126a6800538129115ff339308c11d8c5
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/08/2018
ms.locfileid: "48867030"
---
# <a name="continuous-deployment-with-web-app-for-containers"></a>Implementação contínua com a aplicação Web para contentores

Neste tutorial, vai configurar a implementação contínua para uma imagem de contentor personalizado do managed [Azure Container Registry](https://azure.microsoft.com/services/container-registry/) repositórios ou [Docker Hub](https://hub.docker.com).

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no [portal do Azure](https://portal.azure.com).

## <a name="enable-the-continuous-deployment-feature"></a>Ativar a funcionalidade de implementação contínua

Ativar a funcionalidade de implementação contínua utilizando [CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) e executando o seguinte comando:

```azurecli-interactive
az webapp deployment container config --name name --resource-group myResourceGroup --enable-cd true
```

Na [portal do Azure](https://portal.azure.com/), selecione a **serviço de aplicações** opção no lado esquerdo da página.

Selecione o nome da aplicação para o qual pretende configurar a implementação contínua do Docker Hub.

Sobre o **definições de contentor** página, selecione **no**e, em seguida, selecione **guardar** para ativar a implementação contínua.

![Captura de ecrã da definição de aplicação](./media/app-service-webapp-service-linux-ci-cd/step2.png)

## <a name="prepare-the-webhook-url"></a>Preparar o URL do webhook

Obter o URL do webhook, utilizando [CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) e executando o seguinte comando:

```azurecli-interactive
az webapp deployment container show-cd-url --name sname1 --resource-group rgname
```

Anote o URL do webhook. Precisará das mesmas na próxima seção.
`https://<publishingusername>:<publishingpwd>@<sitename>.scm.azurewebsites.net/docker/hook`.

Pode obter sua `publishingusername` e `publishingpwd` ao transferir a aplicação web de perfil com o portal do Azure de publicação.

![Captura de ecrã de adicionar webhook 2](./media/app-service-webapp-service-linux-ci-cd/step3-3.png)

## <a name="add-a-webhook"></a>Adicionar um webhook

Para adicionar um webhook, siga as etapas nestes guias:

- [O Azure Container Registry](../../container-registry/container-registry-webhook.md) utilizando o URL do webhook
- [Webhooks para o Hub do Docker](https://docs.docker.com/docker-hub/webhooks/)

## <a name="next-steps"></a>Passos Seguintes

* [Introdução ao serviço de aplicações do Azure no Linux](./app-service-linux-intro.md)
* [Registo de contentor do Azure](https://azure.microsoft.com/services/container-registry/)
* [Criar uma aplicação Web .NET Core no Serviço de Aplicações no Linux](quickstart-dotnetcore.md)
* [Criar uma aplicação Ruby web no serviço de aplicações no Linux](quickstart-ruby.md)
* [Implementar uma aplicação web do Docker/Go na aplicação Web para contentores](quickstart-docker-go.md)
* [FAQ do Serviço de Aplicações do Azure no Linux](./app-service-linux-faq.md)
* [Gerir a aplicação Web para contentores com a CLI do Azure](./app-service-linux-cli.md)
