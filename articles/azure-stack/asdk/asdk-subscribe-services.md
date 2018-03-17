---
title: "Neste tutorial, irá aprender a subscrever uma oferta de pilha do Azure | Microsoft Docs"
description: "Este tutorial mostra como criar uma nova subscrição de serviços de pilha do Azure e a oferta de teste através da criação de uma máquina virtual de teste."
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
ms.openlocfilehash: d2adda5613b9a10bc3314045b9d68b0f3196f19a
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/17/2018
---
# <a name="tutorial-create-and-test-a-subscription"></a>Tutorial: criar e testar uma subscrição
Este tutorial mostra como criar uma subscrição que contém uma oferta e, em seguida, testá-lo. Para o teste, irá iniciar sessão a [portal de utilizador](https://portal.local.azurestack.external) como um administrador da nuvem, subscrever a oferta e, em seguida, criar uma máquina virtual.

> [!TIP]
> Mais uma mais avançadas experiência de avaliação, pode [criar uma subscrição para um determinado utilizador](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm#create-a-subscription-as-a-cloud-operator) e, em seguida, inicie sessão como utilizador no portal de utilizador. 

Este tutorial mostra como subscrever a oferta criada no [tutorial anterior](asdk-offer-services.md).

O que aprenderá:

> [!div class="checklist"]
> * Subscrever uma oferta 
> * A oferta de teste

## <a name="subscribe-to-an-offer"></a>Subscrever uma oferta
Para subscrever uma oferta como um utilizador, terá de iniciar sessão no portal de utilizador de pilha do Azure para detetar os serviços que foram disponibilizados pela operadora de rede de pilha do Azure.

1. Inicie sessão no [portal de utilizador](https://portal.local.azurestack.external) e clique em **obter uma subscrição**.

   ![Obter uma subscrição](media/asdk-subscribe-services/get-subscription.png)

2. No campo **Nome a Apresentar**, introduza um nome para a subscrição. Em seguida, clique em **oferecem** para selecionar uma de ofertas disponíveis no **escolher uma oferta** secção e, em seguida, clique em **criar**.

   ![Criar uma oferta](media/asdk-subscribe-services/create-subscription.png)

   > [!TIP]
   > Agora, deve atualizar o portal de utilizador para começar a utilizar a sua subscrição.

3. Para ver a subscrição que acabou de criar, clique em **mais serviços**, clique em **subscrições**, em seguida, clique em sua nova subscrição. Depois de subscrever uma oferta, atualize o portal para ver se os novos serviços foram incluídos como parte da nova subscrição. Neste exemplo, **máquinas virtuais** foi adicionado.

   ![Vista de subscrição](media/asdk-subscribe-services/view-subscription.png)


## <a name="test-the-offer"></a>A oferta de teste
Enquanto tem sessão iniciada [portal de utilizador](https://portal.local.azurestack.external), pode testar a oferta aprovisionar uma máquina virtual utilizando as novas capacidades de subscrição. 

> [!NOTE]
> Este teste requer a imagem de VM do Windows Server 2016 Centro de dados que foi adicionada para o mercado de pilha do Azure num [tutorial anterior](asdk-marketplace-item.md). 

1. Inicie sessão no [portal de utilizador](https://portal.local.azurestack.external).

2. No portal de utilizador, clique em **máquinas virtuais** > **adicionar** > **Datacenter do Windows Server 2016**e, em seguida, clique em **criar** .

3. No **Noções básicas** secção, escreva um **nome**, **nome de utilizador**, e **palavra-passe**, escolha um **subscrição**, criar um **grupo de recursos** (ou selecione um existente) e, em seguida, clique em **OK**.

4. No **escolher um tamanho** secção, clique em **A1 padrão**e, em seguida, clique em **selecione**.  

5. No painel de definições, aceite as predefinições e clique em **OK**.

6. No **resumo** secção, clique em **OK** para criar a máquina virtual.  

7. Para ver a nova máquina virtual, clique em **máquinas virtuais**, em seguida, procure a nova máquina virtual e clique no respetivo nome.

    ![Todos os recursos](media/asdk-subscribe-services/view-vm.png)

> [!NOTE]
> A implementação da máquina virtual irá demorar alguns minutos a concluir.


## <a name="next-steps"></a>Passos Seguintes

O que aprendeu neste tutorial:

> [!div class="checklist"]
> * Subscrever uma oferta 
> * A oferta de teste

Avançar para o próximo tutorial para saber como criar uma VM utilizando um modelo.

> [!div class="nextstepaction"]
> [Criar uma máquina virtual a partir de um modelo](asdk-create-vm-template.md)