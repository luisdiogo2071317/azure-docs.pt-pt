---
title: Gerir metadados de artefactos de conta de integração - Azure Logic Apps | Documentos da Microsoft
description: Adicionar ou obter metadados de artefactos de contas de integração no Azure Logic Apps com o Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.date: 02/23/2018
ms.openlocfilehash: 537014c2780fe94cfb35806759f8bcbd974c4c95
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/28/2018
ms.locfileid: "43128808"
---
# <a name="manage-artifact-metadata-from-integration-accounts-in-azure-logic-apps-with-enterprise-integration-pack"></a>Gerir metadados de artefactos de contas de integração no Azure Logic Apps com o Enterprise Integration Pack

Pode definir metadados personalizados para artefactos em contas de integração e recuperar esses metadados durante o tempo de execução para a aplicação lógica. Por exemplo, pode especificar metadados para artefactos, tais como parceiros, contratos, esquemas e mapas - todos os metadados de armazenamento através de pares chave-valor. 

## <a name="add-metadata-to-artifacts-in-integration-accounts"></a>Adicionar metadados para artefactos em contas de integração

1. No portal do Azure, crie uma [conta de integração](logic-apps-enterprise-integration-create-integration-account.md).

2. Adicionar um artefato à sua conta de integração, por exemplo, um [parceiro](logic-apps-enterprise-integration-partners.md), [contrato](logic-apps-enterprise-integration-agreements.md), ou [esquema](logic-apps-enterprise-integration-schemas.md).

3. Selecione o artefacto, escolha **editar**e introduza os detalhes de metadados.

   ![Introduzir metadados](media/logic-apps-enterprise-integration-metadata/image1.png)

## <a name="retrieve-metadata-from-artifacts-for-logic-apps"></a>Obter metadados de artefactos para o logic apps

1. No portal do Azure, crie uma [aplicação lógica](quickstart-create-first-logic-app-workflow.md).

2. Criar uma [ligação da sua aplicação lógica à sua conta de integração](logic-apps-enterprise-integration-create-integration-account.md#link-account). 

3. No Estruturador da aplicação lógica, adicionar um acionador, como **pedir** ou **HTTP** à sua aplicação lógica.

4. No acionador, escolha **novo passo** > **adicionar uma ação**. Procure "conta de integração", para que possa encontrar e, em seguida, selecione a ação: **conta de integração - pesquisa de artefacto da conta de integração**

   ![Selecione a pesquisa de artefacto da conta de integração](media/logic-apps-enterprise-integration-metadata/image2.png)

5. Selecione o **tipo de Artefato** e forneça o **nome do artefacto**. Por exemplo:

   ![Selecione o tipo de artefacto e especifique o nome do artefacto](media/logic-apps-enterprise-integration-metadata/image3.png)

## <a name="example-retrieve-partner-metadata"></a>Exemplo: Obter metadados de parceiro

Suponha que este parceiro tem esses metadados `routingUrl` detalhes:

![Localizar o parceiro de metadados de "routingURL"](media/logic-apps-enterprise-integration-metadata/image6.png)

1. Na sua aplicação lógica, adicione o acionador, um **conta de integração - pesquisa de artefacto da conta de integração** ação para o seu parceiro e um **HTTP** ação, por exemplo:

   ![Adicionar acionador, a pesquisa de artefacto e a ação de HTTP para a sua aplicação lógica](media/logic-apps-enterprise-integration-metadata/image4.png)

2. Para obter o URI, na barra de ferramentas do Estruturador da aplicação lógica, escolha **vista de código** para a aplicação lógica. A definição da aplicação lógica deve ter um aspeto semelhante a este exemplo:

   ![Pesquisa de pesquisa](media/logic-apps-enterprise-integration-metadata/image5.png)

## <a name="next-steps"></a>Passos Seguintes

* [Saiba mais sobre os contratos](logic-apps-enterprise-integration-agreements.md)
