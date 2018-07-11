---
title: Abrir portas para uma VM com o portal do Azure | Documentos da Microsoft
description: Saiba como abrir uma porta / criar um ponto final para a VM do Windows com o modelo de implementação do resource manager no Portal do Azure
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
ms.assetid: f7cf0319-5ee7-435e-8f94-c484bf5ee6f1
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 12/13/2017
ms.author: cynthn
ms.openlocfilehash: 2820dcabf042d7463f9776b42f277a0457caf3b6
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37929020"
---
# <a name="how-to-open-ports-to-a-virtual-machine-with-the-azure-portal"></a>Como abrir portas para uma máquina virtual com o portal do Azure
[!INCLUDE [virtual-machines-common-nsg-quickstart](../../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Comandos rápidos
Também pode [executar estes passos com o Azure PowerShell](nsg-quickstart-powershell.md).

Primeiro, crie o seu grupo de segurança de rede. Selecione um grupo de recursos no portal, escolha **Add**, em seguida, procure e selecione **grupo de segurança de rede**:

![Adicionar um grupo de segurança de rede](./media/nsg-quickstart-portal/add-nsg.png)

Introduza um nome para o seu grupo de segurança de rede, selecione ou crie um grupo de recursos e selecione uma localização. Selecione **criar** quando concluído:

![Criar um grupo de segurança de rede](./media/nsg-quickstart-portal/create-nsg.png)

Selecione o seu novo grupo de segurança de rede. Selecione "Regras de segurança de entrada", em seguida, selecione o **adicionar** botão para criar uma regra:

![Adicionar uma regra de entrada](./media/nsg-quickstart-portal/add-inbound-rule.png)

Para criar uma regra que permita o tráfego:

- Selecione o **básica** botão. Por predefinição, o **avançadas** janela fornece algumas opções de configuração adicionais, por exemplo, para definir um bloco ou porta intervalo IP de origem específico.
- Escolha uma comum **serviço** no menu pendente, tal como *HTTP*. Também pode selecionar *personalizado* para fornecer uma porta específica a utilizar. 
- Se assim o desejar, altere a prioridade ou o nome. A prioridade afeta a ordem pela qual as regras são aplicadas - menor o valor numérico, quanto mais cedo a regra é aplicada.
- Quando estiver pronto, selecione **OK** para criar a regra:

![Criar uma regra de entrada](./media/nsg-quickstart-portal/create-inbound-rule.png)

Seu passo final é associar o seu grupo de segurança de rede com uma sub-rede ou uma interface de rede específicas. Vamos associar o grupo de segurança de rede com uma sub-rede. Selecione **sub-redes**, em seguida, escolha **associar**:

![Associar um grupo de segurança de rede com uma sub-rede](./media/nsg-quickstart-portal/associate-subnet.png)

Selecione a rede virtual e, em seguida, selecione a sub-rede apropriada:

![Associar um grupo de segurança de rede com a rede virtual](./media/nsg-quickstart-portal/select-vnet-subnet.png)

Acabou de criar um grupo de segurança de rede, criou uma regra de entrada que permita o tráfego na porta 80 e associou-o com uma sub-rede. Todas as VMs que se liga a essa sub-rede se encontram acessíveis na porta 80.

## <a name="more-information-on-network-security-groups"></a>Obter mais informações sobre grupos de segurança de rede
Os comandos rápidos aqui permitem-lhe começar a trabalhar com o tráfego que flui à sua VM. Grupos de segurança de rede fornecem muitos excelentes recursos e granularidade para controlar o acesso aos seus recursos. Pode ler mais sobre [criar um grupo de segurança de rede e a ACL de regras aqui](../../virtual-network/tutorial-filter-network-traffic.md).

Para aplicações web de elevada disponibilidade, deve colocar as suas VMs por trás de um balanceador de carga do Azure. O Balanceador de carga distribui o tráfego para VMs, com um grupo de segurança de rede que fornece a filtragem de tráfego. Para obter mais informações, consulte [como a carga de balanceamento de máquinas virtuais do Linux no Azure para criar uma aplicação de elevada disponibilidade](tutorial-load-balancer.md).

## <a name="next-steps"></a>Passos Seguintes
Neste exemplo, criou uma regra simple para permitir tráfego HTTP. Pode encontrar informações sobre como criar ambientes mais detalhadas nos seguintes artigos:

* [Descrição geral do Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md)
* [O que é um grupo de segurança de rede?](../../virtual-network/security-overview.md)