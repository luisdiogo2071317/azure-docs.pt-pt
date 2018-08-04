---
title: O que é o acesso a aplicações e início de sessão único com o Azure Active Directory? | Microsoft Docs
description: Utilize o Azure Active Directory para ativar o início de sessão único para todas as aplicações SaaS e web que precisa para empresas.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: users-groups-roles
ms.workload: identity
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: barbkess
ms.reviewer: asmalser
ms.custom: it-pro
ms.openlocfilehash: 1a7ecbe42857e522785d5919d46b783feae4caeb
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/03/2018
ms.locfileid: "39494130"
---
# <a name="what-is-application-access-and-single-sign-on-with-azure-active-directory"></a>O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?
Início de sessão único significa ser capaz de aceder a todas as aplicações e recursos de que precisa para fazer negócios, ao iniciar sessão apenas uma vez com uma conta de utilizador único. Depois de iniciar sessão, pode aceder a todos os aplicativos que necessita sem ter que está a ser necessária para autenticar (por exemplo, escreva uma palavra-passe) uma segunda vez.

Muitas organizações contam com software como um aplicativo de serviço (SaaS), como o Office 365, a caixa e Salesforce para produtividade do utilizador final. Historicamente, a equipe de TI precisa individualmente criar e atualizar as contas de utilizador em cada aplicação SaaS, e os utilizadores tenham de memorizar uma palavra-passe para cada aplicação SaaS.

O Azure Active Directory estende Active Directory no local para a cloud, permitindo que os utilizadores utilizem suas contas institucionais primária para não apenas iniciar sessão nos seus dispositivos associados a um domínio e recursos da empresa, mas também todos os web e aplicações SaaS necessário para seu trabalho.

Portanto, não apenas os utilizadores não têm de gerir vários conjuntos de nomes de utilizador e palavras-passe, seus aplicativos de acesso pode ser aprovisionado ou desaprovisionado automaticamente com base nos seus membros do grupo de organização e também o respetivo estado como um funcionário. O Azure Active Directory apresenta os controlos de governação acesso e segurança que lhe permite gerir centralmente o acesso dos utilizadores em aplicações SaaS.

O Azure AD permite integração fácil a muitas aplicações SaaS populares de hoje; fornece gestão de identidades e acesso e permite que os usuários para início de sessão único para aplicações diretamente, ou descobrir e iniciá-los a partir de um portal como o Office 365 ou o painel de acesso do Azure AD.

A arquitetura da integração consiste dos seguintes quatro principais Blocos Modulares:

* Início de sessão único permite aos utilizadores aceder às suas aplicações de SaaS, com base nas suas contas institucionais no Azure AD. Início de sessão único é o que permite que os utilizadores sejam autenticados para uma aplicação com suas contas institucionais única.
* O aprovisionamento de utilizador permite utilizador provisionamento e desprovisionamento para SaaS com base nas alterações feitas no Windows Server Active Directory e/ou do Azure AD de destino. Uma conta de aprovisionamento é o que permite que um utilizador ser autorizado a utilizar uma aplicação, depois de serem autenticados através do início de sessão único.
* Gestão de acesso de aplicativos centralizado no acesso a aplicações do Azure permite portal ponto único de SaaS e a gestão, com a capacidade de delegar a tomada de decisões de acesso de aplicação e aprovações para todas as pessoas na organização
* Unificação de relatórios e monitoramento da atividade do utilizador no Azure AD

## <a name="how-does-single-sign-on-with-azure-active-directory-work"></a>Como funciona um início de sessão único com o Azure Active Directory?
Quando os utilizadores iniciam sessão a uma aplicação, vão através de um processo de autenticação em que são necessárias para provar que são quem dizem que eles são. Sem início de sessão único, este processo de autenticação é geralmente feito ao introduzir uma palavra-passe armazenados no aplicativo e os utilizadores têm de saber esta palavra-passe.

O Azure AD suporta três formas diferentes de iniciar sessão nas aplicações:

* **Único início de sessão federado** permite que os aplicativos redirecionar para o Azure AD para autenticação de utilizador em vez de solicitar a sua própria palavra-passe. Início de sessão único federado é suportada para aplicações que suporte protocolos, tais como SAML 2.0, WS-Federation e OpenID Connect e é o modo mais sofisticado de início de sessão único.
* **Com base em palavra-passe de início de sessão único** permite proteger o armazenamento de palavra-passe de aplicação e utilizar uma extensão de browser ou aplicação móvel de repetição. Com base em palavra-passe de início de sessão único usa o processo existente fornecido pela aplicação, mas permite que um administrador gerir as palavras-passe e não exige que o usuário saber a palavra-passe.
* **Existente início de sessão único** permite ao Azure AD tirar partido de qualquer existente início de sessão único que tenha sido configurado para o aplicativo, mas permite que esses aplicativos a ser ligado aos portais de painel de acesso do Office 365 ou do Azure AD e também ativa adicionais relatórios no Azure AD quando as aplicações são iniciadas lá.

Assim que um usuário foi autenticado com uma aplicação, também têm de ter um registo de conta aprovisionado no aplicativo que informa ao aplicativo, onde as permissões e o nível de acesso estão dentro do aplicativo. O aprovisionamento deste registo de conta pode optar por ocorrer automaticamente, ou pode ocorrer manualmente por um administrador antes do utilizador é fornecido o acesso de início de sessão único.

 Obter mais detalhes sobre estes modos de início de sessão únicos e o aprovisionamento abaixo.

### <a name="federated-single-sign-on"></a>Início de sessão único federado
Início de sessão único federado permite que os utilizadores na sua organização iniciar sessão automaticamente uma aplicação SaaS de terceiros pelo Azure AD com as informações de conta de utilizador do Azure AD.

Neste cenário, quando já foram registados com o Azure AD e, se pretender aceder aos recursos que são controlados por uma aplicação SaaS de terceiros, o Federação elimina a necessidade de um usuário pode ser reautenticada.

O Azure AD pode suporta início de sessão único Federado com aplicações que suportam o SAML 2.0, WS-Federation, ou OpenID connect protocolos.

Consulte também: [gerir certificados para federado início de sessão único](manage-certificates-for-federated-single-sign-on.md)

### <a name="password-based-single-sign-on"></a>Início de sessão único baseado em palavra-passe
Configuração baseada em palavra-passe de início de sessão único permite que os utilizadores na sua organização iniciar sessão automaticamente uma aplicação SaaS de terceiros pelo Azure AD com as informações de conta de utilizador da aplicação SaaS de terceiros. Quando ativa esta funcionalidade, o AD do Azure recolhe e armazena em segurança as informações de conta de utilizador e a palavra-passe relacionada.

O Azure AD pode suportar baseado em palavra-passe de início de sessão único para qualquer aplicativo baseado na nuvem que tem uma baseada em HTML início de sessão na página. Ao utilizar um plug-in de navegador personalizado, o AAD automatiza o processo de início de sessão por meio de forma segura a obter credenciais de aplicativo, como o nome de utilizador e a palavra-passe do diretório e introduz estas credenciais para a página de início de sessão do aplicativo em nome do utilizador. Existem dois casos de utilização:

1. **Administrador gere credenciais** – os administradores podem criar e gerir credenciais de aplicação e atribuir essas credenciais para utilizadores ou grupos que necessitam de aceder à aplicação. Nestes casos, o utilizador final não precisa de saber as credenciais, mas ainda obtém acesso de início de sessão único para o aplicativo simplesmente clicando na mesma no seu painel de acesso ou através de uma ligação fornecida. Este processo permite ambos, gerenciamento de ciclo de vida das credenciais pelo administrador, bem como conveniência para os utilizadores finais no qual não é necessário lembrar-se ou gerir palavras-passe de aplicações específicas. As credenciais são oculto do utilizador final durante o processo automatizado de início de sessão; No entanto, eles são tecnicamente Detetáveis pelo utilizador utilizando ferramentas de depuração de web e os utilizadores e administradores devem seguir as mesmas políticas de segurança como se as credenciais eram apresentadas diretamente pelo utilizador. Credenciais fornecidas pelo administrador são úteis ao fornecer acesso de conta que é partilhado entre vários usuários, como de redes sociais ou aplicativos de compartilhamento de documentos.
2. **Utilizador gere credenciais** – os administradores podem atribuir aplicativos a fim de utilizadores ou grupos e permitir que os utilizadores finais introduzir as suas próprias credenciais diretamente após o acesso à aplicação pela primeira vez no seu painel de acesso. Esta ação cria uma conveniência para os utilizadores finais no qual eles não precisam inserir continuamente as palavras-passe de aplicações específicas a cada vez que acederem à aplicação. Os utilizadores podem continuar a gerir as palavras-passe, atualizar ou excluí-los conforme necessário. Este caso de utilização também pode ser utilizado como uma etapa fundamental para modelos administrativos \ Gestão de credenciais, no qual o administrador pode definir novas credenciais para a aplicação numa data futura sem alterar a experiência de acesso da aplicação do utilizador final.

Em ambos os casos, as credenciais são armazenadas num estado encriptado no diretório e, durante o processo de início de sessão automatizado, apenas são transmitidas através de HTTPS. Utilizar com base em palavra-passe de início de sessão único, o Azure AD oferece uma solução de gestão de acesso de identidade conveniente para aplicações que não têm capacidade de oferecer suporte a protocolos de Federação.

O SSO baseado em palavra-passe se baseia numa extensão de browser para obter a aplicação e informações específicas do usuário do Azure AD e aplicá-la para o serviço de forma segura. A maioria das aplicações de SaaS de terceiros que são suportadas pelo Azure AD suportam esta funcionalidade.

Para SSO de palavra-passe, os navegadores do usuário final podem ser:
* Internet Explorer 11 – no Windows 7 ou posterior
* Edge no Windows 10 Anniversary Edition ou posterior 
* Cromado, No Windows 7 ou posterior e no MacOS X ou posterior
* Firefox 26.0 ou mais tarde, no Windows XP SP2 ou posterior e no Mac OS X 10.6 ou posterior

### <a name="existing-single-sign-on"></a>Existente início de sessão único
Ao configurar o início de sessão único para uma aplicação, o portal do Azure fornece uma terceira opção de "existente Single Sign-On". Esta opção permite simplesmente ao administrador criar uma ligação para um aplicativo e coloque-a no painel de acesso dos utilizadores selecionados.

Por exemplo, se existir uma aplicação que está configurada para autenticar os utilizadores com o Active Directory Federation Services 2.0, um administrador pode utilizar a opção "existente início de sessão único" para criar uma ligação ao mesmo no painel de acesso. Quando os utilizadores acedem a ligação, são autenticados com o Active Directory Federation Services 2.0 ou qualquer que seja única início de sessão na solução existente é fornecida pela aplicação.

### <a name="user-provisioning"></a>Aprovisionamento de utilizadores
Para selecionar aplicativos, do Azure AD permite automatizado do utilizador provisionamento e desprovisionamento de contas em aplicações de SaaS de terceiros partir no portal do Azure, utilizando as suas informações de identidade do Windows Server Active Directory ou do Azure AD. Quando um utilizador recebe permissões no Azure AD para uma destas aplicações, pode ser automaticamente criada uma conta (aprovisionada) no destino aplicação SaaS.

Quando um utilizador é eliminado ou as informações de alteram no Azure AD, essas alterações também são refletidas na aplicação SaaS. Isso significa que, configurar a gestão de ciclo de vida de identidade automática permite aos administradores controlar e fornecer automatizada de aprovisionamento e cancelamento de aprovisionamento de aplicações SaaS. No Azure AD, esta automatização da gestão de ciclo de vida de identidades é ativada através do aprovisionamento de utilizador.

Para obter mais informações, consulte [aprovisionamento automatizado do utilizador e cancelar o aprovisionamento a aplicações SaaS](../active-directory-saas-app-provisioning.md)

## <a name="get-started-with-the-azure-ad-application-gallery"></a>Introdução à Galeria de aplicações do Azure AD
Pronto para começar? Para implementar o início de sessão único entre o Azure AD e aplicações SaaS que utiliza a sua organização, siga estas diretrizes.

### <a name="using-the-azure-ad-application-gallery"></a>Utilizar a Galeria de aplicações do Azure AD
O [Galeria de aplicações do Azure Active Directory](https://azure.microsoft.com/marketplace/active-directory/all/) fornece uma listagem dos aplicativos que são conhecidos para oferecer suporte a um formulário de início de sessão único com o Azure Active Directory.

![Galeria de aplicações online do Azure](media/what-is-single-sign-on/onlineappgallery.png)

Eis algumas sugestões para encontrar aplicações pelas quais capacidades suportam:

* O Azure AD suporta automática provisionamento e desprovisionamento para todas as aplicações "Em destaque" a [Galeria de aplicações do Azure Active Directory](https://azure.microsoft.com/marketplace/active-directory/all/).
* Uma lista de aplicações federadas que suportam especificamente único início de sessão federado utilizando um protocolo, tais como SAML, WS-Federation, ou OpenID Connect podem ser encontrada [aqui](http://social.technet.microsoft.com/wiki/contents/articles/20235.azure-active-directory-application-gallery-federated-saas-apps.aspx).

Depois de encontrar seu aplicativo, pode começar a utilizar, seguindo as instruções passo a passo na Galeria de aplicações e no portal do Azure para ativar o início de sessão único.

### <a name="application-not-in-the-gallery"></a>Aplicação não na galeria do?
Se seu aplicativo não for encontrado na Galeria de aplicações do Azure AD, em seguida, tem estas opções:

* **Adicionar uma aplicação não listada estiver a utilizar** -utilizar a categoria personalizado na Galeria de aplicações no portal do Azure para ligar uma aplicação não listada que sua organização está a utilizar. Pode adicionar qualquer aplicação que suporte SAML 2.0 como um aplicativo federado ou qualquer aplicativo que tenha uma baseada em HTML início de sessão página como uma aplicação SSO de palavra-passe. Para obter mais detalhes, veja este artigo sobre [adicionando seu próprio aplicativo](../application-config-sso-how-to-configure-federated-sso-non-gallery.md).
* **Adicionar a sua própria aplicação estiver a desenvolver** - se de que desenvolveram o aplicativo por conta própria, siga as diretrizes na documentação de programador do Azure AD para implementar o início de sessão único federado ou aprovisionamento com o Azure AD graph API. Para obter mais informações, consulte estes recursos:
  
  * [Cenários de autenticação do Azure AD](../develop/authentication-scenarios.md)
  * [https://github.com/AzureADSamples/WebApp-MultiTenant-OpenIdConnect-DotNet](https://github.com/AzureADSamples/WebApp-MultiTenant-OpenIdConnect-DotNet)
  * [https://github.com/AzureADSamples/WebApp-WebAPI-MultiTenant-OpenIdConnect-DotNet](https://github.com/AzureADSamples/WebApp-WebAPI-MultiTenant-OpenIdConnect-DotNet)
  * [https://github.com/AzureADSamples/NativeClient-WebAPI-MultiTenant-WindowsStore](https://github.com/AzureADSamples/NativeClient-WebAPI-MultiTenant-WindowsStore)
* **Pedir uma integração de aplicações** -pedido de suporte para a aplicação que precisa com o [fórum de comentários do Azure AD](https://feedback.azure.com/forums/169401-azure-active-directory/).

### <a name="using-the-azure-portal"></a>Utilizar o portal do Azure
Pode utilizar a extensão do Active Directory no portal do Azure para configurar a aplicação início de sessão único. Como primeiro passo, tem de selecionar um diretório da seção do Active Directory no portal do:

![](./media/what-is-single-sign-on/azuremgmtportal.png)

Para gerir as suas aplicações de SaaS de terceiros, pode mudar para o separador de aplicações do diretório selecionado. Esta vista permite que os administradores:

* Adicionar novas aplicações a partir da galeria do Azure AD, bem como as aplicações que estiver a desenvolver
* Eliminar aplicações integradas
* Gerir as aplicações que já tiver integrado

Tarefas administrativas típicas para uma aplicação SaaS de terceiros são:

* Ativar o início de sessão único com o Azure AD, utilizando SSO de palavra-passe ou, se disponível para o destino de SaaS, SSO federado
* Opcionalmente, ativar o aprovisionamento do utilizador, provisionamento e desprovisionamento (gestão de ciclo de vida de identidade) de utilizadores
* Para aplicações 
* Quando o aprovisionamento de utilizador está ativado, selecionar os utilizadores que têm acesso a essa aplicação

Para aplicações de galeria que suportam início de sessão único federado, normalmente a configuração requer que forneça definições de configuração adicionais, tais como certificados e os metadados para criar uma confiança federada entre a aplicação de terceiros e o Azure AD. O Assistente de configuração orienta os detalhes e fornece acesso fácil aos dados de específicas da aplicação SaaS e instruções.

Para aplicações de galeria que suportam o aprovisionamento de utilizador automático, isto requer a conceder permissões do Azure AD para gerir as contas na aplicação SaaS. No mínimo, tem de fornecer credenciais do Azure AD devem utilizar durante a autenticação sobre a aplicação de destino. Se as definições de configuração adicionais tem de ser fornecido depende dos requisitos do aplicativo.

## <a name="deploying-azure-ad-integrated-applications-to-users"></a>Implementar aplicações do Azure integradas no AD para utilizadores
O Azure AD fornece várias formas de personalizáveis para implementar aplicações para utilizadores finais em sua organização:

* Painel de acesso do Azure AD
* Iniciador de aplicações do Office 365
* Início de sessão direto em aplicações federadas
* Ligações avançadas para aplicações federadas, baseadas em palavras-passe ou existentes

Qual método (s) optar por implementar na sua organização é a seu critério.

### <a name="azure-ad-access-panel"></a>Painel de acesso do Azure AD
Painel de acesso em https://myapps.microsoft.com é um portal baseado na web que permite que um utilizador final com uma conta organizacional no Azure Active Directory para ver e aplicativos baseados na nuvem de lançamento para a qual tenham sido concedido acesso pelo administrador do Azure AD. Se for um utilizador final com [Azure Active Directory Premium](https://azure.microsoft.com/pricing/details/active-directory/), também pode utilizar as capacidades de gestão de grupos self-service através do painel de acesso.

![Painel de acesso do Azure AD](media/what-is-single-sign-on/azure-ad-access-panel.png)

O painel de acesso separado no portal do Azure e não requer que os utilizadores tenham uma subscrição do Azure ou uma subscrição do Office 365.

Para obter mais informações sobre o painel de acesso do Azure AD, consulte a [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

### <a name="office-365-application-launcher"></a>Iniciador de aplicações do Office 365
Para organizações que implantaram o Office 365, o aplicações atribuídas a utilizadores através do Azure AD também serão apresentadas no portal do Office 365 em https://portal.office.com/myapps. Isto torna mais fácil e conveniente para os utilizadores numa organização para iniciar as aplicações sem ter de utilizar um portal segundo e é a solução de início de aplicação recomendada para organizações que utilizam o Office 365.

![](./media/what-is-single-sign-on/officeapphub.png)

Para obter mais informações sobre o iniciador de aplicações do Office 365, consulte [ter a aplicação aparece no iniciador de aplicações do Office 365](https://msdn.microsoft.com/office/office365/howto/connect-your-app-to-o365-app-launcher).

### <a name="direct-sign-on-to-federated-apps"></a>Início de sessão direto em aplicações federadas
Mais aplicações federadas que suportam SAML 2.0, WS-Federation ou OpenID conectam também suporte a capacidade dos utilizadores iniciar a aplicação e, em seguida, é feita através do Azure AD ao redirecionamento automático ou ao clicar numa ligação para iniciar sessão. Isso é conhecido como fornecedor de serviços-iniciada pelo início de sessão e mais aplicações federadas na Galeria de aplicações do Azure AD suportam esta (consulte a documentação ligada a partir do Assistente de configuração de início de sessão único da aplicação no portal do Azure para obter detalhes).

![](./media/what-is-single-sign-on/workdaymobile.png)

### <a name="direct-sign-on-links-for-federated-password-based-or-existing-apps"></a>Ligações de início de sessão em diretas para aplicações federada, com base em palavra-passe ou existente
O Azure AD também suporta únicas início de sessão em ligações diretas para aplicativos individuais que oferecem suporte baseado em palavra-passe de início de sessão único existente início de sessão único e qualquer outra forma de início de sessão único federado.

Estas ligações são especificamente elaboradas URLs que enviam um utilizador através do processo de início de sessão do AD do Azure para uma aplicação específica sem a necessidade do lançamento de utilizador-los a partir do Azure AD aceder ao painel ou do Office 365. Estes URLs de início de sessão único no separador Dashboard de qualquer aplicação previamente integrada podem ser encontradas na seção do Active Directory do portal do Azure, conforme mostrado na captura de ecrã abaixo.

![](./media/what-is-single-sign-on/deeplink.png)

Estas ligações podem ser copiadas e coladas em qualquer lugar que deseja fornecer uma ligação de início de sessão à aplicação selecionada. Isto pode ser num e-mail ou em qualquer portal baseado na web personalizado que configurou para o acesso de aplicação do utilizador. Eis um exemplo de um URL do Azure AD direto único início de sessão do Twitter:

`https://myapps.microsoft.com/signin/Twitter/230848d52c8745d4b05a60d29a40fced`

Semelhante a URLs específicos da organização para o painel de acesso, pode personalizar ainda mais essa URL adicionando um dos domínios verificados ou Active Directory para o seu diretório depois do domínio myapps.microsoft.com. Isso garante que qualquer corporativa é carregada imediatamente na página de início de sessão sem o utilizador tenha de introduzir o ID de utilizador pela primeira vez:

`https://myapps.microsoft.com/contosobuild.com/signin/Twitter/230848d52c8745d4b05a60d29a40fced`

Quando um utilizador autorizado clica em um desses links específico do aplicativo, eles primeiro ver seus organizacional início de sessão na página (partindo do princípio de que não estejam assinados) e após o início de sessão são redirecionados para a sua aplicação sem parar no painel de acesso pela primeira vez. Se o utilizador está em falta a pré-requisitos para aceder à aplicação, como a extensão de browser de início de sessão único baseado em palavra-passe, a ligação pedirá ao utilizador para instalar a extensão em falta. O URL da ligação também permanece constante se a configuração única início de sessão para a aplicação for alterada.

Estas ligações utilizam os mesmos mecanismos de controlo de acesso, como o painel de acesso e o Office 365, e apenas esses utilizadores ou grupos a quem foi concedidos ao aplicativo no portal do Azure será capazes de autenticar com êxito. No entanto, qualquer utilizador que não está autorizado, verá uma mensagem que explica o que não foi concedidos acesso e recebem uma ligação para carregar o painel de acesso para ver as aplicações disponíveis para os quais têm acesso.

## <a name="related-articles"></a>Artigos relacionados
* [Índice de Artigos da Gestão da Aplicação no Azure Active Directory](../active-directory-apps-index.md)
* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](../saas-apps/tutorial-list.md)
* [Configurar a Cloud Discovery](/cloud-app-security/set-up-cloud-discovery)
* [Introdução à gestão de acesso a aplicações](what-is-access-management.md)
* [Comparar capacidades para o gerenciamento de identidades externas no Azure AD](../active-directory-b2b-compare-b2c.md)


