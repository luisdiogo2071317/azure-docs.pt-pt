---
title: incluir ficheiro
description: incluir ficheiro
services: digital-twins
author: alinamstanciu
ms.service: digital-twins
ms.topic: include
ms.date: 09/19/2018
ms.author: alinast
ms.custom: include file
ms.openlocfilehash: 1887efd741f4779a5186707d60b27ca66fc3c06f
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/08/2018
ms.locfileid: "51283988"
---
1. No [portal do Azure](https://portal.azure.com), abra o **Azure Active Directory** a partir do painel de navegação esquerdo e, em seguida, abra o painel **Propriedades**. Copie o **ID do diretório** para um ficheiro temporário. Irá utilizar este valor para configurar a aplicação de exemplo na secção seguinte.

    ![ID do diretório do Azure Active Directory](./media/digital-twins-permissions/aad-app-reg-tenant.png)

1. Abra o painel **Registos de aplicações** e, em seguida, clique no botão **Novo registo de aplicação**.
    
    ![Novo registo de aplicação do Azure Active Directory](./media/digital-twins-permissions/aad-app-reg-start.png)

1. Atribua um nome amigável a este registo de aplicação no campo **Nome**. Selecione **Tipo de aplicação** como **_Nativo_** e **URI de redirecionamento** como **_https://microsoft.com_**. Clique em **Criar**.

    ![Criar registo de aplicação do Azure Active Directory](./media/digital-twins-permissions/aad-app-reg-create.png)

1. Abra a aplicação registada e copie o valor do campo **ID da aplicação** para um ficheiro temporário; este valor identifica a sua aplicação do Azure Active Directory. Irá utilizar o ID da Aplicação para configurar a aplicação de exemplo nas secções seguintes.

    ![ID da aplicação Azure Active Directory](./media/digital-twins-permissions/aad-app-reg-app-id.png)

1. Abra o painel de registo de aplicação e clique em **Definições** > **Permissões obrigatórias**:
    - Clique em **Adicionar** na parte superior esquerda para abrir o painel **Adicionar acesso à API**.
    - Clique em **Selecionar uma API** e procure **Azure Digital Twins**. Se a pesquisa não localizar a API, procure **Azure Smart Spaces** como alternativa.
    - Selecione a opção **Azure Digital Twins (Azure Smart Spaces Service)** e clique em **Selecionar**.
    - Clique em **Selecionar permissões**. Selecione a caixa de permissões delegadas **Acesso de Leitura/Escrita** e clique em **Selecionar**.
    - Clique em **Concluído** no painel **Adicionar acesso à API**.
    - No painel **Permissões obrigatórias**, clique no botão **Conceder permissões** e aceite a confirmação apresentada.

       ![API para adicionar registo de aplicação do Azure Active Directory](./media/digital-twins-permissions/aad-app-req-permissions.png)
