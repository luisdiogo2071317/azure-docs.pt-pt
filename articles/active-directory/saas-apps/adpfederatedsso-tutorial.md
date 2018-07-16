---
title: 'Tutorial: Integração do Azure Active Directory com ADP | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e ADP.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7be5331b-0481-48f7-9d6b-619dfec657e1
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/29/2018
ms.author: jeedes
ms.openlocfilehash: 75b84c2856373126ceba0fc536e41d270f4d2d05
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39048784"
---
# <a name="tutorial-azure-active-directory-integration-with-adp"></a>Tutorial: Integração do Azure Active Directory com ADP

Neste tutorial, saiba como integrar ADP com o Azure Active Directory (Azure AD).

Integrar ADP no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao ADP.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para ADP (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com ADP, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Uma subscrição de ADP ativada

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando ADP da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-adp-from-the-gallery"></a>Adicionando ADP da Galeria
Para configurar a integração do ADP com o Azure AD, terá de adicionar ADP a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar ADP a partir da galeria, execute os seguintes passos:**

1.  Inicie sessão no seu ambiente de fornecedor de identidade do Microsoft Azure como administrador.

2. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

3. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
4. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

5. Na caixa de pesquisa, escreva **ADP**, selecione **ADP** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![ADP na lista de resultados](./media/adpfederatedsso-tutorial/tutorial_adp_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com ADP com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no ADP a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no ADP deve ser estabelecido.

ADP, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com ADP, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste ADP](#create-an-adp-test-user)**  - para ter um equivalente da Eduarda Almeida na ADP que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo ADP.

**Para configurar o Azure AD início de sessão único com ADP, execute os seguintes passos:**

1. No portal do Azure, sobre o **ADP** página de integração de aplicação, clique em **separador Propriedades** e execute os seguintes passos: 

    ![Propriedades de início de sessão único](./media/adpfederatedsso-tutorial/tutorial_adp_prop.png)

    a. Definir o **ativada para os utilizadores para início de sessão** campo valor a **Sim**.

    b. Copiar o **URL de acesso de utilizador** e tiver colá-lo na **seção URL de início de sessão em configurar**, que é explicado mais tarde no tutorial.

    c. Definir o **atribuição utiliz. necessária** campo valor a **Sim**.

    d. Definir o **visível aos usuários** campo valor a **não**.

2. Clique em **início de sessão único** nos **ADP** página de integração de aplicativo.

    ![Configurar a ligação de início de sessão única][4]

3. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/adpfederatedsso-tutorial/tutorial_adp_samlbase.png)

4. Sobre o **ADP domínio e URLs** secção, execute os seguintes passos:

    ![ADP domínio e URLs únicas início de sessão em informações](./media/adpfederatedsso-tutorial/tutorial_adp_url.png)

    Na **identificador** caixa de texto, escreva um URL: `https://fed.adp.com` 
    
5. O aplicativo ADP espera que as asserções SAML num formato específico, o que requer a adição de mapeamentos de atributos personalizado à sua configuração de atributos de token SAML. Captura de ecrã seguinte mostra um exemplo disso. O nome de afirmação sempre será **"PersonImmutableID"** e o valor que podemos ter mapeado **employeeid**. 

    Aqui o mapeamento de utilizador do Azure AD para ADP será feito sobre o **employeeid** , mas pode mapeá-la para um valor diferente com base nas suas definições de aplicação. Pelo que deve trabalho com [equipa de suporte de ADP](https://www.adp.com/contact-us/overview.aspx) primeiro para utilizar o identificador correto de um usuário e mapear esse valor com o **"PersonImmutableID"** de afirmação.

    ![Configurar o início de sessão único](./media/adpfederatedsso-tutorial/tutorial_adp_attribute.png)

6. No **atributos de utilizador** secção sobre o **início de sessão único** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem e execute os seguintes passos:
    
    | Nome do Atributo | Valor do Atributo |
    | ------------------- | -------------------- |    
    | PersonImmutableID | User.employeeid |
    
    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único](./media/adpfederatedsso-tutorial/tutorial_attribute_04.png)

    ![Configurar o início de sessão único](./media/adpfederatedsso-tutorial/tutorial_attribute_05.png)

    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.
    
    d. Clique em **OK**.

    > [!NOTE] 
    > Antes de poder configurar a asserção de SAML, terá de contactar o seu [equipa de suporte de ADP](https://www.adp.com/contact-us/overview.aspx) e pedir o valor do atributo de identificador exclusivo para o seu inquilino. Vai precisar deste valor para configurar a afirmação personalizada para a sua aplicação. 

7. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/adpfederatedsso-tutorial/tutorial_adp_certificate.png) 

8. Para configurar o início de sessão único em **ADP** lado, terá de carregar o transferido **XML de metadados** sobre o [site ADP](https://adpfedsso.adp.com/public/login/index.fcc).

> [!NOTE]  
> Este processo pode demorar alguns dias. 

### <a name="configure-your-adp-services-for-federated-access"></a>Configurar o seu serviço ou serviços de ADP para acesso federado

>[!Important]
> Os seus funcionários que precisam de acesso federado aos seus serviços ADP tem de ser atribuídos para a aplicação de serviço ADP e, em seguida, os utilizadores devem ser reatribuídos para o serviço de ADP específico.
Após a receção de confirmação do seu representante de ADP, configure os utilizadores de serviços e atribuir/gerir ADP para controlar o acesso de utilizador para o serviço de ADP específico.

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **ADP**, selecione **ADP** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![ADP na lista de resultados](./media/adpfederatedsso-tutorial/tutorial_adp_addservicegallery.png)

5. No portal do Azure, no seu **ADP** página de integração de aplicação, clique em **separador Propriedades** e execute os seguintes passos:  

    ![Linkedproperties de início de sessão único](./media/adpfederatedsso-tutorial/tutorial_adp_linkedproperties.png)

    a.  Definir o **ativada para os utilizadores para início de sessão** campo valor a **Sim**.

    b.  Definir o **atribuição utiliz. necessária** campo valor a **Sim**.

    c.  Definir o **visível aos usuários** campo valor a **Sim**.

6. Clique em **início de sessão único** nos **ADP** página de integração de aplicativo.

    ![Configurar a ligação de início de sessão única][4]

7. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **início de sessão ligado em**. para ligar a sua aplicação à **ADP**.

    ![Início de sessão único ligado](./media/adpfederatedsso-tutorial/tutorial_adp_linked.png)

8. Navegue para o **URL de início de sessão de configurar** secção, execute os seguintes passos:

    ![Prop de início de sessão único](./media/adpfederatedsso-tutorial/tutorial_adp_linkedsignon.png)
                                                              
    a. Colar o **URL de acesso de utilizador**, que copiou acima **separador Propriedades** (a partir da aplicação de ADP principal).
                                                             
    b. Seguem-se a 5 as aplicações que suportam diferentes **URLs de estado de reencaminhamento**. Terá de acrescentar o adequado **URL de estado de reencaminhamento** manualmente para o valor para o aplicativo em particular a **URL de acesso de utilizador**.
    
    * **Agora força de trabalho de ADP**
        
        `<User access URL>?relaystate=https://fed.adp.com/saml/fedlanding.html?WFN`

    * **ADP força de trabalho melhoradas tempo**
        
        `<User access URL>?relaystate=https://fed.adp.com/saml/fedlanding.html?EETDC2`
    
    * **ADP privilegiando HCM**
        
        `<User access URL>?relaystate=https://fed.adp.com/saml/fedlanding.html?ADPVANTAGE`

    * **ADP Enterprise RH**

        `<User access URL>?relaystate=https://fed.adp.com/saml/fedlanding.html?PORTAL`

    * **MyADP**

        `<User access URL>?relaystate=https://fed.adp.com/saml/fedlanding.html?REDBOX`

9. **Guardar** as suas alterações.

10. Após a receção de confirmação do seu representante de ADP, começa o teste com um ou dois utilizadores.

    a. Atribua alguns utilizadores para o serviço de ADP aplicativo federado acesso de teste.

    b. Teste é efetuada com êxito quando os utilizadores acederem à aplicação de serviço ADP na galeria e podem acessar seu serviço ADP.
 
11. No caso de confirmação de um teste com êxito, atribua o serviço de ADP federado para utilizadores individuais ou grupos de utilizadores, que é explicado mais tarde no tutorial e distribuí-lo aos seus funcionários. 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/adpfederatedsso-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/adpfederatedsso-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/adpfederatedsso-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/adpfederatedsso-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-an-adp-test-user"></a>Criar um utilizador de teste ADP

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no ADP. Trabalhar com [equipa de suporte de ADP](https://www.adp.com/contact-us/overview.aspx) para adicionar os utilizadores na conta ADP.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para ADP.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a ADP, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **ADP**.

    ![A ligação de ADP na lista de aplicações](./media/adpfederatedsso-tutorial/tutorial_adp_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico ADP no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo ADP.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/adpfederatedsso-tutorial/tutorial_general_01.png
[2]: ./media/adpfederatedsso-tutorial/tutorial_general_02.png
[3]: ./media/adpfederatedsso-tutorial/tutorial_general_03.png
[4]: ./media/adpfederatedsso-tutorial/tutorial_general_04.png

[100]: ./media/adpfederatedsso-tutorial/tutorial_general_100.png

[200]: ./media/adpfederatedsso-tutorial/tutorial_general_200.png
[201]: ./media/adpfederatedsso-tutorial/tutorial_general_201.png
[202]: ./media/adpfederatedsso-tutorial/tutorial_general_202.png
[203]: ./media/adpfederatedsso-tutorial/tutorial_general_203.png

