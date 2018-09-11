---
title: Configurar a autenticação do Azure Active Directory para a sua aplicação de serviços de aplicações
description: Saiba como configurar a autenticação do Azure Active Directory para a sua aplicação de serviços de aplicações.
author: mattchenderson
services: app-service
documentationcenter: ''
manager: syntaxc4
editor: ''
ms.assetid: 6ec6a46c-bce4-47aa-b8a3-e133baef22eb
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/19/2018
ms.author: mahender
ms.openlocfilehash: 0c2ae7e4cf0b19ab9e1c276504892a2a8aaa8dfc
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/10/2018
ms.locfileid: "44297388"
---
# <a name="configure-your-app-service-app-to-use-azure-active-directory-login"></a>Configurar a aplicação de serviço de aplicações para utilizar o início de sessão do Azure Active Directory
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Este artigo mostra-lhe como configurar os serviços de aplicações do Azure para utilizar o Azure Active Directory como um fornecedor de autenticação.

## <a name="express"> </a>Configurar o Azure Active Directory com as definições rápidas
1. Na [portal do Azure], navegue para a aplicação de serviço de aplicações. No painel de navegação esquerdo, selecione **autenticação / autorização**.
2. Se **autenticação / autorização** não está ativado, selecione **no**.
3. Selecione **do Azure Active Directory**e, em seguida, selecione **Express** sob **modo de gestão**.
4. Selecione **OK** para registar a aplicação de serviço de aplicações no Azure Active Directory. Esta ação cria um novo registo de aplicação. Se pretender escolher um registo de aplicação existente em vez disso, clique em **Selecione uma aplicação existente** e, em seguida, procure o nome de um registo de aplicação criado anteriormente no seu inquilino.
   Clique no registo de aplicações para selecioná-lo e clique em **OK**. Em seguida, clique em **OK** na página de definições do Azure Active Directory.
   Por predefinição, o serviço de aplicações fornece autenticação mas não restringe o acesso autorizado para o conteúdo do site e as APIs. Tem de autorizar os utilizadores no seu código de aplicação.
5. (Opcional) Para restringir o acesso ao seu site para apenas os utilizadores autenticados pelo Azure Active Directory, defina **ação a tomar quando o pedido não é autenticado** ao **iniciar sessão com o Azure Active Directory**. Isto requer que todos os pedidos de ser autenticados e todos os pedidos não autenticados são redirecionados para o Azure Active Directory para autenticação.
6. Clique em **Guardar**.

Agora está pronto para utilizar o Azure Active Directory para autenticação na sua aplicação de serviço de aplicações.

## <a name="advanced"> </a>(Método alternativo) Configurar manualmente o Azure Active Directory com definições avançadas
Também pode optar por fornecer definições de configuração manualmente. Esta é a solução preferida, se pretender utilizar o inquilino do AAD é diferente do inquilino com a qual iniciar a sessão no Azure. Para concluir a configuração, primeiro tem de criar um registo no Azure Active Directory e, em seguida, tem de fornecer alguns dos detalhes de registo no serviço de aplicações.

### <a name="register"> </a>Registar a sua aplicação de serviço de aplicações com o Azure Active Directory
1. Inicie sessão para o [portal do Azure]e navegue para a aplicação de serviço de aplicações. Copie o seu aplicativo **URL**. Irá utilizá-lo para configurar o registo de aplicação do Azure Active Directory.
2. Navegue para **do Active Directory**, em seguida, selecione a **registos das aplicações**, em seguida, clique em **novo registo de aplicação** na parte superior para iniciar um novo registo de aplicação. 
3. No **Create** página, introduza um **nome** para o registo de aplicação, selecione o **aplicação Web / API** digite, no **URL de início de sessão** caixa colar o URL da aplicação (a partir do passo 1). Em seguida, clique para **criar**.
4. Em alguns segundos, deverá ver o novo registo de aplicação que acabou de criar.
5. Assim que o registo de aplicações tiver sido adicionado, clique no nome do registo da aplicação, clique em **configurações** na parte superior, em seguida, clique em **propriedades** 
6. Na **URI de ID de aplicação** caixa, cole o URL de aplicação (a partir do passo 1), também na **URL da Home Page** colar o URL de aplicação (a partir do passo 1), em seguida, clique em **guardar**
7. Agora, clique no **URLs de resposta**, edite a **URL de resposta**, cole o URL de aplicação (a partir do passo 1), modifique o protocolo para se certificar de que tenha **https://** protocolo (http:// não), em seguida, é acrescentado ao fim do URL, */.auth/login/aad/callback* (por exemplo, `https://contoso.azurewebsites.net/.auth/login/aad/callback`). Clique em **Guardar**.   
8.  Neste momento, copie os **ID da aplicação** para a aplicação. Mantenha-a para utilização posterior. Irá precisar dele para configurar a sua aplicação de serviço de aplicações.
9. Fechar o **aplicação registada** página. Na **registos de aplicações** página, clique no **pontos de extremidade** botão na parte superior, em seguida, copie o **documento de metadados de Federação** URL. 
10. Abra uma nova janela do browser e navegue para o URL, colar e navegar para a página XML. Na parte superior do documento é um **EntityDescriptor** elemento. Encontrar o **entityID** de atributo e copie o seu valor. Funciona como seu **URL de emissor**. Irá configurar a sua aplicação para utilizar mais tarde.

### <a name="secrets"> </a>Adicionar informações do Active Directory do Azure à sua aplicação de serviço de aplicações
1. De volta a [portal do Azure], navegue para a aplicação de serviço de aplicações. Clique em **autenticação/autorização**. Se não estiver ativada a funcionalidade de autenticação/autorização, ative o comutador **no**. Clique em **do Azure Active Directory**, sob provedores de autenticação, para configurar a sua aplicação. (Opcional) Por predefinição, o serviço de aplicações fornece autenticação mas não restringe o acesso autorizado para o conteúdo do site e as APIs. Tem de autorizar os utilizadores no seu código de aplicação. Definir **ação a tomar quando o pedido não é autenticado** ao **iniciar sessão com o Azure Active Directory**. Esta opção requer que todos os pedidos de ser autenticados e todos os pedidos não autenticados são redirecionados para o Azure Active Directory para autenticação.
2. Na configuração da autenticação do Active Directory, clique em **avançadas** sob **modo de gestão**. Cole o ID da aplicação na caixa de ID de cliente (a partir do passo 8) e cole o valor de URL de emissor no entityId (a partir do passo 10). Em seguida, clique em **OK**.
3. Na página de configuração da autenticação do Active Directory, clique em **guardar**.

Agora está pronto para utilizar o Azure Active Directory para autenticação na sua aplicação de serviço de aplicações.

## <a name="optional-configure-a-native-client-application"></a>(Opcional) Configurar uma aplicação cliente nativa
O Azure Active Directory também permite que Registre clientes nativos, que proporciona maior controle sobre as permissões de mapeamento. Isso é necessário se pretender efetuar inícios de sessão com uma biblioteca, como o **Active Directory Authentication Library**.

1. Navegue para **do Azure Active Directory** no [portal do Azure].
2. No painel de navegação esquerdo, selecione **registos das aplicações**. Clique em **novo registo de aplicação** na parte superior.
4. Na **Create** página, introduza um **nome** para o seu registo de aplicação. Selecione **nativo** na **tipo de aplicação**.
5. Na **URI de redirecionamento** , introduza o seu site */.auth/login/done* ponto de extremidade, usando o esquema HTTPS. Este valor deve ser semelhante à *https://contoso.azurewebsites.net/.auth/login/done*. Se criar um aplicativo do Windows, em vez disso, utilize o [SID do pacote](../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) como o URI.
5. Clique em **Criar**.
6. Assim que tiver sido adicionado o registo de aplicações, selecione-o para abri-lo. Encontrar o **ID da aplicação** e tome nota deste valor.
7. Clique em **todas as definições** > **permissões obrigatórias** > **adicionar** > **selecionar uma API**.
8. Escreva o nome da aplicação do serviço de aplicações que registrou anteriormente para procurá-lo, em seguida, selecione-o e clique em **selecione**. 
9. Selecione **acesso \<app_name >**. Em seguida, clique em **Selecionar**. Em seguida, clique em **Guardar**.

Agora que configurou uma aplicação cliente nativa que pode aceder à sua aplicação de serviço de aplicações.

## <a name="related-content"> </a>Conteúdo relacionado
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-webapp-url.png
[1]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app_registration.png
[2]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-registration-create.png
[3]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-registration-config-appidurl.png
[4]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-registration-config-replyurl.png
[5]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-endpoints.png
[6]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-endpoints-fedmetadataxml.png
[7]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-webapp-auth.png
[8]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-webapp-auth-config.png



<!-- URLs. -->

[Portal do Azure]: https://portal.azure.com/
[alternative method]:#advanced
