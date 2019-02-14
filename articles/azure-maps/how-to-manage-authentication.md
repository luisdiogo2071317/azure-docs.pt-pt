---
title: Gerir a autenticação no Azure Maps | Documentos da Microsoft
description: Pode utilizar o portal do Azure para gerir a autenticação no Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 02/14/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 68c3c8ac39f5803e01ee1038ec85ddb96ac80b30
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/14/2019
ms.locfileid: "56242690"
---
# <a name="manage-authentication-in-azure-maps"></a>Gerir a autenticação no Azure Maps

Depois de criar uma conta do Azure Maps, o ID de cliente e as chaves são criadas para suportar o Azure Active Directory (Azure AD) ou autenticação de chave partilhada.

Pode encontrar os detalhes da sua autenticação ao navegar para o **autenticação** página sob **definições** no portal do Azure. Navegue até à sua conta. Em seguida, selecione **autenticação** no menu.


## <a name="view-authentication-details"></a>Ver detalhes de autenticação

Para ver os detalhes da sua autenticação, navegue para o **autenticação** opção no menu de definições.

![Autenticação de modo de exibição](./media/how-to-manage-authentication/how-to-view-auth.png)

 Para saber mais sobre autenticação e o Azure Maps, consulte [autenticação com o Azure Maps](https://aka.ms/amauth).


## <a name="configure-azure-ad-app-registration"></a>Configurar o registo de aplicação do Azure AD

Depois de criar uma conta de mapa do Azure, é necessária uma ligação entre o seu inquilino do Azure AD e o recurso do Azure de mapas do Azure. 

1. Aceda ao painel do Azure AD e criar um registo de aplicações com um nome e o URL de início de sessão como a página inicial da aplicação web / API, tais como "https://localhost/". Se já tiver um aplicativo registrado, avance para o passo 2.

    ![Registo de aplicações](./media/how-to-manage-authentication/app-registration.png)

    ![Registo de aplicações](./media/how-to-manage-authentication/app-create.png)

2. Atribuir permissões delegadas de API para o Azure Maps ao navegar para o aplicativo em registos de aplicações e clique em **definições**.  Selecione **permissões obrigatórias** e selecione **Add**. Pesquisar, em seguida, selecione Azure Maps sob **selecionar uma API** e clique em **selecione**.

    ![Permissões da aplicação api](./media/how-to-manage-authentication/app-permissions.png)

3. Por fim, em permissões selecione escolher o acesso do Azure Maps e clique em selecionar.

    ![Selecione as permissões da aplicação api](./media/how-to-manage-authentication/select-app-permissions.png)

4. Siga o passo uma ou b, dependendo da implementação de autenticação.

    1. Se o aplicativo é destinar a utilizar a autenticação de token de utilizador com o nosso SDK Web do Azure Maps, tem de ativar o `oauthEnableImplicitFlow` ao defini-la como true na secção manifesto da página de detalhes do registo da aplicação.
    
       ![Manifesto da aplicação](./media/how-to-manage-authentication/app-manifest.png)

    2. Se a autenticação de servidor/aplicação de utilizações de aplicação para o **chaves** painel dentro do registo da aplicação e a criar uma palavra-passe ou carregar um certificado de chave pública para o registo de aplicações. Se criar uma palavra-passe, uma vez que **guardar**, copie a palavra-passe para utilizar mais tarde e armazena de forma segura, irá utilizá-lo a adquirir tokens do Azure AD.

       ![Chaves da aplicação](./media/how-to-manage-authentication/app-keys.png)


## <a name="grant-rbac-to-azure-maps"></a>RBAC de concessão para o Azure Maps

Depois de uma conta do Azure Maps tiver sido associada ao seu inquilino do Azure AD, o controlo de acesso pode ser concedido ao atribuir o utilizador ou aplicação para funções de controlo de acesso do Azure Maps disponíveis.

1. Navegue para **controlo de acesso** , clique em **atribuições de funções**, e **adicionar atribuição de função**.

    ![RBAC de concessão](./media/how-to-manage-authentication/how-to-grant-rbac.png)

2. No pop de atribuição de função fora da janela, selecione o leitor de data de mapas do Azure (pré-visualização) **função**, **atribuir acesso a**: Utilizador do Azure AD, grupo ou principal de serviço, **selecionar** utilizador ou aplicação no menu pendente, e **guardar**.

    ![Adicionar atribuição de função](./media/how-to-manage-authentication/add-role-assignment.png)

## <a name="view-available-azure-maps-rbac-roles"></a>Ver as funções de RBAC do Azure Maps disponíveis

Para ver as funções de controlo de acesso baseado em funções disponíveis para o Azure Maps, que pode ser concedido o acesso a, navegue para **controlo de acesso (IAM)** opção, clique em **funções**, e a partir da pesquisa para funções **Do azure Maps**.

![Modo de exibição de funções disponíveis](./media/how-to-manage-authentication/how-to-view-avail-roles.png)


## <a name="view-azure-maps-role-based-access-control-rbac"></a>Ver o Azure Maps controlo de acesso baseado em funções (RBAC)

O Azure AD permite para controlo de acesso granular através do controlo de acesso baseado em funções (RBAC). 

Para ver utilizadores ou aplicações que tenham sido concedidas RBAC para o Azure Maps, navegue para **controlo de acesso (IAM)** opção, selecione **atribuições de funções**e filtre por **Azure Maps**. Todas as funções disponíveis serão apresentada abaixo.

![Vista de RBAC](./media/how-to-manage-authentication/how-to-view-amrbac.png)


## <a name="request-tokens-for-azure-maps"></a>Pedir tokens para o Azure Maps

Assim que a sua aplicação esteja registrada e associada mapas do Azure, pode agora pedir tokens de acesso.

* Se o aplicativo é destinar a utilizar a autenticação de token de utilizador com o nosso SDK de Web do Azure Maps (Web), tem de configurar a sua página html com o ID de cliente de mapas do Azure e o ID de aplicação do Azure AD

* Para aplicações através da autenticação de aplicação/servidor, solicitar um token a partir do ponto final de início de sessão do Azure AD `https://login.microsoftonline.com` com o ID de recurso do Azure AD `https://atlas.microsoft.com/`, certificado ou a palavra-passe registo ID de cliente do Azure Maps, ID de aplicação do Azure AD e aplicações do Azure AD.

Para obter mais informações sobre como pedir tokens de acesso do Azure AD para utilizadores e os principais de serviço, consulte [cenários de autenticação do Azure AD](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios).


## <a name="next-steps"></a>Passos Seguintes

* Para saber mais sobre a autenticação do Azure AD e o SDK de Web de mapas do Azure, veja [do Azure AD e o SDK Web do Azure Maps](https://docs.microsoft.com/azure/azure-maps/how-to-use-map-control).