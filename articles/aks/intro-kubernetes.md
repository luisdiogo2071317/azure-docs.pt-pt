---
title: "Introdução ao Azure Container Service para Kubernetes | Microsoft Docs"
description: "O Azure Container Service para Kubernetes permite a implementação e gestão fácil de aplicações baseadas em contentores no Azure."
services: container-service
documentationcenter: 
author: gabrtv
manager: timlt
editor: 
tags: aks, azure-container-service
keywords: "Kubernetes, Docker, Containers, Microsserviços, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/13/2017
ms.author: gamonroy
ms.custom: mvc
ms.openlocfilehash: 9fba9fdda3503ec80fede845466858825e3677a5
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2017
---
# <a name="introduction-to-azure-container-service-aks"></a>Introdução ao Azure Container Service (AKS)

O Azure Container Service (AKS) simplifica a criação, configuração e gestão de um cluster de máquinas virtuais que estão pré-configuradas para executar aplicações em contentores. Isto permite-lhe utilizar as suas competências ou basear-se num corpo extenso e crescente de conhecimentos da comunidade, para implementar e gerir aplicações baseadas em contentores do Microsoft Azure.

Ao utilizar o AKS, pode tirar partido das funcionalidades de nível empresarial do Azure, mantendo, ao mesmo tempo, a portabilidade da aplicação através de Kubernetes e do formato de imagens do Docker.

## <a name="managed-kubernetes-in-azure"></a>Managed Kubernetes no Azure

O AKS reduz a complexidade e a sobrecarga operacional de gerir um cluster de Kubernetes ao descarregar grande parte dessa responsabilidade para o Azure. Enquanto serviço alojado do Kubernetes, o Azure lida com tarefas críticas para si, como a monitorização do estado de funcionamento e a manutenção. Além disso, paga apenas os nós de agente nos seus clusters, e não para os mestres. Enquanto serviço gerido do Kubernetes, o AKS oferece:

> [!div class="checklist"]
> * Atualizações de versão do Kubernetes automatizadas e aplicação de patches
> * Dimensionamento simples de clusters
> * Plano de controlo alojado de autorrecuperação (mestres)
> * Reduções de custos - pague apenas os nós do conjunto do agente em execução

Dado que o Azure lida com a gestão dos nós no cluster do AKS, já não tem de realizar muitas tarefas manualmente, como atualizações de cluster. Uma vez que o Azure trata destas tarefas de manutenção crítica por si, o AKS não concede acesso direto (como com SSH) para o cluster.

## <a name="using-azure-container-service-aks"></a>Utilizar o Azure Container Service (AKS)
O objetivo do AKS é oferecer um ambiente de alojamento de contentores, com ferramentas e tecnologias open-source que são atualmente populares entre os clientes atuais. Para este fim, expomos os pontos finais da API do Kubernetes padrão. Ao utilizar estes pontos finais padrão, pode tirar partido de qualquer software que consiga comunicar com um cluster do Kubernetes. Por exemplo, pode escolher [kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/), [helm](https://helm.sh/) ou [draft](https://github.com/Azure/draft).

## <a name="creating-a-kubernetes-cluster-using-azure-container-service-aks"></a>Criar um cluster do Kubernetes com o Azure Container Service (AKS)
Para começar a utilizar o AKS, implemente um cluster do AKS com a [CLI do Azure](./kubernetes-walkthrough.md) ou através do portal (procure **Azure Container Service** no Marketplace). Se for utilizador avançado e precisar de mais controlo sobre os modelos do Azure Resource Manager, pode utilizar o projeto [acs-engine](https://github.com/Azure/acs-engine) para criar o seu próprio cluster do Kubernetes personalizado e implementá-lo através da CLI `az`.

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

Azure Container Service (AKS) - Azure Friday, Outubro de 2017:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Container-Orchestration-Simplified-with-Managed-Kubernetes-in-Azure-Container-Service-AKS/player]
>
>

Ferramentas para Programar e Implementar Aplicações no Kubernetes - Azure OpenDev, junho de 2017:

> [!VIDEO https://channel9.msdn.com/Events/AzureOpenDev/June2017/Tools-for-Developing-and-Deploying-Applications-on-Kubernetes/player]
>
>

Saiba mais sobre como implementar e gerir o AKS com o guia de introdução do AKS.

> [!div class="nextstepaction"]
> [Tutorial de AKS](./kubernetes-walkthrough.md)