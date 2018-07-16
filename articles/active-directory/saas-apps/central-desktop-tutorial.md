---
title: 'Tutorial: Integração do Azure Active Directory com o ambiente de trabalho Central | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o ambiente de trabalho Central.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: b805d485-93db-49b4-807a-18d446c7090e
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: jeedes
ms.openlocfilehash: 2c571ebc45875167373b592abc720b045d1835bd
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39048760"
---
# <a name="tutorial-azure-active-directory-integration-with-central-desktop"></a>Tutorial: Integração do Azure Active Directory com o ambiente de trabalho Central

Neste tutorial, saiba como integrar o ambiente de trabalho Central com o Azure Active Directory (Azure AD).

Integrar o ambiente de trabalho Central com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso à área de trabalho Central.
- Pode permitir que os utilizadores automaticamente é feita ambiente de trabalho Central com as suas contas do Azure AD.
- Pode gerir as suas contas num local central, o portal do Azure.

Para obter mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o ambiente de trabalho Central, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Uma subscrição do ambiente de trabalho Central única início de sessão-no-ativada

> [!NOTE]
> Não é recomendado utilizar um ambiente de produção para testar os passos neste tutorial.

Para testar os passos neste tutorial, siga as seguintes recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se ainda não tiver um ambiente de avaliação do Azure AD, [obtenha uma avaliação gratuita de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário que é descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o ambiente de trabalho Central da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="add-central-desktop-from-the-gallery"></a>Adicionar área de trabalho Central a partir da Galeria
Para configurar a integração do ambiente de trabalho Central para o Azure AD, terá de adicionar área de trabalho Central a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Central do ambiente de trabalho a partir da galeria, siga os passos seguintes:**

1. Na [portal do Azure](https://portal.azure.com), no painel esquerdo, selecione a **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Aceda a **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar novos aplicativos, selecione o **nova aplicação** botão na parte superior da caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **Central Desktop**. Selecione **Central Desktop** no painel de resultados e, em seguida, selecione a **Add** botão para adicionar a aplicação.

    ![Ambiente de trabalho central na lista de resultados](./media/central-desktop-tutorial/tutorial_centraldesktop_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com o ambiente de trabalho Central com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber que o utilizador de contraparte no ambiente de trabalho Central for para um utilizador no Azure AD. Em outras palavras, tem de estabelecer uma ligação entre um utilizador do Azure AD e um utilizador relacionado no Central Desktop.

Na área de trabalho Central, atribua **nome de utilizador** o mesmo valor como **nome de utilizador** no Azure AD. Agora que estabeleceu a ligação entre os dois usuários.

Para configurar e testar o Azure AD início de sessão único com o ambiente de trabalho Central, tem de concluir os seguintes blocos de construção:

1. [Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on) para permitir aos utilizadores utilizar esta funcionalidade.
2. [Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user) para testar o Azure AD início de sessão único com Eduarda Almeida.
3. [Criar um utilizador de teste do ambiente de trabalho Central](#create-a-central-desktop-test-user) ter um equivalente da Eduarda Almeida na área de trabalho Central que está ligada à representação de utilizador do Azure AD.
4. [Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user) para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. [Testar início de sessão único](#test-single-sign-on) para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação de ambiente de trabalho Central.

**Para configurar o Azure AD início de sessão único com o ambiente de trabalho Central, siga os passos seguintes:**

1. No portal do Azure, sobre o **Central Desktop** página de integração de aplicações, selecione **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Para ativar o início de sessão único, na **início de sessão único** caixa de diálogo a **modo** na lista pendente, selecione **baseado em SAML logon**.
 
    ![Caixa de diálogo de início de sessão único](./media/central-desktop-tutorial/tutorial_centraldesktop_samlbase.png)

3. Na **Central de ambiente de trabalho de domínio e URLs** secção, siga os passos seguintes:

    ![Domínio de ambiente de trabalho central e URLs único informações de início de sessão](./media/central-desktop-tutorial/tutorial_centraldesktop_url.png)

    a. Na **URL de início de sessão** caixa, escreva um URL com o seguinte padrão: `https://<companyname>.centraldesktop.com`

    b. Na **identificador** caixa, escreva um URL com o seguinte padrão:
    | |
    |--|
    | `https://<companyname>.centraldesktop.com/saml2-metadata.php`|
    | `https://<companyname>.imeetcentral.com/saml2-metadata.php`|

    c. Na **URL de resposta** caixa, escreva um URL com o seguinte padrão: `https://<companyname>.centraldesktop.com/saml2-assertion.php`    
     
    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o identificador real, URL de resposta e início de sessão no URL. Contacte os [equipa de suporte de cliente de ambiente de trabalho Central](https://imeetcentral.com/contact-us) obter esses valores. 

4. Na **certificado de assinatura SAML** secção, selecione **certificado**. Em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/central-desktop-tutorial/tutorial_centraldesktop_certificate.png) 

5. Selecione o botão **Guardar**.

    ![Configurar o início de sessão único botão Save](./media/central-desktop-tutorial/tutorial_general_400.png)
    
6. Na **configuração do ambiente de trabalho Central** secção, selecione **configurar o ambiente de trabalho Central** para abrir o **configurar início de sessão** janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir a **referência rápida** secção.

    ![Configuração de ambiente de trabalho central](./media/central-desktop-tutorial/tutorial_centraldesktop_configure.png) 

7. Inicie sessão no seu **Central Desktop** inquilino.

8. Aceda a **definições**. Selecione **avançadas**e, em seguida, selecione **início de sessão único**.

    ![Configuração - Advanced](./media/central-desktop-tutorial/ic769563.png "configuração - avançada")

9. Sobre o **início de sessão único em definições** página, siga os passos seguintes:

    ![Único configurações de logon](./media/central-desktop-tutorial/ic769564.png "início de sessão único em definições")
    
    a. Selecione **ativar SAML v2 início de sessão único**.
    
    b. Na **URL de SSO** caixa, cole a **ID de entidade de SAML** valor que copiou do portal do Azure.
    
    c. Na **URL de início de sessão de SSO** caixa, cole a **SAML único início de sessão no URL do serviço** valor que copiou do portal do Azure.
    
    d. Na **URL de fim de sessão de SSO** caixa, cole a **URL de fim de sessão** valor que copiou do portal do Azure.

10. Na **método de verificação de assinatura de mensagem** secção, siga os passos seguintes:

    ![Método de verificação de assinatura da mensagem](./media/central-desktop-tutorial/ic769565.png "método de verificação de assinatura de mensagem") um. Selecione **Certificado**.
    
    b. Na **certificado SSO** lista, selecione **RSH SHA256**.
    
    c. Abra o seu certificado transferido no bloco de notas. Em seguida, copie o conteúdo de certificado e colá-lo no **certificado SSO** campo.
        
    d. Selecione **apresentar uma ligação à sua página de início de sessão SAMLv2**.
    
    e. Selecione **atualização**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com) enquanto estiver a configurar a aplicação. Depois de adicionar esta aplicação a partir da **do Active Directory** > **aplicações empresariais** secção, selecione o **Single Sign-On** separador e, em seguida, acessar o embedded documentação através da **configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded em [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, siga os passos seguintes:**

1. No portal do Azure, no painel esquerdo, selecione o **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/central-desktop-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**. Em seguida, selecione **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/central-desktop-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, selecione **Add** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/central-desktop-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, siga os passos seguintes:

    ![A caixa de diálogo de utilizador](./media/central-desktop-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Selecione **Criar**.
 
### <a name="create-a-central-desktop-test-user"></a>Criar um utilizador de teste do ambiente de trabalho Central

Para utilizadores do Azure AD possam iniciar sessão, tem de ser aprovisionados no aplicativo de área de trabalho Central. Esta secção descreve como criar contas de utilizador do Azure AD no ambiente de trabalho Central.

> [!NOTE]
> Para aprovisionar contas de utilizador do Azure AD, pode utilizar quaisquer outras ferramentas de criação da conta de utilizador de ambiente de trabalho Central ou APIs fornecidas pelo ambiente de trabalho Central.

**Para aprovisionar contas de utilizador ao ambiente de trabalho Central:**

1. Inicie sessão no seu inquilino de ambiente de trabalho Central.

2. Aceda a **pessoas** > **membros internos**.

3. Selecione **adicionar membros internos**.

    ![As pessoas](./media/central-desktop-tutorial/ic781051.png "pessoas")
    
4. Na **endereço de E-Mail de novos membros** , escreva uma conta do Azure AD que pretende aprovisionar e, em seguida, selecione **próxima**.

    ![Endereços de novos membros de correio eletrónico](./media/central-desktop-tutorial/ic781052.png "endereços dos novos membros de E-Mail")

5. Selecione **membro ou membros adicionar interno**.

    ![Adicionar membro interno](./media/central-desktop-tutorial/ic781053.png "Adicionar membro interno")
   
   >[!NOTE]
   >Os utilizadores que adicionar recebem um e-mail que inclui uma ligação de confirmação para a ativação de suas contas.
   
### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar utilizador Eduarda Almeida para utilizar o Azure início de sessão único, concedendo-lhes acesso à área de trabalho Central.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a Central de ambiente de trabalho, siga os passos seguintes:**

1. No portal do Azure, abra a vista de aplicações. Ir para a vista de diretório e, em seguida, aceda a **aplicações empresariais**.

2. Selecione **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Central Desktop**.

    ![A ligação de ambiente de trabalho Central na lista de aplicações](./media/central-desktop-tutorial/tutorial_centraldesktop_app.png)  

3. No menu à esquerda, selecione **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Selecione o **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. Na **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** no **utilizadores** lista.

6. Na **utilizadores e grupos** caixa de diálogo, clique nas **selecione** botão.

7. Na **adicionar atribuição** caixa de diálogo, selecione a **atribuir** botão.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, teste a configuração do Azure AD única início de sessão utilizando o painel de acesso.

Ao selecionar o mosaico de ambiente de trabalho Central no painel de acesso, automaticamente obter conectado à sua aplicação de ambiente de trabalho Central.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/central-desktop-tutorial/tutorial_general_01.png
[2]: ./media/central-desktop-tutorial/tutorial_general_02.png
[3]: ./media/central-desktop-tutorial/tutorial_general_03.png
[4]: ./media/central-desktop-tutorial/tutorial_general_04.png

[100]: ./media/central-desktop-tutorial/tutorial_general_100.png

[200]: ./media/central-desktop-tutorial/tutorial_general_200.png
[201]: ./media/central-desktop-tutorial/tutorial_general_201.png
[202]: ./media/central-desktop-tutorial/tutorial_general_202.png
[203]: ./media/central-desktop-tutorial/tutorial_general_203.png

