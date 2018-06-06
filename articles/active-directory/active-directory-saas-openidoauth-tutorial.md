---
title: Passos para configurar uma aplicação de OpenID/OAuth da Galeria de aplicações do Azure AD | Microsoft Docs
description: Passos para configurar uma aplicação de OpenID/OAuth da Galeria de aplicações do Azure AD.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: eedebb76-e78c-428f-9cf0-5891852e79fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2018
ms.author: jeedes
ms.openlocfilehash: 109ee4f2d723d3dbebba5374e95a39d058e47f42
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34727908"
---
# <a name="steps-to-configure-an-openidoauth-application-from-azure-ad-app-gallery"></a>Passos para configurar uma aplicação de OpenID/OAuth da Galeria de aplicações do Azure AD

## <a name="process-of-open-id-application-addition-from-gallery"></a>Processo de adição de aplicação de abrir ID da Galeria

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory](./media/active-directory-saas-openidoauth-tutorial/tutorial_general_01.png)

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa](./media/active-directory-saas-openidoauth-tutorial/tutorial_general_02.png)

3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação](./media/active-directory-saas-openidoauth-tutorial/tutorial_general_03.png)

4. Na caixa de pesquisa, escreva **nome da aplicação**, selecione o **pretendido aplicação** no painel de resultados de início de sessão até a aplicação.

    ![A adicionar a aplicação](./media/active-directory-saas-openidoauth-tutorial/addfromgallery.png)

    > [!NOTE]
    > Para abrir ID Connect e OAuth aplicações adicionar botão está desativado por predefinição. Aqui o administrador inquilino deverá clicar no **inscrição** botão e forneça o consentimento para a aplicação. Com que que a aplicação será adicionada para o inquilino de cliente, sem necessidade de adicionar explicitamente e efetue as configurações.

    ![Botão Adicionar](./media/active-directory-saas-openidoauth-tutorial/addbutton.png)

5. Quando clica na ligação de inscrição, são redirecionados para a página do Azure AD para credenciais de início de sessão.

6. Após a autenticação com êxito, tem de utilizador para aceitar o consentimento a partir da página de consentimento e depois, a home page de aplicação será apresentado.

    > [!NOTE]
    > Os clientes estão autorizados a adicionar apenas uma instância da aplicação. Se já tiver adicionado um e tentou fornecer o consentimento novamente que será não ser adicionado novamente no inquilino. Por isso, logicamente podem utilizar apenas uma instância de aplicação no inquilino.

## <a name="authentication-flow-using-openid-connect"></a>Fluxo de autenticação com OpenID Connect

O fluxo de início de sessão mais básico contém os seguintes passos - cada um deles é descrita detalhadamente abaixo.

![Fluxo de autenticação com OpenID Connect](./media/active-directory-saas-openidoauth-tutorial/authenticationflow.png)

* **Aplicação multi-inquilino** -uma aplicação multi-inquilino destina-se em muitas organizações, não apenas uma organização. Estas são normalmente software-como-um-serviço (SaaS) aplicações escritas por um fabricante independente de software (ISV). Aplicações de multi-inquilinos tem de ser aprovisionado no cada diretório onde que serão utilizados, que requer o consentimento do utilizador ou administrador registá-los. Este processo de consentimento começa quando uma aplicação registada no diretório e é dado acesso para a Graph API ou talvez outro web API. Quando um utilizador ou administrador de uma organização diferente, inscreva-se para utilizar a aplicação, são apresentados com uma caixa de diálogo que mostra as permissões que a aplicação requer. O utilizador ou administrador pode, em seguida, autorizar a aplicação, o que permite o acesso de aplicação para os dados declarados e, finalmente, regista a aplicação no seu diretório.

    > [!NOTE]
    > Se estiver a efetuar a aplicação disponível para utilizadores em vários diretórios, precisa de um mecanismo para determinar qual inquilino estão contidos na. Uma aplicação de inquilino único apenas tem de ter um aspeto no seu próprio diretório para um utilizador, enquanto uma aplicação multi-inquilino tem de identificar um utilizador específico de todos os diretórios no Azure AD. Para realizar esta tarefa, o Azure AD fornece um ponto final autenticação comuns em que qualquer aplicação multi-inquilino pode direcionar o início de sessão pedidos, em vez de um ponto final de inquilino específico. Este ponto final é [ https://login.microsoftonline.com/common ](https://login.microsoftonline.com/common) para todos os diretórios no Azure AD, enquanto que um ponto final de inquilino específico poderá ser [ https://login.microsoftonline.com/contoso.onmicrosoft.com ](https://login.microsoftonline.com/contoso.onmicrosoft.com). O ponto final comum é especialmente importante a considerar quando desenvolver a sua aplicação, porque terá da lógica necessária para lidar com vários inquilinos durante o início de sessão, fim de sessão e validação de token.

Equipa AD do Azure por predefinição promove a aplicação multi-inquilino como que podem ser acedidos facilmente entre várias organizações e é fácil de utilizar após aceitar o consentimento do utilizador.

## <a name="what-is-consent-framework"></a>O que é o consentimento Framework?

A estrutura de consentimento do Azure AD torna mais fácil desenvolver aplicações de cliente nativo e web do multi-inquilino. Estas aplicações permitam início de sessão por contas de utilizador de um inquilino do Azure AD, diferente em que a aplicação está registada. Pode também precisam de acesso web APIs, tais como o Microsoft Graph API (para aceder ao Azure Active Directory, o Intune e serviços do Office 365) e APIs de outros serviços da Microsoft, além das suas próprias APIs da web. A arquitetura baseia-se um utilizador ou um administrador dar consentimento para uma aplicação que lhe pede para ser registado no seu diretório, o que poderá envolver o método aceder aos dados de diretório. Depois de consentimento é determinado, a aplicação de cliente será capaz de ligar para o Microsoft Graph API em nome do utilizador e utilize as informações conforme necessário.

O [Microsoft Graph API](https://graph.microsoft.io/) fornece acesso aos dados do Office 365 (por exemplo, calendários e mensagens do Exchange, sites e apresenta uma lista do SharePoint, documentos do OneDrive, blocos de notas do OneNote, tarefas a partir da Planner, livros do Excel, etc.), bem como os utilizadores e grupos do Azure AD e outros objetos de dados da cloud services da Microsoft mais.

Os passos seguintes mostram como o consentimento experiência funciona para o programador da aplicação e o utilizador.

1. Partem do princípio de que tem uma aplicação de cliente web que necessita para pedir permissões específicas para aceder a uma recurso/API. O portal do Azure é utilizado para declarar pedidos de permissão no momento da configuração. Como outras definições de configuração que se tornem parte do registo do AAD da aplicação:

    ![Graph API](./media/active-directory-saas-openidoauth-tutorial/graphapi.png)

2. Considere que foram atualizadas as permissões da sua aplicação, a aplicação está em execução e um utilizador está prestes a utilizá-la pela primeira vez. Primeiro a aplicação tem de obter um código de autorização do Azure AD / autorizar o ponto final. O código de autorização, em seguida, pode ser utilizado para adquirir um novo acesso e atualizar o token.

3. Se o utilizador já não é autenticado, Azure do AD / autorizar pede de ponto final de início de sessão.

    ![Autenticação](./media/active-directory-saas-openidoauth-tutorial/authentication.png)

4. Depois do utilizador tem sessão iniciada, do Azure AD irá determinar se o utilizador tem de ser apresentada uma página de consentimento. Esta determinação baseia-se no se o utilizador (ou o administrador da respetiva organização) já concedeu consentimento a aplicação. Se já não tenha sido concedido consentimento, o Azure AD pede ao utilizador consentimento e apresenta as permissões necessárias, tem de funcionar. O conjunto de permissões que é apresentado na caixa de diálogo de consentimento corresponder aos perfis selecionados nas permissões delegadas no portal do Azure.

    ![Página de consentimento](./media/active-directory-saas-openidoauth-tutorial/consentpage.png)

Alguns permissões podem ser autorizadas por um utilizador normal, enquanto outras precisam de consentimento de um administrador inquilino.

## <a name="whats-the-difference-between-admin-consent-and-user-consent"></a>O que é a diferença entre Admin consentimento e consentimento do utilizador?

Como administrador, pode também autorizar para as permissões delegadas de uma aplicação em nome de todos os utilizadores no seu inquilino. Consentimento administrativo impede a caixa de diálogo de consentimento de apresentação para cada utilizador no inquilino e pode ser efetuado no portal do Azure por utilizadores com a função de administrador. A página de definições para a sua aplicação, clique nas permissões necessárias e clique no botão de conceder permissões.

![Conceder permissão](./media/active-directory-saas-openidoauth-tutorial/grantpermission.png)

> [!NOTE]
> É atualmente necessária para aplicações de página única (SPA) que utilizam ADAL.js consentimento explícito com o botão de conceder permissões de concessão de permissões. Caso contrário, a aplicação falha quando o token de acesso é solicitado.

Permissões só de aplicação necessitam sempre de consentimento de um administrador inquilino. Se a aplicação solicite uma permissão só de aplicação e um utilizador tenta iniciar sessão na aplicação, é apresentada uma mensagem de erro a indicar que o utilizador não for capaz de consentimento.

Se a sua aplicação utilizar permissões que necessitam de consentimento de administrador, tem de ter um gesto de como um botão ou a ligação em que o administrador pode iniciar a ação. O pedido a aplicação envia para esta ação é o habitual OAuth2/OpenID Connect autorização pedido que também inclui a linha de comandos = admin_consent parâmetro de cadeia de consulta. Depois do administrador consentiu e o principal de serviço é criado no inquilino do cliente, os pedidos de início de sessão subsequentes não precisam de linha de = admin_consent parâmetro. Uma vez que o administrador tem de decidir que as permissões de pedido são aceitáveis, não existem outros utilizadores no inquilino recebem consentimento a partir desse ponto. Um administrador inquilino pode desativar a capacidade dos utilizadores regulares autorizar aplicações. Se esta capacidade estiver desativada, o consentimento de admin é sempre necessário para a aplicação a ser utilizado no inquilino. Se pretender testar a aplicação com o consentimento do utilizador final desativado, pode encontrar o parâmetro de configuração no [portal do Azure](https://portal.azure.com/) no [as definições de utilizador](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/UserSettings/menuId/) secção em **Enterprise aplicações**

A linha de comandos = admin_consent parâmetro também pode ser utilizado por aplicações que efetuam pedidos de permissões que não necessitam de consentimento de admin. Quando isto poderia ser utilizado um exemplo é se a aplicação requer uma experiência em que o administrador inquilino "inscreve" é pedido aos utilizadores um de tempo e não outros consentimento a partir desse ponto.

Se necessita de uma aplicação admin consentimento e um administrador inicia sessão sem a linha de comandos = parâmetro admin_consent enviado, quando o administrador permitir com êxito para a aplicação aplica-se apenas para a conta de utilizador. Os utilizadores regulares serão ainda não conseguir iniciar sessão ou autorizar a aplicação. Esta funcionalidade é útil se pretender conceder ao administrador de inquilinos a capacidade para explorar a aplicação antes de permitir acesso a outros utilizadores.