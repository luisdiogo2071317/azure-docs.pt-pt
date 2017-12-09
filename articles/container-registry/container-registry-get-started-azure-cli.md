---
title: "Guia de introdução - criar um registo de Docker privado no Azure com a CLI do Azure"
description: Saiba mais rapidamente criar um registo de contentor do Docker privado com a CLI do Azure.
services: container-registry
author: neilpeterson
manager: timlt
ms.service: container-registry
ms.topic: quicksart
ms.date: 12/07/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: f31f4e5e2b3fe5db85873894a7f2fa9c415392c1
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2017
---
# <a name="create-a-container-registry-using-the-azure-cli"></a>Criar um registo de contentores com a CLI do Azure

O Azure Container Registry é um serviço de registo do contentor Docker gerido, utilizado para armazenar imagens de contentor do Docker privadas. Este detalhes guia criar uma instância de registo de contentor Azure utilizando a CLI do Azure.

Este guia de introdução requer que está a executar a CLI do Azure versão 2.0.21 ou posterior. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, consulte [instalar o Azure CLI 2.0][azure-cli].

Também tem de ter Docker instalado localmente. Docker fornece pacotes que configurar facilmente Docker em qualquer [Mac][docker-mac], [Windows][docker-windows], ou [Linux] [ docker-linux] sistema.

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um grupo de recursos com o comando [az group create][az-group-create]. Um grupo de recursos do Azure é um contentor lógico no qual os recursos do Azure são implementados e geridos.

O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na localização *eastus*.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container-registry"></a>Criar um registo de contentores

Este guia de introdução, criamos um *básico* registo. Registo de contentor do Azure está disponível em vários SKUs diferentes, brevemente descritos na seguinte tabela. Para obter detalhes expandidos em cada um, consulte [registo de contentor SKUs][container-registry-skus].

[!INCLUDE [container-registry-sku-matrix](../../includes/container-registry-sku-matrix.md)]

Criar uma instância ACR utilizando o [az acr criar] [ az-acr-create] comando.

O nome do registo **têm de ser exclusivos**. No exemplo seguinte *myContainerRegistry007* é utilizado. Atualize esta para um valor exclusivo.

```azurecli
az acr create --resource-group myResourceGroup --name myContainerRegistry007 --sku Basic
```

Quando o registo é criado, o resultado é semelhante ao seguinte:

```json
{
  "adminUserEnabled": false,
  "creationDate": "2017-09-08T22:32:13.175925+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry007",
  "location": "eastus",
  "loginServer": "myContainerRegistry007.azurecr.io",
  "name": "myContainerRegistry007",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

Em todo o resto deste guia de introdução, utilizamos `<acrName>` como um marcador de posição para o nome do registo de contentor.

## <a name="log-in-to-acr"></a>Inicie sessão no ACR

Antes de emitir e solicitar imagens de contentor, tem de iniciar sessão na instância do ACR. Para tal, utilize o [início de sessão do az acr] [ az-acr-login] comando.

```azurecli
az acr login --name <acrName>
```

O comando devolve um `Login Succeeded` mensagem depois de concluir.

## <a name="push-image-to-acr"></a>Imagem de push para o ACR

Para enviar uma imagem para um registo de contentor do Azure, primeiro tem de ter uma imagem. Se ainda não tiver quaisquer imagens do contentor de locais, execute o seguinte comando para solicitar uma imagem existente a partir do Hub de Docker.

```bash
docker pull microsoft/aci-helloworld
```

Pode emitir uma imagem para o registo, tem de o Etiquetar com o nome completamente qualificado do seu servidor de início de sessão ACR. Execute o seguinte comando para obter o nome do servidor de início de sessão completa da instância ACR.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Marcar a imagem a utilizar o [tag de docker] [ docker-tag] comando. Substitua `<acrLoginServer>` com o nome do servidor de início de sessão da sua instância ACR.

```bash
docker tag microsoft/aci-helloworld <acrLoginServer>/aci-helloworld:v1
```

Por último, utilize [docker push] [ docker-push] para enviar a imagem para a instância ACR. Substitua `<acrLoginServer>` com o nome do servidor de início de sessão da sua instância ACR.

```bash
docker push <acrLoginServer>/aci-helloworld:v1
```

## <a name="list-container-images"></a>Listar imagens de contentor

O exemplo seguinte lista os repositórios de um registo:

```azurecli
az acr repository list --name <acrName> --output table
```

Saída:

```bash
Result
----------------
aci-helloworld
```

O exemplo seguinte lista as etiquetas no **aci olámundo** repositório.

```azurecli
az acr repository show-tags --name <acrName> --repository aci-helloworld --output table
```

Saída:

```bash
Result
--------
v1
```

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não é necessário, pode utilizar o [eliminação do grupo de az] [ az-group-delete] comando para remover o grupo de recursos, instância ACR e todas as imagens de contentor.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Passos seguintes

Este guia de introdução, criou um registo de contentor do Azure com a CLI do Azure. Se gostaria de utilizar o registo de contentor do Azure com instâncias de contentor do Azure, continue para o tutorial de instâncias de contentor do Azure.

> [!div class="nextstepaction"]
> [Tutorial de instâncias de contentor do Azure][container-instances-tutorial-prepare-app]

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - internal -->
[az-acr-create]: /cli/azure/acr#az_acr_create
[az-acr-login]: /cli/azure/acr#az_acr_login
[az-group-create]: /cli/azure/group#az_group_create
[az-group-delete]: /cli/azure/group#az_group_delete
[azure-cli]: /cli/azure/install-azure-cli
[container-instances-tutorial-prepare-app]: ../container-instances/container-instances-tutorial-prepare-app.md
[container-registry-skus]: container-registry-skus.md