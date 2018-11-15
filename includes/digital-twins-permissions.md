---
title: incluir ficheiro
description: incluir ficheiro
services: digital-twins
author: alinamstanciu
ms.service: digital-twins
ms.topic: include
ms.date: 11/13/2018
ms.author: alinast
ms.custom: include file
ms.openlocfilehash: 216e2db82d5a07bd8e4cae8b9f357ac7dcee330a
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51626413"
---
1. Na [portal do Azure](https://portal.azure.com), abra **Azure Active Directory** no painel à esquerda e, em seguida, abra a **propriedades** painel. Copie o **ID do diretório** para um ficheiro temporário. Irá utilizar este valor para configurar uma aplicação de exemplo na secção seguinte.

    ![ID de diretório do Azure Active Directory](./media/digital-twins-permissions/aad-app-reg-tenant.png)

1. Abra o **registos de aplicações** painel e, em seguida, selecione a **novo registo de aplicação** botão.

    ![Painel de registos de aplicação](./media/digital-twins-permissions/aad-app-reg-start.png)

1. Dê um nome amigável para este registo de aplicações no **nome** caixa. Escolher **tipo de aplicação** como **nativo**, e **URI de redirecionamento** como `https://microsoft.com`. Selecione **Criar**.

    ![Criar Painel](./media/digital-twins-permissions/aad-app-reg-create.png)

1. Abra a aplicação registada e copie o valor do **ID da aplicação** campo num arquivo temporário. Este valor identifica a sua aplicação do Azure Active Directory. Usará o ID da aplicação para configurar o aplicativo de exemplo nas seções a seguir.

    ![ID da aplicação do Azure Active Directory](./media/digital-twins-permissions/aad-app-reg-app-id.png)

1. Abra o painel de registo de aplicação. Selecione **configurações** > **permissões obrigatórias**e, em seguida:

   a. Selecione **Add** no canto superior esquerdo para abrir o **adicionar acesso à API** painel.

   b. Selecione **selecionar uma API** e procure **duplos Digital do Azure**. Se a pesquisa não localizar a API, procure **Azure Smart Spaces** como alternativa.

   c. Selecione o **duplos Digital do Azure (serviço do Azure inteligente espaços)** opção e escolha **selecione**.

   d. Escolher **selecionar permissões**. Selecione o **acesso de leitura/escrita** permissões delegadas caixa de verificação e escolha **selecione**.

   e. Selecione **feito** no **adicionar acesso à API** painel.

   f. Na **permissões obrigatórias** painel, selecione a **conceder permissões** botão e aceite a confirmação de que é apresentada.

      ![Painel permissões obrigatórias](./media/digital-twins-permissions/aad-app-req-permissions.png)
