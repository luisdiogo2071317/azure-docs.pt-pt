---
title: "Perguntas mais frequentes sobre o serviço de contentor do Azure"
description: "Fornece respostas a algumas das perguntas comuns sobre o serviço de contentor do Azure."
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 2/01/2018
ms.author: nepeters
ms.openlocfilehash: 73c49510512c9148f4fee98423b14770fa8602b9
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2018
---
# <a name="frequently-asked-questions-about-azure-container-service-aks"></a>Perguntas mais frequentes sobre o serviço de contentor do Azure (AKS)

Endereços este artigo induzirem frequentes perguntas sobre o serviço de contentor do Azure (AKS).

## <a name="which-azure-regions-will-have-azure-container-service-aks"></a>Em que regiões do Azure terá o serviço de contentor do Azure (AKS)? 

- Canadá Central 
- Leste do Canadá 
- EUA Central 
- EUA Leste 
- Sudeste Asiático 
- Europa Ocidental 
- EUA Oeste 2 

## <a name="when-will-additional-regions-be-added"></a>Quando serão adicionadas mais regiões? 

Regiões adicionais são adicionadas à medida que aumenta a pedido.

## <a name="are-security-updates-applied-to-aks-nodes"></a>Atualizações de segurança são aplicadas a nós AKS? 

OS patches de segurança são aplicadas a nós do cluster com base numa agenda noturna, no entanto, não é efetuada uma reinicialização. Se for necessário, poderão ser reiniciados nós através do portal ou a CLI do Azure. Ao atualizar um cluster, a imagem de Ubuntu mais recente é utilizada e todos os patches de segurança são aplicadas (com um reinício).

## <a name="do-you-recommend-customers-use-acs-or-akss"></a>É recomendada aos clientes utilizar ACS ou AKSs? 

Dado que o serviço de contentor do Azure (AKS) será GA numa data posterior, recomendamos que crie da PoC, desenvolvimento e teste clusters no AKS mas clusters de produção no ACS Kubernetes.  

## <a name="when-will-acs-be-deprecated"></a>Quando os ACS vão ser preteridos? 

ACS que vão ser preteridos perto da hora que AKS torna-se depois da disponibilidade geral Terá de 12 meses após essa data para migrar clusters para AKS. Durante o período de 12 meses, pode executar todas as operações de ACS.

## <a name="does-aks-support-node-autoscaling"></a>AKS suporta o dimensionamento automático de nó? 

Dimensionamento automático de nó não é suportado, mas é no plano. Pode querer observe isto tenham origem open [implementação de dimensionamento automático][auto-scaler].

## <a name="why-are-two-resource-groups-created-with-aks"></a>Por que razão são criados dois grupos de recursos com AKS? 

Cada cluster do serviço de contentor do Azure (AKS) está contida em dois grupos de recursos. O primeiro é criado por si e contém apenas o recurso AKS. O segundo grupo de recursos é automaticamente criada durante a implementação e contém todos os recursos de infraestrutura de cluster como VMs, redes e recursos de armazenamento. Este grupo de recursos é criado para a limpeza de recursos mais fácil. 

O grupo de recursos criado de automática tem um nome semelhante a:

```
MC_myResourceGRoup_myAKSCluster_eastus
```

Ao adicionar os recursos do Azure para ser utilizado com o cluster Kubernetes, tais como contas de armazenamento ou endereço IP público reservado, estes recursos têm de ser criados na automática criou o grupo de recursos.   

## <a name="is-azure-key-vault-integrated-with-aks"></a>O Cofre de chaves do Azure está integrado com AKS? 

Não, não é, mas esta integração é planeada. Entretanto, pode experimentar as seguintes soluções da [Hexadite][hexadite]. 

<!-- LINKS - external -->
[auto-scaler]: https://github.com/kubernetes/autoscaler
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent  