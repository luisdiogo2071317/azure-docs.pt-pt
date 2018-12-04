---
title: 'Tutorial: Integração do Azure Active Directory com o serviço de segurança de Web do Symantec (WSS) | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o serviço de segurança Web (WSS) da Symantec.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: d6e4d893-1f14-4522-ac20-0c73b18c72a5
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: b933bc5f5ecb39c3462e4e9bd300f1e07fd718c0
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52838780"
---
# <a name="tutorial-azure-active-directory-integration-with-symantec-web-security-service-wss"></a>Tutorial: Integração do Azure Active Directory com o serviço de segurança de Web do Symantec (WSS)

Neste tutorial, ficará a saber como integrar a sua conta de serviço de segurança Web (WSS) da Symantec com a sua conta do Azure Active Directory (Azure AD) para que o WSS pode autenticar um utilizador final aprovisionado no Azure AD através da autenticação SAML e impor o utilizador ou regras de política de nível de grupo.

Integrar o serviço de segurança Web (WSS) da Symantec com o Azure AD fornece as seguintes vantagens:

- Gerir todos os utilizadores finais e grupos utilizados pela sua conta WSS a partir do portal do Azure AD. 

- Permitir que os utilizadores finais para se autenticar no WSS através das respetivas credenciais do Azure AD.

- Ativar a imposição de utilizador e grupo de regras de política de nível definidas na sua conta do WSS.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o serviço de segurança de Web do Symantec (WSS), terá dos seguintes itens:

- Uma subscrição do Azure
- Uma conta de serviço de segurança Web (WSS) da Symantec

> [!NOTE]
> Para testar os passos neste tutorial, não é recomendável utilizar uma conta do WSS que está atualmente a ser utilizada para fins de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize a sua conta do WSS que está atualmente a ser utilizada para fins de produção para este teste, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, irá configurar o Azure AD para ativar o início de sessão único para o WSS com as credenciais de utilizador final definidas na sua conta do Azure AD.
O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar a aplicação de serviço de segurança Web (WSS) da Symantec a partir da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-symantec-web-security-service-wss-from-the-gallery"></a>Adicionando o serviço de segurança Web (WSS) da Symantec da Galeria
Para configurar a integração do Symantec Web Security Service (WSS) com o Azure AD, terá de Adicionar serviço de segurança de Web do Symantec (WSS) a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar serviço de segurança de Web do Symantec (WSS) a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **serviço de segurança Web (WSS) da Symantec**, selecione **serviço de segurança Web (WSS) da Symantec** no painel de resultados, em seguida, clique em **adicionar** botão para adicionar o aplicação.

    ![Symantec Web Security Service (WSS) na lista de resultados](./media/symantec-tutorial/tutorial_symantecwebsecurityservicewss_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Symantec Web Security Service (WSS) com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no serviço de segurança de Web do Symantec (WSS) para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no serviço de segurança de Web do Symantec (WSS) deve ser estabelecido.

Na Symantec Web Security Service (WSS), atribuir o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o serviço de segurança de Web do Symantec (WSS), tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste do serviço de segurança Web (WSS) da Symantec](#create-a-symantec-web-security-service-wss-test-user)**  - para ter um equivalente da Eduarda Almeida na segurança de Web da Symantec Service (WSS) que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação de serviço de segurança Web (WSS) da Symantec.

**Para configurar o Azure AD início de sessão único com o serviço de segurança de Web do Symantec (WSS), execute os seguintes passos:**

1. No portal do Azure, no **serviço de segurança Web (WSS) da Symantec** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/symantec-tutorial/tutorial_symantecwebsecurityservicewss_samlbase.png)

1. Sobre o **URLs de domínio de serviço (WSS) de segurança de Web da Symantec e** secção, execute os seguintes passos:

    ![URLs de domínio do serviço de segurança Web (WSS) da Symantec e únicas início de sessão em informações](./media/symantec-tutorial/tutorial_symantecwebsecurityservicewss_url.png)

    a. Na **identificador** caixa de texto, escreva o URL: `https://saml.threatpulse.net:8443/saml/saml_realm`

    b. Na **URL de resposta** caixa de texto, escreva o URL: `https://saml.threatpulse.net:8443/saml/saml_realm/bcsamlpost`

    > [!NOTE]
    > Entre em contato com o [equipa de suporte de cliente do serviço de segurança Web (WSS) da Symantec](https://www.symantec.com/contact-us) se os valores para o **identificador** e **URL de resposta** não estão a funcionar por algum motivo.

1. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/symantec-tutorial/tutorial_symantecwebsecurityservicewss_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/symantec-tutorial/tutorial_general_400.png)
    
1. Para configurar o início de sessão único no lado do serviço de segurança Web (WSS) da Symantec, consulte a documentação online do WSS. O transferido **metadados XML** ficheiro tem de ser importadas para o portal do WSS. Contacte os [equipa de suporte do serviço de segurança Web (WSS) da Symantec](https://www.symantec.com/contact-us) se precisar de assistência com a configuração no portal do WSS.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/symantec-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/symantec-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/symantec-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/symantec-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-symantec-web-security-service-wss-test-user"></a>Criar um utilizador de teste do serviço de segurança Web (WSS) da Symantec

Nesta secção, vai criar um usuário chamado Eduarda Almeida no serviço de segurança de Web do Symantec (WSS). O nome de utilizador final correspondente pode ser criado manualmente no portal do WSS ou pode aguardar que os utilizadores/grupos aprovisionados no Azure AD para ser sincronizadas com o portal WSS após alguns minutos (~ 15 minutos). Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único. O endereço IP público da máquina do usuário final, que será utilizada para procurar os Web sites também tem de ser aprovisionado no portal do serviço de segurança Web (WSS) da Symantec.

> [!NOTE]
> Tente [clique aqui](https://www.bing.com/search?q=my+ip+address&qs=AS&pq=my+ip+a&sc=8-7&cvid=29A720C95C78488CA3F9A6BA0B3F98C5&FORM=QBLH&sp=1) para obter a sua máquina do IPaddress pública.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso ao serviço de segurança de Web da Symantec (WSS).

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida Symantec Web Security Service (WSS), execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Symantec Web Security Service (WSS)**.

    ![A ligação de serviço de segurança Web (WSS) da Symantec na lista de aplicações](./media/symantec-tutorial/tutorial_symantecwebsecurityservicewss_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, irá testar a funcionalidade de início de sessão única, agora que configurou a sua conta do WSS para utilizar o Azure AD para autenticação de SAML.

Depois de ter configurado seu navegador da web para o tráfego de proxy para o WSS, quando abra o browser e tente navegar para um site, em seguida, será redirecionado para a página de início de sessão do Azure. Introduza as credenciais do utilizador final de teste que foi provisionado no Azure AD (ou seja, BrittaSimon) e associadas a palavra-passe. Uma vez autenticado, poderá visitar o Web site que escolheu. Deve criar uma regra de política no lado do WSS para bloquear BrittaSimon de navegar para um site específico, em seguida, verá a página de bloco do WSS, quando está tentando navegar para esse site como utilizador BrittaSimon.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/symantec-tutorial/tutorial_general_01.png
[2]: ./media/symantec-tutorial/tutorial_general_02.png
[3]: ./media/symantec-tutorial/tutorial_general_03.png
[4]: ./media/symantec-tutorial/tutorial_general_04.png

[100]: ./media/symantec-tutorial/tutorial_general_100.png

[200]: ./media/symantec-tutorial/tutorial_general_200.png
[201]: ./media/symantec-tutorial/tutorial_general_201.png
[202]: ./media/symantec-tutorial/tutorial_general_202.png
[203]: ./media/symantec-tutorial/tutorial_general_203.png

