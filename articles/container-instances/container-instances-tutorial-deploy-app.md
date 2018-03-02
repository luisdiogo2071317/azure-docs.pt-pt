---
title: "Tutorial do Azure Container Instances – Implementar a aplicação"
description: "Tutorial do Azure Container Instances parte 3 de 3 – Implementar a aplicação"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: tutorial
ms.date: 02/20/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 250f74b1a05959b93000452c4d5f025311f379d8
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/22/2018
---
# <a name="deploy-a-container-to-azure-container-instances"></a>Implementar um contentor no Azure Container Instances

Este é o último tutorial de uma série de três partes. Nas duas primeiras partes da série [foi criada uma imagem de contentor](container-instances-tutorial-prepare-app.md) e [enviada para um Azure Container Registry](container-instances-tutorial-prepare-acr.md). Este artigo termina a série de tutoriais ao implementar o contentor no Azure Container Instances.

Neste tutorial:

> [!div class="checklist"]
> * Implementar o contentor do Azure Container Registry através da CLI do Azure
> * Ver a aplicação no browser
> * Ver os registos de contentor

## <a name="before-you-begin"></a>Antes de começar

Este tutorial requer a execução da versão 2.0.23 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, consulte [Instalar o Azure CLI 2.0][azure-cli-install].

Para concluir este tutorial, precisa de um ambiente de desenvolvimento do Docker, localmente instalado. O Docker disponibiliza pacotes que o configuram facilmente em qualquer sistema [Mac][docker-mac], [Windows][docker-windows] ou [Linux][docker-linux].

O Azure Cloud Shell não inclui os componentes do Docker necessários para concluir todos os passos deste tutorial. Tem de instalar o CLI do Azure e o ambiente de desenvolvimento do Docker no seu computador local para concluir este tutorial.

## <a name="deploy-the-container-using-the-azure-cli"></a>Implementar o contentor através da CLI do Azure

A CLI do Azure permite a implementação de um contentor no Azure Container Instances com um único comando. Uma vez que a imagem de contentor está alojada no Azure Container Registry privado, terá de incluir as credenciais necessárias para aceder. Obtenha as credenciais com os seguintes comandos da CLI do Azure.

Servidor de início de sessão do registo de contentor (atualize com o nome do seu registo):

```azurecli
az acr show --name <acrName> --query loginServer
```

Palavra-passe do registo de contentor:

```azurecli
az acr credential show --name <acrName> --query "passwords[0].value"
```

Para implementar a imagem de contentor do registo de contentor com um pedido de recurso de 1 núcleo de CPU e 1 GB de memória, execute o seguinte comando. Substitua `<acrLoginServer>` e `<acrPassword>` pelos valores obtidos a com os dois comandos anteriores. Substitua `<acrName>` pelo nome do seu registo de contentor.

```azurecli
az container create --resource-group myResourceGroup --name aci-tutorial-app --image <acrLoginServer>/aci-tutorial-app:v1 --cpu 1 --memory 1 --registry-username <acrName> --registry-password <acrPassword> --dns-name-label aci-demo --ports 80
```

Dentro de alguns segundos, deverá receber uma resposta inicial do Azure Resource Manager. O valor `--dns-name-label` tem de ser exclusivo na região do Azure em que cria a instância de contentor. Atualize o valor no exemplo anterior se receber uma mensagem de erro de **etiqueta de nome DNS** quando executar o comando.

Para ver o estado da implementação, utilize [az container show][az-container-show]:

```azurecli
az container show --resource-group myResourceGroup --name aci-tutorial-app --query instanceView.state
```

Repita o comando [az container show][az-container-show] até que o estado seja alterado de *Pendente* para *Em execução*, o que deve demorar menos de um minuto. Quando o contentor estiver *Em execução*, avance para o passo seguinte.

## <a name="view-the-application-and-container-logs"></a>Ver os registos de aplicações e do contentor

Após a implementação com êxito, apresente o nome de domínio completamente qualificado (FQDN) do contentor com o comando [az container show][az-container-show]:

```bash
az container show --resource-group myResourceGroup --name aci-tutorial-app --query ipAddress.fqdn
```

Exemplo de saída: `"aci-demo.eastus.azurecontainer.io"`

Para ver a aplicação em execução, navegue até ao nome DNS apresentado no seu browser favorito:

![Aplicação Hello World no browser][aci-app-browser]

Também pode ver a saída de registos do contentor:

```azurecli
az container logs --resource-group myResourceGroup --name aci-tutorial-app
```

Saída:

```bash
listening on port 80
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://13.88.176.27/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## <a name="clean-up-resources"></a>Limpar recursos

Se já não precisar dos recursos criados nesta série de tutoriais, poderá executar o comando [az group delete][az-group-delete] para remover o grupo de recursos e todos os recursos nele contidos. Este comando elimina o registo de contentor criado, bem como o contentor em execução, e todos os recursos relacionados.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, concluiu o processo de implementação dos contentores no Azure Container Instances. Foram efetuados os seguintes passos:

> [!div class="checklist"]
> * Foi implementado o contentor do Azure Container Registry através da CLI do Azure
> * Foi visualizada a aplicação no browser
> * Foram visualizados os registos de contentor

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - internal -->
[az-container-show]: /cli/azure/container#az_container_show
[az-group-delete]: /cli/azure/group#az_group_delete
[azure-cli-install]: /cli/azure/install-azure-cli
[prepare-app]: ./container-instances-tutorial-prepare-app.md
