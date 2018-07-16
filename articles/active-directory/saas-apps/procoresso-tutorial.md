---
title: 'Tutorial: Integração do Azure Active Directory com o Procore SSO | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o Procore SSO.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 9818edd3-48c0-411d-b05a-3ec805eafb2e
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: jeedes
ms.openlocfilehash: bd84224f4c3a8a498a296ff50190713111895472
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39051620"
---
# <a name="tutorial-azure-active-directory-integration-with-procore-sso"></a>Tutorial: Integração do Azure Active Directory com o Procore SSO

Neste tutorial, saiba como integrar o Procore SSO com o Azure Active Directory (Azure AD).

Integrar o Procore SSO no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Procore SSO
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Procore SSO (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – o portal de gestão do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Procore SSO, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Procore SSO início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não deve utilizar o seu ambiente de produção, a menos que isso é necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Procore SSO da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-procore-sso-from-the-gallery"></a>Adicionando Procore SSO da Galeria
Para configurar a integração do Procore SSO para o Azure AD, terá de adicionar Procore SSO partir da galeria, à sua lista de aplicações de SaaS geridas.

**Para adicionar Procore SSO a partir da galeria, execute os seguintes passos:**

1. Na  **[do Portal de gestão do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
3. Clique em **adicionar** botão na parte superior da caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **Procore SSO**.

    ![Criar um utilizador de teste do Azure AD](./media/procoresso-tutorial/tutorial_procoresso_search.png)

5. No painel de resultados, selecione **Procore SSO**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/procoresso-tutorial/tutorial_procoresso_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com Procore SSO com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Procore SSO a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Procore SSO tem de ser estabelecida.

Esta relação de ligação é estabelecida atribuindo o valor do **nome de utilizador** no Azure AD como o valor do **Username** no Procore SSO.

Para configurar e testar o Azure AD início de sessão único com o Procore SSO, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste de Procore SSO](#creating-a-procore-sso-test-user)**  - para ter um equivalente da Eduarda Almeida na SSO Procore que está ligado à representação de ela do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal de gestão do Azure e configurar início de sessão único na sua aplicação Procore SSO.

**Para configurar o Azure AD início de sessão único com o Procore SSO, execute os seguintes passos:**

1. No portal de gestão do Azure, sobre o **Procore SSO** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML início de sessão** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/procoresso-tutorial/tutorial_procoresso_samlbase.png)

3. Sobre o **Procore de SSO de domínio e URLs** secção, o utilizador não tem de efetuar outros passos de como a aplicação já está pré-integrada com o Azure.

    ![Configurar o início de sessão único](./media/procoresso-tutorial/tutorial_procoresso_url.png)

4. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro XML em seu computador.

    ![Configurar o início de sessão único](./media/procoresso-tutorial/tutorial_procoresso_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/procoresso-tutorial/tutorial_general_400.png)

6. Sobre o **Procore de SSO de configuração** secção, clique em **configurar SSO Procore** para abrir **configurar início de sessão** janela. Cópia a **ID de entidade de SAML e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/procoresso-tutorial/tutorial_procoresso_configure.png) 

7. Para configurar o início de sessão único num **Procore SSO** lado, o início de sessão para o site da sua empresa procore como administrador.

8. No menu de caixa de ferramentas para baixo, clique em **administrador** para abrir a página de definições de SSO.

    ![Configurar o início de sessão único](./media/procoresso-tutorial/procore_tool_admin.png)

9. Cole os valores nas caixas, conforme descrito abaixo-

    ![Configurar o início de sessão único](./media/procoresso-tutorial/procore_setting_admin.png)  

    a. Na **único início de sessão no URL do emissor** caixa, colar o ID de entidade de SAML copiado a partir do portal do Azure.

    b. Na **início de sessão SAML no URL de destino** caixa, colar o SAML único início de sessão no URL do serviço copiado a partir do portal do Azure.

    c. Agora, abra a **XML de metadados** baixado anteriormente no portal do Azure e copie o certificado na marca com o nome **X509Certificate**. Cole o valor copiado para o **Single Sign On x509 certificado** caixa.

10. Clique em **guardar alterações**.

11. Depois destas definições, precisa enviar o **nome de domínio** (por exemplo, de **contoso.com**) através do qual está a iniciar sessão em Procore para o [equipa de suporte Procore](https://support.procore.com/) e eles serão Ative o SSO federado para esse domínio.

<!--### Next steps

To ensure users can sign-in to Procore SSO after it has been configured to use Azure Active Directory, review the following tasks and topics:

- User accounts must be pre-provisioned into Procore SSO prior to sign-in. To set this up, see Provisioning.
 
- Users must be assigned access to Procore SSO in Azure AD to sign-in. To assign users, see Users.
 
- To configure access polices for Procore SSO users, see Access Policies.
 
- For additional information on deploying single sign-on to users, see [this article](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure Management chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal de gestão do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/procoresso-tutorial/create_aaduser_01.png) 

2. Aceda a **utilizadores e grupos** e clique em **todos os utilizadores** para apresentar a lista de utilizadores.
    
    ![Criar um utilizador de teste do Azure AD](./media/procoresso-tutorial/create_aaduser_02.png) 

3. Na parte superior da caixa de diálogo, clique em **Add** para abrir o **utilizador** caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/procoresso-tutorial/create_aaduser_03.png) 

4. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/procoresso-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-procore-sso-test-user"></a>Criar um utilizador de teste Procore SSO

Siga os passos abaixo para criar um utilizador de teste Procore no seu lado.

1. Início de sessão para o site da sua empresa procore como administrador.  

2. No menu de caixa de ferramentas para baixo, clique em **Directory** para abrir a página do diretório da empresa.

    ![Configurar o início de sessão único](./media/procoresso-tutorial/Procore_sso_directory.png)

3. Clique em **adicionar uma pessoa** executar a opção para abrir o formulário e introduza seguindo as opções,

    ![Configurar o início de sessão único](./media/procoresso-tutorial/Procore_user_add.png)

    a. Na **nome próprio** caixa de texto, nome do usuário do tipo como **Eduarda**.

    b. Na **Apelido** textbox, o apelido do utilizador de tipo, como **Simon**.

    c. Na **endereço de E-Mail** caixa de texto, como o endereço de e-mail do utilizador do tipo **BrittaSimon@contoso.com**.

    d. Selecione **modelo de permissão** como **aplicar o modelo de permissão mais tarde**.

    e. Clique em **Criar**.

4. Verificar e atualizar os detalhes do contacto da recém-adicionada.

    ![Configurar o início de sessão único](./media/procoresso-tutorial/Procore_user_check.png)

5. Clique em **guardar e enviar Invitiation** (se um convite através de correio é necessário) ou **guardar** (guardar diretamente) para concluir o registo de utilizador.
    
    ![Configurar o início de sessão único](./media/procoresso-tutorial/Procore_user_save.png)  

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso para Procore SSO.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a Procore SSO, execute os seguintes passos:**

1. No portal de gestão do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Procore SSO**.

    ![Configurar o início de sessão único](./media/procoresso-tutorial/tutorial_procoresso_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Se pretender testar as definições de início de sessão únicas, abra o painel de acesso. Para obter mais detalhes sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). Quando clica no mosaico de Procore SSO no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação Procore SSO.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/procoresso-tutorial/tutorial_general_01.png
[2]: ./media/procoresso-tutorial/tutorial_general_02.png
[3]: ./media/procoresso-tutorial/tutorial_general_03.png
[4]: ./media/procoresso-tutorial/tutorial_general_04.png

[100]: ./media/procoresso-tutorial/tutorial_general_100.png

[200]: ./media/procoresso-tutorial/tutorial_general_200.png
[201]: ./media/procoresso-tutorial/tutorial_general_201.png
[202]: ./media/procoresso-tutorial/tutorial_general_202.png
[203]: ./media/procoresso-tutorial/tutorial_general_203.png

