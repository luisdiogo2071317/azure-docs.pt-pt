---
title: "Tutorial do Azure Container Instances - preparar a sua aplicação"
description: "Tutorial do Azure Container Instances, parte 1 de 3 - preparar uma aplicação para implementação no Azure Container Instances"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: tutorial
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 5012412ec642a04102836274caea253635376efb
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/27/2018
---
# <a name="create-container-for-deployment-to-azure-container-instances"></a>Criar um contentor para implementação em Instâncias do Azure Container

As Instâncias do Azure Container permitem implementar contentores do Docker na infraestrutura do Azure sem que seja necessário aprovisionar máquinas virtuais ou adotar serviços de nível superior. Neste tutorial, vai compilar uma aplicação Web pequena em Node.js e compactá-la num contentor que pode ser executado com o Azure Container Instances.

Neste artigo, a parte um da série, vai:

> [!div class="checklist"]
> * Clonar o código-fonte da aplicação do GitHub
> * Criar uma imagem de contentor a partir da origem de aplicação
> * Testar a imagem num ambiente local do Docker

Nos tutoriais subsequentes, vai carregar a imagem para um registo do Azure Container Registry e, em seguida, vai implementá-la no Azure Container Instances.

## <a name="before-you-begin"></a>Antes de começar

Este tutorial requer a execução da versão 2.0.23 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, consulte [Instalar o Azure CLI 2.0][azure-cli-install].

Este tutorial pressupõe conhecimentos básicos dos principais conceitos do Docker, como contentores, imagens de contentor e comandos básicos do `docker`. Se for necessário, veja a [Introdução ao Docker][docker-get-started] para obter um manual sobre as noções básicas dos contentores.

Para concluir este tutorial, precisa de um ambiente de desenvolvimento do Docker, localmente instalado. O Docker disponibiliza pacotes que o configuram facilmente em qualquer sistema [Mac][docker-mac], [Windows][docker-windows] ou [Linux][docker-linux].

O Azure Cloud Shell não inclui os componentes do Docker necessários para concluir todos os passos deste tutorial. Tem de instalar o CLI do Azure e o ambiente de desenvolvimento do Docker no seu computador local para concluir este tutorial.

## <a name="get-application-code"></a>Obter o código da aplicação

O exemplo deste tutorial inclui uma aplicação Web simples criada em [Node.js][nodejs]. A aplicação serve uma página HTML estática e tem este aspeto:

![Tutorial de aplicação mostrada no browser][aci-tutorial-app]

Utilize o git para transferir o exemplo:

```bash
git clone https://github.com/Azure-Samples/aci-helloworld.git
```

## <a name="build-the-container-image"></a>Criar a imagem do contentor

O Dockerfile fornecido no repositório de exemplos mostra como é que o contentor é criado. Começa com uma [imagem Node.js oficial][docker-hub-nodeimage] baseada em [Alpine Linux][alpine-linux], uma pequena distribuição adequada para utilização com contentores. Depois, copia os ficheiros da aplicação para o contentor, instala as dependências com o Node Package Manager e, por fim, inicia a aplicação.

```Dockerfile
FROM node:8.9.3-alpine
RUN mkdir -p /usr/src/app
COPY ./app/ /usr/src/app/
WORKDIR /usr/src/app
RUN npm install
CMD node /usr/src/app/index.js
```

Utilize o comando [docker build][docker-build] para criar a imagem de contentor, identificando-a como *aci-tutorial-app*:

```bash
docker build ./aci-helloworld -t aci-tutorial-app
```

O resultado do comando [docker build][docker-build] é semelhante ao seguinte (truncado para legibilidade):

```bash
Sending build context to Docker daemon  119.3kB
Step 1/6 : FROM node:8.9.3-alpine
8.9.3-alpine: Pulling from library/node
88286f41530e: Pull complete
84f3a4bf8410: Pull complete
d0d9b2214720: Pull complete
Digest: sha256:c73277ccc763752b42bb2400d1aaecb4e3d32e3a9dbedd0e49885c71bea07354
Status: Downloaded newer image for node:8.9.3-alpine
 ---> 90f5ee24bee2
...
Step 6/6 : CMD node /usr/src/app/index.js
 ---> Running in f4a1ea099eec
 ---> 6edad76d09e9
Removing intermediate container f4a1ea099eec
Successfully built 6edad76d09e9
Successfully tagged aci-tutorial-app:latest
```

Utilize o comando [docker images][docker-images] para ver a imagem incorporada:

```bash
docker images
```

Saída:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

## <a name="run-the-container-locally"></a>Executar o contentor localmente

Antes de tentar implementar o contentor nas Instâncias do Azure Container, execute-o localmente, para confirmar que está a funcionar. O comutador `-d` permite que o contentor seja executado em segundo plano, ao passo que, com `-p`, pode mapear uma porta arbitrária do computador para a porta 80 do contentor.

```bash
docker run -d -p 8080:80 aci-tutorial-app
```

Abra o browser em http://localhost:8080 para confirmar que o contentor está em execução.

![Executar a aplicação localmente no browser][aci-tutorial-app-local]

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, criou uma imagem de contentor que pode ser implementada em Instâncias do Azure Container. Foram efetuados os seguintes passos:

> [!div class="checklist"]
> * Clonou a origem da aplicação a partir do GitHub
> * Criou imagens de contentor a partir da origem da aplicação
> * Testou o contentor localmente

Avance para o próximo tutorial para saber mais sobre o armazenamento das imagens de contentores em registos do Azure Container Registry.

> [!div class="nextstepaction"]
> [Push images to Azure Container Registry](./container-instances-tutorial-prepare-acr.md) (Enviar imagens para o Azure Container Registry)

<!--- IMAGES --->
[aci-tutorial-app]:./media/container-instances-quickstart/aci-app-browser.png
[aci-tutorial-app-local]: ./media/container-instances-tutorial-prepare-app/aci-app-browser-local.png

<!-- LINKS - External -->
[alpine-linux]: https://alpinelinux.org/
[docker-build]: https://docs.docker.com/engine/reference/commandline/build/
[docker-get-started]: https://docs.docker.com/get-started/
[docker-hub-nodeimage]: https://store.docker.com/images/node
[docker-images]: https://docs.docker.com/engine/reference/commandline/images/
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/
[nodejs]: http://nodejs.org

<!-- LINKS - Internal -->
[azure-cli-install]: /cli/azure/install-azure-cli
