---
title: Tutorial do Kubernetes no Azure - Atualizar uma aplicação
description: Neste tutorial do Azure Kubernetes Service (AKS), saiba como atualizar uma implementação de aplicação existente no AKS com uma nova versão do código da aplicação.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: tutorial
ms.date: 08/14/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: b2dd52fec112b879e072d3ac5598dd7978e68cbc
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/15/2018
ms.locfileid: "41917731"
---
# <a name="tutorial-update-an-application-in-azure-kubernetes-service-aks"></a>Tutorial: Atualizar uma aplicação no Serviço Kubernetes do Azure (AKS)

Depois de a aplicação ser implementada no Kubernetes, pode ser atualizada, ao especificar uma nova imagem de contentor ou uma versão de imagem. Ao fazê-lo, a atualização é testada, para que apenas uma parte da implementação seja atualizada em simultâneo. Esta atualização testada permite que a aplicação continue a ser executada durante a atualização. Também apresenta um mecanismo de reversão se ocorrer uma falha de implementação.

Neste tutorial, parte seis de sete, a aplicação Azure Vote de exemplo é atualizada. Saiba como:

> [!div class="checklist"]
> * Atualizar o código da aplicação de front-end
> * Criar uma imagem de contentor atualizada
> * Colocar a imagem de contentor no Azure Container Registry
> * Implementar a imagem de contentor atualizada

## <a name="before-you-begin"></a>Antes de começar

Nos tutoriais anteriores, foi compactada uma aplicação numa imagem de contentor, carregada a imagem para o Azure Container Registry (ACR) e criado um cluster de Kubernetes. A aplicação foi, em seguida, executada no cluster de Kubernetes.

Também foi clonado um repositório de aplicações que inclui o código de origem da aplicação e foi utilizado um ficheiro do Docker Compose pré-criado neste tutorial. Certifique-se de que criou um clone do repositório e que foram alterados diretórios no diretório clonado. Se ainda não concluiu estes passos e pretende acompanhar, regresse ao [Tutorial 1 – Criar imagens de contentor][aks-tutorial-prepare-app].

Este tutorial requer a execução da versão 2.0.44 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [Install Azure CLI (Instalar o Azure CLI)][azure-cli-install].

## <a name="update-an-application"></a>Atualizar uma aplicação

Vamos fazer uma alteração à aplicação de exemplo e, em seguida, atualizar a versão já implementada no cluster do AKS. O código de origem da aplicação de exemplo pode ser encontrado no interior do diretório *azure-vote*. Abra o ficheiro *config_file.cfg* com um editor, como o `vi`:

```console
vi azure-vote/azure-vote/config_file.cfg
```

Altere os valores de *VOTE1VALUE* e *VOTE2VALUE* para cores diferentes. O exemplo seguinte mostra os valores de cor atualizados:

```
# UI Configurations
TITLE = 'Azure Voting App'
VOTE1VALUE = 'Blue'
VOTE2VALUE = 'Purple'
SHOWHOST = 'false'
```

Guarde e feche o ficheiro.

## <a name="update-the-container-image"></a>Atualizar a imagem de contentor

Para voltar a criar a imagem de front-end e testar a aplicação atualizada, utilize [docker-compose][docker-compose]. O argumento `--build` serve para instruir o Docker Compose para voltar a criar a imagem de aplicação:

```console
docker-compose up --build -d
```

## <a name="test-the-application-locally"></a>Testar a aplicação localmente

Para verificar se a imagem de contentor atualizada mostra as alterações, abra um browser local em http://localhost:8080.

![Imagem do cluster do Kubernetes no Azure no Azure](media/container-service-kubernetes-tutorials/vote-app-updated.png)

Os valores de cor atualizados fornecidos no ficheiro *config_file.cfg* são apresentados na aplicação em execução.

## <a name="tag-and-push-the-image"></a>Etiquetar e enviar a imagem

Para utilizar corretamente a imagem atualizada, etiquete a imagem *azure-vote-front* com o nome do servidor de início de sessão do registo do ACR. Obtenha o nome do servidor de início de sessão com o comando [az acr list](/cli/azure/acr#az_acr_list):

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Utilize a [etiqueta do docker][docker-tag] para etiquetar a imagem. Substitua `<acrLoginServer>` pelo nome do servidor de início de sessão do ACR ou pelo nome de anfitrião do registo público e atualize a versão da imagem para *:v2* da seguinte forma:

```console
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:v2
```

Agora, utilize [docker push][docker-push] para carregar a imagem para o registo. Substitua `<acrLoginServer>` pelo nome do servidor de início de sessão do ACR. Se tiver problemas ao enviar para o seu registo do ACR, certifique-se de que executou o comando [az acr login][az-acr-login].

```console
docker push <acrLoginServer>/azure-vote-front:v2
```

## <a name="deploy-the-updated-application"></a>Implementar a aplicação atualizada

Para garantir o máximo tempo de atividade, tem de executar várias instâncias do pod da aplicação. Verifique se o número de instâncias de front-end em execução com o comando [kubectl get pods][kubectl-get]:

```
$ kubectl get pods

NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-217588096-5w632    1/1       Running   0          10m
azure-vote-front-233282510-b5pkz   1/1       Running   0          10m
azure-vote-front-233282510-dhrtr   1/1       Running   0          10m
azure-vote-front-233282510-pqbfk   1/1       Running   0          10m
```

Se não tiver vários pods de front-end, dimensione a implementação *azure-vote-front* da seguinte forma:

```console
kubectl scale --replicas=3 deployment/azure-vote-front
```

Para atualizar a aplicação, utilize o comando [kubectl set][kubectl-set]. Atualize `<acrLoginServer>` com o nome do anfitrião ou do servidor de início de sessão do registo de contentor e especifique a versão da aplicação *v2*:

```console
kubectl set image deployment azure-vote-front azure-vote-front=<acrLoginServer>/azure-vote-front:v2
```

Para monitorizar a implementação, utilize o comando [kubectl get pod][kubectl-get]. Uma vez que a aplicação atualizada é implementada, os seus pods são terminados e recriados com a nova imagem de contentor.

```console
kubectl get pods
```

O resultado de exemplo seguinte mostra os pods a serem terminados e as novas instâncias em execução à medida que a implementação avança:

```
$ kubectl get pods

NAME                               READY     STATUS        RESTARTS   AGE
azure-vote-back-2978095810-gq9g0   1/1       Running       0          5m
azure-vote-front-1297194256-tpjlg  1/1       Running       0          1m
azure-vote-front-1297194256-tptnx  1/1       Running       0          5m
azure-vote-front-1297194256-zktw9  1/1       Terminating   0          1m
```

## <a name="test-the-updated-application"></a>Testar a aplicação atualizada

Para ver a aplicação atualizada, obtenha primeiro o endereço IP externo do serviço `azure-vote-front`:

```console
kubectl get service azure-vote-front
```

Agora, abra um browser local para o endereço IP.

![Imagem do cluster do Kubernetes no Azure no Azure](media/container-service-kubernetes-tutorials/vote-app-updated-external.png)

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, atualiza uma aplicação e implementa esta atualização num cluster de Kubernetes. Aprendeu a:

> [!div class="checklist"]
> * Atualizar o código da aplicação de front-end
> * Criar uma imagem de contentor atualizada
> * Colocar a imagem de contentor no Azure Container Registry
> * Implementar a imagem de contentor atualizada

Avance para o próximo tutorial para saber como atualizar um cluster do AKS para uma nova versão do Kubernetes.

> [!div class="nextstepaction"]
> [Atualizar Kubernetes][aks-tutorial-upgrade]

<!-- LINKS - external -->
[docker-compose]: https://docs.docker.com/compose/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-set]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#set

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-upgrade]: ./tutorial-kubernetes-upgrade-cluster.md
[az-acr-login]: /cli/azure/acr#az_acr_login
[azure-cli-install]: /cli/azure/install-azure-cli
