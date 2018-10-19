---
title: Início Rápido - Criar um registo do Docker privado no Azure com a CLI do Azure
description: Aprenda rapidamente a criar um registo do contentor do Docker com a CLI do Azure.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: quickstart
ms.date: 03/03/2018
ms.author: danlep
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: 57c72056b669865278fa8109cd7f4963a1f0887a
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/08/2018
ms.locfileid: "48855243"
---
# <a name="quickstart-create-a-container-registry-using-the-azure-cli"></a>Início Rápido: Criar um registo de contentores com a CLI do Azure

O Azure Container Registry é um serviço de registo do contentor Docker gerido, utilizado para armazenar imagens de contentor do Docker privadas. Este guia detalha a criação de uma instância do Azure Container Registry através da CLI do Azure, o envio de uma imagem de contentor por push para o registo e, por fim, a implementação do contentor a partir do registo no Azure Container Instances (ACI).

Este início rápido requer a execução da versão 2.0.27 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [Install Azure CLI (Instalar o Azure CLI)][azure-cli].

Também tem de ter o Docker instalado localmente. O Docker disponibiliza pacotes que o configuram facilmente em qualquer sistema [macOS][docker-mac], [Windows][docker-windows] ou [Linux][docker-linux].

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um grupo de recursos com o comando [az group create][az-group-create]. Um grupo de recursos do Azure é um contentor lógico no qual os recursos do Azure são implementados e geridos.

O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na localização *eastus*.

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container-registry"></a>Criar um registo de contentores

Neste início rápido, vai criar um registo *Básico*. O Azure Container Registry está disponível em várias SKUs diferentes, descritas brevemente na seguinte tabela. Para obter mais detalhes sobre cada uma, veja [SKUs do registo de contentor][container-registry-skus].

[!INCLUDE [container-registry-sku-matrix](../../includes/container-registry-sku-matrix.md)]

Crie uma instância do ACR com o comando [az acr create][az-acr-create]. O nome do registo tem de ser exclusivo no Azure e pode incluir de 5 a 50 carateres alfanuméricos. No exemplo seguinte, é utilizado *myContainerRegistry007*. Atualize para um valor exclusivo.

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

Em todo o resto deste início rápido, `<acrName>` é um marcador de posição para o nome do registo de contentor.

## <a name="log-in-to-acr"></a>Iniciar sessão no ACR

Antes de emitir e solicitar imagens de contentor, tem de iniciar sessão na instância do ACR. Para tal, utilize o comando [az acr login][az-acr-login].

```azurecli
az acr login --name <acrName>
```

O comando devolve uma mensagem de `Login Succeeded` depois de estar concluído.

## <a name="push-image-to-acr"></a>Enviar imagem para o ACR

Para enviar uma imagem para um registo do Azure Container, primeiro tem de ter uma imagem. Se ainda não tiver quaisquer imagens do contentor local, execute o seguinte comando para solicitar uma imagem existente a partir do Hub do Docker.

```bash
docker pull microsoft/aci-helloworld
```

Antes de emitir uma imagem para o registo, tem de o etiquetar com o nome completamente qualificado do seu servidor de início de sessão ACR. Execute o seguinte comando para obter o nome do servidor de início de sessão completa da instância ACR.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Crie uma etiqueta para a imagem com o comando [etiqueta do docker][docker-tag]. Substitua `<acrLoginServer>` pelo nome do servidor de início de sessão da sua instância do ACR.

```bash
docker tag microsoft/aci-helloworld <acrLoginServer>/aci-helloworld:v1
```

Por último, utilize o [envio do docker][docker-push] para enviar a imagem para a instância do ACR. Substitua `<acrLoginServer>` pelo nome do servidor de início de sessão da sua instância do ACR.

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

O exemplo seguinte lista as etiquetas no repositório **aci-helloworld**.

```azurecli
az acr repository show-tags --name <acrName> --repository aci-helloworld --output table
```

Saída:

```bash
Result
--------
v1
```

## <a name="deploy-image-to-aci"></a>Implementar a imagem no ACI

Para implementar uma instância de contentor do registo que criou, tem de fornecer as credenciais de registo quando a implementar. Em cenários de produção, deve utilizar um [principal de serviço][container-registry-auth-aci] para acesso ao registo do contentor mas, para manter pequeno este início rápido, ative o utilizador administrador no seu registo com o seguinte comando:

```azurecli
az acr update --name <acrName> --admin-enabled true
```

Assim que o administrador estiver ativado, o nome de utilizador é o mesmo do seu nome de registo e pode obter a palavra-passe com este comando:

```azurecli
az acr credential show --name <acrName> --query "passwords[0].value"
```

Para implementar a imagem de contentor com 1 núcleo de CPU e 1 GB de memória, execute o comando seguinte. Substitua `<acrName>`, `<acrLoginServer>` e `<acrPassword>` pelos valores obtidos dos comandos anteriores.

```azurecli
az container create --resource-group myResourceGroup --name acr-quickstart --image <acrLoginServer>/aci-helloworld:v1 --cpu 1 --memory 1 --registry-username <acrName> --registry-password <acrPassword> --dns-name-label aci-demo --ports 80
```

Deve obter uma resposta inicial do Azure Resource Manager com os detalhes sobre o contentor. Para monitorizar o estado do contentor e verificar e ver quando está em execução, repita o [az container show][az-container-show]. Deve demorar menos de um minuto.

```azurecli
az container show --resource-group myResourceGroup --name acr-quickstart --query instanceView.state
```

## <a name="view-the-application"></a>Ver a aplicação

Depois de a implementação no ACI ser bem sucedida, obtenha o FQDN do contentor com o comando [az container show][az-container-show]:

```azurecli
az container show --resource-group myResourceGroup --name acr-quickstart --query ipAddress.fqdn
```

Exemplo de saída: `"aci-demo.eastus.azurecontainer.io"`

Para ver a aplicação em execução, navegue até ao endereço IP público no seu browser favorito.

![Aplicação Hello World no browser][aci-app-browser]

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for necessário, pode utilizar o comando [az group delete][az-group-delete] para remover o grupo de recursos, a instância ACR e todas as imagens do contentor.

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Passos seguintes

Neste início rápido, criou um Azure Container Registry com a CLI do Azure, enviou uma imagem de contentor por push para o registo e iniciou uma instância do mesmo através do Azure Container Instances. Avance para o tutorial do Azure Container Instances para ver mais detalhadamente o ACI.

> [!div class="nextstepaction"]
> [Tutorial do Azure Container Instances][container-instances-tutorial-prepare-app]

<!-- IMAGES> -->
[aci-app-browser]: ../container-instances/media/container-instances-quickstart/aci-app-browser.png


<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - internal -->
[az-acr-create]: /cli/azure/acr#az-acr-create
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-group-create]: /cli/azure/group#az-group-create
[az-group-delete]: /cli/azure/group#az-group-delete
[azure-cli]: /cli/azure/install-azure-cli
[az-container-show]: /cli/azure/container#az-container-show
[container-instances-tutorial-prepare-app]: ../container-instances/container-instances-tutorial-prepare-app.md
[container-registry-skus]: container-registry-skus.md
[container-registry-auth-aci]: container-registry-auth-aci.md
