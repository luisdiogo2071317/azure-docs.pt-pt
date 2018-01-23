---
title: Gerir contas de programador, utilizar grupos na API Management do Azure | Microsoft Docs
description: Saiba como gerir contas de programador, utilizar grupos na API Management do Azure
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/17/2018
ms.author: apimpm
ms.openlocfilehash: 5fa4825db7216f4b6e2d833c64d5835d6cef93a6
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/22/2018
---
# <a name="how-to-create-and-use-groups-to-manage-developer-accounts-in-azure-api-management"></a>Como criar e utilizar grupos para gerir contas de programador na API Management do Azure
Na API Management, os grupos são utilizados para gerir a visibilidade dos produtos para os programadores. Produtos primeiro ficam visíveis para os grupos, e, em seguida, os programadores esses grupos podem ver e subscrever os produtos que estão associados os grupos. 

A Gestão de API tem os seguintes grupos de sistema imutáveis:

* **Administradores** – os administradores da subscrição do Azure são membros deste grupo. Os administradores gerem as instâncias de serviço de API Management, criando as APIs, as operações e os produtos que são utilizados pelos programadores.
* **Programadores** – os utilizadores autenticados do portal do programador enquadram-se neste grupo. Os programadores são os clientes que criam aplicações utilizando as suas APIs. Os programadores têm acesso ao portal do programador e criam aplicações que chamam as operações de uma API
* **Convidados** – os utilizadores não autenticados do portal do programador, tais como potenciais clientes que visitam o portal do programador de uma instância de API Management, enquadram-se neste grupo. Estes podem ter determinados acessos só de leitura, como a capacidade de ver APIs mas não de as chamar.

Para além destes grupos de sistema, os administradores podem criar grupos personalizados ou [tirar partido de grupos externos em inquilinos do Azure Active Directory associados][leverage external groups in associated Azure Active Directory tenants]. É possível utilizar grupos personalizados e externos em conjunto com grupos de sistema para dar aos programadores acesso e visibilidade a produtos de API. Por exemplo, pode criar um grupo personalizado para os programadores associados a uma organização parceira específica e permitir-lhes o acesso às APIs a partir de um produto que contém apenas APIs relevantes. Um utilizador pode ser membro de mais do que um grupo.

Este guia mostra como os administradores de uma instância de API Management podem adicionar novos grupos e associá-las com produtos e os programadores.

Além de criar e gerir grupos no portal do publicador, pode criar e gerir os grupos utilizando a API de REST de gestão de API [grupo](https://msdn.microsoft.com/library/azure/dn776329.aspx) entidade.

## <a name="prerequisites"></a>Pré-requisitos

Concluir tarefas neste artigo: [criar uma instância de API Management do Azure](get-started-create-service-instance.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-group"></a>Criar um grupo

Esta secção mostra como adicionar um novo grupo à sua conta de gestão de API.

1. Selecione o **grupos** separador à esquerda do ecrã.
2. Clique em **+ adicionar**.
3. Introduza um nome exclusivo para o grupo e uma descrição opcional.
4. Prima **Criar**.

    ![Adicionar novo grupo](./media/api-management-howto-create-groups/groups001.png)

Quando o grupo for criado, é adicionado ao **grupos** lista. <br/>Para editar o **nome** ou **Descrição** do grupo, clique no nome do grupo e **definições**.<br/>Para eliminar o grupo, clique no nome do grupo e prima **eliminar**.

Agora que o grupo for criado, pode ser associado com produtos e os programadores.

## <a name="associate-group-product"></a>Associar um grupo com um produto

1. Selecione o **produtos** separador à esquerda.
2. Clique no nome do produto pretendido.
3. Prima **controlo de acesso**.
4. Clique em **+ adicionar grupo**.

    ![Adicionar novo grupo](./media/api-management-howto-create-groups/groups002.png)
5. Selecione o grupo que pretende adicionar.

    ![Adicionar novo grupo](./media/api-management-howto-create-groups/groups003.png)

    Para remover um grupo do produto, clique em **eliminar**.

    ![Eliminar um grupo](./media/api-management-howto-create-groups/groups004.png)

Depois de um produto é associado um grupo, os programadores nesse grupo podem ver e subscrever o produto.

> [!NOTE]
> Para adicionar grupos do Active Directory do Azure, consulte [como autorizar contas de programador na API Management do Azure utilizando o Azure Active Directory](api-management-howto-aad.md).

## <a name="associate-group-developer"></a>Associar grupos a programadores

Esta secção mostra como associar grupos membros.

1. Selecione o **grupos** separador à esquerda do ecrã.
2. Selecione **membros**.

    ![Adicionar um membro](./media/api-management-howto-create-groups/groups005.png)
3. Prima **+ adicionar** e selecione um membro.

    ![Adicionar um membro](./media/api-management-howto-create-groups/groups006.png)
4. Prima **selecione**.


Assim que é adicionada a associação entre o programador e o grupo, pode vê-la no **utilizadores** separador.

## <a name="next-steps"> </a>Passos seguintes
* Assim que um programador é adicionado a um grupo, pode ver e subscrever os produtos associados a esse grupo. Para obter mais informações, consulte [como criar e publicar um produto na API Management do Azure][How create and publish a product in Azure API Management],
* Além de criar e gerir grupos no portal do publicador, pode criar e gerir os grupos utilizando a API de REST de gestão de API [grupo](https://msdn.microsoft.com/library/azure/dn776329.aspx) entidade.

[Create a group]: #create-group
[Associate a group with a product]: #associate-group-product
[Associate groups with developers]: #associate-group-developer
[Next steps]: #next-steps

[How create and publish a product in Azure API Management]: api-management-howto-add-products.md

[Get started with Azure API Management]: get-started-create-service-instance.md
[Create an API Management service instance]: get-started-create-service-instance.md
[leverage external groups in associated Azure Active Directory tenants]: api-management-howto-aad.md#how-to-add-an-external-azure-active-directory-group
