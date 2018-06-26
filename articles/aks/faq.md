---
title: Perguntas mais frequentes sobre serviço Kubernetes do Azure
description: Fornece respostas a algumas das perguntas comuns sobre o serviço de Kubernetes do Azure.
services: container-service
author: neilpeterson
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 6/25/2018
ms.author: nepeters
ms.openlocfilehash: 5155d0c85e5b3698b0a13d2d5256a235858f0e82
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938462"
---
# <a name="frequently-asked-questions-about-azure-kubernetes-service-aks"></a>Perguntas mais frequentes sobre o serviço do Azure Kubernetes (AKS)

Endereços este artigo induzirem frequentes perguntas sobre o serviço do Azure Kubernetes (AKS).

## <a name="which-azure-regions-provide-the-azure-kubernetes-service-aks-today"></a>Em que regiões do Azure fornecem o serviço de Kubernetes do Azure (AKS) de hoje em dia?

- Leste da Austrália
- Canadá Central
- Leste do Canadá
- EUA Central
- EUA Leste
- US2 leste
- Europa do Norte
- Reino Unido Sul
- Europa Ocidental
- EUA Oeste
- EUA Oeste 2

## <a name="are-security-updates-applied-to-aks-agent-nodes"></a>Atualizações de segurança são aplicadas a nós de agente AKS?

Azure aplica automaticamente patches de segurança para os nós do cluster com base numa agenda noturna. No entanto, é responsável por assegurar que nós são reiniciados conforme necessário. Tem várias opções para efetuar a reinícios de nó:

- Manualmente, através do portal do Azure ou a CLI do Azure.
- Atualizando o seu cluster AKS. Atualizações do cluster automaticamente [cordon e drenar nós](https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/), em seguida, colocá-los a cópia de segurança com a imagem de Ubuntu mais recente. Atualizar a imagem de SO nos nós sem alterar Kubernetes versões especificando a versão atual do cluster na `az aks upgrade`.
- Utilizar [Kured](https://github.com/weaveworks/kured), um daemon de reinício de open source para Kubernetes. Kured é executada como um [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) e monitoriza a cada nó quanto à presença de um ficheiro com a indicação de que é necessário um reinício. Em seguida, orquestra reinícios no cluster, seguindo o mesmo cordon e o processo de drenagem descrito anteriormente.

## <a name="does-aks-support-node-autoscaling"></a>AKS suporta o dimensionamento automático de nó?

Sim, o dimensionamento automático está disponível através de [Kubernetes autoscaler] [ auto-scaler] partir Kubernetes 1.10.

## <a name="does-aks-support-kubernetes-role-based-access-control-rbac"></a>AKS suporta Kubernetes controlo de acesso baseado em funções (RBAC)?

Sim, o RBAC pode ser ativada quando implementar um cluster AKS do modelo do Azure CLI ou do Azure Resource Manager. Esta funcionalidade ficará em breve no portal do Azure.

## <a name="what-kubernetes-admission-controllers-does-aks-support-can-this-be-configured"></a>Os controladores de admissão Kubernetes suporta AKS? Isto é possível configurar?

AKS suporta as seguintes [controladores admissão][admission-controllers]:

* NamespaceLifecycle
* LimitRanger
* ServiceAccount
* DefaultStorageClass
* DefaultTolerationSeconds
* MutatingAdmissionWebhook 
* ValidatingAdmissionWebhook
* ResourceQuota
* DenyEscalatingExec
* AlwaysPullImages

Não é atualmente possível modificar a lista de controladores de admissão AKS.

## <a name="can-i-deploy-aks-into-my-existing-virtual-network"></a>Pode implementar AKS na minha rede virtual existente?

Sim, pode implementar um cluster AKS para uma rede virtual existente utilizando o [funcionalidade de rede avançada](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/aks/networking-overview.md).

## <a name="is-azure-key-vault-integrated-with-aks"></a>O Cofre de chaves do Azure está integrado com AKS?

AKS não nativamente integrado com o Cofre de chaves do Azure neste momento. No entanto, existem soluções de Comunidade, como [acs-keyvault-agente de Hexadite][hexadite].

## <a name="can-i-run-windows-server-containers-on-aks"></a>Pode executar contentores do Windows Server no AKS?

Para executar os contentores do Windows Server, tem de executar nós baseado no Windows Server. Nós com base no servidor do Windows não estão disponível no AKS neste momento. Se precisar de executar os contentores do Windows Server em Kubernetes no Azure, consulte o [documentação para o motor de acs](https://github.com/Azure/acs-engine/blob/master/docs/kubernetes/windows.md).

## <a name="why-are-two-resource-groups-created-with-aks"></a>Por que razão são criados dois grupos de recursos com AKS?

Cada implementação AKS abrange dois grupos de recursos. O primeiro é criado por si e contém apenas o recurso de serviço Kubernetes. O fornecedor de recursos AKS cria automaticamente um segundo durante a implementação com o nome como *MC_myResourceGroup_myAKSCluster_eastus*. O segundo grupo de recursos contém todos os recursos de infraestrutura associados ao cluster, como VMs, redes e armazenamento. É criado para simplificar a limpeza de recursos.

Se estiver a criar recursos que serão utilizados com o cluster AKS, tais como contas de armazenamento ou endereço IP público reservado, deve colocá-los no grupo de recursos geradas automaticamente.

## <a name="does-aks-offer-a-service-level-agreement"></a>AKS oferecem um contrato de nível de serviço?

Um serviço contrato de nível (SLA), o fornecedor o concordar para reimburse o cliente para o custo do serviço deve o nível de serviço publicadas não ser cumprido. Uma vez que AKS próprio livre, há sem qualquer custo disponível reimburse e assim nenhuma formal SLA. No entanto, iremos procurar o ressarcimento de manter a disponibilidade de, pelo menos, a 99,5% para o servidor de Kubernetes API.

<!-- LINKS - external -->
[auto-scaler]: https://github.com/kubernetes/autoscaler
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent
[admission-controllers]: https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/
