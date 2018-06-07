---
title: Adicionar um Cluster de Kubernetes a pilha do Azure Marketplace | Microsoft Docs
description: Saiba como adicionar um Cluster de Kubernetes a pilha do Azure Marketplace.
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
ms.date: 05/29/2018
ms.author: mabrigg
ms.reviewer: waltero
ms.openlocfilehash: 0ba0e1f3d9e0f1cbb6ba4109a21fc29dc41df5fc
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34603458"
---
# <a name="add-a-kubernetes-cluster-to-the-azure-stack-marketplace"></a>Adicionar um Cluster de Kubernetes a pilha do Azure Marketplace

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

> [!note]  
> O Kubernetes de serviços de contentor do Azure (ACS) na pilha do Azure está em pré-visualização privada. Para pedir acesso para o item do Kubernetes Marketplace necessário para desempenhar as instruções neste artigo, [submeter um pedido para obter acesso](https://aka.ms/azsk8).

Pode oferecer um Cluster de Kubernetes como um item do Marketplace aos seus utilizadores. Os utilizadores podem implementar Kubernetes numa operação única e coordenada.

O seguinte artigo observe utilizando um modelo Azure Resource Manager para implementar e aprovisionar os recursos para um cluster de Kubernetes autónomo. Antes de começar, verifique as definições de global de inquilino do Azure e de pilha do Azure. Recolha as informações necessárias sobre a pilha do Azure. Adicione recursos necessários para o seu inquilino e para a pilha do Azure Marketplace. O cluster depende de um servidor de Ubuntu, script personalizado e os itens de Kubernetes Cluster ser no marketplace.

## <a name="create-a-plan-an-offer-and-a-subscription"></a>Criar um plano, uma oferta e uma subscrição

Crie um plano, uma oferta e uma subscrição para o item do Marketplace de Cluster Kubernetes. Também pode utilizar um plano existente e oferecem.

1. Iniciar sessão para o [do portal de administração.](https://adminportal.local.azurestack.external)

2. Crie um plano como o plano de base. Para obter instruções, consulte [criar um plano na pilha de Azure](azure-stack-create-plan.md).

3. Crie uma oferta. Para obter instruções, consulte [criar uma oferta no Azure pilha](azure-stack-create-offer.md).

4. Selecione **oferece**e localize a oferta que criou.

5. Selecione **descrição geral** no painel da oferta.

6. Selecione **alterar o estado**. Selecione **pública**.

7. Selecione **+ novo** > **oferece e planos** > **subscrição** para criar uma nova subscrição.

    a. Introduza um **nome a apresentar**.

    b. Introduza um **utilizador**. Utilize a conta do Azure AD associada ao seu inquilino.

    c. **Descrição do fornecedor**

    d. Definir o **inquilino do** ao inquilino do Azure AD para a pilha do Azure. 

    e. Selecione **oferecem**. Selecione o nome da oferta que criou. Anote o ID de subscrição.

## <a name="add-an-ubuntu-server-image"></a>Adicionar uma imagem do Ubuntu server

Adicione a imagem de Ubuntu Server seguinte no Marketplace:

1. Iniciar sessão para o [do portal de administração](https://adminportal.local.azurestack.external).

2. Selecione **mais serviços** > **Marketplace gestão**.

3. Selecione **+ adicionar a partir do Azure**.

4. Introduza `UbuntuServer`.

5. Selecione o servidor com o seguinte perfil:
    - **Publicador**: canónico
    - **Oferecer**: UbuntuServer
    - **SKU**: 16.04 LTS
    - **Versão**: 16.04.201802220

    > [!Note]  
    > Mais do que uma versão do Ubuntu Server 16.04 LTS pode ser apresentada. Terá de adicionar a versão que corresponde ao. O Cluster de Kubernetes requer a versão exata do item.

6. Selecione **transferir.**

## <a name="add-a-custom-script-for-linux"></a>Adicionar um script personalizado para Linux

Adicione o Cluster de Kubernetes do Marketplace:

1. Abra o [do portal de administração](https://adminportal.local.azurestack.external).

2. Selecione **mais serviços** > **Marketplace gestão**.

3. Selecione **+ adicionar a partir do Azure**.

4. Introduza `Custom Script for Linux`.

5. Selecione o script com o seguinte perfil:
    - **Oferecer**: Script personalizado para o Linux 2.0
    - **Versão**: 2.0.3
    - **Publicador**: Microsoft Corp

    > [!Note]  
    > Mais de uma versão de Script personalizado para Linux pode ser apresentada. Terá de adicionar a versão que corresponde ao. O Cluster de Kubernetes requer a versão exata do item.

6. Selecione **transferir.**


## <a name="add-the-kubernetes-cluster-to-the-marketplace"></a>Adicionar o Cluster de Kubernetes no Marketplace

1. Abra o [do portal de administração](https://adminportal.local.azurestack.external).

2. Selecione **mais serviços** > **Marketplace gestão**.

3. Selecione **+ adicionar a partir do Azure**.

4. Introduza `Kubernetes Cluster`.

5. Selecione `Kubernetes Cluster`.

6. Selecione **transferir.**

    > [!note]  
    > Poderá demorar cinco minutos para que o item do marketplace a aparecer no Marketplace.

    ![Kubernetes Cluster](user\media\azure-stack-solution-template-kubernetes-deploy\marketplaceitem.png)

## <a name="update-or-remove-the-kubernetes-cluster"></a>Atualizar ou remover o Kubernetes Cluster 

Ao atualizar o item de Kubernetes Cluster, terá de remover o item que está no Marketplace. Em seguida, pode seguir as instruções neste artigo para adicionar o Cluster de Kubernetes no Marketplace.

Para remover o item de Kubernetes Cluster:

1. Anote o nome do item atual, tal como `Microsoft.AzureStackKubernetesCluster.0.1.0`

2. Ligar a pilha do Azure com o PowerShell.

3. Utilize o seguinte cmdlet do PowerShell para remover o item:

    ```PowerShell  
    $Itemname="Microsoft.AzureStackKubernetesCluster.0.1.0"

    Remove-AzsGalleryItem -Name $Itemname
    ```

## <a name="next-steps"></a>Passos Seguintes

[Implementar um Cluster de Kubernetes à pilha do Azure](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy)



[Descrição geral da oferta de serviços na pilha do Azure](azure-stack-offer-services-overview.md)