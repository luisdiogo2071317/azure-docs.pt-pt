---
title: Utilização partilhada imagens VM para criar um conjunto de dimensionamento no Azure | Documentos da Microsoft
description: Saiba como utilizar a CLI do Azure para criar imagens de VM partilhadas a utilizar para a implementação de conjuntos de dimensionamento de máquinas virtuais no Azure.
services: virtual-machine-scale-sets
documentationcenter: ''
author: axayjo
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/20/2018
ms.author: akjosh; cynthn
ms.custom: ''
ms.openlocfilehash: 777f46a0bbe8deb4f8b207e099e4fd83d9fc8507
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47047874"
---
# <a name="create-and-use-shared-images-for-virtual-machine-scale-sets-with-the-azure-cli-20"></a>Criar e utilizar imagens partilhadas para conjuntos de dimensionamento de máquinas virtuais com a CLI 2.0 do Azure

Quando cria um conjunto de dimensionamento, tem de especificar uma imagem a ser utilizada quando as instâncias de VM são implementadas. O serviço de Galeria de imagens de partilhado simplifica bastante a imagem personalizada de partilha na sua organização. As imagens personalizadas são como imagens do marketplace, mas são criadas por si. As imagens personalizadas podem ser utilizadas para configurações do programa de arranque do sistema, como o pré-carregamento de aplicações, configurações de aplicação e outras configurações do SO. A Galeria de imagens partilhado permite que Compartilhe suas imagens VM personalizadas com outras pessoas na sua organização, numa ou em regiões, dentro de um inquilino do AAD. Escolha quais imagens que pretende partilhar, quais são as regiões que pretende tornar disponível no mesmos e a quem pretende partilhá-los com. Pode criar várias galerias, para que pode agrupar logicamente Imagens partilhadas. A galeria é um recurso de nível superior que fornece controlo de acesso completa baseada em funções (RBAC). Imagens podem ser atualizadas, e pode optar por replicar cada versão de imagem para um conjunto diferente de regiões do Azure. A Galeria só funciona com imagens gerenciadas pelo. Neste artigo, vai aprender a:

> [!div class="checklist"]
> * Criar uma galeria de imagem partilhada
> * Criar uma definição de imagem partilhada
> * Criar uma versão de imagem partilhada
> * Criar uma VM a partir de uma imagem partilhada
> * Eliminar um recursos

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.


>[!NOTE]
> Este artigo explica o processo de uso de uma imagem gerida generalizada. Não é suportado para criar uma escala definido a partir de uma imagem VM especializada.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este artigo requer a execução da versão 2.0.46 da CLI do Azure ou posterior. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar a CLI 2.0 do Azure]( /cli/azure/install-azure-cli).


[!INCLUDE [virtual-machines-common-shared-images-cli](../../includes/virtual-machines-common-shared-images-cli.md)]

## <a name="create-a-scale-set-from-the-custom-vm-image"></a>Criar um conjunto de dimensionamento a partir da imagem de VM personalizada
Crie um conjunto com de dimensionamento com [az vmss create](/cli/azure/vmss#az-vmss-create). Em vez de uma imagem de plataforma, como *UbuntuLTS* ou *CentOS*, especifique o nome da sua imagem de VM personalizada. O exemplo seguinte cria um conjunto de dimensionamento com o nome *myScaleSet*, que utiliza a imagem personalizada com o nome *myImage* do passo anterior:

```azurecli-interactive
az vmss create \
  -g myGalleryRG \
  -n myScaleSet \
  --image "/subscriptions/<subscription ID>/resourceGroups/myGalleryRG/providers/Microsoft.Compute/galleries/myGallery/images/myImageDefinition/versions/1.0.0" \
  --admin-username azureuser \
  --generate-ssh-keys
```

A criação e configuração de todas as VMs e recursos do conjunto de dimensionamento demora alguns minutos.


[!INCLUDE [virtual-machines-common-gallery-list-cli](../../includes/virtual-machines-common-gallery-list-cli.md)]


## <a name="clean-up-resources"></a>Limpar recursos
Para remover o seu conjunto de dimensionamento e recursos adicionais, elimine o grupo de recursos e todos os respetivos recursos com [az group delete](/cli/azure/group#az_group_delete). O parâmetro `--no-wait` devolve o controlo à linha de comandos, sem aguardar a conclusão da operação. O parâmetro `--yes` confirma que pretende eliminar os recursos sem uma linha de comandos adicional para fazê-lo.

```azurecli-interactive
az group delete --name myResourceGroup --no-wait --yes
```


## <a name="next-steps"></a>Passos Seguintes

Caso se depare com quaisquer problemas, pode [resolver problemas de galerias de imagem partilhada](troubleshooting-shared-images.md).