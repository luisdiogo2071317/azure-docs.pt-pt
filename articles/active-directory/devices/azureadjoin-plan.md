---
title: Cenários de utilização e considerações de implementação para associação do Azure AD | Documentos da Microsoft
description: Explica como os administradores podem configurar associação do Azure AD para os utilizadores finais (funcionários, estudantes, outros utilizadores). Ele também aborda os diferentes cenários de mundo real para utilizar a associação do Azure AD.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
tags: azure-classic-portal
ms.component: devices
ms.assetid: 81d4461e-21c8-4fdd-9076-0e4991979f62
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2018
ms.author: markvi
ms.openlocfilehash: a145b4184fbebd9c4f447face1c47bec20c0ec32
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39414890"
---
# <a name="usage-scenarios-and-deployment-considerations-for-azure-ad-join"></a>Cenários de utilização e considerações de implementação para associação do Azure AD
## <a name="usage-scenarios-for-azure-ad-join"></a>Cenários de utilização de associação do Azure AD
### <a name="scenario-1-businesses-largely-in-the-cloud"></a>Cenário 1: Empresas em grande parte na cloud
O Azure Active Directory Join (associação do Azure AD) podem beneficiá-lo se atualmente operar e gerir identidades para o seu negócio na cloud ou estiver a mover para a cloud em breve. Pode utilizar uma conta que tenha criado no Azure AD para iniciar sessão no Windows 10. Por meio [processo (FRX) de experiência de primeira execução](azuread-joined-devices-frx.md), ou por associação do Azure AD a partir de [o menu de definições](../user-help/device-management-azuread-joined-devices-setup.md), os utilizadores podem associar seus computadores ao Azure AD.  Os utilizadores também podem desfrutar único início de sessão (SSO) acesso a recursos na cloud, como o Office 365, em seus navegadores ou em aplicativos do Office.

### <a name="scenario-2-educational-institutions"></a>Cenário 2: Instituições de ensino
Instituições de ensino normalmente têm dois tipos de utilizador: corpo docente e estudantes. Membros do corpo docente são considerados membros de longo prazo da organização. A criação de contas no local para os mesmos é desejável. Mas os estudantes são shorter-term membros da organização e das respetivas contas podem ser geridas no Azure AD. Isso significa que o dimensionamento de diretório pode ser enviado para a cloud em vez de serem armazenados no local. Também significa que os alunos serão capazes de iniciar sessão no Windows com as suas contas do Azure AD e obter acesso aos recursos do Office 365 em aplicativos do Office.

### <a name="scenario-3-retail-businesses"></a>Cenário 3: As empresas de varejo
As empresas de varejo têm trabalhadores sazonais e funcionários de longo prazo. Normalmente, criar contas no local e utilizar máquinas associadas a domínios para os funcionários a tempo inteiro longo prazo. Mas trabalhadores sazonais são shorter-term membros da organização e é desejável para gerenciar essas contas em que licenças de utilizador podem ser mais facilmente movidas. Quando cria as contas de utilizador na cloud com licenças do Office 365, estes utilizadores obtém os benefícios do início de sessão para Windows e Office aplicativos com uma conta do Azure AD, enquanto mantém o maior flexibilidade com suas licenças após eles os deixam.

### <a name="scenario-4-additional-scenarios"></a>Cenário de 4: Cenários adicionais
Assim como os benefícios discutidos anteriormente, se beneficiar da que os seus utilizadores se associem os seus dispositivos com o Azure AD devido a uma experiência simplificada de junção, gestão de dispositivos eficiente, inscrição da gestão de dispositivos móveis automática e início de sessão único para o Azure AD e recursos no local.  

## <a name="deployment-considerations-for-azure-ad-join"></a>Considerações de implementação para associação do Azure AD
### <a name="enable-your-users-to-join-a-company-owned-device-directly-to-azure-ad"></a>Permitir que os utilizadores associar um dispositivo da empresa diretamente ao Azure AD
As empresas podem fornecer contas apenas na cloud para empresas associadas e as organizações. Estes parceiros, em seguida, podem aceder facilmente aplicações da empresa e de recursos com o início de sessão único. Este cenário se aplica a utilizadores que acedem a recursos principalmente na cloud, como as aplicações do Office 365 ou SaaS que dependem do Azure AD para autenticação.

### <a name="prerequisites"></a>Pré-requisitos
**No nível empresarial (administrador)**

* Subscrição do Azure com o Azure Active Directory  

**No nível do usuário**

* Windows 10 (edições Professional e Enterprise)

### <a name="administrator-tasks"></a>Tarefas de administrador
* [Configurar o registo do dispositivo](device-management-azure-portal.md)

### <a name="user-tasks"></a>Tarefas de utilizador
* [Configurar um novo dispositivo Windows 10 com o Azure AD durante a configuração](azuread-joined-devices-frx.md)
* [Configurar um dispositivo Windows 10 com o Azure AD no menu Definições](../user-help/device-management-azuread-registered-devices-windows10-setup.md)
* [Junte-se um dispositivo pessoal do Windows 10 para a sua organização](../user-help/device-management-azuread-joined-devices-setup.md)

## <a name="enable-byod-in-your-organization-for-windows-10"></a>Ativar o BYOD na sua organização para o Windows 10
Pode configurar seus funcionários e de utilizadores para utilizar os seus dispositivos Windows pessoais (BYOD) para aceder a aplicações da empresa e os recursos. Os utilizadores podem adicionar as suas contas do Azure AD (contas profissionais ou escolares) para um dispositivo pessoal do Windows para aceder aos recursos de forma segura e em conformidade.

### <a name="prerequisites"></a>Pré-requisitos
**No nível empresarial (administrador)**

* Subscrição do Azure AD

**No nível do usuário**

* Windows 10 (edições Professional e Enterprise)

### <a name="administrator-tasks"></a>Tarefas de administrador
* [Configurar o registo do dispositivo](device-management-azure-portal.md)

### <a name="user-tasks"></a>Tarefas de utilizador
* [Junte-se um dispositivo pessoal do Windows 10 para a sua organização](../user-help/device-management-azuread-joined-devices-setup.md)

## <a name="next-steps"></a>Passos Seguintes

- [Gestão de Dispositivos](overview.md)

