---
title: Criar um padrão de Balanceador de público de carga com zonal IP público endereço front-end através do portal do Azure | Microsoft Docs
description: Saiba como criar um padrão de Balanceador de público de carga com zonal IP público endereço front-end com o portal do Azure
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
ms.date: 03/26/2018
ms.author: kumud
ms.openlocfilehash: 533c48b3a004f85dfbd2970d73dcf7de21811dca
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
#  <a name="create-a-public-load-balancer-standard-with-zonal-public-ip-address-frontend-using-azure-portal"></a>Criar um padrão de Balanceador de público de carga com zonal IP público endereço front-end através do portal do Azure

Os passos neste artigo através da criação de um público [padrão de Balanceador de carga](https://aka.ms/azureloadbalancerstandard) com zonal front-end. Para compreender como funcionam os zonas de disponibilidade com o Balanceador de carga padrão, consulte [zonas padrão Balanceador de carga e a disponibilidade](load-balancer-standard-availability-zones.md). 

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

> [!NOTE]
> Suporte para as zonas de disponibilidade está disponível para famílias de tamanho VM, selecionados de recursos do Azure e regiões. Para obter mais informações sobre como começar a utilizar e os recursos do Azure, regiões e famílias de tamanho VM pode tentar zonas de disponibilidade com, consulte [zonas de descrição geral da disponibilidade](https://docs.microsoft.com/azure/availability-zones/az-overview). Para obter suporte, pode contactar-nos no [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) ou [abrir um pedido de suporte do Azure](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure 

Inicie sessão no portal do Azure em https://portal.azure.com.

## <a name="create-a-load-balancer-with-zonal-frontend-ip-address"></a>Criar um balanceador de carga com o endereço IP de front-end zonal

1. Num browser, navegue para o portal do Azure: [ http://portal.azure.com ](http://portal.azure.com) e início de sessão com a sua conta do Azure.
2. No lado superior esquerdo do ecrã, selecione **crie um recurso** > **redes** > **Balanceador de carga.**
3. No **criar Balanceador de carga** página **nome** tipo **myLoadBalancer**.
4. Em **Tipo**, selecione **Público**.
5. Em SKU, selecione **padrão**.
6. Clique em **escolher um endereço IP público**, clique em **criar nova**e, em **Criar endereço IP público** página, sob o nome, tipo **myPublicIPZonal**, para o SKU, selecione **padrão**, para a zona de disponibilidade, selecione **1**.
    
>[!NOTE] 
> O público IP criada neste passo é de Standard SKU por predefinição.

7. Para **grupo de recursos**, clique em **criar nova**e, em seguida, escreva **myResourceGroupZLB** como o nome do grupo de recursos.
8. Para **localização**, selecione **Europa Ocidental**e, em seguida, clique em **OK**. Em seguida, o balanceador de carga inicia a implementação e demora alguns minutos a concluir a implementação com êxito.

    ![criar o padrão de Balanceador de carga com redundância de zona com o portal do Azure](./media/load-balancer-get-started-internet-availability-zones-zonal-portal/load-balancer-zonal-frontend.png)


## <a name="next-steps"></a>Passos seguintes
- Saiba mais sobre [zonas padrão Balanceador de carga e a disponibilidade](load-balancer-standard-availability-zones.md).



