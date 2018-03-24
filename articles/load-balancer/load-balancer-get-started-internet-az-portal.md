---
title: Criar um padrão de Balanceador de público de carga com redundância de zona IP público endereço front-end através do portal do Azure | Microsoft Docs
description: Saiba como criar um padrão de Balanceador de público de carga com redundância de zona IP público endereço front-end com o portal do Azure
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2018
ms.author: kumud
ms.openlocfilehash: 10a264609469245d4743886b58730304da3df7bb
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
#  <a name="create-a-public-load-balancer-standard-with-zone-redundant-public-ip-address-frontend-using-azure-portal"></a>Criar um padrão de Balanceador de público de carga com redundância de zona IP público endereço front-end através do portal do Azure

Os passos neste artigo através da criação de um público [padrão de Balanceador de carga](https://aka.ms/azureloadbalancerstandard) com um front-end com redundância de zona utilizando um endereço IP público padrão.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="register-for-availability-zones-preview"></a>Registar-se na pré-visualização de zonas de disponibilidade
 
Zonas de disponibilidade estão em pré-visualização e estão prontas para o seu desenvolvimento e teste cenários. O suporte está disponível para famílias de tamanho VM, selecionados de recursos do Azure e regiões. Para obter mais informações sobre como começar a utilizar e os recursos do Azure, regiões e famílias de tamanho VM pode tentar zonas de disponibilidade com, consulte [zonas de descrição geral da disponibilidade](https://docs.microsoft.com/azure/availability-zones/az-overview). Para obter suporte, pode contactar-nos no [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) ou [abrir um pedido de suporte do Azure](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure 

Inicie sessão no portal do Azure em https://portal.azure.com.

## <a name="create-a-zone-redundant-load-balancer"></a>Criar um balanceador de carga redundante de zona

1. Num browser, navegue para o portal do Azure: [ http://portal.azure.com ](http://portal.azure.com) e início de sessão com a sua conta do Azure.
2. No lado superior esquerdo do ecrã, selecione **crie um recurso** > **redes** > **Balanceador de carga.**
3. No * * criar o Balanceador de carga, em **nome** tipo **myPublicLB**.
4. Em **Tipo**, selecione **Público**.
5. Em SKU, selecione **Standard (pré-visualização)**.
6. Clique em **endereço IP público**, clique em **criar nova**, no **criar um endereço IP público** página, sob o nome, tipo **myPublicIPStandard**, e para **zona de disponibilidade (pré-visualização)**, selecione **zona redundante**.
7. Em **localização**, selecione **US2 Leste**e, em seguida, clique em **OK**. Em seguida, o balanceador de carga inicia a implementação e demora alguns minutos a concluir a implementação com êxito.

    ![criar o padrão de Balanceador de carga com redundância de zona com o portal do Azure](./media/load-balancer-get-started-internet-az-portal/create-zone-redundant-load-balancer-standard.png)


## <a name="next-steps"></a>Passos Seguintes
- Saiba como [criar um IP público numa zona de disponibilidade](../virtual-network/virtual-network-public-ip-address.md#create-a-public-ip-address)



