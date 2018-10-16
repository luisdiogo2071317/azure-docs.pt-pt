---
title: Cinco etapas para proteger a sua infraestrutura de identidade no Azure Active Directory
description: Este documento descreve uma lista de ações importantes, os administradores devem implementar para os ajudar a proteger sua organização com capacidades do Azure AD
services: active-directory
author: martincoetzer
manager: manmeetb
tags: azuread
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 06/18/2018
ms.author: martincoetzer
ms.openlocfilehash: 94d96cab28f738984b3d05d5eee0754e8c5e75b6
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49341584"
---
# <a name="five-steps-to-securing-your-identity-infrastructure"></a>Cinco etapas para proteger a sua infraestrutura de identidade

Se estiver lendo este documento, está atento a importância da segurança. Provavelmente já possuem a responsabilidade de proteger a sua organização. Se precisar de convencer outros a importância da segurança, envie-lhes para ler a versão mais recente [relatório de inteligência de segurança da Microsoft](https://www.microsoft.com/security/intelligence-report).

Este documento ajuda-o a obter uma postura mais segura, utilizando as capacidades do Azure Active Directory utilizando uma lista de verificação de cinco etapas para Inocular a organização contra ataques informáticos.

Esta lista de verificação lhe ajudarão a implantar rapidamente críticas ações recomendadas para proteger a sua organização imediatamente ao explicar como:

* Reforce as suas credenciais.
* Reduza sua área de superfície de ataque.
* Automatize a resposta a ameaças.
* Aumente seu conhecimento sobre auditoria e monitorização.
* Ative mais previsível e completa pelos usuários finais segurança com a ajuda autónoma.

> [!NOTE]
> Muitas das recomendações neste documento aplicam-se apenas às aplicações que estão configuradas para utilizar o Azure Active Directory como respetivo fornecedor de identidade. Configurar aplicações para início de sessão único assegura os benefícios das políticas de credencial, deteção de ameaças, auditorias, registos e outras funcionalidades a adicionar a esses aplicativos. [Início de sessão único através do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-sso) é a base - de que todos os baseiam-se estas recomendações.

## <a name="before-you-begin-protect-privileged-accounts-with-mfa"></a>Antes de começar: proteger contas com privilégios com a MFA

Antes de começar esta lista de verificação, certifique-se de que não são comprometidas enquanto estiver lendo esta lista de verificação. Primeiro tem de proteger as suas contas com privilégios.

Os invasores que obtém controle de contas com privilégios podem fazê-lo enorme danos, é fundamental para proteger estas contas pela primeira vez. Ativar e exigir [multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) (MFA) para todos os administradores na sua organização ao utilizar [proteção de linha de base](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-baseline-protection). Se ainda não implementado MFA, fazê-lo agora! É tão importante.

Tudo pronto? Vamos começar a lista de verificação.

## <a name="step-1---strengthen-your-credentials"></a>Passo 1 – reforçar as suas credenciais 

A maioria das falhas de segurança empresarial têm origem com uma conta comprometida com um dos vários métodos como spray de palavra-passe, a repetição de violação ou phishing. Saiba mais sobre estes ataques neste vídeo (15m de 1 hora):
> [!VIDEO https://channel9.msdn.com/events/Ignite/Microsoft-Ignite-Orlando-2017/BRK3016/player]

Se os utilizadores no seu sistema de identidade estão usando senhas fracas e não fortalecendo-los com a autenticação multifator, não é uma questão de se ou quando são a comprometidas – apenas "quantas vezes."

### <a name="make-sure-your-organization-use-strong-authentication"></a>Certifique-se a sua organização utilizar uma autenticação segura

Tendo em conta a frequência de senhas que está a ser percebido, phished, roubado com software maligno ou reutilizados, é fundamental para fazer uma cópia da palavra-passe com alguma forma de credencial forte – Saiba mais sobre [multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication).

### <a name="turn-off-traditional-complexity-expiration-rules-and-start-banning-commonly-attacked-passwords-instead"></a>Desativar a complexidade tradicional, regras de expiração e comece a banir normalmente atacadas as palavras-passe em vez disso

Muitas organizações utilizam a complexidade tradicional (por exemplo, carateres especiais) e as regras de expiração de palavra-passe. Pesquisa da Microsoft mostrou que estas políticas são prejudiciais, fazendo com que aos utilizadores escolher palavras-passe que são mais fáceis de adivinhar.

Recomendações da Microsoft, consistentes com [documentação de orientação do NIST](https://pages.nist.gov/800-63-3/sp800-63b.html), está a implementar os três seguintes:

1. Exigir que as senhas têm, pelo menos, 8 carateres. Já não é necessariamente a melhor, pois estes provocarem aos utilizadores escolher palavras-passe previsível, guardar palavras-passe nos ficheiros ou anotá-las.
2. Desativar regras de expiração, o que levar os seus utilizadores para palavras-passe facilmente adivinháveis como **Summer2018!**.
3. Desative a requisitos de composição de caracteres e impedir que os utilizadores escolham normalmente atacadas palavras-passe, como eles fazem com que os usuários escolham substituições de caráter previsível em palavras-passe.

Pode usar [PowerShell para impedir que as palavras-passe prestes a expirar](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-policy) nos utilizadores, se criar as identidades no Azure AD diretamente. As organizações que utilizam no local AD com o Azure AD Connect para sincronização de identidades para o Azure AD (também conhecido como uma implementação híbrida), deve implementar no local [políticas de palavra-passe inteligente](https://aka.ms/passwordguidance) usando [diretiva de grupo do domínio as definições](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/hh994572(v%3dws.10)) ou [Windows PowerShell](https://docs.microsoft.com/powershell/module/addsadministration/set-addefaultdomainpasswordpolicy).

Do Azure Active Directory [palavra-passe de banidas dinâmica](https://docs.microsoft.com/azure/active-directory/active-directory-secure-passwords) recurso usa o comportamento atual do invasor para impedir que os utilizadores a definição de palavras-passe que podem ser facilmente adivinhadas. Esta capacidade está sempre ativada e as organizações com uma implementação híbrida podem beneficiar desta funcionalidade, permitindo [repetição de escrita de palavra-passe](https://docs.microsoft.com/azure/active-directory/authentication/howto-sspr-writeback) ou podem implementar [proteção de palavra-passe do Azure AD para o Windows Server Active Directory Diretório](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad-on-premises). Proteção de palavra-passe do Azure AD bloqueia os utilizadores escolham palavras-passe comuns em geral e palavras-passe personalizadas pode configurar.

### <a name="protect-against-leaked-credentials-and-add-resilience-against-outages"></a>Proteger contra fugas de credenciais e adicionar resiliência contra falhas

Se a sua organização utiliza uma solução de identidade híbrida, em seguida, deve ativar sincronização de hash de palavra-passe para os dois motivos seguintes:

* O [os utilizadores com fuga de credenciais](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-risk-events) avisa-o de pares de nome de utilizador e palavra-passe, que tenham sido apresentadas na "web escura". o relatório na gestão do Azure AD Um volume incrível de palavras-passe é perdido por meio de reutilização de phishing, malware e palavra-passe em sites de terceiros que são mais tarde infringido. Microsoft encontra muitas dessas fuga de credenciais e informará a, neste relatório, se corresponderem credenciais na sua organização – mas apenas se [ativar a sincronização de hash de palavra-passe](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization)!
* Em caso de interrupção no local (por exemplo, num ataque de ransomware) poderá mudar para [autenticação de cloud com a sincronização de hash de palavra-passe](https://docs.microsoft.com/azure/security/azure-ad-choose-authn). Este método de autenticação de cópia de segurança, poderá continuar a aceder a aplicações configuradas para a autenticação com o Azure Active Directory, incluindo o Office 365.

Saiba mais sobre como [sincronização de hash de palavra-passe](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization) funciona.

### <a name="implement-ad-fs-extranet-lockout"></a>Bloqueio de extranet implementar AD FS

As organizações, que configura as aplicações para autenticar diretamente para o Azure AD se beneficiar [bloqueio inteligente do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-secure-passwords). Se utilizar o AD FS no Windows Server 2012R2, implementar o AD FS [proteção de bloqueio de extranet](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-soft-lockout-protection). Se utilizar o AD FS no Windows Server 2016, implemente [bloqueio de extranet inteligente](https://support.microsoft.com/en-us/help/4096478/extranet-smart-lockout-feature-in-windows-server-2016). AD FS inteligente Extranet lockout protege contra força bruta ataques, de destino que do AD FS, enquanto impede os utilizadores de que está a ser bloqueada no Active Directory.

### <a name="take-advantage-of-intrinsically-secure-easier-to-use-credentials"></a>Tire partido de intrinsecamente seguro, mais fácil de usar credenciais

Usando [Hello do Windows](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification), pode substituir as palavras-passe com autenticação de dois fatores forte em PCs e dispositivos móveis. Esta autenticação é composta por um novo tipo de credencial do usuário que está associado a um dispositivo e utiliza um biometria ou PIN.

## <a name="step-2---reduce-your-attack-surface"></a>Passo 2 - reduzir a superfície de ataque

Tendo em conta a permeabilidade do comprometimento de palavra-passe, minimizar a superfície de ataque da sua organização é fundamental. Eliminar o uso de protocolos mais antigos e menos seguros, limitando a entrada de acesso a pontos e executamos mais significativo controlo de acesso administrativo aos recursos podem ajudar a reduzir a área de superfície de ataque.

### <a name="block-legacy-authentication"></a>Autenticação de legado do bloco

Aplicações com seus próprios métodos herdados para autenticar com o Azure AD e aceder aos dados da empresa, representar outro risco para as organizações. Exemplos de aplicações que utilizam autenticação herdada são clientes POP3, IMAP4 ou SMTP. As aplicações de autenticação autenticar em nome do utilizador e impedem que ao fazê-lo advanced avaliações de segurança do Azure AD. A autenticação moderna, alternativa, irá reduzir o risco de segurança, uma vez que suporta a autenticação multifator e acesso condicional. Recomendamos as seguintes três ações:

1. Bloco [antigos de autenticação se utilizar o AD FS](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/access-control-policies-w2k12).
2. Programa de configuração [SharePoint Online e Exchange Online, para utilizar a autenticação moderna](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-no-modern-authentication).
3. Uso [políticas de acesso condicional para bloquear antigos de autenticação](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-conditions#legacy-authentication).

### <a name="block-invalid-authentication-entry-points"></a>Pontos de entrada de autenticação inválido de bloco

Com a mentalidade de violação de assumir, deve reduzir o impacto de credenciais de utilizador comprometidas quando eles ocorrem. Para cada aplicação no seu ambiente, considere os casos de uso válidos: os grupos, quais as redes, quais os dispositivos e outros elementos estão autorizados – em seguida, bloquear o resto. Tenha cuidado para restringir a utilização de [contas altamente privilegiadas ou serviços](https://docs.microsoft.com/azure/active-directory/admin-roles-best-practices). Com o [acesso condicional do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal), pode controlar o acesso de utilizadores como autorizado, as aplicações e recursos com base em condições específicas que definir.

Atenção especial a contas de serviço (contas utilizadas para efetuar tarefas de forma automática). Utilizar acesso condicional, pode verificar se essas contas só podem ser executado contra o serviço de IP, e no momento do dia, que é apropriado.

### <a name="implement-azure-ad-privileged-identity-management"></a>Implementar o Azure AD Privileged Identity Management

Outro impacto de "assumir que houve violação" é a necessidade de minimizar a probabilidade de que uma conta comprometida pode operar com uma função com privilégios. 

[O Azure AD Privileged Identity Management (PIM)](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure) ajuda minimizado privilégios de conta ao ajudá-lo:

* Identificar e gerir os utilizadores atribuídos a funções administrativas.
* Compreenda as funções de privilégio não utilizadas ou excessiva, que deve remover.
* Estabeleça as regras para tornar as funções privilegiadas-se de que estão protegidas pelo multi-factor authentication.
* Estabeleça as regras para se certificar de que funções com privilégios são concedidas tempo suficiente para realizar a tarefa com privilégios.

Ativar PIM do Azure AD, em seguida, ver os utilizadores que são atribuídos a funções administrativas e remover contas desnecessárias essas funções. Para os restantes utilizadores com privilégios, movê-los de permanente para elegíveis. Por fim, estabelecer políticas adequadas para garantir que quando precisam de obter acesso a essas funções com privilégios, ela poderá fazer isso com segurança.

Como parte da implementação de seu processo de conta com privilégios, siga os [melhor prática para criar, pelo menos, duas contas de emergência](https://docs.microsoft.com/azure/active-directory/admin-roles-best-practices) para se certificar de que tem acesso ao Azure AD Se preso.

## <a name="step-3---automate-threat-response"></a>Passo 3 - automatizar a resposta de ameaças

O Azure Active Directory tem muitos recursos que automaticamente interceptar ataques, para remover a latência entre a deteção e resposta. Pode reduzir os custos e riscos, quando reduz os criminosos tempo utilizam para incorporar próprios em seu ambiente. Eis os passos concretos que pode tomar.

### <a name="implement-user-risk-security-policy-using-azure-ad-identity-protection"></a>Implementar a política de segurança de risco do utilizador com o Azure AD Identity Protection

Risco de utilizador indica a probabilidade de a identidade do utilizador foi comprometida e é calculada com base na [eventos de risco do utilizador](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) que estão associados com a identidade do utilizador. Uma política de risco do utilizador é uma política de acesso condicional que avalia o nível de risco para um grupo ou utilizador específico. Com base na baixa, média, alta de nível de risco, uma política pode ser configurada para bloquear o acesso ou exigir uma alteração de palavra-passe segura através da autenticação multifator. A Microsoft recomenda exigir uma alteração de palavra-passe segura para os utilizadores em risco elevado.

![Utilizadores marcados em risco](media/azure-ad/azure-ad-sec-steps1.png)

### <a name="implement-sign-in-risk-policy-using-azure-ad-identity-protection"></a>Implementar política de risco de início de sessão com o Azure AD Identity Protection

Início de sessão de risco é a probabilidade de que alguém que não seja o proprietário da conta está a tentar iniciar sessão utilizando a identidade. R [política de risco de início de sessão](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) é uma política de acesso condicional que avalia o nível de risco para um grupo ou utilizador específico. Com base no nível de risco (baixo/médio/alto), uma política pode ser configurada para bloquear o acesso ou forçar autenticação multifator. Certifique-se de que força autenticação multifator no Medium ou acima inícios de sessão de risco.

![Iniciar sessão de IPs anónimo](media/azure-ad/azure-ad-sec-steps2.png)

## <a name="step-4---increase-your-awareness"></a>Passo 4 - aumentar o seu conhecimento

Auditoria e Registro em log de eventos relacionados com a segurança e alertas relacionados são componentes essenciais de uma estratégia de proteção eficiente. Registos de segurança e de relatórios proporcionam um registo eletrónico de atividades suspeitas e ajuda detectar padrões que podem indicar uma tentativa ou com êxito penetração externa da rede e ataques internos. Pode utilizar a auditoria para monitorizar a atividade do utilizador, a conformidade a normas documento, executar análises forenses e muito mais. Os alertas fornecem notificações de eventos de segurança.

### <a name="monitor-azure-ad"></a>Monitor do Azure AD

Funcionalidades e serviços do Microsoft Azure oferecem segurança configuráveis, auditoria e registo de opções para ajudar a identificar lacunas na sua mecanismos e as políticas de segurança e abordar essas lacunas para ajudar a evitar falhas. Pode utilizar [do Azure de registo e auditoria](https://docs.microsoft.com/azure/security/azure-log-audit) e utilizar [relatórios de atividade no portal do Azure Active Directory de auditoria](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs).

### <a name="monitor-azure-ad-connect-health-in-hybrid-environments"></a>Monitorizar o Azure AD Connect Health em ambientes híbridos

[Monitorização do AD FS com o Azure AD Connect Health](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-adfs) fornece-lhe uma visão mais clara potenciais problemas e a visibilidade de ataques na sua infraestrutura de AD FS. O Azure AD Connect Health fornece alertas com detalhes, passos de resolução e ligações para documentação relacionada; análise de utilização do várias métricas relacionadas com o tráfego de autenticação: monitorização de desempenho e relatórios.

![Azure AD Connect Health](media/azure-ad/azure-ad-sec-steps4.png)

### <a name="monitor-azure-ad-identity-protection-events"></a>Monitorizar eventos do Azure AD Identity Protection

[O Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) é uma notificação, monitorização e relatórios a ferramenta que pode utilizar para detetar potenciais vulnerabilidades que afetam as identidades da sua organização. Detetar eventos de risco, como a fuga de credenciais, deslocação impossível, e inícios de sessão de infectado dispositivos, endereços IP anónimos, endereços IP associados à atividade suspeita e localizações desconhecidas. Ative alertas de notificação receber o e-mail de utilizadores em risco e/ou um e-mail de resumo semanal.

![Utilizadores marcados em risco](media/azure-ad/azure-ad-sec-steps3.png)

## <a name="step-5---enable-end-user-self-help"></a>Passo 5 – ativar auto-ajuda de utilizador final

Tanto quanto possível vai querer equilibrar a segurança com a produtividade. Um modo geral de atingir o seu percurso com a mentalidade que estiver a configurar uma base de segurança a longo prazo, pode remover o atrito da sua organização ao capacitando seus usuários ao mesmo tempo restante vigilante. 

### <a name="implement-self-service-password-reset"></a>Implementar a reposição de palavra-passe self-service

Do Azure [reposição de palavra-passe self-service (SSPR)](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started) oferece uma forma simples para os administradores de TI permitir que os utilizadores a repor ou desbloquear as suas palavras-passe ou contas sem intervenção do administrador. O sistema inclui relatórios detalhados que controlam quando os utilizadores acedem ao sistema, juntamente com notificações para o alertar quanto a utilizações indevidas ou abusos. 

### <a name="implement-self-service-group-management"></a>Implementar a gestão de grupos self-service

Azure AD fornece a capacidade para gerir o acesso a recursos através de grupos de segurança e grupos do Office 365. Estes grupos podem ser geridos por proprietários do grupo em vez dos administradores de TI. Conhecido como [gestão de grupos self-service](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management), esse recurso permite que os proprietários do grupo que não estão atribuídos uma função administrativa para criar e gerir grupos sem depender de administradores para processar os pedidos.

### <a name="implement-azure-ad-access-reviews"></a>Revisões de acesso de implementar do Azure AD

Com o [revisões de acesso do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview), pode gerir as associações de grupo, acesso a aplicações empresariais, e estendido de atribuições de funções privilegiadas para se certificar de que mantém uma norma de segurança que não dar acesso aos utilizadores para períodos de hora quando eles não precisam dela.

## <a name="summary"></a>Resumo

Há muitos aspectos de uma infraestrutura de identidade segura, mas esta lista de verificação de cinco etapas irá ajudá-lo a realizar com rapidez uma infraestrutura de identidade segura e mais segura:

* Reforce as suas credenciais.
* Reduza sua área de superfície de ataque.
* Automatize a resposta a ameaças.
* Aumente seu conhecimento sobre auditoria e monitorização.
* Ative mais previsível e completa pelos usuários finais segurança com a ajuda autónoma.

Agradecemos muito a sério como levar a segurança de identidade e Espero que este documento é um Roteiro úteis para uma postura mais segura para a sua organização.

## <a name="next-steps"></a>Passos Seguintes
Se precisar de assistência para planear e implementar as recomendações, consulte a [planos de implantação do projeto do Azure AD](http://aka.ms/deploymentplans) para obter ajuda.
