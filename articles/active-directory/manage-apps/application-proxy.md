---
title: Como fornecer acesso remoto seguro a aplicações no local
description: Abrange como utilizar o Proxy de aplicações do Azure AD para fornecer acesso remoto seguro às suas aplicações no local.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/31/2018
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 428f094dae2b9a69b58912190d2959a7dfc467ec
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/31/2018
ms.locfileid: "39365267"
---
# <a name="how-to-provide-secure-remote-access-to-on-premises-applications"></a>Como fornecer acesso remoto seguro a aplicações no local

Os funcionários pretendem hoje mesmo para serem produtivos em qualquer lugar, em qualquer altura e a partir de qualquer dispositivo. Eles querem trabalhar em seus próprios dispositivos, quer sejam tablets, telefones ou computadores portáteis. E esperam ser capazes de aceder a todos os seus aplicativos, ambas as aplicações de SaaS na cloud e aplicações empresariais no local. Fornecer acesso a aplicações no local sempre envolveu redes privadas virtuais (VPNs) ou desmilitarizada zonas (DMZs). Não só são essas soluções complexas e difíceis de tornar segura, mas são dispendiosas configurar e gerir.

Há uma maneira melhor!

Uma força de trabalho moderna no mobile-first, mundo de cloud-first precisa de uma solução de acesso remoto Moderno. Proxy de aplicações do Azure AD é um recurso do Azure Active Directory oferece acesso remoto como um serviço. Isso significa que é fácil de implementar, utilizar e gerir.

[!INCLUDE [identity](../../../includes/azure-ad-licenses.md)]

## <a name="what-is-azure-active-directory-application-proxy"></a>O que é o Proxy de aplicações do Azure Active Directory?
Proxy de aplicações do Azure AD fornece logon único (SSO) e acesso remoto seguro para aplicações web alojadas no local. Algumas aplicações que pretende publicar incluem sites do SharePoint, Outlook Web Access ou quaisquer outras aplicações web LOB que tem. Estas aplicações de web no local são integradas no Azure AD, a plataforma de controle que é utilizada pelo O365 e a mesma identidade. Os utilizadores finais podem aceder as aplicações no local da mesma forma que aceder ao Office 365 e outras aplicações SaaS integradas com o Azure AD. Não precisa de alterar a infraestrutura de rede ou precisar de uma VPN fornecer esta solução para os seus utilizadores.

## <a name="why-is-application-proxy-a-better-solution"></a>Por que é o Proxy de aplicações uma solução melhor?
Proxy de aplicações do Azure AD fornece uma solução de acesso remoto simples, segura e económica para todas as suas aplicações no local.

Proxy de aplicações do Azure AD é:

* **Simples**
   * Não precisa de alterar ou atualizar seus aplicativos para trabalhar com o Proxy de aplicações. 
   * Os utilizadores obtêm uma experiência de autenticação consistente. Podem utilizar o portal MyApps obter início de sessão único para ambas as aplicações de SaaS na cloud e aplicações no local. 
* **Proteger**
   * Quando publicar as suas aplicações com o Proxy de aplicações do Azure AD, pode tirar partido dos controlos de autorização avançadas e análises de segurança no Azure. Obtenha segurança de escala da cloud e recursos de segurança do Azure, como a verificação de dois passos e de acesso condicional.
   * Não tem de abrir ligações de entrada através da firewall para conceder aos seus utilizadores acesso remoto. 
* **Cost-effective**
   * Proxy de aplicações funciona na cloud, que lhe permite poupar tempo e dinheiro. Soluções no local, normalmente, tem de configurar e manter DMZs, servidores de borda ou outras infraestruturas complexas.  

## <a name="what-kind-of-applications-work-with-application-proxy"></a>Que tipo de trabalho de aplicações com o Proxy de aplicações?
Com o Proxy de aplicações do Azure AD pode aceder a diferentes tipos de aplicativos internos:

* Aplicações Web que utilizem [a autenticação integrada do Windows](application-proxy-configure-single-sign-on-with-kcd.md) para autenticação  
* Aplicativos que usam baseada em formulários da Web ou [baseado em cabeçalho](application-proxy-configure-single-sign-on-with-ping-access.md) acesso  
* APIs que deseja expor para aplicativos avançados em diferentes dispositivos para a Web  
* Aplicativos hospedados por trás de um [Gateway de ambiente de trabalho remoto](application-proxy-integrate-with-remote-desktop-services.md)  
* Aplicações de cliente rico que estão integradas com o Active Directory Authentication Library (ADAL)

## <a name="how-does-application-proxy-work"></a>Como funciona o Proxy de aplicações?
Existem dois componentes que terá de configurar para tornar o Proxy de aplicações de trabalho: um conector e um ponto final externo. 

O conector é um agente leve que se encontram no Windows Server no interior da rede. O conector facilita o fluxo de tráfego do serviço Proxy de aplicações na cloud à sua aplicação no local. Utiliza apenas as ligações de saída, para que não tenha de abrir portas de entrada nem colocar nada num DMZ. Os conectores são sem monitoração de estado e extraem informações a partir da cloud conforme necessário. Para obter mais informações sobre conectores, como como eles balanceamento de carga e autenticar, consulte [conectores de Proxy de aplicações do AD Azure compreender](application-proxy-connectors.md). 

O ponto final externo é a forma como os seus utilizadores alcançar seus aplicativos enquanto fora da sua rede. Podem optar por aceder diretamente a um URL externo que determinar, ou eles podem aceder à aplicação através do portal MyApps. Quando os utilizadores aceder a um desses pontos de extremidade, eles se autenticar no Azure AD e, em seguida, são encaminhados através do conector para o aplicativo no local.

 ![Diagrama de Proxy de aplicações do AzureAD](./media/application-proxy/azureappproxxy.png)

1. O usuário acessa o aplicativo através do serviço de Proxy de aplicações e é direcionado para a página de início de sessão do Azure AD para autenticar.
2. Depois de um sessão com êxito-, um token é gerado e enviado para o dispositivo de cliente.
3. O cliente envia o token para o serviço de Proxy de aplicações, que obtém o nome principal de utilizador (UPN) e o nome principal de segurança (SPN) do token, em seguida, direciona o pedido para o conector do Proxy de aplicações.
4. Se tiver configurado o início de sessão único, o conector efetua qualquer autenticação adicional necessária em nome do utilizador.
5. O conector envia o pedido para a aplicação no local.  
6. A resposta é enviada através do serviço Proxy de aplicações e o conector para o usuário.

### <a name="single-sign-on"></a>Início de sessão único
Proxy de aplicações do Azure AD fornece logon único (SSO) para aplicações que utilizam a autenticação integrada do Windows (IWA) ou aplicativos com reconhecimento de declarações. Se a sua aplicação utilizar o IWA, o Proxy de aplicações representa o utilizador a utilizar a delegação restrita de Kerberos para fornecer SSO. Se tiver um aplicativo compatível com declarações que confianças de entidades do Azure Active Directory, o SSO funciona porque o utilizador já foi autenticado pelo Azure AD.

Para obter mais informações sobre Kerberos, consulte [tudo que precisa saber sobre Kerberos Constrained Delegation (KCD)](https://blogs.technet.microsoft.com/applicationproxyblog/2015/09/21/all-you-want-to-know-about-kerberos-constrained-delegation-kcd).

### <a name="managing-apps"></a>Gerir aplicações
Assim que a aplicação é publicada com o Proxy de aplicações, pode geri-la como qualquer outra aplicação empresarial no portal do Azure. Pode utilizar funcionalidades de segurança do Azure Active Directory como verificação de dois passos e de acesso condicional, controlar permissões de utilizador e personalizar a imagem corporativa para a sua aplicação. 

## <a name="get-started"></a>Introdução

Antes de configurar o Proxy de aplicações, certifique-se de que tem um suportados [edição Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/) e um diretório do Azure AD para o qual for um administrador global.

Introdução ao Proxy de aplicações em duas etapas:

1. [Ativar o Proxy da aplicação e configurar o conector](application-proxy-enable.md).    
2. [Publicar aplicações](application-proxy-publish-azure-portal.md) -utilize o Assistente de forma rápido e fácil para obter as aplicações no local publicado e está acessível remotamente.

## <a name="whats-next"></a>Passos seguintes?
Depois de publicar seu primeiro aplicativo, há muito mais que pode fazer com o Proxy de aplicações:

* [Ativar o início de sessão único](application-proxy-configure-single-sign-on-with-kcd.md)
* [Publicar aplicações com o seu próprio nome de domínio](application-proxy-configure-custom-domain.md)
* [Saiba mais sobre os conectores de Proxy de aplicações do Azure AD](application-proxy-connectors.md)
* [Trabalhar com os servidores de Proxy existentes no local](application-proxy-configure-connectors-with-proxy-servers.md) 
* [Definir uma página inicial personalizada](application-proxy-configure-custom-home-page.md)

Para obter as notícias e atualizações mais recentes, consulte o [blogue do Proxy da Aplicação](http://blogs.technet.com/b/applicationproxyblog/)

