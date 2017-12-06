---
title: Monitor do Azure Kubernetes cluster com Datadog
description: "Monitorização de Kubernetes cluster no serviço de contentor Azure utilizando Datadog"
services: container-service
author: bburns
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: 96bbd88f163939b58263f2f3a94b7b4d90f85736
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-datadog"></a>Monitor de um cluster do serviço de contentor do Azure com DataDog

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

## <a name="prerequisites"></a>Pré-requisitos
Esta instrução parte do princípio de que tem [criado um cluster de Kubernetes utilizando o serviço de contentor do Azure](container-service-kubernetes-walkthrough.md).

Também parte do princípio que tem o `az` cli do Azure e `kubectl` as ferramentas instaladas.

Pode testar se tiver o `az` ferramenta instalada através da execução:

```console
$ az --version
```

Se não tiver o `az` ferramenta instalada, existem instruções [aqui](https://github.com/azure/azure-cli#installation).

Pode testar se tiver o `kubectl` ferramenta instalada através da execução:

```console
$ kubectl version
```

Se não tiver `kubectl` instalado, pode executar:

```console
$ az acs kubernetes install-cli
```

## <a name="datadog"></a>DataDog
Datadog é um serviço de monitorização que recolhe dados de monitorização dos contentores num cluster do serviço de contentor do Azure. Datadog tem um Dashboard de integração do Docker onde pode ver métricas específicas dentro os contentores. Métricas recolhidas a partir dos contentores estão organizadas por da CPU, memória, rede e e/s. Datadog divide as métricas numa contentores e imagens.

Terá primeiro de [criar uma conta](https://www.datadoghq.com/lpg/)

## <a name="installing-the-datadog-agent-with-a-daemonset"></a>Instalar o agente de Datadog com um DaemonSet
DaemonSets são utilizados pelo Kubernetes para executar uma única instância de um contentor em cada anfitrião no cluster.
Se estiver a perfeita para a execução de agentes de monitorização.

Depois de ter sessão Datadog, pode seguir o [Datadog instruções](https://app.datadoghq.com/account/settings#agent/kubernetes) para instalar agentes de Datadog no seu cluster utilizando um DaemonSet.

## <a name="conclusion"></a>Conclusão
Já está! Assim que os agentes estão em execução, deverá ver dados na consola dentro de alguns minutos. Pode visitar o integrada [kubernetes dashboard](https://app.datadoghq.com/screen/integration/kubernetes) para ver um resumo do cluster.
