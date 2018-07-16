---
title: 'Tutorial: Integração do Azure Active Directory com o Zscaler ZSCloud | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o Zscaler ZSCloud.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 411d5684-a780-410a-9383-59f92cf569b5
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: ea6008092f8fbfde8963da50d5273d36a4a30fe3
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39050015"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-zscloud"></a>Tutorial: Integração do Azure Active Directory com Zscaler ZSCloud

Neste tutorial, saiba como integrar o Zscaler ZSCloud com o Azure Active Directory (Azure AD).

Integrar o Zscaler ZSCloud no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Zscaler ZSCloud
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Zscaler ZSCloud (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Zscaler ZSCloud, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Zscaler ZSCloud logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Zscaler ZSCloud da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-zscaler-zscloud-from-the-gallery"></a>Adicionando Zscaler ZSCloud da Galeria
Para configurar a integração do Zscaler ZSCloud com o Azure AD, terá de adicionar Zscaler ZSCloud a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Zscaler ZSCloud a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **Zscaler ZSCloud**.

    ![Criar um utilizador de teste do Azure AD](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_search.png)

5. No painel de resultados, selecione **Zscaler ZSCloud**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com o Zscaler ZSCloud com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Zscaler ZSCloud a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Zscaler ZSCloud deve ser estabelecido.

Esta relação de ligação é estabelecida atribuindo o valor do **nome de utilizador** no Azure AD como o valor do **Username** no Zscaler ZSCloud.

Para configurar e testar o Azure AD início de sessão único com o Zscaler ZSCloud, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Configurar definições de proxy](#configuring-proxy-settings)**  - para configurar as definições de proxy no Internet Explorer
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste do Zscaler ZSCloud](#creating-a-zscaler-zscloud-test-user)**  - para ter um equivalente da Eduarda Almeida na Zscaler ZSCloud que está ligado à representação de utilizador do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Zscaler ZSCloud.

**Para configurar o Azure AD início de sessão único com o Zscaler ZSCloud, execute os seguintes passos:**

1. No portal do Azure, sobre o **Zscaler ZSCloud** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_samlbase.png)

3. Sobre o **Zscaler ZSCloud domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_url.png)

     Na **URL de início de sessão** caixa de texto, escreva o URL utilizado pelos seus utilizadores para início de sessão na sua aplicação ZScaler ZSCloud.
    
    > [!NOTE] 
    > Terá de atualizar este valor com o URL de início de sessão real. Contacte [equipa de suporte de cliente do Zscaler ZSCloud](https://help.zscaler.com/zia) para obter este valor. 
 
4. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/zscaler-zscloud-tutorial/tutorial_general_400.png)

6. Na **Zscaler ZSCloud Configuration** secção, clique em **configurar o Zscaler ZSCloud** para abrir **configurar início de sessão** janela. Cópia a **SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_configure.png) 

7. Numa janela do browser web diferente, inicie sessão no site da sua empresa ZScaler ZSCloud como administrador.

8. No menu na parte superior, clique em **administração**.
   
    ![Administração](./media/zscaler-zscloud-tutorial/ic800206.png "administração")

9. Sob **gerir administradores e funções**, clique em **autenticação do & Gerir utilizadores**.   
            
    ![Gerir utilizadores e de autenticação](./media/zscaler-zscloud-tutorial/ic800207.png "gerir utilizadores e de autenticação")

10. Na **escolher opções de autenticação para a sua organização** secção, execute os seguintes passos:   
                
    ![Autenticação](./media/zscaler-zscloud-tutorial/ic800208.png "autenticação")
   
    a. Selecione **autenticar com o SAML Single Sign-On**.

    b. Clique em **configurar SAML únicos início de sessão em parâmetros**.

11. Sobre o **configurar SAML único início de sessão em parâmetros** página de diálogo, execute os seguintes passos e, em seguida, clique em **feito**

    ![Início de sessão único](./media/zscaler-zscloud-tutorial/ic800209.png "início de sessão único")
    
    a. Colar o **SAML único início de sessão no URL do serviço** valor para o **URL do Portal de SAML para o qual os utilizadores são enviados para a autenticação** caixa de texto.
    
    b. Na **atributo que contém o nome de início de sessão** caixa de texto, tipo **NameID**.
    
    c. Para carregar o certificado transferido, clique em **Zscaler pem**.
    
    d. Selecione **ativar o aprovisionamento automático do SAML**.

12. Sobre o **configurar a autenticação de utilizador** caixa de diálogo página, execute os seguintes passos:

    ![Administração](./media/zscaler-zscloud-tutorial/ic800210.png "administração")
    
    a. Clique em **Guardar**.

    b. Clique em **ative agora**.

## <a name="configuring-proxy-settings"></a>Configurar definições de proxy
### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a>Para configurar as definições de proxy no Internet Explorer

1. Inicie **do Internet Explorer**.

2. Selecione **opções da Internet** partir a **ferramentas** menu para abrir o **opções da Internet** caixa de diálogo.   
    
     ![Opções da Internet](./media/zscaler-zscloud-tutorial/ic769492.png "opções da Internet")

3. Clique nas **ligações** separador.   
  
     ![Ligações](./media/zscaler-zscloud-tutorial/ic769493.png "ligações")

4. Clique em **definições de LAN** para abrir o **definições de LAN** caixa de diálogo.

5. Na secção de servidor Proxy, execute os seguintes passos:   
   
    ![Servidor proxy](./media/zscaler-zscloud-tutorial/ic769494.png "servidor Proxy")

    a. Selecione **utilizar um servidor proxy para a rede local**.

    b. Na caixa de texto endereço, escreva **gateway.zscalerone.net**.

    c. Na caixa de texto de porta, escreva **80**.

    d. Selecione **Ignorar servidor proxy para endereços locais**.

    e. Clique em **OK** para fechar a **definições de rede Local (LAN)** caixa de diálogo.

6. Clique em **OK** para fechar a **opções da Internet** caixa de diálogo.

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/zscaler-zscloud-tutorial/create_aaduser_01.png) 

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/zscaler-zscloud-tutorial/create_aaduser_02.png) 

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/zscaler-zscloud-tutorial/create_aaduser_03.png) 

4. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/zscaler-zscloud-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.

### <a name="creating-a-zscaler-zscloud-test-user"></a>Criar um utilizador de teste Zscaler ZSCloud

Para ativar a utilizadores do Azure AD iniciar sessão no ZScaler ZSCloud, tem de ser aprovisionados para ZScaler ZSCloud.  
No caso do ZScaler ZSCloud, aprovisionamento é uma tarefa manual.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Para configurar o aprovisionamento de utilizadores, execute os seguintes passos:

1. Inicie sessão no seu **Zscaler** inquilino.

2. Clique em **administração**.   
   
    ![Administração](./media/zscaler-zscloud-tutorial/ic781035.png "administração")

3. Clique em **gestão de utilizadores**.   
        
     ![Adicione](./media/zscaler-zscloud-tutorial/ic781037.png "adicionar")

4. Na **usuários** separador, clique em **Add**.
      
    ![Adicione](./media/zscaler-zscloud-tutorial/ic781037.png "adicionar")

5. Na secção Adicionar utilizador, execute os seguintes passos:
        
    ![Adicionar utilizador](./media/zscaler-zscloud-tutorial/ic781038.png "adicionar utilizador")
   
    a. Tipo de **UserID**, **nome a apresentar do utilizador**, **palavra-passe**, **Confirmar palavra-passe**e, em seguida, selecione **grupos**e o **departamento** de uma conta do AAD válida que pretende aprovisionar.

    b. Clique em **Guardar**.

> [!NOTE]
> Pode utilizar quaisquer outras ZScaler ZSCloud utilizador conta criação ferramentas ou APIs fornecidas pelo ZScaler ZSCloud para aprovisionar contas de utilizador do AAD.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Zscaler ZSCloud.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a Zscaler ZSCloud, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Zscaler ZSCloud**.

    ![Configurar o início de sessão único](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Se pretender testar as definições de início de sessão únicas, abra o painel de acesso.

Quando clica no mosaico do Zscaler ZSCloud no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação Zscaler ZSCloud.

Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/zscaler-zscloud-tutorial/tutorial_general_01.png
[2]: ./media/zscaler-zscloud-tutorial/tutorial_general_02.png
[3]: ./media/zscaler-zscloud-tutorial/tutorial_general_03.png
[4]: ./media/zscaler-zscloud-tutorial/tutorial_general_04.png

[100]: ./media/zscaler-zscloud-tutorial/tutorial_general_100.png

[200]: ./media/zscaler-zscloud-tutorial/tutorial_general_200.png
[201]: ./media/zscaler-zscloud-tutorial/tutorial_general_201.png
[202]: ./media/zscaler-zscloud-tutorial/tutorial_general_202.png
[203]: ./media/zscaler-zscloud-tutorial/tutorial_general_203.png

