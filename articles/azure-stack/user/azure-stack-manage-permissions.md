---
title: Gerir permissões para recursos por utilizador na pilha do Azure | Microsoft Docs
description: Como um administrador de serviço ou o inquilino, saiba como gerir RBAC permissões.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: cccac19a-e1bf-4e36-8ac8-2228e8487646
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: 026c686b1d5654aa50dd63b9addd619dd5322da0
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/06/2018
ms.locfileid: "34808150"
---
# <a name="manage-access-to-resources-with-azure-stack-role-based-access-control"></a>Gerir o acesso a recursos com controlo de acesso de Azure Stack Role-Based

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Pilha do Azure suporta o controlo de acesso baseado em funções (RBAC), o mesmo [modelo de segurança para gestão de acesso](https://docs.microsoft.com/azure/role-based-access-control/overview) que utiliza o Microsoft Azure. Pode utilizar o RBAC para gerir o acesso de aplicação para as subscrições, recursos e serviços, utilizador ou grupo.

## <a name="basics-of-access-management"></a>Noções básicas de gestão de acesso

Controlo de acesso baseado em funções fornece controlo de acesso detalhado que pode utilizar para proteger o seu ambiente. Dar aos utilizadores as permissões exatas que necessitam, ao atribuir uma função RBAC num determinado âmbito. O âmbito de atribuição de função pode ser uma subscrição, um grupo de recursos ou um único recurso. Leia o [controlo de acesso baseado em funções no portal do Azure](https://docs.microsoft.com/azure/role-based-access-control/overview) artigo para obter informações mais detalhadas sobre a gestão de acesso.

### <a name="built-in-roles"></a>Funções incorporadas

Pilha do Azure tem três funções básicas que pode aplicar a todos os tipos de recursos:

* **Proprietário** podem gerir tudo, incluindo o acesso a recursos.
* **Contribuidor** podem gerir tudo, exceto o acesso aos recursos.
* **Leitor** podem ver tudo, mas não é possível efetuar quaisquer alterações.

### <a name="resource-hierarchy-and-inheritance"></a>Hierarquia de recursos e herança

Pilha do Azure tem a hierarquia de recurso seguinte:

* Cada subscrição pertence a um diretório.
* Cada grupo de recursos pertence a uma subscrição.
* Cada recurso pertence a um grupo de recursos.

Acesso que conceda a um âmbito principal é herdado, os âmbitos subordinados. Por exemplo:

* Atribuir a função de leitor a um grupo do Azure AD no âmbito de subscrição. Os membros desse grupo podem ver cada grupo de recursos e recursos na subscrição.
* Atribuir a função de contribuinte a uma aplicação no âmbito do grupo de recursos. A aplicação pode gerir os recursos de todos os tipos nesse grupo de recursos, mas não outros grupos de recursos na subscrição.

### <a name="assigning-roles"></a>atribuir funções

Pode atribuir mais de uma função a um utilizador e cada função pode ser associada a outro âmbito. Por exemplo:

* Atribuir função leitor a TestUser-A à subscrição 1.
* Atribuir função de proprietário o TestUser-A para TestVM-1.

O Azure [atribuições de funções](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) artigo fornece informações detalhadas sobre a visualização, atribuir e eliminar funções.

### <a name="resource-hierarchy-and-inheritance"></a>Hierarquia de recursos e herança

Pilha do Azure tem a hierarquia de recurso seguinte:

* Cada subscrição pertence a um diretório.
* Cada grupo de recursos pertence a uma subscrição.
* Cada recurso pertence a um grupo de recursos.

Acesso que conceda a um âmbito principal é herdado, os âmbitos subordinados. Por exemplo:

* Atribuir o **leitor** função a um grupo do Azure AD no âmbito de subscrição. Os membros desse grupo podem ver cada grupo de recursos e recursos na subscrição.
* Atribuir o **contribuinte** função a uma aplicação no âmbito do grupo de recursos. A aplicação pode gerir os recursos de todos os tipos nesse grupo de recursos, mas não outros grupos de recursos na subscrição.

### <a name="assigning-roles"></a>atribuir funções

Pode atribuir mais de uma função a um utilizador e cada função pode ser associada a outro âmbito. Por exemplo:

* Atribuir função leitor a TestUser-A à subscrição 1.
* Atribuir função de proprietário o TestUser-A para TestVM-1.

O Azure [atribuições de funções](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) artigo fornece informações detalhadas sobre a visualização, atribuir e eliminar funções.

## <a name="set-access-permissions-for-a-user"></a>Definir permissões de acesso para um utilizador

Os passos seguintes descrevem como configurar permissões para um utilizador.

1. Inicie sessão com uma conta que tenha permissões de proprietário para o recurso que pretende gerir.
2. Na navegação à esquerda, selecione **Grupos de recursos**.
3. Escolha o nome do grupo de recursos que pretende definir permissões na.
4. No painel de navegação do grupo de recursos, escolha **(IAM) do controlo de acesso**. O **controlo de acesso** vista de lista os itens que têm acesso ao grupo de recursos. Pode filtrar estes resultados e utilize uma barra de menu para adicionar ou remover permissões.
5. No **controlo de acesso** menu barra, escolha **+ adicionar**.
6. No **adicionar permissões**:

   * Escolha a função que pretende atribuir a **função** na lista pendente.
   * Escolha o recurso que pretende atribuir a **atribuir acesso** na lista pendente.
   * Selecione o utilizador, grupo ou aplicação no diretório para o qual pretende dar acesso. Pode procurar o diretório com os nomes a apresentar, endereços de correio eletrónico e identificadores de objetos.

7. Selecione **Guardar**.

## <a name="next-steps"></a>Passos Seguintes

[Criar principais de serviço](azure-stack-create-service-principals.md)