---
title: Orquestração de contentor e instâncias de contentor do Azure
description: Compreenda como Azure contentor de instâncias interajam com orchestrators do contentor.
services: container-instances
author: seanmck
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 03/23/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: e1455cba004facfa03dca21544eec754f5dc60be
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="azure-container-instances-and-container-orchestrators"></a>Instâncias de contentor do Azure e orchestrators de contentor

Devido ao respetivo tamanho pequeno e a orientação de aplicação, os contentores são adequados para ambientes de entrega seja ágil e arquiteturas de microsserviço. A tarefa de automatizar e gerir um grande número de contentores e como eles interagem é conhecida como *orchestration*. Orchestrators contentor populares incluem Kubernetes, DC/OS e Docker Swarm.

Instâncias de contentor do Azure fornece algumas das funcionalidades básicas agendamento das plataformas de orquestração. E, apesar de não abrange os serviços do valor mais alto que fornecem essas plataformas, instâncias de contentor do Azure pode ser complementares aos mesmos. Este artigo descreve o âmbito do que processa as instâncias de contentor do Azure e como completa orchestrators contentor podem interagir com ele.

## <a name="traditional-orchestration"></a>Orquestração tradicional

A definição do padrão de orquestração inclui as seguintes tarefas:

- **Agendamento**: dada uma imagem de contentor e um pedido de recurso, localizar uma máquina adequada para executar o contentor.
- **Afinidade/Anti-affinity**: especificar que um conjunto de contentores deverão ser executadas próximas entre si (para um desempenho) ou suficientemente até que ponto apart (para disponibilidade).
- **Monitorização de estado de funcionamento**: ver para as falhas de contentor e automaticamente reprogramá-los.
- **Ativação pós-falha**: monitorizar o que está a ser executado em cada máquina e Reprogramar contentores máquinas falhada a nós em bom estado.
- **Dimensionamento**: Adicionar ou remover instâncias de contentor para corresponder a pedido, manualmente ou automaticamente.
- **Rede**: forneça uma rede de sobreposição para coordenar a contentores para comunicar entre várias máquinas de anfitrião.
- **Deteção do serviço**: Ativar contentores localizar entre si automaticamente, mesmo que tal como mover entre computadores anfitriões e alterar os endereços IP.
- **Coordenado as atualizações de aplicações**: Gerir atualizações do contentor para evitar períodos de indisponibilidade de aplicações e ativar a reversão se algo não bate certo.

## <a name="orchestration-with-azure-container-instances-a-layered-approach"></a>Orquestração com instâncias de contentor do Azure: uma abordagem em camadas

Instâncias de contentor do Azure permite uma abordagem em camadas a orquestração, fornecendo todas as capacidades de agendamento e de gestão necessárias para executar um único contentor, permitindo que as plataformas do orchestrator gerir as tarefas de contentor multi seguir.

Porque a infraestrutura subjacente para instâncias de contentor é gerida pelo Azure, uma plataforma do orchestrator não precisa de concern próprio com localizar uma máquina de anfitrião adequado para executar um único contentor. A elasticidade da nuvem garante que uma está sempre disponível. Em vez disso, o orchestrator pode concentrar-se nas tarefas que simplificam o desenvolvimento de arquiteturas de contentor multi, incluindo o dimensionamento e atualizações coordenadas.

## <a name="potential-scenarios"></a>Potenciais cenários

Enquanto a integração do orchestrator com instâncias de contentor do Azure é ainda nascent, iremos prevê que podem surgir alguns ambientes diferentes:

### <a name="orchestration-of-container-instances-exclusively"></a>Orquestração do contentor de instâncias de forma exclusiva

Uma vez que iniciar rapidamente e cobrar pelo segundo, um ambiente com base exclusivamente nas instâncias de contentor do Azure oferece a forma mais rápida para começar a utilizar e para lidar com cargas de trabalho altamente variáveis.

### <a name="combination-of-container-instances-and-containers-in-virtual-machines"></a>Combinação de instâncias de contentor e contentores nas máquinas virtuais

Para cargas de trabalho de longa execução, estáveis, da orquestração contentores num cluster de máquinas virtuais dedicadas é normalmente mais barata que executar os mesmos contentores com instâncias de contentor do Azure. No entanto, as instâncias de contentor oferecem uma excelente solução para rápida expansão e contracting a capacidade de lidar com picos inesperados ou curta duração em utilização geral.

Em vez de aumentar horizontalmente o número de máquinas virtuais no seu cluster, em seguida, implementar contentores adicionais dessas máquinas, o orchestrator pode simplesmente agendar os contentores adicionais em instâncias de contentor do Azure e eliminá-las quando já não estiverem necessária.

## <a name="sample-implementation-azure-container-instances-connector-for-kubernetes"></a>Implementação de exemplo: conector de instâncias de contentor do Azure para Kubernetes

Para demonstrar como plataformas de orquestração do contentor podem integrar com instâncias de contentor do Azure, ter iniciamos edifício um [conector de exemplo para Kubernetes][aci-connector-k8s].

O conector para Kubernetes mimics o [kubelet] [ kubelet-doc] ao registar como um nó com capacidade ilimitada e emissão a criação de [pods] [ pod-doc] como grupos de contentor em instâncias de contentor do Azure.

Pode ser criados conectores para outras orchestrators que se integram da mesma forma primitivos de plataforma para combinar o poder do orchestrator API com a velocidade e na simplicidade da gestão de contentores em instâncias de contentor do Azure.

> [!WARNING]
> O conector do ACI para Kubernetes *experimental* e não deve ser utilizado em produção.

## <a name="next-steps"></a>Passos Seguintes

Criar o contentor do primeiro com instâncias de contentor Azure utilizando o [guia de introdução ao](container-instances-quickstart.md).

<!-- IMAGES -->

<!-- LINKS -->
[aci-connector-k8s]: https://github.com/virtual-kubelet/virtual-kubelet/tree/master/providers/azure
[kubelet-doc]: https://kubernetes.io/docs/admin/kubelet/
[pod-doc]: https://kubernetes.io/docs/concepts/workloads/pods/pod/