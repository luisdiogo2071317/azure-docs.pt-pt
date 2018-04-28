---
title: Criar um grupo de segurança de rede - portal do Azure | Microsoft Docs
description: Saiba como criar e implementar grupos de segurança de rede através do portal do Azure.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 5bc8fc2e-1e81-40e2-8231-0484cd5605cb
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d8a66de0b0239fef12168733eca7af85c8b08f82
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/23/2018
---
# <a name="create-a-network-security-group-using-the-azure-portal"></a>Criar um grupo de segurança de rede através do portal do Azure

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Este artigo abrange o modelo de implementação do Resource Manager. Também pode [criar NSGs no modelo de implementação clássica](virtual-networks-create-nsg-classic-ps.md).

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]


## <a name="create-the-nsg-frontend-nsg"></a>Criar o NSG NSG-front-end
Para criar o **NSG-front-end** NSG, conforme mostrado no cenário, execute os seguintes passos:

1. Num browser, navegue para https://portal.azure.com e, se necessário, inicie sessão com a sua conta do Azure.
2. Selecione **+ criar um recurso >** > **grupos de segurança de rede**.
   
    ![Portal do Azure – NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure11.png)
3. Em **grupos de segurança de rede**, selecione **adicionar**.
   
    ![Portal do Azure – NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure12.png)
4. Em **criar grupo de segurança de rede**, criar um NSG denominado *NSG-front-end* no *RG NSG* recurso, grupo e, em seguida, selecione **criar** .
   
    ![Portal do Azure – NSGs](./media/virtual-networks-create-nsg-arm-pportal/figure13.png)

## <a name="create-rules-in-an-existing-nsg"></a>Criar regras num NSG existente
Para criar regras num NSG existente do portal do Azure, execute os seguintes passos:

1. Selecione **todos os serviços**, em seguida, procure **grupos de segurança de rede**. Quando **grupos de segurança de rede** seja apresentado, selecione-o.
2. Na lista de NSGs, selecione **NSG-front-end** > **regras de segurança de entrada**
   
    ![Portal do Azure – NSG-front-end](./media/virtual-networks-create-nsg-arm-pportal/figure2.png)
3. Na lista de **regras de segurança de entrada**, selecione **adicionar**.
   
    ![Portal do Azure – Adicionar regra](./media/virtual-networks-create-nsg-arm-pportal/figure3.png)
4. Em **Adicionar regra de segurança de entrada**, criar uma regra com o nome *regra web* com a prioridade de *200* permitir o acesso através de *TCP* à porta *80* para qualquer VM a partir de qualquer origem e, em seguida, selecione **OK**. Tenha em atenção que a maioria destas definições já está a valores predefinidos.
   
    ![Portal do Azure – as definições da regra](./media/virtual-networks-create-nsg-arm-pportal/figure4.png)
5. Após alguns segundos, consulte a nova regra no NSG.
   
    ![Portal do Azure – nova regra](./media/virtual-networks-create-nsg-arm-pportal/figure5.png)
6. Repita os passos 6 para criar uma regra de entrada com o nome *rdp regra* com uma prioridade de *250* permitir o acesso através de *TCP* à porta *3389* para qualquer VM a partir de qualquer origem.

## <a name="associate-the-nsg-to-the-frontend-subnet"></a>Associar o NSG à sub-rede de Front-End

1. Selecione **todos os serviços >**, introduza **grupos de recursos**, selecione **grupos de recursos** quando for apresentada, em seguida, selecione **RG NSG**.
2. Em **RG NSG**, selecione **...**   >  **TestVNet**.
   
    ![Portal do Azure – TestVNet](./media/virtual-networks-create-nsg-arm-pportal/figure14.png)
3. Em **definições**, selecione **sub-redes** > **front-end** > **grupo de segurança de rede**  >  **NSG-front-end**.
   
    ![Portal do Azure – definições da sub-rede](./media/virtual-networks-create-nsg-arm-pportal/figure15.png)
4. No **front-end** painel, selecione **guardar**.
   
    ![Portal do Azure – definições da sub-rede](./media/virtual-networks-create-nsg-arm-pportal/figure16.png)

## <a name="create-the-nsg-backend-nsg"></a>Criar o NSG NSG-back-end
Para criar o **NSG-back-end** NSG e associá-lo para o **back-end** sub-rede, conclua os seguintes passos:

1. Para criar um NSG denominado *NSG-back-end*, repita os passos [criar o NSG NSG-front-end](#Create-the-NSG-FrontEnd-NSG).
2. Para criar o **entrada** regras na tabela que se segue, repita os passos [criar regras num NSG existente](#Create-rules-in-an-existing-NSG).
   
   | Regra de entrada | Regra de saída |
   | --- | --- |
   | ![Portal do Azure - regra de entrada](./media/virtual-networks-create-nsg-arm-pportal/figure17.png) |![Portal do Azure - regra de saída](./media/virtual-networks-create-nsg-arm-pportal/figure18.png) |
3. Para associar o **NSG-back-end** NSG para o **back-end** sub-rede, repita os passos [associar o NSG para sub-rede FrontEnd](#Associate-the-NSG-to-the-FrontEnd-subnet).

## <a name="next-steps"></a>Próximos Passos
* Saiba como [gerir NSGs existentes](manage-network-security-group.md)
* [Ativar o registo](virtual-network-nsg-manage-log.md) para NSGs.