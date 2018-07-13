---
title: Utilizar grupos para gerir o acesso a recursos no Azure AD | Microsoft Docs
description: Como utilizar grupos no Azure Active Directory para gerir o acesso de utilizador a recursos e aplicações no local e na cloud.
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: overview
ms.date: 08/28/2017
ms.author: lizross
ms.custom: H1Hack27Feb2017;it-pro
ms.reviewer: piotrci
ms.openlocfilehash: cce3b4f15acbcc6ce78ea0623c6b9f3c23941cb0
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37859630"
---
# <a name="manage-access-to-resources-with-azure-active-directory-groups"></a>Gerir o acesso a recursos com grupos do Azure Active Directory
O Azure Active Directory (Azure AD) é uma solução de gestão de identidades e acessos abrangente que fornece um sólido conjunto de funções para gerir o acesso a recursos e aplicações no local e na cloud, incluindo serviços online da Microsoft como o Office 365 e uma panóplia de aplicações SaaS que não são da Microsoft. Este artigo fornece uma descrição geral, mas se quiser começar a utilizar grupos do Azure AD neste momento, siga as instruções em [Gerir grupos de segurança no Azure AD](active-directory-groups-create-azure-portal.md). Se quiser ver como pode utilizar o PowerShell para gerir grupos no Azure Active Directory, pode ler mais em [Cmdlets do Azure Active Directory para gestão de grupos](../users-groups-roles/groups-settings-v2-cmdlets.md).

> [!NOTE]
> Para utilizar o Azure Active Directory, precisa de uma conta do Azure. Se não tiver uma conta, pode [inscrever-se para obter uma conta do Azure gratuita](https://azure.microsoft.com/pricing/free-trial/).
>
>

No Azure AD, uma das principais funcionalidades é a capacidade de gerir o acesso aos recursos. Estes recursos podem fazer parte do diretório, como no caso de permissões para gerir objetos através de funções no diretório, ou dos recursos externos ao diretório, como aplicações SaaS, serviços do Azure e sites SharePoint ou recursos no local. Existem quatro formas de atribuir a um utilizador direitos de acesso a um recurso:

1. Atribuição direta

    Os utilizadores podem ser atribuídos diretamente a um recurso pelo proprietário do mesmo.
2. Associação a um grupo

    Um grupo pode ser atribuído a um recurso pelo proprietário do recurso e, ao fazê-lo, conceder aos membros desse grupo acesso ao recurso. Em seguida, a associação do grupo pode ser gerida pelo proprietário do grupo. Efetivamente, o proprietário do recurso delega a permissão de atribuir utilizadores ao seu recurso ao proprietário do grupo.
3. Baseado em regras

    O proprietário do recurso pode utilizar uma regra para expressar a que utilizadores deve ser atribuído o acesso a um recurso. O resultado da regra depende dos atributos utilizados nessa regra e dos respetivos valores para utilizadores específicos. Ao fazê-lo, o proprietário do recurso delega efetivamente o direito de gerir o acesso aos seus recursos à origem autoritativa dos atributos utilizados na regra. O proprietário do recurso continua a gerir a regra e determina que atributos e valores fornecem acesso ao seu recurso.
4. Autoridade externa

    O acesso a um recurso é derivado de uma origem externa; por exemplo, um grupo sincronizado a partir de uma origem autoritativa, como um diretório no local ou uma aplicação SaaS, como o WorkDay. O proprietário do recurso atribui o grupo para fornecer acesso ao recurso e a origem externa gere os membros do grupo.

   ![Descrição geral do diagrama de gestão de acesso](./media/active-directory-manage-groups/access-management-overview.png)

## <a name="watch-a-video-that-explains-access-management"></a>Ver um vídeo que explica a gestão de acesso
Pode ver um vídeo curto que explica mais sobre isso:

**Azure AD: introdução à associação dinâmica para grupos**

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-AD--Introduction-to-Dynamic-Memberships-for-Groups/player]
>
>

## <a name="how-does-access-management-in-azure-active-directory-work"></a>Como funciona a gestão de acesso no Azure Active Directory?
No centro da solução de gestão de acesso do Azure AD está o grupo de segurança. Utilizar um grupo de segurança para gerir o acesso a recursos é um paradigma bem conhecido, que permite ter uma forma flexível e fácil de compreender para fornecer acesso a um recurso para o grupo de utilizadores pretendido. O proprietário do recurso (ou o administrador do diretório), pode atribuir um grupo para fornecer um determinado direito de acesso aos recursos que possui. O acesso será fornecido aos membros do grupo e o proprietário do recurso pode delegar o direito de gerir a lista de membros de um grupo a outra pessoa, como um gestor de departamento ou um administrador de suporte técnico.

![Diagrama da gestão de acesso no Azure Active Directory](./media/active-directory-manage-groups/active-directory-access-management-works.png)

O proprietário de um grupo também pode tornar esse grupo disponível para pedidos personalizados. Ao fazê-lo, um utilizador final pode procurar e encontrar o grupo e efetuar um pedido de adesão, ao procurar eficazmente permissão para aceder aos recursos geridos através do grupo. O proprietário pode configurar o grupo para que os pedidos de adesão sejam aprovados automaticamente ou necessitem de aprovação do proprietário do grupo. Quando um utilizador efetua um pedido de adesão a um grupo, este é encaminhado para os proprietários do grupo. Se um dos proprietários aprovar o pedido, o utilizador requerente é notificado e associado ao grupo. Se um dos proprietários recusar o pedido, o utilizador requerente é notificado mas não é associado ao grupo.

## <a name="getting-started-with-access-management"></a>Introdução à gestão de acesso
Pronto para começar? Deve testar algumas das tarefas básicas que pode fazer com grupos do Azure AD. Utilize estas capacidades para fornecer acesso especializado a diferentes grupos de pessoas de diferentes recursos na sua organização. Segue-se uma lista dos primeiros passos básicos.

* [Criar uma regra simples para configurar associações dinâmicas para um grupo](active-directory-groups-create-azure-portal.md)
* [Utilizar um grupo para gerir o acesso a aplicações SaaS](../users-groups-roles/groups-saasapps.md)
* [Disponibilizar um grupo para personalização por utilizadores finais](../users-groups-roles/groups-self-service-management.md)
* [Sincronizar um grupo no local para o Azure com o Azure AD Connect](../connect/active-directory-aadconnect.md)
* [Gerir proprietários de um grupo](active-directory-accessmanagement-managing-group-owners.md)

## <a name="next-steps"></a>Passos seguintes
Agora que compreendeu as noções básicas da gestão de acesso, seguem-se algumas capacidades avançadas adicionais disponíveis no Azure Active Directory para gerir o acesso às suas aplicações e recursos.

* [Utilizar atributos para criar regras avançadas](../users-groups-roles/groups-dynamic-membership.md)
* [Gerir grupos de segurança no Azure AD](active-directory-groups-create-azure-portal.md)
* [Referência da Graph API para grupos](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#GroupFunctions)
* [Cmdlets do Azure Active Directory para configurar definições de grupo](../users-groups-roles/groups-settings-cmdlets.md)
