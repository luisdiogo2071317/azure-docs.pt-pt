---
title: 'Tutorial: Integração do Azure Active Directory com regular de Data3Sixty Infogix | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e regular a Data3Sixty Infogix.
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
ms.openlocfilehash: ef45bbb20ccb5db8c383717eb81bfe8fa18720c7
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39042848"
---
# <a name="tutorial-azure-active-directory-integration-with-infogix-data3sixty-govern"></a>Tutorial: Integração do Azure Active Directory com regular de Data3Sixty Infogix

Neste tutorial, saiba como integrar a regular de Data3Sixty Infogix de mensagens em fila com o Azure Active Directory (Azure AD).

Integração regular de Data3Sixty Infogix com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso para regular de Data3Sixty Infogix.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Infogix Data3Sixty regular (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com regular de Data3Sixty Infogix, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um regular de Data3Sixty Infogix logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar Infogix Data3Sixty regular a partir da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-infogix-data3sixty-govern-from-the-gallery"></a>Adicionar Infogix Data3Sixty regular a partir da Galeria
Para configurar a integração do regular de Data3Sixty Infogix com o Azure AD, terá de adicionar regular de Data3Sixty Infogix partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar a regular de Data3Sixty Infogix partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **regem o Data3Sixty Infogix**, selecione **regular de Data3Sixty Infogix** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Infogix Data3Sixty regem na lista de resultados](./media/infogix-tutorial/tutorial_infogix_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Infogix Data3Sixty regem com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no regular de Data3Sixty Infogix a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no regular de Data3Sixty Infogix deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com regular de Data3Sixty Infogix, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste regular de Data3Sixty Infogix](#create-an-infogix-data3sixty-govern-test-user)**  - para ter um equivalente da Eduarda Almeida na Infogix Data3Sixty regem que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação regular de Data3Sixty Infogix.

**Para configurar o Azure AD início de sessão único com regular de Data3Sixty Infogix, execute os seguintes passos:**

1. No portal do Azure, sobre o **regular de Data3Sixty Infogix** página de integração de aplicativo, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/infogix-tutorial/tutorial_infogix_samlbase.png)

3. Sobre o **Infogix Data3Sixty regem o domínio e URLs** secção, execute os seguintes passos, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

    ![Infogix Data3Sixty regem o domínio e URLs únicas início de sessão em informações](./media/infogix-tutorial/tutorial_infogix_url.png)

    a. Na **identificador** caixa de texto, escreva um URL: `https://data3sixty.com/ui`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.data3sixty.com/sso/acs`

4. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![Infogix Data3Sixty regem o domínio e URLs únicas início de sessão em informações](./media/infogix-tutorial/tutorial_infogix_url1.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.data3sixty.com`
     
    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de resposta e o URL de início de sessão real. Contacte [equipa de suporte de Infogix Data3Sixty regem cliente](mailto:data3sixtysupport@infogix.com) obter esses valores.

5. Aplicação regular de Data3Sixty Infogix espera que as asserções SAML num formato específico. Configure as seguintes declarações para esta aplicação. Pode gerir os valores destes atributos do **atributos de utilizador** secção na página de integração de aplicações. Captura de ecrã seguinte mostra um exemplo disso.
    
    ![Configurar o início de sessão único attb](./media/infogix-tutorial/tutorial_infogix_attribute.png)
    
6. No **atributos de utilizador** secção sobre o **início de sessão único** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem e execute os seguintes passos:
    
    | Nome do Atributo | Valor do Atributo |
    | ------------------- | -------------------- |    
    | FirstName           | User.givenName |
    | Apelido        | User.Surname |
    | o nome de utilizador       | User.Mail    |
    
    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único adicionar](./media/infogix-tutorial/tutorial_attribute_04.png)

    ![Configurar Addattb de início de sessão único](./media/infogix-tutorial/tutorial_attribute_05.png)

    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d. Deixe o **espaço de nomes** em branco.
    
    e. Clique em **OK**.

7. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (bruto)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/infogix-tutorial/tutorial_infogix_certificate.png)

8. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/infogix-tutorial/tutorial_general_400.png)
    
9. Sobre o **Infogix Data3Sixty regular a configuração** secção, clique em **configurar regular de Data3Sixty Infogix** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Infogix Data3Sixty regem a configuração](./media/infogix-tutorial/tutorial_infogix_configure.png) 

10. Para configurar o início de sessão único num **regem o Data3Sixty Infogix** lado, terá de enviar o transferido **certificado (bruto), URL de fim de sessão, ID de entidade de SAML e SAML único início de sessão no URL do serviço** para [ A equipa de suporte regular de Data3Sixty Infogix](mailto:data3sixtysupport@infogix.com). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/infogix-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/infogix-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/infogix-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/infogix-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-an-infogix-data3sixty-govern-test-user"></a>Criar um utilizador de teste regular de Data3Sixty Infogix


O objetivo desta secção é criar um usuário chamado Eduarda Almeida no regular de Data3Sixty Infogix. Regular de Data3Sixty Infogix suporta o aprovisionamento just-in-time, que está por predefinição, ativada. Não existe nenhum item de ação para nesta secção. Um novo utilizador é criado durante uma tentativa de aceder a regular de Data3Sixty Infogix se não existir ainda.

>[!Note]
>Se precisar de criar manualmente um utilizador, contacte [regem o Data3Sixty Infogix a equipa de suporte](mailto:data3sixtysupport@infogix.com).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para regular de Data3Sixty Infogix.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida para regular de Data3Sixty Infogix, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **regem o Data3Sixty Infogix**.

    ![A ligação regular de Data3Sixty Infogix na lista de aplicações](./media/infogix-tutorial/tutorial_infogix_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico regular de Data3Sixty Infogix no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação regular de Data3Sixty Infogix.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



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

