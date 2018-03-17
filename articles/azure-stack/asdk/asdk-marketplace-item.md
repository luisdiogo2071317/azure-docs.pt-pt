---
title: Adicionar um item do marketplace pilha do Azure a partir do Azure | Microsoft Docs
description: "Descreve como adicionar uma imagem de máquina virtual do Azure com base no Windows Server para a pilha do Azure Marketplace."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 25f179f825526e20377c0e94ccb38a788cadd898
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/17/2018
---
# <a name="tutorial-add-an-azure-stack-marketplace-item-from-azure"></a>Tutorial: adicionar um item do marketplace pilha do Azure a partir do Azure

Como um operador de pilha do Azure, a primeira coisa que precisa de fazer para permitir aos utilizadores implementar uma Máquina Virtual (VM) é adicionar uma imagem VM para o mercado de pilha do Azure. Por predefinição, nada é publicado para o mercado de pilha do Azure, mas pode transferir itens do [uma lista de itens do Azure marketplace organizada](.\.\azure-stack-marketplace-azure-items.md) que foram testadas previamente para executar na pilha do Azure. Utilize esta opção se estão a funcionar num cenário ligado e que registou a instância de pilha do Azure com o Azure.

Neste tutorial, adicione a imagem de VM do Windows Server 2016 do Azure marketplace para o mercado de pilha do Azure.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Adicionar um item do marketplace pilha do Windows Server VM do Azure
> * Certifique-se de que a imagem VM está disponível 
> * A imagem VM de teste

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial:

- Instalar o [módulos do Azure pilha compatível com o Azure PowerShell](asdk-post-deploy.md#install-azure-stack-powershell)
- Transferir o [ferramentas de pilha do Azure](asdk-post-deploy.md#download-the-azure-stack-tools)
- [Registar o ASDK](asdk-register.md) com a sua subscrição do Azure

## <a name="add-a-windows-server-vm-image"></a>Adicionar uma imagem de VM do Windows Server
Pode adicionar uma imagem do Windows Server 2016 para o mercado de pilha do Azure ao transferir a imagem do Azure marketplace. Utilize esta opção se estão a funcionar num cenário ligado e que já tem [registado a sua instância do Azure pilha](asdk-register.md) com o Azure.

1. Iniciar sessão para o [portal de administração ASDK](https://adminportal.local.azurestack.external).

2. Selecione **mais serviços** > **Marketplace gestão** > **adicionar a partir do Azure**. 

   ![Adicionar a partir do Azure](media/asdk-marketplace-item/azs-marketplace.png)

3. Localizar ou procurar o **Datacenter do Windows Server 2016** imagem.

4. Selecione o **Datacenter do Windows Server 2016** imagem e, em seguida, selecione **transferir**.

   ![Transferir a imagem a partir do Azure](media/asdk-marketplace-item/azure-marketplace-ws2016.png)


> [!NOTE]
> Demora aproximadamente uma hora para transferir a imagem VM e publicá-lo para o mercado de pilha do Azure. 

Quando a transferência estiver concluída, a imagem será publicado e está disponível em **Marketplace gestão**. A imagem também está disponível em **computação** para criar novas máquinas virtuais.

## <a name="verify-the-marketplace-item-is-available"></a>Certifique-se de que o item do marketplace está disponível
Utilize estes passos para verificar que a nova imagem VM está disponível no mercado pilha do Azure.

1. Iniciar sessão para o [portal de administração ASDK](https://adminportal.local.azurestack.external).

2. Selecione **mais serviços** > **Marketplace gestão**. 

3. Certifique-se de que a imagem de VM do Windows Server 2016 Centro de dados foi adicionada com êxito.

   ![Imagem transferida a partir do Azure](media/asdk-marketplace-item/azs-marketplace-ws2016.png)

## <a name="test-the-vm-image"></a>A imagem VM de teste
Como um operador de pilha do Azure, pode utilizar o [portal de administrador](https://adminportal.local.azurestack.external) para criar um teste de VM para validar a imagem tem foram disponibilizada com êxito. 

1. Iniciar sessão para o [portal de administração ASDK](https://adminportal.local.azurestack.external).

2. Clique em **novo** > **computação** > **Datacenter do Windows Server 2016** > **criar**.  
 ![Criar a VM de imagem do marketplace](media/asdk-marketplace-item/new-compute.png)

3. No **Noções básicas** painel, introduza as seguintes informações e, em seguida, clique em **OK**:
  - **Name**: test-vm-1
  - **Nome de utilizador**: AdminTestUser
  - **Password**: AzS-TestVM01
  - **Subscrição**: aceitar a subscrição do fornecedor predefinido
  - **Grupo de recursos**: rg de vm de teste
  - **Localização**: local

4. No **escolher um tamanho** painel, clique em **A1 padrão**e, em seguida, clique em **selecione**.  

5. No **definições** painel, aceite as predefinições e clique em **OK**

6. No painel **Resumo**, clique em **OK** para criar a máquina virtual.  
> [!NOTE]
> A implementação da máquina virtual demora alguns minutos a concluir.

7. Para ver a nova VM, clique em **máquinas virtuais**, em seguida, procure **1 de vm de teste** e clique no respetivo nome.
    ![Imagem VM de teste primeiro apresentada](media/asdk-marketplace-item/first-test-vm.png)

## <a name="clean-up-resources"></a>Limpar recursos
Depois de ter com êxito com sessão iniciada para a VM e verificar se a imagem de teste está a funcionar corretamente, deve eliminar o grupo de recursos de teste. Isto irá libertar recursos limitados disponíveis para instalações de ASDK de nó único.

Quando já não é necessário, elimine o grupo de recursos, a VM e todos os recursos relacionados, seguindo estes passos:

1. Iniciar sessão para o [portal de administração ASDK](https://adminportal.local.azurestack.external).
2. Clique em **grupos de recursos** > **rg de vm de teste** > **eliminar grupo de recursos**.
3. Tipo **rg de vm de teste** como o nome do grupo de recursos e, em seguida, clique em **eliminar**.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Adicionar um item do marketplace pilha do Windows Server VM do Azure
> * Certifique-se de que a imagem VM está disponível 
> * A imagem VM de teste

Avançar para o próximo tutorial para saber como criar uma oferta de pilha do Azure e o plano.

> [!div class="nextstepaction"]
> [Oferecer serviços IaaS de pilha do Azure](asdk-offer-services.md)