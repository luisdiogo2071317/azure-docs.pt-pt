---
title: 'Tutorial: Integração do Azure Active Directory com Infogix Data3Sixty governar | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e Infogix Data3Sixty regular.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: aa3109b8-bdbe-45ae-933a-2eb4dc03855c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/23/2018
ms.author: jeedes
ms.openlocfilehash: 3acdfbc7665bec6cfb8821f33e642f29eec84be1
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36214921"
---
# <a name="tutorial-azure-active-directory-integration-with-infogix-data3sixty-govern"></a>Tutorial: Integração do Azure Active Directory com Infogix Data3Sixty governar

Neste tutorial, irá aprender a integrar Infogix Data3Sixty governar com o Azure Active Directory (Azure AD).

Infogix Data3Sixty governar a integração com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Infogix Data3Sixty regular.
- Pode permitir aos utilizadores automaticamente obter com sessão iniciada para Infogix Data3Sixty regulamentar (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Infogix Data3Sixty governar, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Infogix Data3Sixty governar-início de sessão único ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar Infogix Data3Sixty regular a partir da Galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-infogix-data3sixty-govern-from-the-gallery"></a>Adicionar Infogix Data3Sixty regular a partir da Galeria
Para configurar a integração de Infogix Data3Sixty governar com o Azure AD, tem de adicionar Infogix Data3Sixty governem da Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar Infogix Data3Sixty governem da galeria do, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **Infogix Data3Sixty governar**, selecione **Infogix Data3Sixty governar** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![Infogix Data3Sixty governar na lista de resultados](./media/infogix-tutorial/tutorial_infogix_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com Infogix Data3Sixty governar com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo Infogix Data3Sixty governar é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Infogix Data3Sixty governar tem de ser estabelecida.

Para configurar e testar o Azure AD-início de sessão único com Infogix Data3Sixty governar, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste Infogix Data3Sixty governar](#create-an-infogix-data3sixty-govern-test-user)**  - para ter um homólogo de Britta Simon Infogix Data3Sixty governar que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação Infogix Data3Sixty regular.

**Para configurar o Azure AD-início de sessão único com Infogix Data3Sixty governar, execute os seguintes passos:**

1. No portal do Azure, no **Infogix Data3Sixty governar** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/infogix-tutorial/tutorial_infogix_samlbase.png)

3. No **Infogix Data3Sixty governar domínio e os URLs** secção, execute os seguintes passos, se pretender configurar a aplicação no **IDP** iniciada modo:

    ![Infogix Data3Sixty governar domínio e os URLs únicos de informações de início de sessão](./media/infogix-tutorial/tutorial_infogix_url.png)

    a. No **identificador** caixa de texto, escreva um URL: `https://data3sixty.com/ui`

    b. No **URL de resposta** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://<subdomain>.data3sixty.com/sso/acs`

4. Verifique **Mostrar avançadas definições de URL** e executar o passo seguinte, se pretender configurar a aplicação no **SP** iniciada modo:

    ![Infogix Data3Sixty governar domínio e os URLs únicos de informações de início de sessão](./media/infogix-tutorial/tutorial_infogix_url1.png)

    No **URL de início de sessão** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://<subdomain>.data3sixty.com`
     
    > [!NOTE] 
    > Estes valores não estiverem reais. Atualize estes valores com o URL de resposta real e o URL de início de sessão. Contacte [equipa de suporte de Infogix Data3Sixty governar cliente](mailto:data3sixtysupport@infogix.com) para obter estes valores.

5. Aplicação Infogix Data3Sixty governar espera as asserções de SAML num formato específico. Configure as seguintes afirmações para esta aplicação. Pode gerir os valores destes atributos do **atributos de utilizador** secção na página de integração de aplicações. A seguinte captura de ecrã mostra um exemplo para este.
    
    ![Configurar attb Single Sign-On](./media/infogix-tutorial/tutorial_infogix_attribute.png)
    
6. No **atributos de utilizador** secção no **de sessão único-** caixa de diálogo, configurar atributo token SAML, conforme mostrado na imagem e efetuar os seguintes passos:
    
    | Nome do Atributo | Valor do Atributo |
    | ------------------- | -------------------- |    
    | nome próprio           | User.givenName |
    | Apelido        | User.Surname |
    | o nome de utilizador       | User.Mail    |
    
    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único adicionar](./media/infogix-tutorial/tutorial_attribute_04.png)

    ![Configurar Addattb de início de sessão único](./media/infogix-tutorial/tutorial_attribute_05.png)

    b. No **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d. Deixe o **espaço de nomes** em branco.
    
    e. Clique em **OK**.

7. No **certificado de assinatura de SAML** secção, clique em **certificado (bruto)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![A hiperligação de transferência do certificado](./media/infogix-tutorial/tutorial_infogix_certificate.png)

8. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/infogix-tutorial/tutorial_general_400.png)
    
9. No **Infogix Data3Sixty governar configuração** secção, clique em **configurar governar de Data3Sixty Infogix** para abrir **configurar início de sessão** janela. Copiar o **Sign-Out URL, o ID de entidade de SAML e o único início de sessão no URL do serviço SAML** do **secção de referência rápida.**

    ![Infogix Data3Sixty governar configuração](./media/infogix-tutorial/tutorial_infogix_configure.png) 

10. Para configurar o início de sessão único em **Infogix Data3Sixty governar** lado, terá de enviar o transferido **certificado (bruto), Sign-Out URL, ID de entidade de SAML e único início de sessão no URL do serviço SAML** para [ Equipa de suporte Infogix Data3Sixty governar](mailto:data3sixtysupport@infogix.com). Se definir esta definição para que a ligação de SAML SSO corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/infogix-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/infogix-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/infogix-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/infogix-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-an-infogix-data3sixty-govern-test-user"></a>Criar um utilizador de teste Infogix Data3Sixty governar


O objetivo desta secção consiste em criar um utilizador chamado Britta Simon Infogix Data3Sixty regular. Infogix Data3Sixty governar suporta aprovisionamento just-in-time, que é por predefinição ativada. Não há nenhum item de ação para si nesta secção. Um novo utilizador é criado durante a tentativa de aceder Infogix Data3Sixty governar se não existir ainda.

>[!Note]
>Se precisar de criar manualmente um utilizador, contacte [Infogix Data3Sixty governar a equipa de suporte](mailto:data3sixtysupport@infogix.com).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso para Infogix Data3Sixty regulamentar.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon para Infogix Data3Sixty governar a, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **Infogix Data3Sixty governar**.

    ![A ligação Infogix Data3Sixty governar na lista de aplicações](./media/infogix-tutorial/tutorial_infogix_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico Infogix Data3Sixty governar no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação Infogix Data3Sixty regular.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/infogix-tutorial/tutorial_general_01.png
[2]: ./media/infogix-tutorial/tutorial_general_02.png
[3]: ./media/infogix-tutorial/tutorial_general_03.png
[4]: ./media/infogix-tutorial/tutorial_general_04.png

[100]: ./media/infogix-tutorial/tutorial_general_100.png

[200]: ./media/infogix-tutorial/tutorial_general_200.png
[201]: ./media/infogix-tutorial/tutorial_general_201.png
[202]: ./media/infogix-tutorial/tutorial_general_202.png
[203]: ./media/infogix-tutorial/tutorial_general_203.png

