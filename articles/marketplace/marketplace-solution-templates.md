---
title: Oferta de modelo de solução de aplicações do Azure guia de publicação
description: Este artigo descreve os requisitos para publicar um modelo de solução no Marketplace
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: ellacroi
manager: nunoc
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 07/09/2018
ms.author: ellacroi
ms.openlocfilehash: 44d081a0666aa37ec0bf8eeac540b7a7f4f4f904
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39059635"
---
# <a name="azure-applications-solution-template-offer-publishing-guide"></a>Aplicações do Azure: Oferta de modelo de solução guia de publicação

Modelos de soluções são uma das principais formas que publicar uma solução no mercado. Utilize este guia para compreender os requisitos para esta oferta. 

Estes são ofertas de transação que são implementadas e faturadas através do Marketplace. A chamada a ação que um utilizador vê é a "Obter agora."

Utilizar a aplicação do Azure: tipo de oferta para modelo de solução quando a sua solução requer automação de implantação e configuração adicional para além de uma VM simple. Pode automatizar o aprovisionamento de uma ou mais VMs a utilizar aplicações do Azure: modelos de soluções. Também pode aprovisionar os recursos de rede e armazenamento. As aplicações do Azure: modelos de solução oferecem tipo fornece benefícios de automatização para VMs únicas e todas soluções baseadas em IaaS.

## <a name="requirements-for-solution-templates"></a>Requisitos para modelos de soluções

|Requisitos |Detalhes  |
|---------|---------|
|Faturação e medição    |  Os recursos serão aprovisionados na subscrição do Azure do cliente. Máquinas de virtuais de pay as you go (em PAYGO) será transacionadas com o cliente através da Microsoft, cobrado por meio da subscrição do cliente do Azure (em PAYGO) 
No caso de bring-your-own-license, embora a Microsoft irá cobrar os custos de infraestrutura da subscrição de cliente, será transact seu software diretamente de taxas para o cliente de licenciamento        |
|Compatível com o Azure de disco rígido virtual (VHD)    |   As VMs devem ser criadas no Windows ou Linux.<ul> <li>Para obter mais informações sobre a criação de um VHD do Linux, visite o criar um VHD compatível com o Azure (baseado em Linux) secção localizado em [docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#2- Create-an-Azure-Compatible-VHD-Linux-based](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#2-create-an-azure-compatible-vhd-linux-based).</li> <li>Para obter mais informações sobre a criação de um VHD do Windows, visite o criar um VHD compatível com o Azure secção (baseados em Windows), localizado em [docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#3- Create-an-Azure-Compatible-VHD-Windows-Based](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation#3-create-an-azure-compatible-vhd-windows-based).</li> </ul>      |



## <a name="next-steps"></a>Próximos Passos
Se ainda não fez isso, 

- [Registar](https://azuremarketplace.microsoft.com/sell) no marketplace

Se é registrado e estiver a criar uma nova oferta ou trabalhando num já existente

- [Inicie sessão no Portal de parceiro de Cloud](https://cloudpartner.azure.com) para criar ou concluir sua oferta
