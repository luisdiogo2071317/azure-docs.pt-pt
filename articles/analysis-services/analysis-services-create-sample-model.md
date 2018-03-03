---
title: Adicionar um modelo de tabela de exemplo para o servidor de Analysis Services do Azure | Microsoft Docs
description: Saiba como adicionar um modelo de exemplo no Azure Analysis Services.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 02/28/2018
ms.author: owend
ms.openlocfilehash: df83f5dd86d1edf857378ae69b16a86b57f9a2fe
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/02/2018
---
# <a name="tutorial-add-a-sample-model"></a>Tutorial: Adicionar um modelo de exemplo

Neste tutorial, adicionar um modelo do exemplo Adventure Works ao seu servidor. O modelo de exemplo é uma versão completa dos dados (1200) Adventure Works Internet vendas modelação tutorial. Um modelo de exemplo é útil para testar a gestão de modelo, ligar-se com ferramentas e aplicações de cliente e consultar os dados do modelo.

## <a name="before-you-begin"></a>Antes de começar

Para concluir este tutorial, precisa de:

- Um servidor de Analysis Services do Azure
- Permissões de administrador do servidor

## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Inicie sessão no [portal do Azure](https://portal.azure.com/).

## <a name="create-a-sample-model"></a>Criar um modelo de exemplo

1. No servidor **descrição geral**, clique em **novo modelo**.

    ![Criar um modelo de exemplo](./media/analysis-services-create-sample-model/aas-create-sample-new-model.png)

2. No **novo modelo** > **escolha uma origem de dados**, certifique-se **dados de exemplo** está selecionada e, em seguida, clique em **adicionar**.

    ![Selecione os dados de exemplo](./media/analysis-services-create-sample-model/aas-create-sample-data.png)

3. No **descrição geral**, certifique-se a `adventureworks` exemplo é criado.

    ![Selecione os dados de exemplo](./media/analysis-services-create-sample-model/aas-create-sample-verify.png)

## <a name="clean-up-resources"></a>Limpar recursos

O modelo de exemplo está a utilizar recursos de memória de cache. Se não estiver a utilizar o seu modelo de exemplo para fins de teste, deve removê-lo a partir do seu servidor.

> [!NOTE]
> Estes passos descrevem como eliminar um modelo a partir de um servidor utilizando o SSMS. Também pode eliminar um modelo ao utilizar a funcionalidade Web do estruturador de pré-visualização.

1. No SSMS > **Object Explorer**, clique em **Connect** > **Analysis Services**.

2. No **ligar ao servidor**, no nome do servidor, em seguida, cole no **autenticação**, escolha **do Active Directory - Universal com suporte MFA**, introduza o seu nome de utilizador e, em seguida, clique em **Ligar**.

    ![Iniciar sessão](./media/analysis-services-create-sample-model/aas-create-sample-cleanup-signin.png)

3. No **Object Explorer**, clique com botão direito do `adventureworks` da base de dados de exemplo e, em seguida, clique em **eliminar**.

    ![Eliminar a base de dados de exemplo](./media/analysis-services-create-sample-model/aas-create-sample-cleanup-delete.png)

## <a name="next-steps"></a>Passos Seguintes 

[Ligar no Power BI Desktop](analysis-services-connect-pbi.md)   
[Gerir utilizadores e funções de base de dados](analysis-services-database-users.md)


