---
title: "Criar um conjunto de dimensionamento de Máquina Virtual no portal do Azure | Microsoft Docs"
description: "Saiba como criar rapidamente um dimensionamento da máquina virtual no portal do Azure"
keywords: "Conjuntos de dimensionamento de máquina virtual"
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 9c1583f0-bcc7-4b51-9d64-84da76de1fda
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.devlang: na
ms.topic: article
ms.date: 12/19/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a501a852a317ec7d087904c3a675ebefce1bece0
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2018
---
# <a name="create-a-virtual-machine-scale-set-in-the-azure-portal"></a>Criar um conjunto de dimensionamento de Máquina Virtual no portal do Azure
Um conjunto de dimensionamento de máquinas virtuais permite implementar e gerir um conjunto de máquinas virtuais idênticas e de dimensionamento automático. Pode dimensionar o número de VMs no conjunto de dimensionamento manualmente ou definir regras para dimensionar automaticamente com base na utilização de recursos, como CPU, exigência de memória ou tráfego de rede. Neste artigo Introdução ao obter, criar um conjunto no portal do Azure de dimensionamento de máquina virtual. Também pode criar uma escala definida com o [Azure CLI 2.0](virtual-machine-scale-sets-create-cli.md) ou [Azure PowerShell](virtual-machine-scale-sets-create-powershell.md).

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.


## <a name="log-in-to-azure"></a>Iniciar sessão no Azure
Inicie sessão no portal do Azure em http://portal.azure.com.


## <a name="create-virtual-machine-scale-set"></a>Criar conjunto de dimensionamento da máquina virtual
Pode implementar um conjunto com uma imagem do Windows Server ou Linux imagem como RHEL, CentOS, Ubuntu ou SLES de dimensionamento.

1. Clique no botão **Novo** localizado no canto superior esquerdo do portal do Azure.
2. Procurar *conjunto de dimensionamento*, escolha **conjunto de dimensionamento da Máquina Virtual**, em seguida, selecione **criar**.
3. Introduza um nome para o conjunto de dimensionamento, tais como *myScaleSet*.
4. Selecione o tipo de SO pretendido, tais como *Datacenter do Windows Server 2016*.
5. Introduza o nome do grupo de recursos desejados, tais como *myResourceGroup*e a localização, tais como *EUA Leste*.
6. Introduza o seu nome de utilizador pretendido e selecione o tipo de autenticação que preferir.
    - A **palavra-passe** tem de ser, pelo menos, 12 carateres longos e cumprir os três fora os seguintes requisitos de complexidade quatro: uma minúscula, uma maiúscula, um número e um caráter especial. Para obter mais informações, consulte [requisitos de nome de utilizador e palavra-passe](../virtual-machines/windows/faq.md#what-are-the-username-requirements-when-creating-a-vm).
    - Se selecionar uma imagem de disco de SO Linux, em vez disso, pode escolher **chave pública SSH**. Fornecer apenas a chave pública, como *~/.ssh/id_rsa.pub*. Pode utilizar a Shell de nuvem do Azure no portal para [criar e utilizar chaves SSH](../virtual-machines/linux/mac-create-ssh-keys.md).

7. Introduza um **nome do endereço IP público**, tais como *myPublicIP*.
8. Introduza um único **etiqueta de nome de domínio**, tais como *myuniquedns*. Esta etiqueta DNS compõe a base do FQDN para o Balanceador de carga à frente do conjunto de dimensionamento.
9. Para confirmar a escala definir as opções, selecione **criar**.

    ![Criar um conjunto no portal do Azure de dimensionamento de máquina virtual](./media/virtual-machine-scale-sets-create-portal/create-scale-set.png)


## <a name="connect-to-a-vm-in-the-scale-set"></a>Ligar a uma VM no conjunto de dimensionamento
Quando cria um conjunto no portal de dimensionamento, é criado um balanceador de carga. Regras de tradução de endereços (NAT) de rede são utilizadas para distribuir o tráfego para as instâncias do conjunto de dimensionamento de conectividade remota, tais como RDP ou SSH.

Para ver estas NAT regras e as informações da ligação para a escala definir instâncias:

1. Selecione o grupo de recursos que criou no passo anterior, tais como *myResourceGroup*.
2. Na lista de recursos, selecione o **Balanceador de carga**, tais como *myScaleSetLab*.
3. Escolha **regras NAT de entrada** no menu no lado esquerdo da janela.

    ![Regras NAT de entrada permitem-lhe ligar a instâncias de conjunto de dimensionamento de máquina virtual](./media/virtual-machine-scale-sets-create-portal/inbound-nat-rules.png)

Pode ligar para cada VM em escala definida utilizando estas regras NAT. Cada instância VM apresenta uma lista de um endereço IP de destino e o valor da porta TCP. Por exemplo, se o endereço IP de destino é *104.42.1.19* e a porta TCP é *50001*, ligar a instância VM da seguinte forma:

- Para um conjunto de dimensionamento do Windows, ligar-se para a instância VM com o RDP no`104.42.1.19:50001`
- Para um conjunto de dimensionamento do Linux, ligar-se para a instância VM com o SSH no`ssh azureuser@104.42.1.19 -p 50001`

Quando lhe for pedido, introduza as credenciais que especificou no passo anterior, quando criou o conjunto de dimensionamento. As instâncias do conjunto de dimensionamento são VMs regulares que pode interagir com como normal. Para obter mais informações sobre como implementar e executar aplicações no seu dimensionamento instâncias do conjunto, consulte [implementar a sua aplicação em conjuntos de dimensionamento de máquina virtual](virtual-machine-scale-sets-deploy-app.md)


## <a name="clean-up-resources"></a>Limpar recursos
Quando já não for necessário, elimine o grupo de recursos, o conjunto de dimensionamento e todos os recursos relacionados. Para tal, selecione o grupo de recursos para a VM e clique em **Eliminar**.


## <a name="next-steps"></a>Passos Seguintes
Neste artigo Introdução ao obter, criar uma escala básica definida no portal do Azure. Para maior escalabilidade e automatização, expanda o conjunto de dimensionamento com os seguintes artigos de procedimentos:

- [Implementar uma aplicação em conjuntos de dimensionamento de máquinas virtuais](virtual-machine-scale-sets-deploy-app.md)
- Dimensionar automaticamente com o [Azure PowerShell](virtual-machine-scale-sets-autoscale-powershell.md), a [CLI do Azure](virtual-machine-scale-sets-autoscale-cli.md) ou o [portal do Azure](virtual-machine-scale-sets-autoscale-portal.md)
- [Utilizar as atualizações automáticas de SO para as instâncias de VM do seu conjunto de dimensionamento](virtual-machine-scale-sets-automatic-upgrade.md)
