---
title: Descrição geral do Azure Container Instances
description: Compreender o Azure Container Instances
services: container-instances
author: seanmck
manager: jeconnoc
ms.service: container-instances
ms.topic: overview
ms.date: 11/30/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: b5f97dac202edd5405c51b75581275166042ac38
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52841460"
---
# <a name="azure-container-instances"></a>Azure Container Instances

Os contentores estão a tornar-se na forma preferida de empacotar, implementar e gerir aplicações na cloud. O Azure Container Instances é a forma mais rápida e simples de executar um contentor no Azure, sem que tenha de gerir máquinas virtuais e sem que tenha de adotar um serviço de nível superior.

O Azure Container Instances é uma ótima solução para qualquer cenário que possa funcionar em contentores isolados, incluindo aplicações simples, automatização de tarefas e tarefas de compilação. Em cenários nos quais precisa de orquestração de contentores completa, incluindo deteção de serviços em vários contentores, dimensionamento automático e atualizações coordenadas de aplicações, recomendamos o [Serviço Kubernetes do Azure (AKS)](../aks/index.yml).

## <a name="fast-startup-times"></a>Tempos de arranque rápidos

Os contentores oferecem benefícios de arranque significativos em relação às máquinas virtuais (VMs). Com o Azure Container Instances, pode iniciar contentores no Azure em segundos, sem ter de aprovisionar e gerir VMs.

## <a name="public-ip-connectivity-and-dns-name"></a>Conectividade de IP público e nome DNS

O Azure Container Instances permite expor os seus contentores diretamente na Internet com um endereço IP e um nome de domínio completamente qualificado (FQDN). Quando cria uma instância de contentor, pode especificar uma etiqueta de nome DNS personalizada para que a aplicação esteja acessível em *customlabel*.*azureregion*.azurecontainer.io.

## <a name="hypervisor-level-security"></a>Segurança ao nível do hipervisor

Historicamente, os contentores têm vindo a oferecer isolamento de dependências de aplicações e governação de recursos, mas não têm sido considerados suficientemente fortalecidos para utilização multi-inquilinos hostil. O Azure Container Instances garante que a sua aplicação está tão isolada num contentor como estaria numa VM.

## <a name="custom-sizes"></a>Tamanhos personalizados

Normalmente, os contentores são otimizados para executarem apenas uma única aplicação, mas as necessidades exatas de cada uma dessas aplicações podem ser bastante diferentes. O Azure Container Instances proporciona uma utilização ideal ao permitir especificações exatas de núcleos de CPU e memória. A faturação é ao segundo e consoante o que precisar, para que possa ajustar os gastos com base nas suas necessidades reais.

Para as tarefas de computação intensiva, como a aprendizagem automática, Azure Container Instances permite agendar contentores do Linux utilizar NVIDIA Tesla [recursos GPU](container-instances-gpu.md) (pré-visualização).

## <a name="persistent-storage"></a>Armazenamento persistente

Para obter e persistir o estado com o Azure Container Instances, ofereceremos a [montagem direta de partilhas de ficheiros do Azure](container-instances-mounting-azure-files-volume.md).

## <a name="linux-and-windows-containers"></a>Contentores de Linux e Windows

O Azure Container Instances permite agendar contentores do Windows e do Linux com a mesma API. Basta especificar o tipo de SO quando cria os [grupos de contentor](container-instances-container-groups.md).

Algumas funcionalidades estão atualmente restritas para contentores Linux. Enquanto trabalhamos para trazer paridade de funcionalidades para os contentores do Windows, pode encontrar as diferenças da plataforma atual em [Quotas e disponibilidade das regiões do Azure Container Instances](container-instances-quotas.md).

O Azure Container Instances suporta imagens do Windows com base nas versões do Canal de Serviço a Longo Prazo (LTSC). As versões do Canal Semianual (SAC) do Windows, como 1709 e 1803, não são suportadas.

## <a name="co-scheduled-groups"></a>Grupos agendados conjuntamente

O Azure Container Instances suporta o agendamento de [grupos de vários contentores](container-instances-container-groups.md) que partilhem um computador anfitrião, a rede local, o armazenamento e o ciclo de vida. Desta forma, pode combinar o contentor da aplicação principal com outros contentores que têm um papel secundário, tais como sidecars de registo.

## <a name="virtual-network-deployment-preview"></a>Implementação da rede virtual (pré-visualização)

Atualmente em pré-visualização, esta funcionalidade do Azure Container Instances permite [implementar instâncias de contentor numa rede virtual do Azure](container-instances-vnet.md). Ao implementar instâncias de contentor numa sub-rede da sua rede virtual, estes podem comunicar em segurança com outros recursos na rede virtual, incluindo os que estão no local (através da [gateway de VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md) ou de [ ExpressRoute](../expressroute/expressroute-introduction.md)).

> [!IMPORTANT]
> Algumas funcionalidades do Azure Container Instances estão em pré-visualização e algumas [limitações aplicam-se](container-instances-vnet.md#preview-limitations). As pré-visualizações ser-lhe-ão disponibilizadas na condição de concordar com os [termos suplementares de utilização][terms-of-use]. Alguns aspetos destas funcionalidades podem ser alterados antes da disponibilidade geral (GA).

## <a name="next-steps"></a>Passos Seguintes

Experimente implementar um contentor no Azure com um único comando através do nosso guia de início rápido:

> [!div class="nextstepaction"]
> [Início Rápido do Azure Container Instances](container-instances-quickstart.md)

<!-- LINKS - External -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
