---
title: 'Tutorial: Integração do Azure Active Directory com o Zscaler dois | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o Zscaler dois.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 1fd8a940-7320-47e0-a176-2dd4eeca6db2
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: be41b1cc19043faf40804876d11fd43a32a1a45c
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39055801"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-two"></a>Tutorial: Integração do Azure Active Directory com o Zscaler dois

Neste tutorial, saiba como integrar o Zscaler dois com o Azure Active Directory (Azure AD).

Integrar o Zscaler dois no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD, quem tem acesso a dois do Zscaler
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Zscaler dois (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Zscaler dois, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Zscaler dois logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma avaliação de um mês aqui: [oferta de avaliação](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Zscaler dois da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-zscaler-two-from-the-gallery"></a>Adicionando Zscaler dois da Galeria
Para configurar a integração do Zscaler dois para o Azure AD, terá de adicionar Zscaler dois na Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Zscaler dois a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **Zscaler dois**.

    ![Criar um utilizador de teste do Azure AD](./media/zscaler-two-tutorial/tutorial_zscalertwo_search.png)

5. No painel de resultados, selecione **Zscaler dois**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/zscaler-two-tutorial/tutorial_zscalertwo_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com dois Zscaler com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Zscaler dois a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Zscaler dois deve ser estabelecido.

Em dois Zscaler, atribuir o valor do **nome de utilizador** no Azure AD como o valor da **nome de utilizador** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o Zscaler dois, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Configurar definições de proxy](#configuring-proxy-settings)**  - para configurar as definições de proxy no Internet Explorer
3. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
4. **[Criar um utilizador de teste Zscaler dois](#creating-a-zscaler-two-test-user)**  - para ter um equivalente da Eduarda Almeida na Zscaler duas que está ligado à representação de utilizador do Azure AD.
5. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
6. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Zscaler dois.

**Para configurar o Azure AD início de sessão único com o Zscaler dois, execute os seguintes passos:**

1. No portal do Azure, sobre o **Zscaler dois** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/zscaler-two-tutorial/tutorial_zscalertwo_samlbase.png)

3. Sobre o **domínio de dois Zscaler e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/zscaler-two-tutorial/tutorial_zscalertwo_url.png)

   Na caixa de texto URL de início de sessão, escreva o URL utilizado pelos seus utilizadores para início de sessão na sua aplicação ZScaler dois.

    > [!NOTE] 
    > Terá de atualizar este valor com o URL de início de sessão real. Contacte [equipa de suporte de cliente de dois Zscaler](https://www.zscaler.com/company/contact) obter esses valores.

4. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/zscaler-two-tutorial/tutorial_zscalertwo_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/zscaler-two-tutorial/tutorial_general_400.png)

6. Sobre o **Zscaler configuração de dois** secção, clique em **configurar dois Zscaler** para abrir **configurar início de sessão** janela. Cópia a **SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/zscaler-two-tutorial/tutorial_zscalertwo_configure.png) 

7. Numa janela do browser web diferente, inicie sessão no site da sua empresa ZScaler dois como administrador.

8. No menu na parte superior, clique em **administração**.
   
    ![Administração](./media/zscaler-two-tutorial/ic800206.png "administração")

9. Sob **gerir administradores e funções**, clique em **autenticação do & Gerir utilizadores**.   
            
    ![Gerir utilizadores e de autenticação](./media/zscaler-two-tutorial/ic800207.png "gerir utilizadores e de autenticação")

10. Na **escolher opções de autenticação para a sua organização** secção, execute os seguintes passos:   
                
    ![Autenticação](./media/zscaler-two-tutorial/ic800208.png "autenticação")
   
    a. Selecione **autenticar com o SAML Single Sign-On**.

    b. Clique em **configurar SAML únicos início de sessão em parâmetros**.

11. Sobre o **configurar SAML único início de sessão em parâmetros** página de diálogo, execute os seguintes passos e, em seguida, clique em **feito**

    ![Início de sessão único](./media/zscaler-two-tutorial/ic800209.png "início de sessão único")
    
    a. Colar o **SAML único início de sessão no URL do serviço** valor, que copiou do portal do Azure para o **URL do Portal de SAML para o qual os utilizadores são enviados para a autenticação** caixa de texto.
    
    b. Na **atributo que contém o nome de início de sessão** caixa de texto, tipo **NameID**.
    
    c. Para carregar o certificado transferido, clique em **Zscaler pem**.
    
    d. Selecione **ativar o aprovisionamento automático do SAML**.

12. Sobre o **configurar a autenticação de utilizador** caixa de diálogo página, execute os seguintes passos:

    ![Administração](./media/zscaler-two-tutorial/ic800210.png "administração")
    
    a. Clique em **Guardar**.

    b. Clique em **ative agora**.

## <a name="configuring-proxy-settings"></a>Configurar definições de proxy
### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a>Para configurar as definições de proxy no Internet Explorer

1. Inicie **do Internet Explorer**.

2. Selecione **opções da Internet** partir a **ferramentas** menu para abrir o **opções da Internet** caixa de diálogo.   
    
     ![Opções da Internet](./media/zscaler-two-tutorial/ic769492.png "opções da Internet")

3. Clique nas **ligações** separador.   
  
     ![Ligações](./media/zscaler-two-tutorial/ic769493.png "ligações")

4. Clique em **definições de LAN** para abrir o **definições de LAN** caixa de diálogo.

5. Na secção de servidor Proxy, execute os seguintes passos:   
   
    ![Servidor proxy](./media/zscaler-two-tutorial/ic769494.png "servidor Proxy")

    a. Selecione **utilizar um servidor proxy para a rede local**.

    b. Na caixa de texto endereço, escreva **gateway.zscalertwo.net**.

    c. Na caixa de texto de porta, escreva **80**.

    d. Selecione **Ignorar servidor proxy para endereços locais**.

    e. Clique em **OK** para fechar a **definições de rede Local (LAN)** caixa de diálogo.

6. Clique em **OK** para fechar a **opções da Internet** caixa de diálogo.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/zscaler-two-tutorial/create_aaduser_01.png) 

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/zscaler-two-tutorial/create_aaduser_02.png) 

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/zscaler-two-tutorial/create_aaduser_03.png) 

4. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/zscaler-two-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-zscaler-two-test-user"></a>Criar um utilizador de teste duas do Zscaler

Para ativar a utilizadores do Azure AD iniciar sessão no ZScaler dois, tem de ser aprovisionados para ZScaler dois. No caso do ZScaler dois, o aprovisionamento é uma tarefa manual.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Para configurar o aprovisionamento de utilizadores, execute os seguintes passos:

1. Inicie sessão no seu **Zscaler dois** inquilino.

2. Clique em **administração**.   
   
    ![Administração](./media/zscaler-two-tutorial/ic781035.png "administração")

3. Clique em **gestão de utilizadores**.   
        
     ![Adicione](./media/zscaler-two-tutorial/ic781036.png "adicionar")

4. Na **usuários** separador, clique em **Add**.
      
    ![Adicione](./media/zscaler-two-tutorial/ic781037.png "adicionar")

5. Na secção Adicionar utilizador, execute os seguintes passos:
        
    ![Adicionar utilizador](./media/zscaler-two-tutorial/ic781038.png "adicionar utilizador")
   
    a. Tipo de **UserID**, **nome a apresentar do utilizador**, **palavra-passe**, **Confirmar palavra-passe**e, em seguida, selecione **grupos**e o **departamento** de um Azure válido conta AD que pretende aprovisionar.

    b. Clique em **Guardar**.

> [!NOTE]
> Pode utilizar qualquer outro ZScaler duas ferramentas de criação de conta de utilizador ou APIs fornecidas pelo ZScaler duas para aprovisionar contas de utilizador do Azure AD.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Zscaler dois.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a Zscaler dois, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Zscaler dois**.

    ![Configurar o início de sessão único](./media/zscaler-two-tutorial/tutorial_zscalertwo_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Zscaler dois no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação Zscaler dois.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/zscaler-two-tutorial/tutorial_general_01.png
[2]: ./media/zscaler-two-tutorial/tutorial_general_02.png
[3]: ./media/zscaler-two-tutorial/tutorial_general_03.png
[4]: ./media/zscaler-two-tutorial/tutorial_general_04.png

[100]: ./media/zscaler-two-tutorial/tutorial_general_100.png

[200]: ./media/zscaler-two-tutorial/tutorial_general_200.png
[201]: ./media/zscaler-two-tutorial/tutorial_general_201.png
[202]: ./media/zscaler-two-tutorial/tutorial_general_202.png
[203]: ./media/zscaler-two-tutorial/tutorial_general_203.png

