---
title: O Azure Active Directory prova de conceito blocos de construção de playbook | Documentos da Microsoft
description: Explore e implemente rapidamente os cenários de gestão de acesso e identidades
services: active-directory
keywords: o Azure active directory, o playbook, prova de conceito, prova de conceito
documentationcenter: ''
author: dstefanMSFT
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: dstefan
ms.openlocfilehash: d2a63a1a9b335b7765c5eaf8c90e1d755b2ce9c9
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37867125"
---
# <a name="azure-active-directory-proof-of-concept-playbook-building-blocks"></a>O Azure Active Directory prova do playbook do conceito: blocos modulares

## <a name="catalog-of-roles"></a>Catálogo de funções

| Função | Descrição | Prova de responsabilidade de conceito (PoC) |
| --- | --- | --- |
| **Arquitetura de identidade / equipe de desenvolvimento** | Esta equipe geralmente é aquele que projeta a solução, implementa protótipos, unidades de aprovações e finalmente entrega para operações | Eles fornecem os ambientes e são aqueles avaliar os diferentes cenários a partir da perspectiva de capacidade de gestão |
| **Equipe de operações de identidade no local** | Gere a identidade de diferentes origens no local: florestas do Active Directory, diretórios LDAP, sistemas de RH e fornecedores de identidade de Federação. | Fornecer acesso a locais recursos necessários para os cenários de prova de conceito.<br/>Deve estar envolvidos mínimo possível|
| **Proprietários dos aplicativos técnicos** | Proprietários de técnicos das diferentes aplicações e serviços cloud que se integrará com o Azure AD | Fornecem detalhes sobre aplicações SaaS (potencialmente instâncias para fins de teste) |
| **Administrador Global do Azure AD** | Gere a configuração do Azure AD | Forneça credenciais para configurar o serviço de sincronização. Normalmente, o mesmo como a arquitetura de identidade da Equipe durante a prova de conceito, mas separar durante a fase de operações|
| **Equipe de base de dados** | Proprietários da infraestrutura de base de dados | Fornece acesso ao ambiente do SQL (AD FS ou o Azure AD Connect) para preparações de cenário específico.<br/>Deve estar envolvidos mínimo possível |
| **Equipe de rede** | Proprietários da infra-estrutura de rede | Fornecer o acesso necessário ao nível da rede para os servidores de sincronização aceder as origens de dados e serviços (regras de firewall, portas abertas, regras de IPSec etc.) em nuvem adequadamente |
| **Equipa de segurança** | Define a estratégia de segurança, analisa os relatórios de segurança de várias origens e, segue-se por meio de em averiguações. | Fornecer cenários de avaliação de segurança de destino |

## <a name="common-prerequisites-for-all-building-blocks"></a>Pré-requisitos comuns para todos os blocos de construção

Seguem-se alguns pré-requisitos necessários para qualquer prova de conceito com o Azure AD Premium.

| Pré-requisito | Recursos |
| --- | --- |
| Inquilino do Azure AD definido com uma subscrição do Azure válida | [Como obter um inquilino do Azure Active Directory](active-directory-howto-tenant.md)<br/>**Nota:** se já tiver um ambiente com licenças do Azure AD Premium, pode obter uma subscrição sem limite ao navegar para https://aka.ms/accessaad <br/>Saiba mais em: https://blogs.technet.microsoft.com/enterprisemobility/2016/02/26/azure-ad-mailbag-azure-subscriptions-and-azure-ad-2/ e https://technet.microsoft.com/library/dn832618.aspx |
| Domínios definidos e verificado | [Adicionar um nome de domínio personalizado ao Azure Active Directory](active-directory-domains-add-azure-portal.md)<br/>**Nota:** algumas cargas de trabalho como o Power BI foi aprovisionou um inquilino do azure AD nos bastidores. Para verificar se um determinado domínio está associado a um inquilino, navegue até ao https://login.microsoftonline.com/{domain}/v2.0/.well-known/openid-configuration. Se obter uma resposta com êxito, em seguida, o domínio já está atribuído a um inquilino e assumir o controlo pode ser necessário. Se assim for, contacte a Microsoft para obter orientações adicionais. Saiba mais sobre as opções de aquisição em: [o que é a inscrição Self-Service para o Azure?](users-groups-roles/directory-self-service-signup.md) |
| O Azure AD Premium ou EMS ativado de avaliação | [Azure Active Directory Premium gratuito durante um mês](https://azure.microsoft.com/trial/get-started-active-directory/) |
| Atribuiu o Azure AD Premium ou licenças do EMS aos utilizadores de prova de conceito | [Licenciar por conta própria e os seus utilizadores no Azure Active Directory](active-directory-licensing-get-started-azure-portal.md) |
| Credenciais de Administrador Global do AD do Azure | [Atribuir funções de administrador no Azure Active Directory](users-groups-roles/directory-assign-admin-roles.md) |
| Opcional mas vivamente recomendado: ambiente de laboratório paralela como contingência | [Pré-requisitos do Azure AD Connect](./connect/active-directory-aadconnect-prerequisites.md) |

## <a name="directory-synchronization---password-hash-sync-phs---new-installation"></a>Diretório sincronização - sincronização de Hash de palavra-passe (PHS) - nova instalação

Obter uma estimativa de tempo para concluir: uma hora para menos de 1.000 usuários de prova de conceito

### <a name="pre-requisites"></a>Pré-requisitos

| Pré-requisito | Recursos |
| --- | --- |
| Servidor para executar o Azure AD Connect | [Pré-requisitos do Azure AD Connect](./connect/active-directory-aadconnect-prerequisites.md) |
| Os utilizadores de prova de conceito, no mesmo domínio e parte de um grupo de segurança e a UO de destino | [Instalação personalizada do Azure AD Connect](./connect/active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) |
| Funcionalidades do Azure AD Connect necessários para a prova de conceito são identificadas | [Ligar o Active Directory com o Azure Active Directory – configurar a sincronização de recursos](./connect/active-directory-aadconnect.md#configure-sync-features) |
| É necessário ter credenciais no local e ambientes de cloud  | [Azure AD Connect: contas e permissões](./connect/active-directory-aadconnect-accounts-permissions.md) |

### <a name="steps"></a>Passos

| Passo | Recursos |
| --- | --- |
| Baixe a versão mais recente do Azure AD Connect | [Baixe o Microsoft Azure Active Directory Connect](https://www.microsoft.com/download/details.aspx?id=47594) |
| Instalar o Azure AD Connect com o caminho mais simples: Express <br/>1. Filtrar a UO para minimizar o tempo de ciclo de sincronização de destino<br/>2. Escolha o conjunto de destino de utilizadores no grupo de no local.<br/>3. Implementar os recursos necessários para os outros temas de prova de conceito | [Azure AD Connect: Instalação personalizada: domínio e UO filtragem](./connect/active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) <br/>[Azure AD Connect: Instalação personalizada: grupo de filtragem com base em](./connect/active-directory-aadconnect-get-started-custom.md#sync-filtering-based-on-groups)<br/>[O Azure AD Connect: Integrar as identidades no local do Azure Active Directory: configurar funcionalidades de sincronização](./connect/active-directory-aadconnect.md#configure-sync-features) |
| Abra o Azure AD Connect da interface do Usuário e ver o em execução perfis concluída (importar, sincronização e exportação) | [Sincronização do Azure AD Connect: Scheduler](./connect/active-directory-aadconnectsync-feature-scheduler.md) |
| Abra o [portal de gestão do Azure AD](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/), aceda ao painel de "Todos os utilizadores", Adicionar coluna de "Origem de autoridade" e vê que os utilizadores aparecem, marcada corretamente como proveniente de "Windows Server AD" | [Portal de gestão do Azure AD](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) |

### <a name="considerations"></a>Considerações

1. Examinar as considerações de segurança da sincronização de hash de palavra-passe [aqui](./connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md).  Se a sincronização de hash de palavra-passe para utilizadores de produção piloto não definitivamente com é uma opção, em seguida, considere as seguintes alternativas:
   * Crie utilizadores de teste no domínio de produção. Certifique-se de que não sincronizar a qualquer outra conta
   * Mudar para um ambiente de UAT
2.  Se deseja buscar o Federação, vale a pena para compreender que os custos associados uma solução federada do fornecedor de identidade no local para além da prova de conceito e que medir os benefícios que está procurando:
    * Ele está no caminho crítico para que tenha de design de elevada disponibilidade
    * É um serviço no local, que terá de plano de capacidade
    * É um serviço no local, que terá de monitor/manter/correção

Saiba mais: [identidade Noções básicas sobre o Office 365 e Azure Active Directory - identidade federada](https://support.office.com/article/Understanding-Office-365-identity-and-Azure-Active-Directory-06a189e7-5ec6-4af2-94bf-a22ea225a7a9#bk_federated)

## <a name="branding"></a>Imagem corporativa

Obter uma estimativa de tempo para concluir: 15 minutos

### <a name="pre-requisites"></a>Pré-requisitos

| Pré-requisito | Recursos |
| --- | --- |
| Recursos (imagens, logótipos, etc.); Para melhor visualização, certifique-se de que os ativos de tem os tamanhos recomendados. | [Adicionar imagem corporativa na sua página de início de sessão no Azure Active Directory](active-directory-branding-custom-signon-azure-portal.md) |
| Opcional: Se o ambiente tem um servidor do ADFS, o acesso ao servidor para personalizar o tema web | [AD FS início de sessão personalização de utilizador](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/ad-fs-user-sign-in-customization) |
| Computador cliente para executar a experiência de início de sessão do utilizador final |  |
| Opcional: Experiência de dispositivos móveis para validar |  |

### <a name="steps"></a>Passos

| Passo | Recursos |
| --- | --- |
| Aceda ao Portal de gestão do Azure AD | [Portal de gestão do Azure AD - imagem corporativa da empresa](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/LoginTenantBranding) |
| Carregue os ativos para a página de início de sessão (logótipo de hero, logotipo pequeno, etiquetas, etc.). Opcionalmente, se tiver o AD FS, alinhar os ativos mesmo com páginas de início de sessão do AD FS | [Adicionar imagem corporativa para o seu início de sessão e painel de acesso a páginas da empresa: elementos personalizáveis](fundamentals/customize-branding.md) |
| Aguarde alguns minutos para que a alteração entrem completamente em vigor |  |
| Inicie sessão com as credenciais de utilizador de prova de conceito para https://myapps.microsoft.com |  |
| Confirmar o aspeto e funcionalidade no browser | [Adicionar imagem corporativa para o seu início de sessão e painel de acesso a páginas da empresa](fundamentals/customize-branding.md) |
| Opcionalmente, confirme a aparência de outros dispositivos |  |

### <a name="considerations"></a>Considerações

Se o aspeto e funcionalidade antigo permanecem após a personalização, em seguida, libere o cache do cliente de navegador e repita a operação.

## <a name="group-based-licensing"></a>Licenciamento baseado em grupo

Obter uma estimativa de tempo para concluir: 10 minutos

### <a name="pre-requisites"></a>Pré-requisitos

| Pré-requisito | Recursos |
| --- | --- |
| Todos os utilizadores de prova de conceito fazem parte de um grupo de segurança (na cloud ou no local) | [Criar um grupo e adicionar membros no Azure Active Directory](fundamentals/active-directory-groups-create-azure-portal.md) |

### <a name="steps"></a>Passos

| Passo | Recursos |
| --- | --- |
| Aceda ao painel de licenças no Portal de gestão do Azure AD | [Portal de gestão do Azure AD: licenciamento](https://portal.azure.com/#blade/Microsoft_AAD_IAM/LicensesMenuBlade/Products) |
| Atribua as licenças para o grupo de segurança com utilizadores de prova de conceito. | [Atribuir licenças a um grupo de utilizadores no Azure Active Directory](users-groups-roles/licensing-groups-assign.md) |

### <a name="considerations"></a>Considerações

Em caso de quaisquer problemas, aceda a [cenários, limitações e problemas conhecidos com o uso de grupos para gerir o licenciamento no Azure Active Directory](users-groups-roles/licensing-group-advanced.md)

## <a name="saas-federated-sso-configuration"></a>Configuração de SSO Federado de SaaS

Obter uma estimativa de tempo para concluir: 60 minutos

### <a name="pre-requisites"></a>Pré-requisitos

| Pré-requisito | Recursos |
| --- | --- |
| Ambiente de teste da aplicação SaaS disponível. Neste guia, utilizamos ServiceNow como um exemplo.<br/>Recomendamos vivamente a utilizar uma instância de teste para minimizar o efeito de fricção como navegar de qualidade de dados existente e mapeamentos. | Aceda a https://developer.servicenow.com/app.do#! / principal para iniciar o processo de obtenção de uma instância de teste |
| Acesso de administrador para o console de gerenciamento do ServiceNow | [Tutorial: Integração do Azure Active Directory com ServiceNow](saas-apps/servicenow-tutorial.md) |
| Destino definido de usuários para atribuir a aplicação. Recomenda-se um grupo de segurança que contém os utilizadores de prova de conceito. <br/>Se não for possível criar o grupo, em seguida, atribua os utilizadores diretamente para a aplicação para a prova de conceito | [Atribuir um utilizador ou grupo a uma aplicação empresarial no Azure Active Directory](manage-apps/assign-user-or-group-access-portal.md) |

### <a name="steps"></a>Passos

| Passo | Recursos |
| --- | --- |
| Partilhar o tutorial para todos os atores de Documentation da Microsoft  | [Tutorial: Integração do Azure Active Directory com ServiceNow](saas-apps/servicenow-tutorial.md) |
| Defina uma reunião de trabalho e siga os passos de tutoriais com cada ator. | [Tutorial: Integração do Azure Active Directory com ServiceNow](saas-apps/servicenow-tutorial.md) |
| Atribua a aplicação para o grupo identificado nos pré-requisitos. Se a prova de conceito tiver acesso condicional no âmbito, pode rever que mais tarde e adicionar a MFA e semelhantes. <br/>Tenha em atenção de que isto irá iniciar o processo de aprovisionamento (se configurada) |  [Atribuir um utilizador ou grupo a uma aplicação empresarial no Azure Active Directory](manage-apps/assign-user-or-group-access-portal.md) <br/>[Criar um grupo e adicionar membros no Azure Active Directory](fundamentals/active-directory-groups-create-azure-portal.md) |
| Utilizar no Portal do Azure AD management para adicionar a aplicação de ServiceNow da Galeria| [Gestão do AD Azure Portal: aplicações empresariais](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/Overview) <br/>[O que há de novo no gerenciamento de aplicativo empresarial no Azure Active Directory](active-directory-enterprise-apps-whats-new-azure-portal.md) |
| No painel de "Início de sessão único" da aplicação do ServiceNow ativar "baseado em SAML Sign-on" |  |
| Preencha os campos de "Início de sessão no URL" e "Identificador" com o URL do ServiceNow<br/>Selecione a caixa para "Ativar o novo certificado"<br/>e guardar as definições |  |
| Abra o painel de "Configurar ServiceNow" na parte inferior do painel para ver instruções personalizadas para configurar o ServiceNow |  |
| Siga as instruções para configurar o ServiceNow |  |
| No painel de "Aprovisionamento" da aplicação de ServiceNow ativar o aprovisionamento "Automático" | [Gerir a conta de utilizador de aprovisionamento de aplicações empresariais no novo portal do Azure](manage-apps/configure-automatic-user-provisioning-portal.md) |
| Aguarde alguns minutos enquanto o aprovisionamento esteja concluído.  Entretanto, pode verificar os relatórios de aprovisionamento |  |
| Inicie sessão no https://myapps.microsoft.com/ como um utilizador de teste que tem acesso | [O que é o painel de acesso?](active-directory-saas-access-panel-introduction.md) |
| Clique no mosaico da aplicação que acabou de ser criado. Confirmar o acesso |  |
| Opcionalmente, pode verificar os relatórios de utilização da aplicação. Tenha em atenção de que existe alguma latência, por isso terá de aguardar algum tempo para ver o tráfego nos relatórios. | [Relatórios de atividade de início de sessão no portal do Azure Active Directory: utilização de aplicações geridas](active-directory-reporting-activity-sign-ins.md#usage-of-managed-applications)<br/>[Políticas de retenção de relatórios do Azure Active Directory](active-directory-reporting-retention.md) |

### <a name="considerations"></a>Considerações

1. Acima [Tutorial](saas-apps/servicenow-tutorial.md) refere-se para o Azure antigo experiência de gestão do AD. Mas prova de conceito baseia-se no [guia de introdução](active-directory-enterprise-apps-whats-new-azure-portal.md#quickstart-get-going-with-your-new-application-right-away) experiência.
2. Se o aplicativo de destino não está presente na galeria, em seguida, pode utilizar "Traga a sua própria aplicação". Saiba mais: [quais são as novidades na gestão de aplicação empresarial no Azure Active Directory: adicionar aplicações personalizadas num único local](active-directory-enterprise-apps-whats-new-azure-portal.md#add-custom-applications-from-one-place)

## <a name="saas-password-sso-configuration"></a>Configuração de SSO de palavra-passe de SaaS

Obter uma estimativa de tempo para concluir: 15 minutos

### <a name="pre-requisites"></a>Pré-requisitos

| Pré-requisito | Recursos |
| --- | --- |
| Ambiente de teste para aplicações SaaS. Um exemplo de SSO de palavra-passe é o HipChat e o Twitter. Para qualquer outro aplicativo, é necessário o URL exato da página com o formulário de início de sessão de html. | [Twitter no Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/aad.twitter)<br/>[HipChat no Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/aad.hipchat) |
| Contas para os aplicativos de teste. | [Inscrever-se no Twitter](https://twitter.com/signup?lang=en)<br/>[Inscreva-se gratuitamente: HipChat](https://www.hipchat.com/sign_up) |
| Destino definido de usuários para atribuir a aplicação. Os utilizadores contidos de um grupo de segurança é recomendado. | [Atribuir um utilizador ou grupo a uma aplicação empresarial no Azure Active Directory](manage-apps/assign-user-or-group-access-portal.md) |
| Acesso de administrador local para um computador para implementar a extensão do painel de acesso para o Internet Explorer, Chrome ou Firefox | [Extensão do painel de acesso para o IE](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)<br/>[Extensão do painel de acesso para o Chrome](https://go.microsoft.com/fwLink/?LinkID=311859&clcid=0x409)<br/>[Extensão do painel de acesso para Firefox](https://go.microsoft.com/fwLink/?LinkID=626998&clcid=0x409) |

### <a name="steps"></a>Passos

| Passo | Recursos |
| --- | --- |
| Instalar a extensão de browser | [Extensão do painel de acesso para o IE](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)<br/>[Extensão do painel de acesso para o Chrome](https://go.microsoft.com/fwLink/?LinkID=311859&clcid=0x409)<br/>[Extensão do painel de acesso para Firefox](https://go.microsoft.com/fwLink/?LinkID=626998&clcid=0x409) |
| Configurar a aplicação da Galeria | [O que há de novo no gerenciamento de aplicativo empresarial no Azure Active Directory: A Galeria de aplicações de novas e melhoradas](active-directory-enterprise-apps-whats-new-azure-portal.md#improvements-to-the-azure-active-directory-application-gallery) |
| Configurar o SSO de palavra-passe | [Gerir o início de sessão único para aplicações empresariais no novo portal do Azure: início de sessão baseado em palavra-passe](manage-apps/configure-single-sign-on-portal.md#password-based-sign-on) |
| Atribuir a aplicação para o grupo identificado nos pré-requisitos | [Atribuir um utilizador ou grupo a uma aplicação empresarial no Azure Active Directory](manage-apps/assign-user-or-group-access-portal.md) |
| Inicie sessão no https://myapps.microsoft.com/ como um utilizador de teste que tem acesso |  |
| Clique no mosaico da aplicação que acabou de ser criado. | [O que é o painel de acesso?: SSO baseado em palavra-passe sem o aprovisionamento de identidade](active-directory-saas-access-panel-introduction.md#password-based-sso-without-identity-provisioning) |
| Forneça a credencial de aplicação | [O que é o painel de acesso?: SSO baseado em palavra-passe sem o aprovisionamento de identidade](active-directory-saas-access-panel-introduction.md#password-based-sso-without-identity-provisioning) |
| Fechar o navegador e repetir o início de sessão. Desta vez o utilizador deverá ver o acesso totalmente integrado ao aplicativo. |  |
| Opcionalmente, pode verificar os relatórios de utilização da aplicação. Tenha em atenção de que existe alguma latência, por isso terá de aguardar algum tempo para ver o tráfego nos relatórios. | [Relatórios de atividade de início de sessão no portal do Azure Active Directory: utilização de aplicações geridas](active-directory-reporting-activity-sign-ins.md#usage-of-managed-applications)<br/>[Políticas de retenção de relatórios do Azure Active Directory](active-directory-reporting-retention.md) |

### <a name="considerations"></a>Considerações

Se o aplicativo de destino não está presente na galeria, em seguida, pode utilizar "Traga a sua própria aplicação". Saiba mais: [quais são as novidades na gestão de aplicação empresarial no Azure Active Directory: adicionar aplicações personalizadas num único local](active-directory-enterprise-apps-whats-new-azure-portal.md#add-custom-applications-from-one-place)

 Tenha em atenção os seguintes requisitos:
   * Aplicação deve ter um URL de início de sessão conhecidos
   * A página de início de sessão deve conter um formulário HTML com um mais campos de texto que as extensões do navegador podem automaticamente-preencher. No mínimo, esta deve conter o nome de utilizador e palavra-passe.

## <a name="saas-shared-accounts-configuration"></a>Configuração de contas compartilhadas de SaaS

Obter uma estimativa de tempo para concluir: 30 minutos

### <a name="pre-requisites"></a>Pré-requisitos

| Pré-requisito | Recursos |
| --- | --- |
| A lista de aplicações de destino e os URLS exatos início de sessão antes do tempo. Por exemplo, pode utilizar o Twitter. | [Twitter no Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/aad.twitter)<br/>[Inscrever-se no Twitter](https://twitter.com/signup?lang=en) |
| Partilhado credenciais para esta aplicação SaaS. | [Partilhar contas com o Azure AD](active-directory-sharing-accounts.md)<br/>[Azure AD automatizada de palavra-passe rollover para Facebook, Twitter e LinkedIn agora em pré-visualização! -Blogue Enterprise Mobility and Security] (https://blogs.technet.microsoft.com/enterprisemobility/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview/ ) |
| Credenciais para, pelo menos, dois membros de equipe que vão aceder a mesma conta. Têm de ser parte de um grupo de segurança. | [Atribuir um utilizador ou grupo a uma aplicação empresarial no Azure Active Directory](manage-apps/assign-user-or-group-access-portal.md) |
| Acesso de administrador local para um computador para implementar a extensão do painel de acesso para o Internet Explorer, Chrome ou Firefox | [Extensão do painel de acesso para o IE](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)<br/>[Extensão do painel de acesso para o Chrome](https://go.microsoft.com/fwLink/?LinkID=311859&clcid=0x409)<br/>[Extensão do painel de acesso para Firefox](https://go.microsoft.com/fwLink/?LinkID=626998&clcid=0x409) |

### <a name="steps"></a>Passos

| Passo | Recursos |
| --- | --- |
| Instalar a extensão de browser | [Extensão do painel de acesso para o IE](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)<br/>[Extensão do painel de acesso para o Chrome](https://go.microsoft.com/fwLink/?LinkID=311859&clcid=0x409)<br/>[Extensão do painel de acesso para Firefox](https://go.microsoft.com/fwLink/?LinkID=626998&clcid=0x409) |
| Configurar a aplicação da Galeria | [O que há de novo no gerenciamento de aplicativo empresarial no Azure Active Directory: A Galeria de aplicações de novas e melhoradas](active-directory-enterprise-apps-whats-new-azure-portal.md#improvements-to-the-azure-active-directory-application-gallery) |
| Configurar o SSO de palavra-passe | [Gerir o início de sessão único para aplicações empresariais no novo portal do Azure: início de sessão baseado em palavra-passe](manage-apps/configure-single-sign-on-portal.md#password-based-sign-on) |
| Atribuir a aplicação para o grupo identificado nos pré-requisitos ao atribuir-lhes credenciais | [Atribuir um utilizador ou grupo a uma aplicação empresarial no Azure Active Directory](manage-apps/assign-user-or-group-access-portal.md) |
| Iniciar sessão, como diferentes usuários, essa aplicação de acesso, como o **mesmo partilhado conta.**  |  |
| Opcionalmente, pode verificar os relatórios de utilização da aplicação. Tenha em atenção de que existe alguma latência, por isso terá de aguardar algum tempo para ver o tráfego nos relatórios. | [Relatórios de atividade de início de sessão no portal do Azure Active Directory: utilização de aplicações geridas](active-directory-reporting-activity-sign-ins.md#usage-of-managed-applications)<br/>[Políticas de retenção de relatórios do Azure Active Directory](active-directory-reporting-retention.md) |


### <a name="considerations"></a>Considerações

Se o aplicativo de destino não está presente na galeria, em seguida, pode utilizar "Traga a sua própria aplicação". Saiba mais: [quais são as novidades na gestão de aplicação empresarial no Azure Active Directory: adicionar aplicações personalizadas num único local](active-directory-enterprise-apps-whats-new-azure-portal.md#add-custom-applications-from-one-place)

 Tenha em atenção os seguintes requisitos:
   * Aplicação deve ter um URL de início de sessão conhecidos
   * A página de início de sessão deve conter um formulário HTML com um mais campos de texto que as extensões do navegador podem automaticamente-preencher. No mínimo, esta deve conter o nome de utilizador e palavra-passe.

## <a name="app-proxy-configuration"></a>Configuração de Proxy de aplicação

Obter uma estimativa de tempo para concluir: 20 minutos

### <a name="pre-requisites"></a>Pré-requisitos

| Pré-requisito | Recursos |
| --- | --- |
| Um básico do Microsoft Azure AD ou subscrição premium e um diretório do Azure AD para o qual for um administrador global | [Edições do Azure Active Directory](fundamentals/active-directory-whatis.md) |
| Uma aplicação web alojada no local que pretende configurar para acesso remoto |  |
| Um servidor a executar o Windows Server 2012 R2 ou Windows 8.1 ou superior, em que pode instalar o conector do Proxy de aplicações | [Compreender os conectores de Proxy de aplicações do Azure AD](manage-apps/application-proxy-connectors.md) |
| Se existir uma firewall no caminho, certifique-se de que está aberta para que o conector possa fazer pedidos HTTPS (TCP) para o Proxy de aplicações | [Ativar o Proxy de aplicações no portal do Azure: pré-requisitos do Proxy de aplicações](manage-apps/application-proxy-enable.md#application-proxy-prerequisites) |
| Se a sua organização utilizar servidores proxy para estabelecer ligação à internet, dar uma olhada no blog do publicar a trabalhar com servidores de proxy no local existentes para obter detalhes sobre como configurá-las | [Trabalhar com servidores de proxy no local existentes](manage-apps/application-proxy-configure-connectors-with-proxy-servers.md) |


### <a name="steps"></a>Passos

| Passo | Recursos |
| --- | --- |
| Instalar um conector no servidor | [Ativar o Proxy de aplicações no portal do Azure: Instale e registe o conector](manage-apps/application-proxy-enable.md#install-and-register-a-connector) |
| Publicar a aplicação no local no Azure AD como uma aplicação de Proxy de aplicações | [Publicar aplicações com o Proxy de aplicações do Azure AD](manage-apps/application-proxy-publish-azure-portal.md) |
| Atribuir utilizadores de teste | [Publicar aplicações com o Proxy de aplicações do Azure AD: adicionar um utilizador de teste](manage-apps/application-proxy-publish-azure-portal.md#add-a-test-user) |
| Opcionalmente, configure uma única experiência de logon para os seus utilizadores | [Fornecer início de sessão único com o Proxy de aplicações do Azure AD](manage-apps/application-proxy-configure-single-sign-on-password-vaulting.md) |
| Testar a aplicação ao iniciar sessão no portal MyApps como utilizador atribuído | https://myapps.microsoft.com |

### <a name="considerations"></a>Considerações

1. Embora sugerimos que colocar o conector na sua rede empresarial, existem casos quando verá um melhor desempenho, colocando-o na cloud. Saiba mais: [considerações sobre a topologia de rede ao utilizar o Proxy de aplicações do Azure Active Directory](manage-apps/application-proxy-network-topology.md)
2. Para obter mais detalhes de segurança e como isso fornece um acesso remoto seguro particularmente ver a solução apenas mantendo as ligações de saída: [considerações de segurança para aceder a aplicações remotamente utilizando o Proxy de aplicações do Azure AD](manage-apps/application-proxy-security.md)

## <a name="generic-ldap-connector-configuration"></a>Configuração do conector LDAP genérico

Obter uma estimativa de tempo para concluir: 60 minutos

> [!IMPORTANT]
> Esta é uma configuração avançada que exigem um pouco familiarizado com o FIM/MIM. Se usado na produção, aconselhamos perguntas sobre esta configuração percorrer [Premier Support](https://support.microsoft.com/premier).

### <a name="pre-requisites"></a>Pré-requisitos

| Pré-requisito | Recursos |
| --- | --- |
| O Azure AD Connect instalado e configurado | Bloco de construção: [sincronização de diretórios - sincronização de Hash de palavra-passe](#directory-synchronization--password-hash-sync-phs--new-installation) |
| Requisitos de reunião de instância do ADLDS | [Referência técnica do conector LDAP genérica: Descrição geral do conector LDAP genérico](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-connector-genericldap#overview-of-the-generic-ldap-connector) |
| Lista de cargas de trabalho, o que os utilizadores estão a utilizar e atributos associados estas cargas de trabalho | [Sincronização do Azure AD Connect: atributos sincronizados com o Azure Active Directory](./connect/active-directory-aadconnectsync-attributes-synchronized.md) |


### <a name="steps"></a>Passos

| Passo | Recursos |
| --- | --- |
| Adicionar conector LDAP genérico | [Referência técnica do conector LDAP genérica: criar um novo conector](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-connector-genericldap#create-a-new-connector) |
| Criar perfis de execução para o conector criado (importação completa, a importação delta, sincronização completa, sincronização de diferenças, exportação) | [Criar um perfil de execução de agente de gestão](https://technet.microsoft.com/library/jj590219(v=ws.10).aspx)<br/> [Utilização de conectores com o Azure AD Connect sincronização o Service Manager](./connect/active-directory-aadconnectsync-service-manager-ui-connectors.md)|
| Importação completa de perfil de execução e certifique-se de que existem objetos no espaço conector | [Procurar um objeto de espaço conector](https://technet.microsoft.com/library/jj590287(v=ws.10).aspx)<br/>[Utilização de conectores com o Azure AD Connect sincronização o Service Manager: Procurar espaço conector](./connect/active-directory-aadconnectsync-service-manager-ui-connectors.md#search-connector-space) |
| Criar regras de sincronização, para que os objetos no Metaverso têm atributos necessários para cargas de trabalho | [Sincronização do Azure AD Connect: melhores práticas para alterar a configuração predefinida: alterações às regras de sincronização](./connect/active-directory-aadconnectsync-best-practices-changing-default-configuration.md#changes-to-synchronization-rules)<br/>[Sincronização do Azure AD Connect: compreender a aprovisionamento declarativo](./connect/active-directory-aadconnectsync-understanding-declarative-provisioning.md)<br/>[Sincronização do Azure AD Connect: Noções básicas sobre expressões de aprovisionamento declarativas](./connect/active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) |
| Iniciar o ciclo de sincronização completa | [Sincronização do Azure AD Connect: Scheduler: iniciar o agendador](./connect/active-directory-aadconnectsync-feature-scheduler.md#start-the-scheduler) |
| Em caso de problemas de fazer a resolução de problemas | [Resolver problemas relacionados com um objeto que não está a sincronizar com o Azure AD](./connect/active-directory-aadconnectsync-troubleshoot-object-not-syncing.md) |
| Verifique se, utilizador LDAP pode início de sessão e aceder à aplicação | https://myapps.microsoft.com |

### <a name="considerations"></a>Considerações

> [!IMPORTANT]
> Esta é uma configuração avançada que exigem um pouco familiarizado com o FIM/MIM. Se usado na produção, aconselhamos perguntas sobre esta configuração percorrer [Premier Support](https://support.microsoft.com/premier).

## <a name="groups---delegated-ownership"></a>Grupos - propriedade delegado

Obter uma estimativa de tempo para concluir: 10 minutos

### <a name="pre-requisites"></a>Pré-requisitos

| Pré-requisito | Recursos |
| --- | --- |
| Aplicação SaaS (SSO federado ou SSO de palavra-passe) foi já configurada | Bloco de construção: [SaaS configuração de SSO federado](#saas-federated-sso-configuration) |
| Grupo de cloud que é atribuído o acesso à aplicação no #1 é identificado | Bloco de construção: [SaaS configuração de SSO federado](#saas-federated-sso-configuration) <br/>[Criar um grupo e adicionar membros no Azure Active Directory](fundamentals/active-directory-groups-create-azure-portal.md) |
| As credenciais para o proprietário do grupo estão disponíveis | [Manage access to resources with Azure Active Directory groups](fundamentals/active-directory-manage-groups.md) (Gerir o acesso aos recursos com grupos do Azure Active Directory) |
| Foi identificado as credenciais para o operador de informações a aceder as aplicações | [O que é o painel de acesso?](active-directory-saas-access-panel-introduction.md) |


### <a name="steps"></a>Passos

| Passo | Recursos |
| --- | --- |
| Identificar o grupo que tenha sido concedido acesso à aplicação e configure o proprietário de determinado grupo| [Gerir as definições para um grupo no Azure Active Directory ](fundamentals/active-directory-groups-settings-azure-portal.md) |
| Inicie sessão como o proprietário do grupo, consulte a associação a grupos no separador de grupos do painel de acesso | [Página de gestão de grupos do Active Directory do Azure](https://account.activedirectory.windowsazure.com/r#/groups) |
| Adicionar o operador de informações que pretende testar |  |
| Inicie sessão como o operador de informações, confirme que o mosaico está disponível | [O que é o painel de acesso?](active-directory-saas-access-panel-introduction.md) |

### <a name="considerations"></a>Considerações

Se a aplicação tiver ativado o aprovisionamento, poderá ter de aguardar alguns minutos para que o aprovisionamento seja concluída antes de acesso à aplicação como o operador de informações.

## <a name="saas-and-identity-lifecycle"></a>Ciclo de vida de identidade e de SaaS

### <a name="pre-requisites"></a>Pré-requisitos

| Pré-requisito | Recursos |
| --- | --- |
| Aplicação SaaS (SSO federado ou SSO de palavra-passe) foi já configurada | Bloco de construção: [SaaS configuração de SSO federado](#saas-federated-sso-configuration) |
| Grupo de cloud que é atribuído o acesso à aplicação no #1 é identificado | Bloco de construção: [SaaS configuração de SSO federado](#saas-federated-sso-configuration) <br/>[Criar um grupo e adicionar membros no Azure Active Directory](fundamentals/active-directory-groups-create-azure-portal.md) |
| Foi identificado as credenciais para o operador de informações a aceder as aplicações | [O que é o painel de acesso?](active-directory-saas-access-panel-introduction.md) |


### <a name="steps"></a>Passos

| Passo | Recursos |
| --- | --- |
| Remover o utilizador do grupo que a aplicação é atribuída a | [Gerir a associação de grupo para utilizadores no seu inquilino do Azure Active Directory](fundamentals/active-directory-groups-members-azure-portal.md) |
| Aguarde alguns minutos para cancelamento de aprovisionamento | [Automatizada o aprovisionamento de utilizador de aplicações de SaaS no Azure AD: como faz o trabalho de aprovisionamento automatizado?](active-directory-saas-app-provisioning.md#how-does-automatic-provisioning-work) |
| Numa sessão de browser separados, inicie sessão como o operador de informações para meu portal de aplicações e confirme esse mosaico está em falta | http://myapps.microsoft.com |


### <a name="considerations"></a>Considerações

Utilize para tirar conclusões o cenário de prova de conceito para leavers e/ou deixe de ausência cenários. Se o utilizador é desativado no AD no local ou removida, já não existe uma forma de iniciar sessão na aplicação SaaS.

## <a name="self-service-access-to-application-management"></a>Self-Service do acesso à gestão de aplicações

Obter uma estimativa de tempo para concluir: 10 minutos

### <a name="pre-requisites"></a>Pré-requisitos

| Pré-requisito | Recursos |
| --- | --- |
| Identificar os utilizadores de prova de conceito que irão pedir acesso aos aplicativos, como parte do grupo de segurança | Bloco de construção: [SaaS configuração de SSO federado](#saas-federated-sso-configuration) |
| Aplicação de destino implementada | Bloco de construção: [SaaS configuração de SSO federado](#saas-federated-sso-configuration) |

### <a name="steps"></a>Passos

| Passo | Recursos |
| --- | --- |
| Aceda ao painel de aplicações empresariais no Portal de gestão do Azure AD | [Portal de gestão do Azure AD: Aplicações empresariais](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps/menuId/) |
| Configurar a aplicação de pré-requisitos com self-service | [O que há de novo no gerenciamento de aplicativo empresarial no Azure Active Directory: configurar o acesso de aplicações self-service](active-directory-enterprise-apps-whats-new-azure-portal.md#configure-self-service-application-access) |
| Inicie sessão como o operador de informações para meu portal de aplicações | http://myapps.microsoft.com |
| Tenha em atenção "+ Adicionar aplicação" botão no op da página. Utilize-o para obter acesso à aplicação |  |

### <a name="considerations"></a>Considerações

Os aplicativos escolhidos podem ter requisitos de aprovisionamento, então vai imediatamente a aplicação pode fazer com que alguns erros. Se o aplicativo escolhido suporta o aprovisionamento com o azure ad e é configurado, pode usar isso como uma oportunidade para mostrar o fluxo todo trabalho ponto a ponto. Consulte o bloco de construção para [SaaS configuração de SSO federado](#saas-federated-sso-configuration) para obter recomendações

## <a name="self-service-password-reset"></a>Reposição de palavra-passe Self-Service

Obter uma estimativa de tempo para concluir: 15 minutos

### <a name="pre-requisites"></a>Pré-requisitos

| Pré-requisito | Recursos |
| --- | --- |
| Ative a gestão de palavras-passe self-service no seu inquilino. | [O Azure Active Directory reposição palavra-passe para os administradores de TI](active-directory-passwords-update-your-own-password.md) |
| Ative a palavra-passe write back para gerir palavras-passe no local. Observe que isso requer específico do Azure AD Connect versões | [Pré-requisitos da Repetição de Escrita de Palavras-passe](authentication/howto-sspr-writeback.md) |
| Identificar os utilizadores de prova de conceito que irão utilizar esta funcionalidade e certifique-se de que são membros de um grupo de segurança. Os utilizadores têm de ser não-administradores para demonstrar totalmente a capacidade | [Personalizar: Gestão de palavra-passe de AD do Azure: restringir o acesso a reposição de palavra-passe](authentication/howto-sspr-writeback.md) |


### <a name="steps"></a>Passos

| Passo | Recursos |
| --- | --- |
| Navegue até ao Portal de gestão do Azure AD: reposição de palavra-passe | [Portal de gestão do Azure AD: Reposição de palavra-passe](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset) |
| Determine a que política de reposição de palavra-passe. Para fins de prova de conceito, pode usar chamadas telefónicas e perguntas e respostas. É recomendado para ativar o registo a ser necessário no início de sessão para o painel de acesso |  |
| Terminar sessão e iniciar sessão como um operador de informações |  |
| Fornecer os dados de reposição de palavra-passe Self-Service, uma vez configurados para cada passo 2 | https://aka.ms/ssprsetup |
| Feche o browser |  |
| Começar o processo de início de sessão como o operador de informações que utilizou no passo 4 |  |
| Repor a palavra-passe | [Atualizar a sua própria palavra-passe: repor a palavra-passe](active-directory-passwords-update-your-own-password.md) |
| Tente iniciar sessão com a sua nova palavra-passe para o Azure AD, bem como para recursos no local |  |

### <a name="considerations"></a>Considerações

1. Se atualizar o Azure AD Connect vai fazer com que o efeito de fricção, em seguida, considere usá-lo em contas da cloud ou torná-lo uma demonstração em relação a um ambiente separado
2. Os administradores têm uma política diferente e utilizar a conta de administrador para repor a palavra-passe poderá taint a prova de conceito e causar confusão. Certifique-se de que utilizar uma conta de utilizador normal para testar as operações de reposição


## <a name="azure-multi-factor-authentication-with-phone-calls"></a>Multi-factor Authentication com chamadas telefónicas

Obter uma estimativa de tempo para concluir: 10 minutos

### <a name="pre-requisites"></a>Pré-requisitos

| Pré-requisito | Recursos |
| --- | --- |
| Identificar os utilizadores de prova de conceito que irão utilizar o MFA  |  |
| Telefone com boa receção para submissão da MFA  | [O que é o Multi-Factor Authentication do Azure?](authentication/multi-factor-authentication.md) |

### <a name="steps"></a>Passos

| Passo | Recursos |
| --- | --- |
| Navegue para o painel "Utilizadores e grupos" no Portal de gestão do Azure AD | [Portal de gestão do Azure AD: Os utilizadores e grupos](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/Overview/menuId/) |
| Escolha o painel de "Todos os utilizadores" |  |
| Na parte superior da barra botão de "Multi-factor Authentication" Escolher | Direcione o URL para o portal do Azure MFA: https://aka.ms/mfaportal |
| Nas definições de "Usuário" selecionar os utilizadores de prova de conceito e ativá-los para a MFA | [Estados do Utilizador no Multi-Factor Authentication do Azure](authentication/howto-mfa-userstates.md) |
| Inicie sessão como utilizador de prova de conceito e percorra o processo de verificação de segurança  |  |

### <a name="considerations"></a>Considerações

1. Os passos de prova de conceito neste bloco de construção configurando explicitamente o MFA para um utilizador em todos os inícios de sessão. Existem outras ferramentas, como o acesso condicional e a proteção de identidade que interaja com a MFA em mais cenários de destino. Isso será algo a ter em consideração quando mudar de prova de conceito para produção.
2. Os passos de prova de conceito neste bloco de construção são explicitamente usando chamadas telefónicas como o método MFA para se. À medida que faça a transição da prova de conceito para produção, recomendamos a utilização aplicações como o [Microsoft Authenticator](authentication/end-user/current/microsoft-authenticator-app-how-to.md) como seu segundo fator sempre que possível.
Saiba mais: [publicação especial do NIST rascunho 800 63B](https://pages.nist.gov/800-63-3/sp800-63b.html)

## <a name="mfa-conditional-access-for-saas-applications"></a>Acesso condicional do MFA para aplicações SaaS

Obter uma estimativa de tempo para concluir: 10 minutos

### <a name="pre-requisites"></a>Pré-requisitos

| Pré-requisito | Recursos |
| --- | --- |
| Identifique os utilizadores de prova de conceito para direcionar a política. Estes utilizadores devem ser num grupo de segurança para definir o âmbito da política de acesso condicional | [Configuração de SSO Federado de SaaS](#saas-federated-sso-configuration) |
| Aplicação SaaS tiver sido já configurada |  |
| Os utilizadores de prova de conceito já estiverem atribuídos à aplicação |  |
| As credenciais para o utilizador prova de conceito estão disponíveis |  |
| Utilizador de prova de conceito está registado para MFA. Usando um telefone com boa receção | https://aka.ms/ssprsetup |
| Dispositivo na rede interna. Endereço IP configurado no intervalo de endereços internos | Localize o seu endereço ip: https://www.bing.com/search?q=what%27s+my+ip |
| Dispositivo na rede externa (pode ser um telefone com a rede da operadora de móvel) |  |

### <a name="steps"></a>Passos

| Passo | Recursos |
| --- | --- |
| Aceda ao Portal de gestão do Azure AD: painel de acesso condicional | [Portal de gestão do Azure AD: Acesso condicional](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) |
| Crie política de acesso condicional:<br/>-Os utilizadores de prova de conceito do destino em "Utilizadores e grupos"<br/>-Aplicação de prova de conceito destino em "Aplicações de Cloud"<br/>-Direcionar todos os locais exceto aqueles fidedignas em "Condições" -> "Localizações" **Nota:** IPs fidedignos são configurados no [Portal de MFA](https://account.activedirectory.windowsazure.com/UserManagement/MfaSettings.aspx)<br/>-Exigir autenticação multifator em "Concessão" | [Introdução ao acesso condicional no Azure Active Directory: passos de configuração de política](active-directory-conditional-access-azure-portal-get-started.md#policy-configuration-steps) |
| Acesso à aplicação de dentro da rede empresarial | [Introdução ao acesso condicional no Azure Active Directory: a política de teste](active-directory-conditional-access-azure-portal-get-started.md#testing-the-policy) |
| Aplicação de acesso de rede pública | [Introdução ao acesso condicional no Azure Active Directory: a política de teste](active-directory-conditional-access-azure-portal-get-started.md#testing-the-policy) |

### <a name="considerations"></a>Considerações

Se estiver a utilizar o Federação, pode utilizar o fornecedor de identidade (IdP) no local para comunicar o estado de rede da empresa explorando/externa com declarações. Pode usar essa técnica sem ter de gerir a lista de endereços IP que pode ser complexo para avaliar e gerir em grandes organizações. Em que a instalação, precisa de conta para o cenário "roaming de rede" (um utilizador iniciar sessão a partir da rede interna e ao comutadores com sessão iniciada localizações como num café) e certifique-se de que compreende as implicações. Saiba mais: [proteger recursos da cloud com o Azure multi-factor Authentication e o AD FS: IPs fidedignos para utilizadores federados](authentication/howto-mfa-adfs.md#trusted-ips-for-federated-users)

## <a name="privileged-identity-management-pim"></a>Privileged Identity Management (PIM)

Obter uma estimativa de tempo para concluir: 15 minutos

### <a name="pre-requisites"></a>Pré-requisitos

| Pré-requisito | Recursos |
| --- | --- |
| Identificar o administrador global que vai fazer parte da prova de conceito da PIM | [Começar a utilizar o Azure AD Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md) |
| Identificar o administrador global que irá tornar-se o administrador de segurança | [Começar a utilizar o Azure AD Privileged Identity Management](active-directory-privileged-identity-management-getting-started.md)<br/> [Diferentes funções administrativas no Azure Active Directory PIM](active-directory-privileged-identity-management-roles.md) |
| Opcional: Confirme se os administradores globais têm acesso ao e-mail para notificações por e-mail no PIM | [O que é o Azure AD Privileged Identity Management?: configurar as definições de ativação de função](active-directory-privileged-identity-management-configure.md#configure-the-role-activation-settings)


### <a name="steps"></a>Passos

| Passo | Recursos |
| --- | --- |
| Início de sessão para https://portal.azure.com como um administrador global (GA) e o painel PIM de arranque. O Administrador Global, que executa este passo é implantado como o administrador de segurança.  Vamos chamar esta GA1 de ator | [Utilizar o Assistente de segurança no Azure AD Privileged Identity Management](active-directory-privileged-identity-management-security-wizard.md) |
| Identifique o administrador global e movê-los de permanente para elegíveis. Deve ser um administrador separada da utilizada no passo 1 para efeitos de clareza. Vamos chamar esta GA2 de ator | [O Azure AD Privileged Identity Management: Como adicionar ou remover uma função de utilizador](active-directory-privileged-identity-management-how-to-add-role-to-user.md)<br/>[O que é o Azure AD Privileged Identity Management?: configurar as definições de ativação de função](active-directory-privileged-identity-management-configure.md#configure-the-role-activation-settings)  |
| Agora, inicie sessão como GA2 para https://portal.azure.com e tente alterar o "User Settings". Observe que algumas opções são bloqueadas. | |
| Num novo separador e na mesma sessão como o passo 3, navegue agora até https://portal.azure.com e adicionar o painel PIM ao dashboard. | [Como ativar ou desativar funções no Azure AD Privileged Identity Management: adicionar a aplicação de Privileged Identity Management](active-directory-privileged-identity-management-how-to-activate-role.md#add-the-privileged-identity-management-application) |
| Pedir ativação à função de Administrador Global | [Como ativar ou desativar funções no Azure AD Privileged Identity Management: ativar uma função](active-directory-privileged-identity-management-how-to-activate-role.md#activate-a-role) |
| Tenha em atenção que se GA2 nunca tiver inscrito para a MFA, registo do MFA do Azure será necessário |  |
| Regresse ao separador original no passo 3 e clique no botão Atualizar no browser. Tenha em atenção que agora tem acesso para alterar o "User settings" | |
| Opcionalmente, se os administradores globais têm o e-mail ativado, pode verificar GA1 e da GA2 caixa de entrada e ver a notificação da função a ser ativada |  |
| 8 Verifique o histórico de auditoria e observe o relatório para confirmar a elevação de GA2 é apresentado. | [O que é o Azure AD Privileged Identity Management?: função actividade de revisão](active-directory-privileged-identity-management-configure.md#review-role-activity) |

### <a name="considerations"></a>Considerações

Esta capacidade é parte do Azure AD Premium P2 e/ou EMS E5

## <a name="discovering-risk-events"></a>Detetar eventos de risco

Obter uma estimativa de tempo para concluir: 20 minutos

### <a name="pre-requisites"></a>Pré-requisitos

| Pré-requisito | Recursos |
| --- | --- |
| Dispositivo com browser de Tor baixado e instalado | [Baixe o navegador de Tor](https://www.torproject.org/projects/torbrowser.html.en#downloads) |
| Acesso ao utilizador prova de conceito para fazer o início de sessão | [Playbook do Azure Active Directory Identity Protection](active-directory-identityprotection-playbook.md) |

### <a name="steps"></a>Passos

| Passo | Recursos |
| --- | --- |
| Browser de tor aberto | [Baixe o navegador de Tor](https://www.torproject.org/projects/torbrowser.html.en#downloads) |
| Inicie sessão no https://myapps.microsoft.com com a conta de utilizador de prova de conceito | [Playbook do Azure Active Directory Identity Protection: simulação de eventos de risco](active-directory-identityprotection-playbook.md#simulating-risk-events) |
| Aguarde 5 a 7 minutos |  |
| Inicie sessão como um administrador global para https://portal.azure.com e abra o painel de proteção de identidade | https://aka.ms/aadipgetstarted |
| Abra o painel de eventos de risco. Deverá ver uma entrada sob "Inícios de sessão de endereços IP anónimos"  | [Playbook do Azure Active Directory Identity Protection: simulação de eventos de risco](active-directory-identityprotection-playbook.md#simulating-risk-events) |

### <a name="considerations"></a>Considerações

Esta capacidade é parte do Azure AD Premium P2 e/ou EMS E5

## <a name="deploying-sign-in-risk-policies"></a>Implementar políticas de início de sessão risco

Obter uma estimativa de tempo para concluir: 10 minutos

### <a name="pre-requisites"></a>Pré-requisitos

| Pré-requisito | Recursos |
| --- | --- |
| Dispositivo com browser de Tor baixado e instalado | [Baixe o navegador de Tor](https://www.torproject.org/projects/torbrowser.html.en#downloads) |
| Acesso como um utilizador de prova de conceito para fazer o teste de início de sessão |  |
| Utilizador de prova de conceito está registado com a MFA. Certifique-se de usar um telefone com boa receção | Bloco de construção: [multi-factor Authentication com chamadas telefónicas](#azure-multi-factor-authentication-with-phone-calls) |


### <a name="steps"></a>Passos

| Passo | Recursos |
| --- | --- |
| Inicie sessão como um administrador global para https://portal.azure.com e abrir o painel de proteção de identidade | https://aka.ms/aadipgetstarted |
| Ative uma política de risco de início de sessão da seguinte forma:<br/>-Atribuído a: o utilizador de prova de conceito<br/>-Condições: Início de sessão risco médio ou superior (inícios de sessão de localização anônimo é considerado como um nível de risco médio)<br/>-Controles: Exigir a MFA | [Playbook do Azure Active Directory Identity Protection: início de sessão de risco](active-directory-identityprotection-playbook.md) |
| Browser de tor aberto | [Baixe o navegador de Tor](https://www.torproject.org/projects/torbrowser.html.en#downloads) |
| Inicie sessão no https://myapps.microsoft.com com a conta de utilizador de prova de conceito |  |
| Tenha em atenção a submissão da MFA | [Experiências de início de sessão com o Azure AD Identity Protection: recuperação de início de sessão arriscada](active-directory-identityprotection-flows.md#risky-sign-in-recovery)

### <a name="considerations"></a>Considerações

Esta capacidade é parte do Azure AD Premium P2 e/ou EMS E5. Para saber mais sobre eventos de risco, visite: [eventos de risco do Azure Active Directory](active-directory-reporting-risk-events.md)

## <a name="configuring-certificate-based-authentication"></a>Configuração da autenticação baseada em certificado

Obter uma estimativa de tempo para concluir: 20 minutos

### <a name="pre-requisites"></a>Pré-requisitos

| Pré-requisito | Recursos |
| --- | --- |
| Dispositivo com o certificado de utilizador aprovisionado (Windows, iOS ou Android) da PKI de empresa | [Implementar certificados de utilizador](https://msdn.microsoft.com/library/cc770857.aspx) |
| Domínio do Azure AD federadas com o AD FS | [Azure AD Connect e a federação](./connect/active-directory-aadconnectfed-whatis.md)<br/>[Descrição geral de serviços de certificados do Active Directory](https://technet.microsoft.com/library/hh831740.aspx)|
| Para dispositivos iOS têm a aplicação Microsoft Authenticator instalada | [Comece com a aplicação Microsoft Authenticator](authentication/end-user/current/microsoft-authenticator-app-how-to.md) |

### <a name="steps"></a>Passos

| Passo | Recursos |
| --- | --- |
| Ativar a "Autenticação de certificado" no ADFS | [Configurar políticas de autenticação: Para configurar a autenticação primária globalmente no Windows Server 2012 R2](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configure-authentication-policies#to-configure-primary-authentication-globally-in-windows-server-2012-r2) |
| Opcional: Ative a autenticação de certificados no Azure AD para clientes do Exchange Active Sync | [Introdução à autenticação baseada em certificado no Azure Active Directory](active-directory-certificate-based-authentication-get-started.md) |
| Navegue até ao painel de acesso e autenticação utilizando o certificado de utilizador | https://myapps.microsoft.com |

### <a name="considerations"></a>Considerações

Para saber mais sobre limitações desta implementação, visite: [ADFS: autenticação de certificados com o Azure AD & do Office 365](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/)



> [!NOTE]
> Posse do certificado de utilizador deve ser protegido. A gestão de dispositivos ou com o PIN em caso de cartões inteligentes.



[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]
