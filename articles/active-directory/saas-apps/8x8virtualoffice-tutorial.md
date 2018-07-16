---
title: 'Tutorial: Integração do Azure Active Directory com o Office de Virtual de 8 x 8 | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory diretório e 8 x 8 Virtual Office.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: b34a6edf-e745-4aec-b0b2-7337473d64c5
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 3a33f9ba0ca744709e21e9e55acc22b657c2adc2
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39048424"
---
# <a name="tutorial-azure-active-directory-integration-with-8x8-virtual-office"></a>Tutorial: Integração do Azure Active Directory com o Office de Virtual de 8 x 8

Neste tutorial, saiba como integrar o Office de Virtual de 8 x 8 com o Azure Active Directory (Azure AD).

Integração de 8 x 8 Office Virtual com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Office de Virtual de 8 x 8
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para o Office de Virtual de 8 x 8 (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Office de Virtual de 8 x 8, precisa do seguinte:

- Uma subscrição do Azure AD
- Um 8 x 8 Virtual Office-início de sessão único de subscrição ativada

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar 8 x 8 Virtual Office a partir da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-8x8-virtual-office-from-the-gallery"></a>Adicionar 8 x 8 Virtual Office a partir da Galeria
Para configurar a integração do Office de Virtual de 8 x 8 com o Azure AD, terá de adicionar 8 x 8 Virtual Office a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar 8 x 8 Virtual Office a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **8 x 8 Virtual Office**.

    ![Criar um utilizador de teste do Azure AD](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_search.png)

5. No painel de resultados, selecione **8 x 8 Virtual Office**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configurar e testar o Azure AD início de sessão único com 8 x 8 que virtual Office com base num utilizador de teste chamado "Eduarda Almeida."

Para início de sessão único funcione, o Azure AD precisa de saber o que o utilizador de contraparte 8 x 8 Virtual Office for para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado 8 x 8 Virtual Office deve ser estabelecido.

No escritório de Virtual de 8 x 8, atribuir o valor do **nome de utilizador** no Azure AD como o valor da **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o Office de Virtual de 8 x 8, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste de escritório Virtual 8 x 8](#creating-a-8x8-virtual-office-test-user)**  - para ter um equivalente da Eduarda Almeida no escritório de Virtual de 8 x 8 que está ligado à representação de utilizador do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na aplicação do Virtual Office de 8 x 8.

**Para configurar o Azure AD início de sessão único com o Office de Virtual de 8 x 8, execute os seguintes passos:**

1. No portal do Azure, sobre o **8 x 8 Virtual Office** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_samlbase.png)

3. Sobre o **8 x 8 Virtual Office domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_url.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão:

    | `https://sso.8x8.com/<companyname>` |
    | `https://www.8x8.com/<companyname>` |
    | `https://sso.8x8pilot.com/<companyname>` |

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão:

    | `https://<subdomain>.8x8.com/saml2` |
    | `https://<subdomain>.8x8pilot.com/saml2`|

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o identificador real e o URL de resposta. Contacte [equipa de suporte do Virtual Office 8 x 8](https://www.8x8.com/about-us/contact-us) obter esses valores.
 


4. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (bruto)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/8x8virtualoffice-tutorial/tutorial_general_400.png)

6. Na **configuração do Office de Virtual de 8 x 8** secção, clique em **Office Virtual de configurar 8 x 8** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_configure.png) 

7. Início de sessão no seu inquilino do Virtual Office 8 x 8 como administrador.

8. Selecione **Gestor de conta do Virtual Office** no painel de aplicação.
   
    ![Configurar no lado de aplicação](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_001.png)

9. Selecione **Business** conta para gerir e clique em **sessão** botão.
   
    ![Configurar no lado de aplicação](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_002.png)

10. Clique em **contas** guia na lista de menu.
   
    ![Configurar no lado de aplicação](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_003.png)

11. Clique em **início de sessão único** na lista de contas.
   
    ![Configurar no lado de aplicação](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_004.png)

12. Selecione **início de sessão único** sob o método de autenticação e clique em **SAML**.
    
    ![Configurar no lado de aplicação](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_005.png)

13. Cópia **URL de SAML SSO**, **única sessão. o URL do serviço de fim** e **URL do emissor** do Azure AD para **inicie sessão no URL**, **termine de URL**  e **URL do emissor** no escritório de Virtual de 8 x 8. 
    
    ![Configurar no lado de aplicação](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_006.png)
    
14. Clique em **Browser** botão para carregar o certificado que transferiu a partir do Azure AD e clique nas **guardar** botão.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/8x8virtualoffice-tutorial/create_aaduser_01.png) 

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/8x8virtualoffice-tutorial/create_aaduser_02.png) 

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/8x8virtualoffice-tutorial/create_aaduser_03.png) 

4. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/8x8virtualoffice-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-8x8-virtual-office-test-user"></a>Criar um utilizador de teste de escritório Virtual 8 x 8

O objetivo desta secção é criar um utilizador chamado Eduarda Almeida no escritório de Virtual de 8 x 8. 8 x 8 Virtual Office suporta o aprovisionamento just-in-time, que está por predefinição, ativada.

Não existe nenhum item de ação para nesta secção. Um novo utilizador é criado durante uma tentativa de aceder a 8 x 8 Virtual Office se não existir ainda. 

>[!NOTE]
>Se precisar de criar manualmente um utilizador, terá de contactar o [equipa de suporte do Virtual Office 8 x 8](https://www.8x8.com/about-us/contact-us). 

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso ao Office de Virtual de 8 x 8.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida 8 x 8 Virtual Office, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **8 x 8 Virtual Office**.

    ![Configurar o início de sessão único](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de escritório Virtual 8 x 8 no painel de acesso, deve obter automaticamente com sessão iniciada a aplicação do Virtual Office de 8 x 8.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md)

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/8x8virtualoffice-tutorial/tutorial_general_01.png
[2]: ./media/8x8virtualoffice-tutorial/tutorial_general_02.png
[3]: ./media/8x8virtualoffice-tutorial/tutorial_general_03.png
[4]: ./media/8x8virtualoffice-tutorial/tutorial_general_04.png

[100]: ./media/8x8virtualoffice-tutorial/tutorial_general_100.png

[200]: ./media/8x8virtualoffice-tutorial/tutorial_general_200.png
[201]: ./media/8x8virtualoffice-tutorial/tutorial_general_201.png
[202]: ./media/8x8virtualoffice-tutorial/tutorial_general_202.png
[203]: ./media/8x8virtualoffice-tutorial/tutorial_general_203.png

