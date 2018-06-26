---
title: Cinco passos para proteger a sua infraestrutura de identidade no Azure Active Directory
description: Este documento descreve uma lista de ações importantes, devem implementar os administradores para ajudar a proteger a organização utilizar as capacidades do Azure AD
services: active-directory
author: martincoetzer
manager: manmeetb
tags: azuread
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 06/18/2018
ms.author: martincoetzer
ms.openlocfilehash: b15fff6e868bfac973f9d2a7277f0fac1e29d845
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938469"
---
# <a name="five-steps-to-securing-your-identity-infrastructure"></a>Cinco passos para proteger a sua infraestrutura de identidade

Se estiver a ler este documento, está ciente de significância de segurança. É provável que já tem a responsabilidade de proteger a sua organização. Se precisar de convencê--outras pessoas a importância de segurança, enviar-lhes para lerem a versão mais recente [relatório Microsoft segurança Intelligence](https://www.microsoft.com/security/intelligence-report).

Este documento ajuda-o obter uma postura mais segura utilizando as capacidades do Azure Active Directory utilizando uma lista de verificação do passo de cinco para inoculate a sua organização contra ataques de informático.

Esta lista de verificação irá ajudá-lo a implementar rapidamente críticas ações recomendadas para proteger a sua organização imediatamente por explicar como:

* Reforçar as suas credenciais.
* Reduza a sua área de superfície de ataque.
* Automatizar a resposta de ameaça.
* Aumente a deteção de auditoria e monitorização.
* Ative mais previsível e concluída pelo utilizador final segurança com conhecidas.

> [!NOTE]
> Muitas das recomendações neste documento aplicam-se apenas às aplicações que estão configuradas para utilizar o Azure Active Directory como o seu fornecedor de identidade. Configurar aplicações para Single Sign-On garante as vantagens das políticas de credencial, a deteção de ameaças, adicione a auditorias, registo e outras funcionalidades para essas aplicações. [Início de sessão único através do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-sso) é o alicerce - de que todos os estas recomendações baseiam-se.

## <a name="before-you-begin-protect-privileged-accounts-with-mfa"></a>Antes de começar: proteger contas privilegiadas com a MFA

Antes de começar esta lista de verificação, certifique-se de que não são comprometidas enquanto está a ler esta lista de verificação. Primeiro terá de proteger as suas contas privilegiadas.

Os atacantes que obter o controlo de contas com privilégios podem fazer imenso danos, pelo que é fundamental para proteger estas contas primeiro. Ativar e exigir [Azure multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) (MFA) para todos os administradores na sua organização ao utilizar [proteção de linha de base](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-baseline-protection). Se ainda não implementado MFA, fazê-lo agora! É importante que.

Todos os conjuntos de? Vamos começar a utilizar na lista de verificação.

## <a name="step-1---strengthen-your-credentials"></a>Passo 1 – reforçar as suas credenciais 

A maioria das falhas de segurança empresarial provir com uma conta comprometida com um dos alguns dos métodos como spray de palavra-passe, reprodução de violação ou phishing. Saiba mais sobre estes ataques neste vídeo:
> [!VIDEO https://channel9.msdn.com/events/Ignite/Microsoft-Ignite-Orlando-2017/BRK3016/player]

Se os utilizadores no seu sistema de identidade estiver a utilizar palavras-passe fracas e não strengthening-los com multi-factor authentication, não se encontra um fim de se ou quando são a comprometidas – apenas "frequência."

### <a name="make-sure-your-organization-use-strong-authentication"></a>Certifique-se a organização utilizar forte autenticação

Dada a frequência das palavras-passe de que está a ser adivinhado, phished, roubado com software maligno ou reutilizada, é essencial fazer uma cópia da palavra-passe com algum tipo de credencial forte – Saiba mais sobre [Azure multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication).

### <a name="turn-off-traditional-complexity-expiration-rules-and-start-banning-commonly-attacked-passwords-instead"></a>Desativar a complexidade tradicional, regras de expiração e iniciar banning em vez disso, as palavras-passe normalmente atacadas

Muitas organizações utilizam a complexidade tradicional (por exemplo, os carateres especiais) e as regras de expiração de palavra-passe. Pesquisa da Microsoft tem mostrado que estas políticas são prejudiciais, fazendo com que aos utilizadores escolher palavras-passe que são mais fáceis de adivinhar.

Recomendações da Microsoft, consistentes com [orientações NIST](https://pages.nist.gov/800-63-3/sp800-63b.html), está a implementar os três seguintes:

1. Exigir palavras-passe tem, pelo menos, 8 carateres. Já não é necessariamente melhor, como causarem aos utilizadores escolher palavras-passe previsível, guardar palavras-passe nos ficheiros ou anotá-los.
2. Desativar regras de expiração, o que os utilizadores de unidade para palavras-passe facilmente estimadas como **Summer2018!**.
3. Desativar os requisitos de composição de caráter e impedir que os utilizadores escolher palavras-passe normalmente atacadas, como o fazer com que os utilizadores escolham substituições de caráter previsível em palavras-passe.

Pode utilizar [PowerShell para impedir que as palavras-passe prestes a expirar](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-policy) nos utilizadores, se criar identidades no Azure AD diretamente. As organizações com no local AD com o Azure AD Connect para sincronização de identidades para o Azure AD (também conhecido como uma implementação híbrida), deve implementar no local [políticas de palavra-passe inteligente](https://aka.ms/passwordguidance) utilizando [política de grupo de domínio definições](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/hh994572(v%3dws.10)) ou [do Windows PowerShell](https://docs.microsoft.com/powershell/module/addsadministration/set-addefaultdomainpasswordpolicy).

Do Azure Active Directory [palavra-passe de banned dinâmica](https://docs.microsoft.com/azure/active-directory/active-directory-secure-passwords) funcionalidade utiliza o comportamento de atacante atual para impedir que os utilizadores a definição de palavras-passe que podem ser facilmente adivinhadas. Esta capacidade está sempre ativada e organizações com uma implementação híbrida podem beneficiar desta funcionalidade, permitindo [repetição de escrita de palavras-passe](https://docs.microsoft.com/azure/active-directory/authentication/howto-sspr-writeback) ou podem implementar [proteção de palavra-passe do Azure AD para o Windows Server Active Directory Diretório](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad-on-premises). Proteção de palavra-passe do Azure AD impede os utilizadores escolher palavras-passe comuns em geral e as palavras-passe personalizadas pode configurar.

### <a name="protect-against-leaked-credentials-and-add-resilience-against-outages"></a>Proteger contra credenciais obtidas ilicitamente e adicione resiliência contra falhas

Se a sua organização utiliza uma solução de identidade híbrida, deve ativar sincronização de hash de palavra-passe para duas seguintes motivos:

* O [os utilizadores com credenciais obtidas ilicitamente](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-risk-events) avisa-o de pares de nome de utilizador e palavra-passe que foi expostos na "web escuro". o relatório de gestão do Azure AD Um volume incredible de palavras-passe é transmitido através da reutilização phishing, software maligno e palavra-passe em sites de terceiros que são mais tarde infringido. Microsoft localiza a fuga muitas destas credenciais e informará que, neste relatório, se corresponderem a credenciais na sua organização – mas apenas se a [ativar a sincronização de hash de palavra-passe](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization)!
* Em caso de uma falha no local (por exemplo, por um ataque de ransomware), poderá mudar para [autenticação na nuvem utilizando a sincronização de hash de palavra-passe](https://docs.microsoft.com/azure/security/azure-ad-choose-authn). Este método de autenticação de cópia de segurança irá permitir-lhe continuar a aceder a aplicações configuradas para autenticação no Azure Active Directory, incluindo o Office 365.

Saiba mais sobre como [sincronização de hash de palavra-passe](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization) funciona.

### <a name="implement-ad-fs-extranet-lockout"></a>Bloqueio de extranet do implementar AD FS

As organizações que configura as aplicações para autenticar diretamente para o Azure AD beneficiam [bloqueio inteligente do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-secure-passwords). Se utilizar o AD FS, implementar o AD FS [o bloqueio de extranet](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-lockout-protection). O bloqueio de extranet protege contra ataques de força bruta, o destino do AD FS ao impedir que os utilizadores que está a ser bloqueada no Active Directory.

### <a name="take-advantage-of-intrinsically-secure-easier-to-use-credentials"></a>Tirar partido das intrinsecamente seguro e mais fáceis de utilizar as credenciais

Utilizar [Windows Hello](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification), pode substituir as palavras-passe com autenticação de dois fatores forte em PCs e dispositivos móveis. Esta autenticação é composta por um novo tipo de credencial de utilizador que está associado a um dispositivo e utiliza um biométrica ou PIN.

## <a name="step-2---reduce-your-attack-surface"></a>Passo 2 - reduzir a superfície de ataque

Tendo em conta o pervasiveness de compromisso de palavra-passe, que minimiza a superfície de ataque da sua organização é fundamental. Pontos de eliminar utilização dos protocolos mais antigos, menos seguras, limitar o acesso de entrada e exercising mais significativo controlo de acesso administrativo a recursos pode ajudar a reduzir a área de superfície de ataque.

### <a name="block-legacy-authentication"></a>Autenticação de legado do bloco

Aplicações que utilizam os seus próprios métodos legados para autenticar com o Azure AD e aceder aos dados da empresa, implicar riscos outro para organizações. Exemplos de aplicações que utilizam autenticação legada são clientes POP3, IMAP4 ou SMTP. Aplicações de autenticação legado autenticar em nome do utilizador e impedem que ao fazê-lo avançadas avaliações de segurança do Azure AD. A autenticação moderna, alternativa, irá reduzir o risco de segurança, porque suporta autenticação multifator e de acesso condicional. Recomendamos as seguintes três ações:

1. Bloco [autenticação legada, se utilizar o AD FS](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/access-control-policies-w2k12).
2. A configuração [SharePoint Online e Exchange Online, para utilizar a autenticação moderna](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-no-modern-authentication).
3. Utilize [políticas de acesso condicional para bloquear autenticação legada](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-conditions#legacy-authentication).

### <a name="block-invalid-authentication-entry-points"></a>Pontos de entrada do bloco autenticação inválido

Utilizar mentality de violação assume, deve reduzir o impacto de credenciais de utilizador comprometidas quando que ocorrem. Para cada aplicação no seu ambiente, considere os casos de utilização válido: os grupos, quais as redes, os dispositivos e outros elementos estão autorizados –, em seguida, bloquear o resto. Asseguramos para restringir a utilização de [contas altamente privilegiadas ou serviços](https://docs.microsoft.com/azure/active-directory/admin-roles-best-practices). Com [acesso condicional do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal), pode controlar o acesso de utilizadores como autorizado respetivas aplicações e recursos com base nas condições específicas define.

Preste especial atenção a contas de serviço (contas utilizadas para efetuar tarefas de forma automática). Utilizar o acesso condicional, pode certificar-se essas contas só podem ser executado contra o serviço do IP, e o do momento do dia, que é adequado.

### <a name="implement-azure-ad-privileged-identity-management"></a>Implementar o Azure AD Privileged Identity Management

Outro impacto de "partem do princípio de violação" é a necessidade para minimizar a probabilidade de que uma conta comprometida pode operar com uma função com privilégios. 

[Azure AD Privileged Identity Management (PIM)](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure) ajuda minimizado privilégios de conta, ajudando-o:

* Identificar e gerir os utilizadores atribuídos a funções administrativas.
* Compreenda as funções de privilégio utilizadas ou excessivo que deve remover.
* Estabelece regras para tornar funções privilegiadas se de que estão protegidas pelo multi-factor authentication.
* Estabelece regras para se certificar de que funções com privilégios são concedidas apenas tempo suficiente para realizar a tarefa com privilégios.

Ativar o Azure AD PIM, em seguida, ver os utilizadores que estão atribuídos a funções administrativas e remover contas desnecessárias essas funções. Para os restantes utilizadores com privilégios, mova-os partir permanente para elegível. Por fim, estabelecer políticas adequados para se certificar quando precisar de obter acesso a essas funções com privilégios, podem fazer com segurança.

Como parte da implementação do processo de conta com privilégios, siga o [melhor prática criar contas de emergência, pelo menos, dois](https://docs.microsoft.com/azure/active-directory/admin-roles-best-practices) para se certificar de que tem acesso para o Azure AD, se lhe bloquear por si.

## <a name="step-3---automate-threat-response"></a>Passo 3 - automatizar a resposta de ameaça

Azure Active Directory tem muitas funcionalidades automaticamente intercetar ataques, para remover a latência entre deteção e a resposta. Pode reduzir os custos e riscos, ao reduzir os criminals tempo utilizam para incorporar próprios no seu ambiente. Eis os passos concretos que pode tomar.

### <a name="implement-user-risk-security-policy-using-azure-ad-identity-protection"></a>Implementar a política de segurança de risco de utilizador utilizando o Azure AD Identity Protection

O risco de utilizador indica a probabilidade de identidade de um utilizador tiver sido comprometida e é calculada com base no [eventos de risco do utilizador](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) que estão associados a identidade do utilizador. Uma política de risco do utilizador é uma política de acesso condicional que avalia o nível de risco para um grupo ou utilizador específico. Com base em baixa, média, alto de nível de risco, uma política pode ser configurada para bloquear o acesso ou exigir uma alteração de palavra-passe segura utilizando a autenticação multifator. Recomendação da Microsoft é exigir uma alteração de palavra-passe segura para utilizadores de alto risco.

![Utilizadores marcados em risco](media/azure-ad/azure-ad-sec-steps1.png)

### <a name="implement-sign-in-risk-policy-using-azure-ad-identity-protection"></a>Implementar política de início de sessão risco com o Azure AD Identity Protection

Início de sessão risco é a probabilidade que alguém que não seja o proprietário da conta está a tentar iniciar sessão utilizando a identidade. A [política de início de sessão risco](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) é uma política de acesso condicional que avalia o nível de risco para um grupo ou utilizador específico. Com base no nível do risco (alta/média/baixo), uma política pode ser configurada para bloquear o acesso ou forçar a autenticação multifator. Certifique-se que forçar a autenticação multifator, média ou superior risco inícios de sessão.

![Inicie sessão IPs anónimo](media/azure-ad/azure-ad-sec-steps2.png)

## <a name="step-4---increase-your-awareness"></a>Passo 4 - aumentar o seu conhecimento

Auditoria e registo de eventos relacionados com a segurança e alertas relacionados são componentes essenciais de uma estratégia de proteção eficiente. Registos de segurança e relatórios de fornecem um registo eletrónico de atividades suspeitas e ajuda a detetar padrões que possam indicar tentada ou com êxito penetração externa da rede e a ataques internos. Pode utilizar a auditoria para monitorizar a atividade do utilizador, a conformidade regulamentar documento, executar análises forenses e muito mais. Os alertas fornecem notificações de eventos de segurança.

### <a name="monitor-azure-ad"></a>Monitor do Azure AD

Funcionalidades e serviços do Microsoft Azure fornecem-lhe configurável segurança de auditoria e registo opções para o ajudar a identificar lacunas na sua políticas de segurança e mecanismos e os intervalos para ajudar a evitar falhas de endereços. Pode utilizar [registo do Azure e auditoria](https://docs.microsoft.com/azure/security/azure-log-audit) e utilizar [relatórios de atividade no portal do Azure Active Directory de auditoria](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs).

### <a name="monitor-azure-ad-connect-health-in-hybrid-environments"></a>Monitorizar o Azure AD Connect Health em ambientes híbridos

[Monitorização do AD FS com o Azure AD Connect Health](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-adfs) fornece-lhe maior aprofundadas sobre possíveis problemas e visibilidade de ataques na sua infraestrutura de AD FS. O Azure AD Connect Health fornece alertas com detalhes, os passos de resolução e ligações para documentação relacionada; análise de utilização para várias métricas relacionadas com o tráfego de autenticação; monitorização de desempenho e relatórios.

![Azure AD Connect Health](media/azure-ad/azure-ad-sec-steps4.png)

### <a name="monitor-azure-ad-identity-protection-events"></a>Monitorizar os eventos do Azure AD Identity Protection

[Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) uma notificação, monitorização e relatórios ferramenta que pode utilizar para detetar potenciais vulnerabilidades que afetam as identidades da organização. Deteta eventos de risco, como credenciais obtidas ilicitamente, impossível, e inícios de sessão do infetados dispositivos, endereços IP anónimos, endereços IP associados à atividade suspeita e localizações desconhecidas. Ative alertas de notificação receber o e-mail dos utilizadores em risco e/ou um e-mail de texto implícita semanal.

![Utilizadores marcados em risco](media/azure-ad/azure-ad-sec-steps3.png)

## <a name="step-5---enable-end-user-self-help"></a>Passo 5 - Enable conhecidas de utilizador final

Quanto possível poderá ser útil equilibrar a segurança com a produtividade. Ao longo de linhas da mesmas da sua viagem com mente que está a definir uma foundation para segurança longa run a aproximar-se, pode remover friction da sua organização, mas permitir que os utilizadores ao restantes vigilant. 

### <a name="implement-self-service-password-reset"></a>Implementar a reposição de palavra-passe self-service

Do Azure [repor a palavra-passe self-service (SSPR)](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started) oferece uma simples significa para administradores de TI permitir que os utilizadores reponham ou desbloquear as respetivas palavras-passe ou contas sem a intervenção do administrador. O sistema inclui relatórios detalhados que controlam quando os utilizadores acedem ao sistema, juntamente com notificações para o alertar quanto a utilizações indevidas ou abusos. 

### <a name="implement-self-service-group-management"></a>Implementar a gestão de grupos self-service

O Azure AD fornece a capacidade para gerir o acesso a recursos através de grupos de segurança e de grupos do Office 365. Estes grupos podem ser geridos por proprietários do grupo em vez dos administradores de TI. Conhecido como [gestão de grupos self-service](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management), esta funcionalidade permite que os proprietários do grupo que não estão atribuídos uma função administrativa para criar e gerir grupos sem depender de administradores para processar os pedidos.

### <a name="implement-azure-ad-access-reviews"></a>Revisões de acesso de implementar do Azure AD

Com [acesso do Azure AD revê](https://docs.microsoft.com/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview), pode gerir as associações de grupo, acesso a aplicações da empresa, e atribuições de função com privilégios para se certificar de que mantém uma norma de segurança que não conceder acesso aos utilizadores para expandido períodos de Quando não precisarem de tempo.

## <a name="summary"></a>Resumo

Existem vários aspetos a uma infraestrutura de identidade segura, mas esta lista de verificação do passo de cinco ajudará a realizar rapidamente uma infraestrutura de identidade mais segura e segura:

* Reforçar as suas credenciais.
* Reduza a sua área de superfície de ataque.
* Automatizar a resposta de ameaça.
* Aumente a deteção de auditoria e monitorização.
* Ative mais previsível e concluída pelo utilizador final segurança com conhecidas.

Agradecemos muito a sério como efetuar a segurança de identidade e hope que este documento é um plano útil para um postura mais segura para a sua organização.

## <a name="next-steps"></a>Passos Seguintes
Se necessitar de assistência para planear e implementar as recomendações, consulte o [planos de implementação do projeto do Azure AD](http://aka.ms/deploymentplans) para obter ajuda.
