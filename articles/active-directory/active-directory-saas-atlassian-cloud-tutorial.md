---
title: "Tutorial: Integração do Azure Active Directory Atlassian nuvem | Microsoft Docs"
description: "Saiba como configurar o início de sessão entre o Azure Active Directory e Atlassian nuvem."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 729b8eb6-efc4-47fb-9f34-8998ca2c9545
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: jeedes
ms.openlocfilehash: db9e9c7ae8380612bac9d0aeaaaf6df78cba523f
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/13/2017
---
# <a name="tutorial-azure-active-directory-integration-with-atlassian-cloud"></a>Tutorial: Integração do Azure Active Directory Atlassian nuvem

Neste tutorial, irá aprender a integrar Atlassian Cloud com o Azure Active Directory (Azure AD).

Integrar Atlassian Cloud com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso à nuvem Atlassian.
- Pode ativar os seus utilizadores ter sessão iniciada automaticamente (de sessão único-) Atlassian nuvem com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central, o portal do Azure.

Para obter mais informações sobre o software como uma integração de aplicação de serviço (SaaS) com o Azure AD, consulte [que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Atlassian nuvem, terá dos seguintes itens:

- Uma subscrição do Azure AD.
- Para ativar Security Assertion Markup Language (SAML)-início de sessão único para produtos da nuvem Atlassian, terá de definir a segurança do Identity Manager. Saiba mais sobre [Identity Manager]( https://www.atlassian.com/enterprise/cloud/identity-manager).

> [!NOTE]
> Ao testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, siga estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito no tutorial consiste em dois blocos modulares principais:

* A adição de nuvem Atlassian de galeria
* Configurar e testar o Azure AD de sessão único-

## <a name="add-atlassian-cloud-from-the-gallery"></a>Adicionar Atlassian Cloud a partir da Galeria
Para configurar a integração da nuvem Atlassian com o Azure AD, adicione Atlassian nuvem na Galeria à sua lista de aplicações SaaS geridas efetuando o seguinte procedimento:

1. No [portal do Azure](https://portal.azure.com), no painel esquerdo, selecione o **do Azure Active Directory** botão. 

    ![O botão do Azure Active Directory][1]

2. Selecione **aplicações empresariais** > **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar uma aplicação, selecione **nova aplicação**.

    ![A aplicação"novo" botão][3]

4. Na caixa de pesquisa, escreva **Atlassian nuvem**, na lista de resultados, selecione **Atlassian nuvem**e, em seguida, selecione **adicionar**.

    ![Nuvem Atlassian na lista de resultados](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configurar e testar o Azure AD-início de sessão único com Atlassian nuvem, com base num utilizador de teste com o nome *Britta Simon*.

Para início de sessão trabalhar, do Azure AD tem de identificar o utilizador de nuvem Atlassian e o respetivo homólogo no Azure AD. Por outras palavras, tem de estabelecer uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado na nuvem Atlassian.

Para estabelecer a relação de ligação, atribua o como nuvem Atlassian *Username* o mesmo valor que é atribuído para o Azure AD *nome de utilizador*.

Para configurar e testar o Azure AD-início de sessão único com Atlassian nuvem, tem de concluir os blocos modulares nas secções seguintes.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação de nuvem Atlassian.

Para configurar o Azure AD-início de sessão único com Atlassian nuvem, efetue o seguinte:

1. No portal do Azure, no **Atlassian nuvem** painel de integração de aplicações, selecione **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** janela, no **modo de início de sessão único** caixa, selecione **baseados em SAML início de sessão**.
 
    ![Janela de início de sessão único](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_samlbase.png)

3. Para configurar a aplicação no modo iniciadas por IDP, em **Atlassian nuvem domínio e os URLs**, efetue o seguinte procedimento:

    ![Domínio Atlassian nuvem e os URLs únicos de informações de início de sessão](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url.png)
    
    a. No **identificador** caixa, escreva  **`https://auth.atlassian.com/saml/<unique ID>`** .
    
    b. No **URL de resposta** caixa, escreva  **`https://auth.atlassian.com/login/callback?connection=saml-<unique ID>`** .

    c. No **reencaminhamento estado** caixa, escreva um URL com a seguinte sintaxe:  **`https://<instancename>.atlassian.net`** .

4. Para configurar a aplicação no modo iniciado por SP, selecione o **Mostrar avançadas definições de URL** e, em seguida, no **iniciar sessão no URL** caixa, escreva um URL com a seguinte sintaxe:  **`https://<instancename>.atlassian.net`**  .

    ![Domínio Atlassian nuvem e os URLs únicos de informações de início de sessão](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url1.png)

    > [!NOTE] 
    > Os valores anteriores não são reais. Atualize-as com o identificador real, o URL de resposta e o início de sessão valores de URL. Pode obter os valores reais do ecrã de configuração de SAML do Atlassian nuvem. Vamos explicar os valores mais tarde no tutorial.

5. Em **certificado de assinatura de SAML**, selecione **Certificate(Base64)**e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![A hiperligação de transferência do certificado](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_certificate.png) 

6. A aplicação de nuvem Atlassian espera encontrar as asserções de SAML num formato específico, que necessita para adicionar mapeamentos de atributos personalizado à sua configuração de atributos de Token SAML. 

    Por predefinição, o **identificador de utilizador** valor está mapeado para user.userprincipalname. Altere este valor a mapear para user.mail. Também pode escolher qualquer outro valor apropriado, de acordo com a configuração da sua organização, mas, na maioria dos cenários, correio eletrónico deve funcionar.

    ![A hiperligação de transferência do certificado](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_attribute.png) 

7. Selecione **Guardar**.

    ![A configurar-início de sessão único guardar botão](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_400.png)

8. Para abrir o **configurar início de sessão** janela, no **Atlassian a configuração da nuvem** secção, selecione **configurar a nuvem de Atlassian**. 

9. No **referência rápida** secção, copie o **ID de entidade de SAML** e **único início de sessão no URL do serviço SAML**. 

    ![Configuração da nuvem Atlassian](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_configure.png) 

10. Para obter SSO configurado para a sua aplicação, inicie sessão portal do Atlassian com credenciais de administrador.

11. Aceda a **administração de sites Atlassian** > **organizações e segurança**. Se ainda não o fez, crie e nome da sua organização e, em seguida, no painel esquerdo, selecione **domínios**.

    ![Configurar o início de sessão único](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_06.png)

12. Selecione a forma como os que pretende verificar o domínio: **DNS** ou **HTTPS**.

    ![Configurar o início de sessão único](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_17.png)

13. Verificação de DNS, no **domínios** janela, selecione o **DNS** separador e, em seguida, efetue o seguinte:

    ![Configurar o início de sessão único](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_18.png)

    a. Para copiar o valor para o registo de texto (registo TXT), selecione **cópia**.

    b. Para adicionar um registo, vá para a página de definições no seu DNS.

    c. Selecione a opção para adicionar um novo registo e, em seguida, cole o valor que copiou no **domínios** janela para o **valor** campo. O registo de DNS também poderá fazem referência ao mesmo como **resposta** ou **Descrição**.

    d. O registo de DNS também pode incluir os seguintes campos:
    
    * No **tipo de registo** box, introduza **TXT**.
    * No **nome/anfitrião/Alias** caixa, deixe o valor predefinido (@ ou em branco).
    * No **tempo (TTL) em direto** box, introduza **86400**.
    
    e.  Guarde o registo.

14. Volte à **domínios** janela na administração da organização e, em seguida, selecione **verificar domínio**. No **domínio** caixa, escreva o seu nome de domínio e, em seguida, selecione **verificar domínio**.

    ![Configurar o início de sessão único](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_19.png)  

    > [!NOTE]
    > Porque pode demorar até 72 horas para as alterações de registo TXT entrem em vigor, não sabe imediato se a verificação de domínio foi concluída com êxito. Para ver o estado de verificação, verifique o **domínios** janela logo depois de concluir este procedimento. O estado atualizado será apresentado como *Verified*, conforme mostrado na imagem seguinte:
    > 
    > ![Configurar o início de sessão único](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_20.png)
    > 
    > 

15. Verificação de HTTPS, no **domínios** janela, selecione o **HTTPS** separador e, em seguida, efetue o seguinte:

    ![Configurar o início de sessão único](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_21.png)

    a. Para transferir o ficheiro HTML, selecione **transferir ficheiro**.

    b. Carregue o ficheiro HTML para o diretório de raiz do seu domínio.

16. Volte à **domínios** na administração da organização e da página selecionar **verificar domínio**. No **verificar domínio** janela, no **domínio** caixa, escreva o **nome de domínio**e, em seguida, selecione **verificar domínio**.

    ![Configurar o início de sessão único](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_22.png)

17. Se o processo de verificação pode localizar o ficheiro que carregou no diretório de raiz, o estado do domínio é atualizado para *Verified*, conforme mostrado aqui:

    ![Configurar o início de sessão único](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_23.png)

    > [!NOTE]
    > Para obter mais informações, consulte [verificação de domínio Atlassian](https://confluence.atlassian.com/cloud/domain-verification-873871234.html).

18. No painel esquerdo, selecione **SAML-início de sessão único**. Se ainda não o fez, subscreve para Atlassian do Identity Manager.

    ![Configurar o início de sessão único](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_11.png)

19. No **configuração SAML do adicionar** janela, efetue o seguinte procedimento:

    ![Configurar o início de sessão único](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_12.png)

    a. No **fornecedor de identidade ID de entidade** caixa, cole o ID de entidade SAML que copiou do portal do Azure.

    b. No **fornecedor de identidade SSO URL** caixa, cole o URL do serviço de início de sessão único SAML que copiou do portal do Azure.

    c. Abra o certificado transferido do portal do Azure num ficheiro. txt, copie o valor (sem o *começar certificado* e *certificado final* linhas) e, em seguida, cole-a no **X509 público certificado** caixa.
    
    d. Selecione **Guardar configuração**.
     
20. Para se certificar de que configurou os URLs corretos, Atualize as definições do Azure AD efetuando o seguinte procedimento:
  
    ![Configurar o início de sessão único](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_13.png)

    a. Na janela de SAML, copie o **SP identidade ID** e, em seguida, no portal do Azure, na nuvem Atlassian **domínios e URLs**, cole-a no **identificador** caixa.
    
    b. Na janela de SAML, copie o **URL do serviço de consumidor do SP asserção** e, em seguida, no portal do Azure, na nuvem Atlassian **domínios e URLs**, cole-a no **URL de resposta** caixa.  
        O URL de início de sessão é o URL de inquilino da sua nuvem Atlassian. 

    > [!NOTE]
    > Se tiver um cliente existente, depois de atualizar o **SP identidade ID** e **URL do serviço de consumidor do SP asserção** valores no portal do Azure, selecione **Sim, atualize a configuração**. Se tiver um cliente novo, pode ignorar este passo. 
    
21. No portal do Azure, selecione **guardar**.

    ![Configurar o início de sessão único](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_400.png)

> [!TIP]
> Como está a configurar a aplicação, pode ler uma versão das instruções anteriores no concisa o [portal do Azure](https://portal.azure.com). Depois de adicionar esta aplicação a partir de **do Active Directory** > **aplicações empresariais** secção, selecione o **Single Sign-On** separador e, em seguida, aceder a incorporados documentação do **configuração** secção na parte inferior da janela. Para obter mais informações, consulte [do Azure AD incorporado documentação]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

Nesta secção, criar o utilizador de teste Britta Simon no portal do Azure da seguinte forma:

   ![Criar um utilizador de teste do Azure AD][100]

1. No portal do Azure, no painel esquerdo, selecione o **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, selecione **utilizadores e grupos** > **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_02.png)

3. No **todos os utilizadores** janela, selecione **adicionar**.

    ![O botão de adição](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_03.png)

4. No **utilizador** janela, efetue o seguinte procedimento:

    ![A janela de utilizador](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Selecione **Criar**.
  
### <a name="create-an-atlassian-cloud-test-user"></a>Criar um utilizador de teste Atlassian nuvem

Para ativar o Azure AD aos utilizadores iniciar sessão Atlassian Cloud, aprovisionar as contas de utilizador manualmente na nuvem de Atlassian efetuando o seguinte procedimento:

1. No **administração** painel, selecione **utilizadores**.

    ![A ligação de utilizadores de nuvem Atlassian](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_14.png) 

2. Para criar um utilizador na nuvem Atlassian, selecione **convite utilizador**.

    ![Criar um utilizador de nuvem Atlassian](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_15.png) 

3. No **endereço de correio eletrónico** caixa, introduza o endereço de correio eletrónico do utilizador e, em seguida, atribua o acesso de aplicação. 

    ![Criar um utilizador de nuvem Atlassian](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_16.png)
 
4. Para enviar um convite de correio eletrónico ao utilizador, selecione **convidar utilizadores**.  
    É enviado um convite de correio eletrónico ao utilizador e, depois de a aceitar o convite, o utilizador é ativo no sistema. 

>[!NOTE] 
>Também pode em massa-criar utilizadores, selecionando o **em massa criar** clique no botão no **utilizadores** secção.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar utilizador Britta Simon para utilizar o Azure-início de sessão único, concedendo acesso à nuvem Atlassian. Para tal, faça o seguinte:

![Atribuir a função de utilizador][200] 

1. No portal do Azure, abra o **aplicações** ver, aceda à vista de diretório e, em seguida, selecione **aplicações empresariais** > **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. No **aplicações** lista, selecione **Atlassian nuvem**.

    ![A ligação de nuvem Atlassian na lista de aplicações](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_app.png)  

3. No painel esquerdo, selecione **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Selecione **adicionar** e, em seguida, no **adicionar atribuição** painel, selecione **utilizadores e grupos**.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** janela, no **utilizadores** lista, selecione **Britta Simon**.

6. No **utilizadores e grupos** janela, selecione **selecione**.

7. No **adicionar atribuição** janela, selecione **atribuir**.
    
### <a name="test-single-sign-on"></a>Teste o início de sessão único

Nesta secção, teste configuração do Azure AD único início de sessão utilizando o painel de acesso.

Quando seleciona o **Atlassian nuvem** na peça de mosaico do painel de acesso, deve ter sessão iniciada automaticamente à sua aplicação de nuvem Atlassian.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS no Azure Active Directory](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_203.png

