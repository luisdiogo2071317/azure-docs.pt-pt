---
title: "Montar um volume secreto em instâncias de contentor do Azure"
description: "Saiba como montar um volume secreto para armazenar informações confidenciais para acesso pelas instâncias de contentor"
services: container-instances
author: mmacy
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 02/08/2018
ms.author: marsma
ms.openlocfilehash: 6f8e1b6faac11b668a143f8013a198831a428c51
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/14/2018
---
# <a name="mount-a-secret-volume-in-azure-container-instances"></a>Montar um volume secreto em instâncias de contentor do Azure

Saiba como montar um *segredo* volume na sua instâncias de contentor para o armazenamento e obtenção de informações confidenciais, os contentores nos seus grupos de contentor.

> [!NOTE]
> Montar um *segredo* volume está atualmente restrita para contentores de Linux. Enquanto estamos a trabalhar para colocar todas as funcionalidades de contentores do Windows, pode encontrar as diferenças da plataforma atual em [Quotas e disponibilidade das regiões do Azure Container Instances](container-instances-quotas.md).

## <a name="secret-volume"></a>volume secreta

Pode utilizar um *segredo* volume para fornecer informações confidenciais para os contentores de um grupo contentor. O *segredo* volume armazena os segredos especificados nos ficheiros de volume, o qual os contentores no seu grupo contentor, em seguida, podem aceder. Ao utilizar segredos num *segredo* volume, pode evitar colocar dados confidenciais, como chaves SSH ou as credenciais da base de dados no código da aplicação.

Todos os *segredo* volumes são apoiados por [tmpfs][tmpfs], um sistema de ficheiros de segurança de RAM; os respetivos conteúdos nunca são escritos para armazenamento não volátil.

## <a name="mount-a-secret-volume"></a>Montar um volume secreto

Para montar um *segredo* volume numa instância de contentor, tem de implementar utilizando um [modelo Azure Resource Manager](/azure/templates/microsoft.containerinstance/containergroups).

Em primeiro lugar, preencher o `volumes` matriz no grupo contentor `properties` secção do modelo. Em seguida, para cada contentor no grupo de contentor no qual pretende montar o *segredo* volume, preencher o `volumeMounts` matriz no `properties` secção da definição de contentor.

Por exemplo, o modelo do Resource Manager seguinte cria um grupo de contentor constituída por um único contentor. O contentor monta uma *segredo* volume consiste em dois segredos com codificação Base64.

[!code-json[volume-secret](~/azure-docs-json-samples/container-instances/aci-deploy-volume-secret.json)]

Para ver um exemplo de implementação de instância de contentor com um modelo Azure Resource Manager, consulte [implementar o contentor de vários grupos em instâncias de contentor do Azure](container-instances-multi-container-group.md).

## <a name="next-steps"></a>Passos Seguintes

Saiba como montar outros tipos de volume em instâncias de contentor do Azure:

* [Montar uma partilha de ficheiros do Azure em instâncias de contentor do Azure](container-instances-volume-azure-files.md)
* [Montar um volume emptyDir em instâncias de contentor do Azure](container-instances-volume-emptydir.md)
* [Montar um volume gitRepo em instâncias de contentor do Azure](container-instances-volume-gitrepo.md)

<!-- LINKS - External -->
[tmpfs]: https://wikipedia.org/wiki/Tmpfs