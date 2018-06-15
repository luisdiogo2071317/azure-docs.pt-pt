---
title: 'Tutorial: Integração do Azure Active Directory com redes da Palo Alto - Aperture | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e redes da Palo Alto - Aperture.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a5ea18d3-3aaf-4bc6-957c-783e9371d0f1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2018
ms.author: jeedes
ms.openlocfilehash: 29b37b34271680cdd4d1f660ea6eb6172df8b06d
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
ms.locfileid: "34347198"
---
# <a name="tutorial-azure-active-directory-integration-with-palo-alto-networks---aperture"></a>Tutorial: Integração do Azure Active Directory com redes da Palo Alto - Aperture

Neste tutorial, irá aprender a integrar redes da Palo Alto - Aperture com o Azure Active Directory (Azure AD).

Integração da Palo Alto redes - Aperture com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD quem tem acesso às redes da Palo Alto - Aperture.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para redes da Palo Alto - Aperture (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com redes da Palo Alto - Aperture, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Da Palo Alto redes - Aperture-início de sessão único ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. A adição da Palo Alto redes - Aperture a partir da Galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-palo-alto-networks---aperture-from-the-gallery"></a>A adição da Palo Alto redes - Aperture a partir da Galeria
Para configurar a integração das redes da Palo Alto - Aperture com o Azure AD, terá de adicionar redes da Palo Alto - Aperture na Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar redes da Palo Alto - Aperture da galeria, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **redes da Palo Alto - Aperture**, selecione **redes da Palo Alto - Aperture** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![Redes da Palo Alto - Aperture na lista de resultados](./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com redes da Palo Alto - Aperture com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo nas redes da Palo Alto - Aperture é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado nas redes da Palo Alto - Aperture tem de ser estabelecida.

Para configurar e testar o Azure AD-início de sessão único com redes da Palo Alto - Aperture, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um redes da Palo Alto - utilizador de teste de Aperture](#create-a-palo-alto-networks---aperture-test-user)**  - para ter um homólogo de Britta Simon nas redes da Palo Alto - Aperture que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único nas suas redes da Palo Alto - aplicação Aperture.

**Para configurar o Azure AD-início de sessão único com redes da Palo Alto - Aperture, execute os seguintes passos:**

1. No portal do Azure, no **redes da Palo Alto - Aperture** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_samlbase.png)

3. No **da Palo Alto redes - domínio Aperture e URLs** secção, execute os seguintes passos, se pretender configurar a aplicação no **IDP** iniciada modo:

    ![Redes da Palo Alto - Aperture domínio e os URLs únicos de informações de início de sessão](./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_url.png)

    a. No **identificador** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://<subdomain>.aperture.paloaltonetworks.com/d/users/saml/metadata`

    b. No **URL de resposta** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://<subdomain>.aperture.paloaltonetworks.com/d/users/saml/auth`

4. Verifique **Mostrar avançadas definições de URL** e executar o passo seguinte, se pretender configurar a aplicação no **SP** iniciada modo:

    ![Redes da Palo Alto - Aperture domínio e os URLs únicos de informações de início de sessão](./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_url1.png)

    No **URL de início de sessão** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://<subdomain>.aperture.paloaltonetworks.com/d/users/saml/sign_in`
     
    > [!NOTE] 
    > Estes valores não estiverem reais. Atualize estes valores com o identificador de real, a URL de resposta e o URL de início de sessão. Contacte [redes da Palo Alto - equipa de suporte de cliente Aperture](https://live.paloaltonetworks.com/t5/custom/page/page-id/Support) para obter estes valores. 

5. No **certificado de assinatura de SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![A hiperligação de transferência do certificado](./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_certificate.png) 

6. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_general_400.png)


7. No **redes da Palo Alto - configuração Aperture** secção, clique em **configurar redes da Palo Alto - Aperture** para abrir **configurar início de sessão** janela. Copiar o **ID de entidade de SAML e o único início de sessão no URL do serviço SAML** do **secção de referência rápida.**

    ![A ligação de configurar](./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_configure.png)

8. Numa janela do browser web diferente, no início de sessão da Palo Alto redes - Aperture como administrador.

9. Na barra de menu superior, clique em **definições**.

    ![O separador de definições](./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_settings.png)

10. Navegue para **aplicação** secção clique **autenticação** formulário no lado esquerdo do menu.

    ![O separador de autenticação](./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_auth.png)
    
11. No **autenticação** página execute os seguintes passos:
    
    ![O separador de autenticação](./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_singlesignon.png)

    a. Verifique o **ativar o único início de sessão-On(Supported SSP Providers are Okta, Onelogin)** de **Single Sign-On** campo.

    b. No **ID do fornecedor de identidade** caixa de texto, cole o valor de **ID de entidade de SAML**, que copiou do portal do Azure.

    c. Clique em **Escolher ficheiro** para carregar o certificado transferido do Azure AD no **certificado do fornecedor de identidade** campo.

    d. No **URL de SSO de fornecedor de identidade** caixa de texto, cole o valor de **único início de sessão no URL do serviço SAML**, que copiou do portal do Azure.

    e. Reveja as informações de IdP do **Aperture informações** secção e transferir o certificado do **Aperture chave** campo.

    f. Clique em **Guardar**.

> [!TIP]
> Pode agora ler estas instruções dentro de uma versão concisa o [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir do **do Active Directory > aplicações da empresa** secção, basta clicar no **Single Sign-On** separador e aceder à documentação do embedded através de **configuração** secção na parte inferior. Pode ler mais sobre a funcionalidade de documentação incorporados aqui: [do Azure AD incorporado documentação]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/active-directory-saas-paloaltonetworks-aperture-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/active-directory-saas-paloaltonetworks-aperture-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/active-directory-saas-paloaltonetworks-aperture-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/active-directory-saas-paloaltonetworks-aperture-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-palo-alto-networks---aperture-test-user"></a>Criar um redes da Palo Alto - utilizador de teste de Aperture

Nesta secção, vai criar um utilizador chamado Britta Simon nas redes da Palo Alto - Aperture. Trabalhar com [redes da Palo Alto - equipa de suporte de cliente Aperture](https://live.paloaltonetworks.com/t5/custom/page/page-id/Support) para adicionar os utilizadores nas redes da Palo Alto - plataforma Aperture. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único. 

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso às redes da Palo Alto - Aperture.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon a redes da Palo Alto - Aperture, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **redes da Palo Alto - Aperture**.

    ![As redes da Palo Alto - ligação Aperture na lista de aplicações](./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_paloaltonetwork_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Ao clicar em redes da Palo Alto - mosaico Aperture no painel de acesso, deve obter automaticamente com sessão iniciada para as redes da Palo Alto - aplicação Aperture.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-paloaltonetworks-aperture-tutorial/tutorial_general_203.png

