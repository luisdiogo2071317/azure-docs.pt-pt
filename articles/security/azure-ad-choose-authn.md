---
title: Escolha o método de autenticação adequado para a sua solução de identidade híbrida do Azure AD | Microsoft Docs
description: Este guia ajuda-o CEOs, CIOs, CISOs, diretor Arquitetos de identidade, Arquitetos de Enterprise e segurança e IT decisores responsáveis por escolher um método de autenticação para a sua solução de identidade híbrida do Azure AD no médias e grandes empresas.
services: active-directory
keywords: ''
author: martincoetzer
ms.author: martincoetzer
ms.date: 04/12/2018
ms.topic: article
ms.service: active-directory
ms.workload: identity
ms.openlocfilehash: 7e2eb9bad8c95e61d5b750cacd0724fc2536f2c5
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36294114"
---
# <a name="choose-the-right-authentication-method-for-your-azure-active-directory-hybrid-identity-solution"></a>Escolha o método de autenticação adequado para a sua solução de identidade híbrida do Azure Active Directory 

Este artigo começa uma série de artigos que ajudar as organizações a implementar uma solução de identidade híbrida de Azure Active Directory (Azure AD) completa. Esta solução foi descrita como o [Framework de transformação Digital identidade híbrida](https://aka.ms/aadframework). Aborda os resultados de empresas e organizações de objetivos podem concentrar-se em para implementar uma solução de identidade híbrida robusto e seguro. 

O resultado de negócio primeiro do framework spells enviados os requisitos para as organizações a proteger o processo de autenticação quando os utilizadores acedem a aplicações na nuvem. O objetivo de negócio primeiro no resultado de negócio protegidos a autenticação é a capacidade dos próprios utilizadores para iniciar sessão em aplicações em nuvem, utilizando os respetivos nomes de utilizador no local e palavras-passe. Este processo de início de sessão para e a forma como os utilizadores autenticam fazer tudo na nuvem possíveis.

Escolher o método de autenticação correto é o primeiro lugar para as organizações que pretenda mover as suas aplicações para a nuvem. Não esta decisão ânimo leve, pelos seguintes motivos:

1. É a primeira decisão de uma organização que pretende mover para a nuvem. 

2. O método de autenticação é um componente fundamental da presença de uma organização na nuvem. Controla o acesso a todos os dados em nuvem e recursos.

3. É a base de todos os outros segurança avançada e funcionalidades de experiência de utilizador no Azure AD.

4. O método de autenticação é difícil alterar após estar implementada.

A identidade é o novo plane de controlo de segurança de TI. Por isso, a autenticação é a proteção de acesso de uma organização para o novo universo de nuvem. As organizações precisam de um plane de controlo de identidade que strengthens a segurança dos mesmos e mantém as aplicações de nuvem segura de intrusos.

### <a name="out-of-scope"></a>Fora do âmbito
As organizações que não tenham um requisitos de espaço de diretório no local existentes não são o foco deste artigo. Normalmente, essas empresas criam identidades apenas na nuvem, que não necessita de uma solução de identidade híbrida. Identidades apenas na nuvem existem apenas na nuvem e não estão associadas a correspondente identidades no local.

## <a name="authentication-methods"></a>Métodos de autenticação
Quando a solução de identidade híbrida do Azure AD é o novo plane de controlo, a autenticação é a base de acesso à nuvem. Escolher o método de autenticação correto é uma primeira decisão fundamental na configuração de uma solução de identidade híbrida do Azure AD. Implemente o método de autenticação que é configurado através do Azure AD Connect, o que também Aprovisiona utilizadores na nuvem.

Para escolher um método de autenticação, tem de considerar o tempo, a infraestrutura existente, a complexidade e o custo de implementação à sua escolha. Estes fatores são diferentes para cada organização e podem ser alterados ao longo do tempo. 

>[!VIDEO https://www.youtube.com/embed/YtW2cmVqSEw]

AD do Azure suporta os seguintes métodos de autenticação de soluções de identidade híbrida.

### <a name="cloud-authentication"></a>Autenticação em nuvem
Ao escolher este método de autenticação, o Azure AD processa processo de início de sessão dos utilizadores. Conjugados com totalmente integrada-início de sessão único (SSO), os utilizadores podem iniciar sessão aplicações em nuvem sem ter de reintroduzir as suas credenciais. Com a autenticação em nuvem, pode escolher entre duas opções: 

**Sincronização de hash de palavra-passe do Azure AD**. A forma mais simples para ativar a autenticação para objetos de diretório no local no Azure AD. Os utilizadores podem utilizar o mesmo nome de utilizador e palavra-passe que utilizam no local sem ter de implementar quaisquer infraestruturas adicionais. Algumas funcionalidades premium do Azure AD, como Identity Protection, requerem a sincronização de hash de palavra-passe para, independentemente de qual o método de autenticação que escolher.

> [!NOTE] 
> As palavras-passe nunca são armazenadas em texto não encriptado ou encriptadas com um algoritmo reversível no Azure AD. Para obter mais informações sobre o processo real de sincronização de hash de palavra-passe, consulte [implementar a sincronização de hash de palavra-passe com a sincronização do Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-synchronization). 

**Autenticação do Azure AD pass-through**. Fornece uma validação de palavra-passe simples para serviços de autenticação do Azure AD através da utilização de um agente de software que é executado num ou mais servidores no local. Os servidores de validam os utilizadores diretamente com o local no Active Directory, que garante que a validação da palavra-passe não acontecer na nuvem. 

Estados de políticas de palavra-passe de conta de empresas com um requisito de segurança para impor imediatamente utilizadores no local e horas de início de sessão, poderão utilizar este método de autenticação. Para obter mais informações sobre o processo de autenticação pass-through real, consulte [utilizador inicie sessão com a autenticação pass-through do Azure AD](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication).

### <a name="federated-authentication"></a>Autenticação federada
Ao escolher este método de autenticação, às mãos do Azure AD desativado o processo de autenticação para um sistema de autenticação fidedigna separado, como serviços de Federação do Active Directory do local (AD FS), para validar a palavra-passe do utilizador.

O sistema de autenticação pode fornecer os requisitos de autenticação avançadas adicionais. Os exemplos são a autenticação baseada em smart card ou de terceiros a autenticação multifator. Para obter mais informações, consulte [implementar serviços de Federação do Active Directory](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/windows-server-2012-r2-ad-fs-deployment-guide).

A secção seguinte ajuda-o a decidir qual o método de autenticação é adequado para si através de uma árvore de decisões. Ajuda a determinar se pretende implementar na nuvem ou autenticação federada da sua solução de identidade híbrida do Azure AD.

## <a name="decision-tree"></a>Árvore de decisões

![Árvore de decisões de autenticação do Azure AD](media/azure-ad/azure-ad-authn-image1.png)

## <a name="detailed-considerations"></a>Obter considerações detalhadas

### <a name="cloud-authentication-password-hash-synchronization"></a>Autenticação em nuvem: sincronização de hash de palavra-passe

* **Esforço**. Sincronização de hash de palavra-passe requer o esforço do utilizador, pelo menos, relativamente à implementação, a manutenção e a infraestrutura.  Este nível de esforço aplica-se, normalmente, as organizações que precisam apenas os seus utilizadores para iniciar sessão no Office 365, aplicações SaaS e outros recursos do Azure baseada no AD. Quando ativada, a sincronização de hash de palavra-passe faz parte do processo de sincronização do Azure AD Connect e é executada a cada dois minutos.

* **Experiência de utilizador**. Para melhorar a experiência de início de sessão dos utilizadores, implemente SSO totalmente integrado com a sincronização de hash de palavra-passe. SSO totalmente integrado elimina pede desnecessários quando os utilizadores com sessão iniciados.

* **Cenários de avançadas**. Se escolher organizações, é possível utilizar conhecimentos aprofundados sobre identidades com relatórios do Azure AD Identity Protection. Um exemplo é como o relatório de credenciais obtidas ilicitamente. Windows Hello para empresas é outra opção que tenha [requisitos específicos quando utilizar a sincronização de hash de palavra-passe](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification). 

    As organizações que requerem a autenticação multifator com sincronização de hash de palavra-passe tem de utilizar a autenticação multifator do Azure AD. Dessas organizações não é possível utilizar os métodos de autenticação multifator de terceiros ou no local.

* **Continuidade do negócio**. Sincronização de hash de palavra-passe a utilizar com autenticação em nuvem está altamente disponível como um serviço em nuvem que dimensiona para todos os centros de dados do Microsoft. Para garantir que a sincronização de hash de palavra-passe não forem abaixo por períodos prolongados, implemente um segundo servidor do Azure AD Connect no modo numa configuração de modo de espera de teste.

* **Considerações sobre**. Atualmente, a sincronização de hash de palavra-passe não a impor imediatamente as alterações nos Estados de conta no local. Nesta situação, um utilizador tem acesso a aplicações em nuvem até que o estado da conta de utilizador está sincronizado com o Azure AD. As organizações podem querer ultrapassar esta limitação, executando um novo ciclo de sincronização depois dos administradores em massa atualizações aos Estados de conta de utilizador no local. Um exemplo é desativar as contas.

> [!NOTE]
> A palavra-passe expirou e Estados de saída bloqueado conta atualmente não estão sincronizados com o Azure AD com o Azure AD Connect. 

Consulte [implementar a sincronização de hash de palavra-passe](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-synchronization) para os passos de implementação.

### <a name="cloud-authentication-pass-through-authentication"></a>Autenticação em nuvem: autenticação pass-through  

* **Esforço**. Para a autenticação pass-through, precisa de um ou mais (Recomendamos três) simples agentes instalados em servidores existentes. Estes agentes tem de ter acesso ao seu local no Active Directory Domain Services, incluindo no local controladores de domínio do AD. Precisam de acesso à Internet de saída e o acesso aos controladores de domínio. Por este motivo, não é suportada para implementar os agentes numa rede de perímetro. 

    A autenticação pass-through requer acesso à rede para os controladores de domínio. Todo o tráfego de rede é encriptado e é limitado a pedidos de autenticação. Para obter mais informações sobre este processo, consulte o [detalhada da segurança](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-security-deep-dive) autenticação pass-through.

* **Experiência de utilizador**. Para melhorar a experiência de início de sessão dos utilizadores, implemente SSO totalmente integrado com a autenticação pass-through. SSO totalmente integrado elimina pede desnecessários depois dos utilizadores iniciam sessão.

* **Cenários de avançadas**. A autenticação pass-through impõe a política de conta no local no momento de início de sessão. Por exemplo, o acesso é negado quando a conta de um utilizador local no estado estiver desativado, bloqueada, ou [palavra-passe expirou](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-faq#what-happens-if-my-users-password-has-expired-and-they-try-to-sign-in-by-using-pass-through-authentication) ou está fora das horas quando o utilizador tem permissão para iniciar sessão. 

    As organizações que requerem a autenticação multifator com a autenticação pass-through tem de utilizar Azure multi-factor Authentication (MFA). Dessas organizações não é possível utilizar um método de autenticação multifator de terceiros ou no local. Funcionalidades avançadas requerem que a sincronização de hash de palavra-passe está implementada ou não escolha a autenticação pass-through. Um exemplo é o relatório de credenciais obtidas ilicitamente de proteção de identidade.

* **Continuidade do negócio**. Recomendamos que implemente dois agentes de autenticação extra pass-through. Estes extras estão além do primeiro agente no servidor do Azure AD Connect. Esta implementação adicional garante a disponibilidade elevada de pedidos de autenticação. Quando tiver três agentes implementados, um agente ainda pode falhar quando outro agente está inativo para manutenção. 

    Há outra vantagem para implementar a sincronização de hash de palavra-passe para além de autenticação pass-through. Atua como um método de cópia de segurança de autenticação quando o método de autenticação principal já não está disponível.

* **Considerações sobre**. Poderá utilizar a sincronização de hash de palavra-passe como um método de autenticação de cópia de segurança para autenticação pass-through e os agentes não é possível validar as credenciais de um utilizador. Em seguida, a ativação pós-falha para a sincronização de hash de palavra-passe não ocorre automaticamente. Mude manualmente o método de início de sessão utilizando o Azure AD Connect. 

    A autenticação pass-through só suporta aplicações em nuvem que utilizam autenticação moderna e protocolos específicos de Exchange Online. Alguns protocolos são ActiveSync, POP3 e IMAP4. Por exemplo, Microsoft Office 2013 e posterior suporte a autenticação moderna, mas versões anteriores não. Para obter mais informações sobre o suporte de aplicações do Office, consulte [autenticação moderna do Office 365 atualizado](https://blogs.office.com/en-us/2015/11/19/updated-office-365-modern-authentication-public-preview/). Para suportarem outras considerações na autenticação pass-through, incluindo ID alternativo, consulte [perguntas mais frequentes](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-faq).

Consulte [implementar a autenticação pass-through](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication) para os passos de implementação.

### <a name="federated-authentication"></a>Autenticação federada

* **Esforço**. Um sistema de autenticação federada baseia-se no sistema externo fidedigno para autenticar os utilizadores. Algumas empresas pretendem reutilizar o seu investimento de sistema federado existente com a respetiva solução de identidade híbrida do Azure AD. A manutenção e gestão do sistema federado está fora do controlo do Azure AD. É cópias de segurança da organização utilizando o sistema federado para se certificar de que é implementado com segurança e pode processar a carga de autenticação. 

* **Experiência de utilizador**. A experiência de utilizador de autenticação federada depende a implementação de funcionalidades, a topologia e a configuração do farm de Federação. Algumas organizações tem esta flexibilidade para adaptar e configurar o acesso ao farm de Federação de acordo com os seus requisitos de segurança. Por exemplo, é possível configurar dispositivos e utilizadores internamente ligados para iniciar sessão dos utilizadores automaticamente, sem pedir aos mesmos credenciais. Esta configuração funciona porque estes já iniciou sessão para os respetivos dispositivos. Se necessário, algumas funcionalidades de segurança avançada tornam o processo de início de sessão dos utilizadores mais difícil.

* **Cenários de avançadas**. Uma solução de autenticação federada é normalmente necessária quando os clientes têm um requisito de autenticação do Azure AD não suporta nativamente. Ver informações detalhadas para o ajudar a [escolher a opção de início de sessão à direita](https://blogs.msdn.microsoft.com/samueld/2017/06/13/choosing-the-right-sign-in-option-to-connect-to-azure-ad-office-365/). Considere os seguintes requisitos comuns:

    * Autenticação que necessita de smart cards ou certificados.
    * Servidores MFA no local ou fornecedores de terceiros multifator.
    * Autenticação utilizando soluções de autenticação de terceiros. Consulte o [lista de compatibilidades de Federação do Azure AD](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-federation-compatibility).
    * O início de sessão que requer um sAMAccountName, por exemplo, domínio ome de utilizador, em vez de um utilizador nome Principal (UPN), por exemplo, user@domain.com.

* **Continuidade do negócio**. Sistemas federados normalmente necessitam de uma matriz com balanceamento de carga dos servidores, conhecido como um farm. Este farm está configurado numa rede interna e a topologia de rede de perímetro para garantir a elevada disponibilidade para pedidos de autenticação.

    Implemente a sincronização de hash de palavra-passe, juntamente com a autenticação federada como um método de cópia de segurança de autenticação quando o método de autenticação principal já não está disponível. Um exemplo é quando os servidores no local não estão disponíveis. Algumas organizações grande empresa requerem uma solução de Federação para suportar vários pontos de entrada de Internet configurados com georreplicação-DNS para pedidos de autenticação de latência baixa.

* **Considerações sobre**. Sistemas federados normalmente requerem um investimento significativo mais na infraestrutura no local. A maioria das organizações Escolha esta opção se já tiver um investimento de Federação no local. E se for um requisito de negócio forte para utilizar um fornecedor de identidade única. Federação é mais complexo para operar e resolução de problemas em comparação com soluções de autenticação na nuvem.

Para um domínio nonroutable que não pode ser verificado no Azure AD, terá de configuração adicional para implementar a sessão de ID de utilizador. Este requisito é conhecido como início de sessão alternativo suporte de ID. Consulte [configurar ID de início de sessão alternativo](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configuring-alternate-login-id) para requisitos e limitações. Se optar por utilizar um fornecedor de autenticação multifator de terceiros com a Federação, certifique-se de que o fornecedor suporta WS-Trust, para permitir que os dispositivos de associação do Azure AD.

Consulte [implementar servidores de Federação](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/deploying-federation-servers) para os passos de implementação.

> [!NOTE] 
> Ao implementar a solução de identidade híbrida do Azure AD, tem de implementar uma das topologias suportadas do Azure AD Connect. Saiba mais sobre suportam e não suportadas configurações em [topologias do Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-topologies).

## <a name="architecture-diagrams"></a>Diagramas de arquitetura

Os diagramas seguintes descrevem os componentes de arquitetura de alto nível necessários para cada método de autenticação que pode utilizar com a sua solução de identidade híbrida do Azure AD. Fornecem uma descrição geral para o ajudar a comparar as diferenças entre as soluções.

* Simplicidade de uma solução de sincronização de hash de palavra-passe:

    ![Identidade de híbrida do Azure AD com a sincronização de hash de palavra-passe](media/azure-ad/azure-ad-authn-image2.png)

* Requisitos de agente de autenticação pass-through:

    ![Identidade de híbrida do Azure AD com a autenticação pass-through](media/azure-ad/azure-ad-authn-image3.png)

* Componentes necessários para a Federação no perímetro e rede interna da sua organização:

    ![Identidade de híbrida do Azure AD com autenticação federada](media/azure-ad/azure-ad-authn-image4.png)

## <a name="comparing-methods"></a>Métodos de comparação

|Consideração|Sincronização de hash de palavra-passe + SSO totalmente integrada|A autenticação pass-through + SSO totalmente integrada|Federação com o AD FS|
|:-----|:-----|:-----|:-----|
|Onde acontecer autenticação?|Na nuvem|Na nuvem após uma troca de verificação de palavra-passe segura com o agente de autenticação no local|Local|
|Quais são os requisitos de servidor no local para além do sistema de aprovisionamento: o Azure AD Connect?|Nenhuma|Um servidor para cada agente de autenticação adicional|Dois ou mais servidores do AD FS<br><br>Dois ou mais servidores do WAP na rede de perímetro/DMZ|
|Quais são os requisitos para a Internet no local e de rede para além do sistema de aprovisionamento?|Nenhuma|[Acesso de Internet de saída](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-quick-start) dos servidores a executar agentes de autenticação|[Acesso à Internet de entrada](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/overview/ad-fs-requirements) para os servidores do WAP no perímetro<br><br>Acesso à rede de entrada para servidores do AD FS dos servidores WAP no perímetro<br><br>Balanceamento de carga de rede|
|Existe um requisito do certificado SSL?|Não|Não|Sim|
|Existe um solução de monitorização de estado de funcionamento?|Não necessário|Estado do agente fornecido pelo [Centro de administração do Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-troubleshoot-pass-through-authentication)|[Azure AD Connect Health](https://docs.microsoft.com/en-us/azure/active-directory/connect-health/active-directory-aadconnect-health-adfs)|
|Os utilizadores receber o início de sessão único aos recursos de nuvem de dispositivos associados a um domínio na rede da empresa?|Sim com [SSO totalmente integrada](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-sso)|Sim com [SSO totalmente integrada](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-sso)|Sim|
|São suportados os tipos de início de sessão?|UserPrincipalName + palavra-passe<br><br>Autenticação integrada do Windows utilizando [SSO totalmente integrada](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-sso)<br><br>[ID de início de sessão alternativo](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-get-started-custom)|UserPrincipalName + palavra-passe<br><br>Autenticação integrada do Windows utilizando [SSO totalmente integrada](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-sso)<br><br>[ID de início de sessão alternativo](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-faq)|UserPrincipalName + palavra-passe<br><br>sAMAccountName + palavra-passe<br><br>Autenticação Integrada do Windows<br><br>[Autenticação de certificados e smart card](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/configure-user-certificate-authentication)<br><br>[ID de início de sessão alternativo](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/configuring-alternate-login-id)|
|É o Windows Hello para empresas suportadas?|[Modelo de confiança de chave](https://docs.microsoft.com/en-us/windows/security/identity-protection/hello-for-business/hello-identity-verification)<br><br>[Modelo de confiança de certificados com o Intune](https://blogs.technet.microsoft.com/microscott/setting-up-windows-hello-for-business-with-intune/)|[Modelo de confiança de chave](https://docs.microsoft.com/en-us/windows/security/identity-protection/hello-for-business/hello-identity-verification)<br><br>[Modelo de confiança de certificados com o Intune](https://blogs.technet.microsoft.com/microscott/setting-up-windows-hello-for-business-with-intune/)|[Modelo de confiança de chave](https://docs.microsoft.com/en-us/windows/security/identity-protection/hello-for-business/hello-identity-verification)<br><br>[Modelo de confiança de certificados](https://docs.microsoft.com/en-us/windows/security/identity-protection/hello-for-business/hello-key-trust-adfs)|
|Quais são as opções de autenticação multifator?|[Azure MFA](https://docs.microsoft.com/en-us/azure/multi-factor-authentication/)|[Azure MFA](https://docs.microsoft.com/en-us/azure/multi-factor-authentication/)|[Azure MFA](https://docs.microsoft.com/en-us/azure/multi-factor-authentication/)<br><br>[Servidor MFA do Azure](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy)<br><br>[MFA de terceiros](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/configure-additional-authentication-methods-for-ad-fs)|
|Os Estados de conta de utilizador são suportados?|Contas desativadas<br>(até o atraso de 30 minutos)|Contas desativadas<br><br>Conta bloqueada<br><br>Palavra-passe expirada<br><br>Horas de início de sessão|Contas desativadas<br><br>Conta bloqueada<br><br>Palavra-passe expirada<br><br>Horas de início de sessão|
|Quais são as opções de acesso condicional?|[Acesso condicional do Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access-azure-portal)|[Acesso condicional do Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access-azure-portal)|[Acesso condicional do Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access-azure-portal)<br><br>[Regras de afirmações do AD FS](https://adfshelp.microsoft.com/AadTrustClaims/ClaimsGenerator)|
|Está a bloquear protocolos legados suportados?|[Sim](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access-conditions#legacy-authentication)|[Sim](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access-conditions#legacy-authentication)|[Sim](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/access-control-policies-w2k12)|
|Pode personalizar o logótipo, a imagem e a descrição das páginas de início de sessão?|[Sim, com o Azure AD Premium](https://docs.microsoft.com/en-us/azure/active-directory/customize-branding)|[Sim, com o Azure AD Premium](https://docs.microsoft.com/en-us/azure/active-directory/customize-branding)|[Sim](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-federation-management#customlogo)|
|Os cenários avançados são suportados?|[Bloqueio de palavra-passe inteligente](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-secure-passwords)<br><br>[Fuga relatórios de credenciais](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-risk-events)|[Bloqueio de palavra-passe inteligente](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-smart-lockout)|Sistema de autenticação de latência baixa multilocal<br><br>[Bloqueio de extranet do AD FS](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-lockout-protection)<br><br>[Integração com sistemas de identidade de terceiros](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-federation-compatibility)|

## <a name="recommendations"></a>Recomendações
O sistema de identidade garante o acesso dos utilizadores para aplicações na nuvem e as aplicações de linha de negócio que migrar e disponibilizar na nuvem. Para manter os utilizadores autorizados produtivos e mal-intencionadas fora os dados confidenciais da sua organização, autenticação controla o acesso às aplicações.

Utilizar ou ativar a sincronização de hash de palavra-passe para qualquer método de autenticação que escolher, pelos seguintes motivos:

1. **Elevada disponibilidade e recuperação após desastre**. A autenticação pass-through e a Federação baseiam-se na infraestrutura no local. Para a autenticação pass-through, os requisitos de espaço no local incluem o hardware de servidor e a autenticação pass-through de rede necessitam de agentes. Para a Federação, os requisitos de espaço no local é ainda maiores. Requer servidores na sua rede de perímetro para pedidos de autenticação de proxy e os servidores de Federação interno. 

    Para evitar pontos únicos de falhas, implemente servidores redundantes. Em seguida, irão sempre ser servidos pedidos de autenticação se falhar qualquer componente. Autenticação pass-through e a Federação baseiam-se também nos controladores de domínio para responder a pedidos de autenticação, que também podem falhar. Muitos destes componentes tem de manutenção para manter o bom estado de funcionamento. Falhas estejam mais predispostas quando a manutenção não planeada e implementada corretamente. Evite falhas utilizando a sincronização de hash de palavra-passe porque o serviço de autenticação em nuvem do Microsoft Azure AD dimensiona global e está sempre disponível.

2. **No local sobrevivência indisponibilidade**.  As consequências de indisponibilidade no local devido a um ataque informático ou desastre podem ser significativas, vão de danos de marca reputational a uma organização paralyzed não é possível lidar com o ataque. Recentemente, muitas organizações foram victims de ataques de software maligno, incluindo o ransomware destino, o que causou os seus servidores no local Ir para baixo. Quando a Microsoft ajuda os clientes a lidar com estes tipos de ataques, este verá duas categorias de organizações:

   * As organizações que ativou anteriormente sincronização de hash de palavra-passe alterada respetivo método de autenticação a utilizar a sincronização de hash de palavra-passe. Se encontravam online novamente num fim de horas. Ao utilizar o acesso ao e-mail através do Office 365, correu para resolver problemas e aceder a outras cargas de trabalho baseado na nuvem.

   * As organizações que anteriormente não ativar a sincronização de hash de palavra-passe que tiveram de recorrer a sistemas de e-mail não fidedignos consumidor externo para as comunicações e resolução de problemas. Nesses casos, demorou-los semanas ou mais para ser novamente em funcionamento.

3. **Proteção de identidade**. É uma das formas melhor para proteger os utilizadores na nuvem do Azure AD Identity Protection. Microsoft analisa continuamente da Internet para o utilizador e palavra-passe apresenta uma lista que mal-intencionadas propor e disponibilizar na web um escuro. Azure AD pode utilizar estas informações para verificar se qualquer um dos nomes de utilizador e palavras-passe na sua organização sejam comprometidos. Por isso, é fundamental para ativar a sincronização de hash de palavra-passe, independentemente da que método de autenticação a utilizar, se de que está federada ou a autenticação pass-through. As credenciais obtidas ilicitamente são apresentadas como um relatório. Utilize estas informações para bloquear ou force os utilizadores para alterar as palavras-passe quando tentar iniciar sessão com palavras-passe obtida ilicitamente.

Por último, em conformidade com a [Gartner](https://info.microsoft.com/landingIAMGartnerreportregistration.html), a Microsoft tem o conjunto mais completo de funções de gestão de identidades e acessos. Os identificadores de Microsoft [pedidos de autenticação de mil milhões de 450](https://www.microsoft.com/en-us/security/intelligence-report) todos os meses para fornecer acesso a milhares de aplicações SaaS, como o Office 365 a partir de praticamente qualquer dispositivo. 

## <a name="conclusion"></a>Conclusão

Este artigo descreve várias opções de autenticação que as organizações podem configurar e implementar para suportar o acesso a aplicações em nuvem. Para cumprir vários business, segurança e requisitos técnicos, as organizações podem escolher entre a sincronização de hash de palavra-passe, autenticação pass-through e a Federação. 

Considere cada método de autenticação. É o esforço do utilizador para implementar a solução e a experiência do utilizador do processo de início de sessão, abordar os requisitos de negócio? Avalie se a sua organização necessita de funcionalidades de continuidade de negócio de cada método de autenticação e cenários avançados. Por fim, avalie as considerações de cada método de autenticação. Qualquer uma delas impedem que implementar a sua preferência?

## <a name="next-steps"></a>Passos Seguintes

No mundo de hoje, ameaças estão presentes 24 horas por dia em provenientes everywhere. Implementa o método de autenticação correta e será mitigar os riscos de segurança e proteger as identidades.

[Introdução ao](https://docs.microsoft.com/azure/active-directory/get-started-azure-ad) com o Azure AD e implementar a solução de autenticação adequado para a sua organização.

Se estiver a pensar sobre a migração de federado para autenticação de nuvem, saiba mais sobre [alterar o método de início de sessão](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-user-signin#changing-the-user-sign-in-method). Para ajudar a planear e implementar a migração, utilize [estes planos de implementação do projeto](http://aka.ms/deploymentplans).
