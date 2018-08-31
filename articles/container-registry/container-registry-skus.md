---
title: SKUs de registo de contentor do Azure
description: Compare os escalões de serviço diferentes disponíveis no Azure Container Registry.
services: container-registry
author: mmacy
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 08/28/2018
ms.author: marsma
ms.openlocfilehash: 5c10c961519614d1560f27c41ba57237085261ba
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/29/2018
ms.locfileid: "43190413"
---
# <a name="azure-container-registry-skus"></a>SKUs de registo de contentor do Azure

O Azure Container Registry (ACR) está disponível em vários escalões de serviço, conhecidos como SKUs. Essas SKUs fornecem preços previsíveis e várias opções para consonância com os padrões de capacidade e a utilização do seu registo privado do Docker no Azure.

| SKU | Gerido | Descrição |
| --- | :-------: | ----------- |
| **Básica** | Sim | Um ponto de entrada com otimização de custos para os programadores que estão a aprender sobre o Azure Container Registry. Os registos básicos têm as mesmas capacidades programáticas que o Standard e o Premium (integração de autenticação do Azure Active Directory, eliminação de imagens e webhooks), no entanto, existem restrições de tamanho e utilização. |
| **Standard** | Sim | Registos padrão oferecem as mesmas capacidades do básico, com limites de armazenamento maior e débito de imagem. Os registos Standard devem satisfazer as necessidades da maioria dos cenários de produção. |
| **Premium** | Sim | Registos Premium fornecem limites mais elevados nas restrições, como o armazenamento e de operações simultâneas, permitir cenários de grande volume. Para além da capacidade de débito de imagem superior, o Premium adiciona funcionalidades como a [georreplicação] [ container-registry-geo-replication] para gerir um único registo por várias regiões, mantenha um registo de perto da rede a cada implementação. |
| Clássico<sup>1</sup> | Não | Este SKU ativada na versão inicial do serviço do Azure Container Registry no Azure. Registos de clássicos são apoiados por uma conta de armazenamento que o Azure cria na sua subscrição, o que limita a capacidade para o ACR fornecer capacidades de nível mais alto, como o maior débito e a georreplicação. |

<sup>1</sup> será o SKU clássica **preterido** no **Março de 2019**. Utilize o básico, Standard ou Premium para todos os registos de contentores de novo.

Escolher que um SKU de alto nível fornece mais desempenho e dimensionamento, no entanto, todos os SKUs geridos fornecem as mesmas capacidades programáticas. Com várias camadas de serviços, pode começar a utilizar Basic então converter para Standard e Premium, à medida que aumenta de utilização do registo.

## <a name="managed-vs-unmanaged"></a>Recursos gerenciados versus não geridos

O básico, Standard e Premium SKUs são coletivamente conhecidos como *geridos* registos e registos de clássico como *não gerenciado*. A principal diferença entre os dois é como as imagens de contentor são armazenadas.

### <a name="managed-basic-standard-premium"></a>Geridos (básico, Standard, Premium)

Gerido benefício de registos do armazenamento de imagem gerenciado completamente pelo Azure. Ou seja, uma conta de armazenamento que armazena as imagens não aparece na sua subscrição do Azure. Há vários benefícios adquiridos utilizando um dos SKUs do registo gerido, discutida mais detalhes em [armazenamento de imagens de contentor no Azure Container Registry][container-registry-storage]. Este artigo enfoca os SKUs do registo gerido e as respetivas funcionalidades.

### <a name="unmanaged-classic"></a>Não-gerenciado (clássico)

> [!IMPORTANT]
> O SKU clássico está a ser preterido e não estará disponível depois de Março de 2019. Utilize o básico, Standard ou Premium para todos os registos de novo.

Registos de clássicos são "não geridos" no sentido de que a conta de armazenamento que efetua uma cópia de um registo clássico reside dentro *sua* subscrição do Azure. Como tal, são responsáveis pela gestão da conta de armazenamento na qual as imagens de contentor são armazenadas. Com registos de não-gerenciados, não é possível alternar entre SKUs conforme suas necessidades mudam (diferente de [atualizando] [ container-registry-upgrade] para um registo gerido), e vários recursos de registos geridos não estão disponíveis (por exemplo, eliminação da imagem de contentor, [georreplicação][container-registry-geo-replication], e [webhooks][container-registry-webhook]).

Para obter mais informações sobre como atualizar um registo de clássico para um dos SKUs geridos, consulte [atualizar um registo clássico][container-registry-upgrade].

## <a name="sku-feature-matrix"></a>Matriz de recursos SKU

A tabela seguinte fornece detalhes sobre os recursos e os limites dos escalões de serviço básico, Standard e Premium.

[!INCLUDE [container-instances-limits](../../includes/container-registry-limits.md)]

## <a name="changing-skus"></a>Alterar SKUs

Pode alterar o SKU de um registo com a CLI do Azure ou no portal do Azure. Pode mover-se livremente entre os SKUs geridos, desde que o SKU de estar a mudar para tem a capacidade de armazenamento máximo. Se mudar para um dos SKUs geridos da implementação clássica, não pode mover novamente para clássico – é uma conversão unidirecional.

### <a name="azure-cli"></a>CLI do Azure

Para mover entre os SKUs na CLI do Azure, utilize o [atualização do az acr] [ az-acr-update] comando. Por exemplo, para mudar para o Premium:

```azurecli
az acr update --name myregistry --sku Premium
```

### <a name="azure-portal"></a>Portal do Azure

No registo de contentor **descrição geral** no portal do Azure, selecione **Update**, em seguida, selecione um novo **SKU** na lista suspensa SKU.

![Atualizar o registo de contentor SKU no portal do Azure][update-registry-sku]

Se tiver um registo clássico, não é possível selecionar um SKU gerido no portal do Azure. Em vez disso, deve primeiro [atualizar] [ container-registry-upgrade] para um registo gerido (veja [a alteração da implementação clássica](#changing-from-classic)).

## <a name="changing-from-classic"></a>A alteração da implementação clássica

Existem considerações adicionais para levar em conta ao migrar um registo clássico não gerido para uma das gerido básico, Standard ou SKU Premium. Se o registo clássico contém um grande número de imagens e muitos gigabytes de tamanho, o processo de migração pode demorar algum tempo. Além disso, `docker push` operações estão desativadas até que a migração estiver concluída.

Para obter detalhes sobre como atualizar o seu registo de clássico para um dos SKUs geridos, consulte [atualizar um registo de contentor clássico][container-registry-upgrade].

## <a name="pricing"></a>Preços

Para obter informações sobre cada um dos SKUs de registo de contentor do Azure de preços, consulte [preços do Container Registry][container-registry-pricing].

## <a name="next-steps"></a>Passos Seguintes

**Plano de registo de contentor do Azure**

Visite o [mapa de ACR] [ acr-roadmap] no GitHub para obter informações sobre funcionalidades futuras no serviço.

**UserVoice do registo de contentor do Azure**

Submeter e votar em sugestões de funcionalidades novas na [ACR UserVoice][container-registry-uservoice].

<!-- IMAGES -->
[update-registry-sku]: ./media/container-registry-skus/update-registry-sku.png

<!-- LINKS - External -->
[acr-roadmap]: https://aka.ms/acr/roadmap
[container-registry-pricing]: https://azure.microsoft.com/pricing/details/container-registry/
[container-registry-uservoice]: https://feedback.azure.com/forums/903958-azure-container-registry

<!-- LINKS - Internal -->
[az-acr-update]: /cli/azure/acr#az-acr-update
[container-registry-geo-replication]: container-registry-geo-replication.md
[container-registry-upgrade]: container-registry-upgrade.md
[container-registry-storage]: container-registry-storage.md
[container-registry-webhook]: container-registry-webhook.md
