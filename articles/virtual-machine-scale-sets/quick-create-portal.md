---
title: Início Rápido - Criar um conjunto de dimensionamento de máquinas virtuais no Portal do Azure| Microsoft Docs
description: Saiba como criar rapidamente um conjunto de dimensionamento de máquinas virtuais no Portal do Azure
keywords: conjuntos de dimensionamento de máquinas virtuais
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 9c1583f0-bcc7-4b51-9d64-84da76de1fda
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.devlang: na
ms.topic: quickstart
ms.custom: H1Hack27Feb2017
ms.date: 03/27/18
ms.author: cynthn
ms.openlocfilehash: 7644940418b7322e5ec4b02b793219b44ae3aa97
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54885154"
---
# <a name="quickstart-create-a-virtual-machine-scale-set-in-the-azure-portal"></a>Início rápido: Criar um conjunto de dimensionamento de máquinas virtuais no Portal do Azure
Um conjunto de dimensionamento de máquinas virtuais permite implementar e gerir um conjunto de máquinas virtuais idênticas e de dimensionamento automático. Pode dimensionar o número de VMs no conjunto de dimensionamento manualmente ou definir regras para dimensionar automaticamente com base na utilização de recursos como CPU, exigência de memória ou tráfego de rede. Em seguida, um balanceador de carga do Azure distribui o tráfego pelas instâncias de VM no conjunto de dimensionamento. Neste início rápido, vai criar um conjunto de dimensionamento de máquinas virtuais no Portal do Azure.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.


## <a name="log-in-to-azure"></a>Iniciar sessão no Azure
Inicie sessão no portal do Azure em http://portal.azure.com.


## <a name="create-virtual-machine-scale-set"></a>Criar conjunto de dimensionamento da máquina virtual
Pode implementar um conjunto de dimensionamento com uma imagem do Windows Server ou uma imagem do Linux, como RHEL, CentOS, Ubuntu ou SLES.

1. Clique em **Criar um recurso**, no canto superior esquerdo do portal do Azure.
2. Procure *conjunto de dimensionamento*, escolha **Conjunto de dimensionamento de máquinas virtuais** e selecione **Criar**.
3. Introduza um nome para o conjunto de dimensionamento, como *myScaleSet*.
4. Selecione o tipo de SO pretendido, como *Datacenter do Windows Server 2016*.
5. Introduza o nome do grupo de recursos pretendido, como *myResourceGroup*, e a localização, como *E.U.A. Leste*.
6. Introduza o seu nome de utilizador pretendido e selecione o tipo de autenticação que preferir.
    - A **Palavra-passe** tem de ter, pelo menos, 12 carateres e cumprir três dos quatro requisitos de complexidade seguintes: um caráter em letra minúscula, um caráter em letra maiúscula, um número e um caráter especial. Para obter mais informações, veja [requisitos de nome de utilizador e palavra-passe](../virtual-machines/windows/faq.md#what-are-the-username-requirements-when-creating-a-vm).
    - Se selecionar uma imagem de disco de SO Linux, em vez disso, pode escolher **Chave pública SSH**. Forneça apenas a chave pública, como *~/.ssh/id_rsa.pub*. Pode utilizar o Azure Cloud Shell no portal para [criar e utilizar chaves SSH](../virtual-machines/linux/mac-create-ssh-keys.md).

7. Introduza um **Nome do endereço IP público**, como *myPublicIP*.
8. Introduza uma única **Etiqueta de nome de domínio**, como *myuniquedns*. Esta etiqueta DNS constitui a base do FQDN para o balanceador de carga à frente do conjunto de dimensionamento.
9. Para confirmar as opções do conjunto de dimensionamento, selecione **Criar**.

    ![Criar um conjunto de dimensionamento de máquinas virtuais no Portal do Azure](./media/virtual-machine-scale-sets-create-portal/create-scale-set.png)


## <a name="connect-to-a-vm-in-the-scale-set"></a>Ligar a uma VM no conjunto de dimensionamento
Quando cria um conjunto de dimensionamento no portal, é criado um balanceador de carga. As Regras de Tradução de Endereços de Rede (NAT) são utilizadas para distribuir o tráfego pelas instâncias do conjunto de dimensionamento para conectividade remota, como RDP ou SSH.

Para ver estas regras NAT e as informações da ligação para as instâncias do conjunto de dimensionamento:

1. Selecione o grupo de recursos que criou no passo anterior, como *myResourceGroup*.
2. Na lista de recursos, selecione o **Balanceador de carga**, como *myScaleSetLab*.
3. Escolha **Regras NAT de entrada** no menu no lado esquerdo da janela.

    ![As regras NAT de entrada permitem ligar a instâncias do conjunto de dimensionamento de máquinas virtuais](./media/virtual-machine-scale-sets-create-portal/inbound-nat-rules.png)

Pode ligar a cada VM no conjunto de dimensionamento com estas regras NAT. Cada instância de VM apresenta uma lista de um endereço IP de destino e o valor da porta TCP. Por exemplo, se o endereço IP de destino for *104.42.1.19* e a porta TCP for *50001*, liga à instância de VM da seguinte forma:

- Para um conjunto de dimensionamento do Windows, ligue à instância da VM com o RDP em `104.42.1.19:50001`
- Para um conjunto de dimensionamento do Linux, ligue à instância da VM com o SHH em `ssh azureuser@104.42.1.19 -p 50001`

Quando lhe for pedido, introduza as credenciais que especificou no passo anterior, quando criou o conjunto de dimensionamento. As instâncias do conjunto de dimensionamento são VMs regulares com as quais pode interagir normalmente. Para obter mais informações sobre como implementar e executar aplicações nas suas instâncias do conjunto de dimensionamento, veja [Implementar a sua aplicação num conjunto de dimensionamento de máquinas virtuais](virtual-machine-scale-sets-deploy-app.md)


## <a name="clean-up-resources"></a>Limpar recursos
Quando já não for necessário, elimine o grupo de recursos, o conjunto de dimensionamento, a conta do Batch e todos os recursos relacionados. Para tal, selecione o grupo de recursos para a VM e clique em **Eliminar**.


## <a name="next-steps"></a>Passos Seguintes
Neste início rápido, criou um conjunto de dimensionamento básico no Portal do Azure. Para obter mais informações, avance para o tutorial para saber como criar e gerir conjuntos de dimensionamento de máquinas virtuais do Azure.

> [!div class="nextstepaction"]
> [Criar e gerir conjuntos de dimensionamento de máquinas virtuais do Azure](tutorial-create-and-manage-powershell.md)