---
title: "Tutorial: Integração do Azure Active Directory com SuccessFactors | Microsoft Docs"
description: "Saiba como configurar o início de sessão entre o Azure Active Directory e SuccessFactors."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 32bd8898-c2d2-4aa7-8c46-f1f5c2aa05f1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: jeedes
ms.openlocfilehash: b9545599b4ac02927c38931777a3cee623a4e940
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/13/2017
---
# <a name="tutorial-azure-active-directory-integration-with-successfactors"></a>Tutorial: Integração do Azure Active Directory com SuccessFactors

Neste tutorial, irá aprender a integrar SuccessFactors com o Azure Active Directory (Azure AD).

Integrar SuccessFactors com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao SuccessFactors.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para SuccessFactors (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com SuccessFactors, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um SuccessFactors início de sessão único subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar SuccessFactors a partir da Galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-successfactors-from-the-gallery"></a>Adicionar SuccessFactors a partir da Galeria
Para configurar a integração de SuccessFactors com o Azure AD, terá de adicionar SuccessFactors a partir da Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar SuccessFactors a partir da galeria, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **SuccessFactors**, selecione **SuccessFactors** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![SuccessFactors na lista de resultados](./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com SuccessFactors com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo no SuccessFactors é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no SuccessFactors tem de ser estabelecida.

No SuccessFactors, atribua o valor do **nome de utilizador** no Azure AD como o valor a **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD-início de sessão único com SuccessFactors, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste SuccessFactors](#create-a-successfactors-test-user)**  - para ter um homólogo de Britta Simon SuccessFactors que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação SuccessFactors.

**Para configurar o Azure AD-início de sessão único com SuccessFactors, execute os seguintes passos:**

1. No portal do Azure, no **SuccessFactors** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_samlbase.png)

3. No **SuccessFactors domínio e os URLs** secção, execute os seguintes passos:

    ![Domínio SuccessFactors e os URLs únicos de informações de início de sessão](./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_url.png)

    a. No **URL de início de sessão** caixa de texto, escreva um URL a utilizar o padrão do seguinte:
    | |
    |--|
    | `https://<companyname>.successfactors.com/<companyname>`|
    | `https://<companyname>.sapsf.com/<companyname>`|
    | `https://<companyname>.successfactors.eu/<companyname>`|
    | `https://<companyname>.sapsf.eu`|

    b. No **identificador** caixa de texto, escreva um URL a utilizar o padrão do seguinte:
    | |
    |--|
    | `https://www.successfactors.com/<companyname>`|
    | `https://www.successfactors.com`|
    | `https://<companyname>.successfactors.eu`|
    | `https://www.successfactors.eu/<companyname>`|
    | `https://<companyname>.sapsf.com`|
    | `https://hcm4preview.sapsf.com/<companyname>`|
    | `https://<companyname>.sapsf.eu`|
    | `https://www.successfactors.cn`|
    | `https://www.successfactors.cn/<companyname>`|

    c. No **URL de resposta** caixa de texto, escreva um URL a utilizar o padrão do seguinte:
    | |
    |--|
    | `https://<companyname>.successfactors.com/<companyname>`|
    | `https://<companyname>.successfactors.com`|
    | `https://<companyname>.sapsf.com/<companyname>`|
    | `https://<companyname>.sapsf.com`|
    | `https://<companyname>.successfactors.eu/<companyname>`|
    | `https://<companyname>.successfactors.eu`|
    | `https://<companyname>.sapsf.eu`|
    | `https://<companyname>.sapsf.eu/<companyname>`|
    | `https://<companyname>.sapsf.cn`|
    | `https://<companyname>.sapsf.cn/<companyname>`|
         
    > [!NOTE] 
    > Estes valores não estiverem reais. Atualize estes valores com o identificador de real, a URL de resposta e o URL de início de sessão. Contacte [equipa de suporte de cliente SuccessFactors](https://www.successfactors.com/en_us/support.html) para obter estes valores. 

4. No **certificado de assinatura de SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![A hiperligação de transferência do certificado](./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-successfactors-tutorial/tutorial_general_400.png)
    
6. No **SuccessFactors configuração** secção, clique em **configurar SuccessFactors** para abrir **configurar início de sessão** janela. Copiar o **Sign-Out URL, o ID de entidade de SAML e o único início de sessão no URL do serviço SAML** do **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_configure.png) 

7. Numa janela do browser web diferente, inicie sessão no seu **portal de administração de SuccessFactors** como administrador.
    
8. Visite **segurança da aplicação** e nativa **início de sessão único na funcionalidade**. 

9. Coloque qualquer valor no **repor Token** e clique em **guardar Token** para ativar SAML SSO.
   
    ![Configurar o início de sessão único no lado de aplicação][11]

    > [!NOTE] 
    > Este valor é utilizado como o comutador /. Se qualquer valor for guardado, SAML SSO está ON. Se um valor em branco é guardado SAML SSO está OFF.

10. Nativo para baixo captura de ecrã e efetuar as seguintes ações:
   
    ![Configurar o início de sessão único no lado de aplicação][12]
   
    a. Selecione o **SAML v2 SSO** botão de opção
   
    b. Definir o **SAML Asserting terceiros nome**(por exemplo, o emissor SAML + o nome da empresa).
   
    c. No **URL do emissor** caixa de texto, cole o **ID de entidade de SAML** valor que copiou do portal do Azure.
   
    d. Selecione **resposta (cliente gerado/IdP/AP)** como **exigir assinatura obrigatória**.
   
    e. Selecione **ativada** como **ativar SAML sinalizador**.
   
    f. Selecione **não** como **assinatura do pedido de início de sessão (SF gerado/SP/RP)**.
   
    g. Selecione **Browser/Post perfil** como **perfil SAML**.
   
    h. Selecione **não** como **impor período válido de certificado**.
   
    posso. Copie o conteúdo do ficheiro de certificado transferido a partir do portal do Azure e, em seguida, cole-o para o **SAML verificar certificado** caixa de texto.

    > [!NOTE] 
    > O conteúdo de certificado tem de ter começar etiquetas de certificado do certificado e de fim.

11. Navegue para SAML V2 e, em seguida, execute os seguintes passos:
   
    ![Configurar o início de sessão único no lado de aplicação][13]
   
    a. Selecione **Sim** como **suporta iniciado por SP Global terminar**.
   
    b. No **Global URL do serviço fim de sessão (LogoutRequest destino)** caixa de texto, cole o **Sign-Out URL** valor que copiou formulário do portal do Azure.
   
    c. Selecione **não** como **requerem sp tem de encriptar todos os NameID elemento**.
   
    d. Selecione **não especificado** como **NameID formato**.
   
    e. Selecione **Sim** como **ativar SP2 iniciada pelo início de sessão (AuthnRequest)**.
   
    f. No **pedido de envio como emissor de toda a empresa** caixa de texto, colar **único início de sessão no URL do serviço SAML** valor que copiou do portal do Azure.

12. Execute estes passos, se pretender que os nomes de utilizador de início de sessão sensível a maiúsculas e minúsculas.
   
    ![Configurar o início de sessão único][29]
    
    a. Visite **definições da empresa**(perto da parte inferior).
   
    b. Selecione a caixa de verificação junto **ativar o nome de utilizador de caso de não-sensíveis**.
   
    c.Click **guardar**.
   
    > [!NOTE] 
    > Se tentar ativar esta opção, o sistema verifica se cria um nome de início de sessão SAML duplicado. Por exemplo, se o cliente tem nomes de utilizador User1 e user1. Colocar ausente sensibilidade às maiúsculas e minúsculas torna estas duplicados. O sistema dá-lhe uma mensagem de erro e não ativar a funcionalidade. O cliente tem de alterar um dos nomes de utilizador, pelo que está escrito diferentes.

> [!TIP]
> Pode agora ler estas instruções dentro de uma versão concisa o [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir do **do Active Directory > aplicações da empresa** secção, basta clicar no **Single Sign-On** separador e aceder à documentação do embedded através de **configuração** secção na parte inferior. Pode ler mais sobre a funcionalidade de documentação incorporados aqui: [do Azure AD incorporado documentação]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/active-directory-saas-successfactors-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/active-directory-saas-successfactors-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/active-directory-saas-successfactors-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/active-directory-saas-successfactors-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-successfactors-test-user"></a>Criar um utilizador de teste SuccessFactors

Para permitir que os utilizadores do Azure AD iniciem sessão nos SuccessFactors, têm de ser aprovisionados para SuccessFactors.  
No caso de SuccessFactors, o aprovisionamento é uma tarefa manual.

Para obter os utilizadores criados no SuccessFactors, terá de contactar o [SuccessFactors suporta equipa](https://www.successfactors.com/en_us/support.html).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso para SuccessFactors.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon a SuccessFactors, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **SuccessFactors**.

    ![A ligação de SuccessFactors na lista de aplicações](./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Teste o início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico SuccessFactors no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação de SuccessFactors.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_04.png

[11]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_07.png
[12]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_08.png
[13]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_09.png
[14]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_05.png
[15]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_06.png
[29]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_10.png

[100]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_203.png

