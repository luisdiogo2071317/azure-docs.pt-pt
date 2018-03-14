---
title: Abrir portas para uma VM com o portal do Azure | Microsoft Docs
description: "Saiba como abrir uma porta / criar um ponto final para a VM do Windows utilizando o modelo de implementação do Gestor de recursos no Portal do Azure"
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
ms.assetid: f7cf0319-5ee7-435e-8f94-c484bf5ee6f1
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 12/13/2017
ms.author: iainfou
ms.openlocfilehash: 08f0af6ecdb45b263d39c3d2d6442f4ed555e3c3
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/13/2018
---
# <a name="how-to-open-ports-to-a-virtual-machine-with-the-azure-portal"></a>Como abrir portas para uma máquina virtual com o portal do Azure
[!INCLUDE [virtual-machines-common-nsg-quickstart](../../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Comandos rápidos
Também pode [executar estes passos com o Azure PowerShell](nsg-quickstart-powershell.md).

Em primeiro lugar, crie o seu grupo de segurança de rede. Selecione um grupo de recursos no portal, escolha **adicionar**, em seguida, procure e selecione **grupo de segurança de rede**:

![Adicionar um grupo de segurança de rede](./media/nsg-quickstart-portal/add-nsg.png)

Introduza um nome para o grupo de segurança de rede, selecione ou crie um grupo de recursos e selecione uma localização. Selecione **criar** quando concluída:

![Criar um grupo de segurança de rede](./media/nsg-quickstart-portal/create-nsg.png)

Selecione o novo grupo de segurança de rede. Selecionar 'Segurança regras de entrada', em seguida, selecione o **adicionar** botão para criar uma regra:

![Adicionar uma regra de entrada](./media/nsg-quickstart-portal/add-inbound-rule.png)

Para criar uma regra que permite que o tráfego:

- Selecione o **básico** botão. Por predefinição, o **avançadas** janela fornece algumas opções de configuração adicional, tal como para definir um intervalo de blocos ou de porta IP do específica de origem.
- Escolha um comuns **serviço** no menu pendente, tais como *HTTP*. Também pode selecionar *personalizada* para fornecer uma porta específica a utilizar. 
- Se assim o desejar, altere a prioridade ou o nome. A prioridade afeta a ordem pela qual as regras são aplicadas - menor o valor numérico, o anteriormente a regra é aplicada.
- Quando estiver pronto, selecione **OK** para criar a regra:

![Criar uma regra de entrada](./media/nsg-quickstart-portal/create-inbound-rule.png)

O passo final é a associar o grupo de segurança de rede uma sub-rede ou uma interface de rede específicas. Vamos associar o grupo de segurança de rede com uma sub-rede. Selecione **sub-redes**, em seguida, escolha **associar**:

![Associar um grupo de segurança de rede uma sub-rede](./media/nsg-quickstart-portal/associate-subnet.png)

Selecione a rede virtual e, em seguida, selecione a sub-rede adequada:

![Associar um grupo de segurança de rede com redes virtuais](./media/nsg-quickstart-portal/select-vnet-subnet.png)

Acabou de criar um grupo de segurança de rede, criar uma regra de entrada que permita o tráfego na porta 80 e associados a uma sub-rede. Quaisquer VMs que ligar a essa sub-rede se encontram acessíveis na porta 80.

## <a name="more-information-on-network-security-groups"></a>Obter mais informações sobre grupos de segurança de rede
Os comandos rápidos aqui permitem-lhe começar a trabalhar com tráfego que flui para a VM. Grupos de segurança de rede fornecem várias funcionalidades excelentes e granularidade para controlar o acesso aos seus recursos. Pode ler mais sobre [criar um grupo de segurança de rede e a ACL regras aqui](../../virtual-network/virtual-networks-create-nsg-arm-ps.md).

Para aplicações web de elevada disponibilidade, deve colocar as VMs por trás de um balanceador de carga do Azure. O Balanceador de carga distribui o tráfego para VMs, com um grupo de segurança de rede que fornece a filtragem de tráfego. Para obter mais informações, consulte [como carregar equilibrar os computadores virtuais Linux no Azure para criar uma aplicação altamente disponível](tutorial-load-balancer.md).

## <a name="next-steps"></a>Passos Seguintes
Neste exemplo, criou uma regra para permitir tráfego HTTP simples. Pode encontrar informações sobre como criar ambientes mais detalhadas nos seguintes artigos:

* [Descrição geral do Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md)
* [O que é um Grupo de Segurança de Rede (NSG)? (What is a Network Security Group (NSG)?)](../../virtual-network/virtual-networks-nsg.md)