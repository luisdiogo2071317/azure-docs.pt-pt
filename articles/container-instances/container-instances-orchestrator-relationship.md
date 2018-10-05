---
title: O Azure Container Instances e orquestração de contentores
description: Compreenda como o Azure container instâncias interagem com os orquestradores de contentor.
services: container-instances
author: seanmck
ms.service: container-instances
ms.topic: article
ms.date: 10/05/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: c17bdb5a81640a7162ae735a4633a31cdfffbb1d
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48803516"
---
# <a name="azure-container-instances-and-container-orchestrators"></a>O Azure Container Instances e orquestradores de contentor

Devido a seu tamanho de pequeno e orientação da aplicação, os contentores são bastante adequados para ambientes de entrega ágil e arquiteturas baseadas em microsserviços. A tarefa de automatizar e gerir um grande número de contentores e como eles interagem é conhecida como *orquestração*. Orquestradores de contentores populares incluem o Kubernetes, DC/OS e Docker Swarm.

O Azure Container Instances oferece algumas das capacidades básicas de agendamento de plataformas de orquestração. E enquanto não abrange os serviços de maior valor que fornecem essas plataformas, o Azure Container Instances pode ser complementar aos mesmos. Este artigo descreve o escopo do que lida com Azure Container Instances e cheio de orquestradores de contentor podem interagir com ele.

## <a name="traditional-orchestration"></a>Orquestração tradicional

A definição padrão de orquestração inclui as seguintes tarefas:

- **Agendamento**: devido uma imagem de contentor e um pedido de recurso, encontrar uma máquina adequada para executar o contentor.
- **Afinidade/anti-script-affinity**: Especifique que um conjunto de contentores deve ser executado nas proximidades entre si (para o desempenho) ou até o momento suficientemente diferença (para disponibilidade).
- **Monitorização de estado de funcionamento**: Assista para falhas de contentor e automaticamente reagendá-los.
- **Ativação pós-falha**: controlar o que está a ser executado em cada máquina e reagendar contentores a partir de máquinas com falhas para nós em bom estado.
- **Dimensionamento**: Adicionar ou remover instâncias de contentor para corresponder a pedido, manual ou automaticamente.
- **Funcionamento em rede**: forneça uma rede de sobreposição para a coordenação de contentores para comunicar em várias máquinas de host.
- **Deteção do serviço**: ativa contentores localizar uns aos outros automaticamente, mesmo quando eles mover entre computadores anfitrião e alterar os endereços IP.
- **As atualizações de aplicações de coordenada**: Gerir atualizações de contentor para evitar períodos de indisponibilidade de aplicação e ativar a reversão, caso algo corra mal.

## <a name="orchestration-with-azure-container-instances-a-layered-approach"></a>Orquestração com o Azure Container Instances: uma abordagem em camadas

Azure Container Instances permite uma abordagem em camadas de orquestração, fornecendo todas as capacidades de agendamento e gestão necessárias para executar um único contentor, permitindo que plataformas do orchestrator gerir as tarefas de vários contentores com base no mesmo.

Uma vez que a infraestrutura subjacente para as instâncias de contentor é gerida pelo Azure, uma plataforma do orchestrator não precisa de preocupar com a localização de uma máquina de anfitrião adequado para executar um único contentor. A elasticidade da cloud garante que uma está sempre disponível. Em vez disso, o orchestrator pode concentrar-se nas tarefas que simplificam o desenvolvimento de arquiteturas de vários contentores, incluindo o dimensionamento e atualizações coordenadas.

## <a name="potential-scenarios"></a>Cenários potenciais

Embora seja ainda ser a integração do orchestrator com o Azure Container Instances, prevemos que alguns ambientes diferentes podem surgir:

### <a name="orchestration-of-container-instances-exclusively"></a>Orquestração do contentor de instâncias exclusivamente

Porque eles começar rapidamente e são faturadas ao segundo, um ambiente com base exclusivamente no Azure Container Instances oferece a forma mais rápida para começar a utilizar e lidar com cargas de trabalho altamente variáveis.

### <a name="combination-of-container-instances-and-containers-in-virtual-machines"></a>Combinação de instâncias de contentor e contentores em máquinas virtuais

Para cargas de trabalho de longa execução, estáveis, orquestração de contentores num cluster de máquinas virtuais dedicadas, é normalmente mais barato do que executar os mesmos contentores com o Azure Container Instances. No entanto, as instâncias de contentor proporcionam uma excelente solução para rapidamente expandir e contratar sua capacidade geral para lidar com picos inesperados ou curta duração de utilização.

Em vez de aumentar horizontalmente o número de máquinas virtuais no seu cluster, em seguida, implementar contentores adicionais para essas máquinas, o orchestrator pode simplesmente agendar os contentores adicionais no Azure Container Instances e eliminá-los quando não estiverem mais necessário.

## <a name="sample-implementation-virtual-kubelet-for-kubernetes"></a>Implementação de exemplo: Virtual Kubelet para Kubernetes

O [Virtual Kubelet] [ aci-connector-k8s] projeto demonstra como integrar as plataformas de orquestração de contentores com o Azure Container Instances.

Virtual Kubelet imita do Kubernetes [kubelet] [ kubelet-doc] registrando-se como um nó com capacidade ilimitada e expedir a criação de [pods] [ pod-doc] como grupos de contentores no Azure Container Instances.

Poderiam ser compilados conectores para outros orquestradores que da mesma forma se integram com primitivos de plataforma para combinar o poder do orquestrador API com a velocidade e a simplicidade da gestão de contentores no Azure Container Instances.

## <a name="next-steps"></a>Passos Seguintes

Crie seu primeiro contentor com o Azure Container Instances utilizando o [guia de início rápido](container-instances-quickstart.md).

<!-- IMAGES -->

<!-- LINKS -->
[aci-connector-k8s]: https://github.com/virtual-kubelet/virtual-kubelet/tree/master/providers/azure
[kubelet-doc]: https://kubernetes.io/docs/admin/kubelet/
[pod-doc]: https://kubernetes.io/docs/concepts/workloads/pods/pod/