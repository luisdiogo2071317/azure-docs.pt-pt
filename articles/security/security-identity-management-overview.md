---
title: Funcionalidades de segurança do Azure que o ajudam a gestão de identidade | Microsoft Docs
description: " Este artigo fornece uma descrição geral das principais funcionalidades de segurança do Azure que ajudam na gestão de identidades. Microsoft identidades e acessos soluções ajuda da gestão de IT proteger o acesso a aplicações e recursos em todo o datacenter empresarial e para a nuvem, permitindo níveis adicionais de validação, como o multi-factor Authentication e o acesso condicional políticas. "
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 5aa0a7ac-8f18-4ede-92a1-ae0dfe585e28
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: terrylan
ms.openlocfilehash: 8763f1dca110a43586619c09f5d25c340c177b09
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35300662"
---
# <a name="azure-identity-management-security-overview"></a>Descrição geral de segurança de gestão de identidade do Azure
Microsoft identidades e acessos soluções ajuda da gestão de IT proteger o acesso a aplicações e recursos em todo o datacenter empresarial e para a nuvem. Essa proteção permite níveis adicionais de validação, como o multi-factor Authentication e políticas de acesso condicional. Monitorização de atividade suspeita através de segurança avançadas de relatórios, auditoria e ajuda a mitigar potenciais problemas de segurança de alerta. [Azure Active Directory Premium](../active-directory/active-directory-editions.md) fornece início de sessão único (SSO) para milhares de software na nuvem como uma aplicações de serviço (SaaS) e acesso a aplicações web que é executado no local.

Ao tirar partido dos benefícios de segurança do Azure Active Directory (Azure AD), pode:

* Criar e gerir uma identidade única para cada utilizador em toda a empresa híbrida, mantendo os utilizadores, grupos e dispositivos sincronizadas.
* Fornece acesso SSO para as suas aplicações, incluindo milhares de aplicações de SaaS previamente integradas.
* Ative a segurança de acesso da aplicação através da imposição baseada em regras multi-factor Authentication no local e as aplicações em nuvem.
* Aprovisionar o acesso remoto seguro a aplicações web no local, através do Proxy de aplicações do Azure AD.

O objetivo deste artigo é fornecer uma descrição geral das principais funcionalidades de segurança do Azure que ajudam na gestão de identidades. Podemos também fornecem ligações para artigos que fornecer detalhes de cada funcionalidade, pelo que pode saber mais.  

O artigo foca-se nas capacidades de gestão de identidade do Azure de núcleos seguintes:

* Início de sessão único
* Proxy inverso
* Multi-Factor Authentication
* Monitorização de segurança, alertas e relatórios com base na aprendizagem do computador
* Gestão de acesso e identidade do consumidor
* Registo de dispositivo
* Gestão de identidades privilegiadas
* Proteção de identidade
* Gestão de identidade híbrida

## <a name="single-sign-on"></a>Início de sessão único
SSO significa que está a ser capaz de aceder a todas as aplicações e recursos que precisa para fins comerciais, ao iniciar sessão apenas depois de utilizar uma conta de utilizador único. Depois de iniciar sessão, pode aceder a todas as aplicações que precisar, sem ser necessário para autenticação (por exemplo, escreva uma palavra-passe) uma segunda vez.

Muitas organizações confiarem em aplicações de SaaS, como o Office 365, a caixa e a Salesforce para a produtividade do utilizador. Historicamente, equipa de TI necessário individualmente criar e atualizar as contas de utilizador em cada aplicação SaaS e os utilizadores tinham de memorizar uma palavra-passe para cada aplicação SaaS.

Azure AD expande ambientes do Active Directory no local para a nuvem, permitindo que os utilizadores utilizar a respetiva conta organizacional primária para iniciar sessão, não apenas para os respetivos dispositivos associados a um domínio e recursos da empresa, mas também a todos os web e aplicações SaaS que precisam para as respetivas tarefas.

Não apenas os utilizadores não é necessário gerir vários conjuntos de nomes de utilizador e palavras-passe, pode aprovisionar ou anular o aprovisionamento acesso à aplicação automaticamente, com base nos respetivos grupos organizacionais e o respetivo estado de empregado. Azure AD introduz segurança e controlos de governação de acesso com a qual centralmente pode gerir o acesso dos utilizadores em aplicações SaaS.

Saiba mais:

* [Descrição geral sobre o início de sessão único](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](../active-directory/manage-apps/what-is-single-sign-on.md)
* [Integrar aplicações SaaS do Azure Active Directory-início de sessão único](../active-directory/manage-apps/configure-single-sign-on-portal.md)

## <a name="reverse-proxy"></a>Proxy inverso
Proxy de aplicações do Azure AD permite-lhe publicar aplicações no local, tais como [SharePoint](https://support.office.com/article/What-is-SharePoint-97b915e6-651b-43b2-827d-fb25777f446f?ui=en-US&rs=en-US&ad=US) sites, [Outlook Web App](https://technet.microsoft.com/library/jj657718.aspx), e [IIS](http://www.iis.net/)-com base em aplicações dentro da sua rede privada e proporciona acesso seguro aos utilizadores fora da rede. Proxy de aplicações fornece acesso remoto e as aplicações com milhares de aplicações SaaS que suporte do Azure AD de web de SSO para muitos tipos de no local. Os funcionários podem iniciar sessão às suas aplicações de casa nos seus próprios dispositivos e autenticar através deste proxy baseado na nuvem.

Saiba mais:

* [Ativar o Proxy de aplicações do Azure AD](../active-directory/manage-apps/application-proxy-enable.md)
* [Publicar aplicações através do Proxy de aplicações do Azure AD](../active-directory/active-directory-application-proxy-publish.md)
* [Início de sessão único com o Proxy da aplicação](../active-directory/manage-apps/application-proxy-configure-single-sign-on-with-kcd.md)
* [Trabalhar com acesso condicional](../active-directory/manage-apps/application-proxy-integrate-with-sharepoint-server.md)

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication
Multi-factor Authentication do Azure é um método de autenticação que requer a utilização de mais do que um método de verificação e adiciona uma segunda camada crítica de segurança aos inícios de sessão de utilizador e de transações. Multi-factor Authentication ajuda a salvaguardar o acesso a dados e aplicações, cumprindo o pedido do utilizador para um processo de início de sessão simple. Fornece autenticação forte através de uma gama de opções de verificação: chamadas telefónicas, as mensagens de texto ou notificações da aplicação móvel ou códigos de verificação e tokens de OAuth de terceiros.

Saiba mais:

* [Multi-Factor Authentication](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
* [O que é o Multi-Factor Authentication do Azure?](../active-directory/authentication/multi-factor-authentication.md)
* [Como funciona o Azure multi-factor Authentication](../active-directory/authentication/concept-mfa-howitworks.md)

## <a name="security-monitoring-alerts-and-machine-learning-based-reports"></a>Monitorização de segurança, alertas e relatórios com base na aprendizagem do computador
Monitorização de segurança, alertas e relatórios com base na aprendizagem de máquina identificam padrões de acesso inconsistentes podem ajudar a proteger a sua empresa. Pode utilizar o acesso do Azure AD e relatórios de utilização para ganhar visibilidade a integridade e a segurança do diretório da sua organização. Com esta informação, um administrador da directory melhor determinar onde poderá ficar os possíveis riscos de segurança para que estes podem planear adequadamente a mitigar os riscos.

No portal do Azure, os relatórios enquadram-se nas seguintes categorias:

* **Relatórios de anomalias**: contém eventos de início de sessão que são identificadas como estando anómalas. O nosso objetivo é fazer com que conhecimento essa atividade e permitem-lhe determinar se um evento está suspeito.
* **Integrado relatórios de aplicação**: fornecer informações sobre a forma como as aplicações em nuvem estão a ser utilizadas na sua organização. Azure AD oferece a integração com milhares de aplicações em nuvem.
* **Relatórios de erros**: indicar erros que possam ocorrer quando aprovisionar contas em aplicações externas.
* **Relatórios de utilizadores específicos**: apresentar dados de atividade de início de sessão de dispositivo para um utilizador específico.
* **Registos de atividade**: conter um registo de eventos auditados todas as últimas 24 horas, últimos 7 dias, ou últimos 30 dias e alterações ao grupo de atividade e atividade de registo e reposição de palavra-passe.

Saiba mais:

* [Ver os relatórios de acesso e utilização](../active-directory/active-directory-view-access-usage-reports.md)
* [Introdução aos relatórios do Azure Active Directory](../active-directory/active-directory-reporting-getting-started.md)
* [Guia de relatórios do Azure Active Directory](../active-directory/active-directory-reporting-guide.md)

## <a name="consumer-identity-and-access-management"></a>Gestão de acesso e identidade do consumidor
O Azure AD B2C é um serviço de gestão de identidade de elevada disponibilidade, global, para aplicações direcionadas para o consumidor preparada para centenas de milhões de identidades. Pode ser integrado entre plataformas móveis e Web. Os consumidores podem iniciar sessão todas as suas aplicações através de experiências personalizáveis através das respetivas contas de redes sociais existentes ou criação de novas credenciais.

No passado, os programadores de aplicações que pretendiam inscrever-se os clientes e iniciar sessão-los para as aplicações teriam de escrever o seu próprio código. E teriam de utilizar sistemas ou bases de dados no local para armazenar os nomes de utilizador e as palavras-passe. O Azure AD B2C oferece a sua organização uma melhor forma de integrar a gestão de identidades de consumidor nas aplicações com a ajuda de uma plataforma segura, baseada em normas e um grande conjunto de políticas extensíveis.

Quando utiliza o Azure AD B2C, os consumidores podem inscrever-se para as suas aplicações através das respetivas contas de redes sociais existentes (Facebook, Google, Amazon, LinkedIn) ou através da criação de novas credenciais (endereço de correio eletrónico e palavra-passe, ou nome de utilizador e palavra-passe).

Saiba mais:

* [O que é o Azure Active Directory B2C?](https://azure.microsoft.com/services/active-directory-b2c/)
* [Pré-visualização do Azure Active Directory B2C: inscrever-se e iniciar sessão em consumidores nas suas aplicações](../active-directory-b2c/active-directory-b2c-overview.md)
* [Pré-visualização do Azure Active Directory B2C: Tipos de aplicações](../active-directory-b2c/active-directory-b2c-apps.md)

## <a name="device-registration"></a>Registo de dispositivo
Registo de dispositivos do Azure AD é o alicerce da baseado nos dispositivos [acesso condicional](../active-directory/active-directory-conditional-access-device-registration-overview.md) cenários. Quando um dispositivo é registado, o registo de dispositivos do Azure AD fornece o dispositivo com uma identidade que é utilizado para autenticar o dispositivo quando um utilizador inicia sessão. O dispositivo autenticado e os atributos do dispositivo, em seguida, podem ser utilizados para impor políticas de acesso condicional para aplicações que estão alojadas na nuvem e no local.

Quando combinada com uma solução de gestão de dispositivos móveis, como o Intune, os atributos do dispositivo no Azure AD são atualizados com informações adicionais sobre o dispositivo. Em seguida, pode criar regras de acesso condicional que impõem o acesso a partir de dispositivos para cumprir as normas de segurança e conformidade.

Saiba mais:

* [Introdução ao registo de dispositivos do Azure AD](../active-directory/active-directory-conditional-access-device-registration-overview.md)
* [Registo automático de dispositivos com o Azure AD para dispositivos associados a um domínio do Windows](../active-directory/active-directory-conditional-access-automatic-device-registration.md)
* [Configurar o registo automático do Windows dispositivos associados a um domínio com o Azure AD](../active-directory/active-directory-conditional-access-automatic-device-registration-setup.md)

## <a name="privileged-identity-management"></a>Gestão de identidades privilegiadas
Com o Azure AD Privileged Identity Management, pode gerir, controlar e monitorizar as identidades privilegiadas e aceder a recursos no Azure AD, bem como outros serviços online da Microsoft, tais como o Office 365 e o Microsoft Intune.

Por vezes, os utilizadores necessitam de realizar operações privilegiadas de recursos do Azure ou do Office 365 ou outras aplicações SaaS. Esta necessidade, muitas vezes, significa que as organizações têm de conceder acesso privilegiado permanente de utilizadores no Azure AD. Esse acesso é um risco de segurança crescente para recursos alojados na nuvem, porque as organizações suficientemente não é possível monitorizar a que os utilizadores estão a fazer com os respetivos privilégios de administrador. Além disso, se uma conta de utilizador com acesso privilegiado for comprometida, que uma violação pode afetar a segurança de nuvem global da organização. O Azure AD Privileged Identity Management ajuda a mitigar este risco.

Com o Azure AD Privileged Identity Management, pode:

* Veja os utilizadores que são administradores do Azure AD.
* Ative a pedido, just-in-time (JIT) acesso administrativo serviços Microsoft como o Office 365 e o Intune.
* Obter relatórios sobre o histórico de acesso de administrador e as alterações em atribuições de administrador.
* Obtenha alertas sobre o acesso a uma função com privilégios.

Saiba mais:

* [Azure AD Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-configure.md)
* [Funções no Azure AD Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-roles.md)
* [Azure AD Privileged Identity Management: Como adicionar ou remover uma função de utilizador](../active-directory/active-directory-privileged-identity-management-how-to-add-role-to-user.md)

## <a name="identity-protection"></a>Proteção de identidade
Azure AD Identity Protection é um serviço de segurança que fornece uma vista consolidada sobre eventos de risco e potenciais vulnerabilidades que afetam as identidades da organização. Proteção de identidade tira partido das capacidades de deteção de anomalias do Azure AD existentes, que estão disponíveis através de relatórios de atividade anómala do Azure AD. Proteção de identidade também apresenta novos tipos de eventos de risco que consegue de detetar anomalias em tempo real.

Saiba mais:

* [Azure AD Identity Protection](../active-directory/active-directory-identityprotection.md)
* [Canal 9: Do Azure AD e mostrar de identidade: identidade pré-visualização de proteção](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="hybrid-identity-management"></a>Gestão de identidade híbrida
Abrange a abordagem da Microsoft à identidade no local e na nuvem, criar uma identidade de utilizador único para autenticação e autorização para todos os recursos, independentemente da localização.

Saiba mais:

* [Documento de técnico de identidade híbrida](http://download.microsoft.com/download/D/B/A/DBA9E313-B833-48EE-998A-240AA799A8AB/Hybrid_Identity_White_Paper.pdf)
* [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/)
* [Blogue de equipa do Azure AD](https://blogs.technet.microsoft.com/ad/)
