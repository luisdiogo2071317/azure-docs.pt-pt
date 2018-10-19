---
title: Os pré-requisitos do módulo do IoT Edge do Azure | Documentos da Microsoft
description: Pré-requisitos para a publicação de um módulo do IoT Edge.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: pbutlerm
ms.openlocfilehash: 7271a97dc7ab9de1840d809ded0ba1c2940ed83f
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/19/2018
ms.locfileid: "49431653"
---
# <a name="iot-edge-module-publishing-prerequisites"></a>Pré-requisitos de publicação de módulo do IoT Edge

Este artigo descreve os pré-requisitos para a publicação de uma oferta de módulo do IoT Edge.

Para saber mais sobre os módulos do IoT Edge e os benefícios de um módulo de publicação no Azure Marketplace, consulte a [módulos do IoT Edge, guia de publicação](https://docs.microsoft.com/azure/marketplace/iot-edge-module).

## <a name="publishing-prerequisites"></a>Publicação de pré-requisitos

Para publicar o módulo do IoT Edge no Azure Marketplace, tem de cumprir os seguintes pré-requisitos:

<!-- P2: It would be great to point to the terms of use of CPP here. This can often be a blocker for big companies and these terms of use are not anonymously visible yet.-->
- Acesso para o [Portal de parceiros da Cloud](https://cloudpartner.azure.com/). Para obter mais informações, consulte [guia de publicação do Azure Marketplace e AppSource](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide).
- Contrato para o [termos do Azure Marketplace](https://azure.microsoft.com/support/legal/marketplace-terms/)
- Aloje o seu elemento de técnico de módulo do IoT Edge no Azure Container Registry.  Para obter mais informações, consulte [como preparar o IoT Edge recursos técnicos do módulo](./cpp-create-technical-assets.md)
- Ter seus metadados do módulo do IoT Edge pronto a utilizar. Por exemplo, (lista parcial):
    - Um título
    - Uma descrição (no formato HTML)
    - Uma imagem de logótipo (formato PNG e tamanhos de imagem fixa incluindo 40x40px, 90x90px, 115x115px, 255x115px)
    - Uma condição de política de privacidade e de utilização
    - Uma configuração de módulo predefinida que inclui: duplo de rotas, as propriedades pretendidas, createOptions e variáveis de ambiente.
    - Documentação
    - Contactos de suporte

## <a name="next-steps"></a>Passos Seguintes

- [Preparar o IoT Edge recurso de técnicas de módulo](./cpp-create-technical-assets.md)
- [Criar a sua oferta de módulo do IoT Edge](./cpp-create-offer.md)
