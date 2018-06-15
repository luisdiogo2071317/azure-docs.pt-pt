---
title: Montar um volume gitRepo instâncias de contentor do Azure
description: Saiba como montar um volume gitRepo para clonar um repositório de Git para as instâncias de contentor
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 04/16/2018
ms.author: marsma
ms.openlocfilehash: e40d841c07534c9c0074c038d1e3c6e435265564
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
ms.locfileid: "32166784"
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

## <a name="mount-gitrepo-volume-azure-cli"></a>Volume de montagem gitRepo: CLI do Azure

Para montar um volume gitRepo quando implementar instâncias de contentor com o [CLI do Azure](/cli/azure), forneça o `--gitrepo-url` e `--gitrepo-mount-path` parâmetros para o [criar contentor de az] [ az-container-create] comando. Opcionalmente, pode especificar o diretório no volume para clonar para (`--gitrepo-dir`) e o hash de consolidação de revisão para ser clonado (`--gitrepo-revision`).

Este comando de exemplo clones o [aci olámundo] [ aci-helloworld] exemplo de aplicação para `/mnt/aci-helloworld` na instância do contentor:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name hellogitrepo \
    --image microsoft/aci-helloworld \
    --dns-name-label aci-demo \
    --ports 80 \
    --gitrepo-url https://github.com/Azure-Samples/aci-helloworld \
    --gitrepo-mount-path /mnt/aci-helloworld
```

Para verificar o volume de gitRepo foi montado, inicie uma shell no contentor com [exec de contentor az] [ az-container-exec] e lista o diretório:

```console
$ az container exec --resource-group myResourceGroup --name hellogitrepo --exec-command /bin/sh
/usr/src/app # ls -l /mnt/aci-helloworld/
total 16
-rw-r--r--    1 root     root           144 Apr 16 16:35 Dockerfile
-rw-r--r--    1 root     root          1162 Apr 16 16:35 LICENSE
-rw-r--r--    1 root     root          1237 Apr 16 16:35 README.md
drwxr-xr-x    2 root     root          4096 Apr 16 16:35 app
```

## <a name="mount-gitrepo-volume-resource-manager"></a>Volume de montagem gitRepo: Resource Manager

Para montar um volume gitRepo quando implementar instâncias de contentor com um [modelo Azure Resource Manager](/azure/templates/microsoft.containerinstance/containergroups), primeiro preencher o `volumes` matriz no grupo contentor `properties` secção do modelo. Em seguida, para cada contentor no grupo de contentor no qual pretende montar o *gitRepo* volume, preencher o `volumeMounts` matriz no `properties` secção da definição de contentor.

Por exemplo, o modelo do Resource Manager seguinte cria um grupo de contentor constituída por um único contentor. O contentor clones repositórios do GitHub dois especificados pelo *gitRepo* blocos de volume. O volume segundo inclui propriedades adicionais, especificando um diretório a clonagem para e o hash de consolidação de uma revisão específica, a clonagem.

<!-- https://github.com/Azure/azure-docs-json-samples/blob/master/container-instances/aci-deploy-volume-gitrepo.json -->
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

<!-- LINKS - External -->
[aci-helloworld]: https://github.com/Azure-Samples/aci-helloworld

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-exec]: /cli/azure/container#az-container-exec