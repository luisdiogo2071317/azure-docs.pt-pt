---
title: Ativar um fornecedor de serviços em nuvem gerir a sua subscrição do Azure pilha | Microsoft Docs
description: Ative o fornecedor de serviços aceder a uma subscrição na pilha do Azure.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2018
ms.author: mabrigg
ms.reviewer: alfredop
ms.openlocfilehash: f0cff8f575b87872c0032854f1916b140d7fd62b
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
# <a name="enable-a-cloud-service-provider-to-manage-your-azure-stack-subscription"></a>Ativar um fornecedor de serviços em nuvem gerir a sua subscrição de pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas*

Se estiver a utilizar pilha do Azure com um fornecedor de serviços em nuvem (CSP), pode optar por gerir a sua própria subscrição para aceder a recursos no Azure e na pilha do Azure. Também pode permitir que o fornecedor de gerir a sua subscrição para si. Este artigo mostra como para:

 * Conceda o acesso do fornecedor de serviço a sua subscrição.
 * Certifique-se de que o fornecedor de serviços pode gerir o seu serviço.

> [!Note]
>  Se o CSP não está a gerir a conta, e se ignorar os passos seguintes, o CSP não é possível gerir a sua subscrição de pilha do Azure por si.

## <a name="manage-your-subscription-with-a-cloud-service-provider"></a>Gerir a sua subscrição com um fornecedor de serviços Cloud

Adicionar o CSP como **utilizador** à sua subscrição.

1. Adicione o seu CSP como utilizador convidado com a função de utilizador para o seu diretório do inquilino.  Para obter os passos sobre como adicionar um utilizador, consulte [adicionar novos utilizadores ao Azure Active Directory](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory)
2. O CSP cria a subscrição do Azure pilha local para si.
3. Está pronto para começar a utilizar a pilha do Azure.
4. O CSP deve criar um recurso na sua subscrição para verificar que também podem gerir os seus recursos. Por exemplo, pode [criar máquina virtual do Windows com o portal do Azure pilha](azure-stack-quick-windows-portal.md).

## <a name="enable-the-cloud-service-provider-to-manage-your-subscription-using-rbac-rights"></a>Ativar o fornecedor de serviço em nuvem gerir a sua subscrição com direitos RBAC

Adicionar o CSP como **proprietário** à sua subscrição.

1. Adicione o seu CSP como utilizador convidado para o seu diretório do inquilino.  Para obter os passos sobre como adicionar um utilizador, consulte [adicionar novos utilizadores ao Azure Active Directory](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory)
2. Adicione a função de proprietário para o utilizador de convidado do CSP. Para obter os passos sobre como adicionar o utilizador do CSP à sua subscrição, consulte [Use Role-Based o controlo de acesso para gerir o acesso aos recursos da sua subscrição do Azure](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)
3. O CSP cria a subscrição do Azure pilha local para si.
4. Está pronto para começar a utilizar a pilha do Azure.
5. O CSP deve criar um recurso na sua subscrição para verificar que podem gerir os recursos.

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre como obter as informações de utilização de recursos de pilha do Azure, consulte o artigo [utilização e faturação na pilha de Azure](../azure-stack-billing-and-chargeback.md).
