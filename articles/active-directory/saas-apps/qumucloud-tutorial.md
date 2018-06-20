---
title: 'Tutorial: Integração do Azure Active Directory Qumu nuvem | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e Qumu nuvem.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d8c4a97b-4de6-49d4-b64e-42222c2ec6c9
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2018
ms.author: jeedes
ms.openlocfilehash: 488c9c62138cf9b042de749c35b8e8932b1da30f
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36226940"
---
# <a name="tutorial-azure-active-directory-integration-with-qumu-cloud"></a>Tutorial: Integração do Azure Active Directory Qumu nuvem

Neste tutorial, irá aprender a integrar Qumu Cloud com o Azure Active Directory (Azure AD).

Integrar Qumu Cloud com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso à nuvem Qumu.
- Pode permitir aos utilizadores automaticamente obter com sessão iniciada para nuvem Qumu (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Qumu nuvem, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Uma nuvem Qumu-início de sessão único ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. A adição de nuvem Qumu de galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-qumu-cloud-from-the-gallery"></a>A adição de nuvem Qumu de galeria
Para configurar a integração da nuvem Qumu com o Azure AD, tem de adicionar Qumu Cloud na Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar Qumu Cloud a partir da galeria do, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **Qumu nuvem**, selecione **Qumu nuvem** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![Nuvem Qumu na lista de resultados](./media/qumucloud-tutorial/tutorial_qumucloud_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configura e teste do Azure AD-início de sessão único com Qumu nuvem com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo na nuvem Qumu é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado na nuvem Qumu tem de ser estabelecida.

Para configurar e testar o Azure AD-início de sessão único com Qumu nuvem, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste de nuvem Qumu](#create-a-qumu-cloud-test-user)**  - para ter um homólogo de Britta Simon na nuvem Qumu que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação de nuvem Qumu.

**Para configurar o Azure AD-início de sessão único com Qumu nuvem, execute os seguintes passos:**

1. No portal do Azure, no **Qumu nuvem** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/qumucloud-tutorial/tutorial_qumucloud_samlbase.png)

3. No **Qumu nuvem domínio e os URLs** secção, execute os seguintes passos, se pretender configurar a aplicação no **IDP** iniciada modo:

    ![Domínio de nuvem Qumu e URLs único informações de início de sessão](./media/qumucloud-tutorial/tutorial_qumucloud_url.png)

    a. No **identificador** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://<subdomain>.qumucloud.com/saml/SSO`

    b. No **URL de resposta** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://<subdomain>.qumucloud.com/saml/SSO`

4. Verifique **Mostrar avançadas definições de URL** e executar o passo seguinte, se pretender configurar a aplicação no **SP** iniciada modo:

    ![Domínio de nuvem Qumu e URLs único informações de início de sessão](./media/qumucloud-tutorial/tutorial_qumucloud_url1.png)

    No **URL de início de sessão** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://<subdomain>.qumucloud.com`
     
    > [!NOTE] 
    > Estes valores não estiverem reais. Atualize estes valores com o identificador de real, a URL de resposta e o URL de início de sessão. Contacte [equipa de suporte de cliente de Cloud Qumu](mailto:support@qumu.com) para obter estes valores.

4. Aplicação de nuvem Qumu espera as asserções de SAML num formato específico. Configure as seguintes afirmações para esta aplicação. Pode gerir os valores destes atributos a partir de "**atributos de utilizador**" secção na página de integração de aplicações. A seguinte captura de ecrã mostra um exemplo para este.
    
    ![Configurar o início de sessão único](./media/qumucloud-tutorial/attribute.png)
    
5. Clique em **ver e editar todos os outros atributos de utilizador** caixa de verificação no **atributos de utilizador** secção para expandir os atributos. Execute os seguintes passos em cada um dos atributos apresentadas-

    | Nome do Atributo | Valor do Atributo |
    | ---------------| --------------- |    
    | urn:oid:2.5.4.42 | User.givenName |
    | urn:oid:2.5.4.4 | User.Surname |
    | urn:oid:0.9.2342.19200300.100.1.3 | User.Mail |
    | urn:oid:0.9.2342.19200300.100.1.1 | user.userprincipalname |

    a. Clique no atributo para abrir o **Editar atributo** janela.

    ![Configurar o início de sessão único](./media/qumucloud-tutorial/tutorial_attribute_04.png)

    b. No **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    ![Configurar o início de sessão único](./media/qumucloud-tutorial/tutorial_attribute_05.png)

    c. Do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d. Manter o **espaço de nomes** caixa de texto em branco.
    
    e. Clique em **OK**.

5. No **certificado de assinatura de SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![A hiperligação de transferência do certificado](./media/qumucloud-tutorial/tutorial_qumucloud_certificate.png) 

6. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/qumucloud-tutorial/tutorial_general_400.png)
    
7. Para configurar o início de sessão único em **Qumu nuvem** lado, terá de enviar o transferido **XML de metadados** para [equipa de suporte de nuvem Qumu](mailto:support@qumu.com). Se definir esta definição para que a ligação de SAML SSO corretamente em ambos os lados.

> [!TIP]
> Pode agora ler estas instruções dentro de uma versão concisa o [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir do **do Active Directory > aplicações da empresa** secção, basta clicar no **Single Sign-On** separador e aceder à documentação do embedded através de **configuração** secção na parte inferior. Pode ler mais sobre a funcionalidade de documentação incorporados aqui: [do Azure AD incorporado documentação]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/qumucloud-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/qumucloud-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/qumucloud-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/qumucloud-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-qumu-cloud-test-user"></a>Criar um utilizador de teste Qumu nuvem

O objetivo desta secção consiste em criar um utilizador chamado Britta Simon na nuvem Qumu. Nuvem Qumu suportam o aprovisionamento de just-in-time, que é por predefinição ativada. Não há nenhum item de ação para si nesta secção. Um novo utilizador é criado durante a tentativa de aceder Qumu nuvem se não existir ainda.
>[!Note]
>Se precisar de criar manualmente um utilizador, contacte [equipa de suporte de cliente de Cloud Qumu](mailto:support@qumu.com).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso à nuvem Qumu.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon Qumu nuvem, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **Qumu nuvem**.

    ![A ligação de nuvem Qumu na lista de aplicações](./media/qumucloud-tutorial/tutorial_qumucloud_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico de nuvem Qumu no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação de nuvem Qumu.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/qumucloud-tutorial/tutorial_general_01.png
[2]: ./media/qumucloud-tutorial/tutorial_general_02.png
[3]: ./media/qumucloud-tutorial/tutorial_general_03.png
[4]: ./media/qumucloud-tutorial/tutorial_general_04.png

[100]: ./media/qumucloud-tutorial/tutorial_general_100.png

[200]: ./media/qumucloud-tutorial/tutorial_general_200.png
[201]: ./media/qumucloud-tutorial/tutorial_general_201.png
[202]: ./media/qumucloud-tutorial/tutorial_general_202.png
[203]: ./media/qumucloud-tutorial/tutorial_general_203.png

