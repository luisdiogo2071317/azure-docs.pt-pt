---
title: Adicionar um Cluster do Kubernetes para o mercado do Azure Stack | Documentos da Microsoft
description: Saiba como adicionar um Cluster do Kubernetes no Azure Stack Marketplace.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2018
ms.author: mabrigg
ms.reviewer: waltero
ms.openlocfilehash: bff0140d173fc3393ac56ec3d709c3c067842a2a
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/09/2018
ms.locfileid: "42056712"
---
# <a name="add-a-kubernetes-cluster-to-the-azure-stack-marketplace"></a>Adicionar um Cluster do Kubernetes para o mercado do Azure Stack

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

> [!note]  
> O Kubernetes de serviços de Kubernetes do Azure (AKS) no Azure Stack está em pré-visualização privada. Para pedir acesso para o item do Marketplace de Kubernetes necessário para executar as instruções neste artigo, [submeter um pedido para obter acesso](https://aka.ms/azsk8).

Pode oferecer um Cluster do Kubernetes como um item do mercado aos seus utilizadores. Os utilizadores podem implementar Kubernetes numa operação única e coordenada.

O seguinte artigo examinar o uso de um modelo Azure Resource Manager para implementar e aprovisionar os recursos para um cluster de Kubernetes autónomo. Antes de começar, verifique o Azure Stack e as definições de inquilino do Azure global. Recolha as informações necessárias sobre o Azure Stack. Adicione recursos necessários para o seu inquilino e a pilha do Azure Marketplace. O cluster depende de um Ubuntu server, o script personalizado e os itens de Cluster do Kubernetes para ser no marketplace.

## <a name="create-a-plan-an-offer-and-a-subscription"></a>Criar um plano, uma oferta e uma subscrição

Crie um plano, uma oferta e uma subscrição para o item do Marketplace de Cluster de Kubernetes. Também pode utilizar um plano existente e oferecem.

1. Entrar para a [portal de administração.](https://adminportal.local.azurestack.external)

1. Crie um plano de como o plano de base. Para obter instruções, consulte [criar um plano no Azure Stack](azure-stack-create-plan.md).

1. Crie uma oferta. Para obter instruções, consulte [criar uma oferta no Azure Stack](azure-stack-create-offer.md).

1. Selecione **oferece**e localize a oferta que criou.

1. Selecione **descrição geral** no painel da oferta.

1. Selecione **alterar estado**. Selecione **público**.

1. Selecione **+ novo** > **ofertas e planos** > **subscrição** para criar uma nova subscrição.

    a. Introduza um **nome a apresentar**.

    b. Introduza um **utilizador**. Utilize a conta do Azure AD associada ao seu inquilino.

    c. **Descrição do fornecedor**

    d. Definir o **inquilino Directory** para o inquilino do Azure AD para o Azure Stack. 

    e. Selecione **oferecem**. Selecione o nome da oferta que criou. Tome nota do ID de subscrição.

## <a name="add-an-ubuntu-server-image"></a>Adicionar uma imagem do Ubuntu server

Adicione a imagem de Ubuntu Server seguinte no Marketplace:

1. Inicie sessão para o [portal de administração](https://adminportal.local.azurestack.external).

1. Selecione **mais serviços** > **gestão de Marketplace**.

1. Selecione **+ adicionar a partir do Azure**.

1. Introduza `UbuntuServer`.

1. Selecione o servidor com o seguinte perfil:
    - **Publicador**: Canonical
    - **Oferecer**: UbuntuServer
    - **SKU**: 16.04 LTS
    - **Versão**: 16.04.201802220

    > [!Note]  
    > Poderá ser apresentada mais do que uma versão do Ubuntu Server 16.04 LTS. Terá de adicionar a versão que corresponde à. O Cluster de Kubernetes requer a versão exata do item.

1. Selecione **transferir.**

## <a name="add-a-custom-script-for-linux"></a>Adicionar um script personalizado para Linux

Adicione o Cluster de Kubernetes a partir do Marketplace:

1. Abra o [portal de administração](https://adminportal.local.azurestack.external).

1. Selecione **mais serviços** > **gestão de Marketplace**.

1. Selecione **+ adicionar a partir do Azure**.

1. Introduza `Custom Script for Linux`.

1. Selecione o script com o seguinte perfil:
    - **Oferecer**: Script personalizado para Linux 2.0
    - **Versão**: 2.0.3
    - **Publicador**: Microsoft Corp

    > [!Note]  
    > Mais de uma versão de Script personalizado para Linux pode estar relacionada. Terá de adicionar a versão que corresponde à. O Cluster de Kubernetes requer a versão exata do item.

1. Selecione **transferir.**


## <a name="add-the-kubernetes-cluster-to-the-marketplace"></a>Adicionar o Cluster de Kubernetes no Marketplace

1. Abra o [portal de administração](https://adminportal.local.azurestack.external).

1. Selecione **mais serviços** > **gestão de Marketplace**.

1. Selecione **+ adicionar a partir do Azure**.

1. Introduza `Kubernetes Cluster`.

1. Selecione `Kubernetes Cluster`.

1. Selecione **transferir.**

    > [!note]  
    > Poderá demorar cinco minutos para o item do marketplace a aparecer no Marketplace.

    ![Cluster do Kubernetes](user\media\azure-stack-solution-template-kubernetes-deploy\marketplaceitem.png)

## <a name="update-or-remove-the-kubernetes-cluster"></a>Atualizar ou remover o Cluster de Kubernetes 

Ao atualizar o item de Cluster do Kubernetes, terá de remover o item que esteja no Marketplace. Em seguida, pode seguir as instruções neste artigo para adicionar o Cluster de Kubernetes no Marketplace.

Para remover o item de Cluster do Kubernetes:

1. Anote o nome do item atual, por exemplo `Microsoft.AzureStackKubernetesCluster.0.1.0`

1. Ligar ao Azure Stack com o PowerShell.

1. Utilize o seguinte cmdlet do PowerShell para remover o item:

    ```PowerShell  
    $Itemname="Microsoft.AzureStackKubernetesCluster.0.1.0"

    Remove-AzsGalleryItem -Name $Itemname
    ```

## <a name="next-steps"></a>Passos Seguintes

[Implementar um Cluster de Kubernetes no Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy)



[Descrição geral da oferta de serviços no Azure Stack](azure-stack-offer-services-overview.md)