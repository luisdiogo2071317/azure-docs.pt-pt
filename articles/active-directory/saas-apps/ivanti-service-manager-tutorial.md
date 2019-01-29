---
title: 'Tutorial: Integração do Active Directory do Azure com o Gestor de serviço de Ivanti (ISM) | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Gestor de serviço Ivanti (ISM).
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 14297c74-0d57-4146-97fa-7a055fb73057
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/20/2018
ms.author: jeedes
ms.openlocfilehash: 7a125ea70a891d8db0ec652844a99153f19253bd
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55150296"
---
# <a name="tutorial-azure-active-directory-integration-with-ivanti-service-manager-ism"></a>Tutorial: Integração do Active Directory do Azure com o Gestor de serviço de Ivanti (ISM)

Neste tutorial, saiba como integrar o Gestor de serviço Ivanti (ISM) com o Azure Active Directory (Azure AD).

Integrar o Gestor de serviço Ivanti (ISM) no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso para o Gestor de serviço de Ivanti (ISM).
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Ivanti serviço ISM (Manager) (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Gestor de serviço de Ivanti (ISM), terá dos seguintes itens:

- Uma subscrição do Azure
- Um Gestor de serviço Ivanti (ISM) início de sessão único de subscrição ativada

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Ivanti Service Manager (ISM) da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-ivanti-service-manager-ism-from-the-gallery"></a>Adicionando Ivanti Service Manager (ISM) da Galeria

Para configurar a integração do Gestor de serviço Ivanti (ISM) para o Azure AD, terá de adicionar Ivanti Service Manager (ISM) a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Ivanti Service Manager (ISM) a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]

3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **Ivanti Service Manager (ISM)**, selecione **Ivanti Service Manager (ISM)** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Gestor de serviço Ivanti (ISM) na lista de resultados](./media/ivanti-service-manager-tutorial/tutorial-ivanti-service-manager-addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com o Gestor de serviço de Ivanti (ISM) com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Gestor de serviço Ivanti (ISM) a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Gestor de serviço Ivanti (ISM) deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com o Gestor de serviço de Ivanti (ISM), tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste Ivanti Service Manager (ISM)](#creating-an-ivanti-service-manager-ism-test-user)**  - para ter um equivalente da Eduarda Almeida no Gestor de serviço Ivanti (ISM), que está ligado à representação de utilizador do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação do Gestor de serviço Ivanti (ISM).

**Para configurar o Azure AD início de sessão único com o Gestor de serviço de Ivanti (ISM), execute os seguintes passos:**

1. No portal do Azure, sobre o **Ivanti Service Manager (ISM)** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **selecionar um método de início de sessão único** caixa de diálogo, clique em **selecione** para **SAML** modo para ativar o início de sessão único.

    ![Configurar o início de sessão único](common/tutorial-general-301.png)

3. Sobre o **definir a segurança de início de sessão único com o SAML** página, clique em **editar** ícone para abrir **configuração básica de SAML** caixa de diálogo.

    ![Configurar o início de sessão único](common/editconfigure.png)

4. Sobre o **configuração básica de SAML** secção, execute os seguintes passos, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

    ![URLs de domínio do Gestor de serviço Ivanti (ISM) e únicas início de sessão em informações](./media/ivanti-service-manager-tutorial/tutorial-ivanti-service-manager-url.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão:
    | |
    |--|
    | `https://<customer>.saasit.com/` |
    | `https://<customer>.saasiteu.com/` |
    | `https://<customer>.saasitau.com/` |

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<customer>/handlers/sso/SamlAssertionConsumerHandler.ashx`

5. Clique em **definir URLs adicionais** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![URLs de domínio do Gestor de serviço Ivanti (ISM) e únicas início de sessão em informações](./media/ivanti-service-manager-tutorial/tutorial-ivanti-service-manager-url1.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<customer>.saasit.com/`

    > [!NOTE]
    > Estes valores não são reais. Atualize estes valores com o identificador de real, a URL de resposta e o URL de início de sessão. Contacte [equipa de suporte de cliente do Gestor de serviço Ivanti (ISM)](https://www.ivanti.com/support/contact) obter esses valores.

6. No **certificado de assinatura SAML** página, além da **certificado de assinatura SAML** secção, clique em **transferir** para transferir **certificado (bruto)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/ivanti-service-manager-tutorial/tutorial-ivanti-service-manager-certificate.png) 

7. Sobre o **definir segurança Ivanti Service Manager ISM ()** secção, copie o URL adequado, de acordo com seus requisitos.

    a. URL de início de sessão

    b. Identificador do Azure AD

    c. URL de fim de sessão

    ![Configuração do Gestor de serviço (. ISM) de Ivanti](common/configuresection.png)

8. Para configurar o início de sessão único num **Ivanti Service Manager (ISM)** lado, terá de enviar o transferido **certificado (bruto)** e copiado **URL de início de sessão**,  **Identificador do Azure AD**, **URL de fim de sessão** para [equipa de suporte do Gestor de serviço Ivanti (ISM)](https://www.ivanti.com/support/contact). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

1. No portal do Azure, no painel esquerdo, selecione **do Azure Active Directory**, selecione **utilizadores**e, em seguida, selecione **todos os utilizadores**.

    ![Criar utilizador do Azure AD][100]

2. Selecione **novo utilizador** na parte superior do ecrã.

    ![Criar um utilizador de teste do Azure AD](common/create-aaduser-01.png) 

3. Nas propriedades do utilizador, execute os seguintes passos.

    ![Criar um utilizador de teste do Azure AD](common/create-aaduser-02.png)

    a. Na **Name** , insira **BrittaSimon**.
  
    b. Na **nome de utilizador** , digite **brittasimon@yourcompanydomain.extension**  
    Por exemplo, BrittaSimon@contoso.com

    c. Selecione **propriedades**, selecione a **palavra-passe de Show** caixa de verificação e, em seguida, anote o valor que é apresentado na caixa de palavra-passe.

    d. Selecione **Criar**.

### <a name="creating-an-ivanti-service-manager-ism-test-user"></a>Criar um utilizador de teste Ivanti Service Manager (ISM)

O objetivo desta secção é criar um utilizador chamado Eduarda Almeida na Ivanti Service Manager ISM (). Gestor de serviço Ivanti (ISM) suporta o aprovisionamento de just-in-time, que está por predefinição, ativada. Não existe nenhum item de ação para nesta secção. Um novo utilizador é criado durante uma tentativa de aceder Ivanti Service Manager (ISM) se não existir ainda.
>[!Note]
>Se precisar de criar manualmente um utilizador, contacte [equipa de suporte do Gestor de serviço Ivanti (ISM)](https://www.ivanti.com/support/contact).

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso para o Gestor de serviço de Ivanti (ISM).

1. No portal do Azure, selecione **aplicações empresariais**, selecione **todos os aplicativos**.

    ![Atribuir utilizador][201]

2. Na lista de aplicações, selecione **Ivanti Service Manager (ISM)**.

    ![Configurar o início de sessão único](./media/ivanti-service-manager-tutorial/tutorial-ivanti-service-manager-app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. Na **utilizadores e grupos** caixa de diálogo select **Eduarda Almeida** na lista de utilizadores, em seguida, clique o **selecionar** na parte inferior do ecrã.

6. Na **adicionar atribuição** caixa de diálogo select a **atribuir** botão.

### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico do Gestor de serviço Ivanti (ISM) no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação do Gestor de serviço Ivanti (ISM).
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: common/tutorial-general-01.png
[2]: common/tutorial-general-02.png
[3]: common/tutorial-general-03.png
[4]: common/tutorial-general-04.png

[100]: common/tutorial-general-100.png

[201]: common/tutorial-general-201.png
[202]: common/tutorial-general-202.png
[203]: common/tutorial-general-203.png
