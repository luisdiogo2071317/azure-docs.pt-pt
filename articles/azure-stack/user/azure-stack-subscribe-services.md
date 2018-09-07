---
title: Neste tutorial, irá aprender a subscrever uma oferta do Azure Stack | Documentos da Microsoft
description: Este tutorial mostra-lhe como criar uma nova subscrição para serviços do Azure Stack e testar a oferta através da criação de uma máquina virtual de teste.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 09/05/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: 0e2fa9b01d27d68c1eab9097a20b6e350ba47f99
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/06/2018
ms.locfileid: "44028819"
---
# <a name="tutorial-create-and-test-a-subscription"></a>Tutorial: criar e testar uma subscrição
Este tutorial mostra-lhe como criar uma subscrição que contém uma oferta e, em seguida, testá-lo. Para o teste, irá iniciar sessão no portal de utilizador do Azure Stack como um administrador da nuvem, subscreva a oferta e, em seguida, criar uma máquina virtual.

> [!TIP]
> Para obter mais uma mais avançada experiência de avaliação, pode [criar uma subscrição para um usuário específico](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm#create-a-subscription-as-a-cloud-operator) e, em seguida, inicie sessão como utilizador no portal de utilizador. 

Este tutorial mostra como subscrever uma oferta do Azure Stack.

O que irá aprender:

> [!div class="checklist"]
> * Subscrever uma oferta 
> * Testar a oferta

## <a name="subscribe-to-an-offer"></a>Subscrever uma oferta
Para subscrever uma oferta como um utilizador, terá de iniciar sessão no portal de utilizador do Azure Stack para descobrir os serviços que têm sido oferecidos pela operadora de rede do Azure Stack.

1. Inicie sessão no portal de utilizador e clique em **obter uma subscrição**.

   ![Obter uma subscrição](media/azure-stack-subscribe-services/get-subscription.png)

2. No campo **Nome a Apresentar**, introduza um nome para a subscrição. Em seguida, clique em **oferecem** para selecionar uma das ofertas disponíveis no **escolher uma oferta** secção e, em seguida, clique em **criar**.

   ![Criar uma oferta](media/azure-stack-subscribe-services/create-subscription.png)

   > [!TIP]
   > Agora, tem de atualizar o portal de utilizador para começar a utilizar a sua subscrição.

3. Para ver a subscrição que criou, clique em **todos os serviços**.  Em seguida, no **gerais** selecionar categoria **subscrições**e, em seguida, selecione a sua nova subscrição. Depois de subscrever uma oferta, atualize o portal para ver se os novos serviços tenham sido incluídos como parte da nova subscrição. Neste exemplo, **máquinas virtuais** foi adicionado.

   ![Vista de subscrição](media/azure-stack-subscribe-services/view-subscription.png)


## <a name="test-the-offer"></a>Testar a oferta
Com sessão iniciada portal de utilizador, pode testar a oferta ao aprovisionar uma máquina virtual utilizando os novos recursos de subscrição. 

> [!NOTE]
> Este teste requer que primeiro foi adicionada uma VM do Windows Server 2016 Datacenter para o mercado do Azure Stack. 

1. Inicie sessão no portal de utilizador.

2. No portal de utilizador, clique em **máquinas virtuais** > **Add** > **Windows Server 2016 Datacenter**e, em seguida, clique em **Create** .

3. Na **Noções básicas** secção, escreva um **nome**, **nome de utilizador**, e **palavra-passe**, escolha um **subscrição**, criar uma **grupo de recursos** (ou selecione um existente) e, em seguida, clique em **OK**.

4. Na **escolher um tamanho** secção, clique em **A1 padrão**e, em seguida, clique em **selecionar**.  

5. No painel Definições, aceite as predefinições e clique em **OK**.

6. Na **resumo** secção, clique em **OK** para criar a máquina virtual.  

7. Para ver a sua nova máquina virtual, clique em **máquinas virtuais**, em seguida, procure a nova máquina virtual e clique no respetivo nome.

    ![Todos os recursos](media/azure-stack-subscribe-services/view-vm.png)

> [!NOTE]
> A implementação da máquina virtual irá demorar alguns minutos a concluir.


## <a name="next-steps"></a>Passos Seguintes

O que aprendeu neste tutorial:

> [!div class="checklist"]
> * Subscrever uma oferta 
> * Testar a oferta


> [!div class="nextstepaction"]
> [Criar uma VM a partir de um modelo de Comunidade](azure-stack-create-vm-template.md)