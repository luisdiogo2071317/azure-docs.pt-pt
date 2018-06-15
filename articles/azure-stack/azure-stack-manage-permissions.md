---
title: Gerir permissões para recursos por utilizador na pilha do Azure (administrador de serviço e inquilino) | Microsoft Docs
description: Como um administrador de serviço ou o inquilino, saiba como gerir RBAC permissões.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: fenila
editor: ''
ms.assetid: cccac19a-e1bf-4e36-8ac8-2228e8487646
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2018
ms.author: mabrigg
ms.reviewer: thomas.roettinger
ms.openlocfilehash: 0e50ea44ebb0b0a7285dab04666dd55cad480c6a
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
ms.locfileid: "29385641"
---
# <a name="manage-role-based-access-control"></a>Gerir o controlo de acesso baseado em funções

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Um utilizador na pilha do Azure pode ser um leitor, proprietário ou contribuinte para cada instância de um serviço, grupo de recursos ou subscrição. Por exemplo, o utilizador A poderá ter permissões de leitor a uma subscrição, mas tem permissões de proprietário para sete de Máquina Virtual.

 - Leitor: O utilizador pode ver tudo, mas não é possível efetuar quaisquer alterações.
 - Contribuidor: Utilizador pode gerir tudo, exceto acesso aos recursos.
 - O proprietário: Utilizador pode gerir tudo, incluindo o acesso a recursos.

## <a name="set-access-permissions-for-a-user"></a>Definir permissões de acesso para um utilizador

1. Inicie sessão com uma conta que tenha permissões de proprietário para o recurso que pretende gerir.
2. No painel de recursos, clique em de **acesso** ícone ![](media/azure-stack-manage-permissions/image1.png).
3. No **utilizadores** painel, clique em **funções**.
4. No **funções** painel, clique em **adicionar** ao adicionar permissões para o utilizador.

## <a name="set-access-permissions-for-a-universal-group"></a>Definir permissões de acesso para um grupo universal 

> [!Note]  
Aplicável apenas ao Active Directory federado a serviços (AD FS).

1. Inicie sessão com uma conta que tenha permissões de proprietário para o recurso que pretende gerir.
2. No painel de recursos, clique em de **acesso** ícone ![](media/azure-stack-manage-permissions/image1.png).
3. No **utilizadores** painel, clique em **funções**.
4. No **funções** painel, clique em **adicionar** ao adicionar permissões para o grupo Universal para o grupo de Active Directory.

## <a name="next-steps"></a>Passos Seguintes
[Adicionar um inquilino do Azure Stack](azure-stack-add-new-user-aad.md)

