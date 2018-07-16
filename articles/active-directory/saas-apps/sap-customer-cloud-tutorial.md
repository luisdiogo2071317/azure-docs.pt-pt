---
title: 'Tutorial: Integração do Azure Active Directory com SAP Cloud para o cliente | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o SAP Cloud para o cliente.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 90154dab-eba2-4563-bcf0-f2acc797ea97
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: 8855a82c1490c916e040f61c07e1116d9125e7e6
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39045867"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-for-customer"></a>Tutorial: Integração do Azure Active Directory com SAP Cloud para o cliente

Neste tutorial, saiba como integrar o SAP Cloud para clientes com o Azure Active Directory (Azure AD).

Integrar SAP Cloud para o cliente com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso para a Cloud de SAP para o cliente
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para a Cloud de SAP para o cliente (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o SAP Cloud para o cliente, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Uma nuvem SAP para clientes de logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma avaliação de um mês aqui: [oferta de avaliação](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o SAP Cloud para o cliente da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-sap-cloud-for-customer-from-the-gallery"></a>Adicionando o SAP Cloud para o cliente da Galeria
Para configurar a integração da Cloud do SAP para o cliente para o Azure AD, terá de adicionar SAP Cloud para o cliente a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar SAP Cloud para o cliente a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **SAP Cloud para o cliente**.

    ![Criar um utilizador de teste do Azure AD](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_search.png)

5. No painel de resultados, selecione **SAP Cloud para o cliente**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com o SAP Cloud para o cliente com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador equivalente na Cloud SAP para o cliente a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado na Cloud SAP para o cliente tem de ser estabelecida.

Na Cloud de SAP para o cliente, atribui o valor do **nome de utilizador** no Azure AD como o valor da **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o SAP Cloud para o cliente, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar uma nuvem SAP para o utilizador de teste de cliente](#creating-a-sap-cloud-for-customer-test-user)**  - para ter um equivalente da Eduarda Almeida na Cloud do SAP para o cliente que está ligado à representação de utilizador do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua Cloud SAP para a aplicação de cliente.

**Para configurar o Azure AD início de sessão único com o SAP Cloud para o cliente, execute os seguintes passos:**

1. No portal do Azure, sobre o **SAP Cloud para o cliente** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_samlbase.png)

3. Sobre o **SAP Cloud para clientes de domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<server name>.crm.ondemand.com`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<server name>.crm.ondemand.com`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [SAP Cloud para a equipa de suporte do cliente de cliente](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) obter esses valores. 

4. Sobre o **atributos de utilizador** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_attribute.png)

    a. Na **identificador de utilizador** lista, selecione a **ExtractMailPrefix()** função.

    b. Partir do **correio** , selecione o atributo de utilizador que pretende utilizar para a sua implementação.
    Por exemplo, se pretender utilizar o campo IDdeEmpregado como identificador de utilizador exclusivo e armazenou o valor do atributo a ExtensionAttribute2, em seguida, selecione user.extensionattribute2.  

5. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![Configurar o início de sessão único](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_certificate.png) 

6. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/sap-customer-cloud-tutorial/tutorial_general_400.png)

7. Sobre o **SAP Cloud para a configuração de cliente** secção, clique em **configurar SAP na Cloud para o cliente** para abrir **configurar início de sessão** janela. Cópia a **SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_configure.png) 

8. Para obter SSO configurado, execute os seguintes passos:
   
    a. Início de sessão para a Cloud de SAP para o portal do cliente com direitos de administrador.
   
    b. Navegue para o **aplicativo e tarefas comuns de gestão de utilizador** e clique nas **fornecedor de identidade** separador.
   
    c. Clique em **novo fornecedor de identidade** e selecione o ficheiro XML de metadados que transferiu do portal do Azure. Ao importar os metadados, o sistema carrega automaticamente o certificado de assinatura necessária e o certificado de encriptação.
   
    ![Configurar o início de sessão único](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_54.png)
   
    d. O Azure Active Directory requer o elemento de URL do serviço de consumidor de asserção no pedido de SAML, por isso, selecione o **incluem o URL de serviço de consumidor de asserção** caixa de verificação.
   
    e. Clique em **ativar o início de sessão único**.
   
    f. Guarde as alterações.
   
    g. Clique nas **meu sistema** separador.
   
    ![Configurar o início de sessão único](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_52.png)
   
    h. Na **início de sessão do Azure AD no URL** caixa de texto, colar **SAML único início de sessão no URL do serviço** que copiou do portal do Azure.
   
    ![Configurar o início de sessão único](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_53.png)
   
    i. Especifique se o funcionário pode escolher manualmente entre o início de sessão com o ID de utilizador e palavra-passe ou SSO ao selecionar o **seleção de fornecedor de identidade Manual**.
   
    j. Na **URL de SSO** secção, especifique o URL que deve ser utilizado pelos funcionários para iniciar sessão sistema. 
    Na **URL enviado para o funcionário** lista, pode escolher entre as seguintes opções:
   
    **URL não SSO**
   
    O sistema envia apenas o URL de normais do sistema ao funcionário. O funcionário não pode iniciar sessão através do SSO e tem de utilizar palavra-passe ou certificado em vez disso.
   
    **URL DE SSO** 
   
    O sistema envia apenas o URL de SSO ao funcionário. O funcionário pode iniciar sessão através do SSO. Pedido de autenticação é redirecionado através do IdP.
   
    **Seleção automática**
   
    Se o SSO não estiver ativo, o sistema envia o URL de normais do sistema ao funcionário. Se o SSO estiver ativo, o sistema verifica se o funcionário tem uma palavra-passe. Se uma palavra-passe estiver disponível, o URL do SSO e o URL de SSO não são enviadas para o funcionário. No entanto, se o funcionário não tiver nenhuma palavra-passe, apenas o URL de SSO é enviado ao funcionário.
   
    k. Guarde as alterações.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/sap-customer-cloud-tutorial/create_aaduser_01.png) 

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/sap-customer-cloud-tutorial/create_aaduser_02.png) 

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/sap-customer-cloud-tutorial/create_aaduser_03.png) 

4. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/sap-customer-cloud-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-sap-cloud-for-customer-test-user"></a>Criar uma nuvem SAP para o utilizador de teste de cliente

Nesta secção, vai criar um usuário chamado Eduarda Almeida na Cloud do SAP para o cliente. Trabalhe em conjunto com [SAP Cloud para a equipa de suporte ao cliente](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) para adicionar os utilizadores na Cloud SAP para a plataforma de cliente. 

> [!NOTE]
> Certifique-se de que o valor de NameID deve corresponder com o campo de nome de utilizador na Cloud SAP para a plataforma de cliente.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso à nuvem SAP para o cliente.

![Atribuir utilizador][200] 

**Para atribuir Eduarda Almeida a SAP Cloud para o cliente, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **SAP Cloud para o cliente**.

    ![Configurar o início de sessão único](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica em nuvem SAP para o mosaico de cliente no painel de acesso, deve obter automaticamente com sessão iniciada para a Cloud de SAP para a aplicação de cliente.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/sap-customer-cloud-tutorial/tutorial_general_01.png
[2]: ./media/sap-customer-cloud-tutorial/tutorial_general_02.png
[3]: ./media/sap-customer-cloud-tutorial/tutorial_general_03.png
[4]: ./media/sap-customer-cloud-tutorial/tutorial_general_04.png

[100]: ./media/sap-customer-cloud-tutorial/tutorial_general_100.png

[200]: ./media/sap-customer-cloud-tutorial/tutorial_general_200.png
[201]: ./media/sap-customer-cloud-tutorial/tutorial_general_201.png
[202]: ./media/sap-customer-cloud-tutorial/tutorial_general_202.png
[203]: ./media/sap-customer-cloud-tutorial/tutorial_general_203.png

