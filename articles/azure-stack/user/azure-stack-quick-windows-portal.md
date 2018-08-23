---
title: Rápido de pilha do Azure - criar uma máquina virtual do Windows
description: Rápido de pilha do Azure - criar uma VM do Windows com o portal
services: azure-stack
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.topic: quickstart
ms.date: 08/15/2018
ms.author: mabrigg
ms.reviewer: ''
ms.custom: mvc
ms.openlocfilehash: efe6213e5c0261fb26ac40e74c2b0f6e0c9252dd
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/16/2018
ms.locfileid: "42139570"
---
# <a name="quickstart-create-a-windows-server-virtual-machine-with-the-azure-stack-portal"></a>Início rápido: criar uma máquina virtual do Windows server com o portal do Azure Stack

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Pode criar uma máquina virtual do Windows Server 2016 com o portal do Azure Stack. Siga os passos neste artigo para criar e utilizar uma máquina virtual.

## <a name="sign-in-to-the-azure-stack-portal"></a>Inicie sessão no portal do Azure Stack

Inicie sessão no portal do Azure Stack. O endereço do portal do Azure Stack depende de qual produto do Azure Stack ao qual está a ligar:

* Para o Azure Stack Development Kit (ASDK), aceda a: https://portal.local.azurestack.external.
* Para um sistema integrado do Azure Stack, vá para o URL que o operador do Azure Stack fornecido.

## <a name="create-a-virtual-machine"></a>Criar uma máquina virtual

1. Clique em **novos** > **computação** > **Eval do Windows Server 2016 Datacenter** > **criar**. Se não vir **Windows Server 2016 Datacenter Eval** entrada, entre em contato com o operador do Azure Stack. Peça que eles adicioná-la no Marketplace, conforme explicado no [adicionar a imagem de VM do Windows Server 2016 para o mercado do Azure Stack](../azure-stack-add-default-image.md) artigo.

    ![Passos para criar uma máquina virtual do Windows no portal](media/azure-stack-quick-windows-portal/image01.png)
2. Sob **Noções básicas**, escreva um **nome**, **nome de utilizador**, e **palavra-passe**. Escolher uma **subscrição**. Criar uma **grupo de recursos**, ou selecione um existente um, selecione uma **localização**e, em seguida, clique em **OK**.

    ![Configurar as definições básicas](media/azure-stack-quick-windows-portal/image02.png)
3. Sob **escolher um tamanho**, clique em **D1 Standard** > **selecionar**.
    ![Escolha o tamanho da máquina virtual](media/azure-stack-quick-windows-portal/image03.png)
4. Sob **configurações**, aceite as predefinições e clique em **OK**.
    ![Configurar definições da máquina virtual](media/azure-stack-quick-windows-portal/image04.png)
5. Sob **resumo**, clique em **OK** para criar a máquina virtual.
    ![Ver resumo e criar a máquina virtual](media/azure-stack-quick-windows-portal/image05.png)
6. Para ver a sua nova máquina virtual, clique em **todos os recursos**, procure o nome de máquina virtual e, em seguida, clique no respetivo nome nos resultados da pesquisa.
    ![Ver a máquina virtual](media/azure-stack-quick-windows-portal/image06.png)

## <a name="clean-up-resources"></a>Limpar recursos

Quando tiver terminado com a máquina virtual, elimine a máquina virtual e os respetivos recursos. Para tal, selecione o grupo de recursos na página de máquina virtual e clique em **eliminar**.

## <a name="next-steps"></a>Passos Seguintes

Neste início rápido, implementou uma máquina de virtual básica do Windows Server. Para saber mais sobre as máquinas virtuais do Azure Stack, avance para [considerações para máquinas virtuais no Azure Stack](azure-stack-vm-considerations.md).
