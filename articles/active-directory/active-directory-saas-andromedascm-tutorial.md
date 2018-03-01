---
title: "Tutorial: Integração do Azure Active Directory com o Andromeda SCM | Microsoft Docs"
description: "Saiba como configurar o início de sessão entre o Azure Active Directory e Andromeda SCM."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7a142c86-ca0c-4915-b1d8-124c08c3e3d8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2018
ms.author: jeedes
ms.openlocfilehash: 72b66eec34995c334c6d65a1d03637fe21b9dc80
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/27/2018
---
# <a name="tutorial-azure-active-directory-integration-with-andromeda-scm"></a>Tutorial: Integração do Azure Active Directory com o Andromeda SCM

Neste tutorial, irá aprender a integrar o Andromeda SCM com o Azure Active Directory (Azure AD).

Integrar o Andromeda SCM com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Andromeda SCM.
- Pode permitir aos utilizadores automaticamente obter com sessão iniciada para Andromeda SCM (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Andromeda SCM, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Andromeda SCM-início de sessão único ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar Andromeda SCM na galeria do
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-andromeda-scm-from-the-gallery"></a>Adicionar Andromeda SCM na galeria do
Para configurar a integração de Andromeda SCM com o Azure AD, tem de adicionar Andromeda SCM na Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar Andromeda SCM a partir da galeria, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **Andromeda SCM**, selecione **Andromeda SCM** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![Andromeda SCM na lista de resultados](./media/active-directory-saas-andromedascm-tutorial/tutorial_andromedascm_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com o SCM Andromeda com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo no Andromeda SCM é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Andromeda SCM tem de ser estabelecida.

Para configurar e testar o Azure AD-início de sessão único com o Andromeda SCM, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste Andromeda SCM](#create-an-andromeda-scm-test-user)**  - para ter um homólogo de Britta Simon no SCM Andromeda que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação Andromeda SCM.

**Para configurar o Azure AD-início de sessão único com o Andromeda SCM, execute os seguintes passos:**

1. No portal do Azure, no **Andromeda SCM** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/active-directory-saas-andromedascm-tutorial/tutorial_andromedascm_samlbase.png)

3. No **Andromeda SCM domínio e os URLs** secção, execute os seguintes passos, se pretender configurar a aplicação no **IDP** iniciada modo:

    ![Andromeda SCM domínio e os URLs únicos de informações de início de sessão](./media/active-directory-saas-andromedascm-tutorial/tutorial_andromedascm_url.png)

    a. No **identificador** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://<tenantURL>`

    b. No **URL de resposta** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://<tenantURL>`

4. Verifique **Mostrar avançadas definições de URL** e executar o passo seguinte, se pretender configurar a aplicação no **SP** iniciada modo:

    ![Andromeda SCM domínio e os URLs únicos de informações de início de sessão](./media/active-directory-saas-andromedascm-tutorial/tutorial_andromedascm_url1.png)

    No **URL de início de sessão** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://<tenantURL>/SAMLLogon.aspx`
     
    > [!NOTE] 
    > O valor anterior não é o valor real. Atualizar o valor com o identificador de real, a URL de resposta e o URL de início de sessão que é explicada mais tarde no tutorial.

5. Aplicação de Andromeda SCM espera as asserções de SAML num formato específico. Configure as seguintes afirmações para esta aplicação. Pode gerir os valores destes atributos do **atributos de utilizador** secção na página de integração de aplicações. A seguinte captura de ecrã mostra um exemplo para este.
    
    ![Configurar attb Single Sign-On](./media/active-directory-saas-andromedascm-tutorial/tutorial_andromedascm_attribute.png)

    > [!Important]
    > Desmarque terminar as definições de espaço de nomes ao configurar estes.
    
6. No **atributos de utilizador** secção no **de sessão único-** caixa de diálogo, configurar atributo token SAML, conforme mostrado na imagem e efetuar os seguintes passos:
    
    | Nome do Atributo | Valor do Atributo |
    | ------------------- | -------------------- |    
    | função        | DEMO |
    | tipo        | PREDEFINIÇÃO |
    | Empresa       | COMP02    |

    > [!NOTE]
    > Não existem valores reais. Estes valores são apenas para fins de demonstração, utilize as funções de informático.

    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único adicionar](./media/active-directory-saas-andromedascm-tutorial/tutorial_attribute_04.png)

    ![Configurar Addattb de início de sessão único](./media/active-directory-saas-andromedascm-tutorial/tutorial_attribute_05.png)

    b. No **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d. Deixe o **espaço de nomes** em branco.
    
    e. Clique em **OK**.

7. No **certificado de assinatura de SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![A hiperligação de transferência do certificado](./media/active-directory-saas-andromedascm-tutorial/tutorial_andromedascm_certificate.png) 

8. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-andromedascm-tutorial/tutorial_general_400.png)
    
9. No **Andromeda SCM configuração** secção, clique em **configurar SCM de Andromeda** para abrir **configurar início de sessão** janela. Copiar o **único início de sessão no URL do serviço SAML** do **secção de referência rápida.**

    ![Configuração de SCM Andromeda](./media/active-directory-saas-andromedascm-tutorial/tutorial_andromedascm_configure.png)

10. Início de sessão no site da sua empresa Andromeda SCM como administrador.

11. Na parte superior do menubar clique **Admin** e navegue para **administração**.

    ![Andromeda SCM admin](./media/active-directory-saas-andromedascm-tutorial/tutorial_andromedascm_admin.png)

12. No lado esquerdo da barra de ferramentas em **Interfaces** secção, clique em **configuração SAML**.

    ![Andromeda SCM saml](./media/active-directory-saas-andromedascm-tutorial/tutorial_andromedascm_saml.png)

13. No **configuração SAML** secção página, execute os seguintes passos:

    ![Configuração de Andromeda SCM](./media/active-directory-saas-andromedascm-tutorial/tutorial_andromedascm_config.png)

    a. Verifique **ativar a SSO com SAML**.

    b. Em **Andromeda informações** secção, copie o **SP identidade** valor e cole-o para o **identificador** caixa de texto de **Andromeda SCM domínio e URLs** secção.

    c. Copiar o **URL de consumidor** valor e cole-o para o **URL de resposta** caixa de texto de **Andromeda SCM domínio e os URLs** secção.

    d. Copiar o **URL de início de sessão** valor e cole-o para o **URL de início de sessão** caixa de texto de **Andromeda SCM domínio e os URLs** secção.

    e. Em **fornecedor de identidade** secção, escreva o nome do IDP.

    f. No **início de sessão único no ponto final** caixa de texto, cole o valor de **único início de sessão no URL do serviço SAML** onde, tem de ser copiados do portal do Azure.

    g. Abra o transferido **certificado codificado Base64** no portal do Azure no bloco de notas, cole-a para o **X 509 certificado** caixa de texto.
    
    h. Mapear os seguintes atributos com o respetivo valor para facilitar o início de sessão do SSO do Azure AD. O **ID de utilizador** atributo é necessário para iniciar sessão. Para o aprovisionamento, **E-Mail**, **empresa**, **UserType** e **função** são necessários. Nesta secção iremos definir os atributos de mapeamento (nome e os valores) que correlacionar às definida no portal do Azure

    ![Andromeda SCM attbmap](./media/active-directory-saas-andromedascm-tutorial/tutorial_andromedascm_attbmap.png)

    i. Clique em **Guardar**.

> [!TIP]
> Pode agora ler estas instruções dentro de uma versão concisa o [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir do **do Active Directory > aplicações da empresa** secção, basta clicar no **Single Sign-On** separador e aceder à documentação do embedded através de **configuração** secção na parte inferior. Pode ler mais sobre a funcionalidade de documentação incorporados aqui: [do Azure AD incorporado documentação]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/active-directory-saas-andromedascm-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/active-directory-saas-andromedascm-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/active-directory-saas-andromedascm-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/active-directory-saas-andromedascm-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-an-andromeda-scm-test-user"></a>Criar um utilizador de teste Andromeda SCM

O objetivo desta secção consiste em criar um utilizador chamado Britta Simon Andromeda SCM. Andromeda SCM suportam o aprovisionamento de just-in-time, que é por predefinição ativada. Não há nenhum item de ação para si nesta secção. Um novo utilizador é criado durante a tentativa de aceder Andromeda SCM se não existir ainda.

>[!Note]
>Se precisar de criar manualmente um utilizador, contacte [equipa de suporte de cliente de SCM Andromeda](https://www.ngcsoftware.com/support/).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso para Andromeda SCM.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon a Andromeda SCM, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **Andromeda SCM**.

    ![A ligação de Andromeda SCM na lista de aplicações](./media/active-directory-saas-andromedascm-tutorial/tutorial_andromedascm_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Teste o início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico Andromeda SCM no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação de Andromeda SCM.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-andromedascm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-andromedascm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-andromedascm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-andromedascm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-andromedascm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-andromedascm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-andromedascm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-andromedascm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-andromedascm-tutorial/tutorial_general_203.png

