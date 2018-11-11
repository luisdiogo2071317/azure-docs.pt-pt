---
title: Registar uma aplicação no Azure Active Directory B2C | Documentos da Microsoft
description: Saiba como registar a sua aplicação com o Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/01/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 8068c4a8f38cd33a1a0547f5db5079bc75c76ec1
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/05/2018
ms.locfileid: "51013416"
---
# <a name="register-an-application-in-azure-active-directory-b2c"></a>Registar uma aplicação no Azure Active Directory B2C

Para criar uma [aplicativo](active-directory-b2c-apps.md) que aceita o consumidor de inscrição e início de sessão, terá primeiro de registar a aplicação com um inquilino do Azure AD B2C. Este artigo ajuda-o a registar uma aplicação num inquilino do Azure Active Directory (Azure AD) B2C em alguns minutos. Quando tiver terminado, a aplicação fica registada para utilização no inquilino do Azure AD B2C.

## <a name="prerequisites"></a>Pré-requisitos

Obtenha o seu inquilino, utilizando os passos em [criar um inquilino do Azure Active Directory B2C](tutorial-create-tenant.md).

Escolha os passos seguintes com base no seu tipo de aplicação:

- [Registar uma aplicação Web](#register-a-web-application)
- [Registar uma API Web](#register-a-web-api)
- [Registar uma aplicação móvel ou nativa](#register-a-mobile-or-native-application)

## <a name="register-a-web-application"></a>Registar uma aplicação Web

1. Certifique-se de que está a utilizar o diretório que contém o seu inquilino do Azure AD B2C, clicando no **filtro de diretório e subscrição** no menu superior e escolher o diretório que contém o seu inquilino.
2. Escolher **todos os serviços** no canto superior esquerdo do portal do Azure e, em seguida, procure e selecione **do Azure AD B2C**.
3. Selecione **aplicativos**e, em seguida, selecione **Add**.
4. Introduza um nome para a aplicação. Por exemplo *testapp1*.
5. Para **incluir a aplicação web / web API** e **permitir fluxo implícito**, selecione **Sim**.
6. Para **URL de resposta**, introduza o ponto final onde o Azure AD B2C deverá devolver quaisquer tokens que a aplicação solicite. Por exemplo, pode defini-lo para escutar localmente em `https://localhost:44316`. Se ainda não conhece o número de porta, pode introduzir um valor de marcador de posição e alterá-la mais tarde.
7. Clique em **Criar**.

### <a name="create-a-client-secret"></a>Criar um segredo do cliente

Se seu aplicativo chama uma API web protegida pelo Azure AD B2C, terá de criar um segredo de aplicação.

1. Selecione **chaves** e, em seguida, clique em **gerar chave**. 
2. Selecione **guardar** para ver a chave. Anote o valor da **Chave da aplicação**. Utilize o valor como o segredo de aplicação no código da aplicação.
3. Selecione **acesso à API**, clique em **Add**e selecione a web API e os âmbitos (permissões).

## <a name="register-a-web-api"></a>Registar uma API Web

1. Certifique-se de que está a utilizar o diretório que contém o seu inquilino do Azure AD B2C, clicando no **filtro de diretório e subscrição** no menu superior e escolher o diretório que contém o seu inquilino.
2. Escolher **todos os serviços** no canto superior esquerdo do portal do Azure e, em seguida, procure e selecione **do Azure AD B2C**.
3. Selecione **aplicativos**e, em seguida, selecione **Add**.
4. Introduza um nome para a aplicação. Por exemplo *testapp2*.
5. Para **incluir a aplicação web / web API** e **permitir fluxo implícito**, selecione **Sim**.
6. Para **URL de resposta**, introduza o ponto final onde o Azure AD B2C deverá devolver quaisquer tokens que a aplicação solicite. Por exemplo, pode defini-lo para escutar localmente em `https://localhost:44316`. Se ainda não conhece o número de porta, pode introduzir um valor de marcador de posição e alterá-la mais tarde.
7. Para **URI de ID de aplicação**, introduza o identificador utilizado para a API web. O URI identificador completo, incluindo o domínio, é gerado por si. Por exemplo, `https://contosotenant.onmicrosoft.com/api`.
8. Clique em **Criar**.
9. Selecione **âmbitos publicados** para adicionar mais âmbitos conforme necessário. Por predefinição, o `user_impersonation` é definido o âmbito. O `user_impersonation` âmbito fornece a outras aplicações a capacidade de aceder a esta API em nome do utilizador com sessão iniciada. Se desejar, o `user_impersonation` âmbito pode ser removido.

## <a name="register-a-mobile-or-native-application"></a>Registar uma aplicação móvel ou nativa

1. Certifique-se de que está a utilizar o diretório que contém o seu inquilino do Azure AD B2C, clicando no **filtro de diretório e subscrição** no menu superior e escolher o diretório que contém o seu inquilino.
2. Escolher **todos os serviços** no canto superior esquerdo do portal do Azure e, em seguida, procure e selecione **do Azure AD B2C**.
3. Selecione **aplicativos**e, em seguida, selecione **Add**.
4. Introduza um nome para a aplicação. Por exemplo *testapp3*.
5. Para **incluir a aplicação web / web API**, selecione **não**.
6. Para **incluir cliente nativo**, selecione **Sim**.
7. Para **URI de redirecionamento**, introduza um [URI de redirecionamento com um esquema personalizado](active-directory-b2c-apps.md). Certifique-se de escolher um URI de redirecionamento boa e não incluem os carateres especiais, como carateres de sublinhado.
8. Clique em **Criar**.

### <a name="create-a-client-secret"></a>Criar um segredo do cliente

Se seu aplicativo chama uma API web protegida pelo Azure AD B2C, terá de criar um segredo de aplicação.

1. Selecione **chaves** e, em seguida, clique em **gerar chave**. 
2. Selecione **guardar** para ver a chave. Anote o valor da **Chave da aplicação**. Utilize o valor como o segredo de aplicação no código da aplicação.
3. Selecione **acesso à API**, clique em **Add**e selecione a web API e os âmbitos (permissões).

## <a name="next-steps"></a>Passos Seguintes

Agora que tem uma aplicação registada no Azure AD B2C, pode concluir um dos [guias de introdução](active-directory-b2c-overview.md) para começar a trabalhar.

> [!div class="nextstepaction"]
> [Criar uma aplicação Web ASP.NET com inscrição, início de sessão e reposição da palavra-passe](active-directory-b2c-devquickstarts-web-dotnet-susi.md)
