---
title: 'Tutorial: Integração do Active Directory do Azure com o da Palo Alto Networks - Portal cativas | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e da Palo Alto Networks - Portal cativas.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 67a0b476-2305-4157-8658-2ec3625850d5
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/25/2018
ms.author: jeedes
ms.openlocfilehash: eff08cc17f475e2b6ad6406e463de27371bbe5b1
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/07/2019
ms.locfileid: "54064734"
---
# <a name="tutorial-azure-active-directory-integration-with-palo-alto-networks---captive-portal"></a>Tutorial: Integração do Active Directory do Azure com o da Palo Alto Networks - Portal cativas

Neste tutorial, saiba como integrar da Palo Alto Networks - cativas Portal com o Azure Active Directory (Azure AD).
Integração da Palo Alto Networks - cativas Portal com o Azure AD fornece as seguintes vantagens:

* Pode controlar no Azure AD que tenha acesso da Palo Alto Networks - Portal cativas.
* Pode permitir que os utilizadores ser automaticamente sessão iniciada para Palo Alto Networks - Portal cativas (Single Sign-On) com as suas contas do Azure AD.
* Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com da Palo Alto Networks - Portal cativas, terá dos seguintes itens:

* Uma subscrição do Azure AD. Se não tiver um ambiente do Azure AD, pode obter a versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/)
* Subscrição de ativado da Palo Alto Networks - Portal cativas início de sessão único

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, configure e teste do Azure AD início de sessão único num ambiente de teste.

* Da Palo Alto Networks - Portal cativas suporta **IDP** iniciada SSO

* Da Palo Alto Networks - Portal cativas suporta **Just In Time** aprovisionamento de utilizadores

## <a name="adding-palo-alto-networks---captive-portal-from-the-gallery"></a>Adição da Palo Alto Networks - cativas Portal a partir da Galeria

Para configurar a integração da Palo Alto Networks - cativas Portal com o Azure AD, terá de adicionar da Palo Alto Networks - Portal cativas partir da galeria, à sua lista de aplicações de SaaS geridas.

**Para adicionar da Palo Alto Networks - cativas Portal a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![O botão do Azure Active Directory](common/select-azuread.png)

2. Navegue para **aplicações empresariais** e, em seguida, selecione a **todos os aplicativos** opção.

    ![O painel de aplicações empresariais](common/enterprise-applications.png)

3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo](common/add-new-app.png)

4. Na caixa de pesquisa, escreva **Palo Alto Networks - Portal cativas**, selecione **Palo Alto Networks - Portal cativas** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação .

     ![Da Palo Alto Networks - Portal cativas na lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configurar e testar o Azure AD início de sessão único da Palo Alto Networks - Portal cativas com base num utilizador de teste **Eduarda Almeida**.
Para o início de sessão único para o trabalho, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado da Palo Alto Networks - Portal cativas deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único da Palo Alto Networks - Portal cativas, precisa concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Configurar da Palo Alto Networks - cativas Portal início de sessão único](#configure-palo-alto-networks---captive-portal-single-sign-on)**  - para configurar as definições de início de sessão único no lado do aplicativo.
3. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Criar da Palo Alto Networks - utilizador de teste de Portal cativas](#create-palo-alto-networks---captive-portal-test-user)**  - para ter um equivalente da Eduarda Almeida na Palo Alto Networks - Portal cativas, que está ligada à representação de utilizador do Azure AD.
6. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, vai ativar o Azure AD início de sessão único no portal do Azure.

Para configurar o Azure AD início de sessão único da Palo Alto Networks - Portal cativas, execute os seguintes passos:

1. Na [portal do Azure](https://portal.azure.com/), na **Palo Alto Networks - Portal cativas** página de integração de aplicações, selecione **início de sessão único**.

    ![Configurar a ligação de início de sessão única](common/select-sso.png)

2. Sobre o **selecionar um método de início de sessão único** caixa de diálogo, selecione **SAML/WS-Fed** modo para ativar o início de sessão único.

    ![Único início de sessão em modo de seleção](common/select-saml-option.png)

3. Sobre o **definir a segurança de início de sessão único com o SAML** página, clique em **editar** ícone para abrir **configuração básica de SAML** caixa de diálogo.

    ![Editar a configuração SAML do básico](common/edit-urls.png)

4. Sobre o **configuração básica de SAML** caixa de diálogo, execute os seguintes passos:

    ![Da Palo Alto Networks - cativas de Portal de domínio e URLs únicas início de sessão em informações](common/idp-intiated.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<Customer Firewall Hostname>/SAML20/SP`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<Customer Firewall Hostname>/SAML20/SP/ACS`

    > [!NOTE]
    > Estes valores não são reais. Atualize estes valores com o identificador real e o URL de resposta. Contacte [Palo Alto Networks - equipa de suporte de cliente de Portal cativas](https://support.paloaltonetworks.com/support) obter esses valores. Também pode consultar os padrões mostrados a **configuração básica de SAML** secção no portal do Azure.

5. No **definido no início de sessão único com o SAML** página, além do **certificado de assinatura SAML** secção, clique em **transferir** para transferir o **XML de metadados de Federação**  entre as opções de determinado de acordo com seus requisitos e guarde-o no seu computador.

    ![O link de download de certificado](common/metadataxml.png)

### <a name="configure-palo-alto-networks---captive-portal-single-sign-on"></a>Configurar da Palo Alto Networks - cativas Portal início de sessão único

1. Abra o site da Palo Alto como um administrador na outra janela do browser.

2. Clique em **dispositivo**.

    ![Configurar da Palo Alto início de sessão único](./media/paloaltonetworks-captiveportal-tutorial/tutorial_paloaltoadmin_admin1.png)

3. Selecione **fornecedor de identidade de SAML** no painel de navegação esquerda da barra e clique em "Importar" para importar o ficheiro de metadados.

    ![Configurar da Palo Alto início de sessão único](./media/paloaltonetworks-captiveportal-tutorial/tutorial_paloaltoadmin_admin2.png)

4. Executar as ações na janela de importação seguintes

    ![Configurar da Palo Alto início de sessão único](./media/paloaltonetworks-captiveportal-tutorial/tutorial_paloaltoadmin_admin3.png)

    a. No **nome do perfil** caixa de texto, forneça um por exemplo, de nome da interface do Usuário do administrador do Azure AD.
    
    b. Na **metadados de fornecedor de identidade**, clique em **procurar** e selecione o arquivo METADATA XML que transferiu a partir do portal do Azure
    
    c. Clique em **OK**

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD 

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

1. No portal do Azure, no painel esquerdo, selecione **do Azure Active Directory**, selecione **utilizadores**e, em seguida, selecione **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](common/users.png)

2. Selecione **novo utilizador** na parte superior do ecrã.

    ![Novo utilizador botão](common/new-user.png)

3. Nas propriedades do utilizador, execute os seguintes passos.

    ![A caixa de diálogo de utilizador](common/user-properties.png)

    a. Na **Name** campo introduza **BrittaSimon**.
  
    b. Na **nome de utilizador** tipo de campo **brittasimon@yourcompanydomain.extension**  
    Por exemplo, BrittaSimon@contoso.com

    c. Selecione **palavra-passe de Show** caixa de verificação e, em seguida, anote o valor que é apresentado na caixa de palavra-passe.

    d. Clique em **Criar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso para Palo Alto Networks - Portal cativas.

1. No portal do Azure, selecione **aplicações empresariais**, selecione **todos os aplicativos**, em seguida, selecione **da Palo Alto Networks - Portal cativas**.

    ![Painel de aplicações empresariais](common/enterprise-applications.png)

2. Na lista de aplicativos, escreva e selecione **Palo Alto Networks - Portal cativas**.

    ![Da Palo Alto Networks - ligação de Portal cativas na lista de aplicações](common/all-applications.png)

3. No menu à esquerda, selecione **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"](common/users-groups-blade.png)

4. Clique nas **adicionar utilizador** botão, em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição](common/add-assign-user.png)

5. Na **utilizadores e grupos** caixa de diálogo select **Eduarda Almeida** na lista de utilizadores, em seguida, clique o **selecionar** na parte inferior do ecrã.

6. Se está esperando a qualquer valor de função a asserção de SAML, em seguida, no **selecionar função** caixa de diálogo selecione a função adequada para o utilizador na lista, em seguida, clique o **selecione** na parte inferior do ecrã.

7. Na **adicionar atribuição** clique da caixa de diálogo a **atribuir** botão.

### <a name="create-palo-alto-networks---captive-portal-test-user"></a>Criar da Palo Alto Networks - utilizador de teste de Portal cativas

Nesta secção, um usuário chamado Eduarda Almeida é criado da Palo Alto Networks - Portal cativas. Da Palo Alto Networks - Portal cativas suporta **aprovisionamento de utilizadores de just-in-time**, que está ativado por predefinição. Não existe nenhum item de ação para nesta secção. Se um utilizador já não existir da Palo Alto Networks - Portal cativas, é criado um novo após a autenticação.

> [!NOTE]
> Se precisar de criar manualmente um utilizador, terá de contactar o [Palo Alto Networks - equipa de suporte de cliente de Portal cativas](https://support.paloaltonetworks.com/support).

### <a name="test-single-sign-on"></a>Testar o início de sessão único 

Portal cativas está configurado por trás do firewall na VM do Windows. Testar o início de sessão único no Portal cativas, início de sessão na VM do Windows com o protocolo RDP. A partir na sessão do RDP, abra um browser para qualquer web site, deverá abrir automaticamente o url SSO e a linha de comandos para a autenticação. Quando Authenticaiton estiver concluída, deve conseguir navgiate sites da web.

## <a name="additional-resources"></a>Recursos Adicionais

- [ Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [O que é o acesso condicional no Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

