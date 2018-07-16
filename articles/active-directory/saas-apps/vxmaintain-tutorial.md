---
title: 'Tutorial: Integração do Azure Active Directory com vxMaintain | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e vxMaintain.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 841a1066-593c-4603-9abe-f48496d73d10
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.openlocfilehash: 6362bcb701b444c8cd71b270222ce4f87b4cc2e3
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39055863"
---
# <a name="tutorial-azure-active-directory-integration-with-vxmaintain"></a>Tutorial: Integração do Azure Active Directory com vxMaintain

Neste tutorial, saiba como integrar vxMaintain com o Azure Active Directory (Azure AD).

Esta integração proporciona várias vantagens importantes. Pode:

- Controlar no Azure AD que tenha acesso ao vxMaintain.
- Permita que os utilizadores iniciem sessão automaticamente nas vxMaintain com o início de sessão único (SSO) através das respetivas contas do Azure AD.
- Gira as suas contas num local central: portal do Azure.

Para saber mais sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com vxMaintain, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um vxMaintain subscrição SSO ativado

> [!NOTE]
> Quando testa os passos neste tutorial, é recomendável que não use um ambiente de produção.

Para testar os passos neste tutorial, siga as seguintes recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. 

O cenário que descreve este tutorial consiste em dois blocos de construção principais:

* Adicionando vxMaintain da Galeria
* Configuração e teste do Azure AD início de sessão único

## <a name="add-vxmaintain-from-the-gallery"></a>Adicionar vxMaintain a partir da Galeria
Para configurar a integração do vxMaintain com o Azure AD, terá de adicionar vxMaintain a partir da Galeria à sua lista de aplicações de SaaS geridas.

Para adicionar vxMaintain a partir da galeria, faça o seguinte:

1. Na [portal do Azure](https://portal.azure.com), no painel esquerdo, selecione a **Azure Active Directory** botão. 

    ![O botão do Azure Active Directory][1]

2. Selecione **aplicações empresariais** > **todas as aplicações**.

    ![O painel de "Aplicações empresariais"][2]
    
3. Adicionar uma aplicação, o **todos os aplicativos** caixa de diálogo, selecione **novo aplicativo**.

    ![O "novo aplicativo" botão][3]

4. Na caixa de pesquisa, escreva **vxMaintain**.

    ![A "Único início de sessão em modo de" na lista pendente](./media/vxmaintain-tutorial/tutorial_vxmaintain_search.png)

5. Na lista de resultados, selecione **vxMaintain**e, em seguida, selecione **Add**.

    ![A ligação de vxMaintain](./media/vxmaintain-tutorial/tutorial_vxmaintain_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único
Nesta secção, configurar e testar o SSO do Azure AD utilizando o vxMaintain, com base num utilizador de teste chamado "Eduarda Almeida."

Para SSO para funcionar, o Azure AD precisa saber a contrapartida vxMaintain ao utilizador do Azure AD. Ou seja, tem de estabelecer uma relação de ligação entre o utilizador do Azure AD e o utilizador vxMaintain correspondente.

Para estabelecer a relação de ligação, atribua o vxMaintain **nome de utilizador** valor como o Azure AD **Username** valor.

Para configurar e testar o SSO do Azure AD utilizando vxMaintain, conclua os seguintes blocos de construção.

### <a name="configure-azure-ad-sso"></a>Configurar o SSO do Azure AD

Nesta secção, pode tanto ativar o SSO do Azure AD no portal do Azure e configurar o SSO em seu aplicativo vxMaintain efetuando o seguinte procedimento:

1. No portal do Azure, sobre o **vxMaintain** página de integração de aplicações, selecione **início de sessão único**.

    ![O comando "Início de sessão único"][4]

2. Para ativar o SSO, na **modo de início de sessão único** na lista pendente, selecione **baseado em SAML logon**.
 
    ![O comando "baseado em SAML Sign-on"](./media/vxmaintain-tutorial/tutorial_vxmaintain_samlbase.png)

3. Sob **vxMaintain domínio e URLs**, efetue o seguinte procedimento:

    ![A secção de domínio e URLs de vxMaintain](./media/vxmaintain-tutorial/tutorial_vxmaintain_url.png)

    a. Na **identificador** , escreva um URL com a seguinte sintaxe: `https://<company name>.verisae.com`

    b. Na **URL de resposta** , escreva um URL com a seguinte sintaxe: `https://<company name>.verisae.com/DataNett/action/ssoConsume/mobile?_log=true`

    > [!NOTE] 
    > Os valores anteriores não são reais. Atualizá-las com o identificador real e URL de resposta. Para obter os valores, entre em contato com o [equipa de suporte de vxMaintain](https://www.hubspot.com/company/contact).
 
4. Sob **certificado de assinatura SAML**, selecione **XML de metadados**e, em seguida, guarde o ficheiro de metadados para o seu computador.

    ![A secção "Certificado de assinatura de SAML"](./media/vxmaintain-tutorial/tutorial_vxmaintain_certificate.png) 

5. Selecione **Guardar**.

    ![O botão salvar](./media/vxmaintain-tutorial/tutorial_general_400.png)

6. Para configurar **vxMaintain** SSO, enviar o transferido **XML de metadados** ficheiro para o [equipa de suporte de vxMaintain](https://www.hubspot.com/company/contact).

> [!TIP]
> Como configurar a aplicação, pode ler uma versão concisa das instruções anteriores no [portal do Azure](https://portal.azure.com). Depois de adicionar a aplicação a partir da **do Active Directory** > **aplicações empresariais** secção, selecione o **Single Sign-On** separador e, em seguida, acessar o embedded documentação a partir da **configuração** secção. 
>
>Para saber mais sobre a funcionalidade de documentação do embedded, veja [gerir o início de sessão único para aplicações empresariais de](https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
Nesta secção, vai criar o utilizador de teste Eduarda Almeida no portal do Azure, fazendo o seguinte:

![O utilizador de teste do Azure AD][100]

1. Na **portal do Azure**, no painel esquerdo, selecione a **Azure Active Directory** botão.

    ![O botão "Azure Active Directory"](./media/vxmaintain-tutorial/create_aaduser_01.png) 

2. Para apresentar uma lista de utilizadores, aceda a **utilizadores e grupos** > **todos os utilizadores**.
    
    ![A ligação de "Todos os utilizadores"](./media/vxmaintain-tutorial/create_aaduser_02.png)  
    O **todos os utilizadores** é aberta a caixa de diálogo. 

3. Para abrir o **usuário** caixa de diálogo, selecione **Add**.
 
    ![Botão Adicionar](./media/vxmaintain-tutorial/create_aaduser_03.png) 

4. Na **utilizador** diálogo caixa, faça o seguinte:
 
    ![A caixa de diálogo de utilizador](./media/vxmaintain-tutorial/create_aaduser_04.png) 

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador de teste Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que foi gerado no **palavra-passe** caixa.

    d. Selecione **Criar**.
 
### <a name="create-a-vxmaintain-test-user"></a>Criar um utilizador de teste vxMaintain

Nesta secção, vai criar o utilizador de teste Eduarda Almeida na vxMaintain. Para adicionar os utilizadores na plataforma vxMaintain, trabalhar com o [equipa de suporte de vxMaintain](https://www.hubspot.com/company/contact). Antes de utilizar SSO, criar e ativar os utilizadores.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar o utilizador de teste Eduarda Almeida para utilizar o SSO do Azure ao conceder acesso a vxMaintain. Para tal, faça o seguinte:

![Utilizador de teste na lista nome a apresentar][200] 

1. No portal do Azure **aplicativos** vista, aceda à **diretório** vista > **aplicações empresariais** > **todas as aplicações**.

    ![A ligação de "Todas as aplicações"][201] 

2. Na **aplicativos** lista, selecione **vxMaintain**.

    ![A ligação de vxMaintain](./media/vxmaintain-tutorial/tutorial_vxmaintain_app.png) 

3. No painel esquerdo, selecione **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202] 

4. Selecione **Add** e, em seguida, no **adicionar atribuição** painel, selecione **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][203]

5. Na **utilizadores e grupos** caixa de diálogo a **utilizadores** lista, selecione **Eduarda Almeida**e, em seguida, selecione o **selecione** botão.

7. Na **adicionar atribuição** caixa de diálogo, selecione **atribuir**.
    
### <a name="test-your-azure-ad-single-sign-on"></a>Testar sua do Azure AD início de sessão único

Nesta secção, vai testar a configuração de SSO do Azure AD utilizando o painel de acesso.

Selecionar o **vxMaintain** mosaico no painel de acesso deve iniciar a sua sessão no seu aplicativo vxMaintain automaticamente.

Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="next-steps"></a>Passos Seguintes

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/vxmaintain-tutorial/tutorial_general_01.png
[2]: ./media/vxmaintain-tutorial/tutorial_general_02.png
[3]: ./media/vxmaintain-tutorial/tutorial_general_03.png
[4]: ./media/vxmaintain-tutorial/tutorial_general_04.png

[100]: ./media/vxmaintain-tutorial/tutorial_general_100.png

[200]: ./media/vxmaintain-tutorial/tutorial_general_200.png
[201]: ./media/vxmaintain-tutorial/tutorial_general_201.png
[202]: ./media/vxmaintain-tutorial/tutorial_general_202.png
[203]: ./media/vxmaintain-tutorial/tutorial_general_203.png

