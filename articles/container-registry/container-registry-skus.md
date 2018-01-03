---
title: SKUs de registo de contentor do Azure
description: "Compare os diferentes escalões de serviço disponíveis no registo de contentor do Azure."
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: article
ms.date: 12/20/2017
ms.author: marsma
ms.openlocfilehash: 15179fa3e3567f92a5eae69ba9a684addc3138dd
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/18/2017
---
# <a name="azure-container-registry-skus"></a>SKUs de registo de contentor do Azure

Registo de contentor do Azure (ACR) está disponível em vários escalões de serviço, conhecidos como SKUs. Estes SKUs fornecem várias opções para alinhar para os padrões de utilização e capacidade do seu registo de Docker privado no Azure e preços previsíveis.

| SKU | Gerido | Descrição |
| --- | :-------: | ----------- |
| **Básica** | Sim | Um ponto de entrada com otimização de custos para programadores de aprendizagem sobre o registo de contentor do Azure. Os registos do básicos tenham as mesmas capacidades programáticas como Standard e Premium (integração de autenticação do Azure Active Directory, a eliminação de imagem e web hooks), no entanto, existem restrições de utilização e de tamanho. |
| **Standard** | Sim | Os registos do padrão oferecem as mesmas capacidades que básico, com limites de armazenamento maiores e débito de imagem. Os registos do padrão devem satisfazer as necessidades da maioria dos cenários de produção. |
| **Premium** | Sim | Os registos do Premium fornecem limites superiores sobre as restrições, tais como operações simultâneas, ativar cenários de volume elevado e de armazenamento. Para além de maior capacidade de débito de imagem, Premium adiciona funcionalidades como [georreplicação] [ container-registry-geo-replication] para gerir um único registo em várias regiões, manter um registo de fecho de rede a cada implementação. |
| Clássica | Não | O registo de clássico SKU ativada a edição inicial do serviço de registo de contentor do Azure no Azure. Os registos do clássicos são apoiados por uma conta de armazenamento que o Azure cria na sua subscrição, o que limita a capacidade para ACR fornecer capacidades de nível mais elevadas, tais como o aumento do débito e a georreplicação. Devido ao respetivas capacidades limitadas, planeamos de despromover o SKU clássico no futuro. |

Escolher que um SKU de nível mais elevado fornece mais o desempenho e dimensionamento, no entanto, todos os SKUs geridos fornecem as mesmas capacidades programáticas. Com várias camadas de serviços, pode obter uma introdução básica, em seguida, converter para Standard e Premium, à medida que aumenta de utilização do registo.

> [!NOTE]
> Devido a descontinuação planeada do registo clássico SKU, recomendamos que utilize básica, Standard ou Premium para todos os registos de novo. Para obter informações sobre como converter o registo clássico existente, consulte [atualizar um registo de clássico][container-registry-upgrade].
>

## <a name="managed-vs-unmanaged"></a>Gerido vs não gerido

O básicas, Standard e Premium SKUs coletivamente são conhecidos como *geridos* registos e registos de clássico como *não gerido*. A principal diferença entre as duas é a forma como são armazenadas as imagens de contentor.

### <a name="managed-basic-standard-premium"></a>Gerido (básico, Standard, Premium)

Os registos do geridos são apoiados por uma conta de armazenamento do Azure gerida pelo Azure. Ou seja, a conta de armazenamento que armazena as imagens não aparece na sua subscrição do Azure. Existem vários benefícios adquiridos utilizando um registo gerido SKUs, abordado aprofundada no [atualizar um registo de clássico][container-registry-upgrade]. Este artigo incida nos SKUs de registo gerido e as respetivas capacidades.

### <a name="unmanaged-classic"></a>Não gerida (clássica)

Os registos do clássicos são "não serão geridos" na medida em que reside a conta de armazenamento que efetua uma cópia de um registo de clássico no *sua* subscrição do Azure. Como tal, são responsáveis pela gestão da conta de armazenamento na qual são armazenadas as imagens de contentor. Com os registos não geridos, não pode mudar-se entre os SKUs como alterar às suas necessidades (diferente de [atualizar] [ container-registry-upgrade] um registo geridos), e várias funcionalidades de registos geridos não estão disponíveis (por exemplo, eliminação da imagem de contentor, [georreplicação] [ container-registry-geo-replication] e [webhooks][container-registry-webhook]).

Para obter mais informações sobre a atualização de registo do clássico para uma dos SKUs geridos, consulte [atualizar um registo de clássico][container-registry-upgrade].

## <a name="sku-feature-matrix"></a>Matriz de funcionalidade do SKU

A tabela seguinte fornece detalhes sobre as funcionalidades e limites de escalões de serviço básico, Standard e Premium.

[!INCLUDE [container-instances-limits](../../includes/container-registry-limits.md)]

## <a name="changing-skus"></a>SKUs de alteração

Pode alterar o SKU de um registo com a CLI do Azure ou no portal do Azure. Pode mover livremente entre os SKUs de gerido desde que tenha de SKU está a mudar para a capacidade de armazenamento máximo necessária. Se mudar para uma dos SKUs geridos do clássico, não é possível mover novamente para clássico – é uma conversão unidirecional.

### <a name="azure-cli"></a>CLI do Azure

Para mover entre os SKUs da CLI do Azure, utilize o [atualização de acr az] [ az-acr-update] comando. Por exemplo, para mudar para o Premium:

```azurecli
az acr update --name myregistry --sku Premium
```

### <a name="azure-portal"></a>Portal do Azure

No registo do contentor **descrição geral** no portal do Azure, selecione **atualização**, em seguida, selecione um novo **SKU** da lista pendente SKU.

![Atualizar o registo de contentor SKU no portal do Azure][update-registry-sku]

Se tiver um registo de clássico, não é possível selecionar um SKU gerido no portal do Azure. Em vez disso, tem primeiro [atualizar] [ container-registry-upgrade] um registo gerido (consulte [a alteração do clássico](#changing-from-classic)).

## <a name="changing-from-classic"></a>A alteração do clássico

Existem considerações adicionais a ter em consideração quando migrar um registo de clássico não gerido para uma das gerido básica, Standard ou Premium SKUs. Se o registo de clássico contém um grande número de imagens e estiver a muitos gigabytes de tamanho, o processo de migração pode demorar algum tempo. Além disso, `docker push` operations estão desativado até que a migração estar concluída.

Para obter detalhes sobre a atualização do registo do clássico para um os SKUs geridos, consulte [atualizar um registo de contentor clássico][container-registry-upgrade].

## <a name="pricing"></a>Preços

Para obter informações sobre cada uma das SKUs de registo de contentor do Azure de preços, consulte [preços do registo de contentor][container-registry-pricing].

## <a name="next-steps"></a>Passos Seguintes

**Plano de registo de contentor do Azure**

Visite o [ACR plano] [ acr-roadmap] no GitHub para encontrar informações sobre funcionalidades futuras no serviço.

**UserVoice de registo de contentor do Azure**

Submeter e votar em novas sugestões de funcionalidades no [ACR UserVoice][container-registry-uservoice].

<!-- IMAGES -->
[update-registry-sku]: ./media/container-registry-skus/update-registry-sku.png

<!-- LINKS - External -->
[acr-roadmap]: https://aka.ms/acr/roadmap
[container-registry-pricing]: https://azure.microsoft.com/pricing/details/container-registry/
[container-registry-uservoice]: https://feedback.azure.com/forums/903958-azure-container-registry

<!-- LINKS - Internal -->
[az-acr-update]: /cli/azure/acr#az_acr_update
[container-registry-geo-replication]: container-registry-geo-replication.md
[container-registry-upgrade]: container-registry-upgrade.md
[container-registry-webhook]: container-registry-webhook.md
