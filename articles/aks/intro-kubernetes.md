---
title: Introdução ao Serviço Kubernetes do Azure
description: O Serviço Kubernetes do Azure permite a implementação e gestão fácil de aplicações baseadas em contentores no Azure.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: overview
ms.date: 06/13/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 294ccd70e0339ed5ad68022f002e4864fc52b452
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/30/2018
ms.locfileid: "43286725"
---
# <a name="azure-kubernetes-service-aks"></a>Serviço de Kubernetes do Azure (AKS)

O Azure Kubernetes Service (AKS) torna fácil implementar um cluster Kubernetes gerido no Azure. O AKS reduz a complexidade e a sobrecarga operacional de gerir o Kubernetes ao descarregar grande parte dessa responsabilidade para o Azure. Enquanto serviço alojado do Kubernetes, o Azure lida com tarefas críticas para si, como a monitorização do estado de funcionamento e a manutenção. Além disso, o serviço é gratuito, paga apenas os nós de agente nos seus clusters, e não para os mestres.

Este documento fornece uma descrição geral das funcionalidades do Azure Kubernetes Service (AKS).

## <a name="flexible-deployment-options"></a>Opções de implementação flexível

O Azure Kubernetes Service oferece opções de implementação orientadas por portal, linha de comandos e modelo (modelo do Resource Manager e Terraform). Ao implementar um cluster AKS, o mestre Kubernetes e todos os nós são implementados e configurados para si. As funcionalidades adicionais, como redes avançadas, integração do Azure Active Directory e monitorização também podem ser configuradas durante o processo de implementação.

Para obter mais informações, consulte os artigos [Início rápido de portal do AKS][aks-portal] ou [Início rápido da CLI do AKS][aks-cli].

## <a name="identity-and-security-management"></a>Gestão de identidade e segurança

Os clusters AKS suportam o [Controlo de Acesso Baseado em Funções (RBAC)][kubernetes-rbac]. Um cluster AKS também pode ser configurado para integração com o Azure Active Directory. Nesta configuração, o acesso ao Kubernetes pode ser configurado com base na identidade e associação de grupo do Azure Active Directory.

Para obter mais informações, veja [Integrar o Azure Active Directory com o AKS][aks-aad].

## <a name="integrated-logging-and-monitoring"></a>Registo e monitorização integrados

O estado de funcionamento do contentor dá-lhe a visibilidade do desempenho, ao recolher métricas de memória e processador dos contentores, nós e controladores. Os registos do contentor também são recolhidos. Estes dados são armazenados na sua área de trabalho do Log Analytics e estão disponíveis através do portal do Azure, CLI do Azure ou um ponto final REST.

Para obter mais informações, veja [Monitorizar o estado de funcionamento do contentor do Azure Kubernetes Service][container-health].

## <a name="cluster-node-scaling"></a>Dimensionamento de nó de cluster

À medida que aumenta a necessidade de recursos, os nós de um cluster AKS podem ser aumentados horizontalmente para corresponder. Se a procura de recursos diminuir, os nós podem ser removidos ao reduzir horizontalmente o cluster. As operações de dimensionamento do AKS podem ser concluídas com o portal do Azure ou a CLI do Azure.

Para obter mais informações, veja [Dimensionar um cluster do Azure Kubernetes Service (AKS)][aks-scale].

## <a name="cluster-node-upgrades"></a>Atualizações do nó de cluster

O Azure Kubernetes Service oferece várias versões de Kubernetes. À medida que novas versões ficam disponíveis no AKS, a versão do seu cluster pode ser atualizada através do portal do Azure ou da CLI do Azure. Durante o processo de atualização, os nós são cuidadosamente isolados e drenados para minimizar a interrupção das aplicações em execução.

Para obter mais informações, veja [Atualizar um cluster do Azure Kubernetes Service (AKS)][aks-upgrade].

## <a name="http-application-routing"></a>Encaminhamento de aplicações de HTTP

A solução de Encaminhamento de Aplicações de HTTP torna mais fácil aceder às aplicações implementadas no cluster AKS. Quando ativada, a solução de encaminhamento de aplicação de HTTP configura um controlador de entradas no seu cluster AKS. À medida que as aplicações são implementadas, os nomes DNS publicamente acessíveis são configurados automaticamente.

Para obter mais informações, veja [Encaminhamento de aplicações de HTTP][aks-http-routing].

## <a name="gpu-enabled-nodes"></a>Nós de GPU ativados

O AKS suporta a criação de conjuntos de nós ativados por GPU. O Azure oferece atualmente VMs ativadas por GPU, únicas ou várias. As VMs ativadas por GPU foram concebidas para cargas de trabalho de computação intensiva, gráficos intensivos e visualização.

Para obter mais informações, veja [Utilizar GPUs no AKS][aks-gpu].

## <a name="development-tooling-integration"></a>Integração de ferramentas de desenvolvimento

O Kubernetes oferece um ecossistema rico de ferramentas de desenvolvimento e gestão, como o Helm, Draft e a extensão do Kubernetes para o Visual Studio Code. Estas ferramentas funcionam de forma totalmente integrada com o Azure Kubernetes Service.

Adicionalmente, os Espaços de Programador do Azure oferecem uma experiência de desenvolvimento do Kubernetes rápida e iterativa para equipas. Com a configuração mínima, pode executar e depurar contentores diretamente do Azure Kubernetes Service (AKS).

Para obter mais informações, veja [Espaços de Programador do Azure][azure-dev-spaces].

O projeto Azure DevOps fornece uma solução simples para trazer código existente e repositório Git para o Azure. O projeto de DevOps cria automaticamente recursos do Azure, como o AKS, um pipeline de versão no VSTS, que inclui uma definição de compilação para CI, configura uma definição de versão para CD e, em seguida, cria um recurso do Azure Application Insights para monitorização.

Para obter mais informações, veja [Projeto Azure DevOps][azure-devops].

## <a name="virtual-network-integration"></a>Integração da rede virtual

Um cluster AKS pode ser implementado numa VNet existente. Nesta configuração, a cada pod do cluster é atribuído um endereço IP na VNet, e pode comunicar diretamente com outros pods no cluster e outros nós na VNet. Os pods também podem ligar a outros serviços numa VNet em modo de peering e a redes no local através do ExpressRoute e ligações de redes VPN.

Para obter mais informações, veja [Descrição geral de redes AKS][aks-networking].

## <a name="private-container-registry"></a>Registo de contentor privado

Integre com o Azure Container Registry (ACR) para armazenamento privado das suas imagens de Docker.

Para obter mais informações, veja [Azure Container Registry (ACR)][acr-docs].

## <a name="storage-volume-support"></a>Suporte para volume de armazenamento

O Azure Kubernetes Service (AKS) suporta volumes de armazenamento de montagem para dados persistentes. Os clusters AKS são criados com suporte para Ficheiros do Azure e Discos do Azure.

Para obter mais informações, veja [Ficheiros do Azure][azure-files] e [Discos do Azure][azure-disk].

## <a name="docker-image-support"></a>Suporte para imagem do Docker

O Azure Kubernetes Service (AKS) suporta o formato de imagem do Docker.

## <a name="kubernetes-certification"></a>Certificação Kubernetes

O Azure Kubernetes Service (AKS) foi certificado com CNCF como sendo compatível com o Kubernetes.

## <a name="regulatory-compliance"></a>Conformidade regulamentar

O Azure Kubernetes Service (AKS) está em conformidade com SOC, ISO e PCI DSS.

## <a name="next-steps"></a>Passos seguintes

Saiba mais sobre como implementar e gerir o AKS com o guia de introdução do AKS.

> [!div class="nextstepaction"]
> [Início Rápido do AKS][aks-cli]

<!-- LINKS - external -->
[acs-engine]: https://github.com/Azure/acs-engine
[draft]: https://github.com/Azure/draft
[helm]: https://helm.sh/
[kubectl-overview]: https://kubernetes.io/docs/user-guide/kubectl-overview/
[kubernetes-rbac]: https://kubernetes.io/docs/reference/access-authn-authz/rbac/

<!-- LINKS - internal -->
[acr-docs]: ../container-registry/container-registry-intro.md
[aks-aad]: ./aad-integration.md
[aks-cli]: ./kubernetes-walkthrough.md
[aks-gpu]: ./gpu-cluster.md
[aks-http-routing]: ./http-application-routing.md
[aks-networking]: ./networking-overview.md
[aks-portal]: ./kubernetes-walkthrough-portal.md
[aks-scale]: ./scale-cluster.md
[aks-upgrade]: ./upgrade-cluster.md
[azure-dev-spaces]: https://docs.microsoft.com/en-us/azure/dev-spaces/azure-dev-spaces
[azure-devops]: https://docs.microsoft.com/en-us/azure/devops-project/overview
[azure-disk]: ./azure-disks-dynamic-pv.md
[azure-files]: ./azure-files-dynamic-pv.md
[container-health]: ../monitoring/monitoring-container-health.md

