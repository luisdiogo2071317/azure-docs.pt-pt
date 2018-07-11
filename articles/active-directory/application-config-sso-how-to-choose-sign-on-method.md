---
title: Como determinar o início de sessão único no método a utilizar | Documentos da Microsoft
description: Compreender a modos de início de início de sessão únicos suportados pelo Azure AD e como escolher qual delas escolher para a aplicação que está interessado.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: barbkess
ms.openlocfilehash: 79fe30fbe0baed9d62e6bc3328e754958516ef70
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "36335412"
---
# <a name="how-to-determine-what-single-sign-on-method-to-use"></a>Como determinar o início de sessão único no método a utilizar

Este artigo ajuda-o a compreender a modos de início de início de sessão únicos suportados pelo Azure AD e como escolher qual delas escolher para a aplicação que está interessado.

## <a name="single-sign-on-and-provisioning-modes-supported-by-specific-application-types"></a>Início de sessão único e o aprovisionamento de modos suportados por tipos específicos de aplicativos

A tabela seguinte descreve os diferentes únicos início de sessão e aprovisionamento modos suportados por cada um dos tipos de aplicativo anteriores. Pode utilizar esta tabela para ajudá-lo a compreender que aplicação terá de adicionar para oferecer suporte a um objetivo específico.

  ![Tabela de tipos do Pacífico](./media/application-tables/table1.png)

## <a name="how-to-choose-a-single-sign-on-mode"></a>Como escolher um modo de início de sessão único

Seguem-se a suportados **início de sessão único** modos para aplicações do Azure AD.

-   **O Azure AD início de sessão único desativado** – escolha do Azure AD início de sessão único desativado **modo de início de sessão único** se ainda não estiver pronto para integrar esta aplicação com o início de sessão único com o Azure AD, ou são simplesmente testá-lo horizontalmente

-   **Ligado logon** – escolha o [ligado início de sessão](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **modo de início de sessão único** se tiver uma aplicação que já está ligada com uma única início de sessão na solução existente, ou se quiser apenas publicar uma ligação simple para os seus utilizadores no respetivo [painel de acesso de aplicação](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) ou [iniciador de aplicações do Office 365](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)

-   **Com base em palavra-passe de início de sessão** – escolha o [baseado em palavra-passe de início de sessão](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **modo de início de sessão único** se seu aplicativo processa um campo de nome de utilizador e palavra-passe HTML e pretende armazenar esse nome de utilizador e palavra-passe de forma segura a ser reproduzido para a aplicação mais tarde

-   **Baseado em SAML logon** – escolha o [baseado em SAML logon](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) início de sessão único no modo, se o aplicativo dá suporte os protocolos SAML ou OpenID Connect ou, se pretender ser capaz de mapear os utilizadores a funções de aplicação específica, com base em regras define em suas declarações SAML *(* * Nota:** esta opção não está disponível quando o proxy de aplicações está configurado para uma aplicação) *

-   **Com base no cabeçalho de início de sessão** – Escolha esta [com base no cabeçalho de início de sessão](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) único-modo de sessão se tiver uma aplicação com o PingAccess que suporta a autenticação baseada em cabeçalho de HTTP que deseja executar o início de sessão único para *(* * Nota:** esta opção só está disponível quando o proxy de aplicações e o PingAccess estão configuradas para uma aplicação) *

-   **Autenticação Windows integrada** – escolha o [autenticação integrada do Windows](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) início de sessão único no modo ao expor um aplicativo de WIA no local que pretende efetuar o início de sessão único para *(* * Nota:** esta opção só está disponível quando o proxy de aplicações está configurado para uma aplicação) *

## <a name="single-sign-on-modes-for-custom-developed-applications"></a>Modos de início de sessão únicos para aplicações com programação personalizada

Aplicações a que tem personalizadas desenvolvidas ao longo da [aplicação personalizada desenvolvida](#_Custom-Developed_Applications) experiência também suportam adicionais únicos início de sessão em modos listados não tenha sido anteriormente, que incluem:

-   [OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) com base no início de sessão

-   [OpenID Connect 1.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) com base no início de sessão

-   [WS-Federation 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html) com base no início de sessão

-   [SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) com base no início de sessão

Leitura a [Guia do programador do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide) para saber mais sobre como criar uma aplicação personalizada desenvolvida, que oferece suporte a esses modos de início de sessão únicos.

## <a name="how-to-set-an-applications-single-sign-on-mode"></a>Como definir único início de sessão em modo um aplicativo

Para definir um aplicativo **início de sessão único** modo, siga estas instruções:

1.  Abra o [ **portal do Azure** ](https://portal.azure.com/) e inicie sessão como um **Administrador Global** ou **Coadministrador.**

2.  Abra o **extensão do Active Directory do Azure** ao clicar em **todos os serviços** na parte superior do menu de navegação esquerdo principal.

3.  Escreva **"Azure Active Directory**" na caixa de pesquisa de filtro e selecione o **Azure Active Directory** item.

4.  Clique em **aplicações empresariais** no menu de navegação do lado esquerdo do Azure Active Directory.

5.  Clique em **todos os aplicativos** para ver uma lista de todas as suas aplicações.

   * Se não vir a aplicação que quer mostrar aqui, utilize o **filtro** na parte superior do **todas as listas de aplicações** e defina o **mostrar** a opção de **todos os Aplicações.**

6.  Selecione a aplicação que pretende configurar o início de sessão único

7.  Assim que o aplicativo é carregado, clique em **início de sessão único** no menu de navegação do lado esquerdo da aplicação.

## <a name="next-steps"></a>Passos Seguintes
[Fornecer início de sessão único às suas aplicações com o Proxy de aplicações](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md)

