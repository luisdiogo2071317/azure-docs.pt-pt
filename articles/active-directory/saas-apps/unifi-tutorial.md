---
title: 'Tutorial: Integração do Azure Active Directory com UNIFI | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e UNIFI.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: e1f49ee4-d2d4-4a82-9baf-0587ca1f20f6
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 0aa3ec013e93ddb89cd6982c01a38411f67b34c4
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37866353"
---
# <a name="tutorial-azure-active-directory-integration-with-unifi"></a>Tutorial: Integração do Azure Active Directory com UNIFI

Neste tutorial, saiba como integrar UNIFI com o Azure Active Directory (Azure AD).

Integrar UNIFI no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao UNIFI
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para UNIFI (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com UNIFI, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um UNIFI logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando UNIFI da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-unifi-from-the-gallery"></a>Adicionando UNIFI da Galeria
Para configurar a integração do UNIFI com o Azure AD, terá de adicionar UNIFI a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar UNIFI a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **UNIFI**.

    ![Criar um utilizador de teste do Azure AD](./media/unifi-tutorial/tutorial_unifi_search.png)

5. No painel de resultados, selecione **UNIFI**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/unifi-tutorial/tutorial_unifi_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com UNIFI com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no UNIFI a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no UNIFI deve ser estabelecido.

UNIFI, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com UNIFI, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Utilizador de teste de criação de um UNIFI](#creating-a-unifi-test-user)**  - para ter um equivalente da Eduarda Almeida na UNIFI que está ligado à representação de utilizador do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo UNIFI.

**Para configurar o Azure AD início de sessão único com UNIFI, execute os seguintes passos:**

1. No portal do Azure, sobre o **UNIFI** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/unifi-tutorial/tutorial_unifi_samlbase.png)

3. Sobre o **UNIFI domínio e URLs** secção, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

    ![Configurar o início de sessão único](./media/unifi-tutorial/tutorial_unifi_url1.png)

    Na **identificador** caixa de texto, digite o valor: `INVIEWlabs` 

4. Verifique **Mostrar definições de URL avançadas**, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![Configurar o início de sessão único](./media/unifi-tutorial/tutorial_unifi_url2.png)

    Na **URL de início de sessão** caixa de texto, escreva o URL: `https://app.discoverunifi.com/login`

5. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/unifi-tutorial/tutorial_unifi_certificate.png) 

6. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/unifi-tutorial/tutorial_general_400.png)
    
7. Sobre o **UNIFI configuração** secção, clique em **configurar UNIFI** para abrir **configurar início de sessão** janela. Cópia a **SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/unifi-tutorial/tutorial_unifi_configure.png)

8. Numa janela do browser web diferente, inicie sessão no seu **UNIFI** site da empresa como administrador.

9. Clique nas **utilizadores**.

    ![Configurar o início de sessão único](./media/unifi-tutorial/app1.png) 

10. Clique nas **adicionar novo fornecedor de identidade**.

    ![Configurar o início de sessão único](./media/unifi-tutorial/app2.png)

11. Na **Add Identity Provider** secção, execute os seguintes passos:   

    ![Configurar o início de sessão único](./media/unifi-tutorial/app3.png) 

    a. Na **nome do fornecedor** caixa de texto, escreva o nome do fornecedor de identidade....

    b. No **URL de fornecedor** colar de caixa de texto a **SAML único início de sessão no URL do serviço** valor, que copiou do portal do Azure.

    c. Remover do certificado que transferiu do portal do Azure no bloco de notas, abra a **---BEGIN CERTIFICATE---** e **---END CERTIFICATE---** marcar e, em seguida, cole os conteúdos restantes o **Certificado** caixa de texto.

    d. Selecione o **é o fornecedor predefinido** caixa de verificação.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/unifi-tutorial/create_aaduser_01.png) 

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/unifi-tutorial/create_aaduser_02.png) 

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/unifi-tutorial/create_aaduser_03.png) 

4. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/unifi-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-unifi-test-user"></a>Criar um utilizador de teste UNIFI

Nesta secção, vai criar um usuário chamado Eduarda Almeida. **UNIFI** suporta o aprovisionamento automático de utilizadores para que não existem passos manuais sejam necessários. Os utilizadores são criados automaticamente após a autenticação com êxito do Azure AD.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para UNIFI.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a UNIFI, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **UNIFI**.

    ![Configurar o início de sessão único](./media/unifi-tutorial/tutorial_unifi_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico UNIFI no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo UNIFI.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/unifi-tutorial/tutorial_general_01.png
[2]: ./media/unifi-tutorial/tutorial_general_02.png
[3]: ./media/unifi-tutorial/tutorial_general_03.png
[4]: ./media/unifi-tutorial/tutorial_general_04.png

[100]: ./media/unifi-tutorial/tutorial_general_100.png

[200]: ./media/unifi-tutorial/tutorial_general_200.png
[201]: ./media/unifi-tutorial/tutorial_general_201.png
[202]: ./media/unifi-tutorial/tutorial_general_202.png
[203]: ./media/unifi-tutorial/tutorial_general_203.png

