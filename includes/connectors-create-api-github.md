---
title: incluir ficheiro
description: incluir ficheiro
services: logic-apps
author: MandiOhlinger
ms.service: logic-apps
ms.topic: include
ms.date: 03/02/2018
ms.author: mandia
ms.custom: include file
ms.openlocfilehash: ec5b3ca9ccd139cbdf17768056eb1d835336e7a7
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/05/2018
---
1. No [portal do Azure](https://portal.azure.com), criar uma aplicação lógica em branco. 

2. No Designer de aplicações lógica, introduza "github" como o filtro. 

3. Selecione o conector do GitHub e o acionador que pretende utilizar.

   ![Selecione o conector do GitHub e um acionador](./media/connectors-create-api-github/github-connector.png)

   > [!NOTE]
   > Todos os fluxos de trabalho de aplicação de lógica tem de começar com um acionador. Pode selecionar ações apenas quando o fluxo de trabalho lógica já começa com um acionador. 

4. Se não tiver criado anteriormente uma ligação, escolha **sessão** para que possam fornecer as credenciais do GitHub, quando lhe for pedido.  

   ![Inicie sessão com as suas credenciais do GitHub](./media/connectors-create-api-github/github-connector-sign-in-credentials.png)

   A aplicação lógica utiliza estas credenciais para autorizar a ligação e aceder aos dados para a sua conta do GitHub. 

5. Forneça o nome de utilizador do GitHub e a palavra-passe e confirmar a sua autorização.

   ![Forneça as credenciais e confirme a autorização](./media/connectors-create-api-github/github-connector-authorize.png)   

   A ligação está agora criada no portal do Azure e está pronta a utilizar.

6. Continue a definir o fluxo de trabalho de aplicação lógica.

   ![Adicionar mais ações para o fluxo de trabalho de aplicação lógica](./media/connectors-create-api-github/github-connector-logic-app.png)

