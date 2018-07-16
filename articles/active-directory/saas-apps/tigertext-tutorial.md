---
title: 'Tutorial: Integração do Azure Active Directory com o Messenger de proteger TigerText | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e TigerText Messenger de proteger.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 03f1e128-5bcb-4e49-b6a3-fe22eedc6d5e
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.openlocfilehash: 09f04ab4d289eb7fb6664d192404f719f8604e1e
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39041352"
---
# <a name="tutorial-azure-active-directory-integration-with-tigertext-secure-messenger"></a>Tutorial: Integração do Azure Active Directory com o Messenger de proteger TigerText

Neste tutorial, saiba como integrar TigerText Messenger de proteger com o Azure Active Directory (Azure AD).

Integrar TigerText Messenger de seguro no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Messenger de proteger TigerText
- Pode permitir que os utilizadores automaticamente obter sessão iniciada em TigerText Secure Messenger (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Messenger de proteger TigerText, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um TigerText Messenger de proteger o início de sessão único de subscrição ativada

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar TigerText Messenger de proteger a partir da Galeria
2. Configurar e testar o Azure AD início de sessão único

## <a name="add-tigertext-secure-messenger-from-the-gallery"></a>Adicionar TigerText Messenger de proteger a partir da Galeria
Para configurar a integração do Messenger de proteger TigerText com o Azure AD, terá de adicionar TigerText Messenger de proteger a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar TigerText Messenger de proteger a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **Messenger de proteger TigerText**, selecione **Messenger de proteger TigerText** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Adicionar a partir da Galeria](./media/tigertext-tutorial/tutorial_tigertext_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com TigerText Secure Messenger com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador equivalente no TigerText Messenger de proteger a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Messenger de proteger TigerText deve ser estabelecido.

No TigerText Secure Messenger, atribuir o valor do **nome de utilizador** no Azure AD como o valor da **nome de utilizador** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o Messenger de proteger TigerText, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste do Messenger de proteger TigerText](#create-a-tigertext-secure-messenger-test-user)**  - para ter um equivalente da Eduarda Almeida no TigerText Messenger segura que esteja ligado a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo TigerText Messenger de proteger.

**Para configurar o Azure AD início de sessão único com o Messenger de proteger TigerText, execute os seguintes passos:**

1. No portal do Azure, no **Messenger de proteger TigerText** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Inicio de sessão baseado em SAML](./media/tigertext-tutorial/tutorial_tigertext_samlbase.png)

3. Sobre o **TigerText Secure Messenger domínio e URLs** secção, execute os seguintes passos:

    ![Secção TigerText Secure Messenger domínio e URLs](./media/tigertext-tutorial/tutorial_tigertext_url.png)

    a. Na **URL de início de sessão** caixa de texto, URL de tipo como: `https://home.tigertext.com`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://saml-lb.tigertext.me/v1/organization/<instance Id>`

    > [!NOTE] 
    > Este valor não é real. Atualize este valor com o identificador real. Contacte [equipa de suporte de cliente de Messenger Secure TigerText](mailTo:prosupport@tigertext.com) para obter este valor. 
 
4. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![Secção do certificado de assinatura SAML](./media/tigertext-tutorial/tutorial_tigertext_certificate.png) 

5. Clique em **guardar** botão.

    ![Botão Guardar](./media/tigertext-tutorial/tutorial_general_400.png)

6. Para obter o início de sessão único configurado para a sua aplicação, contacte [equipa de suporte do Messenger de proteger TigerText](mailTo:prosupport@tigertext.com) e forneça-lhes a **Downloaded metadados**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/tigertext-tutorial/create_aaduser_01.png) 

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Utilizadores e grupos -> todos os utilizadores](./media/tigertext-tutorial/create_aaduser_02.png) 

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Botão Adicionar](./media/tigertext-tutorial/create_aaduser_03.png) 

4. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Caixa de diálogo de utilizador](./media/tigertext-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="create-a-tigertext-secure-messenger-test-user"></a>Criar um utilizador de teste do Messenger de proteger TigerText

Nesta secção, vai criar um usuário chamado Eduarda Almeida no TigerText. Contacte [equipa de suporte de cliente de Messenger Secure TigerText](mailTo:prosupport@tigertext.com) para adicionar os utilizadores na plataforma TigerText.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso TigerText Messenger de proteger.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida TigerText Messenger de proteger, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Messenger de proteger TigerText**.

    ![TigerText Secure Messenger na lista de aplicações](./media/tigertext-tutorial/tutorial_tigertext_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico TigerText no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo TigerText. Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/tigertext-tutorial/tutorial_general_01.png
[2]: ./media/tigertext-tutorial/tutorial_general_02.png
[3]: ./media/tigertext-tutorial/tutorial_general_03.png
[4]: ./media/tigertext-tutorial/tutorial_general_04.png

[100]: ./media/tigertext-tutorial/tutorial_general_100.png

[200]: ./media/tigertext-tutorial/tutorial_general_200.png
[201]: ./media/tigertext-tutorial/tutorial_general_201.png
[202]: ./media/tigertext-tutorial/tutorial_general_202.png
[203]: ./media/tigertext-tutorial/tutorial_general_203.png

