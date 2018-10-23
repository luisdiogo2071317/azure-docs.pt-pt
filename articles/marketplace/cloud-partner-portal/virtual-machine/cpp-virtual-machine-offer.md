---
title: Oferta de máquina virtual no Azure Marketplace | Documentos da Microsoft
description: Descrição geral do processo de publicação de uma oferta VM no Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 09/28/2018
ms.author: pbutlerm
ms.openlocfilehash: d3682d18fb849b2d851bae0986f9e61f216aaf2c
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/22/2018
ms.locfileid: "49639949"
---
# <a name="virtual-machine-offer"></a>Oferta de máquina virtual

Esta secção descreve os elementos de publicação de uma Máquina Virtual (VM) e destina-se como um guia para o publicador para o [do Azure Marketplace](https://azuremarketplace.microsoft.com).  De que este ponto de vista, ele é dividido as seguintes partes principais:

- [Pré-requisitos](./cpp-prerequisites.md) -lista os requisitos técnicos e empresariais antes de criar ou publicar uma oferta VM
- [Criar oferta de VM](./cpp-create-offer.md) -listas os passos necessários para criar uma nova VM oferecem ao utilizar de entrada a [Cloud Partner Portal](https://cloudpartner.azure.com)
- [Criar recursos técnicos de VM](./cpp-create-technical-assets.md) - a forma como a técnica de criar ativos para uma solução VM e como configurar este pacote como uma VM da oferta no Azure Marketplace
- [Publicar oferta de VM](./cpp-publish-offer.md) -como submeter a oferta para publicação no Azure Marketplace


## <a name="vm-publishing-process-flow"></a>Fluxo do processo de publicação de VM

O diagrama seguinte ilustra as etapas de alto nível em publicar uma oferta VM. 

![Processo de publicação de VM](./media/publishvm_001.png)

1. Criar a oferta - todos os detalhes e informações sobre a oferta são configuradas, incluindo a descrição da oferta, materiais, legal, especificações de recursos e informações de suporte de marketing.

2. Criar os recursos técnicos e comerciais - criar os ativos empresariais (documentos legais e materiais de marketing) e ativos técnicos para a solução associada (aqui, as VMs e os discos ligados). 

3. Criar o SKU - criar o SKU associado de (s) associadas à oferta e enviá-los.  Um SKU exclusivo é necessário para cada imagem que pretende publicar. 
 
4. Certificar e publicar a oferta - assim que a oferta e os ativos técnicos são concluídos, pode enviar a oferta. Esta submissão será iniciado o processo de publicação, na qual a solução é testada, validado, certified, em seguida, "entrar no ar" no marketplace.  

## <a name="next-steps"></a>Passos Seguintes

Antes de considerar estes passos, tem de cumprir os [requisitos técnicos e empresariais](./cpp-prerequisites.md) para publicar uma VM para o Microsoft Azure Marketplace. 
