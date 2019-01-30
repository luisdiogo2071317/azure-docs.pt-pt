---
title: Gerir permissões para recursos por utilizador no Azure Stack (administrador de serviço e inquilino) | Documentos da Microsoft
description: Como um administrador de serviços ou inquilino, saiba como gerir as permissões RBAC.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/14/2019
ms.author: mabrigg
ms.reviewer: thoroet
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: f20cd877e4cc53490016d251c5bdb343ab0cb4b0
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/30/2019
ms.locfileid: "55250338"
---
# <a name="manage-role-based-access-control"></a>Gerir controlo de acesso baseado em funções

*Aplica-se a: Integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Um utilizador no Azure Stack pode ser um leitor, proprietário ou Contribuidor para cada instância de uma subscrição, grupo de recursos ou ao serviço. Por exemplo, o utilizador A pode ter permissões de leitor para um de subscrição, mas tem permissões de proprietário para sete de Máquina Virtual.

 - Leitor de: Utilizador pode ver tudo, mas não é possível fazer alterações.
 - Contribuinte de: Utilizador pode gerir tudo, exceto o acesso aos recursos.
 - Proprietário: Utilizador pode gerir tudo, incluindo o acesso aos recursos.

## <a name="set-access-permissions-for-a-user"></a>Definir permissões de acesso para um utilizador

1. Inicie sessão com uma conta que tenha permissões de proprietário do recurso que pretende gerir.
2. No painel para o recurso, clique a **acesso** ícone ![](media/azure-stack-manage-permissions/image1.png).
3. Na **usuários** painel, clique em **funções**.
4. Na **funções** painel, clique em **Add** para adicionar permissões para o utilizador.

## <a name="set-access-permissions-for-a-universal-group"></a>Definir permissões de acesso para um grupo universal 

> [!Note]  
Aplicável apenas ao Active Directory, federada Services (AD FS).

1. Inicie sessão com uma conta que tenha permissões de proprietário do recurso que pretende gerir.
2. No painel para o recurso, clique a **acesso** ícone ![](media/azure-stack-manage-permissions/image1.png).
3. Na **usuários** painel, clique em **funções**.
4. Na **funções** painel, clique em **Add** para adicionar permissões para grupos universais do Active Directory de grupo.

## <a name="next-steps"></a>Passos Seguintes
[Adicionar um inquilino do Azure Stack](azure-stack-add-new-user-aad.md)

