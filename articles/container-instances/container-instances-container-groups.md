---
title: "Grupos de contentor de instâncias de contentor do Azure"
description: "Compreender como funcionam os grupos de contentor em instâncias de contentor do Azure"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 12/19/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: a42c01917926a4297c97cf9c5dfd1333dbef6793
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/05/2018
---
# <a name="container-groups-in-azure-container-instances"></a>Grupos de contentor em instâncias de contentor do Azure

O recurso de nível superior em instâncias de contentor do Azure está a *grupo contentor*. Este artigo descreve quais são os grupos de contentor e os tipos de cenários que ativar.

## <a name="how-a-container-group-works"></a>Como funciona um grupo contentor

Um grupo contentor é uma coleção de contentores que obter agendada no mesmo computador anfitrião. Os contentores de um grupo contentor partilham um ciclo de vida, rede local e volumes de armazenamento. É semelhante ao conceito de uma *pod* no [Kubernetes] [ kubernetes-pod] e [DC/SO][dcos-pod].

O diagrama seguinte mostra um exemplo de um grupo contentor, que inclui vários contentores.

![Diagrama de grupos de contentor][container-groups-example]

Este grupo de contentor de exemplo:

* Está agendada uma máquina de anfitrião único.
* Expõe um único endereço IP público, com uma porta exposto.
* É composta por dois contentores. Um contentor escuta na porta 80, enquanto o outro escuta na porta 5000.
* Inclui duas partilham de ficheiros do Azure como volume monta e cada contentor monta uma das partilhas localmente.

> [!NOTE]
> Os grupos de contentor multi são atualmente restritos para contentores de Linux. Enquanto que estamos a trabalhar para colocar todas as funcionalidades de contentores do Windows, pode encontrar a atual plataforma as diferenças no [Quotas e disponibilidade de região para instâncias de contentor do Azure](container-instances-quotas.md).

### <a name="networking"></a>Redes

Grupos de contentor partilham um endereço IP e um espaço de nomes de porta nesse endereço IP. Para permitir aos clientes externos atingir um contentor dentro do grupo, tem de expor a porta no endereço IP de e para o contentor. Porque contentores dentro do grupo de partilham de um espaço de nomes de porta, o mapeamento de porta não é suportado. Contentores dentro de um grupo podem aceder entre si através de localhost nas portas que possam tem expostos, mesmo que essas portas não são expostas externamente no endereço IP do grupo.

### <a name="storage"></a>Armazenamento

Pode especificar volumes externos para montar dentro de um grupo contentor. Pode mapear esses volumes em caminhos específicos dentro os contentores individuais num grupo.

## <a name="common-scenarios"></a>Cenários comuns

Os grupos de contentor multi são útil nos casos em que pretende dividir segurança uma única tarefa funcional num pequeno número de imagens de contentor, que podem ser fornecidas por equipas diferentes e tem os requisitos de recursos separado.

Exemplo de utilização pode incluir:

* Um contentor de aplicação e um contentor de registo. O contentor de registo recolhe a saída de registos e as métricas pela aplicação principal e escreve-as para armazenamento de longa duração.
* Uma aplicação e um contentor de monitorização. O contentor de monitorização periodicamente faz um pedido para a aplicação para se certificar de que está em execução e corretamente a responder e gera um alerta se não estiver.
* Um contentor que serve uma aplicação web e um contentor, a extrair o conteúdo mais recente do controlo de origem.

## <a name="next-steps"></a>Passos Seguintes

Saiba como [implementar um grupo contentor Multi](container-instances-multi-container-group.md) com um modelo Azure Resource Manager.

<!-- IMAGES -->
[container-groups-example]: ./media/container-instances-container-groups/container-groups-example.png

<!-- LINKS - External -->
[dcos-pod]: https://dcos.io/docs/1.10/deploying-services/pods/
[kubernetes-pod]: https://kubernetes.io/docs/concepts/workloads/pods/pod/
