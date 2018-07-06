---
title: 'Tutorial: Integração do Azure Active Directory com o Dropbox for Business | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o Dropbox for Business.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 63502412-758b-4b46-a580-0e8e130791a1
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: jeedes
ms.openlocfilehash: 3423f723b3f4857db44c609cd0e2143b6b04653e
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37868634"
---
# <a name="tutorial-azure-active-directory-integration-with-dropbox-for-business"></a>Tutorial: Integração do Azure Active Directory com o Dropbox for Business

Neste tutorial, saiba como integrar o Dropbox for Business no Azure Active Directory (Azure AD).

Integrar o Dropbox for Business no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Dropbox para empresas.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para o Dropbox for Business (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Dropbox for Business, terá dos seguintes itens:

- Uma subscrição do Azure AD
- O Dropbox for Business logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o Dropbox for Business da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-dropbox-for-business-from-the-gallery"></a>Adicionando o Dropbox for Business da Galeria
Para configurar a integração do Dropbox para empresas com o Azure AD, terá de adicionar Dropbox para a empresa a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Dropbox para a empresa a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **Dropbox para empresas**, selecione **Dropbox para empresas** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Dropbox para a empresa na lista de resultados](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com o Dropbox for Business, com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Dropbox para empresas para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado na Dropbox para empresas tem de ser estabelecida.

No Dropbox para empresas, atribuir o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o Dropbox for Business, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um Dropbox para o utilizador de teste de negócios](#create-a-dropbox-for-business-test-user)**  - para ter um equivalente da Eduarda Almeida na Dropbox para a empresa que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único no seu Dropbox para o aplicativo de negócios.

**Para configurar o Azure AD início de sessão único com o Dropbox for Business, execute os seguintes passos:**

1. No portal do Azure, sobre o **Dropbox para empresas** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_samlbase.png)

3. Sobre o **Dropbox para o domínio de negócios e URLs** secção, execute os seguintes passos:

    ![Dropbox para domínio de negócios e URLs únicos início de sessão em informações](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_url1.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://www.dropbox.com/sso/<id>`

    b. Na **identificador** caixa de texto, escreva um valor: `Dropbox`

    > [!NOTE] 
    > O valor de URL de início de sessão anterior não é o valor real. Atualizar o valor com o início de sessão no URL real, que é explicado mais tarde no tutorial. Contacte [Dropbox para a equipa de suporte do cliente de negócios](https://www.dropbox.com/business/contact) para obter o valor. 
 

4. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/dropboxforbusiness-tutorial/tutorial_general_400.png)

6. Sobre o **Dropbox para a configuração de negócios** secção, clique em **configurar o Dropbox for Business** para abrir **configurar início de sessão** janela. Cópia a **SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Dropbox para a configuração de negócios](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_configure.png) 

7. Para configurar o início de sessão único num **Dropbox para empresas** lado, vá no seu Dropbox para o inquilino de negócios.

    a. Inicie sessão seu Dropbox para o inquilino de negócios. 
   
    ![Configurar o início de sessão único](./media/dropboxforbusiness-tutorial/ic769509.png "configurar início de sessão único")
   
    b. No painel de navegação no lado esquerdo, clique em **consola de administração**. 
   
    ![Configurar o início de sessão único](./media/dropboxforbusiness-tutorial/ic769510.png "configurar início de sessão único")
   
    c. Sobre o **consola de administração**, clique em **autenticação** no painel de navegação esquerdo. 
   
    ![Configurar o início de sessão único](./media/dropboxforbusiness-tutorial/ic769511.png "configurar início de sessão único")
   
    d. Na **início de sessão único** secção, selecione **ativar início de sessão único**e, em seguida, clique em **mais** para expandir essa secção.  
   
    ![Configurar o início de sessão único](./media/dropboxforbusiness-tutorial/ic769512.png "configurar início de sessão único")
   
    e. Copie o URL junto a **os utilizadores podem iniciar sessão introduzindo o respetivo endereço de e-mail ou podem aceder diretamente à** e cole-o para o **URL de início de sessão** caixa de texto de **Dropbox para o domínio de negócios e URLs** secção no portal do Azure. 
    
    ![Configurar o início de sessão único](./media/dropboxforbusiness-tutorial/ic769513.png)
    
8. Na **início de sessão único** secção a **autenticação** página, execute os seguintes passos: 
   
    ![Configurar o início de sessão único](./media/dropboxforbusiness-tutorial/IC769516.png "configurar início de sessão único")
   
    a. Clique em **necessário**.
   
    b. Na **URL de início de sessão** caixa de texto, cole o valor de **SAML único início de sessão no URL do serviço** que copiou do portal do Azure.

    c. Clique em **Escolher certificado**e, em seguida, navegue até sua **ficheiro de certificado codificado Base64**.

    d. Clique em **guardar alterações** para concluir a configuração no seu DropBox para o inquilino de negócios.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/dropboxforbusiness-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/dropboxforbusiness-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/dropboxforbusiness-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/dropboxforbusiness-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-dropbox-for-business-test-user"></a>Criar um Dropbox para o utilizador de teste de negócios

Nesta secção, um usuário chamado Eduarda Almeida é criado na Dropbox para empresas. O Dropbox for Business suporta o aprovisionamento de just-in-time, que está ativado por predefinição.

Não existe nenhum item de ação para nesta secção. Se um utilizador já não existir no Dropbox para empresas, uma nova é criada quando tentar acessar o Dropbox for Business.

>[!Note]
>Se precisar de criar manualmente um utilizador, contacte o [Dropbox para a equipa de suporte de empresas do cliente](https://www.dropbox.com/business/contact) 

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para o Dropbox for Business.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida para o Dropbox for Business, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Dropbox para empresas**.

    ![O Dropbox para ligação de negócios na lista de aplicações](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica Dropbox para o mosaico de negócios no painel de acesso, deve obter a página de início de sessão da sua Dropbox para o aplicativo de negócios.
 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/dropboxforbusiness-tutorial/tutorial_general_01.png
[2]: ./media/dropboxforbusiness-tutorial/tutorial_general_02.png
[3]: ./media/dropboxforbusiness-tutorial/tutorial_general_03.png
[4]: ./media/dropboxforbusiness-tutorial/tutorial_general_04.png

[100]: ./media/dropboxforbusiness-tutorial/tutorial_general_100.png

[200]: ./media/dropboxforbusiness-tutorial/tutorial_general_200.png
[201]: ./media/dropboxforbusiness-tutorial/tutorial_general_201.png
[202]: ./media/dropboxforbusiness-tutorial/tutorial_general_202.png
[203]: ./media/dropboxforbusiness-tutorial/tutorial_general_203.png

