---
title: "Montar um volume emptyDir em instâncias de contentor do Azure"
description: "Saiba como montar um volume emptyDir para partilhar dados entre os contentores de um grupo contentor em instâncias de contentor do Azure"
services: container-instances
author: mmacy
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 02/08/2018
ms.author: marsma
ms.openlocfilehash: 01b44f357d160a48852ee843cd8bebd138e95dfc
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/14/2018
---
# <a name="mount-an-emptydir-volume-in-azure-container-instances"></a>Montar um volume emptyDir em instâncias de contentor do Azure

Saiba como montar um *emptyDir* volume para partilhar dados entre os contentores de um grupo contentor em instâncias de contentor do Azure.

> [!NOTE]
> Montar um *emptyDir* volume está atualmente restrita para contentores de Linux. Enquanto estamos a trabalhar para colocar todas as funcionalidades de contentores do Windows, pode encontrar as diferenças da plataforma atual em [Quotas e disponibilidade das regiões do Azure Container Instances](container-instances-quotas.md).

## <a name="emptydir-volume"></a>emptyDir volume

O *emptyDir* volume fornece um diretório gravável acessível a cada contentor de um grupo contentor. Contentores no grupo podem ler e escrever os mesmos ficheiros no volume e podem ser montados os caminhos idêntica ou diferente em cada contentor.

Alguns exemplos utiliza para um *emptyDir* volume:

* Espaço scratch
* Ponto de verificação durante a tarefas de longa execução
* Armazenar os dados obtidos através de um contentor de sidecar e servidos por um contentor de aplicação

Dados de um *emptyDir* volume é persistente através de falhas de contentor. Contentores que são reiniciados, no entanto, não são garantidos que para manter os dados de um *emptyDir* volume.

## <a name="mount-an-emptydir-volume"></a>Montar um volume emptyDir

Para montar um volume emptyDir numa instância de contentor, tem de implementar utilizando um [modelo Azure Resource Manager](/azure/templates/microsoft.containerinstance/containergroups).

Em primeiro lugar, preencher o `volumes` matriz no grupo contentor `properties` secção do modelo. Em seguida, para cada contentor no grupo de contentor no qual pretende montar o *emptyDir* volume, preencher o `volumeMounts` matriz no `properties` secção da definição de contentor.

Por exemplo, o modelo do Resource Manager seguinte cria um grupo de contentor constituídas por duas contentores, cada um dos qual monta o *emptyDir* volume:

[!code-json[volume-emptydir](~/azure-docs-json-samples/container-instances/aci-deploy-volume-emptydir.json)]

Para ver um exemplo de implementação de instância de contentor com um modelo Azure Resource Manager, consulte [implementar o contentor de vários grupos em instâncias de contentor do Azure](container-instances-multi-container-group.md).

## <a name="next-steps"></a>Passos Seguintes

Saiba como montar outros tipos de volume em instâncias de contentor do Azure:

* [Montar uma partilha de ficheiros do Azure em instâncias de contentor do Azure](container-instances-volume-azure-files.md)
* [Montar um volume gitRepo em instâncias de contentor do Azure](container-instances-volume-gitrepo.md)
* [Montar um volume secreto em instâncias de contentor do Azure](container-instances-volume-secret.md)