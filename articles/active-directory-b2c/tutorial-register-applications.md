---
title: Tutorial - registar as suas aplicações no Azure Active Directory B2C | Documentos da Microsoft
description: Saiba como registar as suas aplicações no Azure Active Directory B2C no portal do Azure.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 01/11/2019
ms.author: davidmu
ms.openlocfilehash: 511e1e9f29e6ae7602a977819f5295f76236595d
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2019
ms.locfileid: "54248985"
---
# <a name="tutorial-register-your-applications-in-azure-active-directory-b2c"></a>Tutorial: Registar as suas aplicações no Azure Active Directory B2C

Antes de sua [aplicativos](active-directory-b2c-apps.md) pode interagir com o Azure Active Directory (Azure AD) B2C, tem de estar registados num inquilino que gere. Este tutorial mostra como registar aplicações no portal do Azure.

Neste artigo, vai aprender a:

> [!div class="checklist"]
> * Registar uma aplicação Web
> * Registar uma API Web
> * Registar uma aplicação móvel ou nativa

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Se ainda não tiver criado o seu [inquilino do Azure AD B2C](tutorial-create-tenant.md), crie uma agora. Pode utilizar um inquilino existente.

## <a name="register-a-web-application"></a>Registar uma aplicação Web

1. Certifique-se de que está a utilizar o diretório que contém o seu inquilino do Azure AD B2C, clicando no **filtro de diretório e subscrição** no menu superior e escolher o diretório que contém o seu inquilino.

    ![Mude para o diretório da subscrição](./media/tutorial-register-applications/switch-directories.png)

2. Escolher **todos os serviços** no canto superior esquerdo do portal do Azure e, em seguida, procure e selecione **do Azure AD B2C**.
3. Selecione **aplicativos**e, em seguida, selecione **Add**.

    ![Adicionar aplicação](./media/tutorial-register-applications/add-application.png)

4. Introduza um nome para a aplicação. Por exemplo, *webapp1*.
5. Para **incluir a aplicação web / web API** e **permitir fluxo implícito**, selecione **Sim**.
6. Para **URL de resposta**, introduza um ponto de extremidade em que o Azure AD B2C deverá devolver quaisquer tokens que a aplicação solicite. Por exemplo, pode defini-lo para escutar localmente em `https://localhost:44316` se ainda não conhece o número de porta, pode introduzir um valor de marcador de posição e alterá-la mais tarde. Para fins de teste pode defini-lo `https://jwt.ms`, que exibe o conteúdo de um token para inspeção. Para este tutorial, defina-o como `https://jwt.ms`. 

    O URL tem de começar com o esquema de resposta `https`, e responder a todos os valores de URL têm de partilhar um único domínio DNS. Por exemplo, se o aplicativo tem um URL de resposta de `https://login.contoso.com`, pode adicionar a ele como este URL `https://login.contoso.com/new`. Em alternativa, pode consultar um subdomínio DNS de `login.contoso.com`, tais como `https://new.login.contoso.com`. Se quiser ter uma aplicação com `login-east.contoso.com` e `login-west.contoso.com` como URLs de resposta, tem de adicionar esses URLs de resposta pela seguinte ordem: `https://contoso.com`, `https://login-east.contoso.com`, `https://login-west.contoso.com`. Pode adicionar os dois últimos porque são subdomínios do primeiro URL de resposta, `contoso.com`.

7. Clique em **Criar**.

    ![Definir as propriedades do aplicativo](./media/tutorial-register-applications/application-properties.png)

### <a name="create-a-client-secret"></a>Criar um segredo do cliente

Se estiver aplicativo troca um código de um token, terá de criar um segredo de aplicação.

1. Selecione **chaves** e, em seguida, clique em **gerar chave**.

    ![Gerar chaves](./media/tutorial-register-applications/generate-keys.png)

2. Selecione **guardar** para ver a chave. Anote o valor da **Chave da aplicação**. Utilize o valor como o segredo de aplicação no código da aplicação.

    ![Guardar a chave](./media/tutorial-register-applications/save-key.png)
    
3. Selecione **acesso à API**, clique em **Add**e selecione a web API e os âmbitos (permissões).

    ![Configurar o acesso à API](./media/tutorial-register-applications/api-access.png)

## <a name="register-a-web-api"></a>Registar uma API Web

1. Selecione **aplicativos**e, em seguida, selecione **Add**.
3. Introduza um nome para a aplicação. Por exemplo, *webapi1*.
4. Para **incluir a aplicação web / web API** e **permitir fluxo implícito**, selecione **Sim**.
5. Para **URL de resposta**, introduza um ponto de extremidade em que o Azure AD B2C deverá devolver quaisquer tokens que a aplicação solicite. Por exemplo, pode defini-lo para escutar localmente em `https://localhost:44316`. Se ainda não conhece o número de porta, pode introduzir um valor de marcador de posição e alterá-la mais tarde.
6. Para **URI de ID de aplicação**, introduza o identificador utilizado para a API web. O URI identificador completo, incluindo o domínio, é gerado por si. Por exemplo, `https://contosotenant.onmicrosoft.com/api`.
7. Clique em **Criar**.
8. Selecione o *webapi1* aplicação que criou e, em seguida, selecione **âmbitos publicados** para adicionar mais âmbitos conforme necessário. Por predefinição, o `user_impersonation` é definido o âmbito. O `user_impersonation` âmbito fornece a outras aplicações a capacidade de aceder a esta API em nome do utilizador com sessão iniciada. Se desejar, o `user_impersonation` âmbito pode ser removido.

    ![Definir âmbitos publicados](./media/tutorial-register-applications/published-scopes.png)


## <a name="register-a-mobile-or-native-application"></a>Registar uma aplicação móvel ou nativa

1. Selecione **aplicativos**e, em seguida, selecione **Add**.
2. Introduza um nome para a aplicação. Por exemplo, *nativeapp1*.
3. Para **incluir a aplicação web / web API**, selecione **não**.
4. Para **incluir cliente nativo**, selecione **Sim**.
5. Para **URI de redirecionamento**, introduza um URI de redirecionamento válido com um esquema personalizado. Existem duas considerações importantes ao escolher um URI de redirecionamento:

    - **Exclusivo** -o esquema do URI de redirecionamento deve ser exclusivo para cada aplicação. No exemplo `com.onmicrosoft.contoso.appname://redirect/path`, `com.onmicrosoft.contoso.appname` é o esquema. Este padrão deve ser seguido. Se duas aplicações partilharem o mesmo esquema, o utilizador recebe uma opção para escolher uma aplicação. Se o utilizador fizer uma seleção incorreta, o início de sessão irá falhar.
    - **Completa** -o URI de redirecionamento tem de ter um esquema e um caminho. O caminho tem de conter, pelo menos, uma barra depois do domínio. Por exemplo, `//contoso/` funciona e `//contoso` falhar. Certifique-se de que o URI de redirecionamento não inclui caracteres especiais, como carateres de sublinhado.

6. Clique em **Criar**.

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, aprendeu como:

> [!div class="checklist"]
> * Registar uma aplicação Web
> * Registar uma API Web
> * Registar uma aplicação móvel ou nativa

> [!div class="nextstepaction"]
> [Criar fluxos de utilizador no Azure Active Directory B2C](tutorial-create-user-flows.md)