---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 09/20/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: f2e252a000c5633b2cc1ef34683d7d2c7c133c03
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47048150"
---
## <a name="using-rbac-to-share-images"></a>Utilizar o RBAC para partilhar imagens

Pode partilhar imagens entre subscrições através do controlo de acesso baseado em ' (RBAC) da função. Qualquer utilizador que tem permissões de leitura para uma versão de imagem, até mesmo entre subscrições, será capaz de implantar uma Máquina Virtual utilizando a versão da imagem.

Para obter mais informações sobre como partilhar recursos com o RBAC, veja [gerir o acesso com RBAC e a CLI do Azure](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli).


## <a name="list-information"></a>Informações da lista

Obter a localização, estado e outras informações sobre as galerias de imagem disponíveis usando [lista de sig az](/cli/azure/sig#az-sig-list).

```azurecli-interactive 
az sig list -o table
```

Lista as definições de imagem numa galeria, incluindo informações sobre o tipo de SO e o estado, com [lista de definição de imagem de sig az](/cli/azure/sig/image-definition#az-sig-image-definition-list).

```azurecli-interactive 
az sig image-definition list -g myGalleryRG -r myGallery -o table
```

Lista as versões de imagem partilhada numa galeria, usando [lista de versão da imagem de sig az](/cli/azure/sig/image-version#az-sig-image-version-list).

```azurecli-interactive
az sig image-version list -g myGalleryRG -r myGallery -i myGalleryImage -o table
```

Obter o ID de uma versão de imagem usando [show de versão da imagem de sig az](/cli/azure/sig/image-version#az-sig-image-version-show).

```
az sig image-version show \
-g myGalleryRG \     
-r myGallery \     
-i myGalleryImage \     
--gallery-image-version-name 1.0.0 \     
--query "id"
```