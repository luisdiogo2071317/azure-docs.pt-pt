---
title: Neste tutorial, irá aprender a subscrever uma oferta de pilha do Azure | Microsoft Docs
description: Este tutorial mostra como criar uma nova subscrição de serviços de pilha do Azure e a oferta de teste através da criação de uma máquina virtual de teste.
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
ms.date: 06/07/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: 96b82ede71e7957105dce25096d7873ee876211f
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35238512"
---
# <a name="tutorial-create-and-test-a-subscription"></a>Tutorial: criar e testar uma subscrição
Este tutorial mostra como criar uma subscrição que contém uma oferta e, em seguida, testá-lo. Para o teste, irá iniciar sessão no portal de utilizador de pilha do Azure como um administrador da nuvem, subscrever a oferta e, em seguida, criar uma máquina virtual.

> [!TIP]
> Mais uma mais avançadas experiência de avaliação, pode [criar uma subscrição para um determinado utilizador](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm#create-a-subscription-as-a-cloud-operator) e, em seguida, inicie sessão como utilizador no portal de utilizador. 

Este tutorial mostra como subscrever uma oferta de pilha do Azure.

O que aprenderá:

> [!div class="checklist"]
> * Subscrever uma oferta 
> * A oferta de teste

## <a name="subscribe-to-an-offer"></a>Subscrever uma oferta
Para subscrever uma oferta como um utilizador, terá de iniciar sessão no portal de utilizador de pilha do Azure para detetar os serviços que foram disponibilizados pela operadora de rede de pilha do Azure.

1. Inicie sessão no portal de utilizador e clique em **obter uma subscrição**.

   ![Obter uma subscrição](media/azure-stack-subscribe-services/get-subscription.png)

2. No campo **Nome a Apresentar**, introduza um nome para a subscrição. Em seguida, clique em **oferecem** para selecionar uma de ofertas disponíveis no **escolher uma oferta** secção e, em seguida, clique em **criar**.

   ![Criar uma oferta](media/azure-stack-subscribe-services/create-subscription.png)

   > [!TIP]
   > Agora, deve atualizar o portal de utilizador para começar a utilizar a sua subscrição.

3. Para ver a subscrição que acabou de criar, clique em **mais serviços**, clique em **subscrições**, em seguida, clique em sua nova subscrição. Depois de subscrever uma oferta, atualize o portal para ver se os novos serviços foram incluídos como parte da nova subscrição. Neste exemplo, **máquinas virtuais** foi adicionado.

   ![Vista de subscrição](media/azure-stack-subscribe-services/view-subscription.png)


## <a name="test-the-offer"></a>A oferta de teste
Enquanto tem sessão iniciada portal de utilizador, pode testar a oferta por uma máquina virtual utilizando as novas capacidades de subscrição de aprovisionamento. 

> [!NOTE]
> Este teste necessita que uma VM de centro de dados do Windows Server 2016 primeiro foi adicionada para o mercado de pilha do Azure. 

1. Inicie sessão no portal de utilizador.

2. No portal de utilizador, clique em **máquinas virtuais** > **adicionar** > **Datacenter do Windows Server 2016**e, em seguida, clique em **criar** .

3. No **Noções básicas** secção, escreva um **nome**, **nome de utilizador**, e **palavra-passe**, escolha um **subscrição**, criar um **grupo de recursos** (ou selecione um existente) e, em seguida, clique em **OK**.

4. No **escolher um tamanho** secção, clique em **A1 padrão**e, em seguida, clique em **selecione**.  

5. No painel de definições, aceite as predefinições e clique em **OK**.

6. No **resumo** secção, clique em **OK** para criar a máquina virtual.  

7. Para ver a nova máquina virtual, clique em **máquinas virtuais**, em seguida, procure a nova máquina virtual e clique no respetivo nome.

    ![Todos os recursos](media/azure-stack-subscribe-services/view-vm.png)

> [!NOTE]
> A implementação da máquina virtual irá demorar alguns minutos a concluir.


## <a name="next-steps"></a>Passos Seguintes

O que aprendeu neste tutorial:

> [!div class="checklist"]
> * Subscrever uma oferta 
> * A oferta de teste


> [!div class="nextstepaction"]
> [Criar uma VM a partir de um modelo de Comunidade](azure-stack-create-vm-template.md)