---
title: 'Tutorial: Integração do Azure Active Directory com a área de trabalho pelo Facebook | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e à área de trabalho pelo Facebook.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 30f2ee64-95d3-44ef-b832-8a0a27e2967c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/30/2018
ms.author: jeedes
ms.openlocfilehash: 1f83dd64c7f6773ddb8956e6ebbc37b8c55aacec
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39423876"
---
# <a name="tutorial-azure-active-directory-integration-with-workplace-by-facebook"></a>Tutorial: Integração do Azure Active Directory com a área de trabalho pelo Facebook

Neste tutorial, saiba como integrar à área de trabalho pelo Facebook com o Azure Active Directory (Azure AD).

Integrar à área de trabalho pelo Facebook com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso à área de trabalho pelo Facebook
- Pode permitir que os utilizadores automaticamente obter sessão iniciada em à área de trabalho pelo Facebook (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com à área de trabalho pelo Facebook, precisa do seguinte:

- Uma subscrição do Azure AD
- Uma área de trabalho pelo Facebook logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

> [!NOTE]
> Facebook tem dois produtos, à área de trabalho padrão (gratuito) e Premium à área de trabalho (paga). Nenhum inquilino à área de trabalho Premium pode configurar a integração de SCIM e SSO com outras implicações de custo ou licenças necessárias. SSO e SCIM não estão disponíveis em instâncias à área de trabalho padrão.

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar à área de trabalho pelo Facebook a partir da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-workplace-by-facebook-from-the-gallery"></a>Adicionar à área de trabalho pelo Facebook a partir da Galeria
Para configurar a integração de área de trabalho pelo Facebook com o Azure AD, terá de adicionar à área de trabalho pelo Facebook a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar à área de trabalho pelo Facebook a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **à área de trabalho pelo Facebook**.

    ![Criar um utilizador de teste do Azure AD](./media/workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_search.png)

1. No painel de resultados, selecione **à área de trabalho pelo Facebook**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configurar e testar o Azure AD início de sessão único à área de trabalho pelo Facebook com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador equivalente na área de trabalho pelo Facebook para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado na área de trabalho pelo Facebook deve ser estabelecido.

Esta relação de ligação é estabelecida atribuindo o valor do **nome de utilizador** no Azure AD como o valor da **Username** na área de trabalho pelo Facebook.

Para configurar e testar o Azure AD início de sessão único à área de trabalho pelo Facebook, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Configurar a frequência de reautenticação](#configuring-reauthentication-frequency)**  - para configurar a área de trabalho para solicitar uma verificação SAML.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar uma área de trabalho por utilizador de teste do Facebook](#creating-a-workplace-by-facebook-test-user)**  - para ter um equivalente da Eduarda Almeida na área de trabalho pelo Facebook, que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua área de trabalho por aplicativo para Facebook.

**Para configurar o Azure AD início de sessão único à área de trabalho pelo Facebook, execute os seguintes passos:**

1. No portal do Azure, sobre o **à área de trabalho pelo Facebook** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_samlbase.png)

1. Sobre o **à área de trabalho ao domínio do Facebook e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<instancename>.facebook.com`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://www.facebook.com/company/<instanceID>`

    > [!NOTE] 
    > Estes valores não são o real. Atualize estes valores com o URL de início de sessão e o identificador real. Consulte a página de autenticação do Dashboard à área de trabalho da empresa para os valores corretos para sua Comunidade local de trabalho. 

1. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/workplacebyfacebook-tutorial/tutorial_general_400.png)

1. Sobre o **à área de trabalho pela configuração do Facebook** secção, clique em **configurar o local de trabalho pelo Facebook** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/workplacebyfacebook-tutorial/config.png) 

1. Numa janela do browser web diferente, início de sessão à sua área de trabalho por site de empresa do Facebook como administrador.
  
   > [!NOTE] 
   > Como parte do processo de autenticação SAML, à área de trabalho pode utilizar cadeias de caracteres de consulta de até 2,5 quilobytes de tamanho para passar parâmetros para o Azure AD.

1. Na **Dashboard da empresa**, aceda ao **autenticação** separador.

1. Sob **autenticação SAML**, selecione **SSO apenas** na lista pendente.

1. Os valores copiados a partir de entrada **à área de trabalho pela configuração do Facebook** seção do portal do Azure nos campos correspondentes:

    *   Na **URL de SAML** caixa de texto, cole o valor de **único URL de início de sessão no serviço**, que copiou do portal do Azure.
    *   Na **caixa de texto do URL de emissor de SAML**, cole o valor de **ID de entidade de SAML**, que copiou do portal do Azure.
    *   Na **redirecionamento de fim de sessão de SAML** (opcional), cole o valor de **URL de fim de sessão**, que copiou do portal do Azure.
    *   Abra sua **certificado com codificação base 64** no bloco de notas transferido a partir do portal do Azure, copie o conteúdo do mesmo para a área de transferência e, em seguida, cole-os para o **certificado SAML** caixa de texto.

1. Poderá ter de introduzir o URL de público-alvo, o URL de destinatário, e o ACS (serviço de consumidor de asserção) URL listado sob os **configuração SAML do** secção.

1. Desloque para baixo da seção e clique nas **SSO de teste** botão. O resultado é uma janela de pop-up que aparece com página de início de sessão do Azure AD apresentados. Introduza as credenciais em como normal para autenticar. 

    **Resolução de problemas:** Certifique-se de que está a ser devolvido back do Azure AD é o mesmo que a conta da área de trabalho tiver iniciado sessão com o endereço de e-mail.

1. Quando o teste estiver concluído com êxito, desloque-se para a parte inferior da página e clique nas **guardar** botão.

1. Todos os utilizadores usem à área de trabalho serão agora apresentados com página de início de sessão do Azure AD para autenticação.

1. **Redirecionamento de fim de sessão de SAML (opcional)** - 

    Pode optar por configurar, opcionalmente, um Url de fim de sessão SAML, que podem ser usados para apontar para a página de fim de sessão do Azure AD. Quando esta definição está ativada e configurada, o utilizador já não será direcionado para a página de fim de sessão de área de trabalho. Em vez disso, o utilizador será redirecionado para o url que foi adicionado na definição de redirecionamento de fim de sessão de SAML.

### <a name="configuring-reauthentication-frequency"></a>Configurar a frequência de reautenticação

Pode configurar à área de trabalho para solicitar uma verificação SAML cada dia, três dias, semana, duas semanas, meses ou nunca.

> [!NOTE] 
>O valor mínimo para a verificação SAML em aplicativos móveis está definido para uma semana.

Pode também forçar um SAML repor para todos os utilizadores com o botão: exigir autenticação SAML para todos os utilizadores agora.


### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/workplacebyfacebook-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/workplacebyfacebook-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/workplacebyfacebook-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/workplacebyfacebook-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-workplace-by-facebook-test-user"></a>Criar uma área de trabalho por utilizador de teste do Facebook

Nesta secção, um usuário chamado Eduarda Almeida é criado na área de trabalho pelo Facebook. Área de trabalho pelo Facebook suporta o aprovisionamento de just-in-time, que está ativado por predefinição.

Não existe nenhuma ação para nesta secção. Se um utilizador não existir na área de trabalho pelo Facebook, uma nova é criada quando tentar aceder à área de trabalho pelo Facebook.

>[!Note]
>Se precisar de criar manualmente um utilizador, contacte o [à área de trabalho pela equipa de suporte de cliente do Facebook](https://workplace.fb.com/faq/)

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso à área de trabalho pelo Facebook.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida à área de trabalho pelo Facebook, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **à área de trabalho pelo Facebook**.

    ![Configurar o início de sessão único](./media/workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Se pretender testar as definições de início de sessão únicas, abra o painel de acesso.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).


## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)
* [Configurar o aprovisionamento do utilizador](workplacebyfacebook-provisioning-tutorial.md)



<!--Image references-->

[1]: ./media/workplacebyfacebook-tutorial/tutorial_general_01.png
[2]: ./media/workplacebyfacebook-tutorial/tutorial_general_02.png
[3]: ./media/workplacebyfacebook-tutorial/tutorial_general_03.png
[4]: ./media/workplacebyfacebook-tutorial/tutorial_general_04.png

[100]: ./media/workplacebyfacebook-tutorial/tutorial_general_100.png

[200]: ./media/workplacebyfacebook-tutorial/tutorial_general_200.png
[201]: ./media/workplacebyfacebook-tutorial/tutorial_general_201.png
[202]: ./media/workplacebyfacebook-tutorial/tutorial_general_202.png
[203]: ./media/workplacebyfacebook-tutorial/tutorial_general_203.png
