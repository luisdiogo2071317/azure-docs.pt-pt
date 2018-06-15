---
title: 'Tutorial: Integrar do Azure Active Directory com i gestora de vendas Remix | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e Remix do Gestor de vendas da I.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 89b5022c-0d5b-4103-9877-ddd32b6e1c02
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2018
ms.author: jeedes
ms.openlocfilehash: f681eb91c1e79eb42b572956dfab93e620489e74
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
ms.locfileid: "34342713"
---
# <a name="integrate-azure-active-directory-with-e-sales-manager-remix"></a>Integrar o Remix de Gestor de vendas E do Azure Active Directory

Neste tutorial, saiba como integrar o Azure Active Directory (Azure AD) com Remix do Gestor de vendas da I.

Através da integração do Azure AD com Remix do Gestor de vendas da I, obtenha as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Remix do Gestor de vendas da I.
- Pode permitir aos utilizadores obter a sessão iniciada no automaticamente i vendas Manager Remix (o início de sessão único, ou SSO) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central, o portal do Azure.

Para saber mais sobre a integração de aplicações SaaS com o Azure AD, consulte [que é o acesso a aplicações e início de sessão no Azure Active Directory?](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Remix do Gestor de vendas da I, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Uma subscrição i vendas Manager Remix SSO ativado

> [!NOTE]
> Quando o testar os passos neste tutorial, recomendamos que efetue *não* utilizar um ambiente de produção.

Para testar os passos neste tutorial, siga estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. 

O cenário descrito neste tutorial consiste em dois blocos modulares principais:

* A adição de Remix do Gestor de vendas da i de galeria
* Configurar e testar o Azure AD de sessão único-

## <a name="add-e-sales-manager-remix-from-the-gallery"></a>Adicionar Remix do Gestor de vendas da i a partir da Galeria
Para configurar a integração do Azure AD com Remix do Gestor de vendas da I, adicione Remix do Gestor de vendas da i na Galeria à sua lista de aplicações SaaS geridas efetuando o seguinte procedimento:

1. No [portal do Azure](https://portal.azure.com), no painel esquerdo, selecione **do Azure Active Directory**. 

    ![O botão do Azure Active Directory][1]

2. Selecione **aplicações empresariais** > **todas as aplicações**.

    ![A janela de "Aplicações da empresa"][2]
    
3. Para adicionar uma nova aplicação, selecione **nova aplicação** na parte superior da janela.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **Remix do Gestor de vendas da i**, selecione **Remix do Gestor de vendas da i** na lista de resultados e, em seguida, selecione **adicionar**.

    ![I Remix de Gestor de vendas na lista de resultados](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configurar e testar o Azure AD-início de sessão único com i vendas Manager Remix, com base num utilizador de teste chamado "Britta Simon."

Para início de sessão trabalhar, do Azure AD tem de identificar o utilizador Remix do Gestor de vendas da i e o respetivo homólogo no Azure AD. Por outras palavras, tem de ser estabelecida uma relação de ligação entre um utilizador do Azure AD e o mesmo utilizador na Remix do Gestor de vendas da I.

Para configurar e testar o Azure AD-início de sessão único com Remix do Gestor de vendas da I, conclua os blocos modulares nas cinco seguinte secções:

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Ativar o Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação Remix do Gestor de vendas da i efetuando o seguinte procedimento:

1. No portal do Azure, no **Remix do Gestor de vendas da i** página de integração de aplicações, selecione **de sessão único-**.

    ![A hiperligação "De sessão único"][4]

2. No **de sessão único-** janela, no **modo de início de sessão único** caixa, selecione **baseados em SAML início de sessão**.
 
    ![A janela "De sessão único"](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_samlbase.png)

3. Em **i vendas Manager Remix domínio e os URLs**, efetue o seguinte procedimento:

    ![I vendas Manager Remix domínio e os URLs únicos de informações de início de sessão](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_url.png)

    a. No **URL de início de sessão** caixa, escreva um URL no seguinte formato: *https://\<-com base-URL do servidor > /\<subdomínio > / esales pc*.

    b. No **identificador** caixa, escreva um URL no seguinte formato: *https://\<-com base-URL do servidor > /\<subdomínio > /*.

    c. Tenha em atenção o **identificador** valor para utilização posterior neste tutorial.
    
    > [!NOTE] 
    > Os valores anteriores não são reais. Atualize-as com o URL de início de sessão real e o identificador. Para obter os valores, contacte [equipa de suporte de cliente Remix Manager vendas i](mailto:esupport@softbrain.co.jp).

4. Em **certificado de assinatura de SAML**, selecione **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![A hiperligação de transferência do certificado (Base64)](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_certificate.png) 

5. Selecione o **ver e editar todos os outros atributos de utilizador** caixa de verificação e, em seguida, selecione o **emailaddress** atributo.
    
    ![A janela de atributos de utilizador](./media/active-directory-saas-esalesmanagerremix-tutorial/configure1.png)

    O **Editar atributo** é aberta a janela.

6. Copiar o **espaço de nomes** e **nome** valores. Gerar o valor no padrão de  *\<espaço de nomes > /\<nome >* e guardá-lo para utilização posterior neste tutorial.

    ![A janela Editar atributo](./media/active-directory-saas-esalesmanagerremix-tutorial/configure2.png)

7. Em **Remix configuração do Gestor de vendas i**, selecione **configurar i vendas Manager Remix**.

    ![Configuração de Remix do Gestor de vendas i](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_configure.png) 

    O **configurar início de sessão** é aberta a janela.

8. No **referência rápida** secção, copie o URL de início de sessão e o URL do serviço de início de sessão único SAML.

9. Selecione **Guardar**.

    ![O botão Guardar](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_400.png)

10. Inicie sessão na aplicação Remix do Gestor de vendas da i como administrador.

11. Na parte superior direita, selecione **ao Menu de administrador**.

    ![O comando "Para o administrador Menu"](./media/active-directory-saas-esalesmanagerremix-tutorial/configure4.png)

12. No painel esquerdo, selecione **definições do sistema** > **cooperação com o sistema externo**.

    ![A "Definições de sistema" e ligações de "Cooperação com o sistema externo"](./media/active-directory-saas-esalesmanagerremix-tutorial/configure5.png)
    
13. No **cooperação com o sistema externo** janela, selecione **SAML**.

    ![A janela "Cooperação com o sistema externo"](./media/active-directory-saas-esalesmanagerremix-tutorial/configure6.png)

14. Em **definição de autenticação SAML**, efetue o seguinte procedimento:

    ![A secção "Definição de autenticação SAML"](./media/active-directory-saas-esalesmanagerremix-tutorial/configure3.png)
    
    a. Selecione o **versão PC** caixa de verificação.
    
    b. No **item colaboração** secção, na lista pendente, selecione **e-mail**.

    c. No **item colaboração** caixa, cole o valor de afirmação que copiou anteriormente do portal do Azure (ou seja, **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**).

    d. No **emissor (ID de entidade)** caixa, colar o valor do identificador que copiou anteriormente do **i vendas Manager Remix domínio e os URLs** secção do portal do Azure.

    e. Para carregar o certificado transferido do portal do Azure, selecione **seleção de ficheiros**.

    f. No **URL de início de sessão do fornecedor de ID** caixa, cole o URL do serviço de início de sessão único SAML que copiou anteriormente no portal do Azure.

    g. No **URL de fim de sessão do fornecedor de identidade** caixa, cole o valor de URL de início de sessão que copiou anteriormente no portal do Azure.

    h. Selecione **definição concluída**.

> [!TIP]
> Como está a configurar a aplicação, pode ler uma versão das instruções anteriores no concisa o [portal do Azure](https://portal.azure.com). Depois de adicionar a aplicação no **do Active Directory** > **aplicações empresariais** secção, selecione o **Single Sign-On** separador e, em seguida, aceder a incorporado documentação o **configuração** secção na parte inferior. Para obter mais informações sobre a funcionalidade de documentação embedded, consulte [do Azure AD incorporado documentação]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

Nesta secção, criar o utilizador de teste Britta Simon no portal do Azure da seguinte forma:

![Criar um utilizador de teste do Azure AD][100]

1. No portal do Azure, no painel esquerdo, selecione **do Azure Active Directory**.

    ![A ligação do Azure Active Directory](./media/active-directory-saas-paloaltoadmin-tutorial/create_aaduser_01.png)

2. Para apresentar uma lista de utilizadores atuais, selecione **utilizadores e grupos** > **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/active-directory-saas-paloaltoadmin-tutorial/create_aaduser_02.png)

3. Na parte superior do **todos os utilizadores** janela, selecione **adicionar**.

    ![O botão de adição](./media/active-directory-saas-paloaltoadmin-tutorial/create_aaduser_03.png)
    
    O **utilizador** é aberta a janela.

4. No **utilizador** janela, efetue o seguinte procedimento:

    ![A janela de utilizador](./media/active-directory-saas-paloaltoadmin-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Selecione **Criar**.
 
### <a name="create-an-e-sales-manager-remix-test-user"></a>Criar um utilizador de teste Remix do Gestor de vendas da i

1. Iniciar sessão sua aplicação Remix do Gestor de vendas da i como administrador.

2. Selecione **ao Menu de administrador** no menu na parte superior direita.

    ![Configuração de Remix do Gestor de vendas i](./media/active-directory-saas-esalesmanagerremix-tutorial/configure4.png)

3. Selecione **definições da sua empresa** > **manutenção dos departamentos e funcionários**e, em seguida, selecione **funcionários registados**.

    ![No separador "Funcionários registado"](./media/active-directory-saas-esalesmanagerremix-tutorial/user1.png)

4. No **novo registo de empregado** secção, efetue o seguinte:
    
    ![O "registo do empregado novo" secção](./media/active-directory-saas-esalesmanagerremix-tutorial/user2.png)

    a. No **empregado nome** caixa, escreva o nome do utilizador (por exemplo, **Britta**).

    b. Conclua os campos obrigatórios restantes.
    
    c. Se ativar SAML, o administrador não pode iniciar sessão a partir da página de início de sessão. Conceder início de sessão privilégios de administrador para o utilizador selecionando o **início de sessão de administrador** caixa de verificação.

    d. Selecione **registo**.

5. No futuro, para iniciar sessão como administrador, inicie sessão como o utilizador que tem permissões de administrador e, em seguida, no canto superior direito, selecione **ao Menu de administrador**.

    ![O comando "Para o administrador Menu"](./media/active-directory-saas-esalesmanagerremix-tutorial/configure4.png)

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar utilizador Britta Simon para utilizar o Azure-início de sessão único, concedendo acesso para Remix do Gestor de vendas da I. Para tal, faça o seguinte: 

![Atribuir a função de utilizador][200] 

1. No portal do Azure, abra o **aplicações** vista, aceda ao **diretório** visualizar e, em seguida, selecione **aplicações empresariais** > **todos os aplicações**.

    ![O "As aplicações empresariais" e ligações de "Todas as aplicações"][201] 

2. No **aplicações** lista, selecione **Remix do Gestor de vendas da i**.

    ![A ligação Remix do Gestor de vendas da i](./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_esalesmanagerremix_app.png)  

3. No painel esquerdo, selecione **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Selecione **adicionar** e, em seguida, no **adicionar atribuição** painel, selecione **utilizadores e grupos**.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** janela, no **utilizadores** lista, selecione **Britta Simon**.

6. Selecione o **selecione** botão.

7. No **adicionar atribuição** janela, selecione **atribuir**.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, teste configuração do Azure AD único início de sessão utilizando o painel de acesso.

Quando seleciona o mosaico Remix do Gestor de vendas da i no painel de acesso, que deve ser iniciou sessão automaticamente à sua aplicação Remix do Gestor de vendas da I.

Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre a integração de aplicações SaaS no Azure Active Directory](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-esalesmanagerremix-tutorial/tutorial_general_203.png

