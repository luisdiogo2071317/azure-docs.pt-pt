---
title: 'Tutorial: Integração do Active Directory do Azure com o primo Captivate do Adobe | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o primo Captivate do Adobe.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 2f95b226-1465-47f4-b8b7-de4b0772abbc
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/02/2018
ms.author: jeedes
ms.openlocfilehash: aa20e4544fcd78330c0daa15b9aa058ba80af2d5
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55171954"
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-captivate-prime"></a>Tutorial: Integração do Active Directory do Azure com o primo Captivate do Adobe

Neste tutorial, saiba como integrar o primo Captivate do Adobe com o Azure Active Directory (Azure AD).

Integrar o primo Captivate do Adobe no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao primos Captivate do Adobe.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Adobe Captivate primos (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com primos Captivate do Adobe, terá dos seguintes itens:

- Uma subscrição do Azure
- Um primos Captivate do Adobe logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando primos Captivate do Adobe da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-adobe-captivate-prime-from-the-gallery"></a>Adicionando primos Captivate do Adobe da Galeria
Para configurar a integração dos primos Captivate do Adobe para o Azure AD, terá de adicionar primos Captivate do Adobe a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar primos Captivate do Adobe a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **primos Captivate do Adobe**, selecione **primos Captivate do Adobe** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Adobe Captivate primordiais na lista de resultados](./media/adobecaptivateprime-tutorial/tutorial_adobecaptivateprime_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com o Adobe Captivate primos com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no primos Captivate do Adobe para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Adobe primo de Captivate deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com primos Captivate do Adobe, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste primos Captivate do Adobe](#create-an-adobe-captivate-prime-test-user)**  - para ter um equivalente da Eduarda Almeida no Adobe Captivate primos que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo primos Captivate do Adobe.

**Para configurar o Azure AD início de sessão único com primos Captivate do Adobe, execute os seguintes passos:**

1. No portal do Azure, sobre o **primos Captivate do Adobe** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/adobecaptivateprime-tutorial/tutorial_adobecaptivateprime_samlbase.png)

3. Sobre o **Adobe Captivate primos domínio e URLs** secção, execute os seguintes passos:

    ![Adobe Captivate primos domínio e URLs únicas início de sessão em informações](./media/adobecaptivateprime-tutorial/tutorial_adobecaptivateprime_url.png)

    a. Na **identificador** caixa de texto, escreva um URL: `https://captivateprime.adobe.com`

    b. Na **URL de resposta** caixa de texto, escreva um URL: `https://captivateprime.adobe.com/saml/SSO`

4. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/adobecaptivateprime-tutorial/tutorial_adobecaptivateprime_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/adobecaptivateprime-tutorial/tutorial_general_400.png)

6. Aceda a **propriedades** separador, copie a **URL de acesso de utilizador** e cole-o no bloco de notas.

    ![A ligação de acesso do utilizador](./media/adobecaptivateprime-tutorial/tutorial_adobecaptivateprime_appprop.png)

7. Para configurar o início de sessão único num **primos Captivate do Adobe** lado, terá de enviar o transferido **XML de metadados** e copiado **URL de acesso de utilizador** para [Adobe Captivate equipa de suporte de primos](mailto:captivateprimesupport@adobe.com). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/adobecaptivateprime-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/adobecaptivateprime-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/adobecaptivateprime-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/adobecaptivateprime-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
  
### <a name="create-an-adobe-captivate-prime-test-user"></a>Criar um utilizador de teste primos Captivate do Adobe

Nesta secção, vai criar um usuário chamado Eduarda Almeida no primos Captivate do Adobe. Trabalhar com [equipa de suporte de primos Captivate do Adobe](mailto:captivateprimesupport@adobe.com) para adicionar os utilizadores na plataforma primos Captivate do Adobe. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso para primos Captivate do Adobe.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a primos Captivate do Adobe, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **primos Captivate do Adobe**.

    ![A ligação de primos Captivate do Adobe na lista de aplicações](./media/adobecaptivateprime-tutorial/tutorial_adobecaptivateprime_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico primos Captivate do Adobe no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo primos Captivate do Adobe.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/adobecaptivateprime-tutorial/tutorial_general_01.png
[2]: ./media/adobecaptivateprime-tutorial/tutorial_general_02.png
[3]: ./media/adobecaptivateprime-tutorial/tutorial_general_03.png
[4]: ./media/adobecaptivateprime-tutorial/tutorial_general_04.png

[100]: ./media/adobecaptivateprime-tutorial/tutorial_general_100.png

[200]: ./media/adobecaptivateprime-tutorial/tutorial_general_200.png
[201]: ./media/adobecaptivateprime-tutorial/tutorial_general_201.png
[202]: ./media/adobecaptivateprime-tutorial/tutorial_general_202.png
[203]: ./media/adobecaptivateprime-tutorial/tutorial_general_203.png

