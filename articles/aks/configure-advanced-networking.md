---
title: Configurado o sistema de rede avançado no Azure Kubernetes Service (AKS)
description: Saiba como configurar no Azure Kubernetes Service (AKS), incluindo a implementação de um cluster do AKS numa rede virtual existente e a sub-rede de rede avançada.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 10/11/2018
ms.author: iainfou
ms.openlocfilehash: 4c60474c07a3853e409436359713578178b639fb
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50024865"
---
# <a name="configure-advanced-networking-in-azure-kubernetes-service-aks"></a>Configurar redes avançadas no Azure Kubernetes Service (AKS)

Por predefinição, o AKS clusters uso *básica* rede, que cria e configura uma rede virtual e uma sub-rede para utilização com o cluster. Para controle adicional destas opções de funcionamento em rede, como os intervalos IP, em vez disso, pode utilizar *avançadas* funcionamento em rede. Com o sistema de rede avançado, também pode criar um cluster do AKS numa rede virtual existente e a sub-rede. Esta rede virtual existente, muitas vezes, garante uma conectividade a uma rede no local com o Azure ExpressRoute ou VPN de Site a Site.

Este artigo mostra-lhe como utilizar o sistema de rede avançado para criar e utilizar uma rede virtual com um cluster do AKS. Para obter informações mais gerais sobre o funcionamento em rede, consulte [conceitos de rede para o Kubernetes e o AKS][aks-network-concepts].

## <a name="prerequisites"></a>Pré-requisitos

* A rede virtual para o cluster do AKS têm de permitir conectividade de internet de saída.
* Não crie mais de um cluster do AKS na mesma sub-rede.
* Não podem utilizar clusters do AKS `169.254.0.0/16`, `172.30.0.0/16`, ou `172.31.0.0/16` intervalo de endereços do serviço para o Kubernetes.
* O principal de serviço utilizado pelo cluster do AKS tem de ter, pelo menos, [contribuinte de rede](../role-based-access-control/built-in-roles.md#network-contributor) permissões na sub-rede na sua rede virtual. Se pretender definir um [função personalizada](../role-based-access-control/custom-roles.md) em vez de usar a função de contribuinte de rede incorporada, são necessárias as seguintes permissões:
  * `Microsoft.Network/virtualNetworks/subnets/join/action`
  * `Microsoft.Network/virtualNetworks/subnets/read`

## <a name="plan-ip-addressing-for-your-cluster"></a>Planear o endereçamento IP para o seu cluster

Clusters configurados com um sistema de rede avançado requerem planeamento adicional. O tamanho da sua rede virtual e a respetiva sub-rede deve se acomodar o número de pods que pretende executar e o número de nós do cluster.

Endereços IP para os pods e nós do cluster são atribuídos a partir da sub-rede especificada dentro da rede virtual. Cada nó está configurado com um endereço IP primário. Por predefinição, 30 endereços IP adicionais são pré-configurado pelo CNI do Azure que estão atribuídos a pods agendadas no nó. Ao aumentar horizontalmente o seu cluster, cada nó da mesma forma é configurado com endereços IP da sub-rede. Também pode ver o [pods máximas por nó](#maximum-pods-per-node).

O plano de endereço IP para um cluster do AKS consiste num virtual de rede, pelo menos uma sub-rede para nós e pods e um intervalo de endereços do serviço de Kubernetes.

| Intervalo de endereços / Azure recursos | Limites e dimensionamento |
| --------- | ------------- |
| Rede virtual | A rede virtual do Azure pode ser tão grande quanto /8, mas está limitada a 65.536 endereços IP configurados. |
| Subrede | Tem de ser suficientemente grande para acomodar os nós, pods e recursos de todos os Kubernetes e do Azure que podem ser aprovisionados no seu cluster. Por exemplo, se implementar um balanceador de carga interno do Azure, o IPs de front-end são alocadas a partir da sub-rede de cluster, IPs públicos não. <p/>Para calcular *mínima* tamanho da sub-rede: `(number of nodes) + (number of nodes * maximum pods per node that you configure)` <p/>Exemplo para um cluster de 50 nó: `(50) + (50 * 30 (default)) = 1,550` (/ 21 ou superior)<p>Se não especificar um número máximo de pods por nó, ao criar o cluster, o número máximo de pods por nó é definido como *30*. O número mínimo de endereços IP necessários baseia-se nesse valor. Se calcular seus requisitos de endereço IP mínimos num valor máximo diferente, veja [como configurar o número máximo de pods por nó](#configure-maximum---new-clusters) definir este valor quando implementar o cluster. |
| Intervalo de endereços de serviço do Kubernetes | Não deve ser utilizado pelo qualquer elemento de rede no ou ligado a esta rede virtual neste intervalo. Endereço do serviço CIDR tem de ser menor do que /12. |
| Endereço IP do serviço de DNS do Kubernetes | Intervalo de endereços que será utilizado pela deteção de serviço de cluster (kube-dns) do serviço de endereço IP dentro do Kubernetes. |
| Endereço de bridge do docker | Endereço IP (em notação CIDR) utilizado como a ponte de Docker endereço IP em nós. Predefinição de 172.17.0.1/16. |

## <a name="maximum-pods-per-node"></a>Pods máximas por nó

O número máximo de pods por nó num cluster do AKS é 110. O *predefinição* número máximo de pods por nó varia entre a rede de básico e avançado e o método de implementação de cluster.

| Método de implementação | Padrão básico | Predefinição avançada | Configurável nas implementação |
| -- | :--: | :--: | -- |
| CLI do Azure | 110 | 30 | Sim (até 110) |
| Modelo do Resource Manager | 110 | 30 | Sim (até 110) |
| Portal | 110 | 30 | Não |

### <a name="configure-maximum---new-clusters"></a>Configurar máximo - novos clusters

Pode configurar o número máximo de pods por nó *apenas no momento da implementação de cluster*. Se implementar com a CLI do Azure ou com um modelo do Resource Manager, pode definir os pods máximos por valor do nó tão elevada como 110.

* **CLI do Azure**: Especifique o `--max-pods` argumento ao implementar um cluster com o [criar az aks] [ az-aks-create] comando. O valor máximo é de 110.
* **Modelo do Resource Manager**: Especifique o `maxPods` propriedade na [ManagedClusterAgentPoolProfile] objeto quando implementar um cluster com um modelo do Resource Manager. O valor máximo é de 110.
* **Portal do Azure**: não é possível alterar o número máximo de pods por nó, ao implementar um cluster com o portal do Azure. Clusters de rede avançadas estão limitados a 30 pods por nó, ao implementar no portal do Azure.

### <a name="configure-maximum---existing-clusters"></a>Configurar máximo - clusters existentes

Não é possível alterar os pods máximos por nó num cluster do AKS existente. Pode ajustar o número, apenas quando inicialmente a implementar o cluster.

## <a name="deployment-parameters"></a>Parâmetros de implementação

Quando cria um cluster do AKS, os seguintes parâmetros são configuráveis para funcionamento em rede avançado:

**Rede virtual**: A rede virtual no qual pretende implementar o cluster de Kubernetes. Se quiser criar uma nova rede virtual para o seu cluster, selecione *criar novo* e siga os passos a *criar rede virtual* secção. Para obter informações sobre os limites e quotas para uma rede virtual do Azure, consulte [subscrição do Azure e limites do serviço, quotas e restrições](../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits).

**Sub-rede**: A sub-rede na rede virtual onde pretende implementar o cluster. Se quiser criar uma nova sub-rede na rede virtual para o seu cluster, selecione *criar novo* e siga os passos a *criar a sub-rede* secção.

**Intervalo de endereços de serviço do Kubernetes**: Este é o conjunto de IPs virtuais que Kubernetes atribui a [services] [ services] no seu cluster. Pode utilizar qualquer intervalo de endereços privados que satisfaça os seguintes requisitos:

* Não tem de estar no intervalo de endereços IP de rede virtual do cluster
* Não pode sobrepor com outras redes virtuais com as quais elementos da rede virtual do cluster
* Não pode sobrepor com qualquer IPs no local
* Não tem de estar dentro dos intervalos `169.254.0.0/16`, `172.30.0.0/16`, ou `172.31.0.0/16`

Embora seja tecnicamente possível especificar um intervalo de endereços do serviço dentro da mesma rede virtual como o seu cluster, ao fazê-lo por isso, não é recomendado. Um comportamento imprevisível poderá resultar se os intervalos IP sobrepostos são utilizados. Para obter mais informações, consulte a [FAQ](#frequently-asked-questions) seção deste artigo. Para obter mais informações sobre serviços do Kubernetes, consulte [serviços] [ services] na documentação do Kubernetes.

**Endereço IP do serviço de DNS do Kubernetes**: O endereço IP para o serviço DNS do cluster. Este endereço tem de estar dentro de *intervalo de endereços de serviço do Kubernetes*.

**Endereço de Bridge do docker**: O endereço IP e máscara de rede para atribuir a bridge do Docker. Este endereço IP não tem de estar no intervalo de endereços IP de rede virtual do cluster.

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

  Não. Não é suportada a implementação de VMs na sub-rede utilizada pelo seu cluster do Kubernetes. As VMs podem ser implementadas na mesma rede virtual, mas numa sub-rede diferente.

* *Pode configurar políticas de rede por pod?*

  Não. Políticas de rede por pod não são atualmente suportadas.

* *É o número máximo de pods implementável para um nó configurável?*

  Sim, quando implementar um cluster com a CLI do Azure ou um modelo do Resource Manager. Ver [pods máximo por nó](#maximum-pods-per-node).

  Não é possível alterar o número máximo de pods por nó num cluster existente.

* *Como posso configurar propriedades adicionais para a sub-rede que criei durante a criação de cluster do AKS? Por exemplo, pontos finais de serviço.*

  A lista completa de propriedades para a rede virtual e sub-redes que criou durante a criação de cluster do AKS pode ser configurada na página de configuração de rede virtual padrão no portal do Azure.

* *Pode utilizar uma sub-rede diferente dentro da minha rede virtual do cluster para o* **intervalo de endereços de serviço do Kubernetes**?

  Não é recomendável, mas esta configuração é possível. O intervalo de endereços do serviço é um conjunto de IPs virtuais (VIPs) que atribui do Kubernetes para os serviços no seu cluster. Redes do Azure tem sem visibilidade para o intervalo IP do serviço de cluster de Kubernetes. Devido à falta de visibilidade do intervalo de endereços do serviço do cluster, é possível criar mais tarde uma nova sub-rede na rede virtual de cluster que sobrepõe-se com o intervalo de endereços do serviço. Se ocorrer uma sobreposição desse tipo, o Kubernetes pode atribuir um serviço de um IP já está em utilização por outro recurso na sub-rede, fazendo com que um comportamento imprevisível ou falhas. Ao garantir que usar um intervalo de endereços fora da rede virtual do cluster, é possível evitar este risco de sobreposição.

## <a name="next-steps"></a>Passos Seguintes

### <a name="networking-in-aks"></a>Funcionamento em rede no AKS

Saiba mais sobre o funcionamento em rede no AKS nos seguintes artigos:

- [Utilizar um endereço IP estático com o Balanceador de carga do Azure Kubernetes Service (AKS)](static-ip.md)
- [Utilizar um balanceador de carga interno com o Azure Container Service (AKS)](internal-lb.md)

- [Criar um controlador de entrada básico com conectividade de rede externa][aks-ingress-basic]
- [Ativar o suplemento de encaminhamento de aplicação de HTTP][aks-http-app-routing]
- [Criar um controlador de entrada que utiliza uma rede interna, privada e o endereço IP][aks-ingress-internal]
- [Criar um controlador de entrada com um IP público dinâmico e configurar a encriptar vamos para gerar automaticamente certificados TLS][aks-ingress-tls]
- [Criar um controlador de entrada com um IP público estático e configurar a encriptar vamos para gerar automaticamente certificados TLS][aks-ingress-static-tls]

### <a name="acs-engine"></a>Motor de ACS

[Motor do Azure Container Service (ACS Engine)] [ acs-engine] é um projeto de código-fonte aberto que gera os modelos Azure Resource Manager, pode utilizar para implementar clusters habilitados no Docker no Azure. Kubernetes, DC/OS, modo Swarm e Swarm orquestradores podem ser implementadas com o motor de ACS.

Clusters de Kubernetes criados com o motor de ACS de suportar o [kubenet] [ kubenet] e [Azure CNI] [ cni-networking] plug-ins. Como tal, os cenários de redes básicos e avançados são suportados pelo motor de ACS.

<!-- IMAGES -->
[advanced-networking-diagram-01]: ./media/networking-overview/advanced-networking-diagram-01.png
[portal-01-networking-advanced]: ./media/networking-overview/portal-01-networking-advanced.png

<!-- LINKS - External -->
[acs-engine]: https://github.com/Azure/acs-engine
[services]: https://kubernetes.io/docs/concepts/services-networking/service/
[portal]: https://portal.azure.com
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet

<!-- LINKS - Internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[aks-ssh]: ssh.md
[ManagedClusterAgentPoolProfile]: /azure/templates/microsoft.containerservice/managedclusters#managedclusteragentpoolprofile-object
[aks-network-concepts]: concepts-network.md
[aks-ingress-basic]: ingress-basic.md
[aks-ingress-tls]: ingress-tls.md
[aks-ingress-static-tls]: ingress-static-ip.md
[aks-http-app-routing]: http-application-routing.md
[aks-ingress-internal]: ingress-internal-ip.md
