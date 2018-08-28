---
title: Tutorial do Kubernetes no Azure – Implementar uma aplicação
description: Neste tutorial do Azure Kubernetes Service (AKS), implemente uma aplicação de vários contentores no seu cluster com uma imagem personalizada armazenada no Azure Container Registry.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: tutorial
ms.date: 08/14/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: bf817f553250ead449ec0d5db3d33acc2eff23f3
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/15/2018
ms.locfileid: "41924794"
---
# <a name="tutorial-run-applications-in-azure-kubernetes-service-aks"></a>Tutorial: Executar aplicações no Serviço Kubernetes do Azure (AKS)

O Kubernetes dispõe de uma plataforma distribuída para aplicações em contentores. Cria e implementa as suas próprias aplicações e serviços num cluster do Kubernetes, e permite que o cluster gira a disponibilidade e a conectividade. Neste tutorial, parte quatro de sete, a aplicação de exemplo é implementada num cluster do Kubernetes. Saiba como:

> [!div class="checklist"]
> * Atualizar ficheiros de manifesto Kubernetes
> * Executar uma aplicação no Kubernetes
> * Testar a aplicação

Nos tutoriais subsequentes, esta aplicação é aumentada horizontalmente e atualizada.

Este tutorial pressupõe um conhecimento básico dos conceitos do Kubernetes. Para obter informações detalhadas sobre o Kubernetes, veja a [documentação do Kubernetes][kubernetes-documentation].

## <a name="before-you-begin"></a>Antes de começar

Nos tutoriais anteriores, foi compactada uma aplicação numa imagem de contentor, a imagem foi carregada para o Azure Container Registry e foi criado um cluster de Kubernetes.

Para concluir este tutorial, precisa do ficheiro de manifesto previamente criado do Kubernetes `azure-vote-all-in-one-redis.yaml`. Este ficheiro foi transferido com o código de origem da aplicação num tutorial anterior. Certifique-se de que tem um clone do repositório e que foram alterados diretórios no repositório clonado. Se ainda não concluiu estes passos e pretende acompanhar, regresse ao [Tutorial 1 – Criar imagens de contentor][aks-tutorial-prepare-app].

Este tutorial requer a execução da versão 2.0.44 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [Install Azure CLI (Instalar o Azure CLI)][azure-cli-install].

## <a name="update-the-manifest-file"></a>Atualizar o ficheiro de manifesto

Nestes tutoriais, uma instância do Azure Container Registry (ACR) armazena a imagem de contentor no exemplo de aplicação. Para implementar a aplicação, tem de atualizar o nome da imagem no ficheiro de manifesto do Kubernetes para incluir o nome de servidor de início de sessão do ACR.

Obtenha o nome do servidor de início de sessão ACR com o comando [az acr list][az-acr-list], da seguinte forma:

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

O ficheiro de manifesto de exemplo do repositório git clonado no primeiro tutorial utiliza o nome do servidor de início de sessão da *microsoft*. Abra este ficheiro de manifesto com um editor de texto, como `vi`:

```console
vi azure-vote-all-in-one-redis.yaml
```

Substitua *microsoft* pelo seu nome de servidor de início de sessão do ACR. Este nome de imagem foi encontrado na linha 47 do ficheiro de manifesto. O seguinte exemplo mostra o nome da imagem predefinida:

```yaml
containers:
- name: azure-vote-front
  image: microsoft/azure-vote-front:v1
```

Indique o seu próprio nome de servidor de início de sessão do ACR para que o ficheiro de manifesto seja semelhante ao seguinte exemplo:

```yaml
containers:
- name: azure-vote-front
  image: <acrName>.azurecr.io/azure-vote-front:v1
```

Guarde e feche o ficheiro.

## <a name="deploy-the-application"></a>Implementar a aplicação

Para implementar a sua aplicação, utilize o comando [kubectl apply][kubectl-apply]. Este comando analisa o ficheiro de manifesto e cria os objetos de Kubernetes definidos. Especifique o ficheiro de manifesto de exemplo, conforme mostrado no exemplo a seguir:

```console
kubectl apply -f azure-vote-all-in-one-redis.yaml
```

Os objetos de Kubernetes são criados dentro do cluster, conforme mostrado no exemplo a seguir:

```
$ kubectl apply -f azure-vote-all-in-one-redis.yaml

deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-the-application"></a>Testar a aplicação

O [serviço do Kubernetes][kubernetes-service] é criado, o que expõe a aplicação na Internet. Este processo pode demorar alguns minutos. Para monitorizar o progresso, utilize o comando [kubectl get service][kubectl-get] com o argumento `--watch`:

```console
kubectl get service azure-vote-front --watch
```

O *EXTERNAL-IP* do serviço *azure-vote-front* aparece inicialmente como *pendente*, conforme mostrado no exemplo seguinte:

```
azure-vote-front   10.0.34.242   <pending>     80:30676/TCP   7s
```

Quando o endereço *EXTERNAL-IP* muda de *pendente* para um endereço IP real, utilize `CTRL-C` para parar o processo de observação do kubectl. O exemplo seguinte mostra um endereço IP público agora atribuído:

```
azure-vote-front   10.0.34.242   52.179.23.131   80:30676/TCP   2m
```

Para ver a aplicação em ação, abra um browser para o endereço IP externo.

![Imagem do cluster do Kubernetes no Azure no Azure](media/container-service-kubernetes-tutorials/azure-vote.png)

Se o carregamento da aplicação tiver falhado, poderá dever-se a um problema de autorização com o registo de imagem. Para ver o estado dos seus contentores, utilize o comando `kubectl get pods`. Se as imagens de contentor não puderem ser obtidas, veja [permitir acesso ao Container Registry com um segredo do Kubernetes](https://docs.microsoft.com/azure/container-registry/container-registry-auth-aks#access-with-kubernetes-secret).

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, a aplicação do Azure vote foi implementada num cluster de Kubernetes do AKS. Aprendeu a:

> [!div class="checklist"]
> * Atualizar ficheiros de manifesto Kubernetes
> * Executar uma aplicação no Kubernetes
> * Testar a aplicação

Avance para o próximo tutorial para saber como dimensionar uma aplicação Kubernetes e a infraestrutura de Kubernetes subjacente.

> [!div class="nextstepaction"]
> [Dimensionar a aplicação e infraestrutura do Kubernetes][aks-tutorial-scale]

<!-- LINKS - external -->
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-documentation]: https://kubernetes.io/docs/home/
[kubernetes-service]: https://kubernetes.io/docs/concepts/services-networking/service/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-scale]: ./tutorial-kubernetes-scale.md
[az-acr-list]: /cli/azure/acr#list
[azure-cli-install]: /cli/azure/install-azure-cli
