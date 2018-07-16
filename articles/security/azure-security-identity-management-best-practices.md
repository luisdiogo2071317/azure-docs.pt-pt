---
title: Identidade e acesso segurança melhores práticas do Azure | Documentos da Microsoft
description: Este artigo fornece um conjunto de melhores práticas para gestão de identidades e recursos do Azure através do controlo de acesso internos.
services: security
documentationcenter: na
author: barclayn
manager: mbaldwin
editor: TomSh
ms.assetid: 07d8e8a8-47e8-447c-9c06-3a88d2713bc1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/26/2018
ms.author: barclayn
ms.openlocfilehash: 210781b36f6215afc925266e597031d772a94002
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39060559"
---
# <a name="azure-identity-management-and-access-control-security-best-practices"></a>Melhores práticas de segurança de controlo de acesso e gestão de identidades do Azure

Muitos consideram a identidade para a nova camada de limites para segurança, assumindo essa função do ponto de vista centrado em rede tradicional. Esta evolução do eixo principal para a atenção de segurança e os investimentos vêm do fato de que se tornaram cada vez mais porosos perímetros de rede e essa defesa do perímetro não pode ser tão eficaz quanto eles vez foram antes da explosão de [BYOD ](http://aka.ms/byodcg) dispositivos e aplicações na cloud.

Neste artigo, discutimos a uma coleção de gestão de identidades do Azure e práticas recomendadas de segurança do controlo de acesso. Essas práticas recomendadas são derivadas da nossa experiência com [do Azure AD](../active-directory/fundamentals/active-directory-whatis.md) e as experiências dos clientes, como mesmo.

Para cada melhor prática, Explicamos:

* O que é a melhor prática
* Por que pretende ativar essa prática recomendada
* Se efetuar a ativação para ativar a melhor prática, o que pode ser o resultado
* Possíveis alternativas para a prática recomendada
* Como pode aprender permitir a melhor prática

Esta gestão de identidades do Azure e a segurança de controlo de acesso ao artigo de melhores práticas se baseia numa opinião de consenso e capacidades da plataforma Azure e conjuntos de recursos, conforme existem no momento, este artigo foi escrito. Opiniões e as tecnologias mudam ao longo do tempo e este artigo será atualizado regularmente para refletir essas alterações.

Identidade do Azure gestão e acesso controlo melhores práticas de segurança discutidas neste artigo incluem:

* Centralizar seu gerenciamento de identidade
* Ativar o início de sessão único (SSO)
* Implementar a gestão de palavra-passe
* Impor autenticação multifator (MFA) para os utilizadores
* Utilizar o controlo de acesso baseado em funções (RBAC)
* Controlar as localizações onde os recursos são criados com o Resource Manager
* Desenvolvedores de guia para tirar partido das capacidades de identidade para aplicações SaaS
* Monitorizar ativamente para atividades suspeitas

## <a name="centralize-your-identity-management"></a>Centralizar seu gerenciamento de identidade

Uma etapa importante para proteger a sua identidade é garantir que o departamento de TI pode gerir contas de uma localização única sobre em que esta conta foi criada. Embora a maioria das empresas, organizações de TI tem a sua conta principal diretório no local, implementações na cloud híbrida em alta e é importante que entenda como integrar no local e na cloud diretórios e fornecer um totalmente integrada experiência ao usuário final.

Para tal [identidade híbrida](../active-directory/active-directory-hybrid-identity-design-considerations-overview.md) cenário, recomendamos que duas opções:

* Sincronizar o seu diretório no local com o diretório na cloud com o Azure AD Connect
* Ativar o início de sessão único com [sincronização de hash de palavra-passe](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization), [autenticação pass-through](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-faq) ou federar sua identidade no local com o seu diretório de cloud com [do Active Directory Os serviços de Federação](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/deploying-federation-servers) (AD FS)

As organizações que não obedeçam a integrar a sua identidade no local com a respetiva identidade de nuvem experiência maior sobrecarga administrativa no gerenciamento de contas, que aumenta a probabilidade de erros e falhas de segurança.

Para obter mais informações sobre a sincronização do Azure AD, consulte o artigo [integrar as identidades no local com o Azure Active Directory](../active-directory/active-directory-aadconnect.md).

## <a name="enable-single-sign-on-sso"></a>Ativar o início de sessão único (SSO)

Quando tiver vários diretórios gerir, isso se torna um problema administrativo não só para IT, mas também para os usuários finais que se lembrar de várias palavras-passe. Usando [SSO](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/) é fornecer os seus utilizadores a capacidade de utilizar o mesmo conjunto de credenciais para iniciar sessão e aceder aos recursos que eles precisam, seja como for em que este recurso é localizada no local ou na cloud.

Utilizar o SSO para permitir que os utilizadores aceder aos respetivos [aplicações SaaS](../active-directory/manage-apps/what-is-single-sign-on.md) com base nas suas contas institucionais no Azure AD. Isto é aplicável não só para aplicações SaaS da Microsoft, mas também outras aplicações, tal como [Google Apps](../active-directory/saas-apps/google-apps-tutorial.md) e [Salesforce](../active-directory/saas-apps/salesforce-tutorial.md). Seu aplicativo pode ser configurado para utilizar o Azure AD como um [identidade baseada no SAML](../active-directory/fundamentals-identity.md) fornecedor. Como um controle de segurança do Azure AD não irá emitir um token, permitindo que iniciem sessão na aplicação, a menos que tenham sido concedidos acesso com o Azure AD. Pode conceder acesso diretamente ou através de um grupo que são membros de.

> [!NOTE]
> a decisão de usar o SSO terá impacto sobre como integrar o seu diretório no local com o diretório na cloud. Se pretender que o SSO, terá de usar a Federação, porque a sincronização de diretórios só fornecerá [mesma experiência de início de sessão](../active-directory/active-directory-aadconnect.md).
>
>

As organizações que não é impor o SSO para seus usuários e aplicativos são mais expostas a cenários em que os utilizadores têm várias palavras-passe, que diretamente, aumenta a probabilidade dos utilizadores a reutilização de palavras-passe ou utilizar palavras-passe fracas.

Pode saber mais sobre o SSO do Azure AD ao ler o artigo [personalização com o Azure AD Connect e de gestão do AD FS](../active-directory/active-directory-aadconnect-federation-management.md).

## <a name="deploy-password-management"></a>Implementar a gestão de palavra-passe

Em cenários em que tiver vários inquilinos ou se pretender permitir que os utilizadores [repor a sua própria palavra-passe](../active-directory/user-help/active-directory-passwords-update-your-own-password.md), é importante que use as políticas de segurança apropriado para evitar abusos. No Azure, pode aproveitar a capacidade de reposição de palavra-passe self-service e personalizar as opções de segurança para cumprir os requisitos comerciais.

É importante obter feedback destes utilizadores e Aprenda com suas experiências à medida que tentam executar estes passos. Com base nestas experiências, elaborar um plano para mitigar potenciais problemas que possam ocorrer durante a implementação para um grupo maior. Também é recomendável que utilize o [relatório de atividade de registo de reposição de palavra-passe](../active-directory/active-directory-passwords-get-insights.md) para monitorizar os utilizadores que estão a registar.

As organizações que desejam evitar chamadas de suporte de alteração de palavra-passe, mas permitir que os utilizadores reponham a palavra-passe são mais suscetíveis a um maior volume de chamadas para o serviço de atendimento devido a problemas de palavra-passe. Em organizações que tenham vários inquilinos, é imperativo que implementar este tipo de recurso e permitir que os utilizadores executar dentro dos limites de segurança que foram estabelecidos na política de segurança de reposição de palavra-passe.

Pode saber mais sobre a palavra-passe, repor lendo o artigo [implementar gestão de palavras-passe e formar os utilizadores para utilizá-lo](../active-directory/authentication/howto-sspr-deployment.md).

## <a name="enforce-multi-factor-authentication-mfa-for-users"></a>Impor autenticação multifator (MFA) para os utilizadores

Para organizações que têm de estar em conformidade com padrões da indústria, como [PCI DSS versão 3.2](http://blog.pcisecuritystandards.org/preparing-for-pci-dss-32), autenticação multifator é essencial ter capacidade para autenticar os utilizadores. Para além de estar em conformidade com normas da indústria, impor a MFA para autenticar os utilizadores também pode ajudar as organizações para mitigar o tipo de roubo de credencial de ataque, como [Pass-the-Hash (PtH)](http://aka.ms/PtHPaper).

Ao ativar a MFA do Azure para os seus utilizadores, que está a adicionar uma segunda camada de segurança para inícios de sessão de utilizador e transações. Neste caso, poderá aceder um documento, localizado num servidor de ficheiros ou em seu SharePoint Online a uma transação. Também ajuda a MFA do Azure que a TI reduzir a probabilidade de que uma credencial comprometida tem acesso aos dados da organização.

Por exemplo: impor o MFA do Azure para os seus utilizadores e configurá-lo para utilizar uma chamada telefónica ou mensagem de texto como verificação. Se as credenciais do usuário forem comprometidas, o invasor não é capaz de aceder a qualquer recurso uma vez que não têm acesso ao telefone do utilizador. As organizações que não adicione camadas adicionais de proteção de identidade são mais suscetíveis de ataque de roubo de credenciais, que pode levar ao comprometimento de dados.

Uma alternativa para as organizações que desejam manter o autenticação completa controle no local é usar [servidor do Azure multi-factor Authentication](../active-directory/authentication/howto-mfaserver-deploy.md), também chamado de MFA no local. Ao utilizar este método, continuará a ser capaz de impor autenticação multifator, mantendo o MFA server no local.

Para obter mais informações sobre o MFA do Azure, consulte o artigo [introdução ao multi-factor Authentication na cloud](../active-directory/authentication/howto-mfa-getstarted.md).

## <a name="use-role-based-access-control-rbac"></a>Utilizar o controlo de acesso baseado em funções (RBAC)

Restringir o acesso com base na [precisa de saber](https://en.wikipedia.org/wiki/Need_to_know) e [menor privilégio](https://en.wikipedia.org/wiki/Principle_of_least_privilege) princípios de segurança é fundamental para as organizações que desejam aplicar políticas de segurança para acesso a dados. Do Azure com base em função de controlo de acesso (RBAC) pode ser utilizado para atribuir permissões a utilizadores, grupos e aplicações num determinado âmbito. O âmbito de uma atribuição de função pode ser uma subscrição, um grupo de recursos ou um único recurso.

Pode tirar partido [incorporada RBAC](../role-based-access-control/built-in-roles.md) funções no Azure para atribuir privilégios a utilizadores. Considere a utilização *contribuinte de conta de armazenamento* para os operadores de cloud que necessitam de gerir contas de armazenamento e *contribuinte de conta de armazenamento clássico* função para gerir contas de armazenamento clássicas. Para os operadores de cloud que precisa para gerir VMs e a conta de armazenamento, considere adicionar-lhes *contribuinte de Máquina Virtual* função.

As organizações que não é impor o controlo de acesso de dados ao tirar partido das capacidades como o RBAC podem ser a dar mais privilégios do que o necessário para seus usuários. Isso pode levar a dados comprometimento por permitir aos utilizadores acesso a determinados tipos de dados (por exemplo, elevado impacto comercial) que eles não devem ter em primeiro lugar.

Pode saber mais sobre o RBAC do Azure, leia o artigo [controlo de acesso](../role-based-access-control/role-assignments-portal.md).

## <a name="control-locations-where-resources-are-created-using-resource-manager"></a>Controlar as localizações onde os recursos são criados com o Resource Manager

É importante habilitar operadores da nuvem executar tarefas, enquanto impede-los de danificar as convenções que são necessários para gerir os recursos da sua organização. As organizações que desejam controlar os locais onde os recursos são criados devem muito código estas localizações.

Para conseguir isso, as organizações podem criar políticas de segurança que têm definições que descrevem as ações ou os recursos que serão negados. Atribuir as definições de política no âmbito pretendido, como a subscrição, grupo de recursos ou recurso individual.

> [!NOTE]
> Não é o mesmo que o RBAC, ela aproveita, na verdade, o RBAC para autenticar os utilizadores que têm privilégios para criar esses recursos.
>
>

Tire partido [do Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) para criar políticas personalizadas também para cenários em que a empresa pretende permitir operações apenas quando o Centro de custos apropriado está associado; caso contrário, eles negar o pedido.

As organizações que não a controlar como os recursos são criados são mais suscetíveis a utilizadores que podem abusar o serviço através da criação de mais recursos do que o necessário. O processo de criação de recursos de sistema de proteção é um passo importante para proteger um cenário de multi-inquilino.

Pode saber mais sobre como criar políticas com o Azure Resource Manager lendo o artigo [o que é o Azure Policy?](../azure-policy/azure-policy-introduction.md)

## <a name="guide-developers-to-leverage-identity-capabilities-for-saas-apps"></a>Desenvolvedores de guia para tirar partido das capacidades de identidade para aplicações SaaS

Identidade do utilizador é utilizada em muitos cenários, quando os utilizadores acedem [aplicações SaaS](https://azure.microsoft.com/marketplace/active-directory/all/) que pode ser integrado no local ou na cloud directory. Primeiro e mais importante, recomendamos que os desenvolvedores usem uma metodologia de segura para desenvolver estas aplicações, tal como [Microsoft Security Development Lifecycle (SDL)](https://www.microsoft.com/sdl/default.aspx). O Azure AD simplifica a autenticação para os desenvolvedores ao fornecer identidade como um serviço, com suportam para protocolos de norma da indústria como [OAuth 2.0](http://oauth.net/2/) e [OpenID Connect](http://openid.net/connect/), bem como código-fonte aberto bibliotecas de diferentes plataformas.

Certifique-se para se registar qualquer aplicativo que terceiriza a autenticação para o Azure AD, este é um procedimento obrigatório. O motivo por trás disso é que o Azure AD precisa coordenar a comunicação com a aplicação quando o processamento de início de sessão (SSO) ou a troca de tokens. A sessão do utilizador expira quando expira o tempo de vida do token emitido pelo Azure AD. Avalie sempre se a sua aplicação deve utilizar este tempo ou se é possível reduzir esse tempo. Reduzir o tempo de vida pode agir como uma medida de segurança que irá forçar os usuários a fim de sessão com base num período de inatividade.

As organizações que não impor o controlo de identidade para aceder às aplicações e não a guia dos programadores sobre como integrar com segurança as aplicações com o seu sistema de gestão de identidade podem ser mais suscetíveis a credenciais, tais como o tipo de roubo de ataque, [fraca autenticação e uma sessão de gestão, descrito na parte superior de Open Web Application Security Project (OWASP) 10](https://www.owasp.org/index.php/OWASP_Top_Ten_Cheat_Sheet).

Pode saber mais sobre cenários de autenticação para aplicações SaaS, lendo [cenários de autenticação do Azure AD](../active-directory/active-directory-authentication-scenarios.md).

## <a name="actively-monitor-for-suspicious-activities"></a>Monitorizar ativamente para atividades suspeitas

Em conformidade com [relatório de violação de dados de 2016 da Verizon](http://www.verizonenterprise.com/verizon-insights-lab/dbir/2016/), credenciais comprometidas ainda está em crescimento e a tornar-se um dos negócios mais rentáveis para os criminosos cibernéticos. Por esse motivo, é importante ter um sistema de monitor de identidade do Active Directory no local que possa rapidamente detetar atividade suspeita de comportamento e acionar um alerta para uma investigação mais aprofundada. O Azure AD tem dois recursos principais que podem ajudar as organizações a monitorizar as suas identidades: Azure AD Premium [relatórios de anomalias](../active-directory/active-directory-view-access-usage-reports.md) e o Azure AD [proteção de identidade](../active-directory/active-directory-identityprotection.md) capacidade.

Certifique-se utilizar os relatórios de anomalias para identificar tentativas para iniciar sessão [sem estão a ser rastreados](../active-directory/active-directory-reporting-sign-ins-from-unknown-sources.md), [força bruta](../active-directory/active-directory-reporting-sign-ins-after-multiple-failures.md) ataques contra uma determinada conta, tenta iniciar sessão a partir de várias localizações, iniciar sessão a partir [ dispositivos infetados e suspeita endereços IP. Tenha em atenção que estes são relatórios. Em outras palavras, tem de ter processos e procedimentos corretos para os administradores de TI executar estes relatórios, a base diária ou a pedido (normalmente num cenário de resposta a incidentes).

Por outro lado, do Azure AD identity protection é um sistema de monitoramento ativo e ele sinaliza os riscos atuais no seu próprio dashboard. Além disso, também receberá notificações de resumidas diário por e-mail. Recomendamos que ajustar o nível de risco, de acordo com seus requisitos empresariais. O nível de risco para um evento de risco é uma indicação (elevada, média ou baixa) da gravidade do evento de risco. O nível de risco ajuda os utilizadores de proteção de identidade priorizar ações têm de realizar para reduzir o risco para a sua organização.

As organizações que não monitorizar ativamente seus sistemas de identidade estão em risco de ter credenciais de utilizador comprometidas. Sem o conhecimento que estão a atividades suspeitas colocar com estas credenciais, as organizações não serão possível mitigar este tipo de ameaça.
Pode saber mais sobre a proteção de identidade do Azure, leia [do Azure Active Directory Identity Protection](../active-directory/active-directory-identityprotection.md).
