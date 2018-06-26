---
title: Melhores práticas para proteger o acesso administrativo no Azure AD | Microsoft Docs
description: Certifique-se de que a acesso e admin as contas administrativas sua organização são seguras. Arquitetos de sistema e profissionais de TI que configurar o Azure AD, do Azure e do Microsoft Online Services.
services: active-directory
keywords: ''
author: curtand
manager: mtillman
ms.author: curtand
ms.date: 06/25/2018
ms.topic: article
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.custom: it-pro
ms.reviewer: martincoetzer, MarkMorow
ms.openlocfilehash: 729cb89fbe63dbecb65c6f948052b920bf6e70fe
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36937457"
---
# <a name="securing-privileged-access-for-hybrid-and-cloud-deployments-in-azure-ad"></a>Proteger o acesso privilegiado para implementações híbridas e em nuvem no Azure AD

A segurança da maioria ou todos os recursos empresariais na organização moderna depende a integridade das contas com privilégios que administre e gira os sistemas de TI. Atores maliciosos, incluindo informático atacantes frequentemente de destino de contas de administrador e outros elementos de acesso privilegiado ao tentar obter rapidamente o acesso a dados confidenciais e sistemas de ataques de roubo de credenciais a utilizar. Para a nuvem serviços, prevenção e resposta são conjunta responsabilidades do fornecedor de serviço em nuvem e o cliente. Para obter mais informações sobre as ameaças mais recentes para os pontos finais e a nuvem, consulte o [Microsoft segurança Intelligence relatório](https://www.microsoft.com/security/sir/default.aspx). Este artigo pode ajudar a desenvolver um plano para fechar os intervalos entre os esquemas de atuais e a documentação de orientação descrito aqui.

> [!NOTE] 
> A Microsoft está empenhada em mais altos níveis de fidedignidade, transparência, a conformidade de padrões e a conformidade regulamentar. Obter mais informações sobre como a equipa de resposta a incidentes global da Microsoft atenua os efeitos de ataques contra serviços em nuvem e, como a segurança é criada para produtos de empresas da Microsoft e serviços em nuvem em [Microsoft Trust Center - segurança](https://www.microsoft.com/en-us/trustcenter/security)e destinos de compatibilidade da Microsoft em [Microsoft Trust Center - conformidade](https://www.microsoft.com/en-us/trustcenter/compliance).

<!--## Risk management, incident response, and recovery preparation

A cyber-attack, if successful, can shut down operations not just for a few hours, but in some cases for days or even weeks. The collateral damage, such as legal ramifications, information leaks, and media coverage, could potentially continue for years. To ensure effective company-wide risk containment, cybersecurity and IT pros must align their response and recovery processes. To reduce the risk of business disruption due to a cyber-attack, industry experts recommend you do the following:

* As part of your risk management operations, establish a crisis management team for your organization that is responsible for managing all types of business disruptions.

* Compare your current risk mitigations, incident response, and recovery plan with industry best practices for managing a business disruption before, during, and after a cyber-attack.

* Develop and implement a roadmap for closing the gaps between your current plans and the best practices described in this document.


## Securing privileged access for hybrid and cloud deployments

does the article really start here?-->
Na maioria das organizações, a segurança de recursos de empresa depende a integridade das contas com privilégios que administre e gira os sistemas de TI. Os atacantes de informático focar-se no acesso privilegiado para sistemas de infraestrutura (tais como o Active Directory e o Azure Active Directory) para obter acesso a dados confidenciais da organização. 

Abordagens tradicionais que focar-se sobre como proteger os pontos de entrada como e saída de uma rede de perímetro principal de segurança são menos eficientes devido ao aumento súbito da utilização de aplicações SaaS e dispositivos pessoais na Internet. A substituição para o perímetro de segurança de rede numa empresa moderna complexa natural é os controlos de autenticação e autorização na camada de identidade da organização. 

Contas com privilégios de administradores são efetivamente no controlo deste novo "segurança perímetro." É fundamental para proteger o acesso privilegiado, independentemente se o ambiente está no local, cloud ou híbrida no local e na nuvem serviços alojados. Proteger o acesso administrativo contra adversários qualificados determinados requer que adotar uma abordagem de conclua e thoughtful para isolar os sistemas da sua organização contra riscos. 

Proteger o acesso privilegiado requer alterações ao
* Gestão de dados de conhecimento, práticas administrativas e processos
* Componentes técnicos como defesas de anfitrião, as proteções de conta e gestão de identidades

Este documento centra-se principalmente na criação de um plano para proteger as identidades e acesso de que são geridos ou comunicado no Azure AD, Microsoft Azure, serviços do Office 365 e outros na nuvem. Nas organizações que tenham no local contas administrativas, consulte a documentação de orientação no local e híbridos privilegiado acesso gerido a partir do Active Directory em [proteger o acesso privilegiado](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access). 

> [!NOTE] 
> As orientações neste artigo refere-se principalmente a funcionalidades do Azure Active Directory que estão incluídas nos planos de Azure Active Directory Premium P1 e P2. Do Azure Active Directory Premium P2 está incluída no EMS E5 suite e Microsoft 365 E5 suite. Esta orientação assume que a sua organização já tem licenças do Azure AD Premium P2 adquiridas para os seus utilizadores. Se não tiver estas licenças, alguns das orientações não podem ser aplicadas à sua organização. Além disso, ao longo deste artigo, o administrador global do prazo (ou o administrador global) é synonymous com o "administrador da empresa" ou "administrador do inquilino".

## <a name="develop-a-roadmap"></a>Desenvolver um plano 

A Microsoft recomenda que desenvolver e seguir um plano para proteger o acesso privilegiado contra atacantes informático. Pode sempre ajustar o plano para acomodar as capacidades existentes e requisitos específicos da sua organização. Cada fase de plano deve aumentar o custo e a dificuldade em para adversários qualificados atacar o acesso privilegiado de ativos híbrida, na nuvem e no local. A Microsoft recomenda as seguintes quatro fases de plano: este recomenda agendas de plano mais eficaz e as implementações mais rápidas em primeiro lugar, com base nas experiências da Microsoft com a implementação de incidente e resposta de ataque informático. As linhas cronológicas para este plano são indicativos.

![Fases de plano com linhas de tempo](./media/admin-roles-best-practices/roadmap-timeline.png)

* Fase 1 (48 de 24 horas): itens críticos que recomendamos que efetue imediato

* Fase 2 (2 a 4 semanas): mitigar as técnicas de ataque mais frequentemente utilizadas

* Fase 3 (1 a 3 meses): Crie visibilidade e controlo total da atividade de administrador de compilação

* Fase 4 (seis meses e beyond): continuar a criação de defesas para proteger ainda mais a sua plataforma de segurança

Esta estrutura de plano foi concebida para maximizar a utilização de tecnologias da Microsoft que pode ter já tenha implementado. Também pode tirar partido das tecnologias de segurança atuais e futuras chave e integrar ferramentas de segurança de outros fornecedores, se já tiver implementado ou estiver a considerar implementar. 

## <a name="stage-1-critical-items-that-we-recommend-you-do-right-away"></a>Fase 1: Itens críticos que recomendamos que efetue imediato

![Fase 1](./media/admin-roles-best-practices/stage-one.png)

Fase 1 do plano concentra-se nas tarefas críticas rápida e fácil de implementar. Recomendamos que efetue estes itens alguns imediato nas horas 24 48 primeiro para garantir um nível básico de acesso privilegiado seguro. Nesta fase de plano protegida acesso privilegiado inclui as seguintes ações:

### <a name="general-preparation"></a>Preparação gerais

#### <a name="turn-on-azure-ad-privileged-identity-management"></a>Ativar o Azure AD Privileged Identity Management

Se já não ativou o Azure AD Privileged Identity Management (PIM), tal no seu inquilino de produção. Depois de ativar o Privileged Identity Management, irá receber a notificação de correio eletrónico mensagens para acesso privilegiado alterações de função. Estas notificações fornecem avisos antecipados quando são adicionados utilizadores adicionais para funções de altamente privilegiados no seu diretório.

O Azure AD Privileged Identity Management está incluído no Azure AD Premium P2 ou E5 do EMS. Estas soluções ajudam a proteger o acesso a recursos e aplicações através do ambiente no local e na nuvem. Se ainda não tiver P2 do Azure AD Premium ou EMS E5 e pretende avaliar mais as capacidades referenciadas neste plano, inscreva-se a [Enterprise Mobility + Security avaliação gratuita de 90 dias](https://www.microsoft.com/cloud-platform/enterprise-mobility-security-trial). Utilize estas avaliações de licença para experimentar o Azure AD Privileged Identity Management e o Azure AD Identity Protection, para monitorizar a atividade com o Azure AD avançadas de relatórios de segurança, auditoria e alertas.

Depois de ter ativou o Azure AD Privileged Identity Management:

1. Iniciar sessão para o [portal do Azure](https://portal.azure.com/) com uma conta que seja um administrador global do seu inquilino de produção.

2. Para selecionar o inquilino onde pretende utilizar Privileged Identity Management, selecione o seu nome de utilizador no canto superior direito do portal do Azure.

3. Selecione **todos os serviços** e filtrar a lista para **do Azure AD Privileged Identity Management**.

4. Abra o Privileged Identity Management do **todos os serviços** listar e afixá-lo ao dashboard.

A primeira pessoa a utilizar o Azure AD Privileged Identity Management no seu inquilino é atribuída automaticamente a **administrador de segurança** e **administrador com função privilegiada** funções no inquilino. Apenas os administradores de com função privilegiada podem gerir as atribuições de função de diretório do Azure AD dos utilizadores. Além disso, após a adição do Azure AD Privileged Identity Management, são apresentados o Assistente de segurança que explica-lhe a experiência de deteção e atribuição inicial. Pode sair do assistente sem efetuar alterações adicionais neste momento. 

#### <a name="identify-and-categorize-accounts-that-are-in-highly-privileged-roles"></a>Identificar e categorizar contas que são funções privilegiadas 

Depois de ativar o Azure AD Privileged Identity Management, ver os utilizadores que estão no Administrador Global do diretório funções, o administrador com função privilegiada, o administrador do Exchange Online e o administrador do SharePoint Online. Se não tiver o Azure AD PIM no seu inquilino, pode utilizar o [PowerShell API](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0). Começar a utilizar a função de administrador global, esta função é genérica: um utilizador a quem é atribuído esta função de administrador tem as mesmas permissões em todos os serviços de nuvem para o qual a sua organização tiver subscrito, independentemente se se tiver sido atribuídos esta função no portal do Office 365 , o Azure portal, ou utilizando o módulo do Azure AD do Microsoft PowerShell. 

Remova quaisquer contas que já não são necessárias nessas funções e categorizar as restantes contas que estão atribuídas a funções de administrador:

* Individualmente atribuídas a utilizadores administrativos e também podem ser utilizados para fins não administrativos (por exemplo, e-mail pessoal)
* Individualmente designada para fins administrativos só e atribuídos a utilizadores administrativos
* Partilhado entre vários utilizadores
* Para cenários de acesso de emergência break glass
* Para scripts automatizados
* Para os utilizadores externos

#### <a name="define-at-least-two-emergency-access-accounts"></a>Definir, pelo menos, duas contas de acesso de emergência 

Certifique-se de que não depara-se para uma situação em que foi ser inadvertidamente bloqueados a administração de inquilino do Azure AD devido a uma impossibilidade para iniciar sessão ou ativar um existente individuais da conta de utilizador como administrador. Por exemplo, se a organização estiver federada para um fornecedor de identidade no local, esse fornecedor de identidade não esteja disponível para que os utilizadores não podem iniciar sessão no local. Pode mitigar o impacto de acidental falta de acesso administrativo ao armazenar dois ou mais contas de acesso de emergência no seu inquilino.

Contas de acesso de emergência ajudam as organizações a restringir o acesso privilegiado num ambiente existente do Azure Active Directory. Estas contas são altamente privilegiadas e não estão atribuídas a indivíduos específicos. Contas de acesso de emergência estão limitadas a emergência para 'break glass' cenários onde não não possível utilizar as contas administrativas normais. As organizações tem de garantir o objetivo de controlar e reduzindo a utilização de emergência da conta para apenas esse tempo para o qual é necessário. 

Avalie as contas que são atribuídas ou elegíveis para a função de administrador global. Se não ver qualquer apenas na nuvem contas com o *. domínio onmicrosoft.com (destina-se "break glass" acesso de emergência), criá-los. Para obter mais informações, consulte [gerir as contas administrativas de acesso de emergência no Azure AD](active-directory-admin-manage-emergency-access-accounts.md).

#### <a name="turn-on-multi-factor-authentication-and-register-all-other-highly-privileged-single-user-non-federated-admin-accounts"></a>Ativar a autenticação multifator e registar todas as outras contas altamente privilegiados utilizador único não federada admin 

Precisa, Azure multi-factor Authentication (MFA) no início de sessão para todos os utilizadores individuais que são permanentemente atribuídos a uma ou mais funções de administrador do Azure AD: Administrador Global, o administrador com função privilegiada, o administrador do Exchange Online e o SharePoint Administrador online. Utilize o guia para ativar [multi-factor Authentication (MFA) para as contas de administrador](authentication/howto-mfa-userstates.md) e certifique-se de que todos os esses utilizadores se registou no [ https://aka.ms/mfasetup ](https://aka.ms/mfasetup). Podem encontrar mais informações em passo 2 e o passo 3 do guia [proteger o acesso a dados e serviços do Office 365](https://support.office.com/article/Protect-access-to-data-and-services-in-Office-365-a6ef28a4-2447-4b43-aae2-f5af6d53c68e). 

## <a name="stage-2-mitigate-the-most-frequently-used-attack-techniques"></a>Fase 2: Mitigar as técnicas de ataque mais frequentemente utilizadas

![Fase 2](./media/admin-roles-best-practices/stage-two.png)

Fase 2 do plano concentra-se no mitigar as técnicas utilizadas mais frequentemente de ataque de roubo de credenciais e abuso e foi concebido para ser implementado em aproximadamente 2 a 4 semanas. Nesta fase de plano protegida acesso privilegiado inclui as seguintes ações.

### <a name="general-preparation"></a>Preparação gerais

#### <a name="conduct-a-inventory-of-services-owners-and-admins"></a>Realize um inventário dos serviços, os proprietários e admins

Com o aumento no bring-your-own device (BYOD) e políticas de trabalho de casa e o crescimento da conectividade sem fios em empresas, é fundamental que monitorizar quem está a ligar à sua rede. Uma auditoria de segurança eficaz revela, muitas vezes, dispositivos, aplicações e programas em execução na sua rede que não são suportadas pelo departamento de TI e, por conseguinte, potencialmente não seguro. Para obter mais informações, consulte [gestão de segurança do Azure e a descrição geral da monitorização](../security/security-management-and-monitoring-overview.md). Certifique-se de que inclui todas as tarefas seguintes no seu processo de inventário. 

* Identifique os utilizadores que têm funções administrativas e os serviços de onde pode gerir.
* Utilize o Azure AD PIM para saber quais os utilizadores na sua organização tem acesso de administrador para o Azure AD, incluindo funções adicionais para além dos indicados na fase 1.
* Para além das funções definidas no Azure AD, Office 365 inclui um conjunto de funções de administrador que pode atribuir aos utilizadores na sua organização. Cada função de administrador é mapeado para funções de negócio comuns e fornece as pessoas na sua permissões de organização para efetuar tarefas específicas no Centro de administração do Office 365. Utilize o Centro de administração do Office para saber quais os utilizadores na sua organização tem acesso de administrador para Office 365, incluindo através de funções não geridas no Azure AD. Para obter mais informações, consulte [funções de administrador sobre o Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d) e [melhores práticas de segurança para o Office 365](https://support.office.com/article/Security-best-practices-for-Office-365-9295e396-e53d-49b9-ae9b-0b5828cdedc3).
* Execute o inventário no outros serviços na que sua organização depende, tais como o Azure, o Intune ou o Dynamics 365.
* Certifique-se de que as contas de administrador (contas que são utilizadas para fins de administração, não apenas diárias contas) tem endereços de correio eletrónico ligados aos mesmos a funcionar e registou para o MFA do Azure ou utilizam a MFA no local.
* Peça aos utilizadores para a respetiva justificação de negócio para acesso administrativo.
* Remova o acesso de administrador para essas pessoas e serviços que não será necessário.

#### <a name="identify-microsoft-accounts-in-administrative-roles-that-need-to-be-switched-to-work-or-school-accounts"></a>Identificar as contas Microsoft nas funções administrativas que precisam de ser mudado para o trabalho ou escola contas 

Por vezes, os administradores globais iniciais para uma organização reutilizarem as credenciais da conta Microsoft existentes quando começou a utilizar o Azure AD. Essas contas Microsoft devem ser substituídas pelo contas individuais baseado na nuvem ou sincronizadas. 

#### <a name="ensure-separate-user-accounts-and-mail-forwarding-for-global-administrator-accounts"></a>Certifique-se de que as contas de utilizador em separado e o correio de reencaminhamento para contas de administrador global 

As contas de administrador global não devem ter endereços de e-mail pessoal, as contas de e-mail pessoais regularmente phished pelos atacantes informático. Para ajudar a separar riscos de internet (ataques de phishing navegação na web não intencional) a partir de privilégios administrativos, criar a contas dedicadas para cada utilizador com privilégios administrativos. 

Se ainda não o fez, crie contas separadas para os utilizadores efetuar tarefas de administrador global, para se certificar de que não inadvertidamente abrir e-mails ou executar programas associados com as respetivas contas de administrador. Lembre-se de que essas contas tem acesso ao respetivo e-mail reencaminhado para uma caixa de correio do trabalho.  

#### <a name="ensure-the-passwords-of-administrative-accounts-have-recently-changed"></a>Certifique-se de que as palavras-passe de contas administrativas tem alterado recentemente

Certifique-se de que todos os utilizadores tem sessão iniciada para as respetivas contas administrativas e alterar as palavras-passe, pelo menos, uma vez nos últimos 90 dias. Além disso, certifique-se de que as contas de partilhado no quais vários utilizadores saber a palavra-passe tenha sido as palavras-passe recentemente alteradas.

#### <a name="turn-on-password-hash-synchronization"></a>Ativar sincronização de hash de palavra-passe

Sincronização de hash de palavra-passe é uma funcionalidade utilizada para sincronizar os hashes de hashes de palavra-passe de utilizador de uma instância do Active Directory no local para um Azure baseados na nuvem de instância do AD. Mesmo que se optar por utilizar a Federação com serviços de Federação do Active Directory (AD FS) ou de outros fornecedores de identidade, opcionalmente, pode configura a sincronização de hash de palavra-passe como uma cópia de segurança no caso da infraestrutura no local como o AD ou servidores ADFS falhar ou ficar temporariamente indisponível. Isto permite aos utilizadores iniciar sessão para o serviço utilizando a mesma palavra-passe que podem utilizar para iniciar sessão no local instância do AD. Além disso, permite proteção de identidade detetar credenciais comprometidas comparando os hashes de palavra-passe com palavras-passe conhecidas for comprometida, se um utilizador tem aproveitadas os respetivos mesmo endereço de e-mail e a palavra-passe noutros serviços não ligado ao Azure AD.  Para obter mais informações, consulte [implementar a sincronização de hash de palavra-passe com a sincronização do Azure AD Connect](./connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md).

#### <a name="require-multi-factor-authentication-mfa-for-users-in-all-privileged-roles-as-well-as-exposed-users"></a>Exigir autenticação multifator (MFA) para utilizadores nas funções com todos os privilégios, bem como os utilizadores expostos

Azure AD recomenda que necessitam de autenticação multifator (MFA) para todos os seus utilizadores, incluindo administradores e todos os outros utilizadores que teriam um impacto significativo se a sua conta foi comprometida (por exemplo, financeiros officers). Isto reduz o risco de um ataque devido a uma palavra-passe comprometido.

Ative:

* [MFA para contas de exposição de alta](authentication/multi-factor-authentication-security-best-practices.md) tais como contas para executivo officers numa organização 
* [MFA para cada conta de administrador associada um utilizador individual](authentication/howto-mfa-userstates.md) para outro ligado aplicações SaaS 
* MFA para todos os administradores para aplicações SaaS da Microsoft, incluindo administradores em funções geridos no Exchange Online e ao portal do Office

Se utilizar o Windows Hello para empresas, o requisito MFA pode ser satisfeito utilizando o Windows Hello início de sessão na experiência. Para obter mais informações, consulte [Windows Hello](https://docs.microsoft.com/windows/uwp/security/microsoft-passport). 

#### <a name="configure-identity-protection"></a>Configurar Identity Protection 

Azure AD Identity Protection é com base no algoritmo de monitorização e relatórios ferramenta que pode utilizar para detetar potenciais vulnerabilidades que afetam as identidades da organização. Pode configurar as respostas automáticas a essas atividades suspeitas detetadas e tome as medidas necessárias resolvê-los. Para obter mais informações, consulte [do Azure Active Directory Identity Protection](active-directory-identityprotection.md).

#### <a name="obtain-your-office-365-secure-score-if-using-office-365"></a>Obter a classificação do Office 365 proteger (se utilizar o Office 365)

Proteja os números de pontuação enviados os serviços do Office 365 que estiver a utilizar (como o OneDrive, o SharePoint e o Exchange), em seguida, analisa as suas definições e as atividades e compara-as com uma linha de base estabelecida pela Microsoft. Obterá uma classificação com base na forma como alinhado são com as melhores práticas de segurança. Qualquer pessoa que tenha permissões de administrador (administrador global ou uma função de administrador personalizadas) para uma subscrição do Office 365 empresas Premium ou Enterprise pode aceder a pontuação Secure em [ https://securescore.office.com ](https://securescore.office.com/).

#### <a name="review-the-office-365-security-and-compliance-guidance-if-using-office-365"></a>Reveja as orientações de segurança e conformidade do Office 365 (se utilizar o Office 365)

O [plano de segurança e conformidade](https://support.office.com/article/Plan-for-security-and-compliance-in-Office-365-dc4f704c-6fcc-4cab-9a02-95a824e4fb57) descreve a abordagem de um cliente do Office 365 deve configurar o Office 365 e tirar partido de outras funcionalidades do EMS. Em seguida, reveja os passos 3 a 6 como [proteger o acesso a dados e serviços do Office 365](https://support.office.com/article/Protect-access-to-data-and-services-in-Office-365-a6ef28a4-2447-4b43-aae2-f5af6d53c68e) e o guia para saber como [monitorizar a segurança e conformidade no Office 365](https://support.office.com/article/Monitor-security-and-compliance-in-Office-365-b62f1722-fd39-44eb-8361-da61d21509b6).


#### <a name="configure-office-365-activity-monitoring-if-using-office-365"></a>Configurar a monitorização da atividade do Office 365 (se utilizar o Office 365)

Pode monitorizar a forma como pessoas na sua organização estiver a utilizar serviços do Office 365, permitindo-lhe identificar os utilizadores que tenham uma conta de administrador e que não pode ter do Office 365 aceder devido a assinatura não para os portais. Para obter mais informações, consulte [relatórios de atividade no Centro de administração do Office 365](https://support.office.com/article/Activity-Reports-in-the-Office-365-admin-center-0d6dfb17-8582-4172-a9a9-aed798150263).

#### <a name="establish-incidentemergency-response-plan-owners"></a>Estabelecer os proprietários do plano de resposta de emergência/incidente

Efetuar efetivamente a resposta a incidentes é um undertaking complexa. Por conseguinte, estabelecer uma capacidade de resposta a incidentes com êxito requer planeamento considerável e de recursos. É essencial que continuamente monitorizar ataques de informático e estabelecer procedimentos para atribuir prioridades o processamento de incidentes. Os métodos eficazes de recolha, analisar e dados de relatórios são vitais para criar relações de e para estabelecer comunicação com outros grupos internos e planear os proprietários. Para obter mais informações, consulte [Microsoft Security Response Center](https://technet.microsoft.com/security/dn440717). 

#### <a name="secure-on-premises-privileged-administrative-accounts-if-not-already-done"></a>No local segura privilegiado contas administrativas, se ainda não tenha sido efetuada

Se o seu inquilino do Azure Active Directory está sincronizado com o Active Directory no local, em seguida, siga as orientações no [plano de acesso privilegiado segurança](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access): fase 1. Isto inclui criar contas de administrador separada para os utilizadores que precisam para realizar tarefas administrativas no local, implementar estações de trabalho de acesso com privilégios de administradores do Active Directory e criar palavras-passe de administrador local exclusivo para estações de trabalho e servidores.

### <a name="additional-steps-for-organizations-managing-access-to-azure"></a>Passos adicionais para as organizações a gerir o acesso para o Azure

#### <a name="complete-an-inventory-of-subscriptions"></a>Concluir um inventário das subscrições

Utilize o portal da empresa e o portal do Azure para identificar as subscrições da sua organização que alojam aplicações de produção. 

#### <a name="remove-microsoft-accounts-from-admin-roles"></a>Remover as contas Microsoft a funções de administrador

Contas Microsoft a partir de outros programas, tais como a Xbox, em direto e o Outlook não devem ser utilizadas como contas de administrador para subscrições organizacionais. Remover o estado de admin de todas as contas Microsoft e substitua com o Active Directory (por exemplo, chris@contoso.com) profissional ou escolar contas.

#### <a name="monitor-azure-activity"></a>Monitorizar a atividade do Azure

O registo de atividade do Azure fornece um histórico de eventos de nível de subscrição no Azure. Oferece informações sobre quem criado, atualizar e eliminar os recursos que e quando estes eventos ocorrerem. Para obter mais informações, consulte [auditoria e receber notificações sobre as ações importantes na sua subscrição do Azure](../monitoring-and-diagnostics/monitor-quick-audit-notify-action-in-subscription.md).


### <a name="additional-steps-for-organizations-managing-access-to-other-cloud-apps-via-azure-ad"></a>Passos adicionais para as organizações a gerir o acesso a outras aplicações na nuvem através do Azure AD 

#### <a name="configure-conditional-access-policies"></a>Configurar políticas de acesso condicional

Prepare as políticas de acesso condicional para aplicações alojadas na nuvem e no local. Se tiver dispositivos associado à área de trabalho de utilizadores, obter mais informações de [configurar no local acesso condicional com o registo de dispositivos do Azure Active Directory](active-directory-device-registration-on-premises-setup.md).


## <a name="stage-3-build-visibility-and-take-full-control-of-admin-activity"></a>Fase 3: Criar visibilidade e assumir o controlo total da atividade de administrador

![Fase 3](./media/admin-roles-best-practices/stage-three.png)

Fase 3 baseia-se a mitigações de fase 2 e foi concebido para ser implementada em cerca de 1 a 3 meses. Nesta fase de plano protegida acesso privilegiado inclui os seguintes componentes.

### <a name="general-preparation"></a>Preparação gerais

#### <a name="complete-an-access-review-of-users-in-administrator-roles"></a>Conclua uma revisão do acesso de utilizadores de funções de administrador

Os utilizadores empresariais mais são obtenham acesso privilegiado através dos serviços de nuvem, que pode levar a uma crescente de plataforma não gerida. Isto inclui os utilizadores se tornar administradores globais para o Office 365, os administradores da subscrição do Azure e os utilizadores que têm acesso de administrador para VMs ou através de aplicações SaaS. Em vez disso, as organizações devem ter todos os funcionários, especialmente admins, processar transações de negócio diárias como utilizadores sem privilégios e apenas colocar no direitos de administrador, conforme necessário. Uma vez que o número de utilizadores nas funções de administrador poderá ter ampliado desde adoção inicial, o acesso completo revê para identificar e confirme cada utilizador que seja elegível para ativar os privilégios de administrador. 

Faça o seguinte:

* Determine quais os utilizadores administradores do Azure AD, ativar a pedido, acesso de administração just-in-time e controlos de segurança baseada em funções.
* Converter os utilizadores que não possuem nenhum justificação encriptada para acesso de administrador com privilégios a uma função de diferentes (se nenhuma função elegíveis, remova-as).

#### <a name="continue-rollout-of-stronger-authentication-for-all-users"></a>Continuar a implementação de uma autenticação mais sólida para todos os utilizadores 

Exigir executivos C-suite, gestores de alto nível crítico IT e técnicos de segurança e outros utilizadores altamente expostos ter a autenticação moderna, segura, como o MFA do Azure ou o Windows Hello. 

#### <a name="use-dedicated-workstations-for-administration-for-azure-ad"></a>Utilizar estações de trabalho dedicadas para a administração do Azure AD

Os atacantes podem tentar contas com privilégios para obter acesso aos dados de uma organização e sistemas, de modo que poderá interromper a integridade e a autenticidade de dados, através de código malicioso que altera a lógica de programa ou snoops admin introduzir uma credencial de destino. Estações de trabalho acesso privilegiado (PAWs) fornecem um sistema operativo dedicado para tarefas confidenciais que se encontra protegido contra ataques de Internet e vetores de ameaças. Separar estas tarefas confidenciais e contas do diariamente utilizar estações de trabalho e dispositivos fornece muito forte proteção contra ataques de phishing e e das aplicações SO vulnerabilidades, vários ataques de representação, ataques de roubo de credenciais, tais como a tecla registo de segurança, passagem do Hash e passagem da permissão de segurança. Ao implementar estações de trabalho de acesso privilegiado, pode reduzir o risco que administradores introduzir credenciais de administrador, exceto no ambiente de trabalho que tenha sido protegido. Para obter mais informações, consulte [estações de trabalho de acesso privilegiado](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations).

#### <a name="review-national-institute-of-standards-and-technology-recommendations-for-handling-incidents"></a>Rever as recomendações do National Institute of Standards e tecnologia de processamento de incidentes 

O National Institute of Standards e da tecnológicas (NIST) fornece diretrizes para processamento de incidente, especialmente para analisar dados relacionados com o incidente e determinar a resposta adequada para cada incidente. Para obter mais informações, consulte [o NIST () computador incidente processamento manual de segurança (SP 800 61, revisão 2)](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf).

#### <a name="implement-privileged-identity-management-pim-for-jit-to-additional-administrative-roles"></a>Implementar Privileged Identity Management (PIM) para JIT para funções administrativas adicionais

Para o Azure Active Directory, utilize [do Azure AD Privileged Identity Management](active-directory-privileged-identity-management-configure.md) capacidade. Ativação de tempo limitado de funções com privilégios funciona, permitindo:

* Ativar privilégios de administrador para efetuar uma tarefa específica
* Impor o MFA durante o processo de ativação
* Utilizar alertas para informar os administradores sobre alterações fora de banda
* Permitir que os utilizadores manter a determinados privilégios durante um período de tempo pré-configurado
* Permitir que os administradores de segurança detetar todas as identidades privilegiadas, ver relatórios de auditoria e criar revisões de acesso para identificar todos os utilizadores que é elegível para ativar os privilégios de administrador

Se já estiver a utilizar o Azure AD Privileged Identity Management, ajuste períodos de tempo limite de tempo privilégios conforme necessário (por exemplo, janelas de manutenção).

#### <a name="determine-exposure-to-password-based-sign-in-protocols-if-using-exchange-online"></a>Determinar a exposição a protocolos baseados em palavra-passe de início de sessão (se utilizar o Exchange Online)

No passado, protocolos, pressupõe-se de que combinações de nome de utilizador/palavra-passe foram incorporadas em dispositivos, contas de e-mail, telemóveis e assim sucessivamente. Mas agora com o risco de ataques de informático na nuvem, recomendamos que identifique todos os utilizadores potenciais que, se as suas credenciais comprometidas, foi catastrófica à organização e exclui a capacidade de sessão ao respetivo e-mail através do nome de utilizador / palavra-passe através da implementação, requisitos de autenticação forte e acesso condicional. 

#### <a name="complete-a-roles-review-assessment-for-office-365-roles-if-using-office-365"></a>Concluir uma avaliação de revisão de funções para funções do Office 365 (se utilizar o Office 365)

Avaliar se todos os utilizadores administradores são nas funções de corretas (eliminar e reatribuir, de acordo com esta avaliação).

#### <a name="review-the-security-incident-management-approach-used-in-office-365-and-compare-with-your-own-organization"></a>Reveja a abordagem de gestão de incidentes de segurança utilizada no Office 365 e comparar com a sua própria organização

Pode transferir este relatório a partir da [gestão de incidentes de segurança no Microsoft Office 365](https://www.microsoft.com/download/details.aspx?id=54302).

#### <a name="continue-to-secure-on-premises-privileged-administrative-accounts"></a>Continuar a proteger contas no local com privilégios administrativos

Se o Azure Active Directory está ligado ao Active Directory no local, em seguida, siga as orientações no [plano de acesso privilegiado segurança](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access): fase 2. Isto inclui implementar estações de trabalho de acesso privilegiado para todos os administradores, exigir a MFA, com apenas suficiente administrador para manutenção de DC, reduzir a superfície de ataque de domínios, implementar o ATA para a deteção de ataques.

### <a name="additional-steps-for-organizations-managing-access-to-azure"></a>Passos adicionais para as organizações a gerir o acesso para o Azure

#### <a name="establish-integrated-monitoring"></a>Estabelecer monitorização integrada

O [Centro de segurança do Azure](../security-center/security-center-intro.md) fornece gestão de políticas e monitorização de segurança integrada nas suas subscrições do Azure, ajuda a detetar ameaças que caso contrário podem passar despercebidas e funciona com um ecossistema abrangente de segurança soluções.

#### <a name="inventory-your-privileged-accounts-within-hosted-virtual-machines"></a>As contas com privilégios dentro de máquinas virtuais alojadas de inventário

Na maioria dos casos, não precisa de conceder permissões sem restrições aos utilizadores para todas as suas subscrições do Azure ou recursos. Pode utilizar as funções de administrador do Azure AD para segregar funções na sua organização e conceder apenas a quantidade de acesso a utilizadores que necessitam para efetuar tarefas específicas. Por exemplo, utilize funções de administrador do Azure AD para permitir que um administrador que gere apenas as VMs numa subscrição, enquanto outro pode gerir bases de dados SQL dentro da mesma subscrição. Para obter mais informações, consulte [introdução ao controlo de acesso baseado em funções no portal do Azure](../role-based-access-control/overview.md).

#### <a name="implement-pim-for-azure-ad-administrator-roles"></a>Implementar o PIM para funções de administrador do Azure AD

Utilize o Privileged identity Management com funções de administrador do Azure AD para gerir, controlar e monitorizar o acesso a recursos do Azure. Utilizar o PIM protege contas com privilégios contra ataques de informático, diminuindo o tempo de exposição de privilégios e aumentar a visibilidade para a sua utilização através de relatórios e alertas. Para obter mais informações, consulte [acesso de gerir o RBAC para recursos do Azure Privileged Identity Management](../role-based-access-control/pim-azure-resource.md).

#### <a name="use-azure-log-integrations-to-send-relevant-azure-logs-to-your-siem-systems"></a>Utilizar integrações de registos do Azure para enviar relevantes do Azure regista aos seus sistemas SIEM 

Integração de registos do Azure permite-lhe integrar registos não processados a partir dos seus recursos do Azure para sistemas de Event Management (SIEM) e de informações de segurança existentes da sua organização. [Integração de registos do Azure](../security/security-azure-log-integration-overview.md) recolhe eventos do Windows a partir de registos do Visualizador de eventos do Windows e de recursos do Azure a partir de registos de atividade do Azure, alertas do Centro de segurança do Azure e os registos de diagnóstico do Azure. 


### <a name="additional-steps-for-organizations-managing-access-to-other-cloud-apps-via-azure-ad"></a>Passos adicionais para as organizações a gerir o acesso a outras aplicações na nuvem através do Azure AD

#### <a name="implement-user-provisioning-for-connected-apps"></a>Implementar o aprovisionamento de utilizadores para aplicações ligadas

Azure AD permite-lhe automatizar a criação, a manutenção e a remoção de identidades de utilizador em aplicações de nuvem (SaaS), tais como o Dropbox, Salesforce, ServiceNow e assim sucessivamente. Para obter mais informações, consulte [automatizar utilizador aprovisionamento e desaprovisionamento para aplicações SaaS com o Azure AD](active-directory-saas-app-provisioning.md).

#### <a name="integrate-information-protection"></a>Integrar a proteção de informações

MCAS permite-lhe investigar os ficheiros e definir políticas com base nas etiquetas de classificação do Azure Information Protection, permitindo maior visibilidade e controlo dos seus dados na nuvem. Analisar e classificar ficheiros na nuvem e aplicar etiquetas de proteção de informações do Azure. Para obter mais informações, consulte [integração do Azure Information Protection](https://docs.microsoft.com/cloud-app-security/azip-integration).

#### <a name="configure-conditional-access"></a>Configurar o acesso condicional

Configurar o acesso condicional com base num grupo, localização e sensibilidade de aplicação para [aplicações SaaS](https://azure.microsoft.com/overview/what-is-saas/) e AD do Azure ligado aplicações. 

#### <a name="monitor-activity-in-connected-cloud-apps"></a>Monitorizar a atividade de aplicações em nuvem ligadas

Para garantir o acesso dos utilizadores está protegido na, bem como aplicações ligadas, recomendamos que tiram partido [Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security). Isto protege o acesso da empresa para a nuvem de aplicações, além de proteger as contas de administrador, permitindo-lhe:

* Expandir a visibilidade e controlo de aplicações em nuvem
* Criar políticas de acesso, atividades e partilha de dados
* Identificar automaticamente o risco atividades, comportamentos anormais e ameaças
* Impedir fugas de dados
* Minimizar riscos e ameaças automatizada prevenção e imposição de política

O agente de Cloud App segurança SIEM integra-se a Cloud App Security com o seu servidor SIEM para ativar a monitorização centralizada dos alertas do Office 365 e atividades. Este é executado no seu servidor e obtém alertas e as atividades do Cloud App Security e fluxos-los para o servidor SIEM. Para obter mais informações, consulte [integração SIEM](https://docs.microsoft.com/cloud-app-security/siem).

## <a name="stage-4-continue-building-defenses-to-a-more-proactive-security-posture"></a>Fase 4: Continue a criar defesas para uma mais proativa postura de segurança


![Fase 4](./media/admin-roles-best-practices/stage-four.png)

Fase 4 do plano baseia-se a visibilidade da fase 3 e foi concebido para ser implementada em seis meses e acima. A concluir uma ajuda-o plano desenvolver strong privilegiado proteções de acesso de potenciais ataques que estão atualmente conhecido e está disponível atualmente. Infelizmente, ameaças de segurança constantemente evoluem e deslocar, pelo que recomendamos que veja segurança como um processo contínuo concentra-se em que gera o custo e reduzindo a taxa de êxito de adversários qualificados segmentação do seu ambiente.

Com privilégios de proteger o acesso é um primeiro passo crítico para estabelecer a segurança oferece garantias ao rendimento para recursos de negócio numa organização moderna, mas não é a parte apenas de um programa de segurança completa, que inclui elementos, tais como a política, operações, informações segurança, servidores, aplicações, PCs, dispositivos, recursos de infraestrutura de nuvem e outros componentes fornecem oferece garantias ao rendimento de segurança em curso. 

Para além de gerir as contas de acesso privilegiado, recomendamos que reveja o seguinte numa base contínua:

* Certifique-se de que os administradores estão a fazer as respetivas empresas diárias como os utilizadores sem privilégios.
* Apenas conceda acesso privilegiado quando for necessário e removê-lo posteriormente (just-in-time).
* Manter e reveja a atividade de auditoria relacionado com a contas com privilégios.

Para obter mais informações sobre como criar um plano de segurança completa, consulte [recursos de arquitetura de TI para o Microsoft cloud](https://docs.microsoft.com/office365/enterprise/microsoft-cloud-it-architecture-resources). Para obter mais informações no envolvimento dos serviços da Microsoft para melhorar a qualquer um destes tópicos, contacte o seu representante da Microsoft ou consulte [criar defesas informático fundamental para proteger a sua empresa](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).

Nesta fase final em curso do plano protegida acesso privilegiado inclui os seguintes componentes.

### <a name="general-preparation"></a>Preparação gerais

#### <a name="review-admin-roles-in-azure-active-directory"></a>Reveja as funções de administrador no Azure Active Directory 

Determinar se as funções de administrador atuais incorporado do Azure AD são ainda atualizadas e certifique-se de que os utilizadores estão apenas nas funções e as delegações que precisam de permissões correspondente. Com o Azure AD, pode designar administradores separados para servir diferentes funções. Para obter mais informações, consulte [atribuir funções de administrador no Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md).

#### <a name="review-users-who-have-administration-of-azure-ad-joined-devices"></a>Reveja os utilizadores com administração do Azure AD dispositivos associados ao

Para obter mais informações, consulte [como para configurar híbrida do Azure Active Directory dispositivos associados a um](device-management-hybrid-azuread-joined-devices-setup.md).

#### <a name="review-members-of-built-in-office-365-admin-roleshttpssupportofficecomarticleabout-office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d"></a>Reveja os membros [funções de administrador incorporadas do Office 365](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d)
Se estiver a utilizar o Office 365.
‎
#### <a name="validate-incident-response-plan"></a>Validar o plano de resposta a incidentes

Para melhorar o plano, a Microsoft recomenda que regularmente a validar que o plano funciona conforme esperado:

* Percorrer o mapa da estrada existente para ver o que foi perdido
* Com base na análise postmortem, rever existentes ou definir nova melhores práticas
* Certifique-se de que o seu plano de resposta a incidentes atualizado e melhores práticas são distribuídas em toda a organização


### <a name="additional-steps-for-organizations-managing-access-to-azure"></a>Passos adicionais para as organizações a gerir o acesso para o Azure 

Determinar se necessita de [transferir a propriedade de uma subscrição do Azure para outra conta](../billing/billing-subscription-transfer.md).
‎

## <a name="break-glass-what-to-do-in-an-emergency"></a>"Break glass": o que fazer no emergência

![Emergência](./media/admin-roles-best-practices/emergency.jpeg)

1. Notificar os gestores de chaves e officers de segurança com pertinentes informações sobre o incidente.

2. Consulte o manual de comunicação social de ataque. 

3. Aceder a combinação de nome de utilizador/palavra-passe de conta "break glass" para iniciar sessão Azure AD. 

4. Obter ajuda da Microsoft por [abrir um pedido de suporte do Azure](../azure-supportability/how-to-create-azure-support-request.md).

5. Observe o [relatórios de início de sessão do Azure AD](active-directory-reporting-azure-portal.md). Pode haver um atraso entre a ocorrer um evento e quando está incluído no relatório.

6. Para ambientes híbridos, se Federado e AD servidor FS já não estiver disponível, poderá ter de mudar temporariamente da autenticação federada para utilizar a sincronização de hash de palavra-passe. Isto reverte de Federação de domínio para autenticação gerida até que o servidor do AD FS ficar disponível.

7. Monitorizar contas com privilégios de correio eletrónico.

8. Certifique-se de que guarda as cópias de segurança de registos relevantes para investigação forense e legal potencial.

Para obter mais informações sobre a forma como o Microsoft Office 365 processa a incidentes de segurança, consulte [gestão de incidentes de segurança no Microsoft Office 365](http://aka.ms/Office365SIM).

## <a name="faq-common-questions-we-receive-regarding-securing-privileged-access"></a>FAQ: Perguntas comuns recebemos sobre acesso privilegiado proteger  


**P:** o que devo fazer se os componentes de acesso seguro ainda não implementado ainda?

**Resposta:** definir, pelo menos, dois break-transparência da conta, atribua o MFA para as contas de administrador com privilégios e contas de utilizador em separado das contas de Administrador Global.


**P:** após uma violação, o que é o problema superior que tem de ser resolvidos pela primeira vez?

**Resposta:** Lembre-se de que está a necessidade de autenticação mais fortes para indivíduos expostos a elevada disponibilidade.


**P:** o que acontece se o nosso administradores com privilégios foram desativadas?

**Resposta:** criar uma conta de Administrador Global que é mantida sempre atualizada.


**P:** o que acontece se houver apenas um administrador global à esquerda e não não possível contactar? 

**Resposta:** utilize uma das suas contas de break glass acesso privilegiado imediata.


**P:** como posso proteger admins dentro da minha organização?

**Resposta:** ter administradores fazer sempre as respetivas empresas diárias como os utilizadores "sem privilégios" padrão.
 

**P:** quais são as melhores práticas para criar contas de administrador no Azure AD?

**Resposta:** reserva privilegiado acesso para tarefas de administração específico.


**P:** ferramentas de que existem para reduzir o acesso de administrador persistente?

**Resposta:** funções de administrador de Privileged Identity Management (PIM) e o Azure AD.


**P:** o que é a posição da Microsoft em sincronizar as contas de administrador para o Azure AD?

**Resposta:** camada 0 contas de administrador (incluindo contas, grupos e outros elementos que tenham direto ou indireto controlo administrativo de domínios, controladores de domínio ou floresta do AD e todos os elementos) são utilizadas apenas para no local Contas de anúncios e estão normalmente não sincronizado para o Azure AD para a nuvem. 


**P:** como vamos manter admins de atribuir acesso de administrador aleatório no portal?

**Resposta:** utilizar contas sem privilégios para todos os utilizadores e a maioria dos administradores. Comece por desenvolver requisitos de espaço da organização para determinar que algumas contas de admin devem ser privilegiadas. E monitorize os utilizadores administrativos recentemente criado.


## <a name="next-steps"></a>Passos Seguintes

* [Microsoft Trust Center para a segurança do produto](https://www.microsoft.com/en-us/trustcenter/security) – as funcionalidades de segurança da Microsoft na nuvem, os produtos e serviços

* [Microsoft Trust Center - conformidade](https://www.microsoft.com/en-us/trustcenter/compliance/complianceofferings) – conjunto de ofertas de conformidade para os serviços de nuvem abrangente da Microsoft

* [Documentação de orientação sobre como efetuar uma avaliação de risco](https://www.microsoft.com/en-us/trustcenter/guidance/risk-assessment) -gerir requisitos de segurança e conformidade do cloud services da Microsoft

### <a name="other-ms-online-services"></a>Outros serviços Online da MS 

* [Segurança do Microsoft Intune](https://www.microsoft.com/en-us/trustcenter/security/intune-security) – o Intune fornece gestão de dispositivos móveis, gestão de aplicações móveis e as capacidades de gestão de PCs da nuvem.

* [Segurança do Microsoft Dynamics 365](https://www.microsoft.com/en-us/trustcenter/security/dynamics365-security) – Dynamics 365 é a solução baseado na nuvem da Microsoft que unifica gestão de relação de cliente (CRM) e capacidades (ERP) de planeamento de recursos de empresa.

 
