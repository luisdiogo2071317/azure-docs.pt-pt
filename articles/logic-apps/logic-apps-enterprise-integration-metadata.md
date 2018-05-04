---
title: Gerir metadados de artefacto da conta integração - Azure Logic Apps | Microsoft Docs
description: Adicionar ou obter metadados de artefacto de contas de automatização do Azure Logic Apps
author: padmavc
manager: anneta
editor: ''
services: logic-apps
documentationcenter: ''
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 02/23/2018
ms.author: LADocs; padmavc
ms.openlocfilehash: 8ebca60c21366cddadccfd0456880696457d7777
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/01/2018
---
# <a name="manage-artifact-metadata-in-integration-accounts-for-logic-apps"></a>Gerir metadados de artefactos em contas de integração para aplicações lógicas

Pode definir metadados personalizados para artefactos em contas de automatização e obter esses metadados durante o tempo de execução para a sua aplicação lógica. Por exemplo, pode especificar os metadados para artefactos, tais como parceiros, contratos, esquemas e mapas - todos os metadados de arquivo utilizando pares chave-valor. 

## <a name="add-metadata-to-artifacts-in-integration-accounts"></a>Adicionar metadados a artefactos em contas de automatização

1. No portal do Azure, criar um [conta integração](logic-apps-enterprise-integration-create-integration-account.md).

2. Adicionar um artefacto à sua conta de integração, por exemplo, um [parceiro](logic-apps-enterprise-integration-partners.md), [contrato](logic-apps-enterprise-integration-agreements.md), ou [esquema](logic-apps-enterprise-integration-schemas.md).

3. Selecione o artefacto, escolha **editar**e introduza os detalhes de metadados.

   ![Introduza os metadados](media/logic-apps-enterprise-integration-metadata/image1.png)

## <a name="retrieve-metadata-from-artifacts-for-logic-apps"></a>Obter metadados a partir de artefactos para aplicações lógicas

1. No portal do Azure, criar um [aplicação lógica](quickstart-create-first-logic-app-workflow.md).

2. Criar um [ligação da sua aplicação lógica à sua conta de integração](logic-apps-enterprise-integration-create-integration-account.md#link-account). 

3. No Designer de aplicação lógica, adicionar um acionador como **pedido** ou **HTTP** à sua aplicação lógica.

4. Sob o acionador, escolha **novo passo** > **adicionar uma ação**. Procure "conta de integração", para que possa encontrar e, em seguida, selecione esta ação: **Integration Account - pesquisa de artefacto da conta de integração**

   ![Selecione a pesquisa de artefacto da conta de integração](media/logic-apps-enterprise-integration-metadata/image2.png)

5. Selecione o **artefactos tipo** e forneça o **artefactos nome**. Por exemplo:

   ![Selecione o tipo de artefacto e especifique o nome de artefactos](media/logic-apps-enterprise-integration-metadata/image3.png)

## <a name="example-retrieve-partner-metadata"></a>Exemplo: Obter metadados a parceiro

Suponhamos deste parceiro tem estes metadados com `routingUrl` detalhes:

![Localizar o parceiro de metadados "routingURL"](media/logic-apps-enterprise-integration-metadata/image6.png)

1. Na sua aplicação lógica, adicione o acionador, um **Integration Account - pesquisa de artefacto da conta de integração** ação para o seu parceiro e um **HTTP** ação, por exemplo:

   ![Adicionar o acionador, a pesquisa de artefacto e a ação de HTTP para a sua aplicação lógica](media/logic-apps-enterprise-integration-metadata/image4.png)

2. Para obter o URI, na barra de ferramentas Designer de aplicação lógica, escolha **vista de código** para a sua aplicação lógica. A definição da aplicação lógica deve aspeto neste exemplo:

   ![Pesquisa de pesquisa](media/logic-apps-enterprise-integration-metadata/image5.png)

## <a name="next-steps"></a>Passos Seguintes

* [Saiba mais sobre contratos](logic-apps-enterprise-integration-agreements.md)
