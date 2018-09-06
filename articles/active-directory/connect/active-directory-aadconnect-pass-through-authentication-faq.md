---
title: 'Do Azure AD Connect: A autenticação pass-through - perguntas mais frequentes | Documentos da Microsoft'
description: Respostas às perguntas mais frequentes sobre a autenticação de pass-through do Azure Active Directory
services: active-directory
keywords: Autenticação do Azure AD Connect pass-through, Active Directory, de instalação necessários componentes para o Azure AD, SSO, Single Sign-on
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 58a33b2d3f06deab4f31c76e04d45f8bd0bbba4c
ms.sourcegitcommit: 3d0295a939c07bf9f0b38ebd37ac8461af8d461f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/06/2018
ms.locfileid: "43841969"
---
# <a name="azure-active-directory-pass-through-authentication-frequently-asked-questions"></a>O Azure Active Directory autenticação pass-through: Perguntas mais frequentes

Este artigo aborda perguntas mais frequentes sobre a autenticação de pass-through do Azure Active Directory (Azure AD). Manter a verificação de volta para o conteúdo atualizado.

## <a name="which-of-the-methods-to-sign-in-to-azure-ad-pass-through-authentication-password-hash-synchronization-and-active-directory-federation-services-ad-fs-should-i-choose"></a>Que os métodos para iniciar sessão Azure AD, autenticação pass-through, palavra-passe de hash sincronização e serviços de Federação do Active Directory (AD FS), devo escolher?

Revisão [este guia](https://docs.microsoft.com/azure/security/azure-ad-choose-authn) para uma comparação entre os vários Azure AD início de sessão métodos e como escolher o método certo início de sessão para a sua organização.

## <a name="is-pass-through-authentication-a-free-feature"></a>É uma funcionalidade gratuita de autenticação pass-through?

Autenticação pass-through é um recurso gratuito. Não é necessário qualquer nas edições pagas do Azure AD para utilizá-lo.

## <a name="is-pass-through-authentication-available-in-the-microsoft-azure-germany-cloudhttpwwwmicrosoftdecloud-deutschland-and-the-microsoft-azure-government-cloudhttpsazuremicrosoftcomfeaturesgov"></a>Está disponível na autenticação pass-through a [cloud do Microsoft Azure Alemanha](http://www.microsoft.de/cloud-deutschland) e o [cloud do Microsoft Azure Government](https://azure.microsoft.com/features/gov/)?

Não. Autenticação pass-through só está disponível na instância do Azure AD em todo o mundo.

## <a name="does-conditional-accessactive-directory-conditional-access-azure-portalmd-work-with-pass-through-authentication"></a>Faz [acesso condicional](../active-directory-conditional-access-azure-portal.md) funcionam com a autenticação pass-through?

Sim. Todas as capacidades de acesso condicional, incluindo multi-factor Authentication, trabalham com a autenticação pass-through.

## <a name="does-pass-through-authentication-support-alternate-id-as-the-username-instead-of-userprincipalname"></a>Autenticação pass-through suporta "ID alternativo" como o nome de utilizador, em vez de "userPrincipalName"?

Sim. Suporta a autenticação pass-through `Alternate ID` como o nome de utilizador quando configurado no Azure AD Connect. Para obter mais informações, consulte [instalação personalizada do Azure AD Connect](active-directory-aadconnect-get-started-custom.md). Nem todas as aplicações do Office 365 suportam `Alternate ID`. Consulte a declaração de suporte de documentação do aplicativo específico.

## <a name="does-password-hash-synchronization-act-as-a-fallback-to-pass-through-authentication"></a>Sincronização de hash de palavra-passe atuar como contingência para autenticação pass-through?

Não. Autenticação pass-through _não_ automaticamente a ativação pós-falha para a sincronização de hash de palavra-passe. Para evitar falhas de início de sessão de utilizador, deve configurar a autenticação pass-through para [elevada disponibilidade](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-4-ensure-high-availability).

## <a name="can-i-install-an-azure-ad-application-proxymanage-appsapplication-proxymd-connector-on-the-same-server-as-a-pass-through-authentication-agent"></a>Pode instalar um [Proxy de aplicações do Azure AD](../manage-apps/application-proxy.md) conector no mesmo servidor como um agente de autenticação pass-through?

Sim. As versões remodeladas do agente de autenticação pass-through, versão 1.5.193.0 ou posterior, suportar esta configuração.

## <a name="what-versions-of-azure-ad-connect-and-pass-through-authentication-agent-do-you-need"></a>Quais as versões do Azure AD Connect e o agente de autenticação pass-through precisa?

Para esta funcionalidade funcione, tem de versão 1.1.750.0 ou posterior para o Azure AD Connect e 1.5.193.0 ou posterior para o agente de autenticação pass-through. Instale todo o software em servidores com o Windows Server 2012 R2 ou posterior.

## <a name="what-happens-if-my-users-password-has-expired-and-they-try-to-sign-in-by-using-pass-through-authentication"></a>O que acontece se a palavra-passe meu utilizador expirou e eles tentam iniciar sessão com a autenticação pass-through?

Se tiver configurado [repetição de escrita de palavra-passe](../authentication/concept-sspr-writeback.md) para um utilizador específico, e se o utilizador inicia sessão utilizando a autenticação pass-through, pode alterar ou repor as palavras-passe. As palavras-passe são escritas de volta para o Active Directory no local como esperado.

Se não tiver configurado a repetição de escrita de palavra-passe para um utilizador específico, ou se o utilizador não tem um Azure AD válido licença atribuída, o utilizador não é possível atualizar a palavra-passe na cloud. Eles não é possível atualizar a palavra-passe, mesmo que a palavra-passe expirou. O utilizador em vez disso, vê esta mensagem: "sua organização não permite que atualizar a palavra-passe neste site. A atualização, de acordo com o método recomendado pela sua organização, ou peça ao seu administrador se precisar de ajuda". O utilizador ou o administrador terá de repor a palavra-passe no Active Directory no local.

## <a name="how-does-pass-through-authentication-protect-you-against-brute-force-password-attacks"></a>Como autenticação pass-through protege contra ataques de força bruta a senhas?

[Ler informações sobre o bloqueio inteligente](../authentication/howto-password-smart-lockout.md).

## <a name="what-do-pass-through-authentication-agents-communicate-over-ports-80-and-443"></a>O que a agentes de autenticação pass-through a comunicar através das portas 80 e 443?

- Os agentes de autenticação fazer pedidos HTTPS através da porta 443 para todas as operações de funcionalidade.
- Os agentes de autenticação de fazer solicitações HTTP através da porta 80 para transferir as listas de revogação de certificado SSL (CRL).

     >[!NOTE]
     >Atualizações recentes reduzimos o número de portas de que a funcionalidade requer. Se tiver versões mais antigas do Azure AD Connect ou o agente de autenticação, mantém estas portas abertas também: 5671, 8080, 9090, 9091, 9350, 9352 e 10100 10120.

## <a name="can-the-pass-through-authentication-agents-communicate-over-an-outbound-web-proxy-server"></a>Os agentes de autenticação pass-through possam comunicar através de um servidor de proxy da web de saída?

Sim. Se a deteção automática de Proxy da Web (WPAD) estiver ativada no seu ambiente no local, os agentes de autenticação automaticamente tente localizar e utilizar um servidor web proxy na rede.

## <a name="can-i-install-two-or-more-pass-through-authentication-agents-on-the-same-server"></a>Pode instalar dois ou mais agentes de autenticação pass-through no mesmo servidor?

Não, só pode instalar um agente de autenticação pass-through num único servidor. Se quiser configurar a autenticação pass-through para elevada disponibilidade, [siga as instruções aqui](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-4-ensure-high-availability).

## <a name="how-do-i-remove-a-pass-through-authentication-agent"></a>Como posso remover um agente de autenticação pass-through?

Desde que um agente de autenticação pass-through está em execução, permanece ativa e continuamente processa pedidos de início de sessão do utilizador. Se quiser desinstalar um agente de autenticação, aceda a **painel de controlo -> programas -> programas e funcionalidades** e desinstalar o **agente do Microsoft Azure AD Connect autenticação** e o  **Atualizador de agente ligar do Microsoft Azure AD** programas.

Se verificar o painel de autenticação pass-through a [Centro de administração do Azure Active Directory](https://aad.portal.azure.com) depois de concluir o passo anterior, verá o agente de autenticação que mostra como **Inactive**. Isto é _esperado_. O agente de autenticação é automaticamente removido da lista após alguns dias.

## <a name="i-already-use-ad-fs-to-sign-in-to-azure-ad-how-do-i-switch-it-to-pass-through-authentication"></a>Já posso utilizar o AD FS para iniciar sessão com o Azure AD. Como posso alterná-lo para a autenticação pass-through?

Se estiver a migrar do AD FS (ou outras tecnologias de Federação) para autenticação pass-through, recomendamos vivamente que siga o nosso guia de implementação detalhados publicado [aqui](https://github.com/Identity-Deployment-Guides/Identity-Deployment-Guides/blob/master/Authentication/Migrating%20from%20Federated%20Authentication%20to%20Pass-through%20Authentication.docx).

## <a name="can-i-use-pass-through-authentication-in-a-multi-forest-active-directory-environment"></a>Pode utilizar autenticação pass-through no ambiente de várias florestas do Active Directory?

Sim. Ambientes de várias florestas são suportadas se existem relações de confiança de floresta entre florestas do Active Directory e se o encaminhamento de sufixo de nome está configurado corretamente.

## <a name="how-many-pass-through-authentication-agents-do-i-need-to-install"></a>Quantos agentes de autenticação pass-through é necessário instalar?

Instalar agentes de autenticação pass-through vários garante [elevada disponibilidade](active-directory-aadconnect-pass-through-authentication-quick-start.md#step-4-ensure-high-availability). No entanto, ele não fornece determinística balanceamento de carga entre os agentes de autenticação.

Considere as horas de pico e a carga média de pedidos de início de sessão que pretende ver no seu inquilino. Como um parâmetro de comparação, um único agente de autenticação pode processar autenticações de 300 a 400 por segundo numa CPU de 4 núcleos padrão, servidor de RAM de 16 GB.

Para estimar o tráfego de rede, utilize as seguintes orientações de dimensionamento:
- Cada pedido tem um tamanho de payload de (0,5 k + 1 K * num_of_agents) bytes; Por exemplo, dados do Azure AD para o agente de autenticação. Aqui, "num_of_agents" indica que o número de agentes de autenticação registrado no seu inquilino.
- Cada resposta tem um tamanho de payload de bytes de 1K; Por exemplo, dados do agente de autenticação para o Azure AD.

Para a maioria dos clientes, os agentes de autenticação de dois ou três no total são suficientes para elevada disponibilidade e capacidade. Deve instalar agentes de autenticação perto dos seus controladores de domínio para melhorar a latência de início de sessão.

>[!NOTE]
>Existe um limite de sistema de 12 agentes de autenticação por inquilino.

## <a name="can-i-install-the-first-pass-through-authentication-agent-on-a-server-other-than-the-one-that-runs-azure-ad-connect"></a>Pode instalar o agente de autenticação pass-through primeiro num servidor diferente daquele que é executado o Azure AD Connect?

Não, este cenário é _não_ suportado.

## <a name="why-do-i-need-a-cloud-only-global-administrator-account-to-enable-pass-through-authentication"></a>Por que razão necessito de uma conta de Administrador Global apenas na cloud para ativar a autenticação pass-through?

Recomenda-se que ative ou desative a autenticação pass-through com uma conta de Administrador Global apenas na cloud. Saiba mais sobre [adicionar uma conta de Administrador Global apenas na cloud](../active-directory-users-create-azure-portal.md). Fazê-lo desta forma, garante que não fica bloqueado fora do seu inquilino.

## <a name="how-can-i-disable-pass-through-authentication"></a>Como desativar a autenticação pass-through?

Executar novamente o Assistente do Azure AD Connect e altere o método de início de sessão do utilizador de autenticação pass-through para outro método. Esta alteração desativa a autenticação pass-through no inquilino e desinstala o agente de autenticação do servidor. Tem de desinstalar manualmente os agentes de autenticação de outros servidores.

## <a name="what-happens-when-i-uninstall-a-pass-through-authentication-agent"></a>O que acontece quando posso desinstalar um agente de autenticação pass-through?

Se desinstalar um agente de autenticação pass-through a partir de um servidor, ele faz com que o servidor não aceitar pedidos de início de sessão. Para evitar interromper a capacidade de início de sessão utilizador no seu inquilino, certifique-se de que tenha outro agente de autenticação em execução antes de desinstalar um agente de autenticação pass-through.

## <a name="next-steps"></a>Passos Seguintes
- [Limitações atuais](active-directory-aadconnect-pass-through-authentication-current-limitations.md): Saiba quais cenários são suportados e quais não são.
- [Guia de introdução](active-directory-aadconnect-pass-through-authentication-quick-start.md): comece a utilizar na autenticação pass-through do Azure AD.
- [Migrar do AD FS para autenticação pass-through](https://github.com/Identity-Deployment-Guides/Identity-Deployment-Guides/blob/master/Authentication/Migrating%20from%20Federated%20Authentication%20to%20Pass-through%20Authentication.docx) -um guia detalhado para migrar do AD FS (ou outras tecnologias de Federação) para autenticação pass-through.
- [Bloqueio do smart](../authentication/howto-password-smart-lockout.md): Saiba como configurar a capacidade de bloqueio inteligente no seu inquilino para proteger contas de utilizador.
- [Análise técnica aprofundada](active-directory-aadconnect-pass-through-authentication-how-it-works.md): compreender como funciona a funcionalidade de autenticação pass-through.
- [Resolver problemas de](active-directory-aadconnect-troubleshoot-pass-through-authentication.md): Saiba como resolver problemas comuns com a funcionalidade de autenticação pass-through.
- [Detalhada da segurança](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md): Obtenha informações técnicas detalhadas sobre a funcionalidade de autenticação pass-through.
- [O Azure AD Seamless SSO](active-directory-aadconnect-sso.md): Saiba mais sobre esta funcionalidade complementar.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): Utilize o Fórum do Azure Active Directory para pedidos de novas funcionalidades de ficheiros.

