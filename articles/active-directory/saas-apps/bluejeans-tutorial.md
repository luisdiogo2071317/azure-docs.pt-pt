---
title: 'Tutorial: Integração do Azure Active Directory com BlueJeans | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e BlueJeans.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: dfc634fd-1b55-4ba8-94a8-b8288429b6a9
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2018
ms.author: jeedes
ms.openlocfilehash: d485c16719c07062249e8d40f0feca9685851834
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39043470"
---
# <a name="tutorial-azure-active-directory-integration-with-bluejeans"></a>Tutorial: Integração do Azure Active Directory com BlueJeans

Neste tutorial, saiba como integrar BlueJeans com o Azure Active Directory (Azure AD).

Integrar BlueJeans no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao BlueJeans
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para BlueJeans (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com BlueJeans, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um BlueJeans início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando BlueJeans da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-bluejeans-from-the-gallery"></a>Adicionando BlueJeans da Galeria
Para configurar a integração do BlueJeans com o Azure AD, terá de adicionar BlueJeans a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar BlueJeans a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]

3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **BlueJeans**.

    ![Criar um utilizador de teste do Azure AD](./media/bluejeans-tutorial/tutorial_bluejeans_search.png)

5. No painel de resultados, selecione **BlueJeans**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/bluejeans-tutorial/tutorial_bluejeans_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com BlueJeans com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no BlueJeans a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no BlueJeans deve ser estabelecido.

BlueJeans, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com BlueJeans, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste BlueJeans](#creating-a-bluejeans-test-user)**  - para ter um equivalente da Eduarda Almeida na BlueJeans que está ligado à representação de utilizador do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo BlueJeans.

**Para configurar o Azure AD início de sessão único com BlueJeans, execute os seguintes passos:**

1. No portal do Azure, sobre o **BlueJeans** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.

    ![Configurar o início de sessão único](./media/bluejeans-tutorial/tutorial_bluejeans_samlbase.png)

3. Sobre o **BlueJeans domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/bluejeans-tutorial/tutorial_bluejeans_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<companyname>.BlueJeans.com`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<companyname>.BlueJeans.com`

    > [!NOTE]
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte de cliente BlueJeans](https://support.bluejeans.com/contact) obter esses valores.

4. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/bluejeans-tutorial/tutorial_bluejeans_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/bluejeans-tutorial/tutorial_general_400.png)

6. Sobre o **BlueJeans configuração** secção, clique em **configurar BlueJeans** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão, alterar o URL da palavra-passe e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/bluejeans-tutorial/tutorial_bluejeans_configure.png) 

7. Numa janela do browser web diferente, inicie sessão no seu **BlueJeans** site da empresa como administrador.

8. Aceda a **administrador \> definições de grupo \> segurança**.

   ![Admin](./media/bluejeans-tutorial/IC785868.png "Admin")

9. Na **segurança** secção, execute os seguintes passos:

   ![SAML início de sessão único](./media/bluejeans-tutorial/IC785869.png "SAML início de sessão único")

   a. Selecione **SAML início de sessão único**.

   b. Selecione **ativar o aprovisionamento automático**.

10. Avançar com os seguintes passos:

    ![Caminho de certificado](./media/bluejeans-tutorial/IC785870.png "caminho de certificado")

    a. Clique em **Escolher ficheiro**em seguida, carregue o certificado transferido.

    b. Colar **SAML único início de sessão no URL do serviço** para o **URL de início de sessão** caixa de texto.

    c. Colar **alterar o URL da palavra-passe** para o **URL de alteração de palavra-passe** caixa de texto.

    d. Colar **URL de fim de sessão** para o **URL de fim de sessão** caixa de texto.

11. Avançar com os seguintes passos:

    ![Guardar alterações](./media/bluejeans-tutorial/IC785874.png "guardar alterações")

    a. Na **id de utilizador** caixa de texto, tipo `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    b. Na **E-Mail** caixa de texto, tipo `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    c. Clique em **guardar alterações**.

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/bluejeans-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.

    ![Criar um utilizador de teste do Azure AD](./media/bluejeans-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.

    ![Criar um utilizador de teste do Azure AD](./media/bluejeans-tutorial/create_aaduser_03.png)

4. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:

    ![Criar um utilizador de teste do Azure AD](./media/bluejeans-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.

### <a name="creating-a-bluejeans-test-user"></a>Criar um utilizador de teste BlueJeans

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no BlueJeans. BlueJeans suporta o aprovisionamento automático de utilizadores, que está por predefinição, ativada. Pode encontrar mais detalhes [aqui](bluejeans-provisioning-tutorial.md) sobre como configurar o aprovisionamento automático de utilizadores.

**Se precisar de criar manualmente o utilizador, execute os seguintes passos:**

1. Inicie sessão no seu **BlueJeans** site da empresa como administrador.

2. Aceda a **administrador \> gerir utilizadores \> adicionar utilizador**.

   ![Admin](./media/bluejeans-tutorial/IC785877.png "Admin")

   >[!IMPORTANT]
   >O **adicionar utilizador** separador apenas está disponível se, no **separador segurança**, **ativar o aprovisionamento automático** está desmarcada. 

3. Na **adicionar utilizador** secção, execute os seguintes passos:

    ![Adicionar utilizador](./media/bluejeans-tutorial/IC785886.png "adicionar utilizador")

    a. Introduza um **BlueJeans nome de utilizador**, uma **endereço de E-Mail**, um **BlueJeans reunião ID**, um **código de acesso do moderador**, um **nome completo** , o **empresa** de uma conta do AAD válida que pretende aprovisionar em caixas de texto relacionadas.

    b. Clique em **adicionar utilizador**.

>[!NOTE]
>Pode utilizar quaisquer outras BlueJeans utilizador conta criação ferramentas ou APIs fornecidas pelo BlueJeans para aprovisionar contas de utilizador do AAD.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para BlueJeans.

![Atribuir utilizador][200]

**Para atribuir a Eduarda Almeida a BlueJeans, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201]

2. Na lista de aplicações, selecione **BlueJeans**.

    ![Configurar o início de sessão único](./media/bluejeans-tutorial/tutorial_bluejeans_app.png)

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.

### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico BlueJeans no painel de acesso, deve obter a página de início de sessão da aplicação de BlueJeans.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Configurar o aprovisionamento do utilizador](bluejeans-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/bluejeans-tutorial/tutorial_general_01.png
[2]: ./media/bluejeans-tutorial/tutorial_general_02.png
[3]: ./media/bluejeans-tutorial/tutorial_general_03.png
[4]: ./media/bluejeans-tutorial/tutorial_general_04.png

[100]: ./media/bluejeans-tutorial/tutorial_general_100.png

[200]: ./media/bluejeans-tutorial/tutorial_general_200.png
[201]: ./media/bluejeans-tutorial/tutorial_general_201.png
[202]: ./media/bluejeans-tutorial/tutorial_general_202.png
[203]: ./media/bluejeans-tutorial/tutorial_general_203.png
