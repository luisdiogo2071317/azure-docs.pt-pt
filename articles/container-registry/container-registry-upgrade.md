---
title: "Atualizar um registo de contentor do Azure clássico"
description: "Tirar partido de expandido do conjunto de funcionalidades de básicas, Standard e Premium geridos os registos do contentor, atualizando o registo de contentor clássico não gerido."
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: article
ms.date: 12/20/2017
ms.author: marsma
ms.openlocfilehash: 19090bb69d7165c1e904450dc93b925e23e44782
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/21/2017
---
# <a name="upgrade-a-classic-container-registry"></a>Atualizar um registo de contentor clássico

Registo de contentor do Azure (ACR) está disponível em vários escalões de serviço, [conhecido como SKUs](container-registry-skus.md). A edição inicial do ACR oferecidas um SKU único, clássico, que não possui várias funcionalidades inerentes para básico, Standard e Premium SKUs (coletivamente conhecidos como *geridos* registos). Este artigo fornece detalhes sobre como migrar o registo de clássico não gerido para uma dos SKUs geridos para que pode tirar partido do respetivo conjunto de funcionalidades avançada.

## <a name="why-upgrade"></a>Por que motivo atualizar?

Devido às capacidades limitadas de registos de clássico não gerido, recomendamos que todos os registos de clássico ser atualizados para básico, Standard ou Premium registos geridos. Estes SKUs de nível mais elevados mais aprofundadamente integram o registo para as capacidades do Azure.

Fornecem registos geridos:

* Integração do Active Directory do Azure para [individuais início de sessão](container-registry-authentication.md#individual-login-with-azure-ad)
* Suporte de eliminação de imagem e etiqueta
* [Georreplicação](container-registry-geo-replication.md)
* [Webhooks](container-registry-webhook.md)

Mais de tudo, um registo de clássico depende a conta de armazenamento que Azure automaticamente aprovisionado na sua subscrição do Azure quando criou o registo. Por outro lado, a básicas, Standard e Premium SKUs tirar partido das *armazenamento geridas*. Ou seja, o Azure gere transparente o armazenamento das suas imagens para si – uma conta de armazenamento separada não é criada na sua própria subscrição.

Registo gerido armazenamento fornece as seguintes vantagens:

* As imagens de contentor são [encriptados em descanso ao](../storage/common/storage-service-encryption.md).
* As imagens estão armazenadas utilizando [armazenamento georredundante](../storage/common/storage-redundancy.md#geo-redundant-storage), assuring cópia de segurança das suas imagens com a replicação de multirregião.
* Capacidade de livremente [mover entre os SKUs](container-registry-skus.md#changing-skus), ativar o débito mais elevado quando seleciona um SKU de nível mais elevado. Com cada SKU ACR pode satisfazer as necessidades de débito como aumentam às suas necessidades.
* Modelo de segurança unificada para o registo e o respetivo armazenamento fornece gestão de direitos simplificada. Gerir permissões apenas para o registo de contentor, sem ter de gerir também permissões para uma conta de armazenamento separada.

## <a name="migration-considerations"></a>Considerações sobre a migração

Quando altera um registo do clássico para um registo gerido, Azure tem de copiar todas as imagens de contentor existente da conta do storage ACR criados na sua subscrição para uma conta de armazenamento gerida pelo Azure. Dependendo do tamanho do seu registo, este processo pode demorar alguns minutos a várias horas.

Durante o processo de conversão, todos os `docker push` operações estiverem bloqueadas, enquanto `docker pull` continua a funcionar.

Não eliminar ou modificar o conteúdo da conta do storage, fazer uma cópia do registo clássico durante o processo de conversão. Se o fizer, pode resultar em danos das suas imagens de contentor.

Assim que a migração estiver concluída, a conta de armazenamento na sua subscrição que originalmente efetuou o registo de clássico já é utilizada pelo ACR. Depois de verificar que se a migração foi bem sucedida, considere eliminar a conta de armazenamento para o ajudar a minimizar os custos.

>[!IMPORTANT]
> Atualização do clássico para uma dos SKUs geridos é um **processo unidirecional**. Assim que tiver a converter um registo para básico, Standard ou Premium do clássico, não é possível reverter para clássico. No entanto, pode, livremente mover entre os SKUs de gerido com capacidade suficiente para o registo.

## <a name="how-to-upgrade"></a>Como atualizar

Pode atualizar um registo de clássico não gerido para uma dos SKUs geridos de várias formas. Nas secções seguintes, vamos descrevem o processo para utilizar o [CLI do Azure] [ azure-cli] e [portal do Azure][azure-portal].

## <a name="upgrade-in-azure-cli"></a>Atualização na CLI do Azure

Para atualizar um registo de clássica a CLI do Azure, execute o [atualização de acr az] [ az-acr-update] de comandos e especificar o SKU de novo para o registo. No exemplo seguinte, um registo de clássico com o nome *myclassicregistry* é atualizado para o SKU Premium:

```azurecli-interactive
az acr update --name myclassicregistry --sku Premium
```

Quando a migração estiver concluída, deverá ver um resultado semelhante ao seguinte. Tenha em atenção que o `sku` é "Premium" e o `storageAccount` é "null," que indica que o Azure gere agora o armazenamento de imagem para este registo.

```JSON
{
  "adminUserEnabled": false,
  "creationDate": "2017-12-12T21:23:29.300547+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry",
  "location": "eastus",
  "loginServer": "myregistry.azurecr.io",
  "name": "myregistry",
  "provisioningState": "Succeeded",
  "resourceGroup": "myresourcegroup",
  "sku": {
    "name": "Premium",
    "tier": "Premium"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

Se especificar um registo gerido SKU cuja capacidade máxima é inferior ao tamanho do seu registo clássico, receberá uma mensagem de erro semelhante ao seguinte.

`Cannot update the registry SKU due to reason: Registry size 12936251113 bytes exceeds the quota value 10737418240 bytes for SKU Basic. The suggested SKU is Standard.`

Se receber um erro semelhante, execute o [atualização de acr az] [ az-acr-update] novamente o comando e especifique o SKU sugerido, que é o nível mais elevado seguinte SKU que possam acomodá as imagens.

## <a name="upgrade-in-azure-portal"></a>Atualização no portal do Azure

Ao atualizar um registo de clássica através do portal do Azure, Azure seleciona automaticamente o SKU de nível mais baixo que possam acomodá as imagens. Por exemplo, se o registo contém GiB 12 nas imagens, Azure seleciona automaticamente e converte o registo do clássico para Standard (máximo de 100 GiB).

Para atualizar o seu registo clássica através do portal do Azure, navegue para o registo de contentor **descrição geral** e selecione **atualização para o registo gerido**.

![Registo clássico atualizar botão na IU do portal do Azure][update-classic-01-upgrade]

Selecione **OK** para confirmar que pretende atualizar para um registo gerido.

![Registo clássico atualizar confirmação no portal do Azure da IU][update-classic-02-confirm]

Durante a migração, o portal indica que o registo **estado de aprovisionamento** é *atualização*. Conforme mencionado anteriormente, `docker push` operações são desativadas durante a migração e não tem de eliminar ou atualizar a conta de armazenamento utilizada pelo registo clássica, enquanto a migração está em curso – se o fizer, pode resultar em danos de imagem.

![Registo clássico progresso da atualização do portal do Azure da IU][update-classic-03-updating]

Quando a migração estiver concluída, o **estado de aprovisionamento** indica *com êxito*, e pode novamente `docker push` ao seu registo.

![Estado de conclusão no portal do Azure da IU de atualização de registo clássico][update-classic-04-updated]

## <a name="next-steps"></a>Passos Seguintes

Assim que tiver atualizado um registo para básico, Standard ou Premium do clássico, Azure já não utiliza a conta de armazenamento que originalmente efetuou o registo de clássico. Para reduzir o custo, considere eliminar a conta de armazenamento ou o contentor de Blob na conta que contém as imagens de contentor antigo.

<!-- IMAGES -->
[update-classic-01-upgrade]: ./media/container-registry-upgrade\update-classic-01-upgrade.png
[update-classic-02-confirm]: ./media/container-registry-upgrade\update-classic-02-confirm.png
[update-classic-03-updating]: ./media/container-registry-upgrade\update-classic-03-updating.png
[update-classic-04-updated]: ./media/container-registry-upgrade\update-classic-04-updated.png

<!-- LINKS - internal -->
[az-acr-update]: /cli/azure/acr#az_acr_update
[azure-cli]: /cli/azure/install-azure-cli
[azure-portal]: https://portal.azure.com