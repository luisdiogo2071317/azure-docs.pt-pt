---
title: 'Tutorial: Integração do Azure Active Directory com ClickTime | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e ClickTime.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: d437b5ab-4d71-4c13-96d0-79018cebbbd4
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: jeedes
ms.openlocfilehash: adf3b1310a3e79b12be5294fe9d7a012bfc0d433
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39042943"
---
# <a name="tutorial-azure-active-directory-integration-with-clicktime"></a>Tutorial: Integração do Azure Active Directory com ClickTime

Neste tutorial, saiba como integrar ClickTime com o Azure Active Directory (Azure AD).

Integrar ClickTime no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao ClickTime
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para ClickTime (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com ClickTime, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um ClickTime logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando ClickTime da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-clicktime-from-the-gallery"></a>Adicionando ClickTime da Galeria
Para configurar a integração do ClickTime com o Azure AD, terá de adicionar ClickTime a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar ClickTime a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **ClickTime**, selecione **ClickTime** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![ClickTime na lista de resultados](./media/clicktime-tutorial/tutorial_clicktime_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com ClickTime com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no ClickTime a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no ClickTime deve ser estabelecido.

ClickTime, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com ClickTime, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste ClickTime](#create-a-clicktime-test-user)**  - para ter um equivalente da Eduarda Almeida na ClickTime que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo ClickTime.

**Para configurar o Azure AD início de sessão único com ClickTime, execute os seguintes passos:**

1. No portal do Azure, sobre o **ClickTime** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/clicktime-tutorial/tutorial_clicktime_samlbase.png)

3. Sobre o **ClickTime domínio e URLs** secção, execute os seguintes passos:

    ![ClickTime domínio e URLs únicas início de sessão em informações](./media/clicktime-tutorial/tutorial_clicktime_url.png)

    a. Na **identificador** caixa de texto, escreva um URL como: `https://app.clicktime.com/sp/`
    
    b. Na **URL de resposta** caixa de texto, escreva um URL com os seguintes padrões: 

    | |
    |--|
    | `https://app.clicktime.com/Login/` |
    | `https://app.clicktime.com/App/Login/Consume.aspx` |

4. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/clicktime-tutorial/tutorial_clicktime_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/clicktime-tutorial/tutorial_general_400.png)

6. Sobre o **ClickTime configuração** secção, clique em **configurar ClickTime** para abrir **configurar início de sessão** janela. Cópia a **SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração de ClickTime](./media/clicktime-tutorial/tutorial_clicktime_configure.png) 

7. Numa janela do browser web diferente, inicie sessão no site da sua empresa ClickTime como um administrador.

8. Na barra de ferramentas na parte superior, clique em **preferências**e, em seguida, clique em **configurações de segurança**.

9. Na **as preferências de início de sessão único** configuração secção, execute os seguintes passos:
   
    ![Definições de segurança](./media/clicktime-tutorial/tic777280.png "as definições de segurança")
   
    a.  Selecione **permitir** início de sessão único início de sessão (SSO) com a utilizar **do Azure AD**.
   
    b. Na **ponto final do fornecedor de identidade** caixa de texto, colar **SAML único início de sessão no URL do serviço** que copiou do portal do Azure.
   
    c.  Abra o **certificado com codificação base 64** transferido a partir do portal do Azure na **bloco de notas**, copie o conteúdo e, em seguida, cole-o no **certificado X.509** caixa de texto.
   
    d.  Clique em **Guardar**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/clicktime-tutorial/create_aaduser_01.png) 

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.
    
    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/clicktime-tutorial/create_aaduser_02.png) 

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.
 
    ![Botão Adicionar](./media/clicktime-tutorial/create_aaduser_03.png) 

4. Na **utilizador** diálogo caixa, execute os seguintes passos:
 
    ![A caixa de diálogo de utilizador](./media/clicktime-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="create-a-clicktime-test-user"></a>Criar um utilizador de teste ClickTime

Para habilitar logon ClickTime de utilizadores do Azure AD, tem de ser aprovisionados em ClickTime.  
No caso de ClickTime, o aprovisionamento é uma tarefa manual.

> [!NOTE]
> Pode utilizar quaisquer outras ClickTime utilizador conta criação ferramentas ou APIs fornecidas pelo ClickTime para aprovisionar contas de utilizador do Azure AD.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**
1. Inicie sessão no seu **ClickTime** inquilino.
2. Na barra de ferramentas na parte superior, clique em **empresa**e, em seguida, clique em **pessoas**.
   
    ![As pessoas](./media/clicktime-tutorial/tic777282.png "pessoas")
3. Clique em **Adicionar pessoa**.
   
    ![Adicionar pessoa](./media/clicktime-tutorial/tic777283.png "Adicionar pessoa")
4. Na seção nova pessoa, execute os seguintes passos:
   
    ![As pessoas](./media/clicktime-tutorial/tic777284.png "pessoas")
   
    a.  Na **nome completo** caixa de texto, tipo de nome completo do utilizador, como **Eduarda Almeida**. 
  
    b.  Na **endereço de e-mail** caixa de texto, como o tipo de e-mail do utilizador **brittasimon@contoso.com**.
       
    > [!NOTE]
    > Se quiser, pode definir propriedades adicionais do novo objeto person.
   
    c.  Clique em **Guardar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para ClickTime.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a ClickTime, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **ClickTime**.

    ![Ligação de ClickTimne na lista de aplicações](./media/clicktime-tutorial/tutorial_clicktime_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202] 

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico ClickTime no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo ClickTime.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/clicktime-tutorial/tutorial_general_01.png
[2]: ./media/clicktime-tutorial/tutorial_general_02.png
[3]: ./media/clicktime-tutorial/tutorial_general_03.png
[4]: ./media/clicktime-tutorial/tutorial_general_04.png

[100]: ./media/clicktime-tutorial/tutorial_general_100.png

[200]: ./media/clicktime-tutorial/tutorial_general_200.png
[201]: ./media/clicktime-tutorial/tutorial_general_201.png
[202]: ./media/clicktime-tutorial/tutorial_general_202.png
[203]: ./media/clicktime-tutorial/tutorial_general_203.png

