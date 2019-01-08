---
title: Licenciamento do Premium P2 do Azure Active Directory - lista de verificação da funcionalidade | Documentos da Microsoft
description: Lista de verificação de implementação para implantação de recursos do Azure Active Directory Premium P2 em 30 dias, 90 dias e muito mais.
services: active-directory
ms.service: active-directory
ms.component: ''
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.custom: it-pro, seodec18
ms.openlocfilehash: 4fcb692d7189c84e32f55995538ffc692cb67dd6
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/07/2019
ms.locfileid: "54064717"
---
# <a name="azure-active-directory-premium-p2-licensing-feature-checklist"></a>O Azure Active Directory Premium P2 licenciamento funcionalidade lista de verificação

Ele pode parecer assustadora para implementar o Azure Active Directory (Azure AD) para a sua organização e mantê-lo seguro. Este artigo identifica algumas tarefas comuns que os clientes ser úteis. Os clientes normalmente concluir estas tarefas no decorrer de 30 dias, 90 dias, ou for além para melhorar a sua postura de segurança. Até mesmo organizações que já implementaram do Azure AD podem utilizar esta lista de verificação para certificar-se de que eles esteja tirando o máximo de seus investimentos em.

Uma infraestrutura de identidade bem planejada e executada abre o caminho para um acesso mais seguro às suas cargas de trabalho de produtividade e os dados apenas por dispositivos e utilizadores autenticados.

## <a name="prerequisites"></a>Pré-requisitos

Este guia pressupõe que tem licenças do Azure AD Premium P2, Enterprise Mobility + Security E5, Microsoft 365 E5 ou um pacote de licença semelhante.

[Licenciamento Azure AD](https://azure.microsoft.com/pricing/details/active-directory/)

[Microsoft 365 Enterprise](https://www.microsoft.com/en-us/licensing/product-licensing/microsoft-365-enterprise)

[Enterprise Mobility + Security](https://www.microsoft.com/en-us/licensing/product-licensing/enterprise-mobility-security)

## <a name="plan-and-deploy-day-1-30"></a>Planear e implementar: Dia 1 e 30

- Designar mais de um administrador global (break glass conta)
   - [Gerir o acesso de emergência de contas administrativas no Azure AD](../users-groups-roles/directory-emergency-access.md)
- Ativar o Azure AD Privileged Identity Management (PIM) para ver relatórios
   - [Comece a utilizar o PIM](../privileged-identity-management/pim-getting-started.md)
- Sempre que possível, utilize as funções administrativas não global.
   - [Atribuir funções de administrador no Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md)
- Autenticação
   - [Implementar a reposição personalizada de palavra-passe](../authentication/howto-sspr-deployment.md)
   - Implementar a proteção de palavra-passe do Azure AD (pré-visualização)
      - [Eliminar palavras-passe incorretas na sua organização](../authentication/concept-password-ban-bad.md)
      - [Impor proteção de palavra-passe do Azure AD para o Windows Server Active Directory](../authentication/concept-password-ban-bad-on-premises.md)
   - Configurar políticas de bloqueio de conta
      - [Bloqueio inteligente de Active Directory do Azure](../authentication/howto-password-smart-lockout.md)
      - [AD FS bloqueio de Extranet e bloqueio de Extranet inteligente](/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-smart-lockout-protection)
   - [Implementar a autenticação multi-factor do Azure AD, através de políticas de acesso condicional](../authentication/howto-mfa-getstarted.md)
   - [Ativar o registo convergido para reposição de palavra-passe self-service e do Azure AD multi-factor Authentication (pré-visualização)](../authentication/concept-registration-mfa-sspr-converged.md)
   - [Ativar o Azure Active Directory Identity Protection](../identity-protection/enable.md)
      - [Usar eventos de risco para o acionador multi-factor Authentication e alterações de palavra-passe](../authentication/tutorial-risk-based-sspr-mfa.md)
   - [Orientações de palavra-passe](https://www.microsoft.com/research/publication/password-guidance/)
      - Manter um requisito de comprimento mínimo de oito caracteres, já não é necessariamente a melhor.
      - Elimine requisitos de composição de caracteres.
      - [Elimine redefinições de senha periódicas obrigatório para contas de utilizador.](../authentication/concept-sspr-policy.md#set-a-password-to-never-expire)
- Sincronizar utilizadores do Active Directory no local
   - [Instalar o Azure AD Connect](../connect/active-directory-aadconnect-select-installation.md)
   - [Implementar a sincronização de Hash de palavra-passe](../connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md)
   - [Implementar a repetição de escrita de palavra-passe](../authentication/howto-sspr-writeback.md)
   - [Implementar o Azure AD Connect Health](../connect-health/active-directory-aadconnect-health.md)
- [Atribuir licenças aos utilizadores pela associação de grupo no Azure Active Directory](../users-groups-roles/licensing-groups-assign.md)

## <a name="plan-and-deploy-day-31-90"></a>Planear e implementar: Dia 31 a 90

- [Plano para acesso do utilizador convidado](../b2b/what-is-b2b.md)
   - [Adicionar utilizadores de colaboração do Azure Active Directory B2B no portal do Azure](../b2b/add-users-administrator.md)
   - [Permitir ou bloquear convites para utilizadores B2B de organizações específicos](../b2b/allow-deny-list.md)
   - [Os utilizadores de concessão B2B no Azure AD acedem às suas aplicações no local](../b2b/hybrid-cloud-to-on-premises.md)
- Tomar decisões sobre a estratégia de gerenciamento de ciclo de vida do utilizador
- [Decidir qual a estratégia de gestão de dispositivos](../devices/overview.md)
   - [Cenários de utilização e considerações de implementação para associação do Azure AD](../devices/azureadjoin-plan.md)
- [Gerir o Windows Hello para empresas na sua organização](/windows/security/identity-protection/hello-for-business/hello-manage-in-organization)

## <a name="plan-and-deploy-day-90-and-beyond"></a>Planear e implementar: 90 dias e muito mais

- [Azure AD Privileged Identity Management](../privileged-identity-management/pim-configure.md)
   - [Configurar definições de função de diretório do Azure AD no PIM](../privileged-identity-management/pim-how-to-change-default-settings.md)
   - [Atribuir funções de diretório do Azure AD no PIM](../privileged-identity-management/pim-how-to-add-role-to-user.md)
- [Concluir uma revisão de acesso para funções de diretório do Azure AD no PIM](../privileged-identity-management/pim-how-to-start-security-review.md)
- Gerir o ciclo de vida do utilizador holística
   - O Azure AD tem uma abordagem para gerenciar o ciclo de vida de identidade
   - Remova passos manuais do seu ciclo de vida de conta do funcionário, para evitar acessos não autorizados:
      - Sincronize as identidades da sua origem de verdade (sistema de RH) para o Azure AD.
      - [Utilize grupos dinâmicos para atribuir automaticamente os utilizadores a grupos com base nos seus atributos de RH (ou sua fonte de verdade), por exemplo, departamento, título, região e outros atributos.](../users-groups-roles/groups-dynamic-membership.md)
      - [Utilize o aprovisionamento da gestão de acesso baseado em grupo para aprovisionar automaticamente os utilizadores para aplicações SaaS.](../manage-apps/what-is-access-management.md)

## <a name="next-steps"></a>Passos Seguintes

[Configurações de acesso de dispositivos e identidade](https://docs.microsoft.com/microsoft-365/enterprise/microsoft-365-policies-configurations)

[Comum recomendado políticas de acesso de dispositivos e identidade](https://docs.microsoft.com/microsoft-365/enterprise/identity-access-policies)
