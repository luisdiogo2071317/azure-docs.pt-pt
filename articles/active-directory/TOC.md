# [Documentação do Azure Active Directory](index.md)

# Descrição geral
## [O que é o Azure Active Directory?](fundamentals/active-directory-whatis.md)
## [Sobre a gestão de identidade do Azure](fundamentals/identity-fundamentals.md)
## [Compreender as soluções de identidade do Azure](fundamentals/understand-azure-identity-solutions.md)
## [Escolher uma solução de identidade híbrida](choose-hybrid-identity-solution.md)
## [Associar as subscrições Azure](fundamentals/active-directory-how-subscriptions-associated-directory.md)
## [Residência e considerações de dados](fundamentals/active-directory-data-storage-eu.md)
## [FAQs](fundamentals/active-directory-faq.md)
## [Novidades](fundamentals/whats-new.md)


# Introdução
## [Introdução ao Azure AD](fundamentals/get-started-azure-ad.md)
## [Inscreva-se no Azure AD Premium](fundamentals/active-directory-get-started-premium.md)
## [Adicionar um nome de domínio personalizado](fundamentals/add-custom-domain.md)
## [Configurar o branding da empresa](fundamentals/customize-branding.md)
## [Adicionar utilizadores ao Azure AD](fundamentals/add-users-azure-active-directory.md)
## [Atribuir licenças a utilizadores](fundamentals/license-users-groups.md)
## [Configurar a reposição personalizada de palavra-passe](authentication/quickstart-sspr.md)
## [Adicionar informações de privacidade da sua organização no Azure AD](active-directory-properties-area.md)


# Procedimento
## Planear e conceber
### [Compreender a arquitetura do Azure AD](fundamentals/active-directory-architecture.md)
### [Mapeamento de afirmações no Azure Active Directory](active-directory-claims-mapping.md)
### [Implementar uma solução de identidade híbrida](active-directory-hybrid-identity-design-considerations-overview.md)
#### Determinar os requisitos
##### [Identidade](active-directory-hybrid-identity-design-considerations-business-needs.md)
##### [Sincronização de diretórios](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)
##### [Multi-factor auth](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)
##### [Estratégia de ciclo de vida de identidade](active-directory-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md)
#### [Planear a segurança de dados](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md)
##### [Proteção de dados](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)
##### [Gestão de conteúdos](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)
##### [Controlo de acesso](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)
##### [Resposta a incidentes](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)
#### Planear o ciclo de vida de identidade
##### [Tarefas](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)
##### [Estratégia de adoção](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)
#### [Passos seguintes?](active-directory-hybrid-identity-design-considerations-nextsteps.md)
#### [Comparação de ferramentas](active-directory-hybrid-identity-design-considerations-tools-comparison.md)

## Gerir utilizadores
### [Adicionar novos utilizadores ao Azure AD](fundamentals/add-users-azure-active-directory.md)
### [Gerir perfis de utilizador](fundamentals/active-directory-users-profile-azure-portal.md)
### [Repor palavras-passe do utilizador](fundamentals/active-directory-users-reset-password-azure-portal.md)
### [Contas de partilha](active-directory-sharing-accounts.md)
### [Atribuir utilizadores a funções de administrador](fundamentals/active-directory-users-assign-role-azure-portal.md)
### [Restaurar um utilizador eliminado](fundamentals/active-directory-users-restore.md)
### [Adicionar utilizadores convidados a partir de outro diretório (B2B)](b2b/what-is-b2b.md)
#### [Administradores que adicionam utilizadores B2B](b2b/add-users-administrator.md)
#### [Técnicos de informação que adicionam utilizadores B2B](b2b/add-users-information-worker.md)
#### [API e personalização](b2b/customize-invitation-api.md)
#### [Exemplos de códigos e do Azure PowerShell](b2b/code-samples.md)
#### [Exemplo de portal de inscrição self-service](b2b/self-service-portal.md)
#### [E-mail de convite](b2b/invitation-email-elements.md)
#### [Resgate de convite](b2b/redemption-experience.md)
#### [Adicionar utilizadores B2B sem convite](b2b/add-user-without-invite.md)
#### [Permitir ou bloquear convites](b2b/allow-deny-list.md)
#### [Acesso condicional para B2B](b2b/conditional-access.md)
#### [Políticas de partilha B2B](b2b/delegate-invitations.md)
#### [Adicionar um utilizador B2B a uma função](b2b/add-guest-to-role.md)
#### [Grupos dinâmicos e utilizadores B2B](b2b/use-dynamic-groups.md)
#### [Sair de uma organização](b2b/leave-the-organization.md)
#### [Auditoria e relatórios](b2b/auditing-and-reporting.md)
#### [B2B para organizações híbridas](b2b/hybrid-organizations.md)
##### [Conceder aos utilizadores B2B acesso a aplicações locais](b2b/hybrid-cloud-to-on-premises.md)
##### [Conceder aos utilizadores locais acesso a aplicações na cloud](b2b/hybrid-on-premises-to-cloud.md)
#### [Partilha externa do Office 365 e B2B](b2b/o365-external-user.md)
#### [Licenciamento B2B](b2b/licensing-guidance.md)
#### [Limitações atuais](b2b/current-limitations.md)
#### [FAQ](b2b/faq.md)
#### [Resolução de problemas B2B](b2b/troubleshoot.md)
#### [Compreender o utilizador B2B](b2b/user-properties.md)
#### [Token de utilizador B2B](b2b/user-token.md)
#### [B2B para aplicações integradas do Azure AD](b2b/configure-saas-apps.md)
#### [Mapeamento das afirmações de utilizador B2B](b2b/claims-mapping.md)
#### [Comparar colaboração B2B com B2C](b2b/compare-with-b2c.md)
#### [Obter suporte para B2B](b2b/get-support.md)

## [Gerir grupos e membros](fundamentals/active-directory-manage-groups.md)
### [Gerir grupos](fundamentals/active-directory-groups-create-azure-portal.md)
### [Restaurar um grupo do Office 365 eliminado](fundamentals/active-directory-groups-restore-azure-portal.md)
### [Gerir definições do grupo](fundamentals/active-directory-groups-settings-azure-portal.md)
## [Gerir relatórios](reports-monitoring/overview-reports.md)
### [Atividade de inícios de sessão](reports-monitoring/concept-sign-ins.md)
### [Atividade de auditoria](reports-monitoring/concept-audit-logs.md)
### [Utilizadores em risco](reports-monitoring/concept-user-at-risk.md)
### [Inícios de sessão de risco](reports-monitoring/concept-risky-sign-ins.md)
### [Eventos de risco](reports-monitoring/concept-risk-events.md)
### [A monitorizar os registos com o Azure Monitor](reports-monitoring/overview-activity-logs-in-azure-monitor.md)
### [FAQ](reports-monitoring/reports-faq.md)

### Tarefas
#### [Configurar localizações com nome](active-directory-named-locations.md)
#### [Encontrar relatórios de atividades](reports-monitoring/howto-find-activity-reports.md)
#### [Utilizar o Pacote de Conteúdos do Power BI para o Azure AD](reports-monitoring/howto-power-bi-content-pack.md)
#### [Remediar utilizadores sinalizados como em risco](reports-monitoring/howto-remediate-users-flagged-for-risk.md)
#### [Encaminhar os registos de atividade para um hub de eventos do Azure](reports-monitoring/quickstart-azure-monitor-stream-logs-to-event-hub.md)
#### [Arquivar os registos de atividade para uma conta de armazenamento do Azure](reports-monitoring/quickstart-azure-monitor-route-logs-to-storage-account.md)
#### [Integrar os registos de atividade com o Splunk através do Azure Monitor](reports-monitoring/tutorial-integrate-activity-logs-with-splunk.md)

### Referência
#### [Retenção](reports-monitoring/reference-reports-data-retention.md)
#### [Latências](reports-monitoring/reference-reports-latencies.md)
#### [Referência da atividade de auditoria](reports-monitoring/reference-audit-activities.md)
#### [Códigos de erro de atividade de início de sessão](reports-monitoring/reference-sign-ins-error-codes.md)
#### [Interpretar o esquema de registo de auditoria no Azure Monitor](reports-monitoring/reference-azure-monitor-audit-log-schema.md)
#### [Interpretar o esquema de registo de início de sessão no Azure Monitor](reports-monitoring/reference-azure-monitor-sign-ins-log-schema.md)

### Resolução de problemas
#### [Dados em falta nos registos de atividades do Azure AD](reports-monitoring/troubleshoot-missing-audit-data.md)
#### [Dados em falta em transferências](reports-monitoring/troubleshoot-missing-data-download.md)
#### [Erros do pacote de conteúdos dos registos de atividades do Azure AD](reports-monitoring/troubleshoot-content-pack.md)
#### [Erros na API de Relatórios do Azure AD](reports-monitoring/troubleshoot-graph-api.md)

### [Acesso Programático](reports-monitoring/concept-reporting-api.md)
#### [Pré-requisitos](reports-monitoring/howto-configure-prerequisites-for-reporting-api.md)
#### [Com certificados](reports-monitoring/tutorial-access-api-with-certificates.md)

## [Gerir palavras-passe](authentication/concept-sspr-howitworks.md)
### Documentos do utilizador
#### [Repor ou alterar a palavra-passe](user-help/active-directory-passwords-update-your-own-password.md)
#### [Registar-se na reposição personalizada de palavra-passe](user-help/active-directory-passwords-reset-register.md)


## Gerir dispositivos
### [Descrição geral](devices/overview.md)

### Inícios rápidos
#### [Configurar dispositivos Windows 10 registados no Azure AD](user-help/device-management-azuread-registered-devices-windows10-setup.md)
#### [Configurar dispositivos associados ao Azure AD](user-help/device-management-azuread-joined-devices-setup.md)

### Tutoriais
#### [Configurar a associação híbrida do Azure AD para os domínios geridos](devices/hybrid-azuread-join-managed-domains.md)
#### [Configurar a associação híbrida do Azure AD para os domínios federados](devices/hybrid-azuread-join-federated-domains.md)
#### [Configurar manualmente a associação ao Azure AD híbrido](devices/hybrid-azuread-join-manual-steps.md)
#### [Configurar a associação ao Azure AD durante a experiência de primeira execução do Windows 10](devices/azuread-joined-devices-frx.md)

### Guias
#### [Planear a Associação do Azure AD](devices/azureadjoin-plan.md)
#### [Planear a sua implementação de associação ao Azure AD híbrido](devices/hybrid-azuread-join-plan.md)
#### [Controlar a associação híbrida do Azure AD dos seus dispositivos](devices/hybrid-azuread-join-control.md)
#### [Atribuir administradores locais a dispositivos associados do Azure AD](devices/assign-local-admin.md)
#### [Resolver problemas de dispositivos atuais do Windows associados ao Azure AD híbrido](devices/troubleshoot-hybrid-join-windows-current.md)
#### [Resolver problemas de dispositivos Windows legados associados ao Azure AD híbrido](devices/troubleshoot-hybrid-join-windows-legacy.md)

### Conceitos
#### [Manage devices using the Azure portal](devices/device-management-azure-portal.md) (Gerir dispositivos com o portal do Azure)
#### [FAQs](devices/faq.md)

## Gerir aplicações
### [Descrição geral](manage-apps/what-is-application-management.md)
### [Introdução](manage-apps/plan-an-application-integration.md)
### [Tutoriais de integração de aplicação SaaS](saas-apps/tutorial-list.md)

### [Aprovisionamento e desaprovisionamento de utilizadores para aplicações SaaS](active-directory-saas-app-provisioning.md) 
#### [Tutoriais de integração em aplicação](saas-apps/tutorial-list.md) 
#### [Automatizar o aprovisionamento de aplicações com SCIM ativado](manage-apps/use-scim-to-provision-users-and-groups.md) 
#### [Personalizar os mapeamentos de atributos](active-directory-saas-customizing-attribute-mappings.md) 
#### [Escrever expressões para mapeamentos de atributos](active-directory-saas-writing-expressions-for-attribute-mappings.md) 
#### [Utilizar os filtros de âmbito](active-directory-saas-scoping-filters.md) 
#### [Comunicar sobre o aprovisionamento automático de utilizadores](active-directory-saas-provisioning-reporting.md) 
#### [Resolver problemas de aprovisionamento de utilizadores](active-directory-application-provisioning-content-map.md) 

### [Aceder a aplicações remotamente com o Proxy de Aplicações](manage-apps/application-proxy.md)
#### Introdução
##### [Ativar Proxy de aplicações](manage-apps/application-proxy-enable.md)
##### [Publicar aplicações](manage-apps/application-proxy-publish-azure-portal.md)
##### [Domínios personalizados](manage-apps/application-proxy-configure-custom-domain.md)
#### [Início de sessão único](manage-apps/application-proxy-single-sign-on.md)
##### [SSO com KCD](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md)
##### [SSO com cabeçalhos](manage-apps/application-proxy-configure-single-sign-on-with-ping-access.md)
##### [SSO com colocação de palavras-passe em cofres](manage-apps/application-proxy-configure-single-sign-on-password-vaulting.md)
#### Conceitos
##### [Conectores](manage-apps/application-proxy-connectors.md)
##### [Segurança](manage-apps/application-proxy-security.md)
##### [Redes](manage-apps/application-proxy-network-topology.md)


##### [Atualizar a partir de TMG ou UAG](manage-apps/application-proxy-migration.md)

#### Configurações avançadas
##### [Publicar em redes separadas](manage-apps/application-proxy-connector-groups.md)
##### [Servidores proxy](manage-apps/application-proxy-configure-connectors-with-proxy-servers.md)
##### [Aplicações com suporte para afirmações](manage-apps/application-proxy-configure-for-claims-aware-applications.md)
##### [Aplicações clientes nativas](manage-apps/application-proxy-configure-native-client-application.md)
##### [Instalação silenciosa](manage-apps/application-proxy-register-connector-powershell.md)
##### [Página inicial personalizada](manage-apps/application-proxy-configure-custom-home-page.md)
##### [Converter ligações inline](manage-apps/application-proxy-configure-hard-coded-link-translation.md)
##### [Carateres universais](active-directory-application-proxy-wildcard.md)
##### [Remover dados pessoais](manage-apps/application-proxy-remove-personal-data.md)


#### Instruções de publicação
##### [Ambiente de Trabalho Remoto](manage-apps/application-proxy-integrate-with-remote-desktop-services.md)
##### [SharePoint](manage-apps/application-proxy-integrate-with-sharepoint-server.md)
##### [Equipas da Microsoft](manage-apps/application-proxy-integrate-with-teams.md)
##### [Tableau](manage-apps/application-proxy-integrate-with-tableau.md)
##### [Qlik](active-directory-application-proxy-qlik.md)
#### [PowerShell](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#application_proxy_application_management) 

#### [Resolução de problemas](manage-apps/application-proxy-troubleshoot.md)

### Gerir aplicações empresariais
#### [Adicionar uma aplicação](manage-apps/add-application-portal.md)
#### [Ver aplicações de inquilino](manage-apps/view-applications-portal.md)
#### [Configure single sign-on](manage-apps/configure-single-sign-on-portal.md) (Configurar o início de sessão único)
#### [Atribuir utilizadores](manage-apps/assign-user-or-group-access-portal.md)
#### [Personalizar a imagem corporativa](manage-apps/change-name-or-logo-portal.md)
#### [Desativar inícios de sessão do utilizador](manage-apps/disable-user-sign-in-portal.md)
#### [Remover utilizadores](manage-apps/remove-user-or-group-access-portal.md)

#### [Gerir o aprovisionamento de contas de utilizador](manage-apps/configure-automatic-user-provisioning-portal.md)

#### [Assinatura de certificado avançada para aplicações SAML](manage-apps/certificate-signing-options.md)
#### [Ocultar uma aplicação da experiência de um utilizador](manage-apps/hide-application-from-user-portal.md)
### [Configurar o Início de Sessão Automático-Acelerado através da Política de HRD](manage-apps/configure-authentication-for-federated-users-portal.md)
### [Migrar aplicações do AD FS para o Azure AD](manage-apps/migrate-adfs-apps-to-azure.md) 
### [Gerir o acesso a aplicações](manage-apps/what-is-access-management.md)
#### [Acesso SSO](manage-apps/what-is-single-sign-on.md)
#### [Certificados para SSO](manage-apps/manage-certificates-for-federated-single-sign-on.md)
#### [Restrições de inquilino](manage-apps/tenant-restrictions.md)
#### [Utilizar utilizadores de aprovisionamento SCIM](manage-apps/use-scim-to-provision-users-and-groups.md)


### Resolução de problemas



#### Painel de Acesso
##### [A aplicação não aparece](application-access-panel-unexpected-application-not-appearing.md)
##### [Aparece uma aplicação inesperada](application-access-panel-unexpected-application-appears.md)
##### [Não é possível iniciar sessão](application-access-panel-web-sign-in-problem.md)
##### [Erro ao instalar a extensão de browser](application-access-panel-extension-problem-installing.md)
##### [Como utilizar o acesso personalizado à aplicação](application-access-panel-self-service-applications-how-to.md)
##### [Erro ao utilizar o acesso personalizado à aplicação](application-access-panel-self-service-applications-problem.md)

#### Adicionar uma aplicação
##### [Escolher o tipo de aplicação](application-config-add-app-problem-how-to-choose-application-type.md)
##### [Problemas comuns - aplicações da galeria](application-config-add-app-problem-problem-adding-gallery-app.md)
##### [Problemas comuns - aplicações que não estão na galeria](application-config-add-app-problem-problem-adding-non-gallery-app.md)

#### Proxy da aplicação
##### [Problema ao apresentar a página da aplicação](application-proxy-page-appearance-broken-problem.md)
##### [O carregamento da aplicação é demasiado demorado](application-proxy-page-load-speed-problem.md)
##### [As ligações na página da aplicação não funcionam](application-proxy-page-links-broken-problem.md)
##### [Que portas devo abrir para a minha aplicação](application-proxy-connectivity-ports-how-to.md)
##### [Não existe nenhum conector que funcione num grupo de conectores da minha aplicação](application-proxy-connectivity-no-working-connector.md)
##### [Configurar no portal de administração](application-proxy-config-how-to.md)
##### [Configurar o início de sessão único na minha aplicação](application-proxy-config-sso-how-to.md)
##### [Problema ao criar uma aplicação no portal de administração](application-proxy-config-problem.md)
##### [Configurar Delegação Restrita de Kerberos](application-proxy-back-end-kerberos-constrained-delegation-how-to.md)
##### [Configurar com o PingAccess](application-proxy-back-end-ping-access-how-to.md)
##### [Erro "Não é Possível Aceder a esta Aplicação Empresarial"](application-proxy-sign-in-bad-gateway-timeout-error.md)
##### [Problema ao instalar o Conector do Agente do Proxy de Aplicações](application-proxy-connector-installation-problem.md)


#### Registo da aplicação
##### [Introduzir os campos para o objeto de aplicação](application-dev-registration-config-specific-application-property-how-to.md)
##### [Alterar as predefinições de duração do token](application-dev-registration-config-change-token-lifetime-how-to.md)

#### Autenticação
##### [Configurar pontos finais](application-dev-registration-config-how-to.md)

#### Acesso Condicional
##### [O cliente não cumpriu os pré-requisitos do Registo de Dispositivos](active-directory-conditional-access.md)
##### [O inquilino está bloqueados devido à definição incorreta das políticas de Acesso Condicional](active-directory-conditional-access-device-remediation.md)
##### [Como e quando as regras da rede empresarial entram em vigor?](https://aka.ms/calocation)
##### [Como aumentar o número de dispositivos que esse utilizador tem permissão para registar no Azure AD?](active-directory-azureadjoin-setup.md)
##### [Como configurar o Acesso Condicional para o Exchange Online?](https://aka.ms/csforexchange)
##### [Como configurar o Acesso Condicional para dispositivos Windows 7?](active-directory-conditional-access.md#device-based-conditional-access)
##### [Que aplicações são suportadas com acesso condicional?](active-directory-conditional-access-supported-apps.md)

#### Encontrar uma API
##### [Encontrar uma API](application-dev-api-find-an-api-how-to.md)

#### Gerir o acesso
##### [Atribuir utilizadores e grupos a uma aplicação](application-access-assignment-how-to-add-assignment.md)
##### [Remover o acesso de um utilizador a uma aplicação](application-access-assignment-how-to-remove-assignment.md)
##### [Configurar a atribuição de aplicações personalizada](application-access-self-service-how-to.md)
##### [Atribuição a um utilizador inesperado](application-access-unexpected-user-assignment.md)
##### [Aplicação inesperada na lista de aplicações](application-access-unexpected-application.md)

#### Aplicações multi-inquilino
##### [Configurar uma nova aplicação](application-dev-setup-multi-tenant-app.md)
##### [Adicionar à galeria de aplicações](application-dev-registration-config-multi-tenant-application-add-to-gallery-how-to.md)

#### Permissões
##### [Escolher as permissões de uma API](application-dev-perms-for-given-api.md)
##### [Conceder permissões à minha aplicação](application-dev-registration-config-grant-permissions-how-to.md)
##### [Permissões delegadas vs Permissões de aplicação](application-dev-delegated-and-app-perms.md)
##### [Consentimento da aplicação](application-dev-consent-framework.md)

#### Aprovisionamento
##### [Quanto tempo demora](application-provisioning-when-will-provisioning-finish-specific-user.md)
##### [Tempo - aplicação da galeria](application-provisioning-when-will-provisioning-finish.md)
##### [Configurar o aprovisionamento de utilizadores - aplicação da galeria](application-provisioning-config-how-to.md)
##### [Problema ao configurar o aprovisionamento de utilizadores - aplicação da galeria](application-provisioning-config-problem.md)
##### [Problema ao guardar as credenciais de administrador ao configurar o aprovisionamento de utilizadores - aplicação da galeria](application-provisioning-config-problem-storage-limit.md)
##### [Os utilizadores não estão aprovisionados - aplicação da galeria](application-provisioning-config-problem-no-users-provisioned.md)
##### [Foram aprovisionados os utilizadores errados - aplicação da galeria](application-provisioning-config-problem-wrong-users-provisioned.md)

#### Início de sessão único
##### [Escolher um método](application-config-sso-how-to-choose-sign-on-method.md)
##### [Configurar](application-dev-registration-config-sso-how-to.md)
##### [Configurar federado - aplicações da galeria](application-config-sso-how-to-configure-federated-sso-gallery.md)
##### [Problemas comuns da configuração do federado - aplicações da galeria](application-config-sso-problem-configure-federated-sso-gallery.md)
##### [Configurar federado - aplicações que não estão na galeria](application-config-sso-how-to-configure-federated-sso-non-gallery.md)
##### [Problemas comuns da configuração do federado - aplicações que não estão na galeria](application-config-sso-problem-configure-federated-sso-non-gallery.md)
##### [Configurar palavra-passe - aplicações da galeria](application-config-sso-how-to-configure-password-sso-gallery.md)
##### [Problemas comuns da configuração da palavra-passe - aplicações da galeria](application-config-sso-problem-configure-password-sso-gallery.md)
##### [Configurar palavra-passe - aplicações que não estão na galeria](application-config-sso-how-to-configure-password-sso-non-gallery.md)
##### [Problemas comuns da configuração da palavra-passe - aplicações que não estão na galeria](application-config-sso-problem-configure-password-sso-non-gallery.md)

#### Problemas de início de sessão do utilizador
##### [Pedido de consentimento inesperado](application-sign-in-unexpected-user-consent-prompt.md)
##### [Erro de consentimento do utilizador](application-sign-in-unexpected-user-consent-error.md)
##### [Problemas ao iniciar sessão no portal personalizado](application-sign-in-other-problem-deeplink.md)
##### [Problemas ao iniciar sessão no painel de acesso](application-sign-in-other-problem-access-panel.md)
##### [Erro na página de início de sessão da aplicação](application-sign-in-problem-application-error.md)
##### [Problema de início de sessão único com palavra-passe - aplicação que não está na galeria](application-sign-in-problem-password-sso-non-gallery.md)
##### [Problema de início de sessão único com palavra-passe - aplicação da galeria](application-sign-in-problem-password-sso-gallery.md)
##### [Problema ao iniciar sessão numa aplicação da Microsoft](application-sign-in-problem-first-party-microsoft.md)
##### [Problema de início de sessão único federado - aplicação que não está na galeria](application-sign-in-problem-federated-sso-non-gallery.md)
##### [Problema de início de sessão único federado - aplicação da galeria](application-sign-in-problem-federated-sso-gallery.md)
##### [Problema com aplicações com programação personalizada](application-sign-in-problem-custom-dev.md)
##### [Problema com aplicações no local - Proxy de Aplicações](application-sign-in-problem-on-premises-application-proxy.md)

### [Programar aplicações](active-directory-applications-guiding-developers-for-lob-applications.md)
### [Biblioteca de documentos](active-directory-apps-index.md)

## Gerir o seu diretório
### [Azure AD Connect](./connect/active-directory-aadconnect.md)
### Nomes de domínio personalizados
#### [Início rápido](fundamentals/add-custom-domain.md)
### [Administrar o seu diretório](fundamentals/active-directory-administer.md)
### [Roaming de Estado Empresarial](active-directory-windows-enterprise-state-roaming-overview.md)
#### [Ativar](active-directory-windows-enterprise-state-roaming-enable.md)
#### [Definições da política de grupo](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
#### [Definições do Windows 10](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
#### [FAQs](active-directory-windows-enterprise-state-roaming-faqs.md)
#### [Resolução de problemas](active-directory-windows-enterprise-state-roaming-troubleshooting.md)


### [Integrar identidades no local com o Azure AD Connect](./connect/active-directory-aadconnect.md)

### [Configurar durações de token](active-directory-configurable-token-lifetimes.md)

## Revisões de acesso
### [Descrição geral da revisão de acesso](active-directory-azure-ad-controls-access-reviews-overview.md)
### [Concluir uma revisão de acesso](active-directory-azure-ad-controls-complete-access-review.md)
### [Criar uma revisão de acesso](active-directory-azure-ad-controls-create-access-review.md)
### [Como realizar uma revisão de acesso](active-directory-azure-ad-controls-perform-access-review.md)
### [Como rever o seu acesso](active-directory-azure-ad-controls-how-to-review-your-access.md)
### [Acesso de convidado com revisões de acesso](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md)
### [Gerir o acesso dos utilizadores com revisões](active-directory-azure-ad-controls-manage-user-access-with-access-reviews.md)
### [Gerir programas e controlos](active-directory-azure-ad-controls-manage-programs-controls.md)
### [Obter resultados da revisão do acesso](active-directory-azure-ad-controls-retrieve-access-review.md)

## Proteger as suas identidades
### Autenticação baseada em certificados
#### [Android](active-directory-certificate-based-authentication-android.md)
#### [iOS](active-directory-certificate-based-authentication-ios.md)
#### [Introdução](active-directory-certificate-based-authentication-get-started.md)

### Azure AD Identity Protection
#### [Descrição geral](identity-protection/overview.md)
#### [Ativar](identity-protection/enable.md)
#### [Detetar vulnerabilidades](identity-protection/vulnerabilities.md)
#### [Eventos de risco](active-directory-identity-protection-risk-events.md)
#### [Notificações](identity-protection/notifications.md)
#### [Experiência de início de sessão](identity-protection/flows.md)
#### [Simular eventos de risco](identity-protection/playbook.md)
#### [Desbloquear utilizadores](identity-protection/howto-unblock-user.md)
#### [FAQs](identity-protection/faqs.md)
#### [Glossário](identity-protection/glossary.md)
#### [Microsoft Graph](identity-protection/graph-get-started.md)
### [Privileged Identity Management](privileged-identity-management/pim-configure.md?toc=%2fazure%2factive-directory%2ftoc.json)

## [Implementar os AD FS no Azure](active-directory-aadconnect-azure-adfs.md)
### [Elevada disponibilidade](active-directory-adfs-in-azure-with-azure-traffic-manager.md)
### [Algoritmo de alteração de hash de assinatura](active-directory-federation-sha256-guidance.md)

## [Resolução de problemas](fundamentals/active-directory-troubleshooting-support-howto.md)

## Implementar a Prova de Conceito (PoC) do Azure AD
### [Manual PoC: Introdução](active-directory-playbook-intro.md)
### [Manual PoC: Ingredientes](active-directory-playbook-ingredients.md)
### [Manual PoC: Implementação](active-directory-playbook-implementation.md)
### [Manual PoC: Blocos Modulares](active-directory-playbook-building-blocks.md)

# Referência
## [Exemplos de código](https://azure.microsoft.com/resources/samples/?service=active-directory)
## [cmdlets do Azure PowerShell](/powershell/azure/overview)
## [Referência de Java API](/java/api)
## [API .NET](/active-directory/adal/microsoft.identitymodel.clients.activedirectory)

# Relacionado
## [Multi-Factor Authentication](/azure/multi-factor-authentication/)
## [Azure AD Connect](./connect/active-directory-aadconnect.md)
## [Azure AD Connect Health](./connect-health/active-directory-aadconnect-health.md)
## [Azure AD para programadores](./develop/active-directory-how-to-integrate.md)
## [Azure AD Privileged Identity Management](./privileged-identity-management/pim-configure.md)

# Recursos
## [Planos de implementação do Azure AD](./fundamentals/active-directory-deployment-plans.md)
## [Fórum de comentários do Azure](https://feedback.azure.com/forums/169401-azure-active-directory)
## [Mapa do Azure](https://azure.microsoft.com/roadmap/?category=security-identity)
## [Fórum do MSDN](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)
## [Preços](https://azure.microsoft.com/pricing/details/active-directory/)
## [Calculadora de preços](https://azure.microsoft.com/pricing/calculator/)
## [Atualizações de serviço](https://azure.microsoft.com/updates/?product=active-directory)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-active-directory)
## [Vídeos](https://azure.microsoft.com/documentation/videos/index/?services=active-directory)
