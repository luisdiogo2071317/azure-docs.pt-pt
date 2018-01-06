---
title: "Repositórios de registo de contentor do Azure no portal do Azure"
description: "Como ver repositórios de registo de contentor do Azure no portal do Azure."
services: container-registry
author: cristy
manager: timlt
ms.service: container-registry
ms.topic: article
ms.date: 01/05/2018
ms.author: cristyg
ms.openlocfilehash: 593972e972207a27d1232fcb0c1bf220ac3a8def
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/05/2018
---
# <a name="view-container-registry-repositories-in-the-azure-portal"></a>Ver repositórios de registo do contentor no portal do Azure

Registo de contentor do Azure permite-lhe armazenar Docker imagens de contentor no repositórios. Ao armazenar imagens em repositórios, pode armazenar os grupos de imagens (ou versões das imagens) em ambientes isolados. Pode especificar estes repositórios quando enviar imagens para o seu registo e ver os respetivos conteúdos no portal do Azure.

## <a name="prerequisites"></a>Pré-requisitos

* **Registo de contentor**: criar um registo de contentor na sua subscrição do Azure. Por exemplo, utilizar o [portal do Azure](container-registry-get-started-portal.md) ou [CLI do Azure](container-registry-get-started-azure-cli.md).
* **CLI do docker**: instalar [Docker] [ docker-install] no seu computador local, que fornece a interface de linha de comandos do Docker.
* **Imagem de contentor**: emitir uma imagem para o seu registo de contentor. Para obter orientações sobre como push e pull imagens, consulte [Push e pull uma imagem](container-registry-get-started-docker-cli.md).

## <a name="view-repositories-in-azure-portal"></a>Repositórios de ver no portal do Azure

Pode ver uma lista dos repositórios que alojam as imagens, bem como as etiquetas de imagem, no portal do Azure.

Se seguiu os passos no [Push e pull uma imagem](container-registry-get-started-docker-cli.md) (e, subsequentemente, não eliminou a imagem), deve ter uma imagem de Nginx no seu registo de contentor. As instruções esse artigo especificado que sinalizar a imagem com um espaço de nomes, "exemplos" em `/samples/nginx`. Como um atualizador do [docker push] [ docker-push] comando especificado esse artigo foi:

```Bash
docker push myregistry.azurecr.io/samples/nginx
```

 Como o registo de contentor do Azure suporta esses espaços de nomes de vários níveis de repositório, pode definir o âmbito de coleções de imagens relacionadas com a uma aplicação específica ou uma coleção de aplicações, desenvolvimento diferente ou equipas operacionais. Para ler mais sobre repositórios nos registos do contentor, consulte [os registos do contentor de Docker privados no Azure](container-registry-intro.md).

Para ver um repositório:

1. Iniciar sessão para o [portal do Azure][portal]
1. Selecione o **registo de contentor do Azure** aos quais é feito o Push da imagem de Nginx
1. Selecione **repositórios** para ver uma lista dos repositórios do que contêm as imagens no registo
1. Selecionar um repositório para ver as etiquetas de imagem dentro desse repositório

Por exemplo, se instalada a imagem de Nginx como instruções na [Push e pull uma imagem](container-registry-get-started-docker-cli.md), deverá ver algo semelhante a:

![Repositórios no portal](./media/container-registry-repositories/container-registry-repositories.png)

## <a name="next-steps"></a>Passos Seguintes

Agora que conhece as noções básicas de visualizar e trabalhar com os repositórios no portal, tente utilizar o registo de contentor do Azure com um [serviço de contentor do Azure (AKS)](../aks/tutorial-kubernetes-prepare-app.md) cluster.

<!-- LINKS - External -->
[docker-install]: https://docs.docker.com/engine/installation/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[portal]: https://portal.azure.com
