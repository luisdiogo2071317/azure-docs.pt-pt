---
title: "Montar um volume gitRepo instâncias de contentor do Azure"
description: "Saiba como montar um volume gitRepo para clonar um repositório de Git para as instâncias de contentor"
services: container-instances
author: mmacy
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 02/08/2018
ms.author: marsma
ms.openlocfilehash: 9acde9259fcb392458e7b2fa7d3369776978285e
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/14/2018
---
# <a name="mount-a-gitrepo-volume-in-azure-container-instances"></a>Montar um volume gitRepo em instâncias de contentor do Azure

Saiba como montar um *gitRepo* volume para clonar um repositório de Git para as instâncias de contentor.

> [!NOTE]
> Montar um *gitRepo* volume está atualmente restrita para contentores de Linux. Enquanto estamos a trabalhar para colocar todas as funcionalidades de contentores do Windows, pode encontrar as diferenças da plataforma atual em [Quotas e disponibilidade das regiões do Azure Container Instances](container-instances-quotas.md).

## <a name="gitrepo-volume"></a>gitRepo volume

O *gitRepo* volume monta um diretório e clones no repositório de Git especificado para a mesma durante o arranque do contentor. Utilizando um *gitRepo* volume na sua instâncias de contentor, pode evitar adicionar código para fazê-lo nas suas aplicações.

Ao montar um *gitRepo* volume, pode definir três propriedades para configurar o volume:

| Propriedade | Necessário | Descrição |
| -------- | -------- | ----------- |
| `repository` | Sim | O URL completo, incluindo `http://` ou `https://`, do repositório de Git para ser clonado.|
| `directory` | Não | Diretório no qual deve ser clonado do repositório. O caminho não deve conter ou começar com "`..`".  Se especificar "`.`", é clonar o repositório no diretório do volume. Caso contrário, o repositório de Git foi clonado num subdiretório o nome indicado no diretório de volume. |
| `revision` | Não | O hash de consolidação de revisão para ser clonado. Se não for indicado, o `HEAD` foi clonada a revisão. |

## <a name="mount-a-gitrepo-volume"></a>Montar um volume gitRepo

Para montar um *gitRepo* volume numa instância de contentor, tem de implementar utilizando um [modelo Azure Resource Manager](/azure/templates/microsoft.containerinstance/containergroups).

Em primeiro lugar, preencher o `volumes` matriz no grupo contentor `properties` secção do modelo. Em seguida, para cada contentor no grupo de contentor no qual pretende montar o *gitRepo* volume, preencher o `volumeMounts` matriz no `properties` secção da definição de contentor.

Por exemplo, o modelo do Resource Manager seguinte cria um grupo de contentor constituída por um único contentor. O contentor clones repositórios do GitHub dois especificados pelo *gitRepo* blocos de volume. O volume segundo inclui propriedades adicionais, especificando um diretório a clonagem para e o hash de consolidação de uma revisão específica, a clonagem.

[!code-json[volume-gitrepo](~/azure-docs-json-samples/container-instances/aci-deploy-volume-gitrepo.json)]

A estrutura de diretórios resultante das duas repos clonados definida no modelo anterior é:

```
/mnt/repo1/aci-helloworld
/mnt/repo2/my-custom-clone-directory
```

Para ver um exemplo de implementação de instância de contentor com um modelo Azure Resource Manager, consulte [implementar o contentor de vários grupos em instâncias de contentor do Azure](container-instances-multi-container-group.md).

## <a name="next-steps"></a>Passos Seguintes

Saiba como montar outros tipos de volume em instâncias de contentor do Azure:

* [Montar uma partilha de ficheiros do Azure em instâncias de contentor do Azure](container-instances-volume-azure-files.md)
* [Montar um volume emptyDir em instâncias de contentor do Azure](container-instances-volume-emptydir.md)
* [Montar um volume secreto em instâncias de contentor do Azure](container-instances-volume-secret.md)
