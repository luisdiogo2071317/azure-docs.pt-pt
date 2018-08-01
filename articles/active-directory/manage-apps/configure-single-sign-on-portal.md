---
title: Único início de sessão em gestão de aplicações empresariais no Azure Active Directory | Documentos da Microsoft
description: Gerir definições de início de sessão único para aplicações empresariais na sua organização a partir da Galeria de aplicações do Azure Active Directory
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/19/2017
ms.author: barbkess
ms.reviewer: asmalser
ms.openlocfilehash: 81b959a08f55f13fd0bcadce32b65ba64f9bb270
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/31/2018
ms.locfileid: "39365496"
---
# <a name="managing-single-sign-on-for-enterprise-apps"></a>Gerir o início de sessão único para aplicações empresariais

Este artigo descreve como utilizar o [portal do Azure](https://portal.azure.com) para gerir definições de início de sessão único para aplicações empresariais. Aplicações empresariais são as aplicações que são implementadas e utilizadas na sua organização. Este artigo aplica-se especialmente para aplicações que foram adicionadas a partir da [Galeria de aplicações do Azure Active Directory](what-is-single-sign-on.md#get-started-with-the-azure-ad-application-gallery). 

## <a name="finding-your-apps-in-the-portal"></a>Encontrar as suas aplicações no portal
Todas as aplicações de empresa que esteja configuradas para início de sessão único podem ser vistas e geridas no portal do Azure. Os aplicativos podem ser encontrados no **todos os serviços** &gt; **aplicações empresariais** seção do portal. 

![Painel de aplicações empresariais](./media/configure-single-sign-on-portal/enterprise-apps-blade.png)

Selecione **todos os aplicativos** para ver uma lista de todas as aplicações que tenham sido configurados. Selecionar uma aplicação mostra os recursos para essa aplicação, onde relatórios podem ser visualizados para essa aplicação e uma variedade de definições pode ser gerenciada.

Para gerir as definições de início de sessão únicas, selecione **início de sessão único**.

![Painel de recursos de aplicação](./media/configure-single-sign-on-portal/enterprise-apps-sso-blade.png)

## <a name="single-sign-on-modes"></a>Modos de início de sessão únicos
**Início de sessão único** começa com um **modo** menu, que permite que o único início de sessão em modo de ser configurado. As opções disponíveis incluem:

* **Baseado em SAML logon** -esta opção está disponível se o aplicativo oferece suporte completo início de sessão único federado no Azure Active Directory utilizando o protocolo SAML 2.0, WS-Federation, ou OpenID connect protocolos.
* **Com base em palavra-passe de início de sessão** -esta opção está disponível se o Azure AD suporta preenchendo para esta aplicação de formulário de palavra-passe.
* **Ligado início de sessão** -anteriormente conhecido como "Existente início de sessão único", esta opção permite aos administradores colocar uma ligação a esta aplicação no iniciador de aplicações do painel de acesso do Azure AD ou Office 365 do seu usuário.

Para obter mais informações sobre esses modos, consulte [como início de sessão único com o trabalho do Azure Active Directory](what-is-single-sign-on.md#how-does-single-sign-on-with-azure-active-directory-work).

## <a name="saml-based-sign-on"></a>Baseado em SAML início de sessão
O **baseado em SAML logon** opção está dividida em quatro seções:

### <a name="domains-and-urls"></a>Domínios e URLs
Isso é onde todos os detalhes sobre o domínio e URLs do aplicativo são adicionados ao seu diretório do Azure AD. Todas as entradas necessárias para tornar a aplicação de trabalho de início de sessão único são apresentadas diretamente na tela, enquanto todas as entradas opcionais podem ser visualizadas selecionando o **Mostrar definições de URL avançadas** caixa de verificação. A lista completa de entradas suportadas incluem:

* **início de sessão no URL** – onde o utilizador acede para iniciar sessão na aplicação. Se a aplicação está configurada para executar iniciadas pelo fornecedor de serviço início de sessão único, quando um utilizador abre este URL, o provedor de serviços redireciona para o Azure AD para autenticar e o utilizador iniciar sessão. 
  * Se este campo é preenchido, o Azure AD utiliza o URL para iniciar a aplicação do Office 365 e o painel de acesso do Azure AD.
  * Se este campo for omitido, em seguida, do Azure AD em vez disso, executa iniciada pelo fornecedor de identidade de início de sessão quando a aplicação é iniciada a partir do Office 365, o painel de acesso do Azure AD, ou do Azure AD único início de sessão no URL.
* **Identificador** -este URI deve identificar exclusivamente o aplicativo para o qual o início de sessão único está a ser configurado. Este é o valor que o Azure AD envia para a aplicação como o parâmetro de audiência do SAML token e espera-se que o aplicativo validá-la. Este valor também é apresentado como o ID de entidade em quaisquer metadados SAML fornecido pela aplicação.
* **URL de resposta** -o URL de resposta é onde a aplicação espera receber o token SAML. Isto é também referido como o URL do serviço de consumidor de asserção (ACS). Depois de estes serem introduzidas, clique em seguinte para continuar para o ecrã seguinte. Este ecrã fornece informações sobre o que precisa ser configurado no lado do aplicativo para ativá-la aceitar um token SAML do Azure AD.
* **Estado de reencaminhamento** -o estado de reencaminhamento é um parâmetro opcional que pode ajudar a definir no aplicativo para onde redirecionar o utilizador após a conclusão de autenticação. Normalmente, o valor é um URL válido no aplicativo, no entanto, alguns aplicativos usam esse campo de forma diferente (consulte único início de sessão em documentação a aplicação para obter detalhes). A capacidade de definir o estado de reencaminhamento é um novo recurso que seja exclusivo para o novo portal do Azure.

### <a name="user-attributes"></a>Atributos do Utilizador
Isso é onde os administradores podem ver e editar os atributos que são enviados no token SAML que o Azure AD emite para a aplicação quando os utilizadores iniciarem sessão.

O atributo editable apenas suportado é o **identificador de utilizador** atributo. O valor deste atributo é o campo no Azure AD que identifica unicamente cada usuário dentro do aplicativo. Por exemplo, se a aplicação foi implementada utilizando o "endereço de E-Mail" como o nome de utilizador e o identificador exclusivo, em seguida, o valor deve ser definido para o campo "user.mail" no Azure AD.

### <a name="saml-signing-certificate"></a>Certificado de Assinatura de SAML
Esta secção mostra os detalhes do certificado que o Azure AD utiliza para assinar os tokens SAML emitidos para o aplicativo sempre que o utilizador é autenticado. Ele permite que as propriedades do certificado atual que serão verificadas, incluindo a data de expiração.

### <a name="application-configuration"></a>Configuração da aplicação
A seção final fornece a documentação e/ou os controlos necessários para configurar a própria aplicação para utilizar o Azure Active Directory como um fornecedor de identidade.

O **configurar aplicação** submenus menu fornece novas concisas embedded instruções para configurar o aplicativo. Este é outro novo recurso exclusivo para o novo portal do Azure.

> [!NOTE]
> Para obter um exemplo completo de documentação do embedded, ver a aplicação do Salesforce.com. Documentação de aplicações adicionais está a ser adicionada continuamente.
> 
> 

![Embedded docs](./media/configure-single-sign-on-portal/enterprise-apps-blade-embedded-docs.png)

## <a name="password-based-sign-on"></a>Com base em palavra-passe de início de sessão
Se for suportado para a aplicação, selecionar o modo SSO baseado em palavra-passe e selecionando **guardar** instantaneamente configura-o para fazer o SSO baseado em palavra-passe. Para obter mais informações sobre a implementação do SSO de palavra-passe, consulte [como início de sessão único com o trabalho do Azure Active Directory](what-is-single-sign-on.md#how-does-single-sign-on-with-azure-active-directory-work).

![Com base em palavra-passe de início de sessão](./media/configure-single-sign-on-portal/enterprise-apps-blade-password-sso.png)

## <a name="linked-sign-on"></a>O início de sessão ligado
Se for suportado para a aplicação, selecionar o modo SSO ligado permite-lhe introduzir o URL que pretende que o painel de acesso do Azure AD ou Office 365 para redirecionar para quando os utilizadores clicam nesta aplicação. Para obter mais informações sobre SSO ligado (anteriormente conhecido como "SSO existente"), consulte [como início de sessão único com o trabalho do Azure Active Directory](what-is-single-sign-on.md#how-does-single-sign-on-with-azure-active-directory-work).

![O início de sessão ligado](./media/configure-single-sign-on-portal/enterprise-apps-blade-linked-sso.png)

## <a name="feedback"></a>Comentários

Esperamos que como usar a melhor experiência do Azure AD. Mantenha os comentários chegando! Poste seus comentários e ideias para aprimoramento no **do Portal de administração** secção do nosso [fórum de comentários](https://feedback.azure.com/forums/169401-azure-active-directory/category/162510-admin-portal).  Nós estamos muito entusiasmados sobre como criar algo novo e interessante todos os dias e utilize as diretrizes a forma e definir o que devemos criar a seguir.

