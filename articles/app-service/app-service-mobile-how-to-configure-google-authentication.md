---
title: Como configurar a autenticação de Google para a sua aplicação de serviços aplicacionais
description: Saiba como configurar a autenticação de Google para a sua aplicação de serviços de aplicações.
services: app-service
documentationcenter: ''
author: mattchenderson
manager: syntaxc4
editor: ''
ms.assetid: 2b2f9abf-9120-4aac-ac5b-4a268d9b6e2b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/19/2018
ms.author: mahender
ms.openlocfilehash: f89ff3a030f1da75bca538eefaf2496e9be8e97b
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35233824"
---
# <a name="how-to-configure-your-app-service-application-to-use-google-login"></a>Como configurar a sua aplicação de serviço de aplicações para utilizar o início de sessão do Google
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Este tópico mostra como configurar o serviço de aplicações do Azure para utilizar o Google como um fornecedor de autenticação.

Para concluir o procedimento deste tópico, tem de ter uma conta do Google que tenha um endereço de correio eletrónico verificado. Para criar uma nova conta do Google, aceda a [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).

## <a name="register"> </a>Registar a aplicação com o Google
1. Inicie sessão no [portal do Azure]e navegue até à sua aplicação. Copiar o **URL**, que poderá utiliza mais tarde para configurar a sua aplicação do Google.
2. Navegue para o [Google apis](http://go.microsoft.com/fwlink/p/?LinkId=268303) Web site, inicie sessão com as credenciais da conta Google, clique em **criar projeto**, forneça um **nome do projeto**, em seguida, clique em **criar**.
3. Depois de criado o projeto, selecione-a. No dashboard do projeto, clique em **aceda à descrição geral APIs**.
4. Selecione **ativar APIs e serviços**. Procurar **Google + API**e selecione-o. Em seguida, clique em **ativar**.
5. No painel de navegação esquerdo, **credenciais** > **ecrã de consentimento do OAuth**, em seguida, selecione o **endereço de correio eletrónico**, introduza um **nome de produto**e clique em **guardar**.
6. No **credenciais** separador, clique em **criar credenciais** > **ID de cliente OAuth**.
7. No ecrã "Criar ID de cliente", selecione **aplicação Web**.
8. Cole o serviço de aplicações **URL** que copiou anteriormente para **autorizado origens de JavaScript**, em seguida, cole o redirecionamento URI para **autorizado o URI de redirecionamento**. O redirecionamento URI é o URL da aplicação anexada com o caminho, */.auth/login/google/callback*. Por exemplo, `https://contoso.azurewebsites.net/.auth/login/google/callback`. Certifique-se de que está a utilizar o esquema de HTTPS. Em seguida, clique em **Criar**.
9. No ecrã seguinte, tome nota dos valores de ID de cliente e do segredo do cliente.

    > [!IMPORTANT]
    > O segredo do cliente é uma credencial de segurança importantes. Não partilhe este segredo com ninguém e distribui-lo dentro de uma aplicação de cliente.


## <a name="secrets"> </a>Adicionar informações de Google à sua aplicação
1. Volta a [portal do Azure], navegue até à sua aplicação. Clique em **definições**e, em seguida, **autenticação / autorização**.
2. Se a autenticação / autorização funcionalidade não está ativada, ative o comutador **no**.
3. Clique em **Google**. Cole os valores de ID de aplicação e o segredo de aplicação que obteve anteriormente e, opcionalmente ativar qualquer âmbitos que requer a sua aplicação. Em seguida, clique em **OK**.
   
   ![][1]
   
   Por predefinição, o serviço de aplicações fornece autenticação mas não restringir o acesso autorizado ao conteúdo do site e APIs. Tem de autorizar os utilizadores no seu código de aplicação.
4. (Opcional) Para restringir o acesso ao seu site apenas aos utilizadores autenticados pelo Google, defina **ação a tomar quando o pedido não é autenticado** para **Google**. Isto requer que todos os pedidos de ser autenticados e todos os pedidos não autenticados são redirecionados para o Google para autenticação.
5. Clique em **Guardar**.

Agora está pronto a utilizar o Google para autenticação na sua aplicação.

## <a name="related-content"> </a>Conteúdo relacionado
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Anchors. -->

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-settings.png

<!-- URLs. -->

[Google apis]: http://go.microsoft.com/fwlink/p/?LinkId=268303

[Portal do Azure]: https://portal.azure.com/

