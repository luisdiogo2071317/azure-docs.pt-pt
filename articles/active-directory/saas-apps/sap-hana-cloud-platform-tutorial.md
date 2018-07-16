---
title: 'Tutorial: Integração do Azure Active Directory com SAP Cloud Platform | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o SAP Cloud Platform.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: bd398225-8bd8-4697-9a44-af6e6679113a
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: jeedes
ms.openlocfilehash: 176ed1718818b24866620d86d7eff4d9af1d378f
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39050879"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-platform"></a>Tutorial: Integração do Azure Active Directory com SAP Cloud Platform

Neste tutorial, saiba como integrar a plataforma de Cloud de SAP no Azure Active Directory (Azure AD).

Integrar a plataforma de Cloud de SAP no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao SAP Cloud Platform.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para o SAP Cloud Platform (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o SAP Cloud Platform, precisa do seguinte:

- Uma subscrição do Azure AD
- Uma plataforma de Cloud de SAP logon único habilitado subscrição

Depois de concluir este tutorial, os utilizadores do Azure AD tiver atribuído a SAP Cloud Platform será capazes de início de sessão único para a aplicação a utilizar o [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

>[!IMPORTANT]
>Terá de implementar a sua própria aplicação ou subscrever uma aplicação na sua conta do SAP Cloud Platform, para testar logon único. Neste tutorial, uma aplicação é implementada na conta.
> 

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando a plataforma de Cloud de SAP da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-sap-cloud-platform-from-the-gallery"></a>Adicionando a plataforma de Cloud de SAP da Galeria
Para configurar a integração da plataforma de Cloud de SAP no Azure AD, terá de adicionar plataforma de Cloud de SAP a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar plataforma Cloud do SAP a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **SAP Cloud Platform**, selecione **SAP Cloud Platform** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Plataforma de Cloud SAP na lista de resultados](./media/sap-hana-cloud-platform-tutorial/tutorial_sapcloudplatform_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com o SAP Cloud Platform, com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de equivalente na plataforma Cloud do SAP para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado na plataforma de Cloud de SAP tem de ser estabelecida.

Na plataforma de Cloud do SAP, atribua o valor do **nome de utilizador** no Azure AD como o valor da **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o SAP Cloud Platform, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste de SAP Cloud Platform](#create-a-sap-cloud-platform-test-user)**  - para ter um equivalente da Eduarda Almeida na plataforma de Cloud de SAP que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação SAP Cloud Platform.

**Para configurar o Azure AD início de sessão único com o SAP Cloud Platform, execute os seguintes passos:**

1. No portal do Azure, sobre o **SAP Cloud Platform** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/sap-hana-cloud-platform-tutorial/tutorial_sapcloudplatform_samlbase.png)

3. Sobre o **SAP Cloud Platform domínio e URLs** secção, execute os seguintes passos:

    ![Domínio de plataforma de Cloud do SAP e URLs únicas início de sessão em informações](./media/sap-hana-cloud-platform-tutorial/tutorial_sapcloudplatform_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva o URL utilizado pelos seus utilizadores para iniciar sessão no seu **SAP Cloud Platform** aplicação. Este é o URL de conta específica de um recurso protegido em seu aplicativo SAP Cloud Platform. O URL baseia-se no seguinte padrão: `https://<applicationName><accountName>.<landscape host>.ondemand.com/<path_to_protected_resource>`
      
     >[!NOTE]
     >Este é o URL na sua aplicação SAP Cloud Platform, que requer que o utilizador autenticar.
     > 

    | |
    |--|
    | `https://<subdomain>.hanatrial.ondemand.com/<instancename>` |
    | `https://<subdomain>.hana.ondemand.com/<instancename>` |

    b. Na **identificador** caixa de texto, deve fornecer sua plataforma de Cloud de SAP escreva um URL através de um dos padrões seguintes: 

    | |
    |--|
    | `https://hanatrial.ondemand.com/<instancename>` |
    | `https://hana.ondemand.com/<instancename>` |
    | `https://us1.hana.ondemand.com/<instancename>` |
    | `https://ap1.hana.ondemand.com/<instancename>` |

    c. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão:

    | |
    |--|
    | `https://<subdomain>.hanatrial.ondemand.com/<instancename>` |
    | `https://<subdomain>.hana.ondemand.com/<instancename>` |
    | `https://<subdomain>.us1.hana.ondemand.com/<instancename>` |
    | `https://<subdomain>.dispatcher.us1.hana.ondemand.com/<instancename>` |
    | `https://<subdomain>.ap1.hana.ondemand.com/<instancename>` |
    | `https://<subdomain>.dispatcher.ap1.hana.ondemand.com/<instancename>` |
    | `https://<subdomain>.dispatcher.hana.ondemand.com/<instancename>` |

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão, identificador e o URL de resposta real. Contacte [equipa de suporte de cliente de plataforma de Cloud de SAP](https://help.sap.com/viewer/65de2977205c403bbc107264b8eccf4b/Cloud/5dd739823b824b539eee47b7860a00be.html) para obter o URL de início de sessão e o identificador. URL de resposta, pode obter da secção de gestão de fidedignidade que é explicada mais tarde no tutorial.
    > 
     
4. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/sap-hana-cloud-platform-tutorial/tutorial_sapcloudplatform_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/sap-hana-cloud-platform-tutorial/tutorial_general_400.png)

6. Numa janela do browser web diferente, inicie sessão cabine de plataforma de Cloud de SAP em `https://account.<landscape host>.ondemand.com/cockpit`(por exemplo: https://account.hanatrial.ondemand.com/cockpit).

7. Clique nas **confiar** separador.
   
    ![Confiar](./media/sap-hana-cloud-platform-tutorial/ic790800.png "confiar")

8. Na secção Gestão confiar, sob **fornecedor de serviços locais**, execute os seguintes passos:

    ![Gestão de confiança](./media/sap-hana-cloud-platform-tutorial/ic793931.png "confiar gestão")
   
    a. Clique em **Editar**.

    b. Como **tipo de configuração**, selecione **personalizado**.

    c. Como **nome do fornecedor de Local**, deixe o valor predefinido. Copie este valor e cole-o para o **identificador** campo na configuração do AD do Azure para SAP Cloud Platform.

    d. Para gerar um **chave de assinatura** e uma **certificado de assinatura** par de chaves, clique em **gerar par de chave**.

    e. Como **propagação Principal**, selecione **desativado**.

    f. Como **autenticação de força**, selecione **desativado**.

    g. Clique em **Guardar**.

9. Depois de guardar o **fornecedor de serviços locais** definições, efetue o seguinte para obter o URL de resposta:
   
    ![Obter metadados](./media/sap-hana-cloud-platform-tutorial/ic793930.png "obter metadados")

    a. Transfira o ficheiro de metadados de SAP Cloud Platform clicando **obter metadados**.

    b. Abra o ficheiro XML de metadados SAP Cloud Platform transferido e, em seguida, localize a **ns3:AssertionConsumerService** marca.
 
    c. Copie o valor do **localização** atributo e, em seguida, cole-o para o **URL de resposta** campo na configuração do AD do Azure para SAP Cloud Platform.

10. Clique nas **fornecedor de identidade fidedignos** separador e, em seguida, clique em **Adicionar fornecedor de identidade fidedignos**.
   
    ![Gestão de confiança](./media/sap-hana-cloud-platform-tutorial/ic790802.png "confiar gestão")
   
    >[!NOTE]
    >Para gerir a lista de fornecedores de identidade fidedignos, terá de escolher o tipo de configuração personalizada na secção de fornecedor de serviços locais. Para o tipo de configuração padrão, tem uma confiança implícita e não editáveis para o serviço de ID de SAP. Para None, não tem quaisquer definições de confiança.
    > 
    > 

11. Clique nas **gerais** separador e, em seguida, clique em **procurar** para carregar o ficheiro de metadados baixado.
    
    ![Gestão de confiança](./media/sap-hana-cloud-platform-tutorial/ic793932.png "confiar gestão")
    
    >[!NOTE]
    >Depois de carregar o ficheiro de metadados, os valores para **URL de início de sessão único**, **URL de fim de sessão único**, e **certificado de assinatura** são preenchidos automaticamente.
    > 
     
12. Clique no separador **Atributos**.

13. Sobre o **atributos** separador, executar o passo seguinte:
    
    ![Atributos](./media/sap-hana-cloud-platform-tutorial/ic790804.png "atributos") 

    a. Clique em **Add Assertion-Based atributo**e, em seguida, adicione os seguintes atributos de asserção com base em:
       
    | Atributo de asserção | Atributo principal |
    | --- | --- |
    | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname` |FirstName |
    | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname` |Apelido |
    | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` |e-mail |
   
     >[!NOTE]
     >A configuração dos atributos depende de como as aplicações no SCP são desenvolvidas, ou seja, quais atributos esperam na resposta SAML e em que nome (atributo Principal) acederem este atributo no código.
     > 
    
    b. O **atributo predefinido** na captura de ecrã é apenas para fins de ilustração. Não é necessário para tornar o cenário de trabalho.  
 
    c. Os nomes e valores para **atributo Principal** mostrado na captura de ecrã dependem de como o aplicativo é desenvolvido. É possível que a sua aplicação requer mapeamentos diferentes.

### <a name="assertion-based-groups"></a>Grupos com base em asserção

Como passo opcional, pode configurar grupos com base em asserção para o seu fornecedor de identidade de diretório Active Directory do Azure.

Utilizar grupos na plataforma do SAP na Cloud permite-lhe atribuir dinamicamente um ou mais utilizadores a uma ou mais funções nas suas aplicações SAP Cloud Platform, determinadas pelos valores de atributos na asserção de SAML 2.0. 

Por exemplo, se a asserção contém o atributo "*contrato = temporário*", poderá pretender que os utilizadores as ser adicionado ao grupo"*temporário*". O grupo de "*temporária*" pode conter uma ou mais funções de uma ou mais aplicações implementadas na sua conta do SAP Cloud Platform.
 
Utilize grupos com base em asserção quando deseja atribuir simultaneamente muitos utilizadores a uma ou mais funções de aplicações na sua conta do SAP Cloud Platform. Se pretender atribuir apenas um único ou pequeno número de utilizadores a funções específicas, recomendamos que atribuí-las diretamente na "**autorizações**" Guia de cabine SAP Cloud Platform.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/sap-hana-cloud-platform-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/sap-hana-cloud-platform-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/sap-hana-cloud-platform-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/sap-hana-cloud-platform-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-sap-cloud-platform-test-user"></a>Criar um utilizador de teste de SAP Cloud Platform

Para habilitar os utilizadores do Azure AD iniciar sessão na plataforma Cloud do SAP, tem de atribuir funções na plataforma da Cloud de SAP aos mesmos.

**Para atribuir uma função a um utilizador, execute os seguintes passos:**

1. Inicie sessão no seu **SAP Cloud Platform** cockpit.

2. Execute o seguinte:
   
    ![As autorizações](./media/sap-hana-cloud-platform-tutorial/ic790805.png "autorizações")
   
    a. Clique em **autorização**.

    b. Clique nas **utilizadores** separador.

    c. Na **utilizador** caixa de texto, escreva o endereço de e-mail do utilizador.

    d. Clique em **atribuir** atribuir o utilizador a uma função.

    e. Clique em **Guardar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso à plataforma de Cloud de SAP.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida na plataforma Cloud SAP, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **SAP Cloud Platform**.

    ![A ligação de SAP Cloud Platform na lista de aplicações](./media/sap-hana-cloud-platform-tutorial/tutorial_sapcloudplatform_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

O objetivo desta secção é testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de SAP Cloud Platform no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação SAP Cloud Platform.


## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/sap-hana-cloud-platform-tutorial/tutorial_general_01.png
[2]: ./media/sap-hana-cloud-platform-tutorial/tutorial_general_02.png
[3]: ./media/sap-hana-cloud-platform-tutorial/tutorial_general_03.png
[4]: ./media/sap-hana-cloud-platform-tutorial/tutorial_general_04.png

[100]: ./media/sap-hana-cloud-platform-tutorial/tutorial_general_100.png

[200]: ./media/sap-hana-cloud-platform-tutorial/tutorial_general_200.png
[201]: ./media/sap-hana-cloud-platform-tutorial/tutorial_general_201.png
[202]: ./media/sap-hana-cloud-platform-tutorial/tutorial_general_202.png
[203]: ./media/sap-hana-cloud-platform-tutorial/tutorial_general_203.png

