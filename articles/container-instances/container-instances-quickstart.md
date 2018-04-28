---
title: Início Rápido – Criar o seu primeiro contentor do Azure Container Instances
description: Neste manual de início rápido, vai utilizar o Azure CLI para implementar um contentor no Azure Container Instances
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: quickstart
ms.date: 03/19/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: 57961cf1cb64f90cec7d2be90f3fbfe33344467d
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="quickstart-create-your-first-container-in-azure-container-instances"></a>Início Rápido: crie o seu primeiro contentor no Azure Container Instances

O Azure Container Instances facilita a criação e a gestão de contentores do Docker no Azure, sem ter de aprovisionar as máquinas virtuais ou adotar um serviço de nível mais elevado. Neste início rápido, vai criar um contentor no Azure e expõe-lo na Internet com nome de domínio completamente qualificado (FQDN). Esta operação é concluída com um único comando. Dentro de alguns segundos, verá isto no seu browser:

![Aplicação implementada com o Azure Container Instances vista no browser][aci-app-browser]

Se não tiver uma subscrição do Azure, crie uma [conta gratuita][azure-account] antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pode utilizar o Azure Cloud Shell ou uma instalação local da CLI do Azure para concluir este início rápido. Se optar por instalar e usar a CLI localmente, este início rápido requer a execução da versão 2.0.27 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, consulte [Instalar o Azure CLI 2.0][azure-cli-install].

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

As Azure Container Instances, como todos os recursos do Azure, devem ser colocados num grupo de recursos, uma coleção lógica na qual os recursos do Azure são implementados e geridos.

Crie um grupo de recursos com o comando [az group create][az-group-create].

O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na localização *eastus*.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container"></a>Criar um contentor

Para criar um contentor, pode fornecer um nome, uma imagem do Docker e um grupo de recursos do Azure ao comando [az container create][az-container-create]. Opcionalmente, pode expor o contentor na Internet ao especificar uma etiqueta de nome DNS. Neste início rápido, vai implementar um contentor que aloja uma pequena aplicação Web escrita em [Node.js][node-js].

Execute o seguinte comando para iniciar a instância de um contentor. O valor `--dns-name-label` tem de ser exclusivo dentro da região do Azure na qual cria a instância, pelo que poderá precisar de modificar este valor para garantir a exclusividade.

```azurecli-interactive
az container create --resource-group myResourceGroup --name mycontainer --image microsoft/aci-helloworld --dns-name-label aci-demo --ports 80
```

Dentro de alguns segundos, deve obter uma resposta ao seu pedido. Inicialmente, o contentor está no estado **A criar**, mas deverá ser iniciado ao fim de poucos segundos. Pode verificar o estado com o comando [az container show][az-container-show]:

```azurecli-interactive
az container show --resource-group myResourceGroup --name mycontainer --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" --out table
```

Ao executar o comando, são apresentados o nome de domínio completamente qualificado (FQDN) e o estado de aprovisionamento do contentor:

```console
$ az container show --resource-group myResourceGroup --name mycontainer --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" --out table
FQDN                               ProvisioningState
---------------------------------  -------------------
aci-demo.eastus.azurecontainer.io  Succeeded
```

Quando o contentor passar para o estado **Bem-sucedido**, pode aceder ao mesmo no browser ao navegar para o seu FQDN:

![Captura de ecrã do browser a mostrar a aplicação em execução numa instância do contentor do Azure][aci-app-browser]

## <a name="pull-the-container-logs"></a>Extrair os registos de contentor

Pode extrair os registos do contentor que criou com o comando [az container logs][az-container-logs]:

```azurecli-interactive
az container logs --resource-group myResourceGroup --name mycontainer
```

Deverá ver um resultado semelhante ao seguinte:

```console
$ az container logs --resource-group myResourceGroup -n mycontainer
listening on port 80
::ffff:10.240.255.105 - - [15/Mar/2018:21:18:26 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.146 Safari/537.36"
::ffff:10.240.255.105 - - [15/Mar/2018:21:18:26 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://aci-demo.eastus.azurecontainer.io/" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.146 Safari/537.36"
```

## <a name="attach-output-streams"></a>Anexar fluxos de saída

Além de associar os registos, pode anexar o padrão local e os fluxos de erro padrão para o do contentor.

Em primeiro lugar, execute o comando [az container attach][az-container-attach] para anexar a consola local aos fluxos de saída do contentor:

```azurecli-interactive
az container attach --resource-group myResourceGroup -n mycontainer
```

Depois de anexada, atualize o browser algumas vezes para gerar algumas saídas adicionais. Por último, desligue a consola com `Control+C`. Deverá ver um resultado semelhante ao seguinte:

```console
$ az container attach --resource-group myResourceGroup -n mycontainer
Container 'mycontainer' is in state 'Running'...
(count: 1) (last timestamp: 2018-03-15 21:17:59+00:00) pulling image "microsoft/aci-helloworld"
(count: 1) (last timestamp: 2018-03-15 21:18:05+00:00) Successfully pulled image "microsoft/aci-helloworld"
(count: 1) (last timestamp: 2018-03-15 21:18:05+00:00) Created container with id 3534a1e2ee392d6f47b2c158ce8c1808d1686fc54f17de3a953d356cf5f26a45
(count: 1) (last timestamp: 2018-03-15 21:18:06+00:00) Started container with id 3534a1e2ee392d6f47b2c158ce8c1808d1686fc54f17de3a953d356cf5f26a45

Start streaming logs:
listening on port 80
::ffff:10.240.255.105 - - [15/Mar/2018:21:18:26 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.146 Safari/537.36"
::ffff:10.240.255.105 - - [15/Mar/2018:21:18:26 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://aci-demo.eastus.azurecontainer.io/" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.146 Safari/537.36"
::ffff:10.240.255.107 - - [15/Mar/2018:21:18:44 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.146 Safari/537.36"
::ffff:10.240.255.107 - - [15/Mar/2018:21:18:47 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.146 Safari/537.36"
```

## <a name="delete-the-container"></a>Eliminar o contentor

Quando já não precisar do contentor, pode removê-lo com o comando [az container delete][az-container-delete]:

```azurecli-interactive
az container delete --resource-group myResourceGroup --name mycontainer
```

Para verificar se o contentor foi eliminado, execute o comando [az container list](/cli/azure/container#az_container_list):

```azurecli-interactive
az container list --resource-group myResourceGroup --output table
```

O contentor **mycontainer** não deve aparecer na saída do comando. Não se tiver outros contentores no grupo de recursos, não é apresentada nenhuma saída.

## <a name="next-steps"></a>Passos seguintes

Todos os códigos do contentor utilizados neste início rápido estão disponíveis no [GitHub][app-github-repo], juntamente com os respetivos Dockerfiles. Se quiser tentar criá-lo sozinho e implementá-lo no Azure Container Instances com o Azure Container Registry, avance para o tutorial do Azure Container Instances.

> [!div class="nextstepaction"]
> [Azure Container Instances tutorials](./container-instances-tutorial-prepare-app.md) (Tutoriais do Azure Container Instances)

Para experimentar opções para executar contentores num sistema de orquestração no Azure, veja os inícios rápidos [Service Fabric][service-fabric] ou [Azure Container Service (AKS)][container-service].

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png

<!-- LINKS - External -->
[app-github-repo]: https://github.com/Azure-Samples/aci-helloworld.git
[azure-account]: https://azure.microsoft.com/free/?WT.mc_id=A261C142F
[node-js]: http://nodejs.org

<!-- LINKS - Internal -->
[az-container-attach]: /cli/azure/container#az_container_attach
[az-container-create]: /cli/azure/container#az_container_create
[az-container-delete]: /cli/azure/container#az_container_delete
[az-container-list]: /cli/azure/container#az_container_list
[az-container-logs]: /cli/azure/container#az_container_logs
[az-container-show]: /cli/azure/container#az_container_show
[az-group-create]: /cli/azure/group#az_group_create
[azure-cli-install]: /cli/azure/install-azure-cli
[container-service]: ../aks/kubernetes-walkthrough.md
[service-fabric]: ../service-fabric/service-fabric-quickstart-containers.md
