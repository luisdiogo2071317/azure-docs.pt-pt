# [Documentação do Azure Active Directory](index.md)

# Descrição geral
## [O que é o Azure Active Directory?](fundamentals/active-directory-whatis.md)
## [Sobre a gestão de identidade do Azure](fundamentals/identity-fundamentals.md)
## [Compreender as soluções de identidade do Azure](fundamentals/understand-azure-identity-solutions.md)
## [Associar as subscrições Azure](fundamentals/active-directory-how-subscriptions-associated-directory.md)
## [Residência e considerações de dados](fundamentals/active-directory-data-storage-eu.md)
## [FAQs](fundamentals/active-directory-faq.md)
## [Novidades](fundamentals/whats-new.md)


# Introdução
## [Inscreva-se no Azure AD Premium](fundamentals/active-directory-get-started-premium.md)
## [Adicionar um nome de domínio personalizado](fundamentals/add-custom-domain.md)
## [Configurar o branding da empresa](fundamentals/customize-branding.md)
## [Adicionar utilizadores ao Azure AD](fundamentals/add-users-azure-active-directory.md)
## [Atribuir licenças a utilizadores](fundamentals/license-users-groups.md)
## [Configurar a reposição personalizada de palavra-passe](authentication/quickstart-sspr.md)
## [Adicionar informações de privacidade da sua organização no Azure AD](active-directory-properties-area.md)
## [Aceder ao Azure Active Directory para criar um novo inquilino](fundamentals/active-directory-access-create-new-tenant.md)


# Procedimento
## Planear e conceber
### [Compreender a arquitetura do Azure AD](fundamentals/active-directory-architecture.md)
### [Mapeamento de afirmações no Azure Active Directory](develop/active-directory-claims-mapping.md)
### [Implementar uma solução de identidade híbrida](hybrid/plan-hybrid-identity-design-considerations-overview.md)
#### Determinar os requisitos
##### [Identidade](hybrid/plan-hybrid-identity-design-considerations-business-needs.md)
##### [Sincronização de diretórios](hybrid/plan-hybrid-identity-design-considerations-directory-sync-requirements.md)
##### [Multi-factor auth](hybrid/plan-hybrid-identity-design-considerations-multifactor-auth-requirements.md)
##### [Estratégia de ciclo de vida de identidade](hybrid/plan-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md)
#### [Planear a segurança de dados](hybrid/plan-hybrid-identity-design-considerations-data-protection-strategy.md)
##### [Proteção de dados](hybrid/plan-hybrid-identity-design-considerations-dataprotection-requirements.md)
##### [Gestão de conteúdos](hybrid/plan-hybrid-identity-design-considerations-contentmgt-requirements.md)
##### [Controlo de acesso](hybrid/plan-hybrid-identity-design-considerations-accesscontrol-requirements.md)
##### [Resposta a incidentes](hybrid/plan-hybrid-identity-design-considerations-incident-response-requirements.md)
#### Planear o ciclo de vida de identidade
##### [Tarefas](hybrid/plan-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)
##### [Estratégia de adoção](hybrid/plan-hybrid-identity-design-considerations-identity-adoption-strategy.md)
#### [Passos seguintes?](hybrid/plan-hybrid-identity-design-considerations-nextsteps.md)
#### [Comparação de ferramentas](hybrid/plan-hybrid-identity-design-considerations-tools-comparison.md)

## Gerir utilizadores
### [Adicionar novos utilizadores ao Azure AD](fundamentals/add-users-azure-active-directory.md)
### [Gerir perfis de utilizador](fundamentals/active-directory-users-profile-azure-portal.md)
### [Repor palavras-passe do utilizador](fundamentals/active-directory-users-reset-password-azure-portal.md)
### [Atribuir utilizadores a funções de administrador](fundamentals/active-directory-users-assign-role-azure-portal.md)
### [Adicionar utilizadores convidados a partir de outro diretório (B2B)](b2b/what-is-b2b.md)
#### [Administradores que adicionam utilizadores B2B](b2b/add-users-administrator.md)
#### [Técnicos de informação que adicionam utilizadores B2B](b2b/add-users-information-worker.md)
#### [API e personalização](b2b/customize-invitation-api.md)
#### [Federação da Google](b2b/google-federation.md)
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
### [Eliminar um grupo e seus membros](fundamentals/active-directory-groups-delete-group.md)
### [Gerir definições do grupo](fundamentals/active-directory-groups-settings-azure-portal.md)
## [Gerir relatórios](reports-monitoring/overview-reports.md)
### [Atividade de inícios de sessão](reports-monitoring/concept-sign-ins.md)
### [Atividade de auditoria](reports-monitoring/concept-audit-logs.md)
### [Utilizadores em risco](reports-monitoring/concept-user-at-risk.md)
### [Inícios de sessão de risco](reports-monitoring/concept-risky-sign-ins.md)
### [Eventos de risco](reports-monitoring/concept-risk-events.md)
### [A monitorizar os registos com o Azure Monitor](reports-monitoring/concept-activity-logs-in-azure-monitor.md)
### [FAQ](reports-monitoring/reports-faq.md)

### Tarefas
#### [Transferir um relatório de início de sessão](reports-monitoring/quickstart-download-sign-in-report.md)
#### [Transferir um relatório de auditoria](reports-monitoring/quickstart-download-audit-report.md)
#### [Configurar localizações com nome](reports-monitoring/quickstart-configure-named-locations.md)
#### [Encontrar relatórios de atividades](reports-monitoring/howto-find-activity-reports.md)
#### [Utilizar o Pacote de Conteúdos do Power BI para o Azure AD](reports-monitoring/howto-power-bi-content-pack.md)
#### [Remediar utilizadores sinalizados como em risco](reports-monitoring/howto-remediate-users-flagged-for-risk.md)
#### [Encaminhar os registos de atividade para um hub de eventos do Azure](reports-monitoring/quickstart-azure-monitor-stream-logs-to-event-hub.md)
#### [Arquivar os registos de atividade para uma conta de armazenamento do Azure](reports-monitoring/quickstart-azure-monitor-route-logs-to-storage-account.md)
#### [Integrar os registos de atividade com o Splunk através do Azure Monitor](reports-monitoring/tutorial-integrate-activity-logs-with-splunk.md)
#### [Integrar os registos de atividade com o SumoLogic através do Azure Monitor](reports-monitoring/howto-integrate-activity-logs-with-sumologic.md)
#### [Integre registos de atividades com o Log Analytics com o Azure Monitor](reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

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

## Gerir aplicações
### [Descrição geral](manage-apps/what-is-application-management.md)
### [Introdução](manage-apps/plan-an-application-integration.md)
### [Tutoriais de integração de aplicação SaaS](saas-apps/tutorial-list.md)

### [Aprovisionamento e desaprovisionamento de utilizadores para aplicações SaaS](manage-apps/user-provisioning.md) 
#### [Tutoriais de integração em aplicação](saas-apps/tutorial-list.md) 
#### [Automatizar o aprovisionamento de aplicações com SCIM ativado](manage-apps/use-scim-to-provision-users-and-groups.md) 
#### [Personalizar os mapeamentos de atributos](manage-apps/customize-application-attributes.md) 
#### [Escrever expressões para mapeamentos de atributos](manage-apps/functions-for-customizing-application-data.md) 
#### [Utilizar os filtros de âmbito](manage-apps/define-conditional-rules-for-provisioning-user-accounts.md) 
#### [Comunicar sobre o aprovisionamento automático de utilizadores](manage-apps/check-status-user-account-provisioning.md) 
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
##### [Carateres universais](manage-apps/application-proxy-wildcard.md)
##### [Remover dados pessoais](manage-apps/application-proxy-remove-personal-data.md)


#### Instruções de publicação
##### [Ambiente de Trabalho Remoto](manage-apps/application-proxy-integrate-with-remote-desktop-services.md)
##### [SharePoint](manage-apps/application-proxy-integrate-with-sharepoint-server.md)
##### [Equipas da Microsoft](manage-apps/application-proxy-integrate-with-teams.md)
##### [Tableau](manage-apps/application-proxy-integrate-with-tableau.md)
##### [Qlik](manage-apps/application-proxy-qlik.md)
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

### [Compreender as experiências de consentimento da aplicação do Azure AD](develop/application-consent-experience.md)

### Resolução de problemas



#### Painel de Acesso
##### [A aplicação não aparece](manage-apps/access-panel-troubleshoot-application-not-appearing.md)
##### [Aparece uma aplicação inesperada](manage-apps/access-panel-troubleshoot-unexpected-application.md)
##### [Não é possível iniciar sessão](manage-apps/access-panel-troubleshoot-web-sign-in-problem.md)
##### [Erro ao instalar a extensão de browser](manage-apps/access-panel-extension-problem-installing.md)
##### [Como utilizar o acesso personalizado à aplicação](manage-apps/access-panel-manage-self-service-access.md)
##### [Erro ao utilizar o acesso personalizado à aplicação](manage-apps/access-panel-troubleshoot-self-service-access.md)

#### Adicionar uma aplicação
##### [Escolher o tipo de aplicação](manage-apps/choose-application-type.md)
##### [Problemas comuns - aplicações da galeria](manage-apps/adding-gallery-app-common-problems.md)
##### [Problemas comuns - aplicações que não estão na galeria](manage-apps/adding-non-gallery-app-common-problems.md)

#### Proxy da aplicação
##### [Problema ao apresentar a página da aplicação](manage-apps/application-proxy-page-appearance-broken-problem.md)
##### [O carregamento da aplicação é demasiado demorado](manage-apps/application-proxy-page-load-speed-problem.md)
##### [As ligações na página da aplicação não funcionam](manage-apps/application-proxy-page-links-broken-problem.md)
##### [Que portas devo abrir para a minha aplicação](manage-apps/application-proxy-connectivity-ports-how-to.md)
##### [Não existe nenhum conector que funcione num grupo de conectores da minha aplicação](manage-apps/application-proxy-connectivity-no-working-connector.md)
##### [Configurar no portal de administração](manage-apps/application-proxy-config-how-to.md)
##### [Configurar o início de sessão único na minha aplicação](manage-apps/application-proxy-config-sso-how-to.md)
##### [Problema ao criar uma aplicação no portal de administração](manage-apps/application-proxy-config-problem.md)
##### [Configurar Delegação Restrita de Kerberos](manage-apps/application-proxy-back-end-kerberos-constrained-delegation-how-to.md)
##### [Configurar com o PingAccess](manage-apps/application-proxy-back-end-ping-access-how-to.md)
##### [Erro "Não é Possível Aceder a esta Aplicação Empresarial"](manage-apps/application-proxy-sign-in-bad-gateway-timeout-error.md)
##### [Problema ao instalar o Conector do Agente do Proxy de Aplicações](manage-apps/application-proxy-connector-installation-problem.md)


#### Registo da aplicação
##### [Introduzir os campos para o objeto de aplicação](develop/registration-config-specific-application-property-how-to.md)
##### [Alterar as predefinições de duração do token](develop/registration-config-change-token-lifetime-how-to.md)

#### Autenticação
##### [Configurar pontos finais](develop/registration-config-how-to.md)

#### Acesso Condicional
##### [O cliente não cumpriu os pré-requisitos do Registo de Dispositivos](active-directory-conditional-access.md)
##### [Como e quando as regras da rede empresarial entram em vigor?](https://aka.ms/calocation)
##### [Como aumentar o número de dispositivos que esse utilizador tem permissão para registar no Azure AD?](active-directory-azureadjoin-setup.md)
##### [Como configurar o Acesso Condicional para o Exchange Online?](https://aka.ms/csforexchange)
##### [Como configurar o Acesso Condicional para dispositivos Windows 7?](active-directory-conditional-access.md#device-based-conditional-access)
##### [Que aplicações são suportadas com acesso condicional?](active-directory-conditional-access-supported-apps.md)

#### Encontrar uma API
##### [Encontrar uma API](develop/api-find-an-api-how-to.md)

#### Gerir o acesso
##### [Atribuir utilizadores e grupos a uma aplicação](manage-apps/methods-for-assigning-users-and-groups.md)
##### [Remover o acesso de um utilizador a uma aplicação](manage-apps/methods-for-removing-user-access.md)
##### [Configurar a atribuição de aplicações personalizada](manage-apps/manage-self-service-access.md)
##### [Atribuição a um utilizador inesperado](manage-apps/ways-users-get-assigned-to-applications.md)
##### [Aplicação inesperada na lista de aplicações](manage-apps/application-types.md)

#### Aplicações multi-inquilino
##### [Configurar uma nova aplicação](develop/setup-multi-tenant-app.md)
##### [Adicionar à galeria de aplicações](develop/registration-config-multi-tenant-application-add-to-gallery-how-to.md)

#### Permissões
##### [Escolher as permissões de uma API](develop/perms-for-given-api.md)
##### [Permissões delegadas vs Permissões de aplicação](develop/delegated-and-app-perms.md)
##### [Consentimento da aplicação](develop/consent-framework.md)

#### Aprovisionamento
##### [Quanto tempo demora](manage-apps/application-provisioning-when-will-provisioning-finish-specific-user.md)
##### [Tempo - aplicação da galeria](manage-apps/application-provisioning-when-will-provisioning-finish.md)
##### [Configurar o aprovisionamento de utilizadores - aplicação da galeria](manage-apps/application-provisioning-config-how-to.md)
##### [Problema ao configurar o aprovisionamento de utilizadores - aplicação da galeria](manage-apps/application-provisioning-config-problem.md)
##### [Problema ao guardar as credenciais de administrador ao configurar o aprovisionamento de utilizadores - aplicação da galeria](manage-apps/application-provisioning-config-problem-storage-limit.md)
##### [Os utilizadores não estão aprovisionados - aplicação da galeria](manage-apps/application-provisioning-config-problem-no-users-provisioned.md)
##### [Foram aprovisionados os utilizadores errados - aplicação da galeria](manage-apps/application-provisioning-config-problem-wrong-users-provisioned.md)
##### [Problemas de compatibilidade SCIM - aplicação que não está na galeria](manage-apps/application-provisioning-config-problem-scim-compatibility.md)

#### Início de sessão único
##### [Escolher um método](manage-apps/single-sign-on-modes.md)
##### [Configurar](develop/registration-config-sso-how-to.md)
##### [Configurar federado - aplicações da galeria](manage-apps/configure-federated-single-sign-on-gallery-applications.md)
##### [Problemas comuns da configuração do federado - aplicações da galeria](manage-apps/configure-federated-single-sign-on-gallery-applications-problems.md)
##### [Configurar federado - aplicações que não estão na galeria](manage-apps/configure-federated-single-sign-on-non-gallery-applications.md)
##### [Problemas comuns da configuração do federado - aplicações que não estão na galeria](manage-apps/configure-federated-single-sign-on-non-gallery-applications-problems.md)
##### [Configurar palavra-passe - aplicações da galeria](manage-apps/configure-password-single-sign-on-gallery-applications.md)
##### [Problemas comuns da configuração da palavra-passe - aplicações da galeria](manage-apps/configure-password-single-sign-on-gallery-applications-problems.md)
##### [Configurar palavra-passe - aplicações que não estão na galeria](manage-apps/configure-password-single-sign-on-non-gallery-applications.md)
##### [Problemas comuns da configuração da palavra-passe - aplicações que não estão na galeria](manage-apps/configure-password-single-sign-on-non-gallery-applications-problems.md)

#### Problemas de início de sessão do utilizador
##### [Pedido de consentimento inesperado](manage-apps/application-sign-in-unexpected-user-consent-prompt.md)
##### [Erro de consentimento do utilizador](manage-apps/application-sign-in-unexpected-user-consent-error.md)
##### [Problemas ao iniciar sessão no portal personalizado](manage-apps/application-sign-in-other-problem-deeplink.md)
##### [Problemas ao iniciar sessão no painel de acesso](manage-apps/application-sign-in-other-problem-access-panel.md)
##### [Erro na página de início de sessão da aplicação](manage-apps/application-sign-in-problem-application-error.md)
##### [Problema de início de sessão único com palavra-passe - aplicação que não está na galeria](manage-apps/application-sign-in-problem-password-sso-non-gallery.md)
##### [Problema de início de sessão único com palavra-passe - aplicação da galeria](manage-apps/application-sign-in-problem-password-sso-gallery.md)
##### [Problema ao iniciar sessão numa aplicação da Microsoft](manage-apps/application-sign-in-problem-first-party-microsoft.md)
##### [Problema de início de sessão único federado - aplicação que não está na galeria](manage-apps/application-sign-in-problem-federated-sso-non-gallery.md)
##### [Problema de início de sessão único federado - aplicação da galeria](manage-apps/application-sign-in-problem-federated-sso-gallery.md)
##### [Problema com aplicações com programação personalizada](manage-apps/application-sign-in-problem-custom-dev.md)
##### [Problema com aplicações no local - Proxy de Aplicações](manage-apps/application-sign-in-problem-on-premises-application-proxy.md)


## Gerir o seu diretório
### [Azure AD Connect](hybrid/whatis-hybrid-identity.md)
### Nomes de domínio personalizados
#### [Início rápido](fundamentals/add-custom-domain.md)
### [Administrar o seu diretório](fundamentals/active-directory-administer.md)
### [Integrar identidades no local com o Azure AD Connect](hybrid/whatis-hybrid-identity.md)

### [Configurar durações de token](develop/active-directory-configurable-token-lifetimes.md)

## Proteger as suas identidades

### [Privileged Identity Management](privileged-identity-management/pim-configure.md?toc=%2fazure%2factive-directory%2ftoc.json)



## [Resolução de problemas](fundamentals/active-directory-troubleshooting-support-howto.md)

## Implementar a Prova de Conceito (PoC) do Azure AD
### [Manual de procedimentos PoC: Introdução](active-directory-playbook-intro.md)
### [Manual de procedimentos PoC: Ingredientes](active-directory-playbook-ingredients.md)
### [Manual de procedimentos PoC: Implementação](active-directory-playbook-implementation.md)
### [Manual de procedimentos PoC: Blocos Modulares](active-directory-playbook-building-blocks.md)

# Referência
## [Exemplos de código](https://azure.microsoft.com/resources/samples/?service=active-directory)
## [cmdlets do Azure PowerShell](/powershell/azure/overview)
## [Referência de Java API](/java/api)
## [API .NET](/active-directory/adal/microsoft.identitymodel.clients.activedirectory)

# Relacionado
## [Multi-Factor Authentication](/azure/multi-factor-authentication/)
## [Azure AD Connect](hybrid/whatis-hybrid-identity.md)
## [Azure AD Connect Health](hybrid/whatis-hybrid-identity-health.md)
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
## [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-active-directory)
## [Vídeos](https://azure.microsoft.com/documentation/videos/index/?services=active-directory)
