---
title: Problemas em iniciar sessão a uma aplicação utilizando um deeplink | Microsoft Docs
description: Como resolver problemas de acesso à aplicação a partir de um deeplink URL para utilizar o Azure AD
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
ms.reviewer: asteen
ms.openlocfilehash: 2d325c4f806a1bd4f972de5725659a533ab1803a
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "36335541"
---
# <a name="problems-signing-in-to-an-application-using-a-deeplink"></a>Problemas em iniciar sessão a uma aplicação utilizando um deeplink

O painel de acesso é um portal baseado na web que permite que um utilizador com uma conta escolar ou profissional no Azure Active Directory (Azure AD) para ver e iniciar as aplicações baseadas na nuvem que o administrador do Azure AD concedeu-lhes acesso. 

Estas aplicações estão configuradas em nome do utilizador no portal do Azure AD. A aplicação tem de ser corretamente configurada e atribuída ao utilizador ou um grupo que do utilizador é um membro de para ver a aplicação no painel de acesso.

Acesso de utilizador ou ligações avançadas URLs são os seus utilizadores podem utilizar para aceder às suas aplicações de SSO de palavra-passe diretamente a partir do respetivas barras de URL de browsers de ligações. Ao navegar para esta ligação, os utilizadores ser automaticamente iniciou a sessão na aplicação sem ter de aceder ao painel de acesso, primeiro. Esta é a mesma ligação que os utilizadores utilizam para aceder a estas aplicações o iniciador da aplicação do Office 365.

## <a name="general-issues-to-check-first"></a>Problemas gerais para verificar primeiro

-   Certifique-se a utilização de um **browser** que cumpra os requisitos mínimos para o painel de acesso.

-   Certifique-se de que o browser do utilizador foi adicionado o URL da aplicação a respetiva **fidedigna sites**.

-   Certifique-se a aplicação é **configurado** corretamente.

-   Certifique-se de que a conta de utilizador **ativada** para inícios de sessão.

-   Certifique-se de que a conta de utilizador **não bloqueadas.**

-   Certifique-se de que o utilizador **palavra-passe não está expirado ou esquecimento.**

-   Certifique-se **multi-factor Authentication** não está a bloquear o acesso do utilizador.

-   Certifique-se de um **política de acesso condicional** ou **Identity Protection** política não está a bloquear o acesso do utilizador.

-   Certifique-se de que um utilizador **informações de contacto de autenticação** é atualizado para permitir que as políticas de autenticação Multifator ou o acesso condicional a serem impostas.

-   Certifique-se para também tente limpar os cookies do browser e tentar iniciar sessão novamente.

## <a name="checking-the-deeplink"></a>A verificar o deeplink

Para verificar se tem o deeplink correto, siga estes passos:

1.  Abra o [ **portal do Azure** ](https://portal.azure.com/) e inicie sessão como um **Administrador Global** ou **Co-administrador.**

2.  Abra o **extensão do Active Directory do Azure** clicando **todos os serviços** no topo do menu de navegação esquerdo principal.

3.  Escreva **"do Azure Active Directory**" na caixa de pesquisa de filtro e selecione o **do Azure Active Directory** item.

4.  Clique em **aplicações empresariais** no menu de navegação esquerdo do Azure Active Directory.

5.  Clique em **todas as aplicações** para ver uma lista de todas as suas aplicações.

  * Se não vir a aplicação que pretende mostrar aqui, utilize o **filtro** controlo na parte superior a **lista de todas as aplicações** e defina o **mostrar** opção para **todos os Aplicações.**

6.  Abra o [ **portal do Azure** ](https://portal.azure.com/) e inicie sessão como um **Administrador Global** ou **Co-administrador.**

7.  Abra o **extensão do Active Directory do Azure** clicando **todos os serviços** no topo do menu de navegação esquerdo principal.

8.  Escreva **"do Azure Active Directory**" na caixa de pesquisa de filtro e selecione o **do Azure Active Directory** item.

9.  Clique em **aplicações empresariais** no menu de navegação esquerdo do Azure Active Directory.

10. Clique em **todas as aplicações** para ver uma lista de todas as suas aplicações.

   * Se não vir a aplicação que pretende mostrar aqui, utilize o **filtro** controlo na parte superior a **lista de todas as aplicações** e defina o **mostrar** opção para **todos os Aplicações.**

11. Selecione a aplicação que pretende que a verificação do deeplink para.

12. Localize a etiqueta **URL de acesso de utilizador**. O deeplink deve corresponder a este URL.

## <a name="how-to-install-the-access-panel-browser-extension"></a>Como instalar a extensão de Browser do painel de acesso

Para instalar a extensão de Browser do painel de acesso, siga estes passos:

1.  Abra o [painel de acesso](https://myapps.microsoft.com) dos browsers suportados e início de sessão como um **utilizador** no seu Azure AD.

2.  Clique num **aplicação de palavra-passe SSO** no painel de acesso.

3.  Na linha de pedir instalar o software, selecione **instalar agora**.

4.  Com base no seu browser, que é direcionado para a hiperligação de transferência. **Adicionar** a extensão para o seu browser.

5.  Se o browser pede-lhe, selecione como **ativar** ou **permitir** a extensão.

6.  Uma vez instalado, **reiniciar** a sessão do browser.

7.  Inicie sessão no painel de acesso e veja se pode **iniciar** as aplicações de SSO de palavra-passe

Também pode transferir a extensão para Chrome e Firefox destas hiperligações diretas:

-   [Extensão de painel de acesso do Chrome](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Extensão de painel de acesso de Firefox](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Como configurar a palavra-passe-início de sessão único para uma aplicação de galeria do Azure AD

Para configurar uma aplicação na galeria do Azure AD, tem de:

-   [Adicionar uma aplicação na galeria do Azure AD](#add-an-application-from-the-Azure-AD-gallery)

-   [Configurar a aplicação para a palavra-passe-início de sessão único](#configure-the-application-for-password-single-sign-on)

### <a name="add-an-application-from-the-azure-ad-gallery"></a>Adicionar uma aplicação na galeria do Azure AD

Para adicionar uma aplicação na galeria do Azure AD, siga estes passos:

1.  Abra o [portal do Azure](https://portal.azure.com) e inicie sessão como um **Administrador Global** ou **coadministrador**.

2.  Abra o **extensão do Active Directory do Azure** clicando **todos os serviços** no topo do menu de navegação esquerdo principal.

3.  Escreva **"do Azure Active Directory**" na caixa de pesquisa de filtro e selecione o **do Azure Active Directory** item.

4.  Clique em **aplicações empresariais** no menu de navegação esquerdo do Azure Active Directory.

5.  Clique em de **adicionar** botão no canto superior direito no **aplicações empresariais** painel.

6.  No **introduza um nome** caixa de texto do **adicionar da galeria do** secção, escreva o nome da aplicação.

7.  Selecione a aplicação que pretende configurar para o início de sessão único.

8.  Antes de adicionar a aplicação, pode alterar o nome do **nome** caixa de texto.

9.  Para adicionar a aplicação, clique em **adicionar**.

Após um curto período de tempo, é capaz de ver o painel de configuração da aplicação.

### <a name="configure-the-application-for-password-single-sign-on"></a>Configurar a aplicação para a palavra-passe-início de sessão único

Para configurar o início de sessão para uma aplicação, siga estes passos:

1.  Abra o [ **portal do Azure** ](https://portal.azure.com/) e inicie sessão como um **Administrador Global** ou **Co-administrador.**

2.  Abra o **extensão do Active Directory do Azure** clicando **todos os serviços** no topo do menu de navegação esquerdo principal.

3.  Escreva **"do Azure Active Directory**" na caixa de pesquisa de filtro e selecione o **do Azure Active Directory** item.

4.  Clique em **aplicações empresariais** no menu de navegação esquerdo do Azure Active Directory.

5.  Clique em **todas as aplicações** para ver uma lista de todas as suas aplicações.

  * Se não vir a aplicação que pretende mostrar aqui, utilize o **filtro** controlo na parte superior a **lista de todas as aplicações** e defina o **mostrar** opção para **todos os Aplicações.**

6.  Selecione a aplicação que pretende configurar o início de sessão único.

7.  Quando carrega a aplicação, clique em de **de sessão único-** a partir do menu de navegação esquerdo da aplicação.

8.  Selecione o modo **baseada em palavra-passe de início de sessão.**

9.  [Atribuir utilizadores para a aplicação](#how-to-assign-a-user-to-an-application-directly).

10. Além disso, também pode fornecer as credenciais em nome do utilizador ao selecionar as linhas dos utilizadores e clicar no **credenciais de atualização** e introduzindo o nome de utilizador e palavra-passe em nome de utilizadores. Caso contrário, os utilizadores ser pedido que introduza as credenciais próprios após iniciar.

## <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a>Como configurar a palavra-passe-início de sessão único para uma aplicação não Galeria

Para configurar uma aplicação na galeria do Azure AD, tem de:

-   [Adicionar uma aplicação não Galeria](#add-a-non-gallery-application)

-   [Configurar a aplicação para a palavra-passe-início de sessão único](#configure-the-application-for-password-single-sign-on)

### <a name="add-a-non-gallery-application"></a>Adicionar uma aplicação não Galeria

Para adicionar uma aplicação na galeria do Azure AD, siga estes passos:

1.  Abra o [portal do Azure](https://portal.azure.com) e inicie sessão como um **Administrador Global** ou **coadministrador**.

2.  Abra o **extensão do Active Directory do Azure** clicando **todos os serviços** no topo do menu de navegação esquerdo principal.

3.  Escreva **"do Azure Active Directory**" na caixa de pesquisa de filtro e selecione o **do Azure Active Directory** item.

4.  Clique em **aplicações empresariais** no menu de navegação esquerdo do Azure Active Directory.

5.  Clique em de **adicionar** botão no canto superior direito no **aplicações empresariais** painel.

6.  Clique em **aplicação não galeria.**

7.  Introduza o nome da sua aplicação no **nome** caixa de texto. Selecione **adicionar.**

Após um curto período de tempo, é capaz de ver o painel de configuração da aplicação.

### <a name="configure-the-application-for-password-single-sign-on"></a>Configurar a aplicação para a palavra-passe-início de sessão único

Para configurar o início de sessão para uma aplicação, siga estes passos:

1.  Abra o [ **portal do Azure** ](https://portal.azure.com/) e inicie sessão como um **Administrador Global** ou **Co-administrador.**

2.  Abra o **extensão do Active Directory do Azure** clicando **todos os serviços** no topo do menu de navegação esquerdo principal.

3.  Escreva **"do Azure Active Directory**" na caixa de pesquisa de filtro e selecione o **do Azure Active Directory** item.

4.  Clique em **aplicações empresariais** no menu de navegação esquerdo do Azure Active Directory.

5.  Clique em **todas as aplicações** para ver uma lista de todas as suas aplicações.

    1.  Se não vir a aplicação que pretende mostrar aqui, utilize o **filtro** controlo na parte superior a **lista de todas as aplicações** e defina o **mostrar** opção para **todos os Aplicações.**

6.  Selecione a aplicação que pretende configurar o início de sessão único.

7.  Quando carrega a aplicação, clique em de **de sessão único-** a partir do menu de navegação esquerdo da aplicação.

8.  Selecione o modo **baseada em palavra-passe de início de sessão.**

9.  Introduza o **URL de início de sessão**, o URL onde os utilizadores introduzem o respetivo nome de utilizador e palavra-passe para iniciar sessão. Certifique-se de que os campos de início de sessão estão visíveis no URL.

10. Atribua utilizadores para a aplicação.

11. Além disso, também pode fornecer as credenciais em nome do utilizador ao selecionar as linhas dos utilizadores e clicar no **credenciais de atualização** e introduzindo o nome de utilizador e palavra-passe em nome de utilizadores. Caso contrário, os utilizadores ser pedido que introduza as credenciais próprios após iniciar.

## <a name="how-to-assign-a-user-to-an-application-directly"></a>Como atribuir diretamente um utilizador a uma aplicação

Para atribuir diretamente um ou mais utilizadores a uma aplicação, siga estes passos:

1.  Abra o [ **portal do Azure** ](https://portal.azure.com/) e inicie sessão como um **Administrador Global.**

2.  Abra o **extensão do Active Directory do Azure** clicando **todos os serviços** no topo do menu de navegação esquerdo principal.

3.  Escreva **"do Azure Active Directory**" na caixa de pesquisa de filtro e selecione o **do Azure Active Directory** item.

4.  Clique em **aplicações empresariais** no menu de navegação esquerdo do Azure Active Directory.

5.  Clique em **todas as aplicações** para ver uma lista de todas as suas aplicações.

  * Se não vir a aplicação que pretende mostrar aqui, utilize o **filtro** controlo na parte superior a **lista de todas as aplicações** e defina o **mostrar** opção para **todos os Aplicações.**

6.  Selecione a aplicação que pretende atribuir um utilizador a partir da lista.

7.  Quando carrega a aplicação, clique em **utilizadores e grupos** a partir do menu de navegação esquerdo da aplicação.

8.  Clique em de **adicionar** botão do **utilizadores e grupos** lista para abrir o **adicionar atribuição** painel.

9.  Clique em de **utilizadores e grupos** Seletor do **adicionar atribuição** painel.

10. Escreva o **nome completo** ou **endereço de correio eletrónico** do utilizador que está interessado atribuir para o **pesquisa por nome ou endereço de e-mail** caixa de pesquisa.

11. Coloque o cursor sobre o **utilizador** na lista de revela um **caixa de verificação**. Para adicionar o utilizador para o **selecionados** lista, clique na caixa de verificação junto a fotografia do perfil do utilizador ou logótipo.

12. **Opcional:** se gostaria de **adicionar mais do que um utilizador**, tipo noutra **nome completo** ou **endereço de correio eletrónico** para o **pesquisar por nome ou endereço de correio eletrónico** caixa de pesquisa e clique na caixa de verificação para adicionar este utilizador para o **selecionados** lista.

13. Quando tiver terminado de selecionar utilizadores, clique em de **selecione** botão para os adicionar à lista de utilizadores e grupos atribuídos à aplicação.

14. **Opcional:** clique o **selecionar função** Seletor no **adicionar atribuição** painel para selecionar uma função para atribuir aos utilizadores que selecionou.

15. Clique em de **atribuir** botão para atribuir a aplicação para os utilizadores selecionados.

Após um curto período de tempo, os utilizadores que selecionou conseguir iniciar estas aplicações no painel de acesso.

## <a name="if-these-troubleshooting-steps-do-not-the-resolve-the-issue"></a>Se estes passos de resolução de problemas não não a resolver o problema. 

Abra um pedido de suporte com as seguintes informações se disponíveis:

-   ID de correlação de erro

-   UPN (endereço de e-mail do utilizador)

-   TenantID

-   Tipo de browser

-   Fuso horário e tempo/período de tempo durante o erro ocorre

-   Rastreios de fiddler

## <a name="next-steps"></a>Passos Seguintes
[Fornecer início de sessão único às suas aplicações com o Proxy da aplicação](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md)
