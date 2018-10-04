---
title: 'Tutorial: Integração do Azure Active Directory com o Zendesk | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o Zendesk.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 9d7c91e5-78f5-4016-862f-0f3242b00680
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2018
ms.author: jeedes
ms.openlocfilehash: 37d20eabfc8fb883cda346dc8b55a17b8b959218
ms.sourcegitcommit: f58fc4748053a50c34a56314cf99ec56f33fd616
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48268179"
---
# <a name="tutorial-azure-active-directory-integration-with-zendesk"></a>Tutorial: Integração do Azure Active Directory com o Zendesk

Neste tutorial, saiba como integrar o Zendesk com o Azure Active Directory (Azure AD).

Integração do Zendesk com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Zendesk.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada ao Zendesk (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Zendesk, terá dos seguintes itens:

- Uma subscrição do Azure
- Um Zendesk logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste.
O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar Zendesk a partir da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-zendesk-from-the-gallery"></a>Adicionar Zendesk a partir da Galeria

Para configurar a integração do Zendesk com o Azure AD, terá de adicionar Zendesk a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Zendesk a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![image](./media/zendesk-tutorial/selectazuread.png)

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![image](./media/zendesk-tutorial/a_select_app.png)
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![image](./media/zendesk-tutorial/a_new_app.png)

4. Na caixa de pesquisa, escreva **Zendesk**, selecione **Zendesk** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

     ![image](./media/zendesk-tutorial/a_add_app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Zendesk com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador equivalente no Zendesk para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Zendesk deve ser estabelecido.

No Zendesk, atribuir o valor do **nome de utilizador** no Azure AD como o valor da **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o Zendesk, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste da Zendesk](#create-a-zendesk-test-user)**  - para ter um equivalente da Eduarda Almeida no Zendesk, que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação do Zendesk.

**Para configurar o Azure AD início de sessão único com o Zendesk, execute os seguintes passos:**

1. Na [portal do Azure](https://portal.azure.com/), na **Zendesk** página de integração de aplicações, selecione **início de sessão único**.

    ![image](./media/zendesk-tutorial/b1_b2_select_sso.png)

2. Clique em **modo de início de sessão único de alteração** na parte superior do ecrã para selecionar o **SAML** modo.

      ![image](./media/zendesk-tutorial/b1_b2_saml_ssso.png)

3. Sobre o **selecionar um método de início de sessão único** caixa de diálogo, clique em **selecione** para **SAML** modo para ativar o início de sessão único.

    ![image](./media/zendesk-tutorial/b1_b2_saml_sso.png)

4. Sobre o **definido no início de sessão único com o SAML** página, clique em **editar** botão para abrir **configuração básica de SAML** caixa de diálogo.

    ![image](./media/zendesk-tutorial/b1-domains_and_urlsedit.png)

5. Sobre o **configuração básica de SAML** secção, execute os seguintes passos:

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.zendesk.com`.

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `<subdomain>.zendesk.com`.

    ![image](./media/zendesk-tutorial/b1-domains_and_urls.png)

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte de cliente do Zendesk](https://support.zendesk.com/hc/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise) obter esses valores.

6. Zendesk espera que as asserções SAML num formato específico. Não há nenhum atributo SAML obrigatório, mas, opcionalmente, pode adicionar um atributo de **atributos de utilizador** secção na página de integração de aplicações. Sobre o **definido no início de sessão único com o SAML** página, clique em **editar** botão para abrir **atributos de utilizador** caixa de diálogo.

    ![image](./media/zendesk-tutorial/i4-attribute.png)

7. No **afirmações de utilizador** secção sobre o **atributos de utilizador** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem acima e execute os seguintes passos:

    a. Clique em **Adicionar nova afirmação** para abrir o **afirmações de utilizador de gerir** caixa de diálogo.

    ![image](./media/zendesk-tutorial/i2-attribute.png)

    ![image](./media/zendesk-tutorial/i3-attribute.png)
    
    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Deixe o **espaço de nomes** em branco.

    d. Selecione a origem de dado **atributo**.
    
    e. Partir do **atributo de origem** lista, digite o valor de atributo apresentado para essa linha.
    
    f. Clique em **Ok**

    g. Clique em **Guardar**.

    > [!NOTE]
    > Utilizar atributos de extensão para adicionar os atributos que não estão no Azure AD por predefinição. Clique em [atributos de utilizador que podem ser definidos no SAML](https://support.zendesk.com/hc/en-us/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise-) para obter a lista completa de atributos SAML que **Zendesk** aceita.

8. O SAML assinatura na secção de certificado, o **certificado de assinatura SAML** secção, copie a **Thumbprint**e guarde-o no seu computador.

    ![image](./media/zendesk-tutorial/C3_certificate.png)

    a. Selecione a opção adequada para **assinatura opção** se for necessário.

    b. Selecione a opção adequada para **algoritmo de assinatura** se for necessário.

    c. Clicar em **Guardar**

9. Sobre o **configurar Zendesk** secção, clique em **ver instruções passo a passo** para abrir **configurar início de sessão** janela. Copiar o abaixo URLs, do **secção de referência rápida.**

    Tenha em atenção que o url pode dizer o seguinte:

    a. URL do serviço de início de início de sessão único SAML

    b. ID de entidade

    c. URL de fim de sessão

    ![image](./media/zendesk-tutorial/d1_saml.png) 

10. Existem duas de formas em que pode ser configurado Zendesk - automática e Manual.
  
11. Para automatizar a configuração no Zendesk, tem de instalar **segura de aplicações meu início de sessão da extensão de browser** ao clicar em **instalar a extensão**.

    ![image](./media/zendesk-tutorial/install_extension.png)

12. Depois de adicionar a extensão para o navegador, clique em **configuração do Zendesk** irá direcioná-lo para a aplicação do Zendesk. A partir daí, forneça as credenciais de administrador para iniciar sessão no Zendesk. A extensão do browser irá configurar o aplicativo para e automatizar o passo 13 automaticamente.

     ![image](./media/zendesk-tutorial/d2_saml.png) 

13. Se desejar configurar manualmente o Zendesk, abra uma nova janela de browser e o registo no site da sua empresa Zendesk como administrador e execute os seguintes passos:

    * Clique em **administrador**.

    * No painel de navegação esquerdo, clique em **configurações**e, em seguida, clique em **segurança**.

    * Sobre o **segurança** página, execute os seguintes passos:

      ![Segurança](././media/zendesk-tutorial/ic773089.png "segurança")

      ![Início de sessão único](././media/zendesk-tutorial/ic773090.png "início de sessão único")

      a. Clique nas **agentes e administrador** separador.

      b. Selecione **início de sessão único (SSO) e SAML**e, em seguida, selecione **SAML**.

      c. Na **URL de SSO SAML** caixa de texto, cole o valor de **SAML único início de sessão no URL do serviço** que copiou do portal do Azure.

      d. Na **URL de fim de sessão remoto** caixa de texto, cole o valor de **URL de fim de sessão** que copiou do portal do Azure.

      e. Na **impressão digital de certificado** caixa de texto, colar a **Thumbprint** valor do certificado que copiou do portal do Azure.

      f. Clique em **Guardar**.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD 

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

1. No portal do Azure, no painel esquerdo, selecione **do Azure Active Directory**, selecione **utilizadores**e, em seguida, selecione **todos os utilizadores**.

    ![image](./media/zendesk-tutorial/d_users_and_groups.png)

2. Selecione **novo utilizador** na parte superior do ecrã.

    ![image](./media/zendesk-tutorial/d_adduser.png)

3. Nas propriedades do utilizador, execute os seguintes passos.

    ![image](./media/zendesk-tutorial/d_userproperties.png)

    a. Na **Name** campo introduza **BrittaSimon**.
  
    b. Na **nome de utilizador** tipo de campo **brittasimon@yourcompanydomain.extension**  
    Por exemplo, BrittaSimon@contoso.com

    c. Selecione **propriedades**, selecione a **palavra-passe de Show** caixa de verificação e, em seguida, anote o valor que é apresentado na caixa de palavra-passe.

    d. Selecione **Criar**.

### <a name="create-a-zendesk-test-user"></a>Criar um utilizador de teste do Zendesk

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no Zendesk. Zendesk suporta o aprovisionamento automático de utilizadores, que está por predefinição, ativada. Pode encontrar mais detalhes [aqui](Zendesk-provisioning-tutorial.md) sobre como configurar o aprovisionamento automático de utilizadores.

**Se precisar de criar manualmente o utilizador, execute, os seguintes passos:**

> [!NOTE]
> **Utilizador final** contas são aprovisionadas automaticamente ao iniciar sessão. **Agente** e **administrador** contas têm de ser provisionado manualmente no **Zendesk** antes de iniciar sessão.

1. Inicie sessão no seu **Zendesk** inquilino.

2. Selecione o **lista de clientes** separador.

3. Selecione o **usuário** separador e clique em **Add**.

    ![Adicionar utilizador](././media/zendesk-tutorial/ic773632.png "adicionar utilizador")
4. Tipo de **Name** e **E-Mail** de uma conta do Azure AD existente que pretende aprovisionar e, em seguida, clique em **guardar**.

    ![Novo utilizador](././media/zendesk-tutorial/ic773633.png "novo utilizador")

> [!NOTE]
> Pode utilizar quaisquer outras Zendesk utilizador conta criação ferramentas ou APIs fornecidas pelo Zendesk para aprovisionar contas de utilizador do AAD.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso ao Zendesk.

1. No portal do Azure, selecione **aplicações empresariais**, selecione **todos os aplicativos**.

    ![image](./media/zendesk-tutorial/d_all_applications.png)

2. Na lista de aplicações, selecione **Zendesk**.

    ![image](./media/zendesk-tutorial/d_all_zendeskapplications.png)

3. No menu à esquerda, selecione **utilizadores e grupos**.

    ![image](./media/zendesk-tutorial/d_leftpaneusers.png)

4. Selecione o **Add** botão, em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![image](./media/zendesk-tutorial/d_assign_user.png)

4. Na **utilizadores e grupos** caixa de diálogo select **Eduarda Almeida** na lista de utilizadores, em seguida, clique o **selecionar** na parte inferior do ecrã.

5. Na **adicionar atribuição** caixa de diálogo select a **atribuir** botão.

### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico do Zendesk no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação do Zendesk.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)
* [Configurar o aprovisionamento do utilizador](zendesk-provisioning-tutorial.md)
