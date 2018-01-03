---
title: "Tutorial de instâncias de contentor do Azure – preparar o registo de contentor do Azure"
description: "Azure instâncias de contentor tutorial parte 2 de 3 – Preparar o registo de contentor do Azure"
services: container-instances
author: neilpeterson
manager: timlt
ms.service: container-instances
ms.topic: tutorial
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: c0aad1f9bbaac9a456b34f75633faba92f57f498
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/02/2018
---
# <a name="deploy-and-use-azure-container-registry"></a>Implementar e utilizar o registo de contentor do Azure

Esta é a parte dois de um tutorial de três partes. No [passo anterior](container-instances-tutorial-prepare-app.md), foi criada uma imagem de contentor para uma aplicação web simples escrita em [Node.js][nodejs]. Neste tutorial, push a imagem para um registo de contentor do Azure. Se não tiver criado a imagem de contentor, regressar à [Tutorial 1 – criar imagem de contentor](container-instances-tutorial-prepare-app.md).

O registo de contentor do Azure é um registo baseado no Azure, privado para imagens de contentor do Docker. Este tutorial explica-lhe como implementar uma instância de registo de contentor do Azure e enviar uma imagem de contentor ao mesmo.

Neste artigo, parte dois da série, poderá:

> [!div class="checklist"]
> * Implemente uma instância de registo de contentor do Azure
> * Marcar uma imagem de contentor para o registo de contentor do Azure
> * Carregue a imagem para o registo

O artigo seguinte, o tutorial final da série implementa o contentor a partir do seu registo privado para instâncias de contentor do Azure.

## <a name="before-you-begin"></a>Antes de começar

Este tutorial requer que está a executar a CLI do Azure versão 2.0.23 ou posterior. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, consulte [instalar o Azure CLI 2.0][azure-cli-install].

Para concluir este tutorial, precisa de um ambiente de desenvolvimento do Docker instalado localmente. Docker fornece pacotes que configurar facilmente Docker em qualquer [Mac][docker-mac], [Windows][docker-windows], ou [Linux] [ docker-linux] sistema.

Shell de nuvem do Azure não inclui os componentes de Docker necessários para concluir cada passo neste tutorial. Tem de instalar o ambiente de desenvolvimento do CLI do Azure e Docker no seu computador local para concluir este tutorial.

## <a name="deploy-azure-container-registry"></a>Implementar o registo de contentor do Azure

Quando implementar um registo de contentor do Azure, primeiro precisa de um grupo de recursos. Um grupo de recursos do Azure é uma coleção lógica na qual os recursos estão implementados e geridos.

Crie um grupo de recursos com o comando [az group create][az-group-create]. Neste exemplo, um grupo de recursos denominado *myResourceGroup* é criado no *eastus* região.

```azurecli
az group create --name myResourceGroup --location eastus
```

Criar um registo de contentor do Azure com o [az acr criar] [ az-acr-create] comando. O nome do registo de contentor **têm de ser exclusivos** no Azure e pode conter carateres alfanuméricos de 5 a 50. Substitua `<acrName>` com um nome exclusivo para o registo:

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic
```

Por exemplo criar um registo de contentor do Azure com o nome *mycontainerregistry082*:

```azurecli
az acr create --resource-group myResourceGroup --name mycontainerregistry082 --sku Basic --admin-enabled true
```

Em todo o resto deste tutorial, utilizamos `<acrName>` como um marcador de posição para o nome do registo de contentor que escolheu.

## <a name="container-registry-login"></a>Início de sessão de registo de contentor

Tem de iniciar sessão sua instância do registo de contentor do Azure antes de enviar imagens à mesma. Utilize o [início de sessão do az acr] [ az-acr-login] comando para concluir a operação. Tem de fornecer o nome único fornecido para o registo do contentor quando o criou.

```azurecli
az acr login --name <acrName>
```

O comando devolve um `Login Succeeded` mensagem depois de concluir.

## <a name="tag-container-image"></a>Imagem de contentor da etiqueta

Para implementar uma imagem de contentor de um registo privada, tem de etiquetar a imagem com o `loginServer` nome do registo.

Para ver uma lista de imagens atuais, utilize o [imagens docker] [ docker-images] comando.

```bash
docker images
```

Saída:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

Para obter o nome de loginServer, execute o [mostrar de acr az] [ az-acr-show] comando. Substitua `<acrName>` com o nome do seu registo de contentor.

```azurecli
az acr show --name <acrName> --query loginServer --output table
```

Exemplo de saída:

```
Result
------------------------
mycontainerregistry082.azurecr.io
```

Etiqueta de *aci-tutorial-aplicação* imagem com o loginServer do seu registo de contentor. Além disso, adicionar `:v1` ao fim do nome de imagem. Esta etiqueta indica o número de versão da imagem. Substitua `<acrLoginServer>` com o resultado do [mostrar de acr az] [ az-acr-show] comando que acabou de ser executado.

```bash
docker tag aci-tutorial-app <acrLoginServer>/aci-tutorial-app:v1
```

Depois de etiquetados, executar `docker images` para verificar a operação.

```bash
docker images
```

Saída:

```bash
REPOSITORY                                                TAG                 IMAGE ID            CREATED             SIZE
aci-tutorial-app                                          latest              5c745774dfa9        39 seconds ago      68.1 MB
mycontainerregistry082.azurecr.io/aci-tutorial-app        v1                  a9dace4e1a17        7 minutes ago       68.1 MB
```

## <a name="push-image-to-azure-container-registry"></a>Imagem de push para o registo de contentor do Azure

Push de *aci-tutorial-aplicação* imagem para o registo com o [docker push] [ docker-push] comando. Substitua `<acrLoginServer>` com o nome do servidor de início de sessão completa obter no passo anterior.

```bash
docker push <acrLoginServer>/aci-tutorial-app:v1
```

O `push` operação deve demorar alguns segundos a alguns minutos, consoante a ligação à internet e o resultado é semelhante ao seguinte:

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

## <a name="list-images-in-azure-container-registry"></a>Lista de imagens no registo de contentor do Azure

Para obter uma lista de imagens que tiver sido feito o push para o registo de contentor do Azure, utilize o [lista de repositório az acr] [ az-acr-repository-list] comando. Atualize o comando com o nome do registo de contentor.

```azurecli
az acr repository list --name <acrName> --output table
```

Saída:

```azurecli
Result
----------------
aci-tutorial-app
```

E, em seguida, para ver as etiquetas para uma imagem específica, utilize o [az acr repositório Mostrar-etiquetas] [ az-acr-repository-show-tags] comando.

```azurecli
az acr repository show-tags --name <acrName> --repository aci-tutorial-app --output table
```

Saída:

```azurecli
Result
--------
v1
```

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, preparado num registo de contentor do Azure para utilização com instâncias de contentor do Azure e enviado uma imagem do contentor no registo. Foram efetuados os seguintes passos:

> [!div class="checklist"]
> * Implementada uma instância de registo de contentor do Azure
> * Etiquetados uma imagem de contentor para o registo de contentor do Azure
> * Carregar uma imagem no registo de contentor do Azure

Avançar para o próximo tutorial para saber mais sobre a implementar o contentor do Azure utilizando as instâncias de contentor do Azure.

> [!div class="nextstepaction"]
> [Implementar contentores para instâncias de contentor do Azure](./container-instances-tutorial-deploy-app.md)

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
