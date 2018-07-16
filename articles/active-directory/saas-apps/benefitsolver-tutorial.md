---
title: 'Tutorial: Integração do Azure Active Directory com Benefitsolver | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Benefitsolver.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 333394c1-b5a7-489c-8f7b-d1a5b4e782ea
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2017
ms.author: jeedes
ms.openlocfilehash: d3099b3e770acabe4e5eb74e0931e5b527f9f0c7
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39048067"
---
# <a name="tutorial-azure-active-directory-integration-with-benefitsolver"></a>Tutorial: Integração do Azure Active Directory com Benefitsolver

Neste tutorial, saiba como integrar Benefitsolver com o Azure Active Directory (Azure AD).

Integrar Benefitsolver no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Benefitsolver.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Benefitsolver (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Benefitsolver, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Benefitsolver logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Benefitsolver da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-benefitsolver-from-the-gallery"></a>Adicionando Benefitsolver da Galeria
Para configurar a integração do Benefitsolver com o Azure AD, terá de adicionar Benefitsolver a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Benefitsolver a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **Benefitsolver**, selecione **Benefitsolver** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Benefitsolver na lista de resultados](./media/benefitsolver-tutorial/tutorial_benefitsolver_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Benefitsolver com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Benefitsolver a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Benefitsolver deve ser estabelecido.

Benefitsolver, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Benefitsolver, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste Benefitsolver](#create-a-benefitsolver-test-user)**  - para ter um equivalente da Eduarda Almeida na Benefitsolver que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Benefitsolver.

**Para configurar o Azure AD início de sessão único com Benefitsolver, execute os seguintes passos:**

1. No portal do Azure, sobre o **Benefitsolver** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/benefitsolver-tutorial/tutorial_benefitsolver_samlbase.png)

3. Sobre o **Benefitsolver domínio e URLs** secção, execute os seguintes passos:

    ![Benefitsolver domínio e URLs únicas início de sessão em informações](./media/benefitsolver-tutorial/tutorial_benefitsolver_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `http://<companyname>.benefitsolver.com`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<companyname>.benefitsolver.com/saml20`

    c. Na **URL de resposta** caixa de texto, escreva o URL: `https://www.benefitsolver.com/benefits/BenefitSolverView?page_name=single_signon_saml`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão, identificador e o URL de resposta real. Contacte [equipa de suporte de cliente Benefitsolver](https://www.businessolver.com/contact) obter esses valores.

4. Seu aplicativo Benefitsolver espera que as asserções SAML num formato específico, o que requer a adição de mapeamentos de atributos personalizados para sua **atributos de token saml** configuração.

    ![Secção de atributo Benefitsolver](./media/benefitsolver-tutorial/tutorial_attribute.png)

5. No **atributos de utilizador** secção sobre o **início de sessão único** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem e execute os seguintes passos:
    
    | Nome do Atributo| Valor do Atributo|
    |---------------|----------------|
    | ID de cliente | Tem de obter este valor de sua [equipa de suporte de cliente Benefitsolver](https://www.businessolver.com/contact).|
    | ClientKey | Tem de obter este valor de sua [equipa de suporte de cliente Benefitsolver](https://www.businessolver.com/contact).|
    | LogoutURL | Tem de obter este valor de sua [equipa de suporte de cliente Benefitsolver](https://www.businessolver.com/contact).|
    | employeeID | Tem de obter este valor de sua [equipa de suporte de cliente Benefitsolver](https://www.businessolver.com/contact).|

    a. Clique em adicionar atributo para abrir a caixa de diálogo Adicionar atributo.

    ![Secção de atributo Benefitsolver](./media/benefitsolver-tutorial/tutorial_attribute_04.png)
    
    ![Secção de atributo Benefitsolver](./media/benefitsolver-tutorial/tutorial_attribute_05.png)

    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.
    
    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.
    
    d. Clique em **OK**.

6. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/benefitsolver-tutorial/tutorial_benefitsolver_certificate.png) 

7. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/benefitsolver-tutorial/tutorial_general_400.png)

8. Para configurar o início de sessão único num **Benefitsolver** lado, terá de enviar o transferido **XML de metadados** para [equipa de suporte de Benefitsolver](https://www.businessolver.com/contact).

    > [!NOTE]
    > A equipa de suporte de Benefitsolver tem de fazer a configuração real do SSO. Receberá uma notificação quando SSO foi ativado para a sua subscrição.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/benefitsolver-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/benefitsolver-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/benefitsolver-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/benefitsolver-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-benefitsolver-test-user"></a>Criar um utilizador de teste Benefitsolver

Para habilitar logon Benefitsolver de utilizadores do Azure AD, tem de ser aprovisionados em Benefitsolver. No caso de Benefitsolver, os dados de funcionários são em seu aplicativo preenchido por meio de um ficheiro de censo do seu sistema HRIS (normalmente, todas as noites).

> [!NOTE]
> Pode utilizar quaisquer outras Benefitsolver utilizador conta criação ferramentas ou APIs fornecidas pelo Benefitsolver para aprovisionar contas de utilizador do AAD.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Benefitsolver.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a Benefitsolver, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Benefitsolver**.

    ![A ligação de Benefitsolver na lista de aplicações](./media/benefitsolver-tutorial/tutorial_benefitsolver_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Benefitsolver no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Benefitsolver.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/benefitsolver-tutorial/tutorial_general_01.png
[2]: ./media/benefitsolver-tutorial/tutorial_general_02.png
[3]: ./media/benefitsolver-tutorial/tutorial_general_03.png
[4]: ./media/benefitsolver-tutorial/tutorial_general_04.png

[100]: ./media/benefitsolver-tutorial/tutorial_general_100.png

[200]: ./media/benefitsolver-tutorial/tutorial_general_200.png
[201]: ./media/benefitsolver-tutorial/tutorial_general_201.png
[202]: ./media/benefitsolver-tutorial/tutorial_general_202.png
[203]: ./media/benefitsolver-tutorial/tutorial_general_203.png

