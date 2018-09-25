---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines
author: axayjo
ms.service: virtual-machines
ms.topic: include
ms.date: 09/13/2018
ms.author: akjosh; cynthn
ms.custom: include file
ms.openlocfilehash: 830deb7569772b610b7e6abde649830b7ad67a92
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47047844"
---
## <a name="before-you-begin"></a>Antes de começar

Para concluir o exemplo neste artigo, tem de ter uma imagem gerida existente de uma VM generalizada. Para obter mais informações, consulte [Tutorial: criar uma imagem personalizada de uma VM do Azure com a CLI 2.0 do Azure](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-custom-images). 

## <a name="preview-register-the-feature"></a>Pré-visualização: Registar a funcionalidade

Galerias de imagem partilhada está em pré-visualização, mas tem de registar a funcionalidade antes de poder utilizá-lo. Para registar a funcionalidade de galerias de imagem partilhada:

```azurecli-interactive
az feature register --namespace Microsoft.Compute --name GalleryPreview
az provider register -n Microsoft.Compute
```

Poderá demorar alguns minutos para registar a funcionalidade. Pode verificar o progresso, com:

```azurecli-interactive
az provider show -n Microsoft.Compute
```

## <a name="create-an-image-gallery"></a>Criar uma galeria de imagens 

Uma galeria de imagens é o recurso principal utilizado para ativar a partilha de imagens. Nomes de galeria tem de ser exclusivos na sua subscrição. Crie uma galeria de imagens com [az sig criar](/cli/azure/sig#az-sig-create). O exemplo seguinte cria uma galeria com o nome *myGallery* na *myGalleryRG*.

```azurecli-interactive
az group create --name myGalleryRG --location WestCentralUS
az sig create -g myGalleryRG --gallery-name myGallery
```

## <a name="create-an-image-definition"></a>Criar uma definição de imagem

Criar uma definição de imagem inicial na galeria utilizando [criação da definição de imagem do sig az](/cli/azure/sig/image-definition#az-sig-image-definition-create).

```azurecli-interactive 
az sig image-definition create \
   -g myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --publisher myPublisher \
   --offer myOffer \
   --sku 16.04-LTS \
   --os-type Linux 
```

## <a name="create-an-image-version"></a>Criar uma versão de imagem 
 
Criar versões da imagem, conforme necessário através de [az Galeria criar-imagem-versão da imagem](/cli/azure/sig/image-version#az-sig-image-version-create). Terá de introduzir o ID da imagem gerida a utilizar como uma linha de base para criar a versão da imagem. Pode usar [lista de imagens de az](/cli/azure/image?view#az-image-list) para obter informações sobre imagens que estão num grupo de recursos. Neste exemplo, é a versão da nossa imagem *1.0.0* e, vamos criar 5 réplicas total no *e.u.a. Centro-Oeste*, *Centro-Sul* e E.U.A. Leste 2 * regiões.

```azurecli-interactive 
az sig image-version create \
   -g myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --gallery-image-version 1.0.0 \
   --target-regions "WestCentralUS" "SouthCentralUS=1" "EastUS2=1" \
   --replica-count 5 \
   --managed-image "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/images/myImage"
```

