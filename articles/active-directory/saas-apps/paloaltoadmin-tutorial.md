---
title: 'Tutorial: Integração do Azure Active Directory da Palo Alto Networks - interface do Usuário de administrador | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e da Palo Alto Networks - interface do Usuário de administrador.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: a826eaec-15af-4c85-8855-8a3374d1efb9
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2018
ms.author: jeedes
ms.openlocfilehash: fed368c0df265495d9fee764f86825957fae8bab
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39447429"
---
# <a name="integrate-azure-active-directory-with-palo-alto-networks---admin-ui"></a>Integrar o Azure Active Directory da Palo Alto Networks - interface do Usuário de administrador

Neste tutorial, saiba como integrar o Azure Active Directory (Azure AD) com da Palo Alto Networks - interface do Usuário de administrador.

Pela integração do Azure AD da Palo Alto Networks - interface do Usuário de administrador, obtenha os seguintes benefícios:

- Pode controlar no Azure AD que tenha acesso da Palo Alto Networks - interface do Usuário de administrador.
- Pode permitir que os utilizadores obter entrar automaticamente para Palo Alto Networks - Admin da interface do Usuário (início de sessão único, ou SSO) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central, o portal do Azure.

Para saber mais sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com da Palo Alto Networks - interface do Usuário de administrador, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Firewall de próxima geração de redes da Palo Alto ou Panorama (sistema de gerenciamento centralizado para as firewalls)

> [!NOTE]
> Quando testa os passos neste tutorial, é recomendável que faça *não* utilizar um ambiente de produção.

Para testar os passos neste tutorial, siga as seguintes recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. 

O cenário que é descrito neste tutorial consiste em dois blocos de construção principais:

* Adição da Palo Alto Networks - interface do Usuário de administração da Galeria
* Configuração e teste do Azure AD início de sessão único

## <a name="add-palo-alto-networks---admin-ui-from-the-gallery"></a>Adicionar da Palo Alto Networks - interface do Usuário de administração da Galeria
Para configurar a integração do Azure AD com da Palo Alto Networks - interface do Usuário de administrador, adicione da Palo Alto Networks - Admin da interface do Usuário da Galeria à sua lista de aplicações geridas do SaaS, fazendo o seguinte:

1. Na [portal do Azure](https://portal.azure.com), no painel esquerdo, selecione **Azure Active Directory**. 

    ![O botão do Azure Active Directory][1]

1. Selecione **aplicações empresariais** > **todas as aplicações**.

    ![A janela de "Aplicações empresariais"][2]
    
1. Para adicionar uma nova aplicação, selecione o **nova aplicação** botão na parte superior da janela.

    ![O "novo aplicativo" botão][3]

1. Na caixa de pesquisa, escreva **Palo Alto Networks - interface do Usuário de administrador**, selecione **Palo Alto Networks - interface do Usuário de administrador** na lista de resultados e, em seguida, selecione **Add**.

    ![Da Palo Alto Networks - interface do Usuário de administrador na lista de resultados](./media/paloaltoadmin-tutorial/tutorial_step4-add-from-the-gallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único da Palo Alto Networks - interface do Usuário de administrador, com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa identificar da Palo Alto Networks - utilizador de interface do Usuário de administrador e sua contraparte no Azure AD. Em outras palavras, é necessário estabelecer uma relação de ligação entre um utilizador do Azure AD e o mesmo utilizador da Palo Alto Networks - interface do Usuário de administrador.

Para estabelecer a relação de ligação, atribuir como da Palo Alto Networks - interface do Usuário de administrador *nome de utilizador* o valor da *nome de utilizador* no Azure AD.

Para configurar e testar o Azure AD início de sessão único da Palo Alto Networks - interface do Usuário de administrador, conclua os blocos de construção nas seções a seguir cinco.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Ativar o Azure AD início de sessão único no portal do Azure e configurar o início de sessão único nas redes da Palo Alto - aplicação de interface do Usuário de administrador, fazendo o seguinte:

1. No portal do Azure, sobre o **Palo Alto Networks - interface do Usuário de administrador** página de integração de aplicações, selecione **início de sessão único**.

    ![A ligação de "Início de sessão único"][4]

1. Na **início de sessão único** janela, na **modo de início de sessão único** caixa, selecione **baseado em SAML logon**.
 
    ![A janela de "Início de sessão único"](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_samlbase.png)

1. Sob **Palo Alto Networks - domínio de interface do Usuário de administrador e URLs**, efetue o seguinte procedimento:

    !["Da Palo Alto Networks - domínio de interface do Usuário de administrador e URLs" única informações de início de sessão](./media/paloaltoadmin-tutorial/tutorial_general_show_advanced_url.png)
    
    a. Na **URL de início de sessão** caixa, escreva um URL no seguinte formato: *https://\<FQDN de Firewall do cliente > /php/login.php*.

    b. Na **identificador** caixa, escreva um URL no seguinte formato: *https://\<FQDN de Firewall do cliente >: 443/SAML20/SP*.
    
    c. Na **URL de resposta** caixa, escreva o URL de serviço de consumidor de asserção (ACS) no seguinte formato: *https://\<FQDN de Firewall do cliente >: 443/SAML20/SP/ACS*.
    
    > [!NOTE] 
    > Os valores anteriores não são reais. Atualize-as com o URL de início de sessão real e o identificador. Para obter os valores, contacte [Palo Alto Networks - equipa de suporte de cliente de interface do Usuário administrativo](https://support.paloaltonetworks.com/support). 
 
1. Uma vez que da Palo Alto Networks - aplicação de interface do Usuário de administrador espera que as asserções SAML num formato específico, configure as afirmações conforme mostrado na imagem seguinte. Gerir os valores de atributo na **atributos de utilizador** secção a **integração de aplicações** página ao fazer o seguinte:
    
    ![A lista de atributos de Token SAML](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_attribute.png)
    
   > [!NOTE]
   > Como os valores de atributo são apenas exemplos, mapear os valores adequados para *nome de utilizador* e *adminrole*. Há outro atributo opcional, *accessdomain*, que é utilizado para restringir o acesso de administrador para sistemas virtuais específicos na firewall.
   >
        
    | Nome de atributo | Valor do atributo |
    | --- | --- |    
    | o nome de utilizador | user.userprincipalname |
    | adminrole | customadmin |

    a. Selecione **adicionar atributo**.  
    
    ![O botão "Adicionar atributo"](./media/paloaltoadmin-tutorial/tutorial_attribute_04.png)

    O **adicionar atributo** é aberta a janela.

    ![A janela de "Adicionar atributo"](./media/paloaltoadmin-tutorial/tutorial_attribute_05.png)
    
    b. Na **nome** , escreva o nome de atributo que é mostrado para essa linha.
    
    c. Na **valor** , escreva o valor do atributo que é mostrado para essa linha.
    
    d. Selecione **OK**.

    > [!NOTE]
    > Para obter mais informações sobre os atributos, consulte os artigos seguintes:
    > * [Perfil de função administrativa para a interface do Usuário de administrador (adminrole)](https://www.paloaltonetworks.com/documentation/80/pan-os/pan-os/firewall-administration/manage-firewall-administrators/configure-an-admin-role-profile)
    > * [Domínio de acesso do dispositivo para a interface do Usuário de administrador (accessdomain)](https://www.paloaltonetworks.com/documentation/80/pan-os/web-interface-help/device/device-access-domain)
    >

1. Sob **certificado de assinatura SAML**, selecione **XML de metadados**e, em seguida, selecione **guardar**.

    ![O XML de metadados de ligação de transferência](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_certificate.png) 

    ![O botão salvar](./media/paloaltoadmin-tutorial/tutorial_general_400.png)

1. Abra a interface de Usuário de administrador de Firewall de redes da Palo Alto como um administrador numa nova janela.

1. Selecione o **dispositivo** separador.

    ![O separador de dispositivo](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_admin1.png)

1. No painel esquerdo, selecione **fornecedor de identidade de SAML**e, em seguida, selecione **importar** para importar o ficheiro de metadados.

    ![O botão Importar do ficheiro de metadados](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_admin2.png)

1. Na **SAML identificar fornecedor de perfil de importação do servidor** janela, efetue o seguinte procedimento:

    ![A janela de "SAML identificar fornecedor perfil importação do servidor"](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_idp.png)

    a. Na **nome do perfil** caixa, indique um nome (por exemplo, **IU de Admin do AzureAD**).
    
    b. Sob **metadados de fornecedor de identidade**, selecione **procurar**e selecione o arquivo METADATA XML que transferiu anteriormente no portal do Azure.
    
    c. Limpar o **Validar certificado do fornecedor de identidade** caixa de verificação.
    
    d. Selecione **OK**.
    
    e. Para consolidar as configurações na firewall, selecione **consolidação**.

1. No painel esquerdo, selecione **fornecedor de identidade de SAML**e, em seguida, selecione o perfil de fornecedor de identidade de SAML (por exemplo, **IU de Admin do AzureAD**) que criou no passo anterior. 

    ![O perfil de fornecedor de identidade SAML](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_idp_select.png)

1. Na **perfil de servidor de fornecedor de identidade de SAML** janela, efetue o seguinte procedimento:

    ![A janela de "Perfil do servidor de fornecedor de identidade SAML"](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_slo.png)
  
    a. Na **URL de SLO do fornecedor de identidade** caixa, substitua o URL de SLO importado anteriormente com o seguinte URL: **https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0**.
  
    b. Selecione **OK**.

1. Na IU do Admin da Palo Alto redes Firewall, selecione **dispositivo**e, em seguida, selecione **funções de administrador**.

1. Selecione o **adicionar** botão. 

1. Na **perfil de função de administrador** janela, na **nome** caixa, indique um nome para a função de administrador (por exemplo, **fwadmin**).  
    O nome da função de administrador deve corresponder o nome de atributo de função de administrador de SAML que lhe foi enviado pelo fornecedor de identidade. O nome da função de administrador e o valor foram criados no passo 4.

    ![Configurar a função de administrador de redes da Palo Alto](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_adminrole.png)
  
1. Na IU de administração do Firewall, selecione **dispositivo**e, em seguida, selecione **perfil de autenticação**.

1. Selecione o **adicionar** botão. 

1. Na **perfil de autenticação** janela, efetue o seguinte procedimento: 

    ![A janela de "Perfil de autenticação"](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_authentication_profile.png)

    a. Na **Name** caixa, indique um nome (por exemplo, **AzureSAML_Admin_AuthProfile**).
    
    b. Na **tipo** na lista pendente, selecione **SAML**. 
   
    c. Na **perfil de servidor do IdP** lista pendente, selecione o perfil adequado do servidor do fornecedor de identidade de SAML (por exemplo, **IU de Admin do AzureAD**).
   
    c. Selecione o **ativar a fim de sessão único** caixa de verificação.
    
    d. Na **atributos de função de administrador** , introduza o nome de atributo (por exemplo, **adminrole**). 
    
    e. Selecione o **avançadas** separador e, em **lista de permissões**, selecione **Add**. 
    
    ![Botão Adicionar na guia avançado](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_allowlist.png)
    
    f. Selecione o **todos os** caixa de verificação ou selecione os utilizadores e grupos que podem autenticar-se com este perfil.  
    Quando um utilizador é autenticado, o firewall corresponde o nome de utilizador associado ou o grupo em relação as entradas desta lista. Se não adicionar entradas, os utilizadores não podem autenticar.

    g. Selecione **OK**.

1. Para permitir que os administradores utilizem SAML SSO ao utilizar o Azure, selecione **dispositivo** > **configuração**. Na **programa de configuração** painel, selecione o **gestão** separador e, em **definições de autenticação**, selecione o **definições** botão ("engrenagem") . 

 ![O botão de definições](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_authsetup.png)

1. Selecione o perfil de autenticação SAML que criou no passo 17 (por exemplo, **AzureSAML_Admin_AuthProfile**).

 ![O campo de perfil de autenticação](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_authsettings.png)

1. Selecione **OK**.

1. Para consolidar a configuração, selecione **consolidação**.


> [!TIP]
> Como estamos configurando o aplicativo, pode ler uma versão concisa das instruções anteriores no [portal do Azure](https://portal.azure.com). Depois de adicionar a aplicação no **do Active Directory** > **aplicações empresariais** secção, selecione o **Single Sign-On** separador e, em seguida, aceder a documentação em Embedded a **configuração** seção na parte inferior. Para obter mais informações sobre a funcionalidade de documentação do embedded, consulte [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

Nesta secção, vai criar o utilizador de teste Eduarda Almeida no portal do Azure, fazendo o seguinte:

![Criar um utilizador de teste do Azure AD][100]

1. No portal do Azure, no painel esquerdo, selecione **do Azure Active Directory**.

    ![A ligação do Azure Active Directory](./media/paloaltoadmin-tutorial/create_aaduser_01.png)

1. Para apresentar uma lista de utilizadores atuais, selecione **utilizadores e grupos** > **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/paloaltoadmin-tutorial/create_aaduser_02.png)

1. Na parte superior a **todos os utilizadores** janela, selecione **Add**.

    ![Botão Adicionar](./media/paloaltoadmin-tutorial/create_aaduser_03.png)
    
    O **utilizador** é aberta a janela.

1. Na **utilizador** janela, efetue o seguinte procedimento:

    ![A janela de utilizador](./media/paloaltoadmin-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Selecione **Criar**.
 
### <a name="create-a-palo-alto-networks---admin-ui-test-user"></a>Criar da Palo Alto Networks - utilizador de teste de interface do Usuário de administrador

Da Palo Alto Networks - interface do Usuário de administrador suporta o aprovisionamento de utilizador de just-in-time. Se um utilizador já não existir, será criado automaticamente no sistema após uma autenticação com êxito. Não é necessária ação do utilizador para criar o utilizador.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar utilizador Eduarda Almeida para utilizar o Azure início de sessão único ao conceder acesso para Palo Alto Networks - interface do Usuário de administrador. Para tal, faça o seguinte:

![Atribuir a função de utilizador][200] 

1. No portal do Azure, abra a **aplicativos** modo de exibição, vá para o **diretório** ver e, em seguida, selecione **aplicações empresariais** > **todos os aplicativos**.

    ![A "Aplicações empresariais" e os links de "Todas as aplicações"][201] 

1. Na **aplicativos** lista, selecione **Palo Alto Networks - interface do Usuário de administrador**.

    ![Da Palo Alto Networks - ligação de interface do Usuário de administrador](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_app.png)  

1. No painel esquerdo, selecione **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Selecione **Add** e, em seguida, no **adicionar atribuição** painel, selecione **utilizadores e grupos**.

    ![O painel Adicionar atribuição][203]

1. Na **utilizadores e grupos** janela, na **utilizadores** lista, selecione **Eduarda Almeida**.

1. Selecione o **selecione** botão.

1. Na **adicionar atribuição** janela, selecione **atribuir**.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar seu única início de sessão em configuração do Azure AD utilizando o painel de acesso.

Quando seleciona o Palo Alto Networks - mosaico de interface do Usuário de administrador no painel de acesso, deve estar conectado automaticamente para as suas redes da Palo Alto - aplicação de interface do Usuário de administrador.

Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/paloaltoadmin-tutorial/tutorial_general_01.png
[2]: ./media/paloaltoadmin-tutorial/tutorial_general_02.png
[3]: ./media/paloaltoadmin-tutorial/tutorial_general_03.png
[4]: ./media/paloaltoadmin-tutorial/tutorial_general_04.png

[100]: ./media/paloaltoadmin-tutorial/tutorial_general_100.png

[200]: ./media/paloaltoadmin-tutorial/tutorial_general_200.png
[201]: ./media/paloaltoadmin-tutorial/tutorial_general_201.png
[202]: ./media/paloaltoadmin-tutorial/tutorial_general_202.png
[203]: ./media/paloaltoadmin-tutorial/tutorial_general_203.png

