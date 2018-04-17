---
title: Perguntas mais frequentes sobre o serviço de contentor do Azure
description: Fornece respostas a algumas das perguntas comuns sobre o serviço de contentor do Azure.
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 2/14/2018
ms.author: nepeters
ms.openlocfilehash: 5eb949e420d9b055d014b973d452dc95fc358f83
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="frequently-asked-questions-about-azure-container-service-aks"></a>Perguntas mais frequentes sobre o serviço de contentor do Azure (AKS)

Endereços este artigo induzirem frequentes perguntas sobre o serviço de contentor do Azure (AKS).

> [!IMPORTANT]
> O Azure Container Service (AKS) está atualmente em **pré-visualização**. As pré-visualizações são tornadas disponíveis para si na condição de concordar com os [termos suplementares de utilização](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Alguns aspetos desta funcionalidade podem alterar-se após a disponibilidade geral (GA).
>

## <a name="which-azure-regions-provide-the-azure-container-service-aks-today"></a>Em que regiões do Azure fornecem o serviço de contentor do Azure (AKS) de hoje em dia?

- Canadá Central
- Canada Este
- E.U.A. Central
- E.U.A. Leste
- Sudeste Asiático
- Europa Ocidental
- E.U.A. Oeste 2

## <a name="when-will-additional-regions-be-added"></a>Quando serão adicionadas mais regiões?

Regiões adicionais são adicionadas à medida que aumenta a pedido.

## <a name="are-security-updates-applied-to-aks-agent-nodes"></a>Atualizações de segurança são aplicadas a nós de agente AKS?

Azure aplica automaticamente patches de segurança para os nós do cluster com base numa agenda noturna. No entanto, é responsável por assegurar que nós são reiniciados conforme necessário. Tem várias opções para efetuar a reinícios de nó:

- Manualmente, através do portal do Azure ou a CLI do Azure.
- Atualizando o seu cluster AKS. Atualizações do cluster automaticamente [cordon e drenar nós](https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/), em seguida, colocá-los a cópia de segurança com a imagem de Ubuntu mais recente. Atualizar a imagem de SO nos nós sem alterar Kubernetes versões especificando a versão atual do cluster na `az aks upgrade`.
- Utilizar [Kured](https://github.com/weaveworks/kured), um daemon de reinício de open source para Kubernetes. Kured é executada como um [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) e monitoriza a cada nó quanto à presença de um ficheiro com a indicação de que é necessário um reinício. Em seguida, orquestra os reinícios no cluster, seguindo o mesmo cordon e o processo de drenagem descrito anteriormente.

## <a name="do-you-recommend-customers-use-acs-or-aks"></a>É recomendada aos clientes utilizar ACS ou AKS?

Enquanto AKS permanece na pré-visualização, recomendamos a criação de clusters de produção utilizando ACS Kubernetes ou [motor de acs](https://github.com/azure/acs-engine). Utilize AKS para implementações de prova do conceito e ambientes de desenvolvimento/teste.

## <a name="when-will-acs-be-deprecated"></a>Quando os ACS vão ser preteridos?

ACS que vão ser preteridos perto da hora que AKS torna-se depois da disponibilidade geral Terá de 12 meses após essa data para migrar clusters para AKS. Durante o período de 12 meses, pode executar todas as operações de ACS.

## <a name="does-aks-support-node-autoscaling"></a>AKS suporta o dimensionamento automático de nó?

Dimensionamento automático de nó não é suportado, mas é no plano. Pode querer observe isto tenham origem open [implementação de dimensionamento automático][auto-scaler].

## <a name="does-aks-support-kubernetes-role-based-access-control-rbac"></a>AKS suporta Kubernetes controlo de acesso baseado em funções (RBAC)?

Não, RBAC não é atualmente suportado no AKS mas estará disponível brevemente.

## <a name="can-i-deploy-aks-into-my-existing-virtual-network"></a>Pode implementar AKS na minha rede virtual existente?

Não, isto ainda não está disponível mas estará disponível brevemente.

## <a name="is-azure-key-vault-integrated-with-aks"></a>O Cofre de chaves do Azure está integrado com AKS?

Não, não é, mas esta integração é planeada. Entretanto, experimentar as seguintes soluções da [Hexadite][hexadite].

## <a name="can-i-run-windows-server-containers-on-aks"></a>Pode executar contentores do Windows Server no AKS?

Não, AKS atualmente fornecem nós de agente baseado no Windows Server, por isso não é possível executar contentores do Windows Server. Se precisar de executar os contentores do Windows Server em Kubernetes no Azure, consulte o [documentação para o motor de acs](https://github.com/Azure/acs-engine/blob/master/docs/kubernetes/windows.md).

## <a name="why-are-two-resource-groups-created-with-aks"></a>Por que razão são criados dois grupos de recursos com AKS?

Cada implementação AKS abrange dois grupos de recursos. O primeiro é criado por si e contém apenas o recurso AKS. O fornecedor de recursos AKS cria automaticamente um segundo durante a implementação com o nome como *MC_myResourceGRoup_myAKSCluster_eastus*. O segundo grupo de recursos contém todos os recursos de infraestrutura associados ao cluster, como VMs, redes e armazenamento. É criado para simplificar a limpeza de recursos.

Se estiver a criar recursos que serão utilizados com o cluster AKS, tais como contas de armazenamento ou endereço IP público reservado, deve colocá-los no grupo de recursos geradas automaticamente.

<!-- LINKS - external -->
[auto-scaler]: https://github.com/kubernetes/autoscaler
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent