---
title: Novidades Notas de versão para o Azure Active Directory | Microsoft Docs
description: Saiba o que há de novo no Azure Active Directory (Azure AD), tais como as notas de versão mais recentes, problemas conhecidos, correções de erros, funcionalidade preterida e as alterações futuras.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
featureFlags:
- clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: b7ad535976508cb195991c374995b0a0b6e45e10
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="whats-new-in-azure-active-directory"></a>O que é novo no Azure Active Directory?


> Continuar atualizado com o que há de novo no Azure Active Directory (Azure AD) ao subscrever o [ ![RSS](./media/whats-new/feed-icon-16x16.png)](https://docs.microsoft.com/api/search/rss?search=%22whats%20new%20in%20azure%20active%20directory%22&locale=en-us) [feed](https://docs.microsoft.com/api/search/rss?search=%22whats%20new%20in%20azure%20active%20directory%22&locale=en-us).



Azure AD recebe melhoramentos numa base contínua. Para se manter atualizado com os desenvolvimentos mais recentes, este artigo fornece informações sobre:

-   As versões mais recentes
-   Problemas conhecidos
-   Correções de erros
-   Funcionalidade preterida
-   Planos de alterações

Esta página é atualizada mensalmente, por isso, revê-lo regularmente.


## <a name="april-2018"></a>De 2018 Abril
 


### <a name="azure-ad-b2c-access-token-are-ga"></a>Azure AD B2C acesso Token são GA

**Tipo:** nova funcionalidade  
**Categoria de serviço:** B2C - gestão de identidades de consumidor  
**Capacidade de produto:** B2B/B2C
 

Pode agora aceder da API Web protegida pelo Azure AD B2C utilizando tokens de acesso. A funcionalidade está a mover de pré-visualização pública depois da disponibilidade geral A experiência de IU para configurar aplicações do Azure AD B2C e da API web foi melhorada e outras melhorias secundárias foram efetuadas.
 
Para obter mais informações, consulte [do Azure AD B2C: pedir tokens de acesso](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-access-tokens).


---
 

### <a name="test-single-sign-on-configuration-for-saml-based-applications"></a>Testar a configuração de início de sessão único para aplicações baseadas no SAML

**Tipo:** nova funcionalidade  
**Categoria de serviço:** aplicações da empresa  
**Capacidade de produto:** SSO
 

Quando configurar SAML com base em aplicações de SSO conseguir a integração na página Configuração de teste. Se ocorrer um erro durante o início de sessão, pode fornecer o erro na experiência de teste e do Azure AD fornece-lhe os passos de resolução para resolver o problema específico.

Para obter mais informações, consulte:

- [Configurar o início de sessão único em aplicações que não fazem parte da galeria de aplicações do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps)
- [Como depurar baseados em SAML-início de sessão único para aplicações no Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging)


---
 
### <a name="azure-ad-terms-of-use-now-has-per-user-reporting"></a>Agora tem do Azure AD os termos de utilização por utilizador de relatórios

**Tipo:** nova funcionalidade  
**Categoria de serviço:** os termos de utilização  
**Capacidade de produto:** conformidade
 

Os administradores podem agora selecionar um determinado ToU e ver todos os utilizadores que tenham autorizado que ToU e que data/hora demorou local.


Para obter mais informações, consulte o [termos do Azure AD de utilização da funcionalidade](https://docs.microsoft.com/azure/active-directory/active-directory-tou).
 

---
 
### <a name="azure-ad-connect-health-risky-ip-for-ad-fs-extranet-lockout-protection"></a>O Azure AD Connect Health: IP duvidoso para proteção de bloqueio de extranet do AD FS 

**Tipo:** nova funcionalidade  
**Categoria de serviço:** outros  
**Capacidade de produto:** monitorização e relatórios
 

Ligar o estado de funcionamento agora suporta a capacidade de detetar IP endereços que exceder um limiar de inícios de sessão falhados U/P numa base por hora ou diariamente. As capacidades fornecidas por esta funcionalidade são:

- Relatório abrangente que mostra o número de inícios de sessão falhados gerados uma hora a hora/diariamente com limiar personalizável e endereço IP.
- Alertas baseados em e-mail mostrar quando um endereço IP específico excedeu o limiar de inícios de sessão falhados U/P numa base diária/hora a hora.
- Uma opção de transferência para efetuar uma análise detalhada dos dados


Para obter mais informações, consulte [relatório de IP arriscados](https://aka.ms/aadchriskyip).

 

---
 

### <a name="easy-app-config-with-metadata-file-or-url"></a>Configuração de aplicações fácil com os metadados ficheiro ou URL

**Tipo:** nova funcionalidade  
**Categoria de serviço:** aplicações da empresa  
**Capacidade de produto:** SSO
 

Na página de aplicações da empresa, os administradores podem carregar um ficheiro de metadados SAML para configurar SAML com base em início de sessão para a aplicação AAD Galeria e não galeria.

Além disso, pode utilizar o URL de metadados de Federação de aplicação do Azure AD para configurar o SSO com a aplicação de destino.

Para obter mais informações, consulte [configurar início de sessão único para aplicações que não estejam na Galeria de aplicações do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps).
 

---
 

### <a name="azure-ad-terms-of-use-now-generally-available"></a>Azure AD termos de utilização agora geralmente disponível

**Tipo:** nova funcionalidade  
**Categoria de serviço:** os termos de utilização  
**Capacidade de produto:** conformidade
 

Azure AD os termos de utilização foi movido de pré-visualização pública para geralmente disponível.

Para obter mais informações, consulte o [termos do Azure AD de utilização da funcionalidade](https://docs.microsoft.com/azure/active-directory/active-directory-tou).

 

---
 

### <a name="allow-or-block-invitations-to-b2b-users-from-specific-organizations"></a>Permitir ou bloquear convites aos utilizadores B2B de organizações específicas

**Tipo:** nova funcionalidade  
**Categoria de serviço:** B2B  
**Capacidade de produto:** B2B/B2C
 

Agora pode especificar que as organizações de parceiro que pretende partilhar e colaborar em colaboração B2B do Azure AD. Para tal, pode optar por criar a lista de específicos permitir ou negar domínios. Quando um domínio é bloqueado com estas capacidades, os funcionários já não podem enviar convites para pessoas nesse domínio.

Isto ajuda a controlar o acesso aos seus recursos, ao ativar uma experiência uniforme para os utilizadores aprovados.

Esta funcionalidade de colaboração B2B está disponível para todos os clientes do Azure Active Directory e pode ser utilizada em conjunto com as funcionalidades do Azure AD Premium, como a proteção de identidades e acesso condicional para o controlo mais granular sobre quando e como os utilizadores de negócios externos iniciam sessão no e obter acesso.

Para obter mais informações, consulte [ou de bloqueios convites aos utilizadores B2B de organizações específicas](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-allow-deny-list).

 

---
 

### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Novas aplicações federadas disponíveis na Galeria de aplicações do Azure AD

**Tipo:** nova funcionalidade  
**Categoria de serviço:** aplicações da empresa  
**Capacidade de produto:** 3rd integração de terceiros
 

De 2018 Abril, foram adicionados seguintes 13 novas aplicações na nossa Galeria de aplicações com a Federação suportam:



O critério HCM, [FiscalNote](https://docs.microsoft.com/azure/active-directory/active-directory-saas-fiscalnote-tutorial), [segredo de servidor (no local)](https://docs.microsoft.com/azure/active-directory/active-directory-saas-secretserver-on-premises-tutorial), [sinal dinâmica](https://docs.microsoft.com/azure/active-directory/active-directory-saas-dynamicsignal-tutorial), [mindWireless](https://docs.microsoft.com/azure/active-directory/active-directory-saas-mindwireless-tutorial), [OrgChart Agora](https://docs.microsoft.com/azure/active-directory/active-directory-saas-orgchartnow-tutorial), [Ziflow](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ziflow-tutorial), [Monitor de desempenho AppNeta](https://docs.microsoft.com/azure/active-directory/active-directory-saas-appneta-tutorial), [Elium](https://docs.microsoft.com/azure/active-directory/active-directory-saas-elium-tutorial) , [Fluxx laboratórios](https://docs.microsoft.com/azure/active-directory/active-directory-saas-fluxxlabs-tutorial), [ Nuvem de Cisco](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ciscocloud-tutorial), Shelf, [SafetyNet](https://docs.microsoft.com/azure/active-directory/active-directory-saas-safetynet-tutorial)



 Pode encontrar a lista dos tutoriais disponíveis aqui: [ https://aka.ms/appstutorial ](https://aka.ms/appstutorial).

Para obter mais informações, consulte [listar a aplicação na Galeria de aplicações do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).


 

---
 
### <a name="grant-b2b-users-in-azure-ad-access-to-your-on-premises-applications-public-preview"></a>Os utilizadores do Azure AD B2B conceder acedem às suas aplicações no local (pré-visualização pública)

**Tipo:** nova funcionalidade  
**Categoria de serviço:** B2B  
**Capacidade de produto:** B2B/B2C
 

Como uma organização que utiliza as capacidades de colaboração B2B do Azure Active Directory (Azure AD) para convidar utilizadores convidados de organizações de parceiros ao seu Azure AD, agora pode fornecer estes utilizadores B2B acesso a aplicações no local. Estas aplicações no local podem utilizar a autenticação baseada em SAML ou autenticação integrada de Windows (IWA) com a delegação restringida de Kerberos (KCD).

Para obter mais informações, consulte [conceder B2B utilizadores no Azure AD acedem às suas aplicações no local](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-hybrid-cloud-to-on-premises)
 

---
 
### <a name="get-sso-integration-tutorials-from-the-azure-marketplace"></a>Obter tutoriais de integração do SSO no Azure Marketplace

**Tipo:** funcionalidade foi alterado  
**Categoria de serviço:** outros  
**Capacidade de produto:** 3rd integração de terceiros
 

Se uma aplicação que está listada no [do Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps?page=1) suporta SAML com base início de sessão único, clicando em **obtê-lo agora** fornece-lhe o tutorial de integração associado a essa aplicação. 


---

### <a name="faster-performance-of-azure-ad-automatic-user-provisioning-to-saas-applications"></a>Desempenho mais rápido do Azure AD aprovisionamento de utilizador automáticas para aplicações SaaS

**Tipo:** funcionalidade foi alterado  
**Categoria de serviço:** aprovisionamento de aplicações  
**Capacidade de produto:** 3rd integração de terceiros
 

Anteriormente, os clientes utilizando o aprovisionamento conectores para aplicações SaaS (por exemplo Salesforce, ServiceNow e caixa) de utilizadores do Azure Active Directory foi experiência de desempenho muito lento se os seus inquilinos do Azure AD continha mais de 100 000 utilizadores combinados e grupos e estava a utilizar as atribuições de grupo e utilizador para determinar quais os utilizadores devem ser aprovisionados.

Em 2nd de Abril, melhoramentos de desempenho muito significativas foram implementados para o serviço de aprovisionamento do Azure AD reduzir significativamente a quantidade de tempo necessário para efetuar sincronizações iniciais entre o Azure Active Directory e aplicações de SaaS de destino.


Como resultado, muitos clientes que tinham as sincronizações iniciais para as aplicações que demorou o número de dias ou nunca foi concluída, são agora concluir dentro de um fim de minutos ou horas.

Para obter mais informações, consulte [o que acontece durante o aprovisionamento?](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning#what-happens-during-provisioning)

---
 

### <a name="self-service-password-reset-from-windows-10-lock-screen-for-hybrid-azure-ad-joined-machines"></a>Self-service reposição palavra-passe a partir do ecrã de bloqueio do Windows 10 para o Azure AD híbrido associado máquinas

**Tipo:** funcionalidade foi alterado  
**Categoria de serviço:** reposição personalizada de palavra-passe  
**Capacidade de produto:** autenticação de utilizador
 

Foi atualizado com a funcionalidade de SSPR do Windows 10 incluem suporte para as máquinas que fazem híbrida do Azure AD associado. Esta funcionalidade está disponível no Windows 10 RS4 permite aos utilizadores para repor a palavra-passe no ecrã de bloqueio de uma máquina Windows 10. Os utilizadores que estão ativados e registados para a reposição de palavra-passe self-service podem utilizar esta funcionalidade.

Para obter mais informações, consulte [palavra-passe do Azure AD a partir do ecrã de início de sessão de reposição](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-sspr-windows).
 

---



## <a name="march-2018"></a>De 2018 Março
 

### <a name="certificate-expire-notification"></a>Notificação de expiração do certificado

**Tipo:** fixo  
**Categoria de serviço:** aplicações da empresa  
**Capacidade de produto:** SSO
 
Azure AD envia uma notificação quando um certificado para uma galeria ou aplicação não galeria está prestes a expirar. 

Alguns utilizadores não recebeu notificações para as aplicações empresariais configuradas para baseados em SAML-início de sessão único. Este problema foi resolvido. Azure AD envia a notificação de certificados irá expirar em 7, 30 e 60 dias. Que ere consiga ver este evento nos registos de auditoria. 

Para obter mais informações, consulte:

- [Gerir certificados para federado início de sessão no Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-sso-certs)
- [Relatórios de atividade de auditoria no portal do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)

 
---
 

### <a name="twitter-and-github-identity-providers-in-azure-ad-b2c"></a>Fornecedores de identidade twitter e o GitHub no Azure AD B2C

**Tipo:** nova funcionalidade  
**Categoria de serviço:** B2C - gestão de identidades de consumidor  
**Capacidade de produto:** B2B/B2C
 
Agora pode adicionar Twitter ou GitHub como um fornecedor de identidade no Azure AD B2C. Twitter está a mover de pré-visualização pública depois da disponibilidade geral Está a ser libertado GitHub na pré-visualização pública.


Para obter mais informações, consulte [o que é a colaboração B2B do Azure AD?](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).
 
---


### <a name="restrict-browser-access-using-intune-managed-browser-with-azure-ad-application-based-conditional-access-for-ios-and-android"></a>Restringir o acesso ao browser utilizando o Browser gerido do Intune para iOS e Android com acesso condicional de baseadas em aplicações do Azure AD

**Tipo:** nova funcionalidade  
**Categoria de serviço:** acesso condicional  
**Capacidade de produto:** segurança de identidade e de proteção
 

**Agora em pré-visualização pública!**

**Browser gerido do Intune SSO:** os seus empregados podem utilizar o início de sessão único em clientes nativos (como o Microsoft Outlook) e o Intune Managed Browser para todas as aplicações do Azure AD ligados.

**Gerido Browser condicional acesso suporte do Intune:** pode agora pedir aos funcionários utilizar o Intune Managed browser através de políticas de acesso condicional baseado na aplicação.

Leia mais sobre este assunto na nossa [blogue](https://cloudblogs.microsoft.com/enterprisemobility/2018/03/15/the-intune-managed-browser-now-supports-azure-ad-sso-and-conditional-access/).

Para obter mais informações, consulte:

- [Configurar o acesso condicional baseado na aplicação](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam)

- [Configurar políticas de browser gerido](https://aka.ms/managedbrowser)  



---
 

### <a name="app-proxy-cmdlets-in-powershell-ga-module"></a>Cmdlets do Proxy de aplicações no módulo do Powershell GA

**Tipo:** nova funcionalidade  
**Categoria de serviço:** do Proxy de aplicação  
**Capacidade de produto:** controlo de acesso
 
Suporte para os cmdlets do Proxy da aplicação está agora no módulo Powershell GA! Tenha em atenção que isto requer a se manter atualizado em módulos do Powershell - se tornar-se de mais de um ano atrás, alguns cmdlets poderá parar de funcionar. 


Para obter mais informações, consulte [AzureAD](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0).
 
---
 
### <a name="office-365-native-clients-are-supported-by-seamless-sso-using-a-non-interactive-protocol"></a>Os clientes nativos do Office 365 são suportados pelo SSO totalmente integrada utilizando um protocolo não interativo

**Tipo:** nova funcionalidade  
**Categoria de serviço:** autenticações (inícios de sessão)  
**Capacidade de produto:** autenticação de utilizador
 
Utilizador utilizando clientes nativos do Office 365 (versão 16.0.8730.xxxx e acima) um automática início de sessão experiência SSO totalmente integrado a utilizar. Este suporte é fornecido pela adição um protocolo não interativo (WS-Trust) para o Azure AD.

Para obter mais informações, consulte [como início de sessão num cliente nativo com o trabalho de SSO totalmente integrado?](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-how-it-works#how-does-sign-in-on-a-native-client-with-seamless-sso-work).

 
---
 

### <a name="users-get-a-silent-sign-on-experience-with-seamless-sso-if-an-application-sends-sign-in-requests-to-azure-ads-tenanted-endpoints"></a>Os utilizadores obtêm uma automática início de sessão experiência, com o SSO totalmente integrada, se uma aplicação envia pedidos de início de sessão para os pontos finais de tenanted do Azure AD

**Tipo:** nova funcionalidade  
**Categoria de serviço:** autenticações (inícios de sessão)  
**Capacidade de produto:** autenticação de utilizador
 
Os utilizadores obtêm uma automática início de sessão experiência, com o SSO totalmente integrada, se uma aplicação (por exemplo, `https://contoso.sharepoint.com`) envia pedidos de início de sessão para pontos finais tenanted do Azure AD - ou seja, `https://login.microsoftonline.com/contoso.com/<..>` ou `https://login.microsoftonline.com/<tenant_ID>/<..>` - em vez de ponto final comum do Azure AD (`https://login.microsoftonline.com/common/<...>` ).

Para obter mais informações, consulte [do Azure Active Directory totalmente integrada Single Sign-On](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso). 

---
 

### <a name="need-to-add-only-one-azure-ad-url-instead-of-two-urls-previously-to-users-intranet-zone-settings-to-roll-out-seamless-sso"></a>Tem de adicionar apenas um URL de AD do Azure, em vez de dois URLs anteriormente, as definições de zona de Intranet dos utilizadores para implementar SSO totalmente integrada

**Tipo:** nova funcionalidade  
**Categoria de serviço:** autenticações (inícios de sessão)  
**Capacidade de produto:** autenticação de utilizador
 
Para implementar SSO totalmente integrado aos seus utilizadores, terá de adicionar definições de zona de apenas um URL de AD do Azure à Intranet dos utilizadores através da política de grupo no Active Directory: `https://autologon.microsoftazuread-sso.com`. Anteriormente, os clientes era necessário adicionar dois URLs.

Para obter mais informações, consulte [do Azure Active Directory totalmente integrada Single Sign-On](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso). 
 
---
 

### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Novas federado aplicações disponíveis na Galeria de aplicações do Azure AD

**Tipo:** nova funcionalidade  
**Categoria de serviço:** aplicações da empresa  
**Capacidade de produto:** 3rd integração de terceiros
 
De 2018 de Março, foram adicionados seguintes 15 novas aplicações na nossa Galeria de aplicações com a Federação suportam:

[Boxcryptor](https://docs.microsoft.com/azure/active-directory/active-directory-saas-boxcryptor-tutorial), [CylancePROTECT](https://docs.microsoft.com/azure/active-directory/active-directory-saas-cylanceprotect-tutorial), Wrike, [SignalFx](https://docs.microsoft.com/azure/active-directory/active-directory-saas-signalfx-tutorial), Assistente por FirstAgenda, [YardiOne](https://docs.microsoft.com/azure/active-directory/active-directory-saas-yardione-tutorial), Vtiger CRM, inwink, [Amplitude](https://docs.microsoft.com/azure/active-directory/active-directory-saas-amplitude-tutorial), [Spacio](https://docs.microsoft.com/azure/active-directory/active-directory-saas-spacio-tutorial), [ContractWorks](https://docs.microsoft.com/azure/active-directory/active-directory-saas-contractworks-tutorial), [Bersin](https://docs.microsoft.com/azure/active-directory/active-directory-saas-bersin-tutorial), [Mercell](https://docs.microsoft.com/azure/active-directory/active-directory-saas-mercell-tutorial), [Trisotech Digital Enterprise Server](https://docs.microsoft.com/azure/active-directory/active-directory-saas-trisotechdigitalenterpriseserver-tutorial), [Qumu nuvem](https://docs.microsoft.com/azure/active-directory/active-directory-saas-qumucloud-tutorial).
 
Pode encontrar a documentação para todas as aplicações aqui: [https://aka.ms/appstutorial](https://aka.ms/appstutorial)


 
---
 

### <a name="pim-for-azure-resources-is-generally-available"></a>PIM para recursos do Azure está normalmente disponível

**Tipo:** nova funcionalidade  
**Categoria de serviço:** Privileged Identity Management  
**Capacidade de produto:** Privileged Identity Management
 
Se estiver a utilizar o Azure AD Privileged Identity Management para funções de diretório, agora, pode utilizar o acesso de vínculo de tempo e as capacidades de atribuição do PIM para funções de recursos do Azure, tais como as subscrições, grupos de recursos, máquinas virtuais e quaisquer outros recursos suportados pelo Azure Resource Manager. Impor o multi-factor Authentication durante a ativação em funções Just-In-Time e agendar ativações em coordenação com o windows de alteração aprovados. Além disso, esta versão adiciona melhoramentos não está disponíveis durante a pré-visualização pública, incluindo uma IU atualizada, fluxos de trabalho de aprovação e a capacidade para expandir as funções expira em breve e renovar funções expiradas.

Para obter mais informações, consulte [PIM para recursos do Azure (pré-visualização)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac)
 
---
 

### <a name="adding-optional-claims-to-your-apps-tokens-public-preview"></a>Adicionar opcional afirmações para os tokens de aplicações (pré-visualização pública)

**Tipo:** nova funcionalidade  
**Categoria de serviço:** autenticações (inícios de sessão)  
**Capacidade de produto:** autenticação de utilizador
 
A aplicação do Azure AD pode agora afirmações do pedido, personalizado ou opcional no JWTs ou SAML tokens.  Estes são afirmações sobre o utilizador ou o inquilino que não estão incluídas por predefinição no token, devido a restrições de tamanho ou aplicabilidade.  Esta é atualmente em pré-visualização pública para aplicações do Azure AD nos pontos finais v 1.0 e a v 2.0.  Consulte a documentação para obter informações sobre quais as afirmações que podem ser adicionados e editar o manifesto da aplicação para solicitar-lhes.  

Para obter mais informações, consulte [opcional afirmações no Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-optional-claims).
 
---
 

### <a name="azure-ad-supports-pkce-for-more-secure-oauth-flows"></a>AD do Azure suporta PKCE para fluxos de OAuth mais seguros

**Tipo:** nova funcionalidade  
**Categoria de serviço:** autenticações (inícios de sessão)  
**Capacidade de produto:** autenticação de utilizador
 
Foram atualizados os documentos do Azure AD a ter em atenção o suporte para PKCE, que permite a comunicação mais segura durante o fluxo de concessão do código de autorização do OAuth 2.0.  São suportados os pontos finais v 1.0 e a v 2.0 code_challenges S256 e texto simples. 

Para obter mais informações, consulte o pedido um código de autorização[](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-protocols-oauth-code#request-an-authorization-code). 

 
---
 

### <a name="support-for-provisioning-all-user-attribute-values-available-in-the-workday-getworkers-api"></a>Suporte para o aprovisionamento de todos os valores de atributo de utilizador disponíveis na Workday Get_Workers API

**Tipo:** nova funcionalidade  
**Categoria de serviço:** aprovisionamento de aplicações  
**Capacidade de produto:** 3rd integração de terceiros
 
A pré-visualização pública da entrada de aprovisionamento do Workday para o Active Directory e o Azure AD agora suporta todos os valores de atributo disponíveis na Workday Get_Workers API de aprovisionamento e a capacidade, a extrair. Esta ação adiciona suporte para centenas de padrão adicional e entrada de atributos personalizados além dos fornecidos com a versão inicial do evidenciam aprovisionamento conector.

Para obter mais informações, consulte: [personalizar a lista de atributos de utilizador do Workday](https://docs.microsoft.com/azure/active-directory/active-directory-saas-workday-inbound-tutorial#customizing-the-list-of-workday-user-attributes)

---



### <a name="changing-group-membership-from-dynamic-to-static-and-vice-versa"></a>Alterar associação ao grupo de dinâmico como estático e vice versa

**Tipo:** nova funcionalidade  
**Categoria de serviço:** grupo de gestão  
**Capacidade de produto:** colaboração
 
É possível alterar como é gerida a associação num grupo. Isto é útil quando pretender manter o mesmo nome de grupo e ID no sistema, todas as referências existentes para o grupo ainda são válidas; criar um novo grupo seria necessários atualizar essas referências.
Atualizámos o Centro de administração do Azure AD para adicionar suporte esta funcionalidade. Agora, os clientes podem converter grupos existentes de associação dinâmica associação atribuída e vice-versa. Os cmdlets do PowerShell existentes também ainda estão disponíveis.

Para obter mais informações, consulte [a alteração de associação dinâmica para estática e vice-versa](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal#changing-dynamic-membership-to-static-and-vice-versa)

 

 
---
 

### <a name="improved-sign-out-behavior-with-seamless-sso"></a>Comportamento de início de sessão melhorado com o SSO totalmente integrada

**Tipo:** funcionalidade foi alterado  
**Categoria de serviço:** autenticações (inícios de sessão)  
**Capacidade de produto:** autenticação de utilizador
 
Anteriormente, mesmo que os utilizadores com sessão iniciada explicitamente fora de uma aplicação protegida pelo Azure AD, estes seriam ser iniciadas automaticamente no utilizando SSO totalmente integrado se que estava a tentar aceder a uma aplicação do Azure AD novamente na sua rede empresarial dos seus dispositivos associados a um domínio. Com esta alteração, a sessão é suportada.  Isto permite aos utilizadores escolher do Azure idêntica ou diferente conta do AD para iniciar sessão novamente em, em vez de a ser iniciada automaticamente sessão através de SSO totalmente integrada.

Para obter mais informações, consulte [do Azure Active Directory totalmente integrada Single Sign-On](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso)

 
---
 

### <a name="application-proxy-connector-version-154020-released"></a>Versão do conector de Proxy de aplicação 1.5.402.0 lançadas

**Tipo:** funcionalidade foi alterado  
**Categoria de serviço:** do Proxy de aplicação  
**Capacidade de produto:** segurança de identidade e de proteção
 
Esta versão do conector gradualmente está a ser revertida através de Novembro. Esta nova versão do conector inclui as seguintes alterações:

- O conector agora define os cookies de nível de domínio em vez disso, ao nível do subdomínio. Isto garante uma experiência SSO smoother e evita os pedidos de autenticação redundante.
- Suporte para pedidos de codificação fragmentados
- Monitorização de estado de funcionamento do conector melhorada 
- Várias correções de erros e melhorias estabilidade

Para obter mais informações, consulte [conetores da Proxy da aplicação Azure compreender AD](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors).

 
---
 

 



## <a name="february-2018"></a>De 2018 Fevereiro
 

### <a name="improved-navigation-for-managing-users-and-groups"></a>Navegação melhorada para gerir utilizadores e grupos

**Tipo:** plano de alteração  
**Categoria de serviço:** Directory Management  
**Capacidade de produto:** diretório
 

A experiência de navegação para gerir utilizadores e grupos foi está mais simples. Pode agora navegar a partir da descrição geral do diretamente à lista de todos os utilizadores com acesso fácil à lista de utilizadores eliminados. Também pode navegar da descrição de geral do diretamente à lista de todos os grupos, mais fácil acesso às definições do grupo de gestão. E também da página de descrição geral de diretório, pode procurar um utilizador, o grupo, a aplicação empresarial ou o registo de aplicação.
 

---


### <a name="availability-of-sign-ins-and-audit-reports-in-microsoft-azure-operated-by-21vianet-azure-china-21vianet"></a>Relatórios de disponibilidade de auditoria e inícios de sessão no Microsoft Azure operado pela 21Vianet (21Vianet Azure China)

**Tipo:** nova funcionalidade  
**Categoria de serviço:** Sovereign nuvens  
**Capacidade de produto:** monitorização e relatórios
 

Relatórios de registo de atividade do AD do Azure estão agora disponíveis no Microsoft Azure operado pela 21Vianet (Azure China 21Vianet) instâncias. Os seguintes registos são incluídos:

- **Os registos de atividade de inícios de sessão** -inclui todos os inícios registos associados ao seu inquilino.

- **Self-service registos de auditoria de palavra-passe** -inclui todos os registos de auditoria SSPR.

- **Os registos de auditoria de gestão de diretório** -inclui todas a gestão de diretório relacionadas com registos como utilizador gestão, gestão de aplicações e outros utilizadores de auditoria.

Com estes registos, pode obter informações sobre a forma como o ambiente está a fazer. Os dados fornecidos permite-lhe:

- Determine a forma como as suas aplicações e serviços são utilizados pelos seus utilizadores.

- Resolva problemas de impedir que os utilizadores a obter o seu trabalho.

Para obter mais informações sobre como utilizar estes relatórios, consulte [relatórios do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal).
 

---


### <a name="use-report-reader-role-non-admin-role-to-view-azure-ad-activity-reports"></a>Utilize a função de "Relatório leitor" (função de não administrador) para ver relatórios de atividade do Azure AD

**Tipo:** nova funcionalidade  
**Categoria de serviço:** relatórios  
**Capacidade de produto:** monitorização e relatórios
 

Como parte dos comentários de clientes para ativar as funções de administrador de não ter acesso a atividade do Azure AD registos, iremos ter ativada a capacidade para os utilizadores que estão na função "Leitor do relatório" para acesso inícios de sessão e a atividade de auditoria no Portal do Azure, bem como utilizar os APIs de gráfico. 

Para obter mais informações, como utilizar estes relatórios, consulte [relatórios do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal). 

---
 


### <a name="employeeid-claim-available-as-user-attribute-and-user-identifier"></a>Campo IDdeEmpregado afirmação disponível como atributo de utilizador e o identificador de utilizador

**Tipo:** nova funcionalidade  
**Categoria de serviço:** aplicações da empresa  
**Capacidade de produto:** SSO
 

Pode configurar **campo IDdeEmpregado** como o identificador de utilizador e o atributo de utilizador para os utilizadores de membro e convidados B2B SAML início de sessão aplicações baseadas na IU da aplicação empresarial.

Para obter mais informações, consulte [personalizar afirmações emitidas no token SAML para aplicações da empresa no Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).
 

---


### <a name="simplified-application-management-using-wildcards-in-azure-ad-application-proxy"></a>Gestão de aplicações simplificada utilizando carateres universais no Proxy de aplicações do Azure AD

**Tipo:** nova funcionalidade  
**Categoria de serviço:** do Proxy de aplicação  
**Capacidade de produto:** autenticação de utilizador
 

Para facilitar a implementação de aplicação e reduzem o overhead administrativo, agora suportamos a capacidade de publicar aplicações utilizando carateres universais. Para publicar uma aplicação de carateres universais, pode seguir o fluxo de publicação de aplicações padrão, mas utilizar um caráter universal nos URLs internos e externos.

Para obter mais informações, consulte [aplicações universais do proxy de aplicações do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-wildcard)

 

---
 
### <a name="new-cmdlets-to-support-configuration-of-application-proxy"></a>Novos cmdlets para suportar a configuração do Proxy da aplicação

**Tipo:** nova funcionalidade  
**Categoria de serviço:** do Proxy de aplicação  
**Capacidade de produto:** plataforma
 

A versão mais recente do módulo do PowerShell de AzureAD pré-visualização contém novos cmdlets que permite aos clientes configurar as aplicações de Proxy de aplicações através do PowerShell.

Os novos cmdlets são: 

- Get-AzureADApplicationProxyApplication
- Get-AzureADApplicationProxyApplicationConnectorGroup
- Get-AzureADApplicationProxyConnector
- Get-AzureADApplicationProxyConnectorGroup
- Get-AzureADApplicationProxyConnectorGroupMembers
- Get-AzureADApplicationProxyConnectorMemberOf
- New-AzureADApplicationProxyApplication
- New-AzureADApplicationProxyConnectorGroup
- Remove-AzureADApplicationProxyApplication
- Remove-AzureADApplicationProxyApplicationConnectorGroup
- Remove-AzureADApplicationProxyConnectorGroup
- Set-AzureADApplicationProxyApplication
- Set-AzureADApplicationProxyApplicationConnectorGroup
- Set-AzureADApplicationProxyApplicationCustomDomainCertificate
- Set-AzureADApplicationProxyApplicationSingleSignOn
- Set-AzureADApplicationProxyConnector
- Set-AzureADApplicationProxyConnectorGroup


 

---
 

### <a name="new-cmdlets-to-support-configuration-of-groups"></a>Novos cmdlets para suportar a configuração de grupos

**Tipo:** nova funcionalidade  
**Categoria de serviço:** do Proxy de aplicação  
**Capacidade de produto:** plataforma
 

A versão mais recente do módulo do AzureAD PowerShell contém cmdlets para gerir grupos no Azure AD. Estes cmdlets anteriormente estavam disponíveis no módulo AzureADPreview e agora são adicionados ao módulo AzureAD

Os cmdlets de grupo que são versão agora para disponibilidade geral são: 

- Get-AzureADMSGroup
- New-AzureADMSGroup
- Remove-AzureADMSGroup
- Set-AzureADMSGroup
- Get-AzureADMSGroupLifecyclePolicy
- New-AzureADMSGroupLifecyclePolicy
- Remove-AzureADMSGroupLifecyclePolicy
- Add-AzureADMSLifecyclePolicyGroup
- Remove-AzureADMSLifecyclePolicyGroup
- Reset-AzureADMSLifeCycleGroup   
- Get-AzureADMSLifecyclePolicyGroup
 

---
 
### <a name="a-new-release-of-azure-ad-connect-is-available"></a>Uma nova versão do Azure AD Connect está disponível

**Tipo:** nova funcionalidade  
**Categoria de serviço:** AD Sync  
**Capacidade de produto:** plataforma
 

O Azure AD Connect é a ferramenta preferida para sincronizar dados entre o Azure AD e nas origens de dados no local, incluindo o Windows Server Active Directory e LDAP.

**Importante**
 
Este compilação apresenta esquema e sincronizar as alterações da regra. O serviço do Azure AD Connect sincronização aciona uma importação completa e a sincronização completa passos após uma atualização. Para obter informações sobre como alterar este comportamento, consulte [como diferir sincronização completa após a atualização](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version#how-to-defer-full-synchronization-after-upgrade).

Esta versão tem as seguintes atualizações e alterações:

**Problemas fixos**

- Corrija a janela de temporização tarefas em segundo plano para a página Paritition filtragem quando mudar para a página seguinte.
- Corrigido um erro que provocou a violação de acesso durante a ação personalizada ConfigDB.
- Corrigido um erro ao recuperar a partir de tempo limite de ligação do SQL Server.
- Corrigido um erro em certificados com carateres universais de SAN falharem a verificação de pré-req.
- Corrigido um erro que faz com que miiserver.exe falhas durante a exportação de conector AAD.
- Corrigido um erro que tentativa de palavra-passe incorreta registada no DC quando executar o AAD connect Assistente para alterar a configuração

**Funcionalidades novas e melhoradas**

- Para GDPR, iremos são necessárias para indicar os tipos de dados de cliente que são partilhados com a Microsoft (telemetria, estado de funcionamento, etc.), tem ligações a documentação online detalhada e fornecem uma forma para que possa alterar as suas preferências.  Esta verificação-adiciona o seguinte:
    - Partilha de dados e a notificação de privacidade no limpa instalar página EULA.

    - Privacidade e de partilha de notificação de dados na página de atualização.

    - Uma nova tarefa adicional **definições de privacidade** onde o utilizador pode alterar as respetivas preferências.
 
- Telemetria do Application - administradores podem mudar este tipo de dados /.

- Dados de estado de funcionamento do AD do Azure - os administradores têm de poderem visitar o portal de estado de funcionamento para controlar as definições de estado de funcionamento. Depois da política de serviço foi alterada, os agentes serão ler e impor-lo.

- Adicionar ações de configuração de repetição de escrita do dispositivo e uma barra de progresso para a inicialização da página.

- Melhoria geral diagnóstico com o relatório HTML e recolha de dados completa num texto ZIP / relatório HTML.

- Fiabilidade melhorada auto atualizar e adicionar telemetria adicional para garantir que é possível determinar o estado de funcionamento do servidor.

- Restringir as permissões disponíveis para contas com privilégios na conta do conector do AD. Para novas instalações, o assistente limita as permissões que contas privilegiadas ter em conta o MSOL depois de criar a conta MSOL. As alterações afetam as instalações de rápida e instalações personalizadas com a criação de conta.

- Alterar o instalador para não exigir privilégio SA na instalação de raiz do AADConnect.

- Utilitário de novo para resolver problemas de sincronização para um objeto específico. Atualmente, o utilitário verifica o seguinte:

    - Erro de correspondência de UserPrincipalName entre objetos de utilizador sincronizadas e a conta de utilizador no inquilino do Azure AD.
  
    - Se o objeto está filtrado de sincronização devido a filtragem de domínio
  
    - Se o objeto está filtrado de sincronização devido a unidade organizacional (UO) filtragem

- Utilitário de novo para sincronizar o hash de palavra-passe atual armazenado no diretório do Active Directory no local para uma conta de utilizador específica. O utilitário não necessita de uma alteração de palavra-passe. 
 

---
 

### <a name="applications-supporting-intune-app-protection-policies-added-for-use-with-azure-ad-application-based-conditional-access"></a>As aplicações a políticas de proteção de aplicação do Intune suporte adicionadas para utilizar com acesso condicional de baseadas em aplicações do Azure AD

**Tipo:** funcionalidade foi alterado  
**Categoria de serviço:** acesso condicional  
**Capacidade de produto:** segurança de identidade e de proteção
 

Foi adicionado mais aplicações que suportam o acesso condicional baseado na aplicação. Agora, pode obter acesso ao Office 365 e outras aplicações de nuvem do Azure AD ligados utilizando estas aplicações de cliente aprovada.

As seguintes aplicações serão adicionadas no final de Fevereiro 

- Microsoft PowerBI

- Microsoft Launcher

- Faturação da Microsoft

Para obter mais informações, consulte:

- [Requisito da aplicação de cliente aprovada](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [Azure AD com base na aplicação acesso condicional](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam)

 

---
 

### <a name="terms-of-use-update-to-mobile-experience"></a>Termos de utilização da atualização para a experiência de móvel 

**Tipo:** funcionalidade foi alterado  
**Categoria de serviço:** os termos de utilização  
**Capacidade de produto:** conformidade
 

Quando são apresentados os termos de utilização, pode agora, clique em **ter visualização de relatórios de problemas? Clique aqui**. Ao clicar nesta hiperligação abre os termos de utilização nativamente no seu dispositivo. Independentemente do tamanho do tipo de letra do documento ou o tamanho de ecrã do dispositivo, pode aplicar zoom e ler o documento conforme necessário. 
 

---
 
## <a name="january-2018"></a>De 2018 Janeiro
 

### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Novas federado aplicações disponíveis na Galeria de aplicações do Azure AD 

**Tipo:** nova funcionalidade  
**Categoria de serviço:** aplicações da empresa  
**Capacidade de produto:** 3rd integração de terceiros
 

Em Janeiro de 2018, foram adicionadas as seguintes novas aplicações com suporte de Federação na Galeria de aplicações:

[IBM OpenPages](https://go.microsoft.com/fwlink/?linkid=864698), [Software de gestão da privacidade de OneTrust](https://go.microsoft.com/fwlink/?linkid=861660), [Dealpath](https://go.microsoft.com/fwlink/?linkid=863526), [IriusRisk federado diretório](https://go.microsoft.com/fwlink/?linkid=864699) e [fidelidade NetBenefits](https://go.microsoft.com/fwlink/?linkid=864701).

Para obter uma descrição completa de todos os tutoriais disponíveis, consulte [integração de aplicações de SaaS no Azure Active Directory](https://aka.ms/appstutorial).
 

---
 


### <a name="sign-in-with-additional-risk-detected"></a>Início de sessão com risco adicional detetado

**Tipo:** nova funcionalidade  
**Categoria de serviço:** Identity Protection  
**Capacidade de produto:** segurança de identidade e de proteção
 

As informações que obtém para um evento de risco detetados está associada à sua subscrição do Azure AD. Com a edição do Azure AD Premium P2, obter as informações mais detalhadas sobre todas as deteções subjacentes.

Com a edição do Azure AD Premium P1, deteções que não são abrangidas por sua licença são apresentados como o evento de risco início de sessão com o risco adicional detetado.

Para obter mais informações, consulte [Eventos de risco do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-risk-events).
 

---

### <a name="hide-office-365-applications-from-end-users-access-panels"></a>Ocultar a painéis de acesso do utilizador final de aplicações do Office 365

**Tipo:** nova funcionalidade  
**Categoria de serviço:** aplicações My  
**Capacidade de produto:** SSO
 

Pode agora melhor gerir como aplicações do Office 365 mostram no painel de acesso do utilizador através de uma nova definição de utilizador. Esta opção é útil para reduzir a quantidade de aplicações no painel de acesso de um utilizador, se preferir Mostrar apenas as aplicações do Office no portal do Office. A definição está localizada no **as definições de utilizador** e assinalada como **os utilizadores apenas podem ver as aplicações do Office 365 no portal do Office 365**.
 

Para obter mais informações, consulte [ocultar uma aplicação da experiência do utilizador no Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app).

---
 


### <a name="seamless-sign-into-apps-enabled-for-password-sso-directly-from-apps-url"></a>Início de sessão totalmente integrado em aplicações ativadas para SSO de palavra-passe diretamente a partir do URL da aplicação 

**Tipo:** nova funcionalidade  
**Categoria de serviço:** aplicações My  
**Capacidade de produto:** SSO
 

A extensão de browser de aplicações My está agora disponível através de uma ferramenta conveniente que dá-lhe os minhas aplicações início de sessão único na capacidade como um atalho no seu browser. Depois de instalar o utilizador verá um ícone de waffle no browser que proporciona-lhes acesso rápido às aplicações. Os utilizadores podem agora beneficiar do:

- A capacidade para iniciar sessão diretamente no SSO de palavra-passe com base em aplicações a partir da página de início de sessão da aplicação
- Iniciar qualquer aplicação utilizando a funcionalidade de pesquisa rápida
- Atalhos para aplicações recentemente utilizados da extensão do
- A extensão está disponível para o limite, o Chrome e o Firefox.
 
Para obter mais informações, consulte [segura de aplicações My início de sessão extensão](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction#my-apps-secure-sign-in-extension).

---

### <a name="azure-ad-administration-experience-in-azure-classic-portal-has-been-retired"></a>A administração do Azure AD foi retirada experiência de utilização no portal clássico do Azure

**Tipo:** preterido   
**Categoria de serviço:** do Azure AD  
**Capacidade de produto:** diretório
 

A partir de 8 de Janeiro de 2018, a administração do Azure AD foi retirada experiência de utilização no portal clássico do Azure. Isto demorou local em conjunto com a extinção do portal clássico do Azure em si. Doravante, deve utilizar o [Centro de administração do Azure AD](https://aad.portal.azure.com) todos os seus administração baseada no portal do Azure AD.
 
---

### <a name="the-phonefactor-web-portal-has-been-retired"></a>O portal web do PhoneFactor foi extinguido.

**Tipo:** preterido  
**Categoria de serviço:** do Azure AD  
**Capacidade de produto:** diretório
 

A partir de 8 de Janeiro de 2018, o portal web do PhoneFactor foi extinguido. Este portal foi utilizado para a administração do servidor MFA, mas essas funções terem sido movidas para o portal do Azure em portal.azure.com. 

A configuração da MFA está localizada em: **do Azure Active Directory \> servidor MFA**
 
---
 
### <a name="deprecate-azure-ad-reports"></a>Despromover o relatórios do Azure AD


**Tipo:** preterido  
**Categoria de serviço:** relatórios  
**Capacidade de produto:** gestão de ciclo de vida de identidade  


Com a disponibilidade geral da nova consola de administração do Active Directory do Azure e novas APIs agora disponíveis para relatórios de atividade e segurança, o relatório APIs em ponto final "/ relatórios" foi descontinuado a partir do fim do dia 31 de Dezembro de 2017.


**O que está disponível?**

Como parte da transição para a nova consola de administração, efetuamos 2 novas APIs disponíveis para obter os registos de atividade do Azure AD. O novo conjunto de APIs fornecem mais rico de filtragem e ordenação funcionalidade além de proporcionarem auditoria mais completas e as atividades de início de sessão. Os dados previamente disponíveis através de relatórios de segurança agora podem ser acedidos através de eventos de risco Identity Protection API no Microsoft Graph.

Para obter mais informações, consulte:

- [Introdução ao Azure Active Directory API do relatório](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal)

- [Introdução ao Azure Active Directory Identity Protection e o Microsoft Graph](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection-graph-getting-started)


---


## <a name="december-2017"></a>Dezembro de 2017
 

### <a name="terms-of-use-in-the-access-panel"></a>Termos de utilização no painel de acesso

**Tipo:** nova funcionalidade  
**Categoria de serviço:** termos de utilização  
**Capacidade de produto:** conformidade
 
Agora pode voltar ao painel de acesso e ver os termos de utilização que anteriormente aceites.

Siga estes passos.

1. Vá para o [MyApps portal](https://myapps.microsoft.com)e iniciar sessão.

2. No canto superior direito, selecione o nome e, em seguida, selecione **perfil** da lista. 

3. No seu **perfil**, selecione **rever os termos de utilização**. 

4. Agora pode rever os termos de utilização aceite. 

Para obter mais informações, consulte o [termos do Azure AD da funcionalidade de utilização (pré-visualização)](https://docs.microsoft.com/azure/active-directory/active-directory-tou).
 
---
 

### <a name="new-azure-ad-sign-in-experience"></a>Nova experiência de início de sessão do Azure AD

**Tipo:** nova funcionalidade  
**Categoria de serviço:** do Azure AD  
**Capacidade de produto:** autenticação de utilizador
 
O Azure AD e o sistema de identidade de conta do Microsoft UIs foram redesenhadas para que possam terem uma aspeto e funcionalidade consistentes. Além disso, a página de início de sessão do AD do Azure recolhe o nome de utilizador em primeiro lugar, seguido a credencial num ecrã segundo.

Para obter mais informações, consulte [a nova experiência de início de sessão do Azure AD está agora em pré-visualização pública](https://cloudblogs.microsoft.com/enterprisemobility/2017/08/02/the-new-azure-ad-signin-experience-is-now-in-public-preview/).
 
---
 

### <a name="fewer-sign-in-prompts-a-new-keep-me-signed-in-experience-for-azure-ad-sign-in"></a>Pedidos de início de sessão menos: uma experiência de novo "manter a minha sessão iniciada" para o início de sessão do Azure AD

**Tipo:** nova funcionalidade  
**Categoria de serviço:** do Azure AD  
**Capacidade de produto:** autenticação de utilizador
 
O **manter a minha sessão iniciada** caixa de verificação a página de início de sessão do Azure AD foi substituída por uma nova linha que aparece depois de se autenticar com êxito. 

Se lhe responder **Sim** para esta linha de comandos, o serviço dá-lhe um token de atualização persistente. Este comportamento é o mesmo que o se tiver selecionado o **manter a minha sessão iniciada** caixa de verificação a experiência de antiga. Para inquilinos federados, esta linha de comandos mostra depois de se autenticar com êxito com o serviço federado.

Para obter mais informações, consulte [menos avisos de início de sessão: A nova experiência de "manter a minha sessão iniciada" para o Azure AD está em pré-visualização](https://cloudblogs.microsoft.com/enterprisemobility/2017/09/19/fewer-login-prompts-the-new-keep-me-signed-in-experience-for-azure-ad-is-in-preview/). 

---
 

### <a name="add-configuration-to-require-the-terms-of-use-to-be-expanded-prior-to-accepting"></a>Adicionar configuração para exigir que os termos de utilização para ser expandido antes de a aceitar

**Tipo:** nova funcionalidade  
**Categoria de serviço:** termos de utilização  
**Capacidade de produto:** conformidade
 
Uma opção para os administradores requer que os utilizadores expandir os termos de utilização antes de aceitar os termos.

Selecione **no** ou **desativar** pedir aos utilizadores expandir os termos de utilização. O **no** definição exige que os utilizadores ver os termos de utilização antes de a aceitá-los.

Para obter mais informações, consulte o [termos do Azure AD da funcionalidade de utilização (pré-visualização)](https://docs.microsoft.com/azure/active-directory/active-directory-tou).
 
---
 

### <a name="scoped-activation-for-eligible-role-assignments"></a>Ativação âmbito existência de atribuições de função elegíveis

**Tipo:** nova funcionalidade  
**Categoria de serviço:** Privileged Identity Management  
**Capacidade de produto:** Privileged Identity Management
 
Pode utilizar ativação âmbito para ativar as atribuições de função de recursos do Azure elegível com menos autonomia em que as predefinições de atribuição original. Um exemplo é se estiver atribuído como o proprietário de uma subscrição no seu inquilino. Com a ativação de âmbito, pode ativar a função de proprietário de até cinco recursos contidos dentro da subscrição (tais como grupos de recursos e máquinas virtuais). A ativação de controlo de âmbito, poderá reduzir a possibilidade de executar indesejáveis alterações a recursos do Azure críticos.

Para obter mais informações, consulte [que é o Azure AD Privileged Identity Management?](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure).
 
---
 

### <a name="new-federated-apps-in-the-azure-ad-app-gallery"></a>Novas aplicações federadas na Galeria de aplicações do Azure AD

**Tipo:** nova funcionalidade  
**Categoria de serviço:** aplicações da empresa  
**Capacidade de produto:** 3rd integração de terceiros
 
Dezembro de 2017, as seguintes novas aplicações com suporte de Federação foram adicionadas na Galeria de aplicações:

[Accredible](https://go.microsoft.com/fwlink/?linkid=863523), Gestor de experiência de Adobe, [EFI Digital StoreFront](https://go.microsoft.com/fwlink/?linkid=861685), [Communifire](https://go.microsoft.com/fwlink/?linkid=861676) CybSafe, [FactSet](https://go.microsoft.com/fwlink/?linkid=863525), [imagem FUNCIONA](https://go.microsoft.com/fwlink/?linkid=863517), [MOBI](https://go.microsoft.com/fwlink/?linkid=863521), [MobileIron do Azure AD integração](https://go.microsoft.com/fwlink/?linkid=858027), [Reflektive](https://go.microsoft.com/fwlink/?linkid=863518), [SAML SSO para Bamboo pela resolução GmbH](https://go.microsoft.com/fwlink/?linkid=863520), [SAML SSO para Bitbucket pela resolução GmbH](https://go.microsoft.com/fwlink/?linkid=863519), [Vodeclic](https://go.microsoft.com/fwlink/?linkid=863522), WebHR, a integração do Zenegy Azure AD.

Para obter uma descrição completa de todos os tutoriais disponíveis, consulte [integração de aplicações de SaaS no Azure Active Directory](https://aka.ms/appstutorial).

 
---
 

### <a name="approval-workflows-for-azure-ad-directory-roles"></a>Fluxos de trabalho de aprovação para funções de diretório do Azure AD

**Tipo:** funcionalidade foi alterado  
**Categoria de serviço:** Privileged Identity Management  
**Capacidade de produto:** Privileged Identity Management
 
Fluxo de trabalho de aprovação para funções de diretório do Azure AD é geralmente disponível.

Com o fluxo de trabalho de aprovação, os administradores de função privilegiada podem exigir a membros da função de elegíveis para pedir a ativação de função antes de poder utilizar a função com privilégios. Vários utilizadores e grupos podem ser delegada aprovação responsabilidades. Membros da função elegível recebem notificações quando aprovação estiver concluída e a respetiva função está ativa.

---
 

### <a name="pass-through-authentication-skype-for-business-support"></a>A autenticação pass-through: Skype para suporte de negócio

**Tipo:** funcionalidade foi alterado  
**Categoria de serviço:** autenticações (inícios de sessão)  
**Capacidade de produto:** autenticação de utilizador


Agora a autenticação pass-through suporta utilizador inícios de sessão ao Skype para aplicações de cliente de negócio que suportam a autenticação moderna, que inclui o online e topologias híbridas. 

Para obter mais informações, consulte [Skype para topologias de negócio suportado com a autenticação moderna](https://technet.microsoft.com/library/mt803262.aspx).
 
---
 

### <a name="updates-to-azure-ad-privileged-identity-management-for-azure-rbac-preview"></a>Atualizações para o Azure AD Privileged Identity Management para RBAC do Azure (pré-visualização)

**Tipo:** funcionalidade foi alterado  
**Categoria de serviço:** Privileged Identity Management  
**Capacidade de produto:** Privileged Identity Management
 
Com a atualização de pré-visualização pública do Azure AD Privileged Identity Management (PIM) para controlo de acesso em funções do Azure (RBAC), pode agora:

* Utilize administração Just Enough.
* Exigir a aprovação para ativar as funções de recursos.
* Agende uma ativação futura de uma função que requeira a aprovação para ambas do Azure AD e funções do RBAC do Azure.

 
Para obter mais informações, consulte [Privileged Identity Management para recursos do Azure (pré-visualização)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).

 
---
 
## <a name="november-2017"></a>Novembro de 2017
 
### <a name="access-control-service-retirement"></a>Extinção do serviço de controlo de acesso



**Tipo:** plano de alteração  
**Categoria de serviço:** serviço de controlo de acesso  
**Capacidade de produto:** serviço de controlo de acesso 


 Azure Active Directory controlo de acesso (também conhecido como o serviço de controlo de acesso) será descontinuado dentro de enlace tardio 2018. Obter mais informações, que inclui uma agenda de detalhado e a orientação de migração de elevado nível irão ser fornecidas de algumas semanas seguintes. Pode deixar comentários nesta página com quaisquer perguntas sobre o serviço de controlo de acesso e um membro do agrupamento irá respondê-los.

---

### <a name="restrict-browser-access-to-the-intune-managed-browser"></a>Restringir o acesso ao browser para o Browser gerido do Intune 


**Tipo:** plano de alteração  
**Categoria de serviço:** acesso condicional  
**Capacidade de produto:** identidade de segurança e proteção




Pode restringir o acesso do browser para o Office 365 e outras aplicações de nuvem do Azure AD ligados utilizando o Browser gerido do Intune como uma aplicação aprovada. 

Agora pode configurar a seguinte condição de acesso condicional baseado na aplicação:

**Aplicações de cliente:** Browser

**O que é o efeito da alteração?**

Hoje, o acesso é bloqueado quando utilizar esta condição. Quando a pré-visualização está disponível, todos os acessos irão exigir a utilização da aplicação de browser gerido. 

Procure esta capacidade e obter mais informações nas notas de lançamento e blogues futuras. 

Para obter mais informações, consulte [de acesso condicional no Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).

 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Novas aplicações de cliente aprovada para acesso condicional de baseado em aplicações do Azure AD

 
**Tipo:** plano de alteração  
**Categoria de serviço:** acesso condicional  
**Capacidade de produto:** identidade de segurança e proteção




As seguintes aplicações estão a ser planeadas a adicionar à lista de [aprovados aplicações de cliente](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement):

- [Microsoft Kaizala](https://www.microsoft.com/garage/profiles/kaizala/)
- [Microsoft StaffHub](https://staffhub.office.com/what-it-is)


Para obter mais informações, consulte:

- [Requisito da aplicação de cliente aprovada](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [Azure AD com base na aplicação acesso condicional](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam)


---

### <a name="terms-of-use-support-for-multiple-languages"></a>Suporte de termos de utilização de vários idiomas



**Tipo:** nova funcionalidade    
**Categoria de serviço:** termos de utilização  
**Capacidade de produto:** conformidade





Agora, os administradores podem criar novos termos de utilização que contêm vários documentos PDF. Pode marcar estes documentos PDF com um idioma correspondente. Os utilizadores são apresentados o PDF com o com base nas respetivas preferências de idioma correspondente. Se não houver nenhuma correspondência, é apresentado o idioma predefinido.


---
 

### <a name="real-time-password-writeback-client-status"></a>Estado do cliente de repetição de escrita de palavras-passe em tempo real



**Tipo:** nova funcionalidade  
**Categoria de serviço:** reposição de palavra-passe self-service  
**Capacidade de produto:** autenticação de utilizador


 

Agora pode rever o estado do seu cliente de repetição de escrita de palavras-passe no local. Esta opção está disponível no **integração no local** secção o [reposição de palavra-passe](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset) página. 

Se existirem problemas com a ligação para o seu cliente de repetição de escrita no local, verá uma mensagem de erro que fornece-lhe:

- Informações sobre a razão pela qual não é possível ligar ao seu cliente de repetição de escrita no local.
- Uma ligação para a documentação que ajuda a resolver o problema. 


Para obter mais informações, consulte [integração no local](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-how-it-works#on-premises-integration).

 
---


### <a name="azure-ad-app-based-conditional-access"></a>Azure AD com base na aplicação acesso condicional 



 
**Tipo:** nova funcionalidade  
**Categoria de serviço:** do Azure AD  
**Capacidade de produto:** identidade de segurança e proteção





Agora pode restringir o acesso ao Office 365 e outras aplicações de nuvem do Azure AD ligados para [aprovados aplicações de cliente](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement) que suportam as políticas de proteção de aplicação do Intune através da utilização de [acesso condicional de baseado em aplicações do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam). Políticas de proteção de aplicações do Intune são utilizadas para configurar e proteger dados da empresa nestas aplicações de cliente.

Ao combinar [baseado na aplicação](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam) com [baseado nos dispositivos](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-policy-connected-applications) políticas de acesso condicional, tem a flexibilidade para proteger os dados pessoais e de dispositivos da empresa.

As seguintes condições e os controlos estão agora disponíveis para utilização com acesso condicional baseado na aplicação:

**Condição de plataforma suportada**

- iOS
- Android

**Condição de aplicações de cliente**

- Aplicações móveis e clientes de ambiente de trabalho

**Controlo de acesso**

- Requer aplicação aprovada do cliente


Para obter mais informações, consulte [acesso condicional de baseado em aplicações do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam).

 
---

### <a name="manage-azure-ad-devices-in-the-azure-portal"></a>Gerir dispositivos do Azure AD no portal do Azure



**Tipo:** nova funcionalidade  
**Categoria de serviço:** registo de dispositivos e gestão  
**Capacidade de produto:** identidade de segurança e proteção

 



Agora pode encontrar todos os dispositivos ligados ao Azure AD e as atividades relacionadas com o dispositivo num único local. Há uma nova experiência de administração para gerir identidades de dispositivo e definições no portal do Azure. Nesta versão, pode:

- Ver todos os dispositivos que estão disponíveis para o acesso condicional no Azure AD.
- Ver propriedades, que incluem a híbrida do Azure dispositivos associados a um AD.
- Localizar as chaves do BitLocker para os seus dispositivos associados a um AD do Azure, gerir o seu dispositivo com o Intune e muito mais.
- Gerir definições relacionadas com o dispositivo do Azure AD.

Para obter mais informações, consulte [gerir dispositivos através do portal do Azure](https://docs.microsoft.com/azure/active-directory/device-management-azure-portal).



 
---

### <a name="support-for-macos-as-a-device-platform-for-azure-ad-conditional-access"></a>Suporte para macOS como uma plataforma de dispositivo para o acesso condicional do Azure AD 



**Tipo:** nova funcionalidade    
**Categoria de serviço:** acesso condicional  
**Capacidade de produto:** identidade de segurança e proteção 
 

Agora pode incluir (ou excluir) macOS como uma condição de plataforma do dispositivo na sua política de acesso condicional do Azure AD. Com a adição de macOS para as plataformas de dispositivos suportados, pode:

- **Inscrever e gerir dispositivos macOS através do Intune.** Semelhante para outras plataformas, como iOS e Android, uma aplicação do portal da empresa está disponível para macOS fazer inscrições unificadas. Pode utilizar a nova aplicação do portal da empresa para macOS para inscrever um dispositivo com o Intune e registá-lo com o Azure AD.
- **Certifique-se de que os dispositivos macOS cumprem as políticas de conformidade da sua organização definidas no Intune.** No Intune no portal do Azure, agora pode configurar políticas de conformidade para dispositivos macOS. 
- **Restringir o acesso a aplicações no Azure AD apenas a dispositivos conformes macOS.** Criação de política de acesso condicional tem macOS como uma opção de plataforma de dispositivos separadas. Agora pode criar políticas de acesso condicional macOS específicas para a aplicação de destino definido no Azure.

Para obter mais informações, consulte:

- [Criar uma política de conformidade para dispositivos macOS com o Intune](https://aka.ms/macoscompliancepolicy)
- [Acesso condicional no Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)


 
---

### <a name="network-policy-server-extension-for-azure-multi-factor-authentication"></a>Extensão de servidor de políticas de rede para o multi-factor Authentication do Azure 


**Tipo:** nova funcionalidade    
**Categoria de serviço:** multi-factor authentication  
**Capacidade de produto:** autenticação de utilizador




A extensão do servidor de políticas de rede de Azure multi-factor Authentication adiciona funcionalidades de baseado na nuvem a autenticação multifator para a sua infraestrutura de autenticação utilizando os servidores existentes. Com a extensão de servidor de políticas de rede, pode adicionar a chamada telefónica, mensagem de texto ou verificação de aplicação de telefone para o fluxo de autenticação existente. Não tem de instalar, configurar e manter novos servidores. 

Esta extensão foi criada para as organizações que pretendem proteger as ligações de rede privada virtual sem a implementar o servidor do Azure multi-factor Authentication. O servidor de políticas de rede extensão age como um adaptador entre o RADIUS e baseado na nuvem do Azure multi-factor Authentication para fornecer um segundo fator de autenticação para federada ou utilizadores sincronizados.


Para obter mais informações, consulte [integrar a sua infraestrutura de servidor de políticas de rede existente com o Azure multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-nps-extension).

 
---

### <a name="restore-or-permanently-remove-deleted-users"></a>Restaure ou remover utilizadores eliminados permanentemente


**Tipo:** nova funcionalidade    
**Categoria de serviço:** gestão de utilizadores  
**Capacidade de produto:** diretório 



No Centro de administração do Azure AD, pode agora:

- Restaure um utilizador eliminado. 
- Elimine permanentemente um utilizador.


**Para experimentar:**

1. No Centro de administração do Azure AD, selecione [todos os utilizadores](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All) no **gerir** secção. 

2. Do **mostrar** lista, selecione **recentemente eliminado utilizadores**. 

3. Selecione um ou mais utilizadores recentemente eliminados e, em seguida, restaurá-los ou eliminá-los permanentemente.

 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Novas aplicações de cliente aprovada para acesso condicional de baseado em aplicações do Azure AD

 
**Tipo:** funcionalidade foi alterado  
**Categoria de serviço:** acesso condicional  
**Capacidade de produto:** identidade de segurança e proteção


As aplicações seguintes foram adicionadas à lista de [aprovados aplicações de cliente](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement):

- Microsoft Planner
- Azure Information Protection 


Para obter mais informações, consulte:

- [Requisito da aplicação de cliente aprovada](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [Azure AD com base na aplicação acesso condicional](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-mam)


---

### <a name="use-or-between-controls-in-a-conditional-access-policy"></a>Utilize "Ou" entre controlos numa política de acesso condicional 


**Tipo:** funcionalidade foi alterado    
**Categoria de serviço:** acesso condicional  
**Capacidade de produto:** identidade de segurança e proteção

 
Agora pode utilizar "ou" (exigir um dos controlos selecionados) para controlos de acesso condicional. Pode utilizar esta funcionalidade para criar políticas com "ou" entre controlos de acesso. Por exemplo, pode utilizar esta funcionalidade para criar uma política que requer que um utilizador iniciar sessão utilizando a autenticação multifator "OR" num dispositivo com conformidade.

Para obter mais informações, consulte [controlos de acesso condicional do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-controls).

 
---

### <a name="aggregation-of-real-time-risk-events"></a>Agregação de eventos de risco em tempo real


**Tipo:** funcionalidade foi alterado    
**Categoria de serviço:** Identity protection  
**Capacidade de produto:** identidade de segurança e proteção


No Azure AD Identity Protection, todos os eventos de risco em tempo real que teve o mesmo endereço IP num determinado dia agora são agregados para cada tipo de eventos de risco. Esta alteração limita o volume de eventos de risco mostrado sem qualquer alteração na segurança do utilizador.

A deteção em tempo real subjacente funciona sempre que o utilizador inicia sessão. Se tiver uma política de segurança de início de sessão risco configurar a multi-factor authentication ou bloquear o acesso, ainda é acionado durante cada arriscado início de sessão.

 
---
 




## <a name="october-2017"></a>Outubro de 2017


### <a name="deprecate-azure-ad-reports"></a>Despromover o relatórios do Azure AD


**Tipo:** plano de alteração  
**Categoria de serviço:** relatórios  
**Capacidade de produto:** gestão de ciclo de vida de identidade  



O portal do Azure fornece-lhe:

- Uma nova consola de administração do Azure AD.
- Novas APIs para relatórios de atividade e segurança.
 
Devido a estas novas funcionalidades, o relatório APIs sob o ponto final /reports foram reformada no dia 10 de Dezembro de 2017. 

---

### <a name="automatic-sign-in-field-detection"></a>Deteção de campo de início de sessão automático


**Tipo:** fixo   
**Categoria de serviço:** aplicações My  
**Capacidade de produto:** de sessão único-  



Azure AD suporta deteção de campo de início de sessão automático para as aplicações que compor um campo de nome e palavra-passe de utilizador do HTML. Estes passos documentados em [como capturar automaticamente os campos de início de sessão para uma aplicação](https://docs.microsoft.com/azure/active-directory/application-config-sso-problem-configure-password-sso-non-gallery#how-to-manually-capture-sign-in-fields-for-an-application). Pode encontrar esta capacidade adicionando um *não galeria* aplicação no **aplicações empresariais** página no [portal do Azure](http://aad.portal.azure.com). Além disso, pode configurar o **Single Sign-on** modo nesta aplicação nova para **baseada em palavra-passe Single Sign-on**, introduza um URL de web e, em seguida, guarde a página.
 
Devido a um problema do serviço, esta funcionalidade foi temporariamente desativada. O problema foi resolvido e a deteção de campo de início de sessão automático está novamente disponível.

---

### <a name="new-multi-factor-authentication-features"></a>Novas funcionalidades de autenticação multifator


**Tipo:** nova funcionalidade  
**Categoria de serviço:** multi-factor authentication  
**Capacidade de produto:** identidade de segurança e proteção  



Autenticação multifator (MFA) é uma parte essencial dos proteger a sua organização. Para tornar as credenciais mais adaptável e a experiência mais integrada, foram adicionadas as seguintes funcionalidades: 

- Resultados de desafio multifator diretamente estão integrados com o Azure AD início de sessão no relatório, que inclui acesso programático para resultados MFA.
- A configuração do MFA mais aprofundadamente está integrada na configuração do Azure AD experiência no portal do Azure.

Com esta pré-visualização pública, gestão de MFA e relatórios são uma parte integrada da experiência de configuração do principal do Azure AD. Agora pode gerir a funcionalidade de portal de gestão MFA dentro da experiência do Azure AD.

Para obter mais informações, consulte [referência para MFA relatórios no portal do Azure](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins-mfa). 


---

### <a name="terms-of-use"></a>Termos de utilização



**Tipo:** nova funcionalidade  
**Categoria de serviço:** termos de utilização  
**Capacidade de produto:** conformidade  



Pode utilizar os termos do Azure AD de utilização para apresentar informações tais como exclusões de responsabilidade relevantes para os requisitos legais ou conformidade a utilizadores.

Pode utilizar os termos do Azure AD de utilização nos seguintes cenários:

- Termos de licenciamento gerais de utilização para todos os utilizadores na sua organização
- Termos específicos de utilização com base nos atributos de um utilizador (por exemplo, doctors vs nurses) ou locais vs empregados internacionais, efetuada pelo grupos dinâmicos
- Termos específicos de utilização para aceder a aplicações de elevado impacto comercial, como o Salesforce

Para obter mais informações, consulte [termos do Azure AD de utilização](https://docs.microsoft.com/azure/active-directory/active-directory-tou).


---

### <a name="enhancements-to-privileged-identity-management"></a>Melhoramentos no Privileged Identity Management


**Tipo:** nova funcionalidade  
**Categoria de serviço:** Privileged Identity Management  
**Capacidade de produto:** Privileged Identity Management  


Com o Azure AD Privileged Identity Management, pode gerir, controlar e monitorizar o acesso aos recursos do Azure (pré-visualização) na sua organização para:

- Subscrições
- Grupos de recursos
- Máquinas virtuais 

Todos os recursos no portal do Azure que utilizam a funcionalidade de RBAC do Azure podem tirar partido de todas as capacidades de gestão de ciclo de vida que o Azure AD Privileged Identity Management tem para oferecer e segurança.

Para obter mais informações, consulte [Privileged Identity Management para recursos do Azure](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).


---

### <a name="access-reviews"></a>Revisões de acesso


**Tipo:** nova funcionalidade  
**Categoria de serviço:** revisões de acesso  
**Capacidade de produto:** conformidade  



As organizações podem utilizar revisões de acesso (pré-visualização) para gerir de forma eficiente o acesso a aplicações empresariais e as associações de grupo: 

- Pode voltar a certificar o acesso do utilizador convidado ao utilizar as revisões do respetivo acesso às aplicações e as associações a grupos. Revisores de forma eficiente podem decidir se a permitir que os convidados continuou a acesso com base nas informações fornecidas por revisões de acesso.
- Pode voltar a certificar o acesso dos colaboradores às aplicações e as associações a grupos com revisões de acesso.

Pode recolher os controlos de revisão de acesso relativos a programas relevantes para a sua organização de modo a controlar as revisões de conformidade ou de aplicações sensíveis a riscos.

Para obter mais informações, consulte [acesso do Azure AD revê](https://docs.microsoft.com/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview).


---

### <a name="hide-third-party-applications-from-my-apps-and-the-office-365-app-launcher"></a>Ocultar aplicações de terceiros de iniciador de aplicações do Office 365 e aplicações My



**Tipo:** nova funcionalidade  
**Categoria de serviço:** aplicações My  
**Capacidade de produto:** de sessão único-  



Agora pode melhor gerir aplicações apresentados nos portais dos seus utilizadores através de um novo **ocultar aplicação** propriedade. Pode ocultar as aplicações para ajudar nos casos em que mosaicos da aplicação apareçam para serviços de back-end ou mosaicos duplicados e launchers de aplicação dos utilizadores clutter. O botão de alternar está no **propriedades** secção da aplicação de terceiros e assinalada como **Visible ao utilizador?** Também pode ocultar uma aplicação através de programação através do PowerShell. 

Para obter mais informações, consulte [ocultar uma aplicação de terceiros da experiência do utilizador no Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app). 


**O que está disponível?**

 Como parte da transição para a nova consola de administração, duas novas APIs para obter a atividade do Azure AD estão disponíveis registos. O novo conjunto de APIs fornece mais rico de filtragem e ordenação funcionalidade além de proporcionarem auditoria mais completas e as atividades de início de sessão. Os dados previamente disponíveis através de relatórios de segurança agora podem ser acedidos através da API de eventos de risco do Identity Protection no Microsoft Graph.


## <a name="september-2017"></a>Setembro de 2017

### <a name="hotfix-for-identity-manager"></a>Correção do Identity Manager


**Tipo:** funcionalidade foi alterado  
**Categoria de serviço:** Identity Manager  
**Capacidade de produto:** gestão de ciclo de vida de identidade  



Um pacote de rollup de correção (compilação 4.4.1642.0) está disponível a partir 25 de Setembro de 2017, para Identity Manager 2016 Service Pack 1. Este pacote de rollup:

- Resolve problemas e adiciona melhoramentos.
- É uma atualização cumulativa que substitui todas as atualizações do Identity Manager 2016 Service Pack 1 até compilação 4.4.1459.0 do Identity Manager 2016. 
- Requer que tenha de criar 4.4.1302.0 do Identity Manager 2016. 

Para obter mais informações, consulte [(compilação 4.4.1642.0) do pacote de rollup de correção está disponível para Identity Manager 2016 Service Pack 1](https://support.microsoft.com/help/4021562). 

---
