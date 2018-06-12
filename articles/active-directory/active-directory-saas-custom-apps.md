---
title: Configurar o SSO do Azure AD para aplicações | Microsoft Docs
description: Saiba como ligar aplicações para o Azure Active Directory utilizando SAML e SSO baseada em palavra-passe para Self-Service
services: active-directory
author: asmalser-msft
documentationcenter: na
manager: mtillman
ms.assetid: 0d42eb0c-6d3f-4557-9030-e88e86709a19
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/20/2018
ms.author: asmalser
ms.reviewer: luleon
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3c115414fc6ae599342192196e256597c28e5aa6
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35293199"
---
# <a name="configure-single-sign-on-to-applications-that-are-not-in-the-azure-active-directory-application-gallery"></a>Configurar início de sessão único para aplicações que não estejam na Galeria de aplicações do Azure Active Directory

Este artigo é sobre uma funcionalidade que permite aos administradores configurar início de sessão único para aplicações não estão presentes na Galeria de aplicações do Azure Active Directory *sem escrever código*. Esta funcionalidade foi lançada a partir do technical preview no 18 de Novembro de 2015 e está incluída no [Azure Active Directory Premium](active-directory-whatis.md). Se estiver em vez disso, a procurar orientações para programadores sobre como integrar aplicações personalizadas com o Azure AD através do código, consulte o artigo [cenários de autenticação para o Azure AD](active-directory-authentication-scenarios.md).

Galeria de aplicações do Azure Active Directory fornece uma lista de aplicações que se sabe suportarem um formulário de início de sessão no Azure Active Directory, conforme descrito em [neste artigo](manage-apps/what-is-single-sign-on.md). Uma vez (como um IT especialista em integrador na sua organização) encontrar a aplicação que pretende ligar, pode começar a utilizar seguindo as instruções passo a passo apresentadas no portal do Azure para ativar o início de sessão único.

Clientes com [Azure Active Directory Premium](active-directory-whatis.md) licença também obter estas capacidades adicionais:

* Integração de self-service de qualquer aplicação que suporta SAML 2.0 fornecedores de identidade (iniciado por SP ou iniciadas por IdP)
* Integração de self-service de qualquer aplicação web que tem um baseado em HTML página de início de sessão utilizando [SSO baseada em palavra-passe](manage-apps/what-is-single-sign-on.md#password-based-single-sign-on)
* Ligação self-service das aplicações que utilizam o protocolo SCIM para aprovisionamento de utilizadores ([descrito aqui](manage-apps/use-scim-to-provision-users-and-groups.md))
* Capacidade de adicionar ligações a qualquer aplicação no [iniciador da aplicação do Office 365](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) ou [painel de acesso do Azure AD](manage-apps/what-is-single-sign-on.md#deploying-azure-ad-integrated-applications-to-users)

Isto pode incluir não apenas aplicações de SaaS que utilizar, mas tem não ainda foi integrada para a Galeria de aplicações do Azure AD, mas as aplicações web de terceiros que tenha implementado organização servidores que controlar, ou na nuvem ou no local.

Estas capacidades, também conhecidas como *modelos de integração de aplicação*, forneça pontos de ligação baseada em normas para aplicações que suportam SAML, SCIM ou autenticação baseada em formulários e incluem opções flexíveis e as definições de compatibilidade com um número abrangente de aplicações. 

## <a name="adding-an-unlisted-application"></a>Adicionar uma aplicação não listada
Para ligar uma aplicação utilizando um modelo de integração de aplicações, inicie sessão no portal do Azure com a sua conta de administrador do Azure Active Directory. Navegue para o **do Active Directory > aplicações da empresa > nova aplicação > aplicação Galeria não** secção, selecione **adicionar**e, em seguida, **adicionar uma aplicação a partir da Galeria** .

  ![](./media/active-directory-saas-custom-apps/customapp1.png)

Na Galeria de aplicações, pode adicionar uma aplicação não listada, selecionando o **aplicação Galeria não** mosaico que é apresentado nos resultados da pesquisa se a aplicação pretendida não foi encontrada. Depois de introduzir um nome para a sua aplicação, pode configurar o comportamento e as opções de início de sessão único. 

**Sugestão rápida**: como uma melhor prática, utilize a função de pesquisa para verificar se a aplicação já existe na Galeria de aplicações. Se a aplicação é localizada e respetiva descrição menciona suportadas o início de sessão único, a aplicação já é suportada para federado-início de sessão único.

  ![](./media/active-directory-saas-custom-apps/customapp2.png)

Adicionar uma aplicação desta forma fornece uma experiência semelhante ao disponíveis para aplicações previamente integradas. Para começar, selecione **configurar Single Sign-On** ou clique em **de sessão único-** a partir do menu de navegação esquerdo da aplicação. O ecrã seguinte apresenta as opções para configurar o início de sessão único. As opções são descritas nas secções seguintes deste artigo.
  
![](./media/active-directory-saas-custom-apps/customapp3.png)

## <a name="saml-based-single-sign-on"></a>Início de sessão único baseado em SAML
Selecione esta opção para configurar a autenticação baseada em SAML para a aplicação. Isto requer que a aplicação suporta SAML 2.0. Deve recolher informações sobre como utilizar as capacidades SAML da aplicação antes de continuar. Conclua as secções seguintes para configurar o início de sessão entre a aplicação e o Azure AD.

### <a name="enter-basic-saml-configuration"></a>Introduza a configuração básica de SAML

Para configurar o Azure AD, introduza a configuração básica de SAML. Pode introduzir os valores ou carregar um ficheiro de metadados para extrair o valor dos campos manualmente.

  ![Domínio litware e URLs](./media/active-directory-saas-custom-apps/customapp4.png)

- **URL de início de sessão (iniciado por SP apenas)** – onde o utilizador passa para início de sessão para esta aplicação. Se a aplicação está configurada para executar serviço iniciadas por fornecedor-início de sessão único, em seguida, quando um utilizador navega para este URL, o fornecedor de serviços será efetuado o redirecionamento necessário para o Azure AD para autenticar e iniciar sessão do utilizador no. Se este campo é preenchido, Azure AD irá utilizar este URL para iniciar a aplicação do Office 365 e o painel de acesso do Azure AD. Se este campo for omitido, em seguida, do Azure AD em vez disso, executará o fornecedor de identidade-iniciada pelo início de sessão quando a aplicação é iniciada a partir do Office 365, o painel de acesso do Azure AD, ou do do Azure AD URL single sign-on (copyable a partir do separador Dashboard).
- **Identificador** -deve identificar exclusivamente a aplicação para o qual o início de sessão único está a ser configurado. Pode encontrar este valor como o elemento de emissor a AuthRequest (pedido de SAML) enviado pela aplicação. Este valor é também apresentado como o **ID de entidade** nos metadados qualquer SAML fornecido pela aplicação. Consulte a documentação de SAML da aplicação para obter detalhes sobre o que é o respetivo valor de ID de entidade ou o público-alvo. 

    Segue-se um exemplo de como o identificador ou o emissor aparece no pedido SAML enviado pela aplicação para o Azure AD:

    ```
    <samlp:AuthnRequest
    xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
    ID="id6c1c178c166d486687be4aaf5e482730"
    Version="2.0" IssueInstant="2013-03-18T03:28:54.1839884Z"
    xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
    </samlp:AuthnRequest>
    ```

- **URL de resposta** -o URL de resposta é onde a aplicação de espera receber o token SAML. Este é também referido como o URL do serviço de consumidor da asserção (ACS). Consulte a documentação de SAML da aplicação para obter detalhes sobre a resposta de token SAML URL ou URL do ACS. 

    Para configurar várias replyURLs pode utilizar o seguinte script do PowerShell.

    ```PowerShell
    $sp = Get-AzureADServicePrincipal -SearchString "<Exact App  name>"
    $app = Get-AzureADApplication -SearchString "<Exact app name>"
    Set-AzureADApplication -ObjectId $app.ObjectId -ReplyUrls "<ReplyURLs>"
    Set-AzureADServicePrincipal -ObjectId $sp.ObjectId -ReplyUrls "<ReplyURLs>"
    ```

Para obter mais informações, consulte [pedidos de autenticação SAML 2.0 e as respostas que suporta o Azure Active Directory (Azure AD)](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference?/?WT.mc_id=DOC_AAD_How_to_Debug_SAML)


### <a name="review-or-customize-the-claims-issued-in-the-saml-token"></a>Rever ou personalizar as afirmações emitidas no SAML token

Quando um utilizador efetua a autenticação para a aplicação, do Azure AD irá emitir um token SAML para a aplicação que contém informações (ou afirmações) sobre o utilizador que identifica exclusivamente. Por predefinição inclui o nome de utilizador, endereço de correio eletrónico, nome próprio e apelido do utilizador. 

Pode ver ou editar as afirmações enviadas no token SAML para a aplicação a **atributos** separador.

  ![](./media/active-directory-saas-custom-apps/customapp7.png)

Existem dois motivos por que razão poderá ser necessário editar as afirmações emitidas no SAML token:

- A aplicação foi escrita para solicitar a um conjunto diferente de afirmação URIs ou valores de afirmação.
- A aplicação foi implementada de forma que requer a afirmação NameIdentifier ser algo que o nome de utilizador (também conhecidas como nome principal do utilizador) armazenado no Azure Active Directory. 

Para obter mais informações, consulte [personalizar afirmações emitidas no token SAML para aplicações da empresa](./develop/active-directory-saml-claims-customization.md). 



### <a name="review-certificate-expiration-data-status-and-email-notification"></a>Data de expiração do certificado de revisão, o estado e notificação por e-mail

Quando cria uma galeria ou uma aplicação não galeria, do Azure AD irá criar um certificado específico da aplicação com uma data de expiração de 3 anos a contar da data de criação. É necessário que este certificado para configurar a confiança entre o Azure AD e a aplicação. Para obter detalhes sobre o formato de certificado, consulte a documentação de SAML da aplicação. 

Do Azure AD, pode transferir o certificado em Base64 ou formato não processado. Além disso, pode obter o certificado ao transferir o ficheiro de XML de metadados de aplicação ou utilizando o URL de metadados de Federação da aplicação.

  ![Certificado](./media/active-directory-saas-custom-apps/certificate.png)

Certifique-se de que o certificado tem:

- A data de expiração pretendido. Pode configurar a data de expiração no máximo três anos.
- Estado do Active Directory. Se o estado está inativo, altere o estado para o Active Directory. Para alterar o estado, verifique **Active Directory** e, em seguida, guarde a configuração. 
- O e-mail de notificação correto. Quando o certificado do Active Directory estiver próxima da data de expiração, o Azure AD irá enviar uma notificação para o endereço de e-mail configurado neste campo.  

Para obter mais informações, consulte [gerir os certificados federado início de sessão no Azure Active Directory](manage-apps/manage-certificates-for-federated-single-sign-on.md).

### <a name="set-up-target-application"></a>Configurar a aplicação de destino

Para configurar a aplicação para o início de sessão único, localize a documentação da aplicação. Para encontrar a documentação, desloque-se ao fim da página configuração baseada em SAML início de sessão e, em seguida, clique em **configurar <application name>** . 

Os valores necessários variam de acordo com a aplicação. Para obter detalhes, consulte a documentação de SAML da aplicação. O início de sessão e o URL do serviço fim de sessão ambos resolver para o mesmo ponto de final, o que é o ponto final de processamento de pedidos SAML para a instância do Azure AD. O ID de entidade de SAML é o valor que é apresentado como o emissor no token SAML que é emitido para a aplicação.


### <a name="assign-users-and-groups-to-your-saml-application"></a>Atribuir utilizadores e grupos para a aplicação de SAML

Assim que a sua aplicação tiver sido configurada para utilizar o Azure AD como um fornecedor de identidade baseada em SAML, está quase pronto para testar. Como um controlo de segurança do Azure AD não irá emitir um token de permitir que um utilizador iniciar sessão na aplicação, a menos que o Azure AD concedeu acesso ao utilizador. Podem ser concedido acesso a utilizadores diretamente ou através de uma associação de grupo. 

Para atribuir um utilizador ou grupo à sua aplicação, clique o **atribuir utilizadores** botão. Selecione o utilizador ou grupo que pretende atribuir e, em seguida, selecione o **atribuir** botão.

  ![](./media/active-directory-saas-custom-apps/customapp6.png)

Atribuição de um utilizador irá permitir que o Azure AD para emitir um token para o utilizador. Também faz com que um mosaico para esta aplicação a aparecer no painel de acesso do utilizador. Um mosaico de aplicações também será apresentado o iniciador da aplicação do Office 365 se o utilizador estiver a utilizar o Office 365. 

> [!NOTE] 
> Pode carregar um logótipo de mosaico para a aplicação utilizando o **carregar o logótipo** botão no **configurar** separador para a aplicação. 


### <a name="test-the-saml-application"></a>Testar a aplicação de SAML

Antes de testar a aplicação de SAML, tem de ter configurar a aplicação com o Azure AD e atribuídos a utilizadores ou grupos à aplicação.

  ![Testes](./media/active-directory-saas-custom-apps/testing.png)

Página início de sessão único, clique em **definições de teste SAML** na secção de URLs e domínio. Esta ação abre um painel de conteúdo com instruções sobre como testar a aplicação.

1. Inicie sessão aplicação. Se a aplicação está configurada como serviço iniciadas por fornecedor-início de sessão único, são redirecionados para a URL de início de início de sessão único em que pode iniciar o início de sessão. Se a aplicação está configurada como identidade iniciadas por fornecedor-início de sessão único, em seguida, que é assinados sessão à aplicação.
2.  Se vir qualquer erro na sua página de início de sessão da empresa, copie o erro e voltar para o Azure AD único início de sessão no painel de conteúdo de teste. Cole o erro para a caixa e clique em **obter os passos de resolução**. Se o erro na página da aplicação, terá de contactar o fornecedor da aplicação e partilhar a sua configuração no Azure AD para validar os valores. 
3.  Com base sobre o erro, o Azure AD fornece os passos específicos sobre como resolver o problema.

Para obter mais informações, consulte [como depurar baseados em SAML-início de sessão único para aplicações no Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-saml-debugging/?WT.mc_id=DMC_AAD_?WT.mc_id=UI_AAD_Configure_NonGalleryApps)


## <a name="password-single-sign-on"></a>Palavra-passe-início de sessão único

Selecione esta opção para configurar [baseada em palavra-passe de início de sessão](manage-apps/what-is-single-sign-on.md) para uma aplicação web que tem uma página de início de sessão HTML. Baseado em palavra-passe SSO, também referido como palavra-passe vaulting, permite-lhe gerir o acesso de utilizador e palavras-passe de aplicações web que não suporta a Federação de identidade. Também é útil para cenários em que várias os utilizadores precisam de partilhar uma única conta, tal como para contas de aplicação da sua organização de redes sociais. 

Depois de selecionar **seguinte**, será solicitado para introduzir o URL da baseada na web início de sessão na página da aplicação. Tenha em atenção que tem de ser a página que inclua os campos de entrada de nome de utilizador e palavra-passe. Uma vez introduzidas, do Azure AD inicia um processo para analisar a página de início de sessão para introduzir um nome de utilizador e uma palavra-passe de entrada. Se o processo não for bem sucedido, em seguida,-orienta-o num processo de instalação de uma extensão de browser (requer o Internet Explorer, Chrome ou Firefox) que irá permitir-lhe capturar manualmente os campos alternativo.

Depois da página de início de sessão é capturada, podem ser atribuídos a utilizadores e grupos e políticas de credencial podem ser definidas como regular [palavra-passe SSO aplicações](manage-apps/what-is-single-sign-on.md).

> [!NOTE] 
> Pode carregar um logótipo de mosaico para a aplicação utilizando o **carregar o logótipo** botão no **configurar** separador para a aplicação. 
>

## <a name="existing-single-sign-on"></a>Existente-início de sessão único
Selecione esta opção para adicionar uma ligação para uma aplicação no portal de painel de acesso do Azure AD ou o Office 365 da sua organização. Pode utilizar este para adicionar hiperligações para aplicações web personalizado que a utilizam atualmente os serviços de Federação do Active Directory do Azure (ou outro serviço de Federação) em vez do Azure AD para autenticação. Em alternativa, pode adicionar ligações avançadas para páginas do SharePoint específicas ou de outras páginas web que pretende aparecem no painel de acesso do utilizador. 

Depois de selecionar **seguinte**, será solicitado para introduzir o URL da aplicação para ligar a. Depois de concluída, utilizadores e grupos que podem ser atribuídos a aplicação, o que faz com que a aplicação a aparecer no [iniciador da aplicação do Office 365](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) ou [painel de acesso do Azure AD](manage-apps/what-is-single-sign-on.md#deploying-azure-ad-integrated-applications-to-users) para os utilizadores.

> [!NOTE] 
> Pode carregar um logótipo de mosaico para a aplicação utilizando o **carregar o logótipo** botão no **configurar** separador para a aplicação. 
>

## <a name="related-articles"></a>Artigos relacionados

- [Índice de Artigos da Gestão da Aplicação no Azure Active Directory](active-directory-apps-index.md)
- [Como personalizar afirmações emitidas no Token SAML para aplicações previamente integradas](active-directory-saml-claims-customization.md)
- [Resolução de problemas baseados em SAML Single Sign-On](active-directory-saml-debugging.md)

