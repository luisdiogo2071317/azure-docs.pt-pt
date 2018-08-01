---
title: Publicar aplicações com o Proxy da Aplicação do Azure AD | Microsoft Docs
description: Publicar aplicações no local para a cloud com o Proxy de aplicações do Azure AD no portal do Azure.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/24/2018
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 1224642bb7e6fc0c51b3f839a78449132db5b4bb
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/31/2018
ms.locfileid: "39364262"
---
# <a name="publish-applications-using-azure-ad-application-proxy"></a>Publicar aplicações com o Proxy da Aplicação do Azure AD

Proxy de aplicações do Azure Active Directory (AD) ajuda-o a oferecer suporte a funcionários remotos ao publicar aplicações no local para ser acedido através da internet. Pode publicar estas aplicações através do portal do Azure para fornecer acesso remoto seguro a partir de fora da rede.

Este artigo orienta-o pelos passos para publicar uma aplicação no local com o Proxy de aplicações. Depois de concluir este artigo, os utilizadores poderão aceder remotamente à sua aplicação. E estará pronto para configurar funcionalidades adicionais para a aplicação como o início de sessão único, informações personalizadas e requisitos de segurança.

Se estiver familiarizado com o Proxy da aplicação, saiba mais sobre esta funcionalidade com o artigo [como fornecer acesso remoto seguro a aplicações no local](application-proxy.md).

## <a name="before-you-begin"></a>Antes de começar

Este artigo pressupõe que já instalou e registar um conector. Se ainda precisar de fazer esses passos, consulte [introdução ao Proxy da aplicação e instalar o conector](application-proxy-enable.md).

## <a name="publish-an-on-premises-app-for-remote-access"></a>Publicar uma aplicação no local para acesso remoto

Siga estes passos para publicar as suas aplicações com o Proxy de aplicações. Se ainda não tiver já transferido e configurado um conector para a sua organização, aceda a [introdução ao Proxy da aplicação e instalar o conector](application-proxy-enable.md) primeiro e, em seguida, publique a sua aplicação.

> [!TIP]
> Se estiver testando o Proxy de aplicações pela primeira vez, escolha uma aplicação que está configurada para autenticação baseada em palavra-passe. Proxy de aplicações suporta outros tipos de autenticação, mas os aplicativos baseados em palavra-passe são a mais fácil de pôr em funcionamento rapidamente. 

1. Inicie sessão como administrador no [portal do Azure](https://portal.azure.com/).
2. Selecione **do Azure Active Directory** > **aplicações empresariais** > **novo aplicativo**.

  ![Adicionar uma aplicação empresarial](./media/application-proxy-publish-azure-portal/add-app.png)

3. Selecione **todos os**, em seguida, selecione **aplicação no local**.  

  ![Adicionar a sua própria aplicação](./media/application-proxy-publish-azure-portal/add-your-own.png)

4. Forneça as seguintes informações sobre a aplicação:

   - **Nome**: O nome da aplicação que será apresentado no painel de acesso e no portal do Azure. 

   - **URL interno**: O URL que utilizar para aceder à aplicação do interior da rede privada. Pode fornecer um caminho específico no servidor de back-end para publicação, enquanto o resto do servidor não é publicado. Dessa forma, pode publicar sites diferentes no mesmo servidor que aplicações diferentes e atribuir cada uma de suas próprias regras de acesso e o nome.

     > [!TIP]
     > Se publicar um caminho, certifique-se de que inclui todas as imagens, scripts e folhas de estilo necessários para a sua aplicação. Por exemplo, se seu aplicativo está em https://yourapp/app e utiliza as imagens localizadas em https://yourapp/media, em seguida, deve publicar https://yourapp/ como o caminho. Este URL interno não tem de ser os seus utilizadores verão a página de destino. Para obter mais informações, consulte [definir uma página inicial personalizada para aplicações publicadas](application-proxy-configure-custom-home-page.md).

   - **URL externo**: O endereço os utilizadores serão Ir para conseguir aceder à aplicação a partir de fora da rede. Se não pretender utilizar o domínio de Proxy de aplicações predefinido, ler sobre [domínios personalizados no Proxy de aplicações do Azure AD](application-proxy-configure-custom-domain.md).
   - **Pré-autenticação**: como Proxy da aplicação verifica os utilizadores antes de conceder acesso à sua aplicação. 

     - Azure Active Directory: a Proxy da Aplicação redireciona os utilizadores para iniciarem sessão no Azure AD, que autentica as respetivas permissões para o diretório e aplicação. É recomendável manter esta opção como predefinição, para que possam tirar partido das funcionalidades de segurança do Azure AD como o acesso condicional e multi-factor Authentication.
     - Passth-Rough: Os utilizadores não têm a autenticação no Azure Active Directory para aceder à aplicação. Pode ainda configurar os requisitos de autenticação back-end.
   - **Grupo do conector**: conectores processam o acesso remoto à sua aplicação e ajudam a organizar os conectores e aplicações por região, rede ou para fins de grupos de conector. Se não tem quaisquer grupos de conector ainda criados, a sua aplicação é atribuída a **predefinido**.

>[!NOTE]
>Se a sua aplicação utilizar websockets para se ligar, certifique-se de que tem a versão do conector 1.5.612.0 ou superior com suporte de websocket e o grupo do conector atribuído utiliza apenas estes conectores.

   ![Configurar a aplicação](./media/application-proxy-publish-azure-portal/configure-app.png)
5. Se for necessário, configure as definições adicionais. Para a maioria dos aplicativos, deve manter estas definições em seus Estados de predefinição. 
   - **Tempo limite da aplicação de back-end**: defina este valor para **longo** apenas se a sua aplicação está lenta autenticar e ligar. 
   - **Traduzir URLs em cabeçalhos**: manter este valor como **Sim** , a menos que seu aplicativo necessário o cabeçalho de anfitrião original no pedido de autenticação.
   - **Traduzir URLs no corpo do aplicativo**: manter este valor como **não** a menos que tenha inserido no código HTML links para outras aplicações no local e não utilizar domínios personalizados. Para obter mais informações, consulte [vincular a tradução com Proxy de aplicações](application-proxy-configure-hard-coded-link-translation.md).
   
   ![Configurar a aplicação](./media/application-proxy-publish-azure-portal/additional-settings.png)

6. Selecione **Adicionar**.


## <a name="add-a-test-user"></a>Adicionar um utilizador de teste 

Para testar que a aplicação foi publicada corretamente, adicione uma conta de utilizador de teste. Certifique-se de que esta conta já tem permissões para aceder à aplicação a partir de dentro da rede empresarial.

1. Novamente no painel início rápido, selecione **atribuir um utilizador para fins de teste**.

  ![Atribuir um utilizador para fins de teste](./media/application-proxy-publish-azure-portal/assign-user.png)

2. No painel de grupos e utilizadores, selecione **adicionar**.

  ![Adicionar um utilizador ou grupo](./media/application-proxy-publish-azure-portal/add-user.png)

3. No painel de atribuição de adicionar, selecione **utilizadores e grupos** , em seguida, escolha a conta que pretende adicionar. 
4. Selecione **Atribuir**.

## <a name="test-your-published-app"></a>Testar a aplicação publicada

No seu browser, navegue para o URL externo que configurou durante a etapa de publicação. Deve ver o ecrã de início e ser capaz de iniciar sessão com a conta de teste que configurou.

![Testar a aplicação publicada](./media/application-proxy-publish-azure-portal/test-app.png)


## <a name="next-steps"></a>Passos Seguintes
- [Transferir conectores](application-proxy-enable.md) e [criar grupos de conector](application-proxy-connector-groups.md) a publicação de aplicações em redes separadas e localizações.

- [Configurar o início de sessão único](application-proxy-configure-single-sign-on-password-vaulting.md) para seu aplicativo publicado recentemente
