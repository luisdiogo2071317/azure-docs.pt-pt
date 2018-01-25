---
title: "Novidades Notas de versão para o Azure Active Directory | Microsoft Docs"
description: "Saiba o que há de novo no Azure Active Directory (Azure AD), tais como as notas de versão mais recentes, problemas conhecidos, correções de erros, funcionalidade preterida e as alterações futuras."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
featureFlags: clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: f32fc2d2df73f4a01c300d565595e2c00b2baf4b
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/25/2018
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

Para obter mais informações, consulte [Eventos de risco do Azure Active Directory](active-directory-reporting-risk-events.md).
 

---

### <a name="hide-office-365-applications-from-end-users-access-panels"></a>Ocultar a painéis de acesso do utilizador final de aplicações do Office 365

**Tipo:** nova funcionalidade  
**Categoria de serviço:** aplicações My  
**Capacidade de produto:** SSO
 

Pode agora melhor gerir como aplicações do Office 365 mostram no painel de acesso do utilizador através de uma nova definição de utilizador. Esta opção é útil para reduzir a quantidade de aplicações no painel de acesso de um utilizador, se preferir Mostrar apenas as aplicações do Office no portal do Office. A definição está localizada no **as definições de utilizador** e assinalada como **os utilizadores apenas podem ver as aplicações do Office 365 no portal do Office 365**.
 

Para obter mais informações, consulte [ocultar uma aplicação da experiência do utilizador no Azure Active Directory](active-directory-coreapps-hide-third-party-app.md).

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
 
Para obter mais informações, consulte [segura de aplicações My início de sessão extensão](active-directory-saas-access-panel-introduction.md#my-apps-secure-sign-in-extension).

---

### <a name="azure-ad-administration-experience-in-azure-classic-portal-has-been-retired"></a>A administração do Azure AD foi retirada experiência de utilização no portal clássico do Azure

**Tipo:** preterido   
**Categoria de serviço:** do Azure AD  
**Capacidade de produto:** diretório
 

A partir de 8 de Janeiro de 2018, a administração do Azure AD foi retirada experiência de utilização no portal clássico do Azure. Isto demorou local em conjunto com a extinção do portal clássico do Azure em si. Doravante, deve utilizar o [Centro de administração do Azure AD](https://aad.portal.azure.com) todos os seus administração baseada no portal do Azure AD.
 
---

### <a name="azure-ad-administration-experience-in-azure-classic-portal-has-been-retired"></a>A administração do Azure AD foi retirada experiência de utilização no portal clássico do Azure

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

- [Introdução ao Azure Active Directory API do relatório](active-directory-reporting-api-getting-started-azure-portal.md)

- [Introdução ao Azure Active Directory Identity Protection e o Microsoft Graph](active-directory-identityprotection-graph-getting-started.md)


---


## <a name="december-2017"></a>Dezembro de 2017
 

### <a name="terms-of-use-in-the-access-panel"></a>Termos de utilização no painel de acesso

**Tipo:** nova funcionalidade  
**Categoria de serviço:** termos de utilização  
**Capacidade de produto:** governação/compatibilidade
 
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
**Capacidade de produto:** governação
 
Uma opção para os administradores requer que os utilizadores expandir os termos de utilização antes de aceitar os termos.

Selecione **no** ou **desativar** pedir aos utilizadores expandir os termos de utilização. O **no** definição exige que os utilizadores ver os termos de utilização antes de a aceitá-los.

Para obter mais informações, consulte o [termos do Azure AD da funcionalidade de utilização (pré-visualização)](active-directory-tou.md).
 
---
 

### <a name="scoped-activation-for-eligible-role-assignments"></a>Ativação âmbito existência de atribuições de função elegíveis

**Tipo:** nova funcionalidade  
**Categoria de serviço:** Privileged Identity Management  
**Capacidade de produto:** Privileged Identity Management
 
Pode utilizar ativação âmbito para ativar as atribuições de função de recursos do Azure elegível com menos autonomia em que as predefinições de atribuição original. Um exemplo é se estiver atribuído como o proprietário de uma subscrição no seu inquilino. Com a ativação de âmbito, pode ativar a função de proprietário de até cinco recursos contidos dentro da subscrição (tais como grupos de recursos e máquinas virtuais). A ativação de controlo de âmbito, poderá reduzir a possibilidade de executar indesejáveis alterações a recursos do Azure críticos.

Para obter mais informações, consulte [que é o Azure AD Privileged Identity Management?](active-directory-privileged-identity-management-configure.md).
 
---
 

### <a name="new-federated-apps-in-the-azure-ad-app-gallery"></a>Novas aplicações federadas na Galeria de aplicações do Azure AD

**Tipo:** nova funcionalidade  
**Categoria de serviço:** aplicações da empresa  
**Capacidade de produto:** integração de terceiros
 
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

Para obter mais informações, consulte [de acesso condicional no Azure AD](active-directory-conditional-access-azure-portal.md).

 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Novas aplicações de cliente aprovada para acesso condicional de baseado em aplicações do Azure AD

 
**Tipo:** plano de alteração  
**Categoria de serviço:** acesso condicional  
**Capacidade de produto:** identidade de segurança e proteção




As seguintes aplicações estão a ser planeadas a adicionar à lista de [aprovados aplicações de cliente](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement):

- [Microsoft Kaizala](https://www.microsoft.com/garage/profiles/kaizala/)
- [Microsoft StaffHub](https://staffhub.office.com/what-it-is)


Para obter mais informações, consulte:

- [Requisito da aplicação de cliente aprovada](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement)
- [Azure AD com base na aplicação acesso condicional](active-directory-conditional-access-mam.md)


---

### <a name="terms-of-use-support-for-multiple-languages"></a>Suporte de termos de utilização de vários idiomas



**Tipo:** nova funcionalidade    
**Categoria de serviço:** termos de utilização  
**Capacidade de produto:** governação/compatibilidade





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


Para obter mais informações, consulte [integração no local](active-directory-passwords-how-it-works.md#on-premises-integration).

 
---


### <a name="azure-ad-app-based-conditional-access"></a>Azure AD com base na aplicação acesso condicional 



 
**Tipo:** nova funcionalidade  
**Categoria de serviço:** do Azure AD  
**Capacidade de produto:** identidade de segurança e proteção





Agora pode restringir o acesso ao Office 365 e outras aplicações de nuvem do Azure AD ligados para [aprovados aplicações de cliente](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement) que suportam as políticas de proteção de aplicação do Intune através da utilização de [acesso condicional de baseado em aplicações do Azure AD](active-directory-conditional-access-mam.md). Políticas de proteção de aplicações do Intune são utilizadas para configurar e proteger dados da empresa nestas aplicações de cliente.

Ao combinar [baseado na aplicação](active-directory-conditional-access-mam.md) com [baseado nos dispositivos](active-directory-conditional-access-policy-connected-applications.md) políticas de acesso condicional, tem a flexibilidade para proteger os dados pessoais e de dispositivos da empresa.

As seguintes condições e os controlos estão agora disponíveis para utilização com acesso condicional baseado na aplicação:

**Condição de plataforma suportada**

- iOS
- Android

**Condição de aplicações de cliente**

- Aplicações móveis e clientes de ambiente de trabalho

**Controlo de acesso**

- Requer aplicação aprovada do cliente


Para obter mais informações, consulte [acesso condicional de baseado em aplicações do Azure AD](active-directory-conditional-access-mam.md).

 
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

Para obter mais informações, consulte [gerir dispositivos através do portal do Azure](device-management-azure-portal.md).



 
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

- [Criar uma política de conformidade de dispositivos para dispositivos macOS com o Intune](https://aka.ms/macoscompliancepolicy)
- [Acesso condicional no Azure AD](active-directory-conditional-access-azure-portal.md)


 
---

### <a name="network-policy-server-extension-for-azure-multi-factor-authentication"></a>Extensão de servidor de políticas de rede para o multi-factor Authentication do Azure 


**Tipo:** nova funcionalidade    
**Categoria de serviço:** multi-factor authentication  
**Capacidade de produto:** autenticação de utilizador




A extensão do servidor de políticas de rede de Azure multi-factor Authentication adiciona funcionalidades de baseado na nuvem a autenticação multifator para a sua infraestrutura de autenticação utilizando os servidores existentes. Com a extensão de servidor de políticas de rede, pode adicionar a chamada telefónica, mensagem de texto ou verificação de aplicação de telefone para o fluxo de autenticação existente. Não tem de instalar, configurar e manter novos servidores. 

Esta extensão foi criada para as organizações que pretendem proteger as ligações de rede privada virtual sem a implementar o servidor do Azure multi-factor Authentication. O servidor de políticas de rede extensão age como um adaptador entre o RADIUS e baseado na nuvem do Azure multi-factor Authentication para fornecer um segundo fator de autenticação para federada ou utilizadores sincronizados.


Para obter mais informações, consulte [integrar a sua infraestrutura de servidor de políticas de rede existente com o Azure multi-factor Authentication](../multi-factor-authentication/multi-factor-authentication-nps-extension.md).

 
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


As aplicações seguintes foram adicionadas à lista de [aprovados aplicações de cliente](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement):

- Microsoft Planner
- Azure Information Protection 


Para obter mais informações, consulte:

- [Requisito da aplicação de cliente aprovada](active-directory-conditional-access-technical-reference.md#approved-client-app-requirement)
- [Azure AD com base na aplicação acesso condicional](active-directory-conditional-access-mam.md)


---

### <a name="use-or-between-controls-in-a-conditional-access-policy"></a>Utilize "Ou" entre controlos numa política de acesso condicional 


**Tipo:** funcionalidade foi alterado    
**Categoria de serviço:** acesso condicional  
**Capacidade de produto:** identidade de segurança e proteção

 
Agora pode utilizar "ou" (exigir um dos controlos selecionados) para controlos de acesso condicional. Pode utilizar esta funcionalidade para criar políticas com "ou" entre controlos de acesso. Por exemplo, pode utilizar esta funcionalidade para criar uma política que requer que um utilizador iniciar sessão utilizando a autenticação multifator "OR" num dispositivo com conformidade.

Para obter mais informações, consulte [controlos de acesso condicional do Azure AD](active-directory-conditional-access-controls.md).

 
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



Azure AD suporta deteção de campo de início de sessão automático para as aplicações que compor um campo de nome e palavra-passe de utilizador do HTML. Estes passos documentados em [como capturar automaticamente os campos de início de sessão para uma aplicação](application-config-sso-problem-configure-password-sso-non-gallery.md#how-to-manually-capture-sign-in-fields-for-an-application). Pode encontrar esta capacidade adicionando um *não galeria* aplicação no **aplicações empresariais** página no [portal do Azure](http://aad.portal.azure.com). Além disso, pode configurar o **Single Sign-on** modo nesta aplicação nova para **baseada em palavra-passe Single Sign-on**, introduza um URL de web e, em seguida, guarde a página.
 
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

Para obter mais informações, consulte [referência para MFA relatórios no portal do Azure](active-directory-reporting-activity-sign-ins-mfa.md). 


---

### <a name="terms-of-use"></a>Termos de utilização



**Tipo:** nova funcionalidade  
**Categoria de serviço:** termos de utilização  
**Capacidade de produto:** governação  



Pode utilizar os termos do Azure AD de utilização para apresentar informações tais como exclusões de responsabilidade relevantes para os requisitos legais ou conformidade a utilizadores.

Pode utilizar os termos do Azure AD de utilização nos seguintes cenários:

- Termos de licenciamento gerais de utilização para todos os utilizadores na sua organização
- Termos específicos de utilização com base nos atributos de um utilizador (por exemplo, doctors vs nurses) ou locais vs empregados internacionais, efetuada pelo grupos dinâmicos
- Termos específicos de utilização para aceder a aplicações de elevado impacto comercial, como o Salesforce

Para obter mais informações, consulte [termos do Azure AD de utilização](active-directory-tou.md).


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

Para obter mais informações, consulte [Privileged Identity Management para recursos do Azure](privileged-identity-management/azure-pim-resource-rbac.md).


---

### <a name="access-reviews"></a>Revisões de acesso


**Tipo:** nova funcionalidade  
**Categoria de serviço:** revisões de acesso  
**Capacidade de produto:** governação  



As organizações podem utilizar revisões de acesso (pré-visualização) para gerir de forma eficiente o acesso a aplicações empresariais e as associações de grupo: 

- Pode voltar a certificar o acesso do utilizador convidado ao utilizar as revisões do respetivo acesso às aplicações e as associações a grupos. Revisores de forma eficiente podem decidir se a permitir que os convidados continuou a acesso com base nas informações fornecidas por revisões de acesso.
- Pode voltar a certificar o acesso dos colaboradores às aplicações e as associações a grupos com revisões de acesso.

Pode recolher os controlos de revisão de acesso relativos a programas relevantes para a sua organização de modo a controlar as revisões de conformidade ou de aplicações sensíveis a riscos.

Para obter mais informações, consulte [acesso do Azure AD revê](active-directory-azure-ad-controls-access-reviews-overview.md).


---

### <a name="hide-third-party-applications-from-my-apps-and-the-office-365-app-launcher"></a>Ocultar aplicações de terceiros de iniciador de aplicações do Office 365 e aplicações My



**Tipo:** nova funcionalidade  
**Categoria de serviço:** aplicações My  
**Capacidade de produto:** de sessão único-  



Agora pode melhor gerir aplicações apresentados nos portais dos seus utilizadores através de um novo **ocultar aplicação** propriedade. Pode ocultar as aplicações para ajudar nos casos em que mosaicos da aplicação apareçam para serviços de back-end ou mosaicos duplicados e launchers de aplicação dos utilizadores clutter. O botão de alternar está no **propriedades** secção da aplicação de terceiros e assinalada como **Visible ao utilizador?** Também pode ocultar uma aplicação através de programação através do PowerShell. 

Para obter mais informações, consulte [ocultar uma aplicação de terceiros da experiência do utilizador no Azure AD](active-directory-coreapps-hide-third-party-app.md). 


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
