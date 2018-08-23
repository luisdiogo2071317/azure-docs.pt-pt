---
title: 'Tutorial: Integração do Azure Active Directory com Optimizely | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Optimizely.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 28ef03e1-9aad-4301-af97-d94e853edc74
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2018
ms.author: jeedes
ms.openlocfilehash: be56218e174e5d8b0e6bde394f2dfd40fc91e87d
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/09/2018
ms.locfileid: "42056920"
---
# <a name="tutorial-azure-active-directory-integration-with-optimizely"></a>Tutorial: Integração do Azure Active Directory com Optimizely

Neste tutorial, saiba como integrar Optimizely com o Azure Active Directory (Azure AD).

Integrar Optimizely no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Optimizely
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Optimizely (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Optimizely, terá dos seguintes itens:

- Uma subscrição do Azure
- Um Optimizely logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste.
O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Optimizely da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-optimizely-from-the-gallery"></a>Adicionando Optimizely da Galeria

Para configurar a integração do Optimizely com o Azure AD, terá de adicionar Optimizely a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Optimizely a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]

3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **Optimizely**.

    ![Criar um utilizador de teste do Azure AD](./media/optimizely-tutorial/tutorial_optimizely_search.png)

5. No painel de resultados, selecione **Optimizely**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/optimizely-tutorial/tutorial_optimizely_addfromgallery.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Optimizely com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Optimizely a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Optimizely deve ser estabelecido.

Esta relação de ligação é estabelecida atribuindo o valor do **nome de utilizador** no Azure AD como o valor da **Username** no Optimizely.

Para configurar e testar o Azure AD início de sessão único com Optimizely, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste Optimizely](#creating-an-optimizely-test-user)**  - para ter um equivalente da Eduarda Almeida na Optimizely que está ligado à representação de utilizador do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Optimizely.

**Para configurar o Azure AD início de sessão único com Optimizely, execute os seguintes passos:**

1. No portal do Azure, sobre o **Optimizely** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.

    ![Configurar o início de sessão único](./media/optimizely-tutorial/tutorial_optimizely_samlbase.png)

3. Sobre o **Optimizely domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/optimizely-tutorial/tutorial_optimizely_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://app.optimizely.net/<instance name>`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão:  `urn:auth0:optimizely:contoso`

    > [!NOTE]
    > Estes valores não são o real. Atualizar o valor com o URL de início de sessão real e o identificador, o que é explicado mais tarde no tutorial.

4. Aplicação de Optimizely espera que as asserções SAML num formato específico. Configure as seguintes declarações para esta aplicação. Pode gerir os valores destes atributos da "**atributos de utilizador**" secção na página de integração de aplicações. Captura de ecrã seguinte mostra um exemplo disso.
    
    ![Configurar o início de sessão único](./media/optimizely-tutorial/tutorial_optimizely_attribute.png)
    
5. Clique em **ver e editar todos os outros atributos de utilizador** caixa de seleção o **atributos de utilizador** secção para expandir os atributos. Execute os seguintes passos em cada um dos atributos apresentados-

    | Nome do Atributo | Valor do Atributo |
    | ---------------| --------------- |
    | e-mail | User.Mail |

    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único](./media/optimizely-tutorial/tutorial_attribute_04.png)

    ![Configurar o início de sessão único](./media/optimizely-tutorial/tutorial_attribute_05.png)

    b. Na **nome** caixa de texto, tipo a **nome do atributo** mostrado para essa linha.

    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d. Clique em **OK**.

6. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/optimizely-tutorial/tutorial_optimizely_certificate.png)

7. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/optimizely-tutorial/tutorial_general_400.png)

8. Sobre o **Optimizely configuração** secção, clique em **configurar Optimizely** para abrir **configurar início de sessão** janela. Cópia a **SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/optimizely-tutorial/tutorial_optimizely_configure.png)

9. Para configurar o início de sessão único num **Optimizely** , contacte o seu Gestor de conta Optimizely e fornecerá o transferido **certificado (Base64)**, e **SAML único início de sessão no URL do serviço**.

10. Em resposta ao seu correio eletrónico, Optimizely fornece o início de sessão no URL (SSO iniciado por SP) e os valores de identificador (ID de entidade do fornecedor de serviço).

    a. Copiar o **URL de SSO iniciado por SP** fornecido pelo Optimizely e colar para o **URL de início de sessão** caixa de texto no **Optimizely domínio e URLs** secção no portal do Azure.

    b. Copiar o **ID de entidade do fornecedor de serviço** fornecido pelo Optimizely e colar para o **identificador** caixa de texto no **Optimizely domínio e URLs** secção no portal do Azure.

11. Numa janela do browser diferente, início de sessão na sua aplicação Optimizely.

12. Clique em que o nome de conta no canto superior direito e, em seguida **definições de conta**.

    ![O Azure AD Single Sign-On](./media/optimizely-tutorial/tutorial_optimizely_09.png)

13. No separador de conta, a caixa de verificação **ativar o SSO** sob o início de sessão único no **descrição geral** secção.
  
    ![O Azure AD Single Sign-On](./media/optimizely-tutorial/tutorial_optimizely_10.png)

14. Clicar em **Guardar**

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/optimizely-tutorial/create_aaduser_01.png) 

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/optimizely-tutorial/create_aaduser_02.png) 

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/optimizely-tutorial/create_aaduser_03.png) 

4. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/optimizely-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** da Eduarda Almeida.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.

### <a name="creating-an-optimizely-test-user"></a>Criar um utilizador de teste Optimizely

Nesta secção, vai criar um usuário chamado Eduarda Almeida no Optimizely.

1. Na home page, selecione **colaboradores** separador.

2. Para adicionar o novo funcionário ao projeto, clique em **novo funcionário**.
   
    ![Criar um utilizador de teste do Azure AD](./media/optimizely-tutorial/create_aaduser_10.png)

3. Preencha o endereço de e-mail e atribuir-lhes uma função. Clique em **convidar**.

    ![Criar um utilizador de teste do Azure AD](./media/optimizely-tutorial/create_aaduser_11.png)

4. Eles recebem um convite por e-mail. Usando o endereço de e-mail, têm de iniciar sessão no Optimizely.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Optimizely.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a Optimizely, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201]

2. Na lista de aplicações, selecione **Optimizely**.

    ![Configurar o início de sessão único](./media/optimizely-tutorial/tutorial_optimizely_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.

### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Optimizely no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Optimizely.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/optimizely-tutorial/tutorial_general_01.png
[2]: ./media/optimizely-tutorial/tutorial_general_02.png
[3]: ./media/optimizely-tutorial/tutorial_general_03.png
[4]: ./media/optimizely-tutorial/tutorial_general_04.png

[100]: ./media/optimizely-tutorial/tutorial_general_100.png

[200]: ./media/optimizely-tutorial/tutorial_general_200.png
[201]: ./media/optimizely-tutorial/tutorial_general_201.png
[202]: ./media/optimizely-tutorial/tutorial_general_202.png
[203]: ./media/optimizely-tutorial/tutorial_general_203.png