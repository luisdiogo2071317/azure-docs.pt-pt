---
title: 'Tutorial: Integrar do Azure Active Directory com redes da Palo Alto - Admin IU | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e redes da Palo Alto - UI de Admin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: a826eaec-15af-4c85-8855-8a3374d1efb9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: jeedes
ms.openlocfilehash: aa3366810a40b004fe510cb2909f8da0f3513ddb
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
# <a name="integrate-azure-active-directory-with-palo-alto-networks---admin-ui"></a>Integrar do Azure Active Directory com redes da Palo Alto - UI de Admin

Neste tutorial, saiba como integrar o Azure Active Directory (Azure AD) com redes da Palo Alto - UI de Admin.

Ao integrar o Azure AD com redes da Palo Alto - Admin IU, obtenha as seguintes vantagens:

- Pode controlar no Azure AD quem tem acesso às redes da Palo Alto - UI de Admin.
- Pode permitir aos utilizadores obter com sessão iniciada automaticamente a redes da Palo Alto - Admin IU (o início de sessão único, ou SSO) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central, o portal do Azure.

Para saber mais sobre a integração de aplicações SaaS com o Azure AD, consulte [que é o acesso a aplicações e início de sessão no Azure Active Directory?](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com redes da Palo Alto - Admin IU, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Uma Firewall de próxima geração de redes da Palo Alto ou Panorama (gestão centralizada de sistema para as firewalls)

> [!NOTE]
> Quando o testar os passos neste tutorial, recomendamos que efetue *não* utilizar um ambiente de produção.

Para testar os passos neste tutorial, siga estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. 

O cenário é descrito neste tutorial consiste em dois blocos modulares principais:

* A adição da Palo Alto redes - Admin IU da galeria do
* Configurar e testar o Azure AD de sessão único-

## <a name="add-palo-alto-networks---admin-ui-from-the-gallery"></a>Adicionar redes da Palo Alto - Admin IU da galeria do
Para configurar a integração do Azure AD com redes da Palo Alto - Admin IU, adicione redes da Palo Alto - Admin IU da Galeria à sua lista de aplicações SaaS geridas efetuando o seguinte procedimento:

1. No [portal do Azure](https://portal.azure.com), no painel esquerdo, selecione **do Azure Active Directory**. 

    ![O botão do Azure Active Directory][1]

2. Selecione **aplicações empresariais** > **todas as aplicações**.

    ![A janela de "Aplicações da empresa"][2]
    
3. Para adicionar uma nova aplicação, selecione o **nova aplicação** botão na parte superior da janela.

    ![A aplicação"novo" botão][3]

4. Na caixa de pesquisa, escreva **redes da Palo Alto - Admin IU**, selecione **redes da Palo Alto - Admin IU** na lista de resultados e, em seguida, selecione **adicionar**.

    ![Redes da Palo Alto - UI de administrador na lista de resultados](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_step4-add-from-the-gallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com redes da Palo Alto - Admin IU, com base num utilizador de teste chamado "Britta Simon."

Para início de sessão trabalhar, do Azure AD tem de identificar redes da Palo Alto - utilizador de Admin IU e o respetivo homólogo no Azure AD. Por outras palavras, tem de ser estabelecida uma relação de ligação entre um utilizador do Azure AD e o mesmo utilizador nas redes da Palo Alto - UI de Admin.

Para estabelecer a relação de ligação, atribua as redes da Palo Alto - Admin IU *Username* o valor da *nome de utilizador* no Azure AD.

Para configurar e testar o Azure AD-início de sessão único com redes da Palo Alto - Admin IU, conclua os blocos modulares nas secções junto cinco.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Ativar o Azure AD início de sessão no portal do Azure e configurar o início de sessão único nas suas redes da Palo Alto - a aplicação de IU de administração da seguinte forma:

1. No portal do Azure, no **redes da Palo Alto - Admin IU** página de integração de aplicações, selecione **de sessão único-**.

    ![A hiperligação "De sessão único"][4]

2. No **de sessão único-** janela, no **modo de início de sessão único** caixa, selecione **baseados em SAML início de sessão**.
 
    ![A janela "De sessão único"](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_samlbase.png)

3. Em **da Palo Alto redes - administrador de domínio de IU e URLs**, efetue o seguinte procedimento:

    !["Redes da Palo Alto - administrador de domínio de IU e URLs" único informações de início de sessão](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_show_advanced_url.png)
    
    a. No **URL de início de sessão** caixa, escreva um URL no seguinte formato: *https://\<cliente Firewall FQDN > /php/login.php*.

    b. No **identificador** caixa, escreva um URL no seguinte formato: *https://\<cliente Firewall FQDN >: 443/SAML20/SP*.
    
    c. No **URL de resposta** caixa, escreva o URL do serviço de consumidor da asserção (ACS) no seguinte formato: *https://\<cliente Firewall FQDN >: 443/SAML20/SP/ACS*.
    
    > [!NOTE] 
    > Os valores anteriores não são reais. Atualize-as com o URL de início de sessão real e o identificador. Para obter os valores, contacte [redes da Palo Alto - equipa de suporte de cliente de IU de Admin](https://support.paloaltonetworks.com/support). 
 
4. Porque as redes da Palo Alto - a aplicação de IU de Admin espera as asserções de SAML num formato específico, configure as afirmações conforme mostrado na imagem seguinte. Gerir os valores de atributo no **atributos de utilizador** secção o **integração de aplicações** página efetuando o seguinte procedimento:
    
    ![A lista de atributos de Token SAML](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_attribute.png)
    
   > [!NOTE]
   > Porque os valores de atributo são apenas exemplos, mapear os valores adequados para *username* e *adminrole*. Não há outro atributo opcional, *accessdomain*, que é utilizada para restringir o acesso de administrador aos sistemas virtuais específicos na firewall.
   >
        
    | Nome do atributo | Valor do atributo |
    | --- | --- |    
    | o nome de utilizador | user.userprincipalname |
    | adminrole | customadmin |

    a. Selecione **adicionar atributo**.  
    
    ![O botão "Adicionar atributo"](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_attribute_04.png)

    O **adicionar atributo** é aberta a janela.

    ![A janela de "Adicionar atributo"](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_attribute_05.png)
    
    b. No **nome** caixa, escreva o nome do atributo que é apresentado para essa linha.
    
    c. No **valor** caixa, digite o valor do atributo que é apresentado para essa linha.
    
    d. Selecione **OK**.

    > [!NOTE]
    > Para obter mais informações sobre os atributos, consulte os artigos seguintes:
    > * [Perfil de função administrativa para a IU de Admin (adminrole)](https://www.paloaltonetworks.com/documentation/80/pan-os/pan-os/firewall-administration/manage-firewall-administrators/configure-an-admin-role-profile)
    > * [Domínio de acesso do dispositivo para a IU de Admin (accessdomain)](https://www.paloaltonetworks.com/documentation/80/pan-os/web-interface-help/device/device-access-domain)
    >

5. Em **certificado de assinatura de SAML**, selecione **XML de metadados**e, em seguida, selecione **guardar**.

    ![O XML de metadados transferir ligação](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_certificate.png) 

    ![O botão Guardar](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_400.png)

6. Abra a IU da Firewall da Palo Alto redes Admin como um administrador numa nova janela.

7. Selecione o **dispositivo** separador.

    ![O separador de dispositivo](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_admin1.png)

8. No painel esquerdo, selecione **fornecedor de identidade**e, em seguida, selecione **importar** para importar o ficheiro de metadados.

    ![O botão de ficheiro de metadados de importação](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_admin2.png)

9. No **SAML identificar fornecedor de perfil de importação do servidor** janela, efetue o seguinte procedimento:

    ![A janela "SAML identificar fornecedor perfil importação do servidor"](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_idp.png)

    a. No **nome do perfil** caixa, forneça um nome (por exemplo, **AzureAD Admin IU**).
    
    b. Em **metadados do fornecedor de identidade**, selecione **procurar**e selecione o ficheiro metadata.xml que transferiu anteriormente a partir do portal do Azure.
    
    c. Limpar o **Validar certificado do fornecedor de identidade** caixa de verificação.
    
    d. Selecione **OK**.
    
    e. Para confirmar as configurações na firewall, selecione **consolidação**.

10. No painel esquerdo, selecione **fornecedor de identidade**e, em seguida, selecione o perfil de fornecedor de identidade de SAML (por exemplo, **AzureAD Admin IU**) que criou no passo anterior. 

    ![O perfil de fornecedor de identidade SAML](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_idp_select.png)

11. No **perfil de servidor de fornecedor de identidade de SAML** janela, efetue o seguinte procedimento:

    ![A janela "Perfil do servidor de fornecedor de identidade SAML"](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_slo.png)
  
    a. No **URL de SLO de fornecedor de identidade** caixa, substitua o URL de SLO importado anteriormente com o seguinte URL: **https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0**.
  
    b. Selecione **OK**.

12. Na IU de administração do redes Firewall da Palo Alto, selecione **dispositivo**e, em seguida, selecione **funções de administrador**.

13. Selecione o **adicionar** botão. 

14. No **perfil de função de administrador** janela, no **nome** caixa, forneça um nome para a função de administrador (por exemplo, **fwadmin**).  
    O nome da função de administrador deve corresponder ao nome de atributo de função de administrador de SAML que lhe foi enviado pelo fornecedor de identidade. O nome de função de administrador e o valor foram criados no passo 4.

    ![Configurar a função de administrador de redes da Palo Alto](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_adminrole.png)
  
15. Na IU de administração da Firewall, selecione **dispositivo**e, em seguida, selecione **perfil autenticação**.

16. Selecione o **adicionar** botão. 

17. No **perfil autenticação** janela, efetue o seguinte procedimento: 

    ![A janela de "Autenticação perfil"](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_authentication_profile.png)

    a. No **nome** caixa, forneça um nome (por exemplo, **AzureSAML_Admin_AuthProfile**).
    
    b. No **tipo** na lista pendente, selecione **SAML**. 
   
    c. No **IdP servidor perfil** pendente lista, selecione o perfil de servidor do fornecedor de identidade de SAML adequado (por exemplo, **AzureAD Admin IU**).
   
    c. Selecione o **ativar a fim de sessão único** caixa de verificação.
    
    d. No **atributos da função de administrador** box, introduza o nome de atributo (por exemplo, **adminrole**). 
    
    e. Selecione o **avançadas** separador e, em **permitir lista**, selecione **adicionar**. 
    
    ![Botão Adicionar no separador Avançadas](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_allowlist.png)
    
    f. Selecione o **todos os** caixa de verificação, ou selecione os utilizadores e grupos que podem autenticar com este perfil.  
    Quando um utilizador efetua a autenticação, a firewall corresponde o nome de utilizador associado ou grupo relativamente as entradas nesta lista. Se não adicionar entradas, nenhum utilizador pode autenticar.

    g. Selecione **OK**.

18. Para permitir que os administradores utilizem SAML SSO utilizando o Azure, selecione **dispositivo** > **configuração**. No **configuração** painel, selecione o **gestão** separador e, em **definições de autenticação**, selecione o **definições** botão ("gear") . 

 ![O botão de definições](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_authsetup.png)

19. Selecione o perfil de autenticação SAML que criou no passo 17 (por exemplo, **AzureSAML_Admin_AuthProfile**).

 ![O campo de perfil de autenticação](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_authsettings.png)

20. Selecione **OK**.

21. Para confirmar a configuração, selecione **consolidação**.


> [!TIP]
> Como está a configurar a aplicação, pode ler uma versão das instruções anteriores no concisa o [portal do Azure](https://portal.azure.com). Depois de adicionar a aplicação no **do Active Directory** > **aplicações empresariais** secção, selecione o **Single Sign-On** separador e, em seguida, aceder a incorporado documentação o **configuração** secção na parte inferior. Para obter mais informações sobre a funcionalidade de documentação embedded, consulte [do Azure AD incorporado documentação]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

Nesta secção, criar o utilizador de teste Britta Simon no portal do Azure da seguinte forma:

![Criar um utilizador de teste do Azure AD][100]

1. No portal do Azure, no painel esquerdo, selecione **do Azure Active Directory**.

    ![A ligação do Azure Active Directory](./media/active-directory-saas-paloaltoadmin-tutorial/create_aaduser_01.png)

2. Para apresentar uma lista de utilizadores atuais, selecione **utilizadores e grupos** > **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/active-directory-saas-paloaltoadmin-tutorial/create_aaduser_02.png)

3. Na parte superior do **todos os utilizadores** janela, selecione **adicionar**.

    ![O botão de adição](./media/active-directory-saas-paloaltoadmin-tutorial/create_aaduser_03.png)
    
    O **utilizador** é aberta a janela.

4. No **utilizador** janela, efetue o seguinte procedimento:

    ![A janela de utilizador](./media/active-directory-saas-paloaltoadmin-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Selecione **Criar**.
 
### <a name="create-a-palo-alto-networks---admin-ui-test-user"></a>Criar um redes da Palo Alto - utilizador de teste de IU de Admin

Redes da Palo Alto - Admin IU suportam o aprovisionamento de utilizador de just-in-time. Se um utilizador já não existir, será criado automaticamente no sistema após uma autenticação com êxito. Não tem de fazer nada, para criar o utilizador.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar utilizador Britta Simon para utilizar o Azure-início de sessão único, concedendo acesso às redes da Palo Alto - Admin IU. Para tal, faça o seguinte:

![Atribuir a função de utilizador][200] 

1. No portal do Azure, abra o **aplicações** vista, aceda ao **diretório** visualizar e, em seguida, selecione **aplicações empresariais** > **todos os aplicações**.

    ![O "As aplicações empresariais" e ligações de "Todas as aplicações"][201] 

2. No **aplicações** lista, selecione **redes da Palo Alto - Admin IU**.

    ![As redes da Palo Alto - ligação de IU de Admin](./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_paloaltoadmin_app.png)  

3. No painel esquerdo, selecione **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Selecione **adicionar** e, em seguida, no **adicionar atribuição** painel, selecione **utilizadores e grupos**.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** janela, no **utilizadores** lista, selecione **Britta Simon**.

6. Selecione o **selecione** botão.

7. No **adicionar atribuição** janela, selecione **atribuir**.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, teste configuração do Azure AD único início de sessão utilizando o painel de acesso.

Quando seleciona as redes da Palo Alto - mosaico de IU de administrador no painel de acesso, que deve ser iniciou sessão automaticamente para as redes da Palo Alto - a aplicação de IU de Admin.

Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre a integração de aplicações SaaS no Azure Active Directory](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-paloaltoadmin-tutorial/tutorial_general_203.png

