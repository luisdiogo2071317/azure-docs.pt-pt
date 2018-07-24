---
title: Perguntas mais frequentes sobre o Azure Kubernetes Service
description: Fornece respostas para algumas das perguntas comuns sobre o Azure Kubernetes Service.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/20/2018
ms.author: iainfou
ms.openlocfilehash: ea22b33233f85da117de54829e5a16bd7dcab36a
ms.sourcegitcommit: 30221e77dd199ffe0f2e86f6e762df5a32cdbe5f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/23/2018
ms.locfileid: "39205253"
---
# <a name="frequently-asked-questions-about-azure-kubernetes-service-aks"></a>Perguntas mais frequentes sobre o Azure Kubernetes Service (AKS)

Este artigo aborda o frequentes perguntas sobre o Azure Kubernetes Service (AKS).

## <a name="which-azure-regions-provide-the-azure-kubernetes-service-aks-today"></a>Que regiões do Azure fornecem o Azure Kubernetes Service (AKS) hoje em dia?

Veja o Azure Kubernetes Service [regiões e disponibilidade] [ aks-regions] documentação para obter uma lista completa.

## <a name="are-security-updates-applied-to-aks-agent-nodes"></a>Atualizações de segurança são aplicadas a nós de agente do AKS?

Azure aplica automaticamente patches de segurança para os nós do cluster com base numa agenda noturna. No entanto, é responsável por assegurar que nós são reiniciados conforme necessário. Tem várias opções para a execução de reinicializações de nó:

- Manualmente, por meio do portal do Azure ou a CLI do Azure.
- Ao atualizar o seu cluster do AKS. Atualizações do cluster automaticamente [cordão e drenagem de nós](https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/), em seguida, colocá-los a cópia de segurança com a imagem mais recente do Ubuntu. Atualizar a imagem de sistema operacional em seus nós sem alteração da versão do Kubernetes ao especificar a versão atual do cluster em `az aks upgrade`.
- Usando [Kured](https://github.com/weaveworks/kured), um daemon de reinício de código-fonte aberto do Kubernetes. Kured é executado como um [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) e monitoriza cada nó para a presença de um arquivo que indica que é necessário um reinício. Em seguida, orquestra reinicializações no cluster, seguindo a mesma cordon e o processo de drenagem descrito anteriormente.

## <a name="does-aks-support-node-autoscaling"></a>AKS suporta o dimensionamento automático de nó?

Sim, o dimensionamento automático está disponível através da [dimensionamento automático do Kubernetes] [ auto-scaler] no momento da elaboração 1.10 de Kubernetes.

## <a name="does-aks-support-kubernetes-role-based-access-control-rbac"></a>O AKS suporta Kubernetes controlo de acesso baseado em funções (RBAC)?

Sim, o RBAC pode ser ativada quando [implementar um cluster do AKS do modelo da CLI do Azure ou Azure Resource Manager](https://docs.microsoft.com/en-us/azure/aks/aad-integration). Esta funcionalidade chegará em breve no portal do Azure.

## <a name="what-kubernetes-admission-controllers-does-aks-support-can-this-be-configured"></a>Os controladores de admissão do Kubernetes AKS oferece suporte? Isso é possível configurar?

AKS suporta as seguintes [controladores de admissão][admission-controllers]:

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

Não é atualmente possível modificar a lista de controladores de admissão no AKS.

## <a name="can-i-deploy-aks-into-my-existing-virtual-network"></a>Pode implementar AKS em minha rede virtual existente?

Sim, pode implementar um cluster do AKS numa rede virtual existente com o [funcionalidade de rede avançada](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/aks/networking-overview.md).

## <a name="can-i-restrict-the-kubernetes-api-server-to-only-be-accessible-within-my-virtual-network"></a>Posso restringir o servidor de API do Kubernetes para apenas ser acessível em minha rede virtual?

Neste momento, não. O servidor de API do Kubernetes é exposto como um nome de domínio público de completamente qualificado (FQDN). Deve controlar o acesso ao seu cluster através de [Kubernetes com base em função de controlo de acesso (RBAC) e Azure Active Directory (AAD)](https://docs.microsoft.com/en-us/azure/aks/aad-integration).

## <a name="is-azure-key-vault-integrated-with-aks"></a>Azure Key Vault está integrado com o AKS?

AKS não é integrado de forma nativa com o Azure Key Vault neste momento. No entanto, existem soluções de Comunidade, como [o acs-Cofre de chaves-agente de Hexadite][hexadite].

## <a name="can-i-run-windows-server-containers-on-aks"></a>Pode executar contentores do Windows Server no AKS?

Para executar contentores do Windows Server, terá de executar nós com base no Windows Server. Neste momento, o Windows baseado em servidor nós não estão disponíveis no AKS. No entanto, pode usar o Virtual Kubelet agendar contentores do Windows no Azure Container Instances e geri-los como parte do cluster do AKS. Para obter mais informações, consulte [utilização Virtual Kubelet com o AKS][virtual-kubelet].

## <a name="why-are-two-resource-groups-created-with-aks"></a>Por que dois grupos de recursos são criados com o AKS?

Cada implementação do AKS abrange dois grupos de recursos. A primeira é criada por si e contém apenas o recurso de serviço do Kubernetes. O fornecedor de recursos do AKS cria automaticamente o um segundo durante a implementação com um nome como *MC_myResourceGroup_myAKSCluster_eastus*. O segundo grupo de recursos contém todos os recursos de infraestrutura associados ao cluster, como VMs, redes e armazenamento. Ele é criado para simplificar a limpeza de recursos.

Se estiver a criar recursos que serão utilizados com o seu cluster do AKS, como contas de armazenamento ou endereço IP público reservado, deve colocá-los no grupo de recursos gerada automaticamente.

## <a name="does-aks-offer-a-service-level-agreement"></a>O AKS oferece um contrato de nível de serviço?

Num contrato ao nível de serviço, (SLA), o fornecedor concorda em reembolsar o cliente para o custo do serviço deve o nível de serviço publicadas não ser cumprido. Como AKS em si é gratuito, não há nenhum custo disponível para reembolsar e, portanto, não existe SLA formal. No entanto, podemos tentar manter a disponibilidade de, pelo menos, a 99,5% para o servidor de API do Kubernetes.

<!-- LINKS - internal -->

[aks-regions]: ./container-service-quotas.md
[virtual-kubelet]: virtual-kubelet.md

<!-- LINKS - external -->
[auto-scaler]: https://github.com/kubernetes/autoscaler
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent
[admission-controllers]: https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/
