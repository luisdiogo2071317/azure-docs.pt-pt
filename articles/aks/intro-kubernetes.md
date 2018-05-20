---
title: Introdução ao Serviço Kubernetes do Azure
description: O Serviço Kubernetes do Azure permite a implementação e gestão fácil de aplicações baseadas em contentores no Azure.
services: container-service
author: gabrtv
manager: timlt
ms.service: container-service
ms.topic: overview
ms.date: 11/13/2017
ms.author: gamonroy
ms.custom: mvc
ms.openlocfilehash: 4936554465fbbed45000f43853a6a77567c3028f
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="introduction-to-azure-kubernetes-service-aks-preview"></a>Introdução à pré-visualização do Serviço Kubernetes do Azure (AKS)

O Serviço Kubernetes do Azure (AKS) simplifica a criação, configuração e gestão de um cluster de máquinas virtuais que estão pré-configuradas para executar aplicações em contentores. Isto permite-lhe utilizar as suas competências ou basear-se num corpo extenso e crescente de conhecimentos da comunidade, para implementar e gerir aplicações baseadas em contentores do Microsoft Azure.

Ao utilizar o AKS, pode tirar partido das funcionalidades de nível empresarial do Azure, mantendo, ao mesmo tempo, a portabilidade da aplicação através de Kubernetes e do formato de imagens do Docker.

> [!IMPORTANT]
> O Serviço Kubernetes do Azure (AKS) está atualmente em **pré-visualização**. As pré-visualizações são tornadas disponíveis para si na condição de concordar com os [termos suplementares de utilização](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Alguns aspetos desta funcionalidade podem alterar-se após a disponibilidade geral (GA).
>

## <a name="managed-kubernetes-in-azure"></a>Managed Kubernetes no Azure

O AKS reduz a complexidade e a sobrecarga operacional de gerir um cluster de Kubernetes ao descarregar grande parte dessa responsabilidade para o Azure. Enquanto serviço alojado do Kubernetes, o Azure lida com tarefas críticas para si, como a monitorização do estado de funcionamento e a manutenção. Além disso, paga apenas os nós de agente nos seus clusters, e não para os mestres. Enquanto serviço gerido do Kubernetes, o AKS oferece:

> [!div class="checklist"]
> * Atualizações de versão do Kubernetes automatizadas e aplicação de patches
> * Dimensionamento simples de clusters
> * Plano de controlo alojado de autorrecuperação (mestres)
> * Reduções de custos - pague apenas os nós do conjunto do agente em execução

Dado que o Azure lida com a gestão dos nós no cluster do AKS, já não tem de realizar muitas tarefas manualmente, como atualizações de cluster. Uma vez que o Azure trata destas tarefas de manutenção crítica por si, o AKS não concede acesso direto (como com SSH) para o cluster.

## <a name="using-azure-kubernetes-service-aks"></a>Utilizar o Serviço Kubernetes do Azure (AKS)
O objetivo do AKS é oferecer um ambiente de alojamento de contentores, com ferramentas e tecnologias open-source que são atualmente populares entre os clientes atuais. Para este fim, expomos os pontos finais da API do Kubernetes padrão. Ao utilizar estes pontos finais padrão, pode tirar partido de qualquer software que consiga comunicar com um cluster do Kubernetes. Por exemplo, pode escolher [kubectl][kubectl-overview], [helm][helm] ou [draft][draft].

## <a name="creating-a-kubernetes-cluster-using-azure-kubernetes-service-aks"></a>Criar um cluster do Kubernetes com o Serviço Kubernetes do Azure (AKS)
Para começar a utilizar o AKS, implemente um cluster do AKS com a [CLI do Azure][aks-quickstart] ou através do portal (procure **Serviço Kubernetes do Azure** no Marketplace). Se for utilizador avançado e precisar de mais controlo sobre os modelos do Azure Resource Manager, utilize o projeto [acs-engine][acs-engine] para criar o seu próprio cluster do Kubernetes personalizado e implementá-lo através da CLI `az`.

### <a name="using-kubernetes"></a>Utilizar Kubernetes
O Kubernetes automatiza a implementação, o dimensionamento e a gestão de aplicações no contentor. Tem um conjunto avançado de funcionalidades, incluindo:
* Empacotamento automático
* Autorrecuperação
* Dimensionamento horizontal
* Deteção do serviço e balanceamento de carga
* Implementações e reversões automáticas
* Gestão de segredos e de configurações
* Orquestração de armazenamento
* Execução de lotes

## <a name="videos"></a>Vídeos

Serviço Kubernetes do Azure (AKS) - Azure Friday, Outubro de 2017:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Container-Orchestration-Simplified-with-Managed-Kubernetes-in-Azure-Container-Service-AKS/player]
>
>

Ferramentas para Programar e Implementar Aplicações no Kubernetes - Azure OpenDev, junho de 2017:

> [!VIDEO https://channel9.msdn.com/Events/AzureOpenDev/June2017/Tools-for-Developing-and-Deploying-Applications-on-Kubernetes/player]
>
>

Saiba mais sobre como implementar e gerir o AKS com o guia de introdução do AKS.

> [!div class="nextstepaction"]
> [Tutorial do AKS][aks-quickstart]

<!-- LINKS - external -->
[acs-engine]: https://github.com/Azure/acs-engine
[draft]: https://github.com/Azure/draft
[helm]: https://helm.sh/
[kubectl-overview]: https://kubernetes.io/docs/user-guide/kubectl-overview/

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md

