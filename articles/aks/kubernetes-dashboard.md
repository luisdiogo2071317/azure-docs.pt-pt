---
title: Gerir o cluster de Kubernetes do Azure com a IU da web
description: Saiba como utilizar o dashboard de interface do Usuário de web incorporado do Kubernetes com o Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/09/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: e197a251df3f34e5416bafacfd54a3fc7f51d503
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37928221"
---
# <a name="access-the-kubernetes-dashboard-with-azure-kubernetes-service-aks"></a>Aceder ao dashboard do Kubernetes com o Azure Kubernetes Service (AKS)

Kubernetes inclui um dashboard da web que pode ser utilizado para operações de gestão básicas. Este artigo mostra-lhe como aceder ao dashboard do Kubernetes com a CLI do Azure, em seguida, orienta-o de algumas operações básicas do dashboard. Para obter mais informações sobre o dashboard do Kubernetes, consulte [Dashboard de interface do Usuário de Web do Kubernetes][kubernetes-dashboard].

## <a name="before-you-begin"></a>Antes de começar

Os passos detalhados neste documento partem do princípio de que já criou um cluster do AKS e estabeleceu uma `kubectl` ligação com o cluster. Se precisar de criar um cluster do AKS, consulte a [início rápido do AKS][aks-quickstart].

Precisa também da versão 2.0.27 da CLI do Azure ou posterior instalada e configurada. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [Install Azure CLI (Instalar o Azure CLI)][install-azure-cli].

## <a name="start-kubernetes-dashboard"></a>Iniciar o dashboard do Kubernetes

Para iniciar o dashboard do Kubernetes, utilize o [procurar az aks] [ az-aks-browse] comando. O exemplo seguinte abre o dashboard para o cluster com o nome *myAKSCluster* no grupo de recursos com o nome *myResourceGroup*:

```azurecli
az aks browse --resource-group myResourceGroup --name myAKSCluster
```

Este comando cria um proxy entre o sistema de desenvolvimento e a API do Kubernetes e abre um browser para o dashboard do Kubernetes.

### <a name="for-rbac-enabled-clusters"></a>Para clusters habilitados em RBAC

Se o seu cluster do AKS utiliza o RBAC, um *ClusterRoleBinding* tem de ser criado antes de poder aceder o dashboard. Sem um enlace de função, a CLI do Azure devolve um erro semelhante ao seguinte exemplo:

```
error: unable to forward port because pod is not running. Current status=Pending
```

Para criar uma ligação, crie um ficheiro denominado *dashboard admin.yaml* e cole o seguinte exemplo. Este enlace de exemplo não é aplicável a quaisquer componentes de autenticação adicional. Pode utilizar mecanismos, como os tokens de portador ou uma nome de utilizador/palavra-passe para controlar quem pode acessar o dashboard e o que permissões que têm. Para obter mais informações sobre métodos de autenticação, consulte o wiki de dashboard do Kubernetes no [controlos de acesso][dashboard-authentication].

```yaml
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: kubernetes-dashboard
  labels:
    k8s-app: kubernetes-dashboard
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: kubernetes-dashboard
  namespace: kube-system
```

Aplicar o vínculo com o [kubectl aplicam-se] [ kubectl-apply] e especifique seus *dashboard admin.yaml*, conforme mostrado no exemplo a seguir:

```
$ kubectl apply -f dashboard-admin.yaml

clusterrolebinding.rbac.authorization.k8s.io/kubernetes-dashboard created
```

Agora pode aceder ao dashboard do Kubernetes no seu cluster habilitados no RBAC. Para iniciar o dashboard do Kubernetes, utilize o [procurar az aks] [ az-aks-browse] comando conforme detalhado no passo anterior.


## <a name="run-an-application"></a>Executar uma aplicação

No dashboard do Kubernetes, clique a **criar** botão na janela superior direita. Atribua o nome de implementação `nginx` e introduza `nginx:latest` para o nome de imagem de contentor. Sob **serviço**, selecione **externos** e introduza `80` para a porta e a porta de destino.

Quando estiver pronto, clique em **Deploy** para criar a implementação.

![Caixa de diálogo Criar de serviço do Kubernetes](./media/container-service-kubernetes-ui/create-deployment.png)

## <a name="view-the-application"></a>Ver a aplicação

Estado sobre a aplicação pode ser visto no dashboard do Kubernetes. Quando o aplicativo estiver em execução, cada componente tem uma caixa de verificação verde junto ao mesmo.

![Pods do Kubernetes](./media/container-service-kubernetes-ui/complete-deployment.png)

Para ver mais informações sobre os pods de aplicação, clique em **Pods** no menu do lado esquerdo e, em seguida, selecione a **NGINX** pod. Aqui pode ver informações de pod específicas, tais como o consumo de recursos.

![Recursos do Kubernetes](./media/container-service-kubernetes-ui/running-pods.png)

Para encontrar o endereço IP da aplicação, clique em **serviços** no menu do lado esquerdo e, em seguida, selecione a **NGINX** serviço.

![modo de exibição do nginx](./media/container-service-kubernetes-ui/nginx-service.png)

## <a name="edit-the-application"></a>Editar a aplicação

Além de criar e aplicações de visualização, o dashboard de Kubernetes pode ser utilizado para editar e atualizar as implementações de aplicações.

Para editar uma implementação, clique em **implementações** no menu do lado esquerdo e, em seguida, selecione a **NGINX** implementação. Por fim, selecione **editar** na barra de navegação direita superior.

![Edição do Kubernetes](./media/container-service-kubernetes-ui/view-deployment.png)

Localize o `spec.replica` valor, que deve ser 1, altere este valor para 3. Ao fazer isso, a contagem de réplicas da implementação do NGINX aumenta de 1 a 3.

Selecione **atualização** quando pronto.

![Edição do Kubernetes](./media/container-service-kubernetes-ui/edit-deployment.png)

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre o dashboard do Kubernetes, consulte a documentação do Kubernetes.

> [!div class="nextstepaction"]
> [Dashboard de interface do Usuário da Web do Kubernetes][kubernetes-dashboard]

<!-- LINKS - external -->
[kubernetes-dashboard]: https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/
[dashboard-authentication]: https://github.com/kubernetes/dashboard/wiki/Access-control
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
[install-azure-cli]: /cli/azure/install-azure-cli
[az-aks-browse]: /cli/azure/aks#az-aks-browse
