---
title: Escolha o método de autenticação adequado para a sua solução de identidade híbrida do Azure AD | Microsoft Docs
description: Este guia destina-se a ajudar CEOs, CIOs, CISOs, diretor Arquitetos de identidade, Arquitetos de Enterprise e segurança e IT decisores responsáveis por escolher um método de autenticação para a sua solução de identidade híbrida do Azure AD em média a grande organizações.
services: active-directory
keywords: ''
author: martincoetzer
ms.author: martincoetzer
ms.date: 04/12/2018
ms.topic: article
ms.service: active-directory
ms.workload: identity
ms.openlocfilehash: eb5fb008e602c2026e57d3436875ec485b350af8
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/08/2018
---
# <a name="choosing-the-right-authentication-method-for-your-azure-active-directory-hybrid-identity-solution"></a>Escolher o método de autenticação adequado para a sua solução de identidade híbrida do Azure Active Directory 

Este artigo é o primeiro de uma série de artigos para ajudar as organizações implementam uma conclusão solução de identidade híbrida do Azure AD. Uma conclusão solução de identidade híbrida do Azure AD foi descrita como o Framework de transformação Digital identidade híbrida e cobre os resultados de empresas e organizações de objetivos devem focar-se para se certificar de que tem implementado uma identidade híbrida robusto e segura solução. O resultado de negócio primeiro do framework spells enviados os requisitos para as organizações a proteger o processo de autenticação quando os utilizadores acedem a aplicações na nuvem. O primeiro objetivo de negócio no resultado de negócio protegidos a autenticação é a capacidade dos utilizadores iniciar sessão com o respetivo no local nomes de utilizador e palavras-passe de aplicações em nuvem. Este processo de início de sessão e a forma como autenticam os utilizadores possibilita tudo na nuvem.

Escolher o método de autenticação correto é o primeiro lugar para as organizações que pretenda mover as suas aplicações para a nuvem. Esta decisão não deve ser executada ligeiramente pelos seguintes motivos:

1. É a primeira decisão para uma organização que pretenda mover para a nuvem. 

2. O método de autenticação é um componente fundamental da presença de uma organização na nuvem, controlar o acesso a todos os dados em nuvem e recursos.

3. É a base de todos os outros segurança avançada e funcionalidades de experiência de utilizador no Azure AD.

4. Dificuldade em ao alterar o método de autenticação implementado uma vez.

Com a identidade como o novo plane de controlo de segurança de TI, a autenticação é a proteção de acesso de uma organização para o novo universo de nuvem. As organizações devem certificar-se de que o plane de controlo de identidade, faz com que a segurança dos mesmos mais forte e manter as suas aplicações em nuvem das intrusos.

### <a name="out-of-scope"></a>Fora do âmbito

As organizações que não tenham um requisitos de espaço de diretório no local existentes não são o foco deste artigo. Normalmente, essas empresas criam identidades apenas na nuvem, que não necessita de uma solução de identidade híbrida. Identidades apenas na nuvem existem apenas na nuvem e não associados a correspondente identidades no local.  

## <a name="choosing-the-right-authentication-method"></a>Escolher o método de autenticação correto

Com a solução de identidade do híbrida do Azure AD como o novo plane de controlo, a autenticação é a base de acesso à nuvem. Escolher o método de autenticação correto é uma primeira decisão fundamental na configuração de uma solução de identidade híbrida do Azure AD. Implementar o método de autenticação está configurado com o Azure AD Connect, que também aprovisionar utilizadores na nuvem. 

Para escolher um método de autenticação, tem de considerar o tempo, a infraestrutura existente, a complexidade e o custo de implementação à sua escolha. Estes fatores são diferentes para cada organização e podem ser alterados ao longo do tempo. 

AD do Azure suporta os seguintes métodos de autenticação de soluções de identidade híbrida:

### <a name="cloud-authentication"></a>Autenticação em nuvem
Ao escolher este método de autenticação, o Azure AD processa o processo de início de sessão para os utilizadores. Conjugados com totalmente integrada-início de sessão único (SSO), os utilizadores podem iniciar sessão aplicações em nuvem sem ter de reintroduzir as suas credenciais. Com a autenticação em nuvem pode escolher entre duas opções: 

**Sincronização de Hash de palavra-passe (PHS)** – a forma mais simples para ativar a autenticação para objetos de diretório no local no Azure AD. Sincronização de Hash de palavra-passe permite que os utilizadores utilizem o mesmo nome de utilizador e palavra-passe que utilizam no local sem ter de implementar quaisquer infraestruturas adicionais. Algumas funcionalidades premium do Azure AD, como a proteção de identidade, necessita de sincronização de Hash de palavra-passe, independentemente de qual o método de autenticação está selecionado.

> [!NOTE] 
> As palavras-passe nunca são armazenadas em texto não encriptado ou encriptadas com um algoritmo reversível no Azure AD. Para obter mais informações sobre o processo real de sincronização de Hash de palavra-passe, consulte [implementar a sincronização de hash de palavra-passe com a sincronização do Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-synchronization). 

**Autenticação pass-through (PTA)** – fornece uma validação de palavra-passe simples para serviços de autenticação do Azure AD através de um agente de software em execução num ou mais servidores no local para validar os utilizadores diretamente com o Active Directory no local Não ocorre assegurar que a validação da palavra-passe de diretório na nuvem. Estados de empresas com um requisito de segurança para impor imediatamente a conta de utilizador no local, as políticas de palavra-passe e horas de início de sessão que pretende utilizar este método de autenticação. Para obter mais informações sobre o processo de autenticação pass-through real, consulte [utilizador inicie sessão com a autenticação pass-through do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication).

### <a name="federated-authentication"></a>Autenticação federada
Quando escolher este método de autenticação às mãos do Azure AD desativar o processo de autenticação para um sistema de autenticação fidedigna separado, por exemplo, um local Serviços do Active Directory Federação (AD FS) para validar a palavra-passe do utilizador. O sistema de autenticação pode fornecer os requisitos de autenticação adicionais, tais como a autenticação baseada em smart card ou uma autenticação multifator de terceiros. Para obter mais informações, consulte [implementar serviços de Federação do Active Directory](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/windows-server-2012-r2-ad-fs-deployment-guide).

A secção seguinte irá ajudá-lo a decidir qual o método de autenticação é adequado para si, através de uma árvore de decisões. Irá ajudá-lo a determinar se pretende implementar na nuvem ou autenticação federada da sua solução de identidade híbrida do Azure AD.

## <a name="azure-ad-authentication-decision-tree"></a>Árvore de decisões de autenticação do Azure AD

![image1](media/azure-ad/azure-ad-authn-image1.png)

## <a name="detailed-considerations-on-authentication-methods"></a>Obter considerações detalhadas nos métodos de autenticação

### <a name="cloud-authentication-password-hash-sync"></a>Autenticação em nuvem: sincronização de Hash de palavra-passe 

* **Esforço:** sincronização de Hash de palavra-passe requer o esforço do utilizador, pelo menos, relativamente à implementação, a manutenção e a infraestrutura para organizações que só tem de permitir que os utilizadores iniciar sessão no Office 365, aplicações SaaS e outros recursos do Azure baseada no AD. Quando ativada, a sincronização de Hash de palavra-passe faz parte do processo de sincronização do Azure AD Connect e é executada a cada dois minutos. 

* **Experiência de utilizador:** recomenda-se que as organizações implementam totalmente integrada-início de sessão único (SSO) com sincronização de Hash de palavra-passe para melhorar a experiência de início de sessão do utilizador evitando desnecessários pede uma vez que tem sessão iniciada.

* **Cenários de avançadas:** se escolher organizações, é possível utilizar conhecimentos aprofundados sobre identidades com relatórios do Azure AD Identity Protection, tais como o relatório de credenciais obtidas ilicitamente. Windows Hello para empresas é outra opção que tenha [requisitos específicos quando utilizar a sincronização de Hash de palavra-passe](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification). As organizações a necessidade de multi-factor authentication com sincronização de Hash de palavra-passe tem de utilizar a autenticação multifator do Azure AD e não é possível utilizar uma linha de terceiros ou métodos de autenticação multifator no local.

* **Continuidade do negócio:** sincronização de Hash de palavra-passe está inerentemente altamente disponível como um serviço em nuvem que dimensiona para todos os centros de dados do Microsoft. Recomenda-se que um segundo servidor do Azure AD Connect é implementado no modo numa configuração em modo de espera para fins de recuperação após desastre de teste.

* **Considerações:** sincronização de Hash de palavra-passe não impor imediatamente as alterações nos Estados de conta no local atualmente. Nesta situação, um utilizador terá acesso a aplicações em nuvem até que o estado da conta de utilizador está sincronizado para o Azure AD. Se pretenderem que as organizações a ultrapassar esta limitação, recomenda-se um novo ciclo de sincronização está ativado depois dos administradores em massa atualizações aos Estados de conta de utilizador no local, como desativar as contas. 

> [!NOTE] 
> A palavra-passe expirou e conta bloqueada Estados não são actualmente sincronizado com o Azure AD com o Azure AD Connect. 

Consulte [implementar a sincronização de Hash de palavra-passe](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-synchronization) para os passos de implementação.

### <a name="cloud-authentication-pass-through-authentication"></a>Autenticação em nuvem: autenticação pass-through  

* **Esforço:** para autenticação pass-through, precisa de uma ou mais (três recomendado) simples agentes instalados em servidores existentes com acesso ao seu local no Active Directory Domain Services, incluindo o acesso ao seu local domínio AD controladores. Estes agentes tem acesso à Internet de saída e têm acesso aos controladores de domínio. Por este motivo, não é suportada para implementar os agentes numa rede de perímetro, porque requer acesso à rede para os controladores de domínio. Todo o tráfego de rede é encriptado e é limitado a pedidos de autenticação. Para obter mais informações sobre este processo, consulte o [detalhada da segurança](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-security-deep-dive) autenticação pass-through.

* **Experiência de utilizador:** recomenda-se que as organizações implementam totalmente integrada-início de sessão único com autenticação pass-through para melhorar a experiência de início de sessão do utilizador evitando desnecessários pede uma vez que tem sessão iniciada.

* **Cenários de avançadas:** autenticação pass-through impõe a política de conta no local no momento de início de sessão. Por exemplo, acesso negado, quando a conta de um utilizador local no estado estiver desativado, bloqueada, [palavra-passe expirou](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-faq#what-happens-if-my-users-password-has-expired-and-they-try-to-sign-in-by-using-pass-through-authentication ) ou fica fora do utilizador permitidas horas de início de sessão.  As organizações a necessidade de multi-factor authentication com autenticação pass-through devem utilizar o Azure multi-factor Authentication (MFA) e não é possível utilizar um método de autenticação multifator de terceiros ou no local. Funcionalidades avançadas, tais como o relatório de credenciais obtidas ilicitamente de Identity Protection requer a sincronização de Hash de palavra-passe é independentemente se optar por autenticação pass-through.

* **Continuidade do negócio:** é recomendável implementar dois agentes de autenticação extra pass-through, para além do primeiro de agentes no servidor do Azure AD Connect para garantir a disponibilidade elevada de pedidos de autenticação. Quando tiver três agentes implementados, um agente ainda pode falhar quando outro agente está inativo para manutenção. Outra vantagem de implementar a sincronização de Hash de palavra-passe para além de autenticação pass-through, é que possa atuar como método de cópia de segurança de autenticação quando o método de autenticação principal já não está disponível, por exemplo, quando os servidores no local não estão disponíveis.

* **Considerações:** se utilizar a sincronização de Hash de palavra-passe como um método de autenticação de cópia de segurança para autenticação pass-through e os agentes não é possível validar as credenciais do utilizador, em seguida, a ativação pós-falha para a sincronização de Hash de palavra-passe não ocorrem automaticamente. Terá de mudar o método de início de sessão manualmente utilizando o Azure AD Connect. A autenticação pass-through só suporta aplicações em nuvem que utilizam autenticação moderna e protocolos específicos de Exchange Online, como o ActiveSync, POP3 e IMAP4. Por exemplo, [Microsoft Office 2013 e posterior suporta a autenticação moderna, mas versões anteriores não](https://blogs.office.com/en-us/2015/11/19/updated-office-365-modern-authentication-public-preview/) em obter mais informações de suporte de aplicações do Office. Consulte [perguntas mais frequentes](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-faq) e outras considerações na autenticação pass-through, incluindo ID alternativo suportam.

Consulte [implementar a autenticação pass-through](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication) para os passos de implementação.

### <a name="federated-authentication"></a>Autenticação federada

* **Esforço:** a utilizar um sistema de autenticação federada dependem de um sistema externo para autenticar os utilizadores. Algumas empresas pretendem reutilizar o seu investimento de sistema federado existente com a respetiva solução de identidade híbrida do Azure AD. A manutenção e gestão do sistema federado está fora do controlo do Azure AD. Trata-se até a organização utilizar o sistema federado para se certificar de que é implementado com segurança e pode processar a carga de autenticação. 

* **Experiência de utilizador:** a experiência de utilizador de autenticação federada está dependente da implementação de funcionalidades, a topologia e a configuração do farm de Federação. Algumas organizações requerem esta flexibilidade para adaptar e configurar o acesso ao farm de Federação de acordo com os seus requisitos de segurança. Por exemplo, é possível configurar dispositivos e utilizadores internamente ligados para iniciar sessão dos utilizadores automaticamente, sem pedir aos mesmos credenciais, porque estes tiverem já sessão iniciada para os respetivos dispositivos. Por outro lado, se necessário, algumas funcionalidades de segurança avançada podem tornar iniciar sessão no processo o utilizador mais difícil.

* **Avançadas cenários:** Federated solução de autenticação é normalmente necessária quando os clientes têm um requisito de autenticação nativamente não suportado pelo Azure AD, informações detalhadas [listados aqui](https://blogs.msdn.microsoft.com/samueld/2017/06/13/choosing-the-right-sign-in-option-to-connect-to-azure-ad-office-365/), mas comuns os requisitos incluem:

    * A necessidade de smart cards ou certificados de autenticação
    * Utilizar no local servidor MFA ou o fornecedor de multi-factor de terceiros.
    * Autenticação através da solução de autenticação de terceiros. Consulte o [lista de compatibilidades de Federação do Azure AD](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-federation-compatibility).
    * Os utilizadores tem de iniciar sessão com as respetivas sAMAccountName, por exemplo, domínio ome de utilizador, em vez de utilizar um utilizador nome Principal (UPN), por exemplo, user@domain.com

* **Continuidade do negócio:** Federated sistemas necessitam normalmente de uma carga equilibrada matriz de servidores, também conhecido como um farm, configurada numa topologia de rede de perímetro e de rede interna para garantir a elevada disponibilidade para pedidos de autenticação. Sincronização de Hash de palavra-passe pode ser implementada, juntamente com a autenticação de Federated como método de cópia de segurança de autenticação quando o método de autenticação principal já não está disponível, por exemplo, quando os servidores no local não estão disponíveis. Algumas organizações grande empresa requerem uma solução de Federação para suportar vários pontos de entrada de Internet configurados com georreplicação-DNS para pedidos de autenticação de latência baixa.

* **Considerações:** Federated sistemas normalmente requerem um investimento significativo mais na infraestrutura no local. A maioria das organizações Escolha esta opção se já tiverem um investimento de Federação no local e é um requisito de negócio forte para utilizar um fornecedor de identidade única. Federação é mais complexo para operar e resolução de problemas em comparação com soluções de autenticação na nuvem. Utilizar IDs de utilizador com um domínio não encaminhável que não pode ser verificado no Azure AD para iniciar a configuração no need adicional para implementar. Este requisito é conhecido como início de sessão alternativo suporte de ID. Consulte [configurar ID de início de sessão alternativo](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configuring-alternate-login-id) para requisitos e limitações. Se optar por utilizar um fornecedor de autenticação multifator de terceiros com a Federação, certifique-se que o fornecedor suporta WS-Trust, para permitir que os dispositivos de associação do Azure AD.

Consulte [implementar os serviços de Federação](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/deploying-federation-servers) para os passos de implementação.

> [!NOTE] 
> Ao implementar a solução de identidade híbrida do Azure AD, certifique-se de que implementa uma das topologias suportadas do Azure AD Connect. Saiba mais sobre suportam e não suportadas configurações em [topologias do Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-topologies).

## <a name="architecture-diagrams"></a>Diagramas de arquitetura

Os seguintes destaques de diagrama os componentes de arquitetura de alto nível necessários para cada método de autenticação que pode utilizar com a sua solução de identidade híbrida do Azure AD. Disponibiliza uma descrição geral para comparar as diferenças entre as soluções.

O diagrama a seguir descreve a simplicidade de uma solução de sincronização de Hash de palavra-passe:

![PHS](media/azure-ad/azure-ad-authn-image2.png)

O diagrama a seguir descreve os requisitos de agente de autenticação pass-through:

![PTA](media/azure-ad/azure-ad-authn-image3.png)

O diagrama a seguir descreve os componentes necessários para a Federação no perímetro e rede interna da sua organização:

![ADFS](media/azure-ad/azure-ad-authn-image4.png)

## <a name="comparing-the-authentication-methods"></a>Comparar os métodos de autenticação

|Consideração|Sincronização de Hash de palavra-passe + SSO totalmente integrada|A autenticação pass-through + SSO totalmente integrada|Federação com o AD FS|
|:-----|:-----|:-----|:-----|
|Onde acontecer autenticação?|Na nuvem|Na nuvem, após uma troca de verificação de palavra-passe segura com o agente de autenticação no local|Local|
|Quais são os requisitos de servidor no local para além do sistema de aprovisionamento: o Azure AD Connect?|Nenhuma|1 server para cada agente de autenticação adicional|2 ou mais servidores AD FS<br>2 ou mais servidores do WAP na rede de perímetro/DMZ|
|Quais são Internet no local e requisitos, para além do sistema de aprovisionamento de rede?|Nenhuma|[Acesso de Internet de saída](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-quick-start) dos servidores a executar agentes de autenticação|[Acesso à Internet de entrada](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/overview/ad-fs-requirements) para os servidores do WAP no perímetro<br>Acesso à rede de entrada para servidores do AD FS dos servidores WAP no perímetro<br>Balanceamento de carga de rede|
|Existe um requisito do certificado SSL?|Não|Não|Sim|
|Existe um solução de monitorização de estado de funcionamento?|Não necessário|Estado do agente fornecido pelo [Centro de administração do Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-troubleshoot-pass-through-authentication)|[Azure AD Connect Health](https://docs.microsoft.com/en-us/azure/active-directory/connect-health/active-directory-aadconnect-health-adfs)|
|Os utilizadores receber o início de sessão único aos recursos de nuvem de dispositivos associados a um domínio na rede da empresa?|Sim com [SSO totalmente integrada](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-sso)|Sim com [SSO totalmente integrada](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-sso)|Sim|
|São suportados os tipos de início de sessão?|UserPrincipalName + palavra-passe<br>Utilizando a autenticação integrada do Windows [SSO totalmente integrada](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-sso)<br>[ID de início de sessão alternativo](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-get-started-custom)|UserPrincipalName + palavra-passe<br>Utilizando a autenticação integrada do Windows [SSO totalmente integrada](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-sso)<br>[ID de início de sessão alternativo](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-faq)|UserPrincipalName + palavra-passe<br>sAMAccountName + palavra-passe<br>Autenticação Integrada do Windows<br>[Autenticação de certificados e Smart Card](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/configure-user-certificate-authentication)<br>[ID de início de sessão alternativo](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/configuring-alternate-login-id)|
|É o Windows Hello para empresas suportadas?|[Modelo de confiança de chave](https://docs.microsoft.com/en-us/windows/security/identity-protection/hello-for-business/hello-identity-verification)<br>[Modelo de confiança de certificados com o Intune](https://blogs.technet.microsoft.com/microscott/setting-up-windows-hello-for-business-with-intune/)|[Modelo de confiança de chave](https://docs.microsoft.com/en-us/windows/security/identity-protection/hello-for-business/hello-identity-verification)<br>[Modelo de confiança de certificados com o Intune](https://blogs.technet.microsoft.com/microscott/setting-up-windows-hello-for-business-with-intune/)|[Modelo de confiança de chave](https://docs.microsoft.com/en-us/windows/security/identity-protection/hello-for-business/hello-identity-verification)<br>[Modelo de confiança de certificados](https://docs.microsoft.com/en-us/windows/security/identity-protection/hello-for-business/hello-key-trust-adfs)|
|Quais são as opções de autenticação multifator?|[Azure MFA](https://docs.microsoft.com/en-us/azure/multi-factor-authentication/)|[Azure MFA](https://docs.microsoft.com/en-us/azure/multi-factor-authentication/)|[Azure MFA](https://docs.microsoft.com/en-us/azure/multi-factor-authentication/)<br>[Servidor MFA do Azure](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy)<br>[MFA de terceiros](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/configure-additional-authentication-methods-for-ad-fs)|
|Os Estados de conta de utilizador são suportados?|Contas desativadas<br>(até o atraso de 30 minutos)|Contas desativadas<br>Conta bloqueada<br>Palavra-passe expirada<br>Horas de início de sessão|Contas desativadas<br>Conta bloqueada<br>Palavra-passe expirada<br>Horas de início de sessão|
|Quais são as opções de acesso condicional?|[Acesso condicional do Azure AD ](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access-azure-portal)|[Acesso condicional do Azure AD ](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access-azure-portal)|[Acesso condicional do Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access-azure-portal)<br>[Regras de afirmações do AD FS](https://adfshelp.microsoft.com/AadTrustClaims/ClaimsGenerator)|
|Está a bloquear protocolos legados suportados?|Não|Não|[Sim](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/access-control-policies-w2k12)|
|Pode personalizar o logótipo, a imagem e a descrição das páginas de início de sessão?|[Sim, com o Azure AD Premium](https://docs.microsoft.com/en-us/azure/active-directory/customize-branding)|[Sim, com o Azure AD Premium](https://docs.microsoft.com/en-us/azure/active-directory/customize-branding)|[Sim](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-federation-management#customlogo)|
|Os cenários avançados são suportados?|[Bloqueio de palavra-passe inteligente](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-secure-passwords)<br>[Fuga relatórios de credenciais](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-risk-events)|[Bloqueio de palavra-passe inteligente](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-smart-lockout)|Sistema de autenticação de latência baixa multilocal<br>[Bloqueio de extranet do AD FS](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-lockout-protection)<br>[Integração com os sistemas de identidade de terceiros](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-federation-compatibility)|

## <a name="recommendations-and-considerations-from-azure-ad"></a>Recomendações e considerações do Azure AD

O sistema de identidade garante que os utilizadores têm acesso a aplicações e aplicações de linha de negócio migrar e disponibilizar na nuvem da nuvem. Autenticação controla o acesso a aplicações, para manter os utilizadores autorizados produtivos e mal-intencionadas fora os dados confidenciais da sua organização. Por este motivo, considere as seguintes recomendações no escolher o método de autenticação adequado para a sua organização. 

Utilizar ou ativar a sincronização de Hash de palavra-passe, independentemente de qual o método de autenticação que escolher, pelos seguintes motivos:

1. **Elevada disponibilidade e recuperação após desastre:** autenticação pass-through e a Federação baseiam-se na infraestrutura no local. Para a autenticação pass-through, os requisitos de espaço no local incluem o hardware de servidor e de rede que necessitam dos agentes de autenticação pass-through. Para a Federação, os requisitos de espaço no local é ainda maiores, porque necessita de servidores na sua rede de perímetro para pedidos de autenticação de proxy e os servidores de Federação interno. Para evitar pontos únicos de falhas, organização deve implementar servidores redundantes para garantir que os pedidos de autenticação são sempre servidos se falhar qualquer componente. Autenticação pass-through e a Federação também são nos controladores de domínio para responder a pedidos de autenticação, que também podem falhar. Muitos destes componentes tem de manutenção para manter o bom estado de funcionamento e falhas estejam mais predispostas quando a manutenção não planeada e implementada corretamente. Falhas podem ser evitadas, utilizando a sincronização de Hash de palavra-passe porque o serviço de autenticação do Microsoft Azure AD cloud dimensiona global e está sempre disponível.

2. **No local sobrevivência de indisponibilidade:** as consequências de indisponibilidade no local devido a um ataque informático ou desastre podem ser significativas, vão de danos de marca reputational a uma organização paralyzed não é possível lidar com o ataque. No último ano, muitas organizações foram victims de ataques de software maligno, incluindo o ransomware destino, o que causou os seus servidores no local inativo. No que ajuda os clientes lidar com estes tipos de ataques, Microsoft reparado duas categorias de organizações:

   a. As organizações que anteriormente ativado a sincronização de Hash de palavra-passe foi novamente online num fim de horas alterando o respetivo método de autenticação a utilizar a sincronização de Hash de palavra-passe. Utilizar o acesso ao e-mail através do Office 365, pode funcionar para resolver problemas e tem acesso a outras cargas de trabalho baseado na nuvem.

   b. As organizações que anteriormente não ativar a sincronização de Hash de palavra-passe que tiveram de recorrer a sistemas de e-mail não fidedignos consumidor externo para as comunicações e resolução de problemas. Nesses casos, demorou-los semanas ou mais para ser novamente em funcionamento.

3. **Proteção de identidade:** é uma das formas melhor para proteger os utilizadores na nuvem do Azure AD Identity Protection. Microsoft analisa continuamente da Internet para o utilizador e palavra-passe listas, mal-intencionadas propor e disponibilizar na web um escuro. Azure AD pode utilizar estas informações para verificar se qualquer um dos nomes de utilizador e palavras-passe na sua organização sejam comprometidos. Consequentemente, é fundamental para ativar a sincronização de Hash de palavra-passe, independentemente da que método de autenticação a utilizar, se de que é autenticação federada ou pass-through. As credenciais obtidas ilicitamente são apresentadas como um relatório e podem ser utilizadas para bloquear ou forçar a um utilizador para alterar a palavra-passe quando tentar iniciar sessão com uma palavra-passe obtida ilicitamente.

Por último, em conformidade com a [Gartner](https://info.microsoft.com/landingIAMGartnerreportregistration.html), a Microsoft tem o conjunto mais completo de funções de gestão de acesso e identidade. Os identificadores de Microsoft [pedidos de autenticação de mil milhões de 450](https://www.microsoft.com/en-us/security/intelligence-report) todos os meses para fornecer acesso a milhares de aplicações SaaS, como o Office 365 a partir de praticamente qualquer dispositivo. 

## <a name="conclusion"></a>Conclusão

No destaques neste artigo várias opções de autenticação, as organizações podem configurar e implementar para suportar o acesso a aplicações em nuvem. Para cumprir vários business, segurança e requisitos técnicos, as organizações podem escolher entre a sincronização de Hash de palavra-passe, autenticação pass-through e a Federação. Com cada método de autenticação, a organização pode escolher se os requisitos de negócio são tratados pelo esforço do utilizador para implementar a solução e a experiência de utilizador do processo de início de sessão. Também terá de avaliar se a sua organização necessita de funcionalidades de continuidade de negócio de cada método de autenticação e cenários avançados. Por fim, terá de avaliar as considerações de cada método de autenticação para ver se qualquer impedem que implementar à sua escolha.

## <a name="next-steps"></a>Passos Seguintes

No mundo de hoje, ameaças estão presentes 24 horas por dia em provenientes everywhere. O método de autenticação correto de implementação irá ajudar a mitigar os riscos de segurança e proteger as identidades. 

[Introdução ao](https://docs.microsoft.com/azure/active-directory/get-started-azure-ad) com o Azure AD e implementar a solução de autenticação adequado para a sua organização.

Se estiver a considerar a migrar do federado para autenticação em nuvem, saiba mais [sobre o método de início de sessão alterar](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-user-signin#changing-the-user-sign-in-method). Para ajudar a planear e implementar a migração, pode utilizar [estes planos para ajudá-lo do projeto](http://aka.ms/deploymentplans).
