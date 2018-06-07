---
title: Gateway de dados do local de utilização para origens de dados de rede Virtual do Azure | Microsoft Docs
description: Saiba como configurar um servidor para utilizar um gateway para origens de dados na VNet.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 04/20/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 7cb1fb7bcd3a50532d5ff994afac56e226fb2018
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34596911"
---
# <a name="use-gateway-for-data-sources-on-an-azure-virtual-network-vnet"></a>Utilizar o gateway para origens de dados numa rede Virtual do Azure (VNet)

Este artigo descreve o **AlwaysUseGateway** propriedade do servidor para utilização quando são origens de dados num [rede Virtual do Azure (VNet)](../virtual-network/virtual-networks-overview.md).

## <a name="server-access-to-vnet-data-sources"></a>Acesso de servidor para origens de dados de VNet

Se as origens de dados são acedidas através de uma VNet, tem de ligar o servidor de Analysis Services do Azure para as origens de dados, que se encontrem no local, no seu próprio ambiente. Pode configurar o **AlwaysUseGateway** propriedade do servidor para especificar o servidor para aceder a todos os dados de origem de dados através de um [gateway no local](analysis-services-gateway.md). 

> [!NOTE]
> Esta propriedade é eficaz apenas quando um [gateway de dados no local](analysis-services-gateway.md) está instalado e configurado. O gateway pode estar na VNet.

## <a name="configure-alwaysusegateway-property"></a>Configurar a propriedade AlwaysUseGateway

1. No SSMS > servidor > **propriedades** > **geral**, selecione **propriedades Mostrar avançadas (todos)**.
2. No **ASPaaS\AlwaysUseGateway**, selecione **verdadeiro**.

    ![Utilize sempre a propriedade de gateway](media/analysis-services-vnet-gateway/aas-ssms-always-property.png)


## <a name="see-also"></a>Consulte também
[A ligar a origens de dados no local](analysis-services-gateway.md)   
[Instalar e configurar um gateway de dados no local](analysis-services-gateway-install.md)   
[Rede Virtual do Azure (VNET)](../virtual-network/virtual-networks-overview.md)   

