---
title: "Tutorial: Integração do Azure Active Directory com EBSCO | Microsoft Docs"
description: "Saiba como configurar o início de sessão entre o Azure Active Directory e EBSCO."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 144f7f65-69e9-4016-a151-fe1104fd6ba8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: jeedes
ms.openlocfilehash: ea7fe09c31d88cf2095b3a3777b6b1f9feb8df46
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
---
# <a name="tutorial-azure-active-directory-integration-with-ebsco"></a>Tutorial: Integração do Azure Active Directory com EBSCO

Neste tutorial, irá aprender a integrar EBSCO com o Azure Active Directory (Azure AD).

Integrar EBSCO com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao EBSCO.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para EBSCO (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com EBSCO, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um EBSCO início de sessão único subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar EBSCO a partir da Galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-ebsco-from-the-gallery"></a>Adicionar EBSCO a partir da Galeria
Para configurar a integração de EBSCO com o Azure AD, terá de adicionar EBSCO a partir da Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar EBSCO a partir da galeria, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **EBSCO**, selecione **EBSCO** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![EBSCO na lista de resultados](./media/active-directory-saas-ebsco-tutorial/tutorial_ebsco_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com EBSCO com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo no EBSCO é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no EBSCO tem de ser estabelecida.

Para configurar e testar o Azure AD-início de sessão único com EBSCO, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste EBSCO](#create-an-ebsco-test-user)**  -pode automatizar EBSCOhost aprovisionamento/personalização de utilizador. EBSCO suporta Just-In-Time o aprovisionamento de utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação EBSCO.

**Para configurar o Azure AD-início de sessão único com EBSCO, execute os seguintes passos:**

1. No portal do Azure, no **EBSCO** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/active-directory-saas-ebsco-tutorial/tutorial_ebsco_samlbase.png)

3. No **EBSCO domínio e os URLs** secção, execute os seguintes passos, se pretender configurar a aplicação no **IDP** iniciada modo:

    ![Domínio EBSCO e os URLs únicos de informações de início de sessão](./media/active-directory-saas-ebsco-tutorial/tutorial_ebsco_url.png)

    No **identificador** caixa de texto, escreva um URL:`pingsso.ebscohost.com`

4. Verifique **Mostrar avançadas definições de URL** e executar o passo seguinte, se pretender configurar a aplicação no **SP** iniciada modo:

    ![Domínio EBSCO e os URLs únicos de informações de início de sessão](./media/active-directory-saas-ebsco-tutorial/tutorial_ebsco_url1.png)

    No **URL de início de sessão** caixa de texto, escreva um URL a utilizar o padrão do seguinte:`http://search.ebscohost.com/login.aspx?authtype=sso&custid=<unique EBSCO customer ID>&profile=<profile ID>`
     
    > [!NOTE] 
    > O valor do URL de início de sessão não é real. Atualize o valor com o URL de início de sessão real. Contacte [equipa de suporte de cliente EBSCO](mailto:sso@ebsco.com) para obter o valor. 

    Nã **elementos exclusivos:**  

    Nã **Custid** = Enter EBSCO ao cliente um ID exclusivo 

    Nã **perfil** = clientes podem personalizar a ligação para direcionar os utilizadores a um perfil específico (consoante o que eles comprar EBSCO). Podem introduzir um ID de perfil específico. Os IDs principais são eds (serviço de deteção EBSCO) e ehost (EBSOCOhost bases de dados). Instruções para o mesmo recebem [aqui](https://help.ebsco.com/interfaces/EBSCOhost/EBSCOhost_FAQs/How_do_I_set_up_direct_links_to_EBSCOhost_profiles_and_or_databases#profile).

5. Aplicação de EBSCO espera as asserções de SAML num formato específico. Configure as seguintes afirmações para esta aplicação. Pode gerir os valores destes atributos a partir de "**atributos de utilizador**" secção na página de integração de aplicações. A seguinte captura de ecrã mostra um exemplo para este.
    
    ![Configurar o início de sessão único](./media/active-directory-saas-ebsco-tutorial/tutorial_ebsco_attribute.png)

    > [!Note]
    > O **nome** atributo é obrigatório e está mapeada com **identificador de utilizador** na aplicação EBSCO. Isto é adicionado por predefinição por isso não terá de adicionar este manualmente.
    
6. No **atributos de utilizador** secção no **de sessão único-** caixa de diálogo, configurar atributos token SAML, conforme mostrado na imagem acima e execute os seguintes passos:
    
    | Nome do Atributo | Valor do Atributo |
    | ---------------| --------------- |    
    | FirstName   | user.givenname |
    | LastName   | user.surname |
    | E-mail   | user.mail |

    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único](./media/active-directory-saas-ebsco-tutorial/tutorial_officespace_04.png)

    ![Configurar o início de sessão único](./media/active-directory-saas-ebsco-tutorial/tutorial_attribute_05.png)
    
    b. No **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.
    
    c. Do **valor** lista, digite o valor de atributo apresentado para essa linha.
    
    d. Clique em **Ok**

7. No **certificado de assinatura de SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![A hiperligação de transferência do certificado](./media/active-directory-saas-ebsco-tutorial/tutorial_ebsco_certificate.png) 

8. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-ebsco-tutorial/tutorial_general_400.png)
    
9. Para configurar o início de sessão único em **EBSCO** lado, terá de enviar o transferido **XML de metadados** para [EBSCO suporta equipa](mailto:sso@ebsco.com). Se definir esta definição para que a ligação de SAML SSO corretamente em ambos os lados.

> [!TIP]
> Pode agora ler estas instruções dentro de uma versão concisa o [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir do **do Active Directory > aplicações da empresa** secção, basta clicar no **Single Sign-On** separador e aceder à documentação do embedded através de **configuração** secção na parte inferior. Pode ler mais sobre a funcionalidade de documentação incorporados aqui: [do Azure AD incorporado documentação]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/active-directory-saas-ebsco-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/active-directory-saas-ebsco-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/active-directory-saas-ebsco-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/active-directory-saas-ebsco-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-an-ebsco-test-user"></a>Criar um utilizador de teste EBSCO

No caso de EBSCO, aprovisionamento de utilizadores é automático.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

Azure AD transmite os dados necessários para aplicação EBSCO. Aprovisionamento de utilizadores do EBSCO pode ser automática ou necessitam de um formulário de uso individual. Depende se o cliente tem inúmeras pré-existente EBSCOhost contas com as definições pessoais guardadas. O mesmo pode ser explicado com o [EBSCO suporta equipa](mailto:sso@ebsco.com) durante a implementação. Qualquer forma, o cliente não é necessário criar as contas de EBSCOhost antes de testar.

   >[!Note]
   >Pode automatizar EBSCOhost aprovisionamento/personalização de utilizador. Contacte [EBSCO suporta equipa](mailto:sso@ebsco.com) sobre Just-In-Time o aprovisionamento de utilizador. 
 
### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso para EBSCO.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon a EBSCO, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **EBSCO**.

    ![A ligação EBSCO na lista de aplicações](./media/active-directory-saas-ebsco-tutorial/tutorial_ebsco_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Teste o início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

1. Quando clica no mosaico EBSCO no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação de EBSCO.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](active-directory-saas-access-panel-introduction.md).

2. Depois de iniciar a sessão na aplicação, clique em de **sessão** botão no canto superior direito.

    ![O início de sessão EBSCO na lista de aplicações](./media/active-directory-saas-ebsco-tutorial/tutorial_ebsco_signin.png)
 
3. Receberá uma única linha de emparelhar o início de sessão institutional/SAML com um **associar a sua conta de MyEBSCOhost existente à sua conta instituição agora** ou **criar uma nova conta de MyEBSCOhost e ligá-lo para o conta de instituição**. A conta é utilizada para personalização na aplicação EBSCOhost. Selecione a opção **criar uma nova conta** e verá que o formato para personalização é previamente concluído com os valores da resposta saml, conforme mostrado na captura de ecrã abaixo. Clique em **'Continuar'** para guardar esta seleção.
    
     ![O utilizador EBSCO na lista de aplicações](./media/active-directory-saas-ebsco-tutorial/tutorial_ebsco_user.png)

4. Depois de concluir o programa de configuração acima, limpe início de sessão e a cache de cookies/novamente. Não terá manualmente início de sessão novamente e as definições de personalização são memorizadas

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ebsco-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ebsco-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ebsco-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ebsco-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ebsco-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ebsco-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ebsco-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ebsco-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ebsco-tutorial/tutorial_general_203.png

