---
title: Eliminar uma oferta ou SKU do Azure Marketplace | Documentos da Microsoft
description: Passos para eliminar uma oferta ou SKU.
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
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: 3370767947399b167f4f1c81b57d8f92edfa0c4d
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51242692"
---
<a name="delete-an-offer-or-sku-from-azure-marketplace"></a>Eliminar uma oferta ou SKU do Azure Marketplace
==========================================

Pode decidir, por várias razões, remover a sua oferta do Marketplace. A Remoção da Oferta impede que novos clientes possam comprar ou implementar a sua oferta, mas não afeta os clientes existentes.
A Extinção da Oferta é o processo de extinção do serviço e/ou contrato de licenciamento entre si e os seus clientes existentes. Documentação de orientação e as políticas relacionadas com a remoção e extinção da oferta são regidas pelos [contrato de publicador do Microsoft Marketplace](https://go.microsoft.com/fwlink/?LinkID=699560) (consulte a seção
7) e o [políticas de participação](https://azure.microsoft.com/support/legal/marketplace/participation-policies/) (consulte a secção 6.2). Fala este artigo sobre os diferentes suportado elimina cenários e os passos que pode seguir para eles.

<a name="delete-a-live-sku-from-azure-marketplace"></a>Eliminar um SKU em direto do Azure Marketplace
----------------------------------------

Pode eliminar um SKU em direto do Azure Marketplace através dos seguintes passos:

1.  Inicie sessão para o [Portal de parceiros da Cloud](https://cloudpartner.azure.com/).

2.  Selecione a oferta desde o **todas as ofertas** separador.

3.  No painel do lado esquerdo do ecrã, selecione o **SKUs** separador.

4.  Selecione o SKU que pretende eliminar e clique no botão delete para esse SKU.

5.  [Voltar a publicar](./cloud-partner-portal-make-offer-live-on-Azure-Marketplace.md) a oferta no Azure Marketplace.

Depois da oferta está publicada no Azure Marketplace, o SKU será eliminado do Azure Marketplace e o portal do Azure.

<a name="roll-back-to-a-previous-sku-version"></a>Reverter para uma versão anterior de SKU
----------------------------------

É possível eliminar a versão atual de um SKU em direto do Azure Marketplace, seguindo os passos aqui descritos. Quando o processo estiver concluído, o SKU será revertido para a versão anterior.

1.  Inicie sessão para o [Portal de parceiros da Cloud](https://cloudpartner.azure.com/).

2.  Selecione a oferta desde o **todas as ofertas** separador.

3.  No painel do lado esquerdo do ecrã, selecione o **SKUs** separador.

4.  Eliminar a versão mais recente **versão de disco** na lista de versões publicadas do disco.

5.  [Voltar a publicar](./cloud-partner-portal-make-offer-live-on-Azure-Marketplace.md) a oferta no Azure Marketplace.

Depois da oferta está publicada no Azure Marketplace, a versão atual do SKU listada será eliminada do Azure Marketplace e o portal do Azure.
O SKU será revertido para a versão anterior.

<a name="delete-a-live-offer"></a>Eliminar uma oferta em direto
-------------------

Existem vários aspectos que têm de ser resolvido em caso de um pedido para remover uma oferta em direto. Siga os passos abaixo para obter documentação de orientação da equipa do suporte para remover uma oferta em direto do Azure Marketplace:

1.  Emitir um pedido de suporte através desta [link](https://go.microsoft.com/fwlink/?linkid=844975), ou ao clicar em suporte no canto superior direito do Portal de parceiro de CLoud.

2.  Selecione o tipo de oferta específico no **tipo de problema** lista e **remover uma oferta publicada** no **categoria** lista.

3.  Submeta o pedido.

A equipa de suporte irá guiá-lo no processo de eliminação de oferta.

>[!NOTE]
>A eliminar uma oferta/SKU não afetará as compras atuais da oferta/SKU. Estas ofertas/SKUs irá continuar a trabalhar como antes. No entanto, não estará disponíveis para quaisquer novas compras no futuro.
