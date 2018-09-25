---
title: Como dimensionar um cluster do Azure Container Service para o Machine Learning | Documentos da Microsoft
description: Dimensionar um cluster de ACS - dimensionamento estático; e dimensionamento automático Dimensionar o número de nós do cluster
services: machine-learning
author: aashishb
ms.author: aashishb
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 10/04/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 718f351ad4ce325f3585ebcbe7adfc4b5905e734
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46998615"
---
# <a name="scaling-the-cluster-to-manage-web-service-throughput"></a>Dimensionar o cluster para gerir o débito de serviço da web

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]


## <a name="why-scale-the-cluster"></a>Por que motivo a dimensionar o cluster?

Dimensionar o cluster do Azure Container Service (ACS) é uma forma eficaz para otimizar a taxa de transferência do serviço enquanto mantém o tamanho do cluster para um mínimo para reduzir os custos. 

Para compreender melhor o dimensionamento automático, considere o seguinte exemplo de um cluster com três serviços da web:

![Exemplo: Três serviços num cluster](media/how-to-scale-clusters/three-services.png)

Os serviços têm vários em horários de pico: serviço 1 (linha azul) requer 40 pods em picos de procura, Service 2 (linha laranja) requer 38 no pico e 3 de serviço (linha cinza) necessita de 50 no pico. Se a reserva a capacidade de pico necessária para cada serviço individualmente, este cluster seria necessário, pelo menos, 40 + 38 + 50 = 128 total de pods.

Mas considere a utilização real de pod em qualquer ponto no tempo, representado pela linha preta tracejada no gráfico. Neste caso, o *maior número de pods utilizadas num dado momento* é 64, que ocorre em 20:00 quando o serviço 3 estiver no pico. Neste momento, Service 3 utiliza 50 pods, mas Service 2 usa apenas 9 pods e 1 do serviço utiliza apenas 5. Lembre-se de que se trata *picos de utilização* para este cluster. Isso significa que nenhum momento o cluster usa mais de 64 pods – metade o requisito calculado de 128 pods para os três serviços dimensionados de forma independente para picos de utilização.

Por meio da reatribuição pods no cluster: ou seja, ao redimensionar, para satisfazer a exigência atual de cada serviço, em vez de simplesmente exigir recursos suficientes para o pico na procura de todos os serviços, pode diminuir o tamanho do cluster. Neste exemplo simples, o dimensionamento automático diminui o número necessário de pods de 128 para 64, diminuindo o tamanho de cluster necessários em metade.

Dimensionar o número de pods é uma operação relativamente rápida, que exigem menos de um minuto, para que a capacidade de resposta do serviço não é afetada muito a sério.

> [!NOTE]
> Dimensionar um cluster não ajudará com problemas de latência de pedido. Para fins de operacionalização, o aumento vertical deve aumentar o número de êxitos e reduzir os erros de serviço indisponível. 

## <a name="how-to-scale-web-services-on-your-acs-cluster"></a>Como dimensionar serviços web no seu cluster de ACS

A opção de configuração de cluster da gestão de modelos da CLI, por predefinição configura dois agentes e um pod em seu ambiente. Também é instalado a CLI do Kubernetes.

Pode dimensionar os serviços da web que implementou no ACS ao ajustar:

* O número de nós de agente no cluster
* O número de réplicas de pod de Kubernetes em execução em nós de agente

### <a name="scaling-the-number-of-nodes-in-the-cluster"></a>Dimensionar o número de nós do cluster

O comando a seguir define o número de nós de agente no cluster:

```
az acs scale -g <resource group> -n <cluster name> --new-agent-count <new scale>
```

Este processo irá demorar alguns minutos a concluir. Para obter mais informações sobre como aumentar o número de nós no cluster, consulte [Dimensionar nós de agente no cluster do Container Service](https://docs.microsoft.com/azure/container-service/container-service-scale).

### <a name="scaling-the-number-of-kubernetes-pod-replicas-in-a-cluster"></a>Dimensionar o número de réplicas de pod de Kubernetes num cluster
 
Pode dimensionar o número de réplicas de pod atribuído ao cluster com a CLI do Azure Machine Learning ou [dashboard do Kubernetes] (https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/).

Para obter mais informações sobre o Kubernetes pods de réplica, consulte a [Pods do Kubernetes](https://kubernetes.io/docs/concepts/workloads/pods/pod/) documentação.

#### <a name="scaling-a-cluster-with-the-azure-machine-learning-cli"></a>Dimensionar um cluster com a CLI do Azure Machine Learning

Existem duas formas de dimensionar um cluster com a CLI:

- Dimensionamento Automático
- Dimensionamento estático

Dimensionamento automático está ativo por predefinição, quando o serviço é criado e, na maioria das situações, é o método preferencial de dimensionamento.

##### <a name="autoscale"></a>Dimensionamento Automático

O seguinte comando permite o dimensionamento automático e define o número mínimo e máximo de réplicas para o serviço.

```
az ml service update realtime -i <service id> --autoscale-enabled true --autoscale-min-replicas <positive number> --autoscale-max-replicas <positive number>
```

Por exemplo, definir `autoscale-min-replicas` para 5, irá criar cinco réplicas. Para chegar a um número ideal para o serviço web, defina o número de valores como 10 e monitorizar o número de mensagens de erro 503. Em seguida, ajuste o número em conformidade.


| Nome do parâmetro | Tipo | Descrição |
|--------------------|--------------------|--------------------|
| `autoscale-enabled` | boolean | Especifica se o dimensionamento automático está ativado. Predefinição: VERDADEIRO |
| `autoscale-min-replicas` | inteiro | Especifica o número mínimo de pods. Tem de ser 0 ou superior. Predefinição: 1 |
| `autoscale-max-replicas` | inteiro | Especifica o número máximo de pods. Tem de ser 1 ou superior. Se o dimensionamento automático-máximo de réplicas é menor do que o dimensionamento automático-mínimo de réplicas, dimensionamento automático-máximo de réplicas será ignorada. Predefinição: 10 |
| `autoscale-refresh-period-seconds` | inteiro | Especifica a duração em segundos entre as atualizações de dimensionamento automático. Predefinição: 1 |
| `autoscale-target-utilization` | inteiro | Especifica a percentagem de utilização que destinos de dimensionamento automático, entre 1 e 100. Predefinição: 70 |

Dimensionamento automático funciona para garantir que as duas condições seguintes:

1. A utilização de destino é cumprida
2. Dimensionamento nunca excede as configurações mínimas e máximas

Serviços num cluster competem por recursos de cluster. Um serviço de dimensionado automaticamente irá aumentar a sua utilização de recursos de cluster como suas solicitações por segundo (RPS) aumenta e lentamente liberará a recursos como os diminuições RPS. Recursos do cluster irão ser adquiridos a pedido, desde que esses recursos existem para o serviço ao adquirir.

Para obter mais informações sobre como utilizar os parâmetros de dimensionamento automático, consulte a [referência de modelo de Interface de linha de comandos de gestão](model-management-cli-reference.md) documentação.

##### <a name="static-scale"></a>Dimensionamento estático

Em geral, o dimensionamento estático deve ser evitado, uma vez que não permite a redução do tamanho de cluster de dimensionamento automático. Mesmo assim, em algumas situações dimensionamento estático pode ser aconselhado. Por exemplo, quando um cluster é dedicado a um único serviço, o dimensionamento automático não fornece benefícios; todos os recursos de cluster devem ser atribuídos a esse serviço.

Para poder estaticamente dimensionar um cluster, o dimensionamento automático deve estar desativado. Desative o dimensionamento automático com o seguinte comando:

```
az ml service update realtime -i <service id> --autoscale-enabled false
```

Depois de desativar o dimensionamento automático, o comando seguinte dimensiona diretamente o número de réplicas para um serviço.

```
az ml service update realtime -i <service id> -z <replica count>
```
 
Para obter mais informações sobre como aumentar o número de nós no cluster, consulte Dimensionar nós de agente no cluster do Container Service.

#### <a name="scaling-number-of-replicas-using-the-kubernetes-dashboard"></a>Número de dimensionamento de réplicas utilizando o dashboard do Kubernetes

Na linha de comandos, introduza:

```
kubectl proxy
```

No Windows, a localização de instalação do Kubernetes não é automaticamente adicionada ao caminho. Em primeiro lugar navegue para a pasta de instalação:

```
c:\users\<user name>\bin
```

Depois de executar o comando, verá a seguinte mensagem informativa:

```
Starting to serve on 127.0.0.1:8001
```

Se a porta já está em utilização, verá uma mensagem semelhante ao seguinte exemplo:

```
F0612 21:49:22.459111   59621 proxy.go:137] listen tcp 127.0.0.1:8001: bind: address already in use
```

Pode especificar uma porta alternativa número utilizando a *– porta* parâmetro.

```
kubectl proxy --port=8010
Starting to serve on 127.0.0.1:8010
```

Depois de iniciar o servidor de dashboard, abra um browser e introduza o seguinte URL:

```
127.0.0.1:<port number>/ui
```

No ecrã principal do dashboard, clique em **implementações** na barra de navegação esquerdo. Se não apresentar o painel de navegação, selecione este ícone ![Menu consiste em três linhas horizontais curtas](media/how-to-scale-clusters/icon-hamburger.png) no canto superior esquerdo.

Localize a implementação para modificar e clique neste ícone ![ícone de Menu consiste em três pontos verticais](media/how-to-scale-clusters/icon-kebab.png) à direita e, em seguida, clique em **YAML de visualizar/Editar**.

No ecrã de implementação de edição, localize a *spec* nó, modificar os *réplicas* valor e clique em **atualização**.
