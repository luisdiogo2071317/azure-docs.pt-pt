---
title: "Criar, ligar, eliminar ou mover uma conta de integração em aplicações lógicas do Azure | Microsoft Docs"
description: "Como criar uma conta de integração e ligá-lo para as logic apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: divyaswarnkar
manager: anneta
editor: 
ms.assetid: d3ad9e99-a9ee-477b-81bf-0881e11e632f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/23/2017
ms.author: LADocs; divswa
ms.openlocfilehash: fb1d0ceb26c5ed792f22051e2af10a7572200bdc
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="what-is-an-integration-account"></a>O que é uma conta de integração?

Uma conta de integração proporciona uma forma para a sua empresa aplicações de integração, especificamente logic apps para aceder e gerir B2B artefactos, por exemplo, comerciais parceiros, contratos, mapas, esquemas, certificados e assim sucessivamente. Para fornecer este acesso, associe a sua conta de integração para a sua aplicação lógica, após certificar-se de que tanto a integração conta e a lógica da aplicação têm de *mesma localização do Azure*.

## <a name="create-an-integration-account"></a>Criar uma conta de Integração

1. Inicie sessão no [portal do Azure](http://portal.azure.com "portal do Azure"). 

2. No menu principal do Azure, selecione **todos os serviços**. Na caixa de pesquisa, introduza "integração" e, em seguida, selecione **contas de automatização**.

   ![criar conta de integração](./media/logic-apps-enterprise-integration-accounts/account-1.png)

3. Na parte superior da página, escolha **adicionar**.

   ![Escolher adicionar](./media/logic-apps-enterprise-integration-accounts/account-3.png)

4. Nome da sua conta de integração e selecione a subscrição do Azure que pretende utilizar. Pode criar um novo **grupo de recursos** ou selecione um grupo de recursos existente. Selecione um **localização** para alojar a sua conta de integração e um **escalão de preço**. Quando estiver pronto, selecione **criar**.

   ![Forneça detalhes para a sua conta de integração](./media/logic-apps-enterprise-integration-accounts/account-4.png)

   Azure Aprovisiona a sua conta de integração na localização selecionada, o que deve ser concluída num minuto.

5. Atualize a página. Consulte a nova conta de integração listada.

   ![É apresentada a sua nova conta de integração](./media/logic-apps-enterprise-integration-accounts/account-5.png) 

Em seguida, associe a conta de integração que criou a sua aplicação lógica. 

## <a name="link-an-integration-account-to-a-logic-app"></a>Associar uma conta de integração para uma aplicação lógica

Para conceder o acesso de aplicações lógicas a B2B artefactos, tais como comerciais parceiros, contratos, mapas e esquemas na sua conta de integração, ligue a conta de integração para a sua aplicação lógica. 

1. No portal do Azure, selecione a sua aplicação lógica e verifique a localização da sua aplicação lógica.

   ![Selecione a sua aplicação lógica, verifique a localização](./media/logic-apps-enterprise-integration-accounts/linkaccount-1.png)

2. Em **definições**, selecione **Integration Account**.

   ![Selecione "Conta de integração"](./media/logic-apps-enterprise-integration-accounts/linkaccount-2.png)

3. Do **selecionar uma conta de integração** lista, selecione a conta de integração que pretende associar à sua aplicação lógica. Para concluir a ligação, escolha **guardar**.

   ![Selecione a sua conta de integração](./media/logic-apps-enterprise-integration-accounts/linkaccount-3.png)

   Receberá uma notificação que mostra a sua integração conta está ligada à sua aplicação lógica, e que todos os artefactos na sua conta de integração estão agora disponíveis para a sua aplicação lógica.

   ![A aplicação lógica está ligada à sua conta de integração](./media/logic-apps-enterprise-integration-accounts/linkaccount-5.png)

Agora que a sua conta de integração está ligada à sua aplicação lógica, pode utilizar os conectores de B2B na sua aplicação lógica. Alguns conectores B2B comuns incluem validação XML e Codificar/descodificar com ficheiros simples.  

## <a name="delete-your-integration-account"></a>Eliminar a conta de integração

1. No menu principal do Azure, selecione **todos os serviços**. Na caixa de pesquisa, introduza "integração" e, em seguida, selecione **contas de automatização**.

   ![Localizar a conta de integração](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. Selecione a conta de integração que pretende eliminar.

    ![Selecione a conta de integração a eliminar](./media/logic-apps-enterprise-integration-accounts/account-5.png)

3. No menu, escolha **eliminar**.

    ![Escolha "Eliminar"](./media/logic-apps-enterprise-integration-accounts/delete.png)

4. Confirme a sua escolha para eliminar a conta de integração.

## <a name="move-your-integration-account"></a>Mover a sua conta de integração

Para mover uma conta de integração para outro grupo de subscrição ou recurso do Azure, siga estes passos. Depois de mover a conta de integração, certifique-se que Atualize todos os scripts para utilizar o novo IDs de recurso.

1. No menu principal do Azure, selecione **todos os serviços**. Na caixa de pesquisa, introduza "integração" e, em seguida, selecione **contas de automatização**.

   ![Localizar a conta de integração](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. Selecione a conta de integração que pretende mover. Em **definições**, escolha **propriedades**.

   ![Selecione a conta de integração para mover. Em definições, escolha Propriedades](./media/logic-apps-enterprise-integration-accounts/move.png)

3. Altere o grupo de recursos ou subscrição do Azure associado à sua conta de integração.

   ![Escolha o grupo de recursos de alteração ou de subscrição de alteração](./media/logic-apps-enterprise-integration-accounts/move-2.png)

## <a name="next-steps"></a>Passos Seguintes

* [Saiba mais sobre contratos](../logic-apps/logic-apps-enterprise-integration-agreements.md "Saiba mais sobre contratos de integração do enterprise")  

