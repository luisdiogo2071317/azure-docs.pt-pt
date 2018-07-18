---
title: Configuração de rede no Azure Kubernetes Service (AKS)
description: Saiba mais sobre a configuração de rede básicas e avançadas no Azure Kubernetes Service (AKS).
services: container-service
author: mmacy
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/16/2018
ms.author: marsma
ms.openlocfilehash: cb7b27b178197cde040e1d106ed5a5ee20905823
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/18/2018
ms.locfileid: "39115800"
---
# <a name="network-configuration-in-azure-kubernetes-service-aks"></a>Configuração de rede no Azure Kubernetes Service (AKS)

Quando cria um cluster do Azure Kubernetes Service (AKS), pode selecionar a partir de duas opções de rede: **básica** ou **avançadas**.

## <a name="basic-networking"></a>Rede básica

O **básica** opção de rede é a configuração predefinida para a criação de cluster do AKS. A configuração de rede do cluster e os seus pods são geridas completamente pelo Azure e é adequada para implementações que não necessitam de configuração de VNet personalizada. Não tem controlo sobre a configuração de rede, tais como atribuídos ao cluster, ao selecionar rede básica de intervalos de endereços de sub-redes ou o IP.

Nós num cluster do AKS configurada para utilização de rede básica a [kubenet] [ kubenet] Plug-in do Kubernetes.

## <a name="advanced-networking"></a>Sistema de rede avançado

**Advanced** redes coloca os seus pods uma rede Virtual do Azure (VNet) que configurar, fornecendo conectividade automática aos recursos da VNet e a integração com o avançado conjunto de recursos, essa oferta de VNets. Sistema de rede avançado está disponível quando implementar o AKS clusters com o [portal do Azure][portal], CLI do Azure, ou com um modelo do Resource Manager.

Nós num cluster do AKS configurada para utilização de rede avançada a [Interface de rede contentor do Azure (CNI)] [ cni-networking] Plug-in do Kubernetes.

![Diagrama que mostra dois nós com pontes ligar cada uma a uma VNet do Azure única][advanced-networking-diagram-01]

## <a name="advanced-networking-features"></a>Funcionalidades de rede avançadas

Sistema de rede avançado fornece as seguintes vantagens:

* Implementar o seu cluster do AKS para uma VNet já existente ou criar uma nova VNet e sub-rede para o seu cluster.
* Cada pod do cluster é atribuído um endereço IP na VNet e pode comunicar diretamente com outros pods no cluster e a outros nós na VNet.
* Um pod pode ligar a outros serviços numa VNet em modo de peering e redes no local através do ExpressRoute e de site a site (S2S) ligações VPN. Pods também se encontram acessíveis no local.
* Expor um serviço de Kubernetes externa ou internamente através do Balanceador de carga do Azure. Também é uma funcionalidade do sistema de rede básica.
* Podem ligar com segurança pods numa sub-rede que têm pontos finais de serviço ativados os serviços do Azure, por exemplo, armazenamento do Azure e SQL DB.
* Utilize utilizador rotas definidas pelo (UDR) para encaminhar o tráfego de pods para uma aplicação Virtual de rede.
* Pods podem aceder a recursos na Internet pública. Também é uma funcionalidade do sistema de rede básica.

## <a name="advanced-networking-prerequisites"></a>Advanced pré-requisitos do sistema de rede

* VNet para o cluster do AKS têm de permitir conectividade de internet de saída.
* Não crie mais de um cluster do AKS na mesma sub-rede.
* Sistema de rede avançado para o AKS não suporta a VNets que utilizam as zonas privadas do DNS do Azure.
* Não podem utilizar clusters do AKS `169.254.0.0/16`, `172.30.0.0/16`, ou `172.31.0.0/16` intervalo de endereços do serviço para o Kubernetes.
* O principal de serviço utilizado para o cluster do AKS tem de ter `Contributor` permissões para o grupo de recursos que contém a VNet existente.

## <a name="plan-ip-addressing-for-your-cluster"></a>Planear o endereçamento IP para o seu cluster

Clusters configurados com um sistema de rede avançado requerem planeamento adicional. O tamanho da sua VNet e a respetiva sub-rede deve se acomodar tanto o número de pods que pretende executar, bem como o número de nós do cluster.

Endereços IP para os pods e nós do cluster são atribuídos a partir da sub-rede especificada dentro da VNet. Cada nó está configurado com um IP primário, que é o IP do nó e 30 endereços IP adicionais pré-configurado pelo Azure CNI que estão atribuídos a pods agendados para o nó. Ao aumentar horizontalmente o seu cluster, cada nó da mesma forma é configurado com endereços IP da sub-rede.

O plano de endereço IP para um cluster do AKS é composta por uma VNet, pelo menos uma sub-rede para nós e pods e um intervalo de endereços do serviço de Kubernetes.

| Intervalo de endereços / Azure recursos | Limites e dimensionamento |
| --------- | ------------- |
| Rede virtual | VNet do Azure pode ser tão grande quanto /8, mas pode apenas 16.000 configurar os endereços IP. |
| Subrede | Tem de ser suficientemente grande para acomodar os nós, pods e recursos de todos os Kubernetes e do Azure que podem ser aprovisionados no seu cluster. Por exemplo, se implementar um balanceador de carga interno do Azure, o IPs de front-end são alocadas a partir da sub-rede de cluster, IPs públicos não. <p/>Para calcular *mínima* tamanho da sub-rede: `(number of nodes) + (number of nodes * pods per node)` <p/>Exemplo para um cluster de 50 nó: `(50) + (50 * 30) = 1,550` (/ 21 ou superior) |
| Intervalo de endereços de serviço do Kubernetes | Não deve ser utilizado pelo qualquer elemento de rede no ou ligado a esta VNet neste intervalo. Endereço do serviço CIDR tem de ser menor do que /12. |
| Endereço IP do serviço de DNS do Kubernetes | Intervalo de endereços que será utilizado pela deteção de serviço de cluster (kube-dns) do serviço de endereço IP dentro do Kubernetes. |
| Endereço de bridge do docker | Endereço IP (em notação CIDR) utilizado como a ponte de Docker endereço IP em nós. Predefinição de 172.17.0.1/16. |

Cada VNet aprovisionada para utilização com o plug-in do Azure CNI é limitada a **16.000 configurar os endereços IP**.

## <a name="maximum-pods-per-node"></a>Pods máximas por nó

O número máximo de padrão de pods por nó num cluster do AKS varia entre redes básicas e avançadas e o método de implementação de cluster.

### <a name="default-maximum"></a>Predefinição máxima

* Rede básica: **110 pods por nó**
* Advanced networking **30 pods por nó**

### <a name="configure-maximum"></a>Configurar máximo

Dependendo de seu método de implementação, poderá modificar o número máximo de pods por nó num cluster do AKS.

* **CLI do Azure**: Especifique o `--max-pods` argumento ao implementar um cluster com o [criar az aks] [ az-aks-create] comando.
* **Modelo do Resource Manager**: Especifique o `maxPods` propriedade na [ManagedClusterAgentPoolProfile] objeto quando implementar um cluster com um modelo do Resource Manager.
* **Portal do Azure**: não é possível modificar o número máximo de pods por nó, ao implementar um cluster com o portal do Azure. Clusters de rede avançadas estão limitados a 30 pods por nó quando implementado no portal do Azure.

## <a name="deployment-parameters"></a>Parâmetros de implementação

Quando cria um cluster do AKS, os seguintes parâmetros são configuráveis para funcionamento em rede avançado:

**Rede virtual**: A VNet para o qual pretende implementar o cluster de Kubernetes. Se quiser criar uma nova VNet para o seu cluster, selecione *criar novo* e siga os passos a *criar rede virtual* secção. A VNet está limitada a 16 000 endereços IP configurados.

**Sub-rede**: A sub-rede dentro da VNet onde pretende implementar o cluster. Se quiser criar uma nova sub-rede na VNet para o seu cluster, selecione *criar novo* e siga os passos a *criar a sub-rede* secção.

**Intervalo de endereços de serviço do Kubernetes**: A *intervalo de endereços de serviço do Kubernetes* é o intervalo IP a partir do qual os endereços são atribuídos aos serviços do Kubernetes no seu cluster (para obter mais informações sobre serviços do Kubernetes, consulte [ Serviços] [ services] na documentação do Kubernetes).

Intervalo de endereços IP do serviço de Kubernetes:

* Não tem de estar no intervalo de endereços IP de VNet do seu cluster
* Não pode sobrepor com quaisquer outras VNets com a qual o cluster VNet elementos
* Não pode sobrepor com qualquer IPs no local

Um comportamento imprevisível poderá resultar se os intervalos IP sobrepostos são utilizados. Por exemplo, se um pod tenta acessar um IP fora do cluster e que o IP também é um IP de serviço, poderá ver um comportamento imprevisível e falhas.

**Endereço IP do serviço de DNS do Kubernetes**: O endereço IP para o serviço DNS do cluster. Este endereço tem de estar dentro de *intervalo de endereços de serviço do Kubernetes*.

**Endereço de Bridge do docker**: O endereço IP e máscara de rede para atribuir a bridge do Docker. Este endereço IP não tem de estar no intervalo de endereços IP de VNet do seu cluster.

## <a name="configure-networking---cli"></a>Configurar redes - CLI

Quando cria um cluster do AKS com a CLI do Azure, também pode configurar a rede avançada. Utilize os seguintes comandos para criar um novo cluster do AKS com funcionalidades avançadas de rede ativadas.

Em primeiro lugar, obtenha o ID de recurso de sub-rede para a sub-rede existente no qual vai ser associado um cluster do AKS:

```console
$ az network vnet subnet list --resource-group myVnet --vnet-name myVnet --query [].id --output tsv

/subscriptions/d5b9d4b7-6fc1-46c5-bafe-38effaed19b2/resourceGroups/myVnet/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/default
```

Utilize o [criar az aks] [ az-aks-create] comando com o `--network-plugin azure` argumento para criar um cluster com a rede avançada. Atualização do `--vnet-subnet-id` valor com o ID de sub-rede recolhido no passo anterior:

```azurecli
az aks create --resource-group myAKSCluster --name myAKSCluster --network-plugin azure --vnet-subnet-id <subnet-id> --docker-bridge-address 172.17.0.1/16 --dns-service-ip 10.2.0.10 --service-cidr 10.2.0.0/24
```

## <a name="configure-networking---portal"></a>Configurar redes - portal

A seguinte captura de ecrã do portal do Azure mostra um exemplo de configurar estas definições durante a criação de cluster do AKS:

![Avançadas de configuração de rede no portal do Azure][portal-01-networking-advanced]

## <a name="frequently-asked-questions"></a>Perguntas mais frequentes

As perguntas e respostas seguintes aplicam-se para o **avançadas** configuração da rede.

* *Pode implementar VMs na sub-rede meu cluster?*

  Não. Não é suportada a implementação de VMs na sub-rede utilizada pelo seu cluster do Kubernetes. As VMs podem ser implementadas na mesma VNet, mas numa sub-rede diferente.

* *Pode configurar políticas de rede por pod?*

  Não. Políticas de rede por pod não são atualmente suportadas.

* *É o número máximo de pods implementável para um nó configurável?*

  Sim, quando implementar um cluster com a CLI do Azure ou um modelo do Resource Manager. Ver [pods máximo por nó](#maximum-pods-per-node).

* *Como posso configurar propriedades adicionais para a sub-rede que criei durante a criação de cluster do AKS? Por exemplo, pontos finais de serviço.*

  A lista completa de propriedades para a VNet e sub-redes que criou durante a criação de cluster do AKS pode ser configurada na página de configuração de VNet standard no portal do Azure.

## <a name="next-steps"></a>Passos Seguintes

### <a name="networking-in-aks"></a>Funcionamento em rede no AKS

Saiba mais sobre o funcionamento em rede no AKS nos seguintes artigos:

[Utilizar um endereço IP estático com o Balanceador de carga do Azure Kubernetes Service (AKS)](static-ip.md)

[Entrada HTTPS no Azure Container Service (AKS)](ingress.md)

[Utilizar um balanceador de carga interno com o Azure Container Service (AKS)](internal-lb.md)

### <a name="acs-engine"></a>Motor de ACS

[Motor do Azure Container Service (ACS Engine)] [ acs-engine] é um projeto de código-fonte aberto que gera os modelos Azure Resource Manager, pode utilizar para implementar clusters habilitados no Docker no Azure. Kubernetes, DC/OS, modo Swarm e Swarm orquestradores podem ser implementadas com o motor de ACS.

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
[ManagedClusterAgentPoolProfile]: /azure/templates/microsoft.containerservice/managedclusters#managedclusteragentpoolprofile-object
