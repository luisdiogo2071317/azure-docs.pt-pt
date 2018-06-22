---
title: Configuração de rede no serviço do Azure Kubernetes (AKS)
description: Saiba mais sobre a configuração de rede básicas e avançadas no serviço do Azure Kubernetes (AKS).
services: container-service
author: mmacy
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/15/2018
ms.author: marsma
ms.openlocfilehash: 207accc30e10c4e2bed5b713fc59e2f9ad86a876
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309849"
---
# <a name="network-configuration-in-azure-kubernetes-service-aks"></a>Configuração de rede no serviço do Azure Kubernetes (AKS)

Quando cria um cluster do serviço de Kubernetes do Azure (AKS), pode selecionar entre duas opções de rede: **básico** ou **avançadas**.

## <a name="basic-networking"></a>Funcionamento em rede básico

O **básico** opção de rede é a configuração predefinida para a criação do cluster AKS. A configuração de rede do cluster e o respetivos pods são geridas completamente pelo Azure, não sendo adequada para implementações que não necessitam de configuração de VNet personalizada. Não têm controlo sobre a configuração de rede, tais como atribuídos ao cluster quando seleciona o funcionamento em rede básico de intervalos de endereços de sub-redes ou IP.

Nós num cluster AKS configurada para utilização de rede básica de [kubenet] [ kubenet] Plug-in do Kubernetes.

## <a name="advanced-networking"></a>Funcionamento em rede avançado

**Avançadas** redes coloca os pods no Azure de uma rede Virtual (VNet) configurar, fornecendo-las automática conectividade a recursos de VNet e integração com a avançada conjunto de capacidades que oferta VNets.
Redes avançada estão disponível quando implementar AKS clusters com o [portal do Azure][portal], CLI do Azure, ou com um modelo do Resource Manager.

Nós num cluster AKS configurada para utilização avançada de rede a [Interface de rede contentor do Azure (CNI)] [ cni-networking] Plug-in do Kubernetes.

![Diagrama que mostra dois nós com pontes ligar cada um para um único Azure VNet][advanced-networking-diagram-01]

## <a name="advanced-networking-features"></a>Funcionalidades de rede avançadas

Redes avançadas fornecem as seguintes vantagens:

* Implementar o seu cluster AKS para uma VNet existente ou crie uma nova VNet e sub-rede para o cluster.
* Cada pod do cluster é atribuído um endereço IP na VNet e pode comunicar diretamente com outros pods no cluster e outros nós na VNet.
* Um pod pode ligar a outros serviços numa VNet em modo de peering e redes no local através do ExpressRoute e de site a site (S2S) ligações VPN. Pods também se encontram acessíveis no local.
* Expõe um serviço Kubernetes externamente ou internamente através do Balanceador de carga do Azure. Também é uma funcionalidade do funcionamento em rede básica.
* Pods numa sub-rede com pontos finais de serviço ativados podem ligar de forma segura aos serviços do Azure, por exemplo, armazenamento do Azure e base de dados SQL.
* Utilize definido pelo utilizador rotas (UDR) para encaminhar o tráfego de pods para uma aplicação Virtual de rede.
* Pods podem aceder a recursos na Internet pública. Também é uma funcionalidade do funcionamento em rede básica.

> [!IMPORTANT]
> Cada nó num cluster AKS configurado para redes avançadas podem alojar um máximo de **30 pods** quando configurado através do portal do Azure.  Pode alterar o valor máximo apenas ao modificar a propriedade maxPods quando implementar um cluster com um modelo do Resource Manager. Cada VNet aprovisionada para utilização com o plug-in Azure CNI está limitada a **4096 endereços IP configurados**.

## <a name="advanced-networking-prerequisites"></a>Avançadas rede pré-requisitos

* A VNet para o cluster AKS deve permitir a ligação à internet de saída.
* Não crie mais de um cluster AKS na mesma sub-rede.
* Funcionamento em rede avançado para AKS não suporta as VNets que utilizam zonas de DNS do Azure privado.
* Não é possível utilizar AKS clusters `169.254.0.0/16`, `172.30.0.0/16`, ou `172.31.0.0/16` para o Kubernetes service intervalo de endereços.
* O principal de serviço utilizado para o cluster AKS tem de ter `Contributor` permissões para o grupo de recursos que contém a VNet existente.

## <a name="plan-ip-addressing-for-your-cluster"></a>Planear o endereçamento IP para o cluster

Clusters configurados com redes avançadas necessitam de planeamento adicionais. O tamanho da sua VNet e respetiva sub-rede deve suportar tanto o número de pods de que pretende executar, bem como o número de nós do cluster.

Endereços IP para os pods e nós do cluster estão atribuídos a partir da sub-rede especificada dentro da VNet. Cada nó é configurado com um IP principal, que é o IP o nó e 30 endereços IP adicionais pré-configurados pelo Azure CNI que estão atribuídos a pods agendadas para o nó. Quando ampliar o seu cluster, cada nó do mesmo modo está configurado com endereços IP da sub-rede.

O plano de endereço IP para um cluster AKS é composta por uma VNet, pelo menos uma subrede para nós e pods e um intervalo de endereços de serviço Kubernetes.

| Intervalo de endereços / Azure recursos | Limites e dimensionamento |
| --------- | ------------- |
| Rede virtual | VNet do Azure pode ser tão grande como /8 mas poderá apenas 4096 configurar os endereços IP. |
| Subrede | Tem de ser suficientemente grande para acomodar os nós e Pods. Para calcular o tamanho mínimo da sub-rede: (número de nós) + (número de nós * Pods por nó). Para um cluster de 50 nó: (50) + (50 * 30) = 1,550, a sub-rede seria têm de ser um /21 ou superior. |
| Intervalo de endereços de serviço Kubernetes | Este intervalo não deve ser utilizado por qualquer elemento de rede no ou ligado nesta VNet. Endereço do serviço CIDR tem de ser menor que /12. |
| Endereço IP do serviço Kubernetes DNS | Endereço IP do Kubernetes serviço intervalo de endereços que será utilizado pela deteção do serviço de cluster (kube dns). |
| Endereço de bridge de docker | Endereço IP (em notação CIDR) utilizado como a bridge de Docker endereço IP em nós. Predefinição de 172.17.0.1/16. |

Tal como mencionado anteriormente, cada VNet aprovisionada para utilização com o plug-in Azure CNI está limitada a **4096 endereços IP configurados**. Cada nó de um cluster configurado para redes avançadas podem alojar um máximo de **30 pods**.

## <a name="deployment-parameters"></a>Parâmetros de implementação

Quando criar um cluster AKS, os seguintes parâmetros são configuráveis para funcionamento em rede avançado:

**Rede virtual**: A VNet para o qual pretende implementar o cluster Kubernetes. Se pretender criar uma nova VNet para o cluster, selecione *criar nova* e siga os passos a *criar rede virtual* secção.

**Sub-rede**: A sub-rede dentro da VNet onde pretende implementar o cluster. Se pretender criar uma nova subrede na VNet para o cluster, selecione *criar nova* e siga os passos a *criar sub-rede* secção.

**Intervalo de endereços de serviço Kubernetes**: O *Kubernetes service intervalo de endereços* é o intervalo IP a partir da qual os endereços são atribuídos ao Kubernetes serviços no seu cluster (para obter mais informações sobre serviços Kubernetes, consulte [ Serviços] [ services] na documentação do Kubernetes).

Intervalo de endereços IP do serviço de Kubernetes:

* Não tem de estar no intervalo de endereços IP de VNet do cluster
* Não pode sobrepor com quaisquer outras VNets com a qual o cluster VNet elementos da rede
* Não pode sobrepor-se com os IPs no local

Um comportamento imprevisível pode resultar se forem utilizados intervalos de IP sobrepostos. Por exemplo, se um pod tenta aceder a um IP fora do cluster e que também acontece IP ser um IP de serviço, poderá ver um comportamento imprevisível e falhas.

**O endereço IP do serviço Kubernetes DNS**: O endereço IP para o serviço DNS do cluster. Este endereço tem de estar dentro do *Kubernetes service intervalo de endereços*.

**Endereço de Bridge de docker**: O endereço IP e máscara de rede para atribuir a bridge de Docker. Este endereço IP não tem de estar no intervalo de endereços IP de VNet do cluster.

## <a name="configure-networking---cli"></a>Configurar redes - CLI

Quando cria um cluster AKS com a CLI do Azure, também pode configurar redes avançadas. Utilize os seguintes comandos para criar um novo cluster AKS com funcionalidades avançadas de rede ativadas.

Em primeiro lugar, obter o ID de recurso de sub-rede para a sub-rede existente para o qual o cluster AKS será associado:

```console
$ az network vnet subnet list --resource-group myVnet --vnet-name myVnet --query [].id --output tsv

/subscriptions/d5b9d4b7-6fc1-46c5-bafe-38effaed19b2/resourceGroups/myVnet/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/default
```

Utilize o [az aks criar] [ az-aks-create] comando com o `--network-plugin azure` argumento para criar um cluster com o funcionamento em rede avançado. Atualização do `--vnet-subnet-id` valor com o ID de sub-rede recolhidos no passo anterior:

```azurecli
az aks create --resource-group myAKSCluster --name myAKSCluster --network-plugin azure --vnet-subnet-id <subnet-id> --docker-bridge-address 172.17.0.1/16 --dns-service-ip 10.2.0.10 --service-cidr 10.2.0.0/24
```

## <a name="configure-networking---portal"></a>Configurar redes - portal

A seguinte captura de ecrã do portal do Azure mostra um exemplo de como configurar estas definições durante a criação do cluster AKS:

![Avançadas de configuração de rede no portal do Azure][portal-01-networking-advanced]

## <a name="frequently-asked-questions"></a>Perguntas mais frequentes

As seguintes perguntas e respostas aplicam-se para o **avançadas** configuração da rede.

* *Pode configurar redes avançada com a CLI do Azure?*

  Não. Redes avançadas estão atualmente disponível apenas quando implementar AKS clusters no portal do Azure ou com um modelo do Resource Manager.

* *Pode implementar VMs na minha sub-rede de cluster?*

  Não. Implementar VMs na sub-rede utilizada pelo Kubernetes cluster não é suportada. VMs podem ser implementadas na mesma VNet, mas numa sub-rede diferente.

* *Pode configurar políticas de rede por pod?*

  Não. Políticas de rede por pod não são atualmente suportadas.

* *É o número máximo de pods implementável para um nó configurável?*

  Por predefinição, cada nó pode alojar um máximo de 30 pods. Pode alterar o valor máximo apenas ao modificar o `maxPods` propriedade quando implementar um cluster com um modelo do Resource Manager.

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
[services]: https://kubernetes.io/docs/concepts/services-networking/service/
[portal]: https://portal.azure.com

<!-- LINKS - Internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[aks-ssh]: aks-ssh.md
