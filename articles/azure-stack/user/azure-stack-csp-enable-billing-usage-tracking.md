---
title: Ativar fornecedor de serviços Cloud gerir a sua subscrição do Azure Stack | Documentos da Microsoft
description: Ative o fornecedor de serviço para aceder a uma subscrição no Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2018
ms.author: sethm
ms.reviewer: alfredop
ms.openlocfilehash: fca08ef1d803c3bd47b0ae925c4dd12255175f2c
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/15/2018
ms.locfileid: "45630720"
---
# <a name="enable-a-cloud-service-provider-to-manage-your-azure-stack-subscription"></a>Ativar fornecedor de serviços Cloud gerir a sua subscrição do Azure Stack

*Aplica-se a: sistemas integrados do Azure Stack*

Se estiver a utilizar do Azure Stack com um fornecedor de serviços Cloud (CSP), pode optar por gerir a sua própria subscrição para aceder aos recursos no Azure e no Azure Stack. Também pode permitir que o fornecedor de gerir a sua subscrição para si. Este artigo mostra-lhe como para:

 * Conceda o acesso ao fornecedor de serviço a sua subscrição.
 * Certifique-se de que o fornecedor de serviços pode gerir o seu serviço.

> [!Note]
>  Se o CSP não está a gerir a conta e ignorar as etapas a seguir, o CSP não pode gerir a sua subscrição do Azure Stack para.

## <a name="manage-your-subscription-with-a-cloud-service-provider"></a>Gerir a sua subscrição com o fornecedor de serviços Cloud

Adicionar o CSP como **utilizador** à sua subscrição.

1. Adicione seu CSP como utilizador convidado com a função de utilizador ao diretório do inquilino.  Para obter passos sobre como adicionar um utilizador, consulte [adicionar novos utilizadores ao Azure Active Directory](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory)
2. O CSP cria a subscrição do Azure Stack local para.
3. Está pronto para começar a utilizar o Azure Stack.
4. O CSP deve criar um recurso na sua subscrição para verificar que também podem gerir os seus recursos. Por exemplo, pode [criar uma máquina virtual do Windows com o portal do Azure Stack](azure-stack-quick-windows-portal.md).

## <a name="enable-the-cloud-service-provider-to-manage-your-subscription-using-rbac-rights"></a>Ativar o fornecedor de serviços Cloud gerir a sua subscrição com direitos RBAC

Adicionar o CSP como **proprietário** à sua subscrição.

1. Adicione seu CSP como utilizador convidado ao diretório do inquilino.  Para obter passos sobre como adicionar um utilizador, consulte [adicionar novos utilizadores ao Azure Active Directory](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory)
2. Adicione a função de proprietário para o utilizador convidado CSP. Para obter passos sobre como adicionar o utilizador CSP à sua subscrição, veja [Use Role-Based o controlo de acesso para gerir o acesso aos recursos da sua subscrição do Azure](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)
3. O CSP cria a subscrição do Azure Stack local para.
4. Está pronto para começar a utilizar o Azure Stack.
5. O CSP deve criar um recurso na sua subscrição para verificar que pode gerir os recursos.

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre como recuperar informações de utilização de recursos do Azure Stack, veja [utilização e faturação no Azure Stack](../azure-stack-billing-and-chargeback.md).
