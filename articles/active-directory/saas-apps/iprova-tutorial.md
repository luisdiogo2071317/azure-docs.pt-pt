---
title: 'Tutorial: Integração do Active Directory do Azure com iProva | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e iProva.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 1eaeef9b-4479-4a9f-b1b2-bc13b857c75c
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/24/2018
ms.author: jeedes
ms.openlocfilehash: eb64fd7a7270c3bae03a94385fc77581637ac675
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/02/2019
ms.locfileid: "53978086"
---
# <a name="tutorial-azure-active-directory-integration-with-iprova"></a>Tutorial: Integração do Active Directory do Azure com iProva

Neste tutorial, saiba como integrar iProva com o Azure Active Directory (Azure AD).
Integrar iProva no Azure AD fornece as seguintes vantagens:

* Pode controlar no Azure AD que tenha acesso ao iProva.
* Pode permitir que os utilizadores ser automaticamente sessão iniciada para iProva (Single Sign-On) com as suas contas do Azure AD.
* Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com iProva, terá dos seguintes itens:

* Uma subscrição do Azure AD. Se não tiver um ambiente do Azure AD, pode obter a versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/)
* iProva logon único habilitado subscrição

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, configure e teste do Azure AD início de sessão único num ambiente de teste.

* suporta iProva **SP** iniciada SSO

## <a name="adding-iprova-from-the-gallery"></a>Adicionando iProva da Galeria

Para configurar a integração do iProva com o Azure AD, terá de adicionar iProva a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar iProva a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![O botão do Azure Active Directory](common/select-azuread.png)

2. Navegue para **aplicações empresariais** e, em seguida, selecione a **todos os aplicativos** opção.

    ![O painel de aplicações empresariais](common/enterprise-applications.png)

3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo](common/add-new-app.png)

4. Na caixa de pesquisa, escreva **iProva**, selecione **iProva** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

     ![iProva na lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com iProva com base num utilizador de teste **Eduarda Almeida**.
Para o início de sessão único funcionar, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no iProva deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com iProva, tem de concluir os seguintes blocos de construção:

1. **[Obter informações de configuração de iProva](#retrieve-configuration-information-from-iprova)**  – como uma preparação para os passos seguintes.
2. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
3. **[Configurar iProva início de sessão único](#configure-iprova-single-sign-on)**  - para configurar as definições de início de sessão único no lado do aplicativo.
4. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
5. **[Criar utilizador de teste iProva](#create-iprova-test-user)**  - para ter um equivalente da Eduarda Almeida na iProva que está ligado à representação de utilizador do Azure AD.
6. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
7. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="retrieve-configuration-information-from-iprova"></a>Obter informações de configuração de iProva

Nesta secção, irá obter algumas informações essenciais de iProva.
Estas informações são necessárias para configurar o Azure AD início de sessão único

1. Abra um browser e navegue para o **página de informações de SAML2** iProva, a utilizar o padrão de URL seguinte:

    | | |
    |-|-|
    | `https://SUBDOMAIN.iprova.nl/saml2info`|
    | `https://SUBDOMAIN.iprova.be/saml2info`|
    | | |

     ![Ver a página de informações de iProva SAML2](media/iprova-tutorial/iprova-saml2-info.png)

2. Deixe o separador do browser aberta enquanto continuar com os passos seguintes no outro separador do browser.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, vai ativar o Azure AD início de sessão único no portal do Azure.

Para configurar o Azure AD início de sessão único com iProva, execute os seguintes passos:

1. Na [portal do Azure](https://portal.azure.com/), na **iProva** página de integração de aplicações, selecione **início de sessão único**.

    ![Configurar a ligação de início de sessão única](common/select-sso.png)

2. Sobre o **selecionar um método de início de sessão único** caixa de diálogo, selecione **SAML/WS-Fed** modo para ativar o início de sessão único.

    ![Único início de sessão em modo de seleção](common/select-saml-option.png)

3. Sobre o **definir a segurança de início de sessão único com o SAML** página, clique em **editar** ícone para abrir **configuração básica de SAML** caixa de diálogo.

    ![Editar a configuração SAML do básico](common/edit-urls.png)

4. Sobre o **configuração básica de SAML** secção, execute os seguintes passos:

    a. Preencher o **identificador** campo com o valor que é apresentado por trás da etiqueta **EntityID** no **página de informações de iProva SAML2** (ainda abrir no seu outro separador do browser).

    b. Preencher o **URL de resposta** campo com o valor que é apresentado por trás da etiqueta **URL de resposta** no **página de informações de iProva SAML2** (ainda abrir no seu outro separador do browser).

    c. Preencher o **URL de início de sessão** campo com o valor que é apresentado por trás da etiqueta **URL de início de sessão** no **página de informações de iProva SAML2** (ainda abrir no seu outro separador do browser).

    ![iProva domínio e URLs únicas início de sessão em informações](common/sp-identifier-reply.png)

5. aplicação de iProva espera que as asserções SAML num formato específico. Configure as seguintes declarações para esta aplicação. Pode gerir os valores destes atributos do **atributos de utilizador** secção na página de integração de aplicações. Sobre o **definido no início de sessão único com o SAML** página, clique em **editar** botão para abrir **atributos de utilizador** caixa de diálogo.

    ![image](common/edit-attribute.png)

6. No **afirmações de utilizador** secção sobre o **atributos de utilizador** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem acima e execute os seguintes passos:

    | Nome | Atributo de origem| Espaço de nomes |
    | ---------------| -------- | -----|
    | `samaccountname` | `user.onpremisessamaccountname`| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims`|
    | | |

    a. Clique em **Adicionar nova afirmação** para abrir o **afirmações de utilizador de gerir** caixa de diálogo.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Partir do **espaço de nomes** lista, digite o valor de espaço de nomes mostrado para essa linha.

    d. Selecione a origem de dado **atributo**.

    e. Partir do **atributo de origem** lista, digite o valor de atributo apresentado para essa linha.

    f. Clique em **Ok**

    g. Clique em **Guardar**.

7. No **definir a segurança de início de sessão único com o SAML** na página a **certificado de assinatura SAML** secção, clique em botão Copiar para copiar **Url de metadados de Federação de aplicação** e guarde-o no seu computador.

    ![O link de download de certificado](common/copy-metadataurl.png)

### <a name="configure-iprova-single-sign-on"></a>Configurar iProva início de sessão único

1. Iniciar sessão no iProva com o **administrador** conta.

2. Abra o **vá para** menu.

3. Clique em **gestão de aplicações**.

4. Clique em **gerais** no **definições do sistema** painel.

5. Clique em **editar**.

6. Desloque para baixo até **controlo de acesso**.

    ![definições de controlo de acesso de iProva](media/iprova-tutorial/iprova-accesscontrol.png)

7. Encontrar a definição **os usuários fazem logon automaticamente com as suas contas de rede**e altere-o para **Sim, autenticação através de SAML**. Opções adicionais aparecerão agora.

8. Clique nas **configuração** botão.

9. Clique nas **seguinte** botão.

10. iProva agora perguntará se deseja transferir dados de Federação a partir de uma URL, ou carregá-lo a partir de um ficheiro. Selecione o **partir do URL** opção.

    ![Transferir os metadados do Azure AD](media/iprova-tutorial/iprova-download-metadata.png)

11. Agora cole o URL de metadados que guardou no último passo dos **configurar o Azure AD início de sessão único** capítulo.

12. Clique no botão em forma de seta para transferir os metadados do Azure AD.

13. Quando a transferência estiver concluída, a mensagem de confirmação **ficheiro de dados de federação válido transferido** aparecerá.

14. Clique nas **seguinte** botão.

15. Ignorar a **início de sessão de teste** opção por agora e clique nas **próxima** botão.

16. Na lista pendente com o nome **afirmam usar**, selecione **windowsaccountname**.

17. Clique no botão **Concluir**.

18. Agora retornar para o **editar as definições gerais** ecrã. **Desloque para baixo** na parte inferior da página e clique nas **OK** botão para guardar a configuração.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

1. No portal do Azure, no painel esquerdo, selecione **do Azure Active Directory**, selecione **utilizadores**e, em seguida, selecione **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](common/users.png)

2. Selecione **novo utilizador** na parte superior do ecrã.

    ![Novo utilizador botão](common/new-user.png)

3. Nas propriedades do utilizador, execute os seguintes passos.

    ![A caixa de diálogo de utilizador](common/user-properties.png)

    a. Na **Name** campo introduza o nome, como **BrittaSimon**.
  
    b. Na **nome de utilizador** tipo de campo **yourname@yourcompanydomain.extension**  
    Por exemplo, BrittaSimon@contoso.com

    c. Selecione **palavra-passe de Show** caixa de verificação e, em seguida, anote o valor que é apresentado na caixa de palavra-passe.

    d. Clique em **Criar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso a iProva.

1. No portal do Azure, selecione **aplicações empresariais**, selecione **todos os aplicativos**, em seguida, selecione **iProva**.

    ![Painel de aplicações empresariais](common/enterprise-applications.png)

2. Na lista de aplicações, selecione **iProva**.

    ![A ligação de iProva na lista de aplicações](common/all-applications.png)

3. No menu à esquerda, selecione **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"](common/users-groups-blade.png)

4. Clique nas **adicionar utilizador** botão, em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição](common/add-assign-user.png)

5. Na **utilizadores e grupos** caixa de diálogo select **Eduarda Almeida** na lista de utilizadores, em seguida, clique o **selecionar** na parte inferior do ecrã.

6. Se está esperando a qualquer valor de função a asserção de SAML, em seguida, no **selecionar função** caixa de diálogo selecione a função adequada para o utilizador na lista, em seguida, clique o **selecione** na parte inferior do ecrã.

7. Na **adicionar atribuição** clique da caixa de diálogo a **atribuir** botão.

### <a name="create-iprova-test-user"></a>Criar utilizador de teste iProva

1. Iniciar sessão no iProva com o **administrador** conta.

2. Abra o **vá para** menu.

3. Clique em **gestão de aplicações**.

4. Clique em **usuários** no **utilizadores e grupos de utilizadores** painel.

5. Clique no botão **Adicionar**.

6. Na **nome de utilizador** tipo de campo **brittasimon@yourcompanydomain.extension**  
    Por exemplo, BrittaSimon@contoso.com

7. Na **FullName** campo introduza o nome completo como **BrittaSimon**.

8. Selecione o **nenhuma palavra-passe (utilização início de sessão único)** opção.

9. Na **endereço de correio eletrónico** tipo de campo **yourname@yourcompanydomain.extension** por exemplo, BrittaSimon@contoso.com

10. Desloque para baixo até o final da página e clique nas **concluir** botão.

### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico iProva no painel de acesso, deve ser automaticamente sessão iniciada no iProva para o qual configura o SSO. Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos Adicionais

- [ Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [O que é o acesso condicional no Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [iProva - como configurar SAML2 início de sessão único](https://webshare.iprova.nl/0wqwm45yn09f5poh/Document.aspx)