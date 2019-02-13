---
title: 'Tutorial: Integração do Active Directory do Azure com Vibe HCM | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Vibe HCM.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4379bef7-adc9-4b6d-9384-c46d9a914bfe
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 79787045f0379e6b672350206740297000f298c5
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56185575"
---
# <a name="tutorial-azure-active-directory-integration-with-vibe-hcm"></a>Tutorial: Integração do Active Directory do Azure com Vibe HCM

Neste tutorial, saiba como integrar Vibe HCM com o Azure Active Directory (Azure AD).

Integrar Vibe HCM no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Vibe HCM.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Vibe HCM (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Vibe HCM, terá dos seguintes itens:

- Uma subscrição do Azure
- Um Vibe HCM logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Vibe HCM da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-vibe-hcm-from-the-gallery"></a>Adicionando Vibe HCM da Galeria
Para configurar a integração de Vibe HCM com o Azure AD, terá de adicionar Vibe HCM a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Vibe HCM a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **Vibe HCM**, selecione **Vibe HCM** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Vibe HCM na lista de resultados](./media/vibehcm-tutorial/tutorial_vibehcm_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com HCM Vibe com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Vibe HCM a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Vibe HCM deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com Vibe HCM, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste de Vibe HCM](#create-a-vibe-hcm-test-user)**  - para ter um equivalente da Eduarda Almeida na Vibe HCM que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo de Vibe HCM.

**Para configurar o Azure AD início de sessão único com Vibe HCM, execute os seguintes passos:**

1. No portal do Azure, sobre o **Vibe HCM** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/vibehcm-tutorial/tutorial_vibehcm_samlbase.png)

3. Sobre o **Vibe HCM domínio e URLs** secção, he utilizador tem de efetuar outros passos de como a aplicação já está pré-integrada com o Azure:

    ![Vibe HCM domínio e URLs únicas início de sessão em informações](./media/vibehcm-tutorial/tutorial_vibehcm_url.png)

4. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![Vibe HCM domínio e URLs únicas início de sessão em informações](./media/vibehcm-tutorial/tutorial_vibehcm_url1.png)

    Na **iniciar sessão no URL** caixa de texto, escreva um URL com o seguinte padrão: `https://<companyName>.vibehcm.com/portal.jsp`
     
    > [!NOTE] 
    > O valor do URL de início de sessão não é real. Atualize o valor com o URL de início de sessão real. Contacte [equipa de suporte de Vibe HCM](mailto:support@vibehcm.com) para obter o valor.
 
4. Sobre o **certificado de assinatura SAML** secção, clique no botão de cópia para copiar **Url de metadados de Federação de aplicação** e cole-o no bloco de notas.

    ![O link de download de certificado](./media/vibehcm-tutorial/tutorial_vibehcm_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/vibehcm-tutorial/tutorial_general_400.png)

6. Para configurar o início de sessão único num **Vibe HCM** lado, terá de enviar o copiado **Url de metadados de Federação de aplicação** para [equipa de suporte de Vibe HCM](mailto:support@vibehcm.com). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/vibehcm-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/vibehcm-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/vibehcm-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/vibehcm-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-vibe-hcm-test-user"></a>Criar um utilizador de teste de Vibe HCM

Nesta secção, vai criar um usuário chamado Eduarda Almeida no Vibe HCM. Trabalhar com [equipa de suporte de Vibe HCM](mailto:support@vibehcm.com) para adicionar os utilizadores na plataforma Vibe HCM. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso para Vibe HCM.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a Vibe HCM, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Vibe HCM**.

    ![A ligação de Vibe HCM na lista de aplicações](./media/vibehcm-tutorial/tutorial_vibehcm_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de Vibe HCM no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação de Vibe HCM.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/vibehcm-tutorial/tutorial_general_01.png
[2]: ./media/vibehcm-tutorial/tutorial_general_02.png
[3]: ./media/vibehcm-tutorial/tutorial_general_03.png
[4]: ./media/vibehcm-tutorial/tutorial_general_04.png

[100]: ./media/vibehcm-tutorial/tutorial_general_100.png

[200]: ./media/vibehcm-tutorial/tutorial_general_200.png
[201]: ./media/vibehcm-tutorial/tutorial_general_201.png
[202]: ./media/vibehcm-tutorial/tutorial_general_202.png
[203]: ./media/vibehcm-tutorial/tutorial_general_203.png

