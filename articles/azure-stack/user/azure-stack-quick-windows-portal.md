---
title: Rápido do Azure de pilha iniciar - criar uma máquina virtual do Windows
description: Rápido do Azure de pilha iniciar - criar uma VM do Windows através do portal
services: azure-stack
author: brenduns
manager: femila
ms.service: azure-stack
ms.topic: quickstart
ms.date: 04/23/2018
ms.author: brenduns
ms.reviewer: ''
ms.custom: mvc
ms.openlocfilehash: 5776fc472483018eb2c9e4f8962d0b1e8bce8081
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="quickstart-create-a-windows-server-virtual-machine-with-the-azure-stack-portal"></a>Início rápido: criar uma máquina virtual do Windows server com o portal de pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Pode criar uma máquina virtual do Windows Server 2016 com o portal de pilha do Azure. Siga os passos neste artigo para criar e utilizar uma máquina virtual.

## <a name="sign-in-to-the-azure-stack-portal"></a>Inicie sessão no portal do Azure pilha

Inicie sessão no portal do Azure pilha. O endereço do portal do Azure pilha depende no produto que pilha do Azure está a ligar a:

* Para o Azure pilha Development Kit (ASDK), aceda a: https://portal.local.azurestack.external.
* Para um sistema de pilha do Azure integrado, avance para o URL que o operador de pilha do Azure fornecido.

## <a name="create-a-virtual-machine"></a>Criar uma máquina virtual

1. Clique em **novo** > **computação** > **Datacenter do Windows Server 2016 Eval** > **criar**. Se não vir **Windows Server 2016 Datacenter Eval** entrada, contacte o operador de pilha do Azure. Peça que eles adicioná-lo para o mercado conforme explicado no [adicionar a imagem de VM do Windows Server 2016 para o mercado de pilha do Azure](../azure-stack-add-default-image.md) artigo.

    ![Passos para criar uma máquina virtual do Windows no portal](media/azure-stack-quick-windows-portal/image01.png)
2. Em **Noções básicas**, escreva um **nome**, **nome de utilizador**, e **palavra-passe**. Escolha um **subscrição**. Criar um **grupo de recursos**, ou selecione um existente um, selecione um **localização**e, em seguida, clique em **OK**.

    ![Configurar as definições básicas](media/azure-stack-quick-windows-portal/image02.png)
3. Em **escolher um tamanho**, clique em **D1 padrão** > **selecione**.
    ![Escolha o tamanho da máquina virtual](media/azure-stack-quick-windows-portal/image03.png)
4. Em **definições**, aceite as predefinições e clique em **OK**.
    ![Configurar definições da máquina virtual](media/azure-stack-quick-windows-portal/image04.png)
5. Em **resumo**, clique em **OK** para criar a máquina virtual.
    ![Ver resumo e criar a máquina virtual](media/azure-stack-quick-windows-portal/image05.png)
6. Para ver a nova máquina virtual, clique em **todos os recursos**, procure o nome da máquina virtual e, em seguida, clique no respetivo nome nos resultados da pesquisa.
    ![Consulte a máquina virtual](media/azure-stack-quick-windows-portal/image06.png)

## <a name="clean-up-resources"></a>Limpar recursos

Quando tiver terminado de utilizar a máquina virtual, elimine a máquina virtual e os respetivos recursos. Para tal, selecione o grupo de recursos na página de máquina virtual e clique em **eliminar**.

## <a name="next-steps"></a>Passos Seguintes

Este guia de introdução, implementada uma máquina de virtual básica do Windows Server. Para saber mais sobre as máquinas virtuais de pilha do Azure, avance para [considerações para máquinas virtuais no Azure pilha](azure-stack-vm-considerations.md).
