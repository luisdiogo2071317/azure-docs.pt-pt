---
title: 'Tutorial: Integração do Azure Active Directory com o Portal de financiamento | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o Portal de financiamento.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 4663cc8a-976a-4c6c-b3b4-1e5df9b66744
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: 98ddd26bce7a8e2741db4f8e064a1b5bf2b6bed5
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37865863"
---
# <a name="tutorial-azure-active-directory-integration-with-the-funding-portal"></a>Tutorial: Integração do Azure Active Directory com o Portal de financiamento

Neste tutorial, saiba como integrar o Portal de financiamento com o Azure Active Directory (Azure AD).

Integrar o Portal de financiamento com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso para o Portal de financiamento
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para o Portal de financiamento (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Portal de financiamento, precisa do seguinte:

- Uma subscrição do Azure AD
- Um Portal de financiamento para o início de sessão único de subscrição ativada

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o Portal de financiamento da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-the-funding-portal-from-the-gallery"></a>Adicionando o Portal de financiamento da Galeria
Para configurar a integração entre o Portal de financiamento para o Azure AD, terá de adicionar o Portal de financiamento a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar o Portal de financiamento a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **o Portal de financiamento**.

    ![Criar um utilizador de teste do Azure AD](./media/thefundingportal-tutorial/tutorial_thefundingportal_search.png)

5. No painel de resultados, selecione **o Portal de financiamento**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/thefundingportal-tutorial/tutorial_thefundingportal_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com o Portal de financiamento com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador equivalente no Portal de financiamento à um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Portal de financiamento o deve ser estabelecido.

No Portal de financiamento a, atribuir o valor do **nome de utilizador** no Azure AD como o valor da **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o Portal de financiamento, precisa concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar utilizador de teste o Portal de financiamento](#creating-the-funding-portal-test-user)**  - para ter um equivalente da Eduarda Almeida no Portal de financiamento o que está ligado à representação de utilizador do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão único no portal do Azure e configurar o início de sessão único na sua aplicação do Portal de financiamento.

**Para configurar o Azure AD início de sessão único com o Portal de financiamento, execute os seguintes passos:**

1. No portal do Azure, sobre o **o Portal de financiamento** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/thefundingportal-tutorial/tutorial_thefundingportal_samlbase.png)

3. Sobre o **Domain Portal financiamento e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/thefundingportal-tutorial/tutorial_thefundingportal_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.regenteducation.net/`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.regenteducation.net`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte do cliente de Portal de financiamento](mailto:info@regenteducation.com) obter esses valores. 

4. A aplicação do Portal de financiamento espera que as asserções SAML para conter um atributo com o nome "externalId1". O valor de "externalId1" deve ser um studentID reconhecido. Configure a afirmação "externalId1" para esta aplicação. Pode gerir os valores destes atributos do **atributos de utilizador** do aplicativo. Captura de ecrã seguinte mostra um exemplo disso.

    ![Configurar o início de sessão único](./media/thefundingportal-tutorial/tutorial_thefundingportal_attribute.png)

5. No **atributos de utilizador** secção sobre o **início de sessão único** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem e execute os seguintes passos:

    | Nome de atributo | Valor do atributo |
    | ------------------- | ---------------- |
    | externalId1 | User.extensionattribute1 |

    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único](./media/thefundingportal-tutorial/tutorial_attribute_04.png)

    ![Configurar o início de sessão único](./media/thefundingportal-tutorial/tutorial_attribute_05.png)

    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Partir do **valor do atributo** , selecione o atributo que pretende utilizar para a sua implementação. Por exemplo, se tiver armazenado o valor de StudentID no ExtensionAttribute1, em seguida, selecione user.extensionattribute1.
    
    d. Clique em **OK**.
 
6. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![Configurar o início de sessão único](./media/thefundingportal-tutorial/tutorial_thefundingportal_certificate.png) 

7. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/thefundingportal-tutorial/tutorial_general_400.png)

8. Para configurar o início de sessão único num **o Portal de financiamento** lado, terá de enviar o transferido **XML de metadados** para [equipa de suporte do Portal de financiamento o](mailto:info@regenteducation.com). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/thefundingportal-tutorial/create_aaduser_01.png) 

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/thefundingportal-tutorial/create_aaduser_02.png) 

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/thefundingportal-tutorial/create_aaduser_03.png) 

4. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/thefundingportal-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-the-funding-portal-test-user"></a>Criar utilizador de teste o Portal de financiamento

Nesta secção, vai criar um usuário chamado Eduarda Almeida no Portal de financiamento. Trabalhar com [equipa de suporte do Portal de financiamento o](mailto:info@regenteducation.com) para adicionar o utilizador de teste e ativar o SSO.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para o Portal de financiamento.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida para o Portal de financiamento, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **o Portal de financiamento**.

    ![Configurar o início de sessão único](./media/thefundingportal-tutorial/tutorial_thefundingportal_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

O objetivo desta secção é testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica em mosaico o Portal de financiamento, no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação do Portal de financiamento.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/thefundingportal-tutorial/tutorial_general_01.png
[2]: ./media/thefundingportal-tutorial/tutorial_general_02.png
[3]: ./media/thefundingportal-tutorial/tutorial_general_03.png
[4]: ./media/thefundingportal-tutorial/tutorial_general_04.png

[100]: ./media/thefundingportal-tutorial/tutorial_general_100.png

[200]: ./media/thefundingportal-tutorial/tutorial_general_200.png
[201]: ./media/thefundingportal-tutorial/tutorial_general_201.png
[202]: ./media/thefundingportal-tutorial/tutorial_general_202.png
[203]: ./media/thefundingportal-tutorial/tutorial_general_203.png

