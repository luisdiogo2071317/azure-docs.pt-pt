---
title: 'Tutorial: Integração do Azure Active Directory com absorver LMS | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e absorver LMS.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: ba9f1b3d-a4a0-4ff7-b0e7-428e0ed92142
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: jeedes
ms.openlocfilehash: 066ae92056e4b80b6627b371d6ebeb3235b2781d
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39043783"
---
# <a name="tutorial-azure-active-directory-integration-with-absorb-lms"></a>Tutorial: Integração do Azure Active Directory com absorver LMS

Neste tutorial, saiba como integrar absorver LMS com o Azure Active Directory (Azure AD).

Integração de LMS absorver com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao absorver LMS.
- Pode permitir que os utilizadores iniciem sessão automaticamente nas absorver LMS (por meio de início de sessão único) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central, o portal do Azure.

Se quiser saber mais sobre o software como uma integração de aplicação de serviço (SaaS) com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com absorver LMS, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Uma LMS absorver logon único habilitado subscrição

> [!NOTE]
> Recomendamos que não utilize um ambiente de produção para este tutorial.

Para testar os passos neste tutorial, siga as seguintes recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

* Adicionando absorver LMS da Galeria
* Configuração e teste do Azure AD início de sessão único

## <a name="add-absorb-lms-from-the-gallery"></a>Adicionar absorver LMS a partir da Galeria
Para configurar a integração de LMS absorver com o Azure AD, adicione absorver LMS a partir da Galeria à sua lista de aplicações de SaaS geridas.

Para adicionar absorver LMS a partir da galeria, faça o seguinte:

1. Na [portal do Azure](https://portal.azure.com), no painel esquerdo, selecione a **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Aceda a **aplicações empresariais** > **todas as aplicações**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar uma aplicação, selecione o **nova aplicação** botão.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **absorver LMS**, selecione **absorver LMS** na aplicação de severas painel e, em seguida, selecione o **Add** botão.

    ![Absorver LMS na lista de resultados](./media/absorblms-tutorial/tutorial_absorblms_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com LMS absorver com base num utilizador de teste chamado Eduarda Almeida.

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de equivalente de LMS absorver no Azure AD. Em outras palavras, tem de estabelecer uma relação de ligação entre um utilizador no Azure AD e o utilizador correspondente no absorver LMS.

Estabelecer esta relação de ligação ao atribuir a *nome de utilizador* valor no Azure AD como o *nome de utilizador* valor na absorver LMS.

Para configurar e testar o Azure AD início de sessão único com absorver LMS, conclua os blocos de construção nas seções a seguir cinco.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo de absorver LMS.

Para configurar o Azure AD início de sessão único com absorver LMS, faça o seguinte:

1. No portal do Azure, sobre o **absorver LMS** página de integração de aplicações, selecione **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Na **início de sessão único** caixa de diálogo a **modo** caixa, selecione **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/absorblms-tutorial/tutorial_absorblms_samlbase.png)

3. Na **domínio de LMS absorver e URLs** secção, efetue o seguinte:

    ![Absorver LMS domínio e URLs únicas início de sessão em informações](./media/absorblms-tutorial/tutorial_absorblms_url.png)

    a. Na **identificador** , escreva um URL que utiliza a seguinte sintaxe: `https://<subdomain>.myabsorb.com/Account/SAML`.

    b. Na **URL de resposta** , escreva um URL que utiliza a seguinte sintaxe: `https://<subdomain>.myabsorb.com/Account/SAML`.
     
    > [!NOTE] 
    > Estes URLs não são os valores reais. Atualize-os com o identificador e os URLs de resposta real. Para obter estes valores, entre em contato com o [equipa de suporte de cliente de LMS absorver](https://www.absorblms.com/support). 

4. Na **certificado de assinatura SAML** na secção a **transferir** coluna, selecione **metadados XML**e, em seguida, guarde o ficheiro de metadados para o seu computador.

    ![O link de download de certificado assinatura](./media/absorblms-tutorial/tutorial_absorblms_certificate.png) 

5. Selecione **Guardar**.

    ![Configurar o botão único início de sessão em Guardar](./media/absorblms-tutorial/tutorial_general_400.png)
    
6. Na **absorver configuração de LMS** secção, selecione **configurar LMS absorver** para abrir **configurar início de sessão** janela e copie o **URL de fim de sessão** no **secção de referência rápida.**

    ![O painel de configuração de LMS absorver](./media/absorblms-tutorial/tutorial_absorblms_configure.png) 

7. Numa nova janela do browser web, inicie sessão no site da sua empresa absorver LMS como administrador.

8. Selecione o **conta** botão na parte superior direita. 

    ![O botão de conta](./media/absorblms-tutorial/1.png)

9. No painel de conta, selecione **as definições do Portal**.

    ![A ligação de definições do Portal](./media/absorblms-tutorial/2.png)
    
10. Selecione o separador **Utilizadores**.

    ![O separador de utilizadores](./media/absorblms-tutorial/3.png)

11. Na página de configuração de início de sessão único, faça o seguinte:

    ![A página de configuração de início de sessão único](./media/absorblms-tutorial/4.png)

    a. Na **modo** caixa, selecione **fornecedor de identidade iniciada pelo**.

    b. No bloco de notas, abra o certificado que transferiu a partir do portal do Azure. Remover os **---BEGIN CERTIFICATE---** e **---END CERTIFICATE---** etiquetas. Em seguida, na **chave** caixa, cole os conteúdos restantes.
    
    c. Na **propriedade de Id** caixa, selecione o atributo que configurou como o identificador de utilizador no Azure AD. Por exemplo, se *userPrincipalName* está selecionado no Azure AD, selecione **Username**.

    d. Na **URL de início de sessão** caixa, cole a **URL de acesso de utilizador** partir do aplicativo **propriedades** página do portal do Azure.

    e. Na **URL de fim de sessão**, cole a **URL de fim de sessão** valor que copiou do **configurar início de sessão** janela do portal do Azure.

12. Botão de alternar **permitir apenas o início de sessão SSO** ao **no**.

    ![O botão de alternar apenas permitir o SSO início de sessão](./media/absorblms-tutorial/5.png)

13. Selecione **guardar.**

> [!TIP]
> Pode ler uma versão concisa destas instruções no [portal do Azure](https://portal.azure.com) enquanto estiver a configurar a aplicação. Depois de adicionar a aplicação a partir da **do Active Directory** > **aplicações empresariais** secção, selecione o **Single Sign-On** separador e acessar o embedded documentação através da **configuração** seção na parte inferior. Para obter mais informações, consulte [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

Nesta secção, vai criar o utilizador de teste Eduarda Almeida no portal do Azure.

![Criar um utilizador de teste do Azure AD][100]

Para criar um utilizador de teste no Azure AD, faça o seguinte:

1. No portal do Azure, no painel esquerdo, selecione **do Azure Active Directory**.

    ![O botão do Azure Active Directory](./media/absorblms-tutorial/create_aaduser_01.png) 

2. Para apresentar a lista de utilizadores, selecione **utilizadores e grupos** > **todos os utilizadores**.
    
    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/absorblms-tutorial/create_aaduser_02.png) 

3. Na parte superior da caixa de diálogo, selecione **adicionar**.
 
    ![Botão Adicionar](./media/absorblms-tutorial/create_aaduser_03.png) 

4. Na **utilizador** diálogo caixa, faça o seguinte:
 
    ![A caixa de diálogo de utilizador](./media/absorblms-tutorial/create_aaduser_04.png) 

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, escreva o endereço de e-mail da Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, observe o valor a **palavra-passe** caixa.

    d. Selecione **Criar**.

### <a name="create-an-absorb-lms-test-user"></a>Criar um utilizador de teste de LMS absorver

Para utilizadores do Azure AD iniciar sessão no absorver LMS, eles tem de ser definidos no absorver LMS.  

Para absorver LMS, o programa de configuração é uma tarefa manual.

Para configurar uma conta de utilizador, faça o seguinte:

1. Inicie sessão no site da sua empresa absorver LMS como um administrador.

2. No painel esquerdo, selecione **utilizadores**.

    ![A ligação de utilizadores de LMS absorver](./media/absorblms-tutorial/absorblms_users.png)

3. Na **usuários** painel, selecione **utilizadores**.

    ![A ligação de utilizadores](./media/absorblms-tutorial/absorblms_userssub.png)

4. Na **adicionar novo** na lista pendente, selecione **utilizador**.

    ![A adicionar novo na lista pendente](./media/absorblms-tutorial/absorblms_createuser.png)

5. Sobre o **adicionar utilizador** página, efetue o seguinte:

    ![A página Adicionar utilizador](./media/absorblms-tutorial/user.png)

    a. Na **nome próprio** , escreva o nome, tal como **Eduarda**.

    b. Na **sobrenome** , escreva o apelido, tal como **Simon**.
    
    c. Na **nome de utilizador** , escreva um nome completo, tal como **Eduarda Almeida**.

    d. Na **palavra-passe** , escreva a palavra-passe de Eduarda Almeida.

    e. Na **Confirmar palavra-passe** caixa, escreva novamente a palavra-passe.
    
    f. Definir o **é Active** alternar para **Active Directory**.  

6. Selecione **guardar.**
 
### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar utilizador Eduarda Almeida para utilizar o Azure início de sessão único, concedendo acesso para absorver LMS.

![Atribuir a função de utilizador][200]

Para atribuir utilizador Eduarda Almeida a absorver LMS, faça o seguinte:

1. No portal do Azure, abrir a vista de aplicações, vá para a vista de diretório e, em seguida, selecione **aplicações empresariais** > **todas as aplicações**.

    ![A ligação de "Todas as aplicações"][201] 

2. Na **aplicativos** lista, selecione **absorver LMS**.

    ![A ligação de LMS absorver na lista de aplicações](./media/absorblms-tutorial/tutorial_absorblms_app.png) 

3. No painel esquerdo, selecione **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202] 

4. Selecione **Add** e, em seguida, no **adicionar atribuição** painel, selecione **utilizadores e grupos**.

    ![O painel Adicionar atribuição][203]

5. Na **utilizadores e grupos** caixa de diálogo a **utilizadores** lista, selecione **Eduarda Almeida**.

6. Na **utilizadores e grupos** caixa de diálogo, selecione a **selecione** botão.

7. Na **adicionar atribuição** caixa de diálogo, selecione a **atribuir** botão.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar seu única início de sessão em configuração do Azure AD utilizando o painel de acesso.

No painel de acesso, selecionando o **absorver LMS** mosaico automaticamente iniciada para a sua aplicação de absorver LMS. Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/absorblms-tutorial/tutorial_general_01.png
[2]: ./media/absorblms-tutorial/tutorial_general_02.png
[3]: ./media/absorblms-tutorial/tutorial_general_03.png
[4]: ./media/absorblms-tutorial/tutorial_general_04.png

[100]: ./media/absorblms-tutorial/tutorial_general_100.png

[200]: ./media/absorblms-tutorial/tutorial_general_200.png
[201]: ./media/absorblms-tutorial/tutorial_general_201.png
[202]: ./media/absorblms-tutorial/tutorial_general_202.png
[203]: ./media/absorblms-tutorial/tutorial_general_203.png
