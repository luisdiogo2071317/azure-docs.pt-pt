---
title: 'Tutorial: Integração do Azure Active Directory com SignalFx | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e SignalFx.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 6d5ab4b0-29bc-4b20-8536-d64db7530f32
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/16/2018
ms.author: jeedes
ms.openlocfilehash: 3fe91df742ab09a95902a349e797eb89c9a84db1
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/18/2018
ms.locfileid: "35954257"
---
# <a name="tutorial-azure-active-directory-integration-with-signalfx"></a>Tutorial: Integração do Azure Active Directory com SignalFx

Neste tutorial, irá aprender a integrar SignalFx com o Azure Active Directory (Azure AD).

Integrar SignalFx com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao SignalFx.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para SignalFx (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com SignalFx, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um SignalFx-início de sessão único ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar SignalFx a partir da Galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-signalfx-from-the-gallery"></a>Adicionar SignalFx a partir da Galeria
Para configurar a integração de SignalFx com o Azure AD, terá de adicionar SignalFx a partir da Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar SignalFx a partir da galeria, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **SignalFx**, selecione **SignalFx** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![SignalFx na lista de resultados](./media/signalfx-tutorial/tutorial_signalfx_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com SignalFx com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo no SignalFx é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no SignalFx tem de ser estabelecida.

Para configurar e testar o Azure AD-início de sessão único com SignalFx, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste SignalFx](#create-a-signalfx-test-user)**  - para ter um homólogo de Britta Simon SignalFx que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação SignalFx.

**Para configurar o Azure AD-início de sessão único com SignalFx, execute os seguintes passos:**

1. No portal do Azure, no **SignalFx** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/signalfx-tutorial/tutorial_signalfx_samlbase.png)

3. No **SignalFx domínio e os URLs** secção, execute os seguintes passos:

    ![Domínio SignalFx e os URLs únicos de informações de início de sessão](./media/signalfx-tutorial/tutorial_signalfx_url.png)

    a. No **identificador** caixa de texto, escreva um URL: `https://api.signalfx.com/v1/saml/metadata`

    b. No **URL de resposta** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://api.signalfx.com/v1/saml/acs/<integration ID>`

    > [!NOTE] 
    > O valor anterior não é o valor real. Atualize o valor com o URL de resposta real, que é explicada mais tarde no tutorial.

4. Aplicação de SignalFx espera as asserções de SAML num formato específico. Configure as seguintes afirmações para esta aplicação. Pode gerir os valores destes atributos do **atributos de utilizador** secção na página de integração de aplicações. A seguinte captura de ecrã mostra um exemplo para este.   

    ![Configurar o início de sessão único](./media/signalfx-tutorial/tutorial_signalfx_attribute.png)

5. No **atributos de utilizador** secção no **de sessão único-** caixa de diálogo, configurar atributo token SAML, conforme mostrado na imagem e efetuar os seguintes passos:
    
    | Nome do Atributo | Valor do Atributo |
    | ------------------- | -------------------- |    
    | User.FirstName          | User.givenName |
    | User.email          | User.Mail |
    | PersonImmutableID       | user.userprincipalname    |
    | User.LastName       | User.Surname    |

    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único adicionar](./media/signalfx-tutorial/tutorial_attribute_04.png)

    ![Configurar Addattb de início de sessão único](./media/signalfx-tutorial/tutorial_attribute_05.png)

    b. No **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d. Deixe o **espaço de nomes** em branco.
    
    e. Clique em **OK**.
 
6. No **certificado de assinatura de SAML** secção, execute os seguintes passos: 

    ![A hiperligação de transferência do certificado](./media/signalfx-tutorial/tutorial_signalfx_certificate.png)

    a. Clique no botão Copiar para copiar **Url de metadados de Federação de aplicação** e cole-o bloco de notas.

    b. Clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

7. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/signalfx-tutorial/tutorial_general_400.png)

8. No **SignalFx configuração** secção, clique em **configurar SignalFx** para abrir **configurar início de sessão** janela. Copiar o **ID de entidade de SAML** do **secção de referência rápida.**

    ![Configuração de SignalFx](./media/signalfx-tutorial/tutorial_signalfx_configure.png) 

9. Início de sessão no site da sua empresa SignalFx como administrador.

10. No SignalFx, no superior, clique em **integrações** para abrir a página de integrações.

    ![Integração de SignalFx](./media/signalfx-tutorial/tutorial_signalfx_intg.png)

11. Clique em **do Azure Active Directory** mosaico em **serviços de início de sessão** secção.
 
    ![SignalFx saml](./media/signalfx-tutorial/tutorial_signalfx_saml.png)

12. Clique em **integração novo** e, no **instalar** separador execute os seguintes passos:
 
    ![SignalFx samlintgpage](./media/signalfx-tutorial/tutorial_signalfx_azure.png)

    a. No **nome** como o tipo de caixa de texto, um novo nome de integração, **OurOrgName SAML SSO**.

    b. Copiar o **integração ID** valor e de acréscimo com o **URL de resposta** como `https://api.signalfx.com/v1/saml/acs/<integration ID>` no **URL de resposta** caixa de texto de **SignalFx domínio e URLs** secção no portal do Azure.

    c. Clique em **carregar o ficheiro** para carregar o **certificado codificado Base64** transferido a partir do portal do Azure no **certificado** caixa de texto.

    d. No **URL do emissor** caixa de texto, cole o valor de **ID de entidade de SAML**, que copiou do portal do Azure.

    e. No **URL de metadados** caixa de texto, cole o **Url de metadados de Federação de aplicação** que copiou do portal do Azure.

    f. Clique em **Guardar**.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/signalfx-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/signalfx-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/signalfx-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/signalfx-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
  
### <a name="create-a-signalfx-test-user"></a>Criar um utilizador de teste SignalFx

O objetivo desta secção consiste em criar um utilizador chamado Britta Simon SignalFx. SignalFx suporta o aprovisionamento de just-in-time, que está por predefinição, ativada. Não há nenhum item de ação para si nesta secção. Um novo utilizador é criado durante a tentativa de aceder SignalFx se não existir ainda.

Quando um utilizador inicia sessão SignalFx de SAML SSO pela primeira vez, [equipa de suporte de SignalFx](mailto:kmazzola@signalfx.com) envia-as mensagens de correio eletrónico que contenham uma ligação que deve clicar para se autenticar. Isto acontece apenas a primeira vez que o utilizador inicia sessão; tentativas de início de sessão subsequentes não requeira validação de e-mail.

>[!Note]
>Se precisar de criar manualmente um utilizador, contacte [SignalFx a equipa de suporte](mailto:kmazzola@signalfx.com)

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso para SignalFx.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon a SignalFx, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **SignalFx**.

    ![A ligação de SignalFx na lista de aplicações](./media/signalfx-tutorial/tutorial_signalfx_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico SignalFx no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação de SignalFx.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/signalfx-tutorial/tutorial_general_01.png
[2]: ./media/signalfx-tutorial/tutorial_general_02.png
[3]: ./media/signalfx-tutorial/tutorial_general_03.png
[4]: ./media/signalfx-tutorial/tutorial_general_04.png

[100]: ./media/signalfx-tutorial/tutorial_general_100.png

[200]: ./media/signalfx-tutorial/tutorial_general_200.png
[201]: ./media/signalfx-tutorial/tutorial_general_201.png
[202]: ./media/signalfx-tutorial/tutorial_general_202.png
[203]: ./media/signalfx-tutorial/tutorial_general_203.png

