---
title: incluir ficheiro
description: incluir ficheiro
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 01/23/2019
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 7ed8b96a528d56b28262936c4b200762b3e93b8e
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/30/2019
ms.locfileid: "55302249"
---
## <a name="push-image-to-registry"></a>Enviar imagem para o registo

Para enviar uma imagem para um registo do Azure Container, primeiro tem de ter uma imagem. Se ainda não tiver quaisquer imagens de contentor local, execute o seguinte [pull do docker] [ docker-pull] comando para solicitar uma imagem existente do Hub do Docker. Neste exemplo, extrair o `hello-world` imagem.

```Docker
docker pull hello-world
```

Antes de emitir uma imagem para o registo, tem de o etiquetar com o nome completamente qualificado do seu servidor de início de sessão ACR. O nome do servidor de início de sessão está no formato  *\<nome do registo\>. azurecr.io* (em minúsculas), por exemplo, *mycontainerregistry007.azurecr.io*.

Crie uma etiqueta para a imagem com o comando [etiqueta do docker][docker-tag]. Substitua `<acrLoginServer>` pelo nome do servidor de início de sessão da sua instância do ACR.

```Docker
docker tag hello-world <acrLoginServer>/hello-world:v1
```

Por último, utilize o [envio do docker][docker-push] para enviar a imagem para a instância do ACR. Substitua `<acrLoginServer>` pelo nome do servidor de início de sessão da sua instância do ACR. Este exemplo cria os **hello-world** repositório, que contém o `hello-world:v1` imagem.

```Docker
docker push <acrLoginServer>/hello-world:v1
```

Depois de enviar a imagem para o seu registo de contentor, remova o `hello-world:v1` imagem do seu ambiente Docker local. (Tenha em atenção que este [docker rmi] [ docker-rmi] comando não remove a imagem a partir do **hello-world** repositório no seu registo de contentor do Azure.)

```Docker
docker rmi <acrLoginServer>/hello-world:v1
```

<!-- LINKS - External -->
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-pull]: https://docs.docker.com/engine/reference/commandline/pull/
[docker-rmi]: https://docs.docker.com/engine/reference/commandline/rmi/
[docker-run]: https://docs.docker.com/engine/reference/commandline/run/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/

<!-- LINKS - Internal -->

