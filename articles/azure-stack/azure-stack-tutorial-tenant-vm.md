---
title: Máquinas virtuais disponibilizar aos utilizadores de pilha do Azure | Microsoft Docs
description: Saiba como tornar as máquinas virtuais disponíveis na pilha do Azure
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/07/2018
ms.author: jeffgilb
ms.reviewer: ''
ms.custom: mvc
ms.openlocfilehash: 9329cb0dbfa24cf239b820573ef7f642cdca9103
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35248164"
---
# <a name="tutorial-make-virtual-machines-available-to-your-azure-stack-users"></a>Tutorial: disponibilizar máquinas virtuais para os utilizadores de pilha do Azure

Como um administrador da nuvem de pilha do Azure, pode criar ofertas podem subscrever os seus utilizadores (algumas vezes referidos como inquilinos). Ao subscrever uma oferta, os utilizadores podem consumir os serviços de pilha do Azure fornece uma oferta.

Este tutorial mostra como criar uma oferta para uma máquina virtual e, em seguida, inicie sessão como um utilizador para testar a oferta.

O que aprenderá:

> [!div class="checklist"]
> * Criar uma oferta
> * Adicionar uma imagem
> * A oferta de teste

Na pilha do Azure, os serviços são fornecidos aos utilizadores através de subscrições, ofertas e planos. Os utilizadores podem subscrever oferece várias. Uma oferta pode ter um ou mais planos e um plano pode ter um ou mais serviços.

![As subscrições, ofertas e planos](media/azure-stack-key-features/image4.png)

Para obter mais informações, consulte [conceitos na pilha do Azure e as funcionalidades da chave](azure-stack-key-features.md).

## <a name="create-an-offer"></a>Criar uma oferta

Ofertas são grupos de um ou mais planos que Fornecedores aos utilizadores para comprar ou subscrever. O processo de criação de uma oferta tem vários passos. Em primeiro lugar, é-lhe pedido que criar a oferta, em seguida, um plano e, por fim, quotas.

1. [Inicie sessão no](azure-stack-connect-azure-stack.md) no portal como um administrador da nuvem e, em seguida, selecione **novo** > **oferece + planos** > **oferecem**.

   ![Nova oferta](media/azure-stack-tutorial-tenant-vm/image01.png)

2. No **oferecem novo**, introduza um **nome a apresentar** e **nome do recurso**e, em seguida, selecione o novo ou existente **grupo de recursos**. O Nome a Apresentar é o nome amigável da oferta. Apenas o operador da nuvem, pode ver o nome do recurso. Trata-se do nome que o administrador utiliza para trabalhar com a oferta como um recurso do Azure Resource Manager.

   ![Nome a apresentar](media/azure-stack-tutorial-tenant-vm/image02.png)

3. Selecione **Base planos**e, no **plano** secção, selecione **adicionar** para adicionar um novo plano para a oferta.

   ![Adicione um plano](media/azure-stack-tutorial-tenant-vm/image03.png)

4. No **planear novo** secção, preencha **nome a apresentar** e **nome do recurso**. O nome de apresentação é o nome amigável do plano e que os utilizadores veem. Apenas o operador da nuvem, pode ver o nome do recurso. É o nome que utilizam os operadores da nuvem para funcionar com o plano como um recurso do Azure Resource Manager.

   ![Planear o nome a apresentar](media/azure-stack-tutorial-tenant-vm/image04.png)

5. Selecione **serviços**. Na lista de serviços, escolher **Microsoft. Compute**, **Network**, e **Microsoft**. Escolha **selecione** para adicionar estes serviços para o plano.

   ![Planear os serviços](media/azure-stack-tutorial-tenant-vm/image05.png)

6. Selecione **Quotas**e, em seguida, selecione o primeiro serviço que pretende criar uma quota para. Para uma quota de IaaS, utilize o exemplo seguinte como guia para configurar quotas para os serviços de computação, rede e armazenamento.

   - Em primeiro lugar, crie uma quota para o serviço de computação. Na lista de espaço de nomes, selecione **Microsoft. Compute** e, em seguida, selecione **criar nova quota**.

     ![Criar nova quota](media/azure-stack-tutorial-tenant-vm/image06.png)

   - No **Criar quota**, introduza um nome para a quota. Pode alterar ou aceitar a qualquer um dos valores de quota que são apresentados para a quota de que está a criar. Neste exemplo, podemos aceitar as predefinições e selecione **OK**.

     ![Nome de quota](media/azure-stack-tutorial-tenant-vm/image07.png)

   - Escolha **Microsoft. Compute** na lista de espaço de nomes e, em seguida, selecione a quota que criou. Esta ação liga a quota para o serviço de computação.

     ![Selecione quota](media/azure-stack-tutorial-tenant-vm/image08.png)

      Repita estes passos para os serviços de rede e armazenamento. Quando tiver terminado, selecione **OK** no **Quotas** para guardar todas as quotas.

7. No **novo plano**, selecione **OK**.

8. Em **plano**, selecione o novo plano e, em seguida, **selecione**.

9. No **oferta novo**, selecione **criar**. Verá uma notificação quando é criada a oferta.

10. No menu do dashboard, selecione **oferece** e, em seguida, escolha a oferta que criou.

11. Selecione **alteração de estado**e, em seguida, escolha **pública**.

    ![Estado público](media/azure-stack-tutorial-tenant-vm/image09.png)

## <a name="add-an-image"></a>Adicionar uma imagem

Pode aprovisionar as máquinas virtuais, tem de adicionar uma imagem do marketplace de pilha do Azure. Pode adicionar a imagem à sua escolha, incluindo as imagens de Linux, do Azure Marketplace.

Se estão a funcionar num cenário ligado e se registou a instância de pilha do Azure com o Azure, em seguida, pode transferir a imagem de VM do Windows Server 2016 do Azure Marketplace utilizando os passos descritos no [marketplace de transferência itens a partir do Azure a pilha de Azure](azure-stack-download-azure-marketplace-item.md) tópico.

Para obter informações sobre como adicionar itens diferentes para o mercado, consulte [o Azure Marketplace de pilha](azure-stack-marketplace.md).

## <a name="test-the-offer"></a>A oferta de teste

Agora que criou uma oferta, pode testá-lo. Irá iniciar sessão como um utilizador, subscrever a oferta e, em seguida, adicionar uma máquina virtual.

1. **Subscrever uma oferta**

   a. Inicie sessão no portal de utilizador com uma conta de utilizador e selecione o **obter uma subscrição** mosaico.
   - Para um sistema integrado, o URL varia com base na região e nome de domínio externo o operador e vai estar no formato https://portal.&lt; *região*&gt;.&lt; *FQDN*&gt;.
   - Se estiver a utilizar o Kit de desenvolvimento de pilha do Azure, o endereço de portal é https://portal.local.azurestack.external.

   ![Obter uma subscrição](media/azure-stack-subscribe-plan-provision-vm/image01.png)

   b. No **obter uma subscrição**, introduza um nome para a sua subscrição no **nome a apresentar** campo. Selecione **oferecem**e, em seguida, escolha uma das ofertas no **escolher uma oferta** lista. Selecione **Criar**.

   ![Criar uma oferta](media/azure-stack-subscribe-plan-provision-vm/image02.png)

   c. Para ver a subscrição, selecione **mais serviços**e, em seguida, selecione **subscrições**. Selecione a sua nova subscrição para ver quais os serviços fazem parte da subscrição.

   >[!NOTE]
   >Depois de subscrever uma oferta, poderá ter de atualizar o portal para ver quais os serviços fazem parte da nova subscrição.

2. **Aprovisionar uma máquina virtual**

   No portal de utilizador pode aprovisionar uma máquina virtual utilizando a nova subscrição.

   a. Inicie sessão no portal de utilizador com uma conta de utilizador.
      - Para um sistema integrado, o URL varia com base na região e nome de domínio externo o operador e vai estar no formato https://portal.&lt; *região*&gt;.&lt; *FQDN*&gt;.
   - Se estiver a utilizar o Kit de desenvolvimento de pilha do Azure, o endereço de portal é https://portal.local.azurestack.external.

   b.  No dashboard, selecione **novo** > **computação** > **Windows Server 2016 Datacenter Eval**e, em seguida, selecione **criar**.

   c. No **Noções básicas**, forneça as seguintes informações:
      - Introduza um **nome**
      - Introduza um **nome de utilizador**
      - Introduza um **palavra-passe**
      - Escolha um **subscrição**
      - Criar um **grupo de recursos** (ou selecione um existente.) 
      - Selecione **OK** para guardar estas informações.

   d. No **escolher um tamanho**, selecione **A1 padrão**e, em seguida, **selecione**.  

   e. No **definições**, selecione **rede Virtual**.

   f. No **escolha de rede virtual**, selecione **criar nova**.

   g. No **criar rede virtual**, aceite todas as predefinições e selecione **OK**.

   h. Selecione **OK** no **definições** para guardar a configuração de rede.

   ![Criar a rede virtual](media/azure-stack-provision-vm/image04.png)

   i. No **resumo**, selecione **OK** para criar a máquina virtual.  

   j. Para ver a nova máquina virtual, selecione **todos os recursos**. Pesquisa para a máquina virtual e selecione o nome de resultados da pesquisa.

   ![Todos os recursos](media/azure-stack-provision-vm/image06.png)

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Criar uma oferta
> * Adicionar uma imagem
> * A oferta de teste

Avançadas para o próximo tutorial para saber como:
> [!div class="nextstepaction"]
> [Disponibilizar bases de dados SQL para os utilizadores de pilha do Azure](azure-stack-tutorial-sql-server.md)