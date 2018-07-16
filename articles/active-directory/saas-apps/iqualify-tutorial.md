---
title: 'Tutorial: Integração do Azure Active Directory com iQualify LMS | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e iQualify LMS.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 8a3caaff-dd8d-4afd-badf-a0fd60db3d2c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/04/2017
ms.author: jeedes
ms.openlocfilehash: 3ff5a833da5bbe99c5c6d1576b9775051b3ce07b
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39048094"
---
# <a name="tutorial-azure-active-directory-integration-with-iqualify-lms"></a>Tutorial: Integração do Azure Active Directory com iQualify LMS

Neste tutorial, saiba como integrar iQualify LMS com o Azure Active Directory (Azure AD).

Integrar iQualify LMS no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao iQualify LMS.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para iQualify LMS (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com iQualify LMS, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um iQualify LMS logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando iQualify LMS da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-iqualify-lms-from-the-gallery"></a>Adicionando iQualify LMS da Galeria
Para configurar a integração do iQualify LMS com o Azure AD, terá de adicionar iQualify LMS a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar iQualify LMS a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **iQualify LMS**, selecione **iQualify LMS** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![iQualify LMS na lista de resultados](./media/iqualify-tutorial/tutorial_iqualify_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configurar e testar o Azure AD início de sessão único com iQualify que LMs com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa de saber o que o utilizador de contraparte iQualify LMS for para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no iQualify LMS deve ser estabelecido.

IQualify LMS, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com iQualify LMS, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste LMS iQualify](#create-an-iqualify-lms-test-user)**  - para ter um equivalente da Eduarda Almeida na iQualify LMS que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo de LMS iQualify.

**Para configurar o Azure AD início de sessão único com iQualify LMS, execute os seguintes passos:**

1. No portal do Azure, sobre o **iQualify LMS** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/iqualify-tutorial/tutorial_iqualify_samlbase.png)

3. Sobre o **iQualify LMS domínio e URLs** secção, execute os seguintes passos, se desejar configurar a aplicação no modo de IDP iniciado:

    ![informações de iQualify LMS domínio e URLs de início de sessão único](./media/iqualify-tutorial/tutorial_iqualify_url.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: 
    | |
    |--|--|
    | Ambiente de produção: `https://<yourorg>.iqualify.com/`|
    | Ambiente de teste: `https://<yourorg>.iqualify.io`|
    
    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: 
    | |
    |--|--|
    | Ambiente de produção: `https://<yourorg>.iqualify.com/auth/saml2/callback` |
    | Ambiente de teste: `https://<yourorg>.iqualify.io/auth/saml2/callback` |

4. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![informações de iQualify LMS domínio e URLs de início de sessão único](./media/iqualify-tutorial/tutorial_iqualify_url1.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão:
    | |
    |--|--|
    | Ambiente de produção: `https://<yourorg>.iqualify.com/login` |
    | Ambiente de teste: `https://<yourorg>.iqualify.io/login` |
     
    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o identificador de real, a URL de resposta e o URL de início de sessão. Contacte [equipa de suporte de cliente de LMS iQualify](https://www.iqualify.com) obter esses valores. 

5. O aplicativo de LMS iQualify espera que as asserções de Security Assertion Markup Language (SAML) a ser exibido num formato específico. As declarações de configurar e gerir os valores dos atributos na **atributos de utilizador** secção iQualify página da aplicação integração, conforme mostrado na captura de ecrã seguinte:
    
    ![Configurar o início de sessão único](./media/iqualify-tutorial/atb.png)

6. No **atributos de utilizador** secção sobre o **início de sessão único** diálogo execute os seguintes passos para cada linha, mostrada na tabela abaixo:
    
    | Nome do Atributo | Valor do Atributo |
    | --- | --- |    
    | e-mail | user.userprincipalname |
    | first_name | User.givenName |
    | last_name | User.Surname |
    | person_id | "o atributo" | 

    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único](./media/iqualify-tutorial/atb2.png)

    ![Configurar o início de sessão único](./media/iqualify-tutorial/atb3.png)
    
    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.
    
    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.
    
    d. Clique em **Ok**

    e. Repita os passos "a" através de "d" para as linhas da tabela seguintes. 

    > [!Note]
    > Repetir os passos "a" através de "d" para o **person_id** atributo é **opcional**

7. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base 64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/iqualify-tutorial/tutorial_iqualify_certificate.png) 

8. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/iqualify-tutorial/tutorial_general_400.png)
    
9. Sobre o **iQualify configuração de LMS** secção, clique em **configurar iQualify LMS** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![iQualify LMS configuração](./media/iqualify-tutorial/tutorial_iqualify_configure.png) 

10.  Abra uma nova janela do browser e, em seguida, inicie sessão no seu ambiente de iQualify como administrador.

11. Assim que tiver iniciado a sessão, clique no seu avatar no canto superior direito, em seguida, clique em **"Definições da conta".**

    ![Definições de conta](./media/iqualify-tutorial/setting1.png) 
12. Na área de definições de conta, clique no menu à esquerda da faixa de opções e clique em **"INTEGRAÇÕES."**
    
    ![INTEGRAÇÕES](./media/iqualify-tutorial/setting2.png)

13. Em INTEGRAÇÕES, clique nas **SAML** ícone.

    ![Ícone SAML](./media/iqualify-tutorial/setting3.png)

14. Na **definições de autenticação SAML** diálogo caixa, execute os seguintes passos:

    ![Definições de autenticação SAML](./media/iqualify-tutorial/setting4.png)

    a. Na **URL do serviço Diante de início de sessão único SAML** caixa, cole a **URL de serviço do SAML único Sign‑On** valor copiados a partir da janela de configuração de aplicação do Azure AD.
    
    b. Na **URL de fim de sessão de SAML** caixa, cole a **URL de Sign‑Out** valor copiados a partir da janela de configuração de aplicação do Azure AD.
    
    c. Abra o ficheiro de certificado transferido no bloco de notas, copie o conteúdo e, em seguida, cole-a no **certificado público** caixa.
    
    d. Na **etiqueta do botão de início de sessão** introduza o nome do botão a ser apresentado na página de início de sessão.
    
    e. Clique em **GUARDAR**.

    f. Clique em **ATUALIZAÇÃO**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/iqualify-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/iqualify-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/iqualify-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/iqualify-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-an-iqualify-lms-test-user"></a>Criar um utilizador de teste LMS iQualify

Nesta secção, um usuário chamado Eduarda Almeida é criado na iQualify. iQualify LMS suporta o aprovisionamento de utilizadores just‑in‑time, que está ativado por predefinição.

Não existe nenhum item de ação para nesta secção. Se um utilizador já não existir no iQualify, uma nova é criada quando tentar acessar iQualify LMS.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso aos iQualify LMS.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a iQualify LMS, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **iQualify LMS**.

    ![A ligação LMS iQualify na lista de aplicações](./media/iqualify-tutorial/tutorial_iqualify_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica iQualify LMS mosaico no painel de acesso, deve obter a página de início de sessão da sua aplicação de LMS iQualify. 

   ![página de início de sessão](./media/iqualify-tutorial/login.png) 

Clique em **iniciar sessão no Azure AD** botão e deve obter automaticamente sessão iniciada em seu aplicativo de LMS iQualify.

Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/iqualify-tutorial/tutorial_general_01.png
[2]: ./media/iqualify-tutorial/tutorial_general_02.png
[3]: ./media/iqualify-tutorial/tutorial_general_03.png
[4]: ./media/iqualify-tutorial/tutorial_general_04.png

[100]: ./media/iqualify-tutorial/tutorial_general_100.png

[200]: ./media/iqualify-tutorial/tutorial_general_200.png
[201]: ./media/iqualify-tutorial/tutorial_general_201.png
[202]: ./media/iqualify-tutorial/tutorial_general_202.png
[203]: ./media/iqualify-tutorial/tutorial_general_203.png

