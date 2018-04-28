---
title: Tutorial - registar uma aplicação para ativar a inscrição e o início de sessão utilizando o Azure Active Directory B2C | Microsoft Docs
description: Utilize o portal do Azure para criar um inquilino do Azure AD B2C e registar uma aplicação com o mesmo.
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: patricka
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 03/08/2018
ms.author: davidmu
ms.openlocfilehash: 81ab3288d7a365c2665b3b38ca220a3e7cb648c7
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="tutorial-register-an-application-to-enable-sign-up-and-sign-in-using-azure-active-directory-b2c"></a>Tutorial: Registar uma aplicação para ativar a inscrição e o início de sessão utilizando o Azure Active Directory B2C

Este tutorial ajuda-o a criar um inquilino do Microsoft Azure Active Directory (Azure AD) B2C e registar uma aplicação com o mesmo em apenas alguns minutos.

Neste artigo, vai aprender a:

> [!div class="checklist"]
> * Criar um inquilino do Azure AD B2C
> * Ligar o seu inquilino para a sua subscrição
> * Registar a sua aplicação

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no [Portal do Azure](https://portal.azure.com/).

## <a name="create-an-azure-ad-b2c-tenant"></a>Criar um inquilino do Azure AD B2C

Os inquilinos existentes não podem ser ativadas funcionalidades do B2C. Terá de criar um inquilino do Azure AD B2C.

[!INCLUDE [active-directory-b2c-create-tenant](../../includes/active-directory-b2c-create-tenant.md)]

Parabéns, criou um inquilino do Azure Active Directory B2C. É um Administrador Global do inquilino. Pode adicionar outros administradores globais conforme necessário. Para mudar para o seu inquilino novo, clique em de *gerir a nova ligação de inquilino*.

![Gerir a nova ligação de inquilino](./media/active-directory-b2c-get-started/manage-new-b2c-tenant-link.png)

> [!IMPORTANT]
> Se estiver a planear utilizar um inquilino do B2C para uma aplicação de produção, leia o artigo sobre [escala de produção vs inquilinos de pré-visualização B2C](active-directory-b2c-reference-tenant-type.md). Não existe são problemas conhecidos quando eliminar um inquilino do B2C existente e volte a criá-la com o mesmo nome de domínio. Terá de criar um inquilino do B2C com um nome de domínio diferente.
>
>

## <a name="link-your-tenant-to-your-subscription"></a>Ligar o seu inquilino para a sua subscrição

Terá de ligar o seu inquilino do Azure AD B2C a sua subscrição do Azure para ativar todas as funcionalidades do B2C e paga custos de utilização. Para obter mais informações, leia [neste artigo](active-directory-b2c-how-to-enable-billing.md). Se não ligar o seu inquilino do Azure AD B2C a sua subscrição do Azure, algumas funcionalidades é bloqueada e, se vir uma mensagem de aviso ("nenhuma subscrição associado a este inquilino do B2C ou as necessidades de subscrição sua atenção.") nas definições do B2C. É importante que efetuar este passo antes das suas aplicações são enviados para a produção.


[!INCLUDE [active-directory-b2c-find-service-settings](../../includes/active-directory-b2c-find-service-settings.md)]

Também pode aceder ao painel introduzindo `Azure AD B2C` no **procurar recursos** na parte superior do portal. Na lista de resultados, selecione **do Azure AD B2C** aceder ao painel de definições do B2C.

## <a name="register-your-application"></a>Registar a sua aplicação

Para criar uma aplicação que aceite a inscrição e o início de sessão do consumidor, terá primeiro de registar a aplicação com um inquilino do Azure Active Directory B2C. Obtenha o seu inquilino, utilizando os passos descritos em [Criar um inquilino do Azure AD B2C](active-directory-b2c-get-started.md).

As aplicações criadas no portal do Azure têm de ser geridas a partir da mesma localização. Se utilizar o PowerShell ou outro portal para editar as aplicações B2C do Azure AD, estas deixam de ser suportadas e não irão funcionar com o Azure AD B2C. Veja os detalhes na secção [Aplicações com falha](#faulted-apps). 

Este artigo utiliza exemplos que o ajudam a começar com os nossos exemplos. Pode saber mais sobre estes exemplos nos artigos subsequentes.

### <a name="navigate-to-b2c-settings"></a>Navegar para as definições do B2C

Inicie sessão no [Portal do Azure](https://portal.azure.com/) como Administrador Global do inquilino do B2C. 

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](../../includes/active-directory-b2c-portal-navigate-b2c-service.md)]

### <a name="choose-next-steps-based-on-your-application-type"></a>Escolha os passos seguintes com base no tipo da sua aplicação

* [Registar uma aplicação Web](#register-a-web-app)
* [Registar uma API Web](#register-a-web-api)
* [Registar uma aplicação móvel ou nativa](#register-a-mobile-or-native-app)
 
### <a name="register-a-web-app"></a>Registar uma aplicação Web

[!INCLUDE [active-directory-b2c-register-web-app](../../includes/active-directory-b2c-register-web-app.md)]

### <a name="create-a-web-app-client-secret"></a>Criar um segredo do cliente da aplicação Web

Se a sua aplicação Web chamar uma API Web protegida pelo Azure AD B2C, efetue estes passos:
   1. Crie um segredo de aplicação ao aceder ao painel **Chaves** e ao clicar no botão **Gerar Chave**. Anote o valor da **Chave da aplicação**. Utilize o valor como o segredo de aplicação no código da aplicação.
   2. Clique em **Acesso à API**, clique em **Adicionar** e selecione a API Web e os âmbitos (permissões).

> [!NOTE]
> Um **Segredo de Aplicação** é uma credencial de segurança importante e deve ser protegida devidamente.
> 

[Ir para os **passos seguintes**](#next-steps)

### <a name="register-a-web-api"></a>Registar uma API Web

[!INCLUDE [active-directory-b2c-register-web-api](../../includes/active-directory-b2c-register-web-api.md)]

Clique em **Âmbitos publicados** para adicionar mais âmbitos, conforme necessário. Por predefinição, é definido o âmbito "user_impersonation". O âmbito user_impersonation fornece a outras aplicações a capacidade de aceder a esta API em nome do utilizador com sessão iniciada. Se quiser, o âmbito user_impersonation pode ser removido.

[Ir para os **passos seguintes**](#next-steps)

### <a name="register-a-mobile-or-native-app"></a>Registar uma aplicação móvel ou nativa

[!INCLUDE [active-directory-b2c-register-mobile-native-app](../../includes/active-directory-b2c-register-mobile-native-app.md)]

[Ir para os **passos seguintes**](#next-steps)

### <a name="choosing-a-web-app-or-api-reply-url"></a>Escolher uma aplicação Web ou um URL de resposta da API

Atualmente, as aplicações registadas com o Azure AD B2C estão restritas a um conjunto limitado de valores de URL de resposta. O URL de resposta para aplicações e serviços Web tem de começar com o esquema `https`, e todos os valores de URL de resposta têm de partilhar um único domínio de DNS. Por exemplo, não pode registar uma aplicação Web que tenha um destes URLs de resposta:

`https://login-east.contoso.com`

`https://login-west.contoso.com`

O sistema de registo compara o nome DNS completo do URL de resposta existente ao nome DNS do URL de resposta que está a adicionar. O pedido para adicionar o nome DNS falha se uma das seguintes condições for verdadeira:

* O nome DNS completo do novo URL de resposta não corresponde ao nome DNS do URL de resposta existente.
* O nome DNS completo do novo URL de resposta não é um subdomínio do URL de resposta existente.

Por exemplo, se a aplicação tiver este URL de resposta:

`https://login.contoso.com`

Pode adicioná-lo, da seguinte forma:

`https://login.contoso.com/new`

Neste caso, o nome DNS corresponde exatamente. Ou pode fazer o seguinte:

`https://new.login.contoso.com`

Neste caso, está a referir-se a um subdomínio de DNS de login.contoso.com. Se quiser ter uma aplicação com login-east.contoso.com e login-west.contoso.com como URLs de resposta, tem de adicionar esses URLs de resposta pela seguinte ordem:

`https://contoso.com`

`https://login-east.contoso.com`

`https://login-west.contoso.com`

Pode adicionar os dois últimos porque são subdomínios do primeiro URL de resposta, contoso.com.

### <a name="choosing-a-native-app-redirect-uri"></a>Escolher um URI de redirecionamento da aplicação nativa

Existem duas considerações importantes sobre quando selecionar um URI de redirecionamento para aplicações móveis/nativas:

* **Exclusivo**: o esquema do URI de redirecionamento deve ser exclusivo para cada aplicação. No exemplo (com.onmicrosoft.contoso.appname://redirect/path), com.onmicrosoft.contoso.appname é o esquema. Recomendamos seguir este padrão. Se duas aplicações partilharem o mesmo esquema, o utilizador vê uma caixa de diálogo "selecionar aplicação". Se o utilizador fizer uma seleção incorreta, o início de sessão falha.
* **Completo**: o URI de redirecionamento tem de ter um esquema e um caminho. O caminho tem de conter pelo menos uma barra depois do domínio (por exemplo, //contoso/ funciona e //contoso falha).

Certifique-se de que não existem carateres especiais, como carateres de sublinhado, no uri de redirecionamento.

### <a name="faulted-apps"></a>Aplicações com falha

As aplicações B2C NÃO devem ser editadas:

* Noutros portais de gestão de aplicações, como o [Portal de Registo da Aplicação](https://apps.dev.microsoft.com/).
* Utilizar o Graph API ou o PowerShell

Se editar a aplicação B2C do Azure AD, conforme descrito, e tentar editá-la novamente nas funcionalidades do Azure AD B2C no portal do Azure, ela torna-se uma aplicação com falha e a sua aplicação deixa de ser utilizável com o Azure AD B2C. Deve eliminar a aplicação e criá-la novamente.

Para eliminar a aplicação, aceda ao [Portal de Registo da Aplicação](https://apps.dev.microsoft.com/) e elimine a aplicação. Para que a aplicação fique visível, tem de ser o proprietário da aplicação (e não apenas um administrador do inquilino).

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, aprendeu como:

> [!div class="checklist"]
> * Criar um inquilino do Azure AD B2C
> * Ligar o seu inquilino para a sua subscrição
> * Registar a sua aplicação

> [!div class="nextstepaction"]
> [Ativar uma aplicação web para autenticar com contas](active-directory-b2c-tutorials-web-app.md)