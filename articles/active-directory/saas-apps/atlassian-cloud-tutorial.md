---
title: 'Tutorial: Integração do Azure Active Directory com nuvem Atlassian | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e na Cloud da Atlassian.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 729b8eb6-efc4-47fb-9f34-8998ca2c9545
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: jeedes
ms.openlocfilehash: f0ad879bb084a8d3a50a0934557eae64621c0160
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39054263"
---
# <a name="tutorial-azure-active-directory-integration-with-atlassian-cloud"></a>Tutorial: Integração do Azure Active Directory com nuvem Atlassian

Neste tutorial, saiba como integrar Atlassian Cloud no Azure Active Directory (Azure AD).

Integrar Atlassian Cloud no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso à nuvem da Atlassian.
- Pode permitir que os utilizadores iniciar sessão automaticamente (início de sessão único) para a Cloud da Atlassian com as suas contas do Azure AD.
- Pode gerir as suas contas num local central, o portal do Azure.

Para obter mais informações sobre o software como uma integração de aplicação de serviço (SaaS) com o Azure AD, consulte [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com a nuvem da Atlassian, terá dos seguintes itens:

- Uma subscrição do Azure AD.
- Para ativar a Security Assertion Markup Language (SAML) início de sessão único para produtos da Atlassian Cloud, terá de definir a cópia de segurança do Identity Manager. Saiba mais sobre [Identity Manager]( https://www.atlassian.com/enterprise/cloud/identity-manager).

> [!NOTE]
> Quando testa os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, siga as seguintes recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste.
O cenário descrito no tutorial consiste em dois blocos de construção principais:

* Adicionar Atlassian Cloud a partir da Galeria
* Configuração e teste do Azure AD início de sessão único

## <a name="add-atlassian-cloud-from-the-gallery"></a>Adicionar Atlassian Cloud a partir da Galeria
Para configurar a integração da Atlassian nuvem com o Azure AD, adicione Atlassian Cloud partir da Galeria à sua lista de aplicações geridas do SaaS, fazendo o seguinte:

1. Na [portal do Azure](https://portal.azure.com), no painel esquerdo, selecione a **Azure Active Directory** botão.

    ![O botão do Azure Active Directory][1]

2. Selecione **aplicações empresariais** > **todas as aplicações**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar uma aplicação, selecione **nova aplicação**.

    ![O "novo aplicativo" botão][3]

4. Na caixa de pesquisa, escreva **Atlassian Cloud**, na lista de resultados, selecione **Atlassian Cloud**e, em seguida, selecione **Add**.

    ![Atlassian Cloud na lista de resultados](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configurar e testar o Azure AD início de sessão único com a nuvem da Atlassian, com base num utilizador de teste com o nome *Eduarda Almeida*.

Para o início de sessão único funcione, o Azure AD precisa identificar o utilizador de nuvem da Atlassian e sua contraparte no Azure AD. Em outras palavras, tem de estabelecer uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado na Cloud da Atlassian.

Para estabelecer a relação de ligação, atribuir, como a Cloud da Atlassian *nome de utilizador* o mesmo valor que é atribuído para o Azure AD *nome de utilizador*.

Para configurar e testar o Azure AD início de sessão único com Atlassian Cloud, terá de concluir os blocos de construção nas seções a seguir.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação na Cloud da Atlassian.

Para configurar o Azure AD início de sessão único com a nuvem da Atlassian, faça o seguinte:

1. No portal do Azure, no **Atlassian Cloud** painel de integração de aplicações, selecione **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Na **início de sessão único** janela, na **modo de início de sessão único** caixa, selecione **baseado em SAML logon**.

    ![Janela de início de sessão única](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_samlbase.png)

3. Para configurar a aplicação no modo de iniciado o IDP, em **Atlassian Cloud domínio e URLs**, efetue o seguinte procedimento:

    ![URLs de domínio na Cloud da Atlassian e único informações de início de sessão](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_url.png)
    
    a. Na **identificador** , escreva **`https://auth.atlassian.com/saml/<unique ID>`**.
    
    b. Na **URL de resposta** , escreva **`https://auth.atlassian.com/login/callback?connection=saml-<unique ID>`**.

    c. Na **estado de reencaminhamento** caixa, escreva um URL com a seguinte sintaxe: **`https://<instancename>.atlassian.net`**.

4. Para configurar a aplicação no modo iniciado por SP, selecione o **Mostrar definições de URL avançadas** e, em seguida, no **iniciar sessão no URL** caixa, escreva um URL com a seguinte sintaxe: **`https://<instancename>.atlassian.net`** .

    ![URLs de domínio na Cloud da Atlassian e único informações de início de sessão](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_url1.png)

    > [!NOTE]
    > Os valores anteriores não são reais. Atualize-as com o identificador real, o URL de resposta e o início de sessão em valores de URL. Pode obter os valores reais do ecrã de configuração de SAML do Cloud Atlassian. Explicamos os valores mais tarde no tutorial.

5. Sob **certificado de assinatura SAML**, selecione **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_certificate.png)

6. Seu aplicativo em nuvem da Atlassian espera encontrar as asserções SAML num formato específico, o que requer a adição de mapeamentos de atributos personalizado à sua configuração de atributos de Token SAML. 

    Por predefinição, o **identificador de utilizador** valor é mapeado para user.userprincipalname. Altere este valor a mapear para user.mail. Também pode escolher qualquer outro valor apropriado, de acordo com a configuração da sua organização, mas, na maioria dos casos, o e-mail deve funcionar.

    ![O link de download de certificado](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_attribute.png)

7. Selecione **Guardar**.

    ![A configurar início de sessão único botão Save](./media/atlassian-cloud-tutorial/tutorial_general_400.png)

8. Para abrir o **configurar o início de sessão** janela, na **configuração de Cloud da Atlassian** secção, selecione **configurar Cloud da Atlassian**.

9. Na **referência rápida** secção, copie a **ID de entidade de SAML** e **SAML único início de sessão no URL do serviço**.

    ![Configuração de Cloud da Atlassian](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_configure.png)

10. Para obter SSO configurado para a sua aplicação, inicie sessão no portal da Atlassian com credenciais de administrador.

11. Terá de verificar o seu domínio antes de ir para configurar o início de sessão único. Para obter mais informações, consulte [verificação de domínio da Atlassian](https://confluence.atlassian.com/cloud/domain-verification-873871234.html) documento.

12. No painel esquerdo, selecione **SAML início de sessão único**. Se ainda não o fez, inscreva-se para Atlassian Identity Manager.

    ![Configurar o início de sessão único](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_11.png)

13. Na **configuração SAML do adicionar** janela, efetue o seguinte procedimento:

    ![Configurar o início de sessão único](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_12.png)

    a. Na **ID de entidade do fornecedor de identidade** caixa, cole o ID de entidade SAML que copiou do portal do Azure.

    b. Na **fornecedor de identidade SSO URL** caixa, cole o URL do serviço de início de início de sessão único SAML que copiou do portal do Azure.

    c. Abra o certificado transferido do portal do Azure num arquivo. txt, copie o valor (sem o *Begin Certificate* e *End Certificate* linhas) e, em seguida, cole-a no **X509 pública certificado** caixa.
    
    d. Selecione **Guardar configuração**.
     
14. Para garantir que tem de configurar os URLs corretos, Atualize as definições do Azure AD efetuando o seguinte procedimento:

    ![Configurar o início de sessão único](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_13.png)

    a. Na janela de SAML, copie o **ID de identidade de SP** e, em seguida, no portal do Azure, em nuvem da Atlassian **domínio e URLs**, cole-a no **identificador** caixa.
    
    b. Na janela de SAML, copie os **URL do serviço de consumidor de asserção de SP** e, em seguida, no portal do Azure, em nuvem da Atlassian **domínio e URLs**, cole-a no **URL de resposta** caixa. O URL de início de sessão é o URL de inquilino da sua Atlassian Cloud.

    > [!NOTE]
    > Se for um cliente existente, depois de atualizar o **ID de identidade de SP** e **URL do serviço de consumidor de asserção de SP** valores no portal do Azure, selecione **Sim, atualizar a configuração do**. Se for um novo cliente, pode ignorar este passo.
    
15. No portal do Azure, selecione **guardar**.

    ![Configurar o início de sessão único](./media/atlassian-cloud-tutorial/tutorial_general_400.png)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

Nesta secção, vai criar o utilizador de teste Eduarda Almeida no portal do Azure, fazendo o seguinte:

   ![Criar um utilizador de teste do Azure AD][100]

1. No portal do Azure, no painel esquerdo, selecione o **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/atlassian-cloud-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, selecione **utilizadores e grupos** > **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/atlassian-cloud-tutorial/create_aaduser_02.png)

3. Na **todos os utilizadores** janela, selecione **Add**.

    ![Botão Adicionar](./media/atlassian-cloud-tutorial/create_aaduser_03.png)

4. Na **utilizador** janela, efetue o seguinte procedimento:

    ![A janela de utilizador](./media/atlassian-cloud-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Selecione **Criar**.

### <a name="create-an-atlassian-cloud-test-user"></a>Criar um utilizador de teste na Cloud da Atlassian

Para ativar a utilizadores do Azure AD iniciar sessão na Cloud da Atlassian, Aprovisione as contas de utilizador manualmente na Cloud de Atlassian efetuando o seguinte procedimento:

1. Na **Administration** painel, selecione **utilizadores**.

    ![A ligação de utilizadores de nuvem da Atlassian](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_14.png)

2. Para criar um utilizador na Cloud da Atlassian, selecione **utilizador de convite**.

    ![Criar um utilizador de nuvem da Atlassian](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_15.png)

3. Na **endereço de E-Mail** caixa, introduza o endereço de e-mail do utilizador e, em seguida, atribua o acesso de aplicação.

    ![Criar um utilizador de nuvem da Atlassian](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_16.png)

4. Para enviar um convite por e-mail ao utilizador, selecione **convidar utilizadores**. Um convite por e-mail é enviado para o utilizador e, depois de aceitar o convite, o utilizador está ativo no sistema.

>[!NOTE]
>Também pode efetuar em massa-criar utilizadores, selecionando o **criar em massa** botão no **utilizadores** secção.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar utilizador Eduarda Almeida para utilizar o Azure início de sessão único ao conceder acesso à nuvem da Atlassian. Para tal, faça o seguinte:

![Atribuir a função de utilizador][200]

1. No portal do Azure, abra a **aplicativos** ver, vá para a vista de diretório e, em seguida, selecione **aplicações empresariais** > **todos os aplicativos**.

    ![Atribuir utilizador][201]

2. Na **aplicativos** lista, selecione **Atlassian Cloud**.

    ![A ligação de nuvem da Atlassian na lista de aplicações](./media/atlassian-cloud-tutorial/tutorial_atlassiancloud_app.png)

3. No painel esquerdo, selecione **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Selecione **Add** e, em seguida, no **adicionar atribuição** painel, selecione **utilizadores e grupos**.

    ![O painel Adicionar atribuição][203]

5. Na **utilizadores e grupos** janela, na **utilizadores** lista, selecione **Eduarda Almeida**.

6. Na **utilizadores e grupos** janela, selecione **selecione**.

7. Na **adicionar atribuição** janela, selecione **atribuir**.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar seu única início de sessão em configuração do Azure AD utilizando o painel de acesso.

Quando seleciona a **Atlassian Cloud** mosaico no painel de acesso, deve ter sessão iniciada automaticamente para seu aplicativo em nuvem da Atlassian.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/atlassian-cloud-tutorial/tutorial_general_01.png
[2]: ./media/atlassian-cloud-tutorial/tutorial_general_02.png
[3]: ./media/atlassian-cloud-tutorial/tutorial_general_03.png
[4]: ./media/atlassian-cloud-tutorial/tutorial_general_04.png

[100]: ./media/atlassian-cloud-tutorial/tutorial_general_100.png

[200]: ./media/atlassian-cloud-tutorial/tutorial_general_200.png
[201]: ./media/atlassian-cloud-tutorial/tutorial_general_201.png
[202]: ./media/atlassian-cloud-tutorial/tutorial_general_202.png
[203]: ./media/atlassian-cloud-tutorial/tutorial_general_203.png
