---
title: Criar e gerir contas de automatização para soluções de B2B - Azure Logic Apps | Microsoft Docs
description: Criar, ligar, mover e eliminar contas de automatização para integração empresarial com e soluções de B2B com Azure Logic Apps
services: logic-apps
documentationcenter: ''
author: ecfan
manager: cfowler
editor: ''
ms.assetid: d3ad9e99-a9ee-477b-81bf-0881e11e632f
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 04/30/2018
ms.author: estfan
ms.openlocfilehash: 97cdc809a00078ee86a40fd4567ab2d3357ed3bd
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34726436"
---
# <a name="create-and-manage-integration-accounts-for-b2b-solutions-with-logic-apps"></a>Criar e gerir contas de automatização para soluções de B2B com logic apps

Antes de poder compilar [integração empresarial com e soluções de B2B](../logic-apps/logic-apps-enterprise-integration-overview.md) com [Azure Logic Apps](../logic-apps/logic-apps-overview.md), primeiro tem de ter uma conta de integração, o que é onde criar, armazenar e gerir B2B artefactos, tais como comercial parceiros, contratos, mapas, esquemas, certificados e assim sucessivamente. Antes da aplicação lógica pode trabalhar com os artefactos na sua conta de integração e utilizar os conectores de Logic Apps B2B, tais como a validação de XML tem [associar a sua conta de integração](#link-account) à sua aplicação lógica. Ao ligá-las, tanto a integração conta e a lógica da aplicação tem de ter o *mesmo* localização do Azure ou a região.

Este artigo mostra como realizar estas tarefas:

* Crie a sua conta de integração.
* Associe a sua conta de integração para uma aplicação lógica.
* Mover a sua conta de integração para o Azure noutro grupo de recursos ou subscrição.
* Elimine a conta de integração.

Se não tiver uma subscrição do Azure, <a href="https://azure.microsoft.com/free/" target="_blank">inscreva-se para obter uma conta do Azure gratuita</a>.

## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Inicie sessão no <a href="https://portal.azure.com" target="_blank">portal do Azure</a> com as credenciais da sua conta do Azure.

## <a name="create-integration-account"></a>criar conta de integração

1. No menu principal do Azure, selecione **todos os serviços**. Na caixa de pesquisa, introduza "contas de automatização" como o filtro e selecione **contas de automatização**.

   ![Localizar contas de automatização](./media/logic-apps-enterprise-integration-create-integration-account/create-integration-account.png)

2. Em **contas de automatização**, escolha **adicionar**.

   ![Escolha "Adicionar" para criar a conta de integração](./media/logic-apps-enterprise-integration-create-integration-account/add-integration-account.png)

3. Fornece informações sobre a sua conta de integração: 

   ![Forneça detalhes para a sua conta de integração](./media/logic-apps-enterprise-integration-create-integration-account/integration-account-details.png)

   | Propriedade | Necessário | Valor de exemplo | Descrição | 
   |----------|----------|---------------|-------------|
   | Nome | Sim | conta de integração de teste | O nome para a sua conta de integração. Neste exemplo, utilize o nome especificado. | 
   | Subscrição | Sim | <*Nome da subscrição do Azure*> | O nome da subscrição do Azure a utilizar | 
   | Grupo de recursos | Sim | teste-integração-conta-rg | O nome para o [grupo de recursos do Azure](../azure-resource-manager/resource-group-overview.md) utilizados para organizar os recursos relacionados. Neste exemplo, crie um novo grupo de recursos com o nome especificado. | 
   | Escalão de Preço | Sim | Gratuito | O escalão de preço que pretende utilizar. Para este exemplo, selecione **livres**, mas para obter mais informações, consulte [limites Logic Apps e a configuração](../logic-apps/logic-apps-limits-and-config.md) e [Logic Apps preços](https://azure.microsoft.com/pricing/details/logic-apps/). | 
   | Localização | Sim | EUA Oeste | A região onde pretende armazenar as informações da conta de integração. Selecione a mesma localização que a sua aplicação lógica ou criar uma aplicação lógica na mesma localização que a sua conta de integração. | 
   | Log Analytics | Não | Desativado | Mantenha a definição **Desativado** para o registo de diagnósticos. | 
   ||||| 

4. Quando estiver pronto, selecione **Afixar ao dashboard** e escolha **Criar**.

   Depois de Azure implementa a sua conta de integração para a localização selecionada, o que normalmente é concluído dentro de um minuto, o Azure abre-se a conta de integração.

   ![Azure abre-se a sua conta de integração](./media/logic-apps-enterprise-integration-create-integration-account/integration-account-created.png)

Agora, antes da aplicação lógica pode utilizar a sua conta de integração, tem de ligar a conta de integração para a sua aplicação lógica.

<a name="link-account"></a>

## <a name="link-to-logic-app"></a>Ligar a aplicação lógica

Para dar acesso a aplicações a lógica para uma conta de integração que contém os artefactos de B2B, tais como comerciais parceiros, contratos, mapas e esquemas, tem de associar a sua conta de integração para a sua aplicação lógica. 

> [!NOTE]
> A aplicação de conta e a lógica de integração tem de existir na mesma região.

1. No portal do Azure, localize e abra a aplicação lógica.

2. No menu da sua aplicação lógica, sob **definições**, selecione **definições de fluxo de trabalho**. No **selecionar uma conta de integração** lista, selecione a conta de integração para ligar a sua aplicação lógica.

   ![Selecione a sua conta de integração](./media/logic-apps-enterprise-integration-create-integration-account/linkaccount-2.png)

3. Para concluir a ligação, escolha **guardar**.

   ![Selecione a sua conta de integração](./media/logic-apps-enterprise-integration-create-integration-account/linkaccount-3.png)

   Quando a sua conta de integração foi associada com êxito, o Azure mostra uma mensagem de confirmação. 

   ![Azure confirma que a ligação com êxito](./media/logic-apps-enterprise-integration-create-integration-account/linkaccount-5.png)

Agora a sua aplicação lógica pode utilizar qualquer e todos os artefactos na sua conta de integração mais conectores B2B, tais como a validação XML e o ficheiro simples de codificação ou descodificação.  

## <a name="unlink-from-logic-app"></a>Desassociar de aplicação lógica

Para ligar a sua aplicação lógica para outra conta de integração ou deixar de utilizar uma conta de integração com a sua aplicação lógica, pode eliminar a ligação através do Explorador de recursos do Azure.

1. No seu browser, aceda a <a href="https://resources.azure.com" target="_blank">Explorador de recursos do Azure (https://resources.azure.com)</a>. Certifique-se de que iniciou sessão com as mesmas credenciais do Azure.

   ![Explorador de Recursos do Azure](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer.png)

2. Na caixa de pesquisa, introduza o nome da sua aplicação lógica, em seguida, localize e selecione a sua aplicação lógica.

   ![Localize e selecione a aplicação lógica](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer-find-logic-app.png)

3. Na barra de título do explorer, escolha **leitura/escrita**.

   ![Ativar o modo de "Leitura/escrita"](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer-choose-read-write-mode.png)

4. No **dados** separador, escolha **editar**.

   ![No separador de "Dados", selecione "Editar"](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer-choose-edit.png)

5. No editor de localizar o `integrationAccount` propriedade para a integração de contas e eliminar essa propriedade com este formato:

   ```json
   "integrationAccount": {
      "name": "<integration-account-name>",
      "id": "<integration-account-resource-ID>",
      "type": "Microsoft.Logic/integrationAccounts"  
   },
   ```

   Por exemplo:

   ![Localizar a definição de propriedade de "integrationAccount"](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer-delete-integration-account.png)

6. No **dados** separador, escolha **colocar** para guardar as alterações. 

   ![Escolha a "Colocar" para guardar as alterações](./media/logic-apps-enterprise-integration-create-integration-account/resource-explorer-save-changes.png)

7. No portal do Azure, na sua aplicação lógica **definições de fluxo de trabalho**, verifique se o **conta integração** propriedade aparece em branco.

   ![Verifique que a conta de integração não está ligada](./media/logic-apps-enterprise-integration-create-integration-account/unlinked-account.png)

## <a name="move-integration-account"></a>Mover a conta de integração

Pode mover a sua conta de integração para outro grupo de subscrição ou recurso do Azure.

1. No menu principal do Azure, selecione **todos os serviços**. Na caixa de pesquisa, introduza "contas de automatização" como o filtro e selecione **contas de automatização**.

   ![Localizar a conta de integração](./media/logic-apps-enterprise-integration-create-integration-account/create-integration-account.png)

2. Em **contas de automatização**, selecione a conta de integração que pretende mover. No seu integração conta menu, em **definições**, escolha **propriedades**.

   ![Em "Definições", escolha "Propriedades"](./media/logic-apps-enterprise-integration-create-integration-account/integration-account-properties.png)

3. Altere o grupo de recursos do Azure ou uma subscrição para a sua conta de integração.

   ![Escolha o "Grupo de recursos de alteração" ou "Alteração de subscrição"](./media/logic-apps-enterprise-integration-create-integration-account/change-resource-group-subscription.png)

4. Quando tiver terminado, certifique-se que Atualize todos os scripts com o novo IDs de recurso para os seus artefactos.  

## <a name="delete-integration-account"></a>Eliminar a conta de integração

1. No menu principal do Azure, selecione **todos os serviços**. Na caixa de pesquisa, introduza "contas de automatização" como o filtro e selecione **contas de automatização**.

   ![Localizar a conta de integração](./media/logic-apps-enterprise-integration-create-integration-account/create-integration-account.png)

2. Em **contas de automatização**, selecione a conta de integração que pretende eliminar. No menu de conta de integração, escolha **descrição geral**, em seguida, escolha **eliminar**. 

   ![Selecione a conta de integração. Na página de "descrição geral", escolha "Eliminar"](./media/logic-apps-enterprise-integration-create-integration-account/delete-integration-account.png)

3. Para confirmar que pretende eliminar a conta de integração, escolha **Sim**.

   ![Para confirmar a eliminação, selecione "Sim"](./media/logic-apps-enterprise-integration-create-integration-account/confirm-delete.png)

## <a name="next-steps"></a>Passos Seguintes

* [Crie parceiros comerciais](../logic-apps/logic-apps-enterprise-integration-partners.md)
* [Criar contratos](../logic-apps/logic-apps-enterprise-integration-agreements.md)
