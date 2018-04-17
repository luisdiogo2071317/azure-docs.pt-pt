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
ms.openlocfilehash: 26ba68be6d4932da77befaf7c968525393c0a033
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="enable-a-cloud-service-provider-to-manage-your-azure-stack-subscription"></a>Ativar um fornecedor de serviços em nuvem gerir a sua subscrição de pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas*

Se estiver a utilizar pilha do Azure com um fornecedor de serviços em nuvem (CSP), o acesso aos recursos na sua subscrição do Azure e na pilha do Azure pode ser gerido pelo fornecedor. Ou pode gerir a sua própria subscrição. Este artigo observa como ativar o fornecedor de serviços para aceder à sua subscrição em seu nome, ou para se certificar de que o fornecedor de serviços pode gerir o seu serviço.

> [!Note]  
>  Se os seguintes passos são ignorados e o CSP já não está a gerir a conta, em seguida, o CSP não será capaz de gerir a sua subscrição do Azure pilha em seu nome.

## <a name="manage-your-subscription-with-a-cloud-service-provider"></a>Gerir a sua subscrição com um fornecedor de serviços Cloud

1. Adicione o seu CSP como utilizador convidado com a função de utilizador para o seu diretório do inquilino.  Para obter os passos sobre como adicionar um utilizador, consulte [adicionar novos utilizadores ao Azure Active Directory](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory)
2. O CSP, em seguida, irá criar a subscrição do Azure pilha local para si.
3. Está pronto para começar a utilizar a pilha do Azure.
3. O CSP, em seguida, deve criar um recurso na sua subscrição para verificar que também podem gerir os seus recursos. Por exemplo, pode [criar máquina virtual do Windows com o portal do Azure pilha](azure-stack-quick-windows-portal.md).

## <a name="enable-the-cloud-service-provider-to-manage-your-subscription-using-rbac-rights"></a>Ativar o fornecedor de serviço em nuvem gerir a sua subscrição com direitos RBAC

Adicione o CSP como proprietário à sua subscrição. 

1. Adicione o seu CSP como utilizador convidado. com a função de proprietário para o seu diretório do inquilino.  Para obter os passos sobre como adicionar um utilizador, consulte [adicionar novos utilizadores ao Azure Active Directory](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory)
2. Adicione função de proprietário para o utilizador de convidado do CSP. Para obter os passos sobre como adicionar o utilizador do CSP à sua subscrição, consulte [Use Role-Based o controlo de acesso para gerir o acesso aos recursos da sua subscrição do Azure](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)
3. O CSP, em seguida, irá criar a subscrição do Azure pilha local para si.
4. Está pronto para começar a utilizar a pilha do Azure.
5. O CSP, em seguida, deve criar um recurso na sua subscrição para verificar que podem gerir os recursos. 

## <a name="next-steps"></a>Passos Seguintes

  - Para obter mais informações sobre como obter as informações de utilização de recursos de pilha do Azure, consulte o artigo [utilização e faturação na pilha de Azure](../azure-stack-billing-and-chargeback.md).
