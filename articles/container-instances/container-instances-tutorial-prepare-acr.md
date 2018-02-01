---
title: "Tutorial do Azure Container Instances – Preparar o Azure Container Registry"
description: "Tutorial do Azure Container Instances, parte 2 de 3 – Preparar o Azure Container Registry"
services: container-instances
author: neilpeterson
manager: timlt
ms.service: container-instances
ms.topic: tutorial
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 94ecba44b8281460da4518c146aab814d2eaa850
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/22/2018
---
# <a name="deploy-and-use-azure-container-registry"></a>Implementar e utilizar o Azure Container Registry

Esta é a parte dois de um tutorial de três partes. No [passo anterior](container-instances-tutorial-prepare-app.md), foi criada uma imagem de contentor para uma aplicação Web simples escrita em [Node.js][nodejs]. Neste tutorial, envia a imagem para um Azure Container Registry. Se não tiver criado a imagem de contentor, regresse ao [Tutorial 1 – Criar imagem de contentor](container-instances-tutorial-prepare-app.md).

O Azure Container Registry é um registo privado e baseado no Azure para imagens de contentor do Docker. Este tutorial orienta-o na implementação de uma instância do Azure Container Registry e no envio de uma imagem de contentor para a mesma.

Neste artigo, parte dois da série, vai:

> [!div class="checklist"]
> * Implementar uma instância do Azure Container Registry
> * Marcar uma imagem de contentor para o seu registo de contentor do Azure
> * Carregar a imagem para o seu registo

No próximo artigo, o último tutorial da série, vai implementar o contentor do seu registo privado para o Azure Container Instances.

## <a name="before-you-begin"></a>Antes de começar

Este tutorial requer a execução da versão 2.0.23 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, consulte [Instalar o Azure CLI 2.0][azure-cli-install].

Para concluir este tutorial, precisa de um ambiente de desenvolvimento do Docker, localmente instalado. O Docker disponibiliza pacotes que o configuram facilmente em qualquer sistema [Mac][docker-mac], [Windows][docker-windows] ou [Linux][docker-linux].

O Azure Cloud Shell não inclui os componentes do Docker necessários para concluir todos os passos deste tutorial. Tem de instalar o CLI do Azure e o ambiente de desenvolvimento do Docker no seu computador local para concluir este tutorial.

## <a name="deploy-azure-container-registry"></a>Implementar o Azure Container Registry

Ao implementar um Azure Container Registry, tem de começar por obter um grupo de recursos. Um grupo de recursos do Azure é uma coleção lógica na qual os recursos do Azure são implementados e geridos.

Crie um grupo de recursos com o comando [az group create][az-group-create]. Neste exemplo, é criado um grupo de recursos chamado *myResourceGroup* na região *eastus*.

```azurecli
az group create --name myResourceGroup --location eastus
```

Crie um registo de contentor do Azure com o comando [az acr create][az-acr-create]. O nome do registo de contentor tem de ser exclusivo no Azure e pode incluir de 5 a 50 carateres alfanuméricos. Substitua `<acrName>` por um nome exclusivo para o seu registo:

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic
```

Por exemplo, para criar um registo de contentor do Azure com o nome *mycontainerregistry082*:

```azurecli
az acr create --resource-group myResourceGroup --name mycontainerregistry082 --sku Basic --admin-enabled true
```

Em todo o resto deste tutorial, utilizamos `<acrName>` como um marcador de posição para o nome do registo de contentor que escolheu.

## <a name="container-registry-login"></a>Iniciar sessão no registo de contentor

Tem de iniciar sessão na sua instância do Azure Container Registry antes de enviar imagens para a mesma. Utilize o comando [az acr login][az-acr-login] para concluir a operação. Tem de fornecer o nome exclusivo que forneceu para o registo de contentor quando o criou.

```azurecli
az acr login --name <acrName>
```

O comando devolve uma mensagem de `Login Succeeded` depois de estar concluído.

## <a name="tag-container-image"></a>Imagem de contentor

Para implementar uma imagem de contentor a partir de um registo privado, tem de marcar a imagem com o nome `loginServer` do registo.

Para ver uma lista das imagens atuais, utilize o comando [docker images][docker-images].

```bash
docker images
```

Saída:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

Para obter o nome loginServer, execute o comando [az acr show][az-acr-show] comando. Substitua `<acrName>` pelo nome do seu registo de contentor.

```azurecli
az acr show --name <acrName> --query loginServer --output table
```

Exemplo de saída:

```
Result
------------------------
mycontainerregistry082.azurecr.io
```

Marque a imagem *aci-tutorial-app* com o loginServer do seu registo de contentor. Além disso, adicione `:v1` ao final do nome da imagem. Esta etiqueta identifica o número da versão da imagem. Substitua `<acrLoginServer>` pelo resultado do comando [az acr show][az-acr-show] que acabou de executar.

```bash
docker tag aci-tutorial-app <acrLoginServer>/aci-tutorial-app:v1
```

Depois de marcada, execute `docker images` para verificar a operação.

```bash
docker images
```

Saída:

```bash
REPOSITORY                                                TAG                 IMAGE ID            CREATED             SIZE
aci-tutorial-app                                          latest              5c745774dfa9        39 seconds ago      68.1 MB
mycontainerregistry082.azurecr.io/aci-tutorial-app        v1                  a9dace4e1a17        7 minutes ago       68.1 MB
```

## <a name="push-image-to-azure-container-registry"></a>Enviar imagens para o Azure Container Registry

Envie a imagem *aci-tutorial-app* para o registo com o comando [docker push][docker-push]. Substitua `<acrLoginServer>` pelo nome completo de servidor de início de sessão que obteve no passo anterior.

```bash
docker push <acrLoginServer>/aci-tutorial-app:v1
```

A operação `push` deverá demorar alguns segundos a alguns minutos, consoante a sua ligação à Internet, e os resultados são semelhantes ao seguinte:

```bash
The push refers to a repository [mycontainerregistry082.azurecr.io/aci-tutorial-app]
3db9cac20d49: Pushed
13f653351004: Pushed
4cd158165f4d: Pushed
d8fbd47558a8: Pushed
44ab46125c35: Pushed
5bef08742407: Pushed
v1: digest: sha256:ed67fff971da47175856505585dcd92d1270c3b37543e8afd46014d328f05715 size: 1576
```

## <a name="list-images-in-azure-container-registry"></a>Listar imagens no Azure Container Registry

Para devolver uma lista de imagens que foram enviadas para o seu registo de contentor do Azure, utilize o comando [az acr repository list][az-acr-repository-list]. Atualize o comando com o nome do registo de contentor.

```azurecli
az acr repository list --name <acrName> --output table
```

Saída:

```azurecli
Result
----------------
aci-tutorial-app
```

Em seguida, para ver as etiquetas de uma imagem específica, utilize o comando [az acr repository show-tags][az-acr-repository-show-tags].

```azurecli
az acr repository show-tags --name <acrName> --repository aci-tutorial-app --output table
```

Saída:

```azurecli
Result
--------
v1
```

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, preparou um Azure Container Registry para utilização com o Azure Container Instances e enviou uma imagem de contentor para o registo. Foram efetuados os seguintes passos:

> [!div class="checklist"]
> * Implementou uma instância do Azure Container Registry
> * Marcou uma imagem de contentor para o Azure Container Registry
> * Carregou uma imagem para o Azure Container Registry

Avance para o próximo tutorial para saber mais sobre implementar o contentor para o Azure através do Azure Container Instances.

> [!div class="nextstepaction"]
> [Implementar contentores para o Azure Container Instances](./container-instances-tutorial-deploy-app.md)

<!-- LINKS - External -->
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
[az-acr-create]: /cli/azure/acr#az_acr_create
[az-acr-login]: /cli/azure/acr#az_acr_login
[az-acr-repository-list]: /cli/azure/acr/repository#az_acr_list
[az-acr-repository-show-tags]: /cli/azure/acr/repository#az_acr_repository_show_tags
[az-acr-show]: /cli/azure/acr#az_acr_show
[az-group-create]: /cli/azure/group#az_group_create
[azure-cli-install]: /cli/azure/install-azure-cli
