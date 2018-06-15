---
title: Armazenamento de imagem no registo de contentor do Azure
description: Detalhes sobre como as imagens de contentor do Docker são armazenadas no registo de contentor do Azure, incluindo a segurança, redundância e capacidade.
services: container-registry
author: mmacy
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 03/21/2018
ms.author: marsma
ms.openlocfilehash: 92e60b4213cb80d193a7c35f68b8f9fd099481d7
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
ms.locfileid: "32165100"
---
# <a name="container-image-storage-in-azure-container-registry"></a>Armazenamento de imagem do contentor no registo de contentor do Azure

Cada [básicas, Standard e Premium](container-registry-skus.md) beneficia de registo de contentor do Azure de funcionalidades avançadas de armazenamento do Azure, como a encriptação em rest para segurança de dados de imagem e georredundância para proteção de dados de imagem. As secções seguintes descrevem as funcionalidades e os limites de armazenamento de imagem no registo de contentor do Azure (ACR).

## <a name="encryption-at-rest"></a>Encriptação em rest

Todas as imagens de contentor no seu registo são encriptadas em pausa. Azure automaticamente encripta uma imagem antes de armazenar e desencripta a mesma no momento quando a imagem de inquérito, ou as suas aplicações e serviços.

## <a name="geo-redundant-storage"></a>Armazenamento georredundante

Azure utiliza um esquema de armazenamento georredundante para proteger contra perda das suas imagens de contentor. Registo de contentor do Azure replica automaticamente as imagens de contentor para vários centros de dados, geograficamente distante, impedindo os respetivos perda em caso de falha de armazenamento regional.

## <a name="geo-replication"></a>Georreplicação

Para cenários que requerem ainda mais garantia de elevada disponibilidade, considere a utilização de [georreplicação](container-registry-geo-replication.md) funcionalidade de registos de Premium. Replicação geográfica ajuda a proteger contra a perda de aceso ao registo no caso de um *total* falha regional, não apenas uma falha de armazenamento. Georreplicação fornece outras vantagens, demasiado, como a imagem de fecho de rede armazenamento para o mais rapidamente pushes e obtém em cenários de desenvolvimento ou implementação distribuídos.

## <a name="image-limits"></a>Limites de imagem

A tabela seguinte descreve os contentor imagem e limites de armazenamento no local para os registos do contentor do Azure.

| Recurso | Limite |
| -------- | :---- |
| Repositórios | Sem limite |
| Imagens | Sem limite |
| Camadas | Sem limite |
| Etiquetas | Sem limite|
| Armazenamento | 5 TB |

Um número muito elevado de repositórios e as etiquetas pode afetar o desempenho do seu registo. Eliminar periodicamente repositórios de não utilizados, as etiquetas e imagens utilizando a [CLI do Azure](/cli/azure/acr), o ACR [REST API](/rest/api/containerregistry/), ou o [portal do Azure] [ portal] como parte da sua rotina de manutenção do registo. Eliminar recursos de registo como etiquetas, imagens e repositórios *não é possível* recuperou após a eliminação.

## <a name="storage-cost"></a>Custo de armazenamento

Para obter detalhes completos sobre preços, consulte [preços do registo de contentor do Azure][pricing].

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre os diferentes Azure contentor registo SKUs (básico, Standard, Premium), consulte [SKUs de registo de contentor do Azure](container-registry-skus.md).

<!-- IMAGES -->

<!-- LINKS - External -->
[portal]: https://portal.azure.com
[pricing]: http://aka.ms/acr/pricing

<!-- LINKS - Internal -->
