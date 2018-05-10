---
title: Configuração de rede no serviço do Azure Kubernetes (AKS)
description: Saiba mais sobre a configuração de rede básicas e avançadas no serviço do Azure Kubernetes (AKS).
services: container-service
author: mmacy
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/07/2018
ms.author: marsma
ms.openlocfilehash: 80d12d1f5d6b388c46ed90eb84b7bc00250e17ff
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="network-configuration-in-azure-kubernetes-service-aks"></a>Configuração de rede no serviço do Azure Kubernetes (AKS)

Quando cria um cluster do serviço de Kubernetes do Azure (AKS), pode selecionar entre duas opções de rede: **básico** ou **avançadas**.

## <a name="basic-networking"></a>Funcionamento em rede básico

O **básico** opção de rede é a configuração predefinida para a criação do cluster AKS. A configuração de rede do cluster e o respetivos pods são geridas completamente pelo Azure, não sendo adequada para implementações que não necessitam de configuração de VNet personalizada. Não têm controlo sobre a configuração de rede, tais como atribuídos ao cluster quando seleciona o funcionamento em rede básico de intervalos de endereços de sub-redes ou IP.

Nós num cluster AKS configurada para utilização de rede básica de [kubenet] [ kubenet] Plug-in do Kubernetes.

## <a name="advanced-networking"></a>Funcionamento em rede avançado

**Avançadas** redes coloca os pods no Azure de uma rede Virtual (VNet) configurar, fornecendo-las automática conectividade a recursos de VNet e integração com a avançada conjunto de capacidades que oferta VNets.
Redes avançadas estão atualmente disponível apenas quando implementar AKS clusters no [portal do Azure] [ portal] ou com um modelo do Resource Manager.

Nós num cluster AKS configurada para utilização avançada de rede a [Interface de rede contentor do Azure (CNI)] [ cni-networking] Plug-in do Kubernetes.

![Diagrama que mostra dois nós com pontes ligar cada um para um único Azure VNet][advanced-networking-diagram-01]

## <a name="advanced-networking-features"></a>Funcionalidades de rede avançadas

Redes avançadas fornecem as seguintes vantagens:

* Implementar o seu cluster AKS para uma VNet existente ou crie uma nova VNet e sub-rede para o cluster.
* Cada pod do cluster é atribuído um endereço IP na VNet e pode comunicar diretamente com outros pods no cluster e as outras VMs na VNet.
* Um pod pode ligar a outros serviços numa VNet em modo de peering e redes no local através do ExpressRoute e de site a site (S2S) ligações VPN. Pods também se encontram acessíveis no local.
* Expõe um serviço Kubernetes externamente ou internamente através do Balanceador de carga do Azure. Também é uma funcionalidade do funcionamento em rede básica.
* Pods numa sub-rede com pontos finais de serviço ativados podem ligar de forma segura aos serviços do Azure, por exemplo, armazenamento do Azure e base de dados SQL.
* Utilize definido pelo utilizador rotas (UDR) para encaminhar o tráfego de pods para uma aplicação Virtual de rede.
* Pods podem aceder a recursos na Internet pública. Também é uma funcionalidade do funcionamento em rede básica.

> [!IMPORTANT]
> Cada nó num cluster AKS configurado para redes avançadas podem alojar um máximo de **30 pods**. Cada VNet aprovisionada para utilização com o plug-in Azure CNI está limitada a **4096 endereços IP** (/ 20).

## <a name="configure-advanced-networking"></a>Configurar redes avançadas

Quando lhe [criar um cluster AKS](kubernetes-walkthrough-portal.md) no portal do Azure, são configuráveis para funcionamento em rede avançado os seguintes parâmetros:

**Rede virtual**: A VNet para o qual pretende implementar o cluster Kubernetes. Se pretender criar uma nova VNet para o cluster, selecione *criar nova* e siga os passos a *criar rede virtual* secção.

**Sub-rede**: A sub-rede dentro da VNet onde pretende implementar o cluster. Se pretender criar uma nova subrede na VNet para o cluster, selecione *criar nova* e siga os passos a *criar sub-rede* secção.

**Intervalo de endereços de serviço Kubernetes**: intervalo de endereços IP para o serviço do cluster Kubernetes IPs. Este intervalo não pode estar no intervalo de endereços IP de VNet do cluster.

**O endereço IP do serviço Kubernetes DNS**: O endereço IP para o serviço DNS do cluster. Este endereço tem de estar dentro do *Kubernetes service intervalo de endereços*.

**Endereço de Bridge de docker**: O endereço IP e máscara de rede para atribuir a bridge de Docker. Este endereço IP não tem de estar no intervalo de endereços IP de VNet do cluster.

A seguinte captura de ecrã do portal do Azure mostra um exemplo de como configurar estas definições durante a criação do cluster AKS:

![Avançadas de configuração de rede no portal do Azure][portal-01-networking-advanced]

## <a name="plan-ip-addressing-for-your-cluster"></a>Planear o endereçamento IP para o cluster

Clusters configurados com redes avançadas necessitam de planeamento adicionais. O tamanho da sua VNet e respetiva sub-rede tem de acomodar o número de pods de que pretende executar em simultâneo no cluster, bem como os requisitos de dimensionamento.

Endereços IP para os pods e nós do cluster estão atribuídos a partir da sub-rede especificada dentro da VNet. Cada nó é configurado com um IP principal, qual é o IP do próprio nó e 30 endereços IP adicionais pré-configurados pelo Azure CNI que estão atribuídos a pods agendadas para o nó. Quando ampliar o seu cluster, cada nó do mesmo modo está configurado com endereços IP da sub-rede.

Tal como mencionado anteriormente, cada VNet aprovisionada para utilização com o plug-in Azure CNI está limitada a **4096 endereços IP** (/ 20). Cada nó de um cluster configurado para redes avançadas podem alojar um máximo de **30 pods**.

## <a name="frequently-asked-questions"></a>Perguntas mais frequentes

As seguintes perguntas e respostas aplicam-se para o **avançadas** configuração da rede.

* *Pode configurar redes avançada com a CLI do Azure?*

  Não. Redes avançadas estão atualmente disponível apenas quando implementar AKS clusters no portal do Azure ou com um modelo do Resource Manager.

* *Pode implementar VMs na minha sub-rede de cluster?*

  Não. Implementar VMs na sub-rede utilizada pelo Kubernetes cluster não é suportada. VMs podem ser implementadas na mesma VNet, mas numa sub-rede diferente.

* * Pode configurar políticas de rede por pod?

  Não. Políticas de rede por pod não são atualmente suportadas.

* *É o número máximo de pods implementável para um nó configurável?*

  Por predefinição, cada nó pode alojar um máximo de 30 pods. Atualmente, pode alterar o valor máximo apenas ao modificar o `maxPods` propriedade quando implementar um cluster com um modelo do Resource Manager.

* *Como posso configurar propriedades adicionais para a sub-rede que criar durante a criação do cluster AKS? Por exemplo, pontos finais de serviço.*

  A lista completa das propriedades para a VNet e sub-redes que cria durante a criação do cluster AKS pode ser configurada na página de configuração de VNet padrão no portal do Azure.

## <a name="next-steps"></a>Passos Seguintes

### <a name="networking-in-aks"></a>Funcionamento em rede no AKS

Saiba mais sobre o funcionamento em rede no AKS nos seguintes artigos:

[Utilizar um endereço IP estático com o Balanceador de carga do serviço de Kubernetes do Azure (AKS)](static-ip.md)

[Entrada HTTPS no serviço de contentor do Azure (AKS)](ingress.md)

[Utilizar um balanceador de carga interno com o serviço de contentor do Azure (AKS)](internal-lb.md)

### <a name="acs-engine"></a>Motor de ACS

[Motor do serviço de contentor do Azure (motor de ACS)] [ acs-engine] for um projeto de open source que gera modelos Azure Resource Manager, pode utilizar para implementar clusters preparados para Docker no Azure. Orchestrators Kubernetes, DC/OS, modo Swarm e Swarm podem ser implementados com o motor de ACS.

Clusters de Kubernetes criados com o motor de ACS de suportar o [kubenet] [ kubenet] e [Azure CNI] [ cni-networking] plug-ins. Como tal, os cenários de redes básicos e avançados são suportados pelo motor de ACS.

<!-- IMAGES -->
[advanced-networking-diagram-01]: ./media/networking-overview/advanced-networking-diagram-01.png
[portal-01-networking-advanced]: ./media/networking-overview/portal-01-networking-advanced.png

<!-- LINKS - External -->
[acs-engine]: https://github.com/Azure/acs-engine
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet
[portal]: https://portal.azure.com

<!-- LINKS - Internal -->
[aks-ssh]: aks-ssh.md
