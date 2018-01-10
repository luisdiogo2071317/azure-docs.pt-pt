---
title: Restaurar um utilizador eliminado no Azure Active Directory | Microsoft Docs
description: Como restaurar um utilizador eliminado, ver os utilizadores que possam ser restaurados ou eliminar permanentemente um utilizador no Azure Active Directory
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 01/08/2018
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: it-pro
ms.openlocfilehash: c3b7550c2aea0e8bcb7998e0e8c732894b500403
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/10/2018
---
# <a name="restore-a-deleted-user-in-azure-active-directory"></a>Restaurar um utilizador eliminado no Azure Active Directory

Este artigo contém instruções para restaurar ou eliminar permanentemente um utilizador eliminado anteriormente. Quando elimina um utilizador no Azure Active Directory (Azure AD), o utilizador eliminado é mantido durante 30 dias a contar da data de eliminação. Durante este período, é possível restaurar o utilizador e as respetivas propriedades. 

## <a name="required-permissions"></a>Permissões obrigatórias
As seguintes permissões são suficientes para restaurar um utilizador.

Função  | Permissões 
--------- | ---------
Administrador de Empresa<p>Parceiro de Suporte de Escalão 1<p>Parceiro de Suporte de Escalão 2<p>Administrador de Conta de Utilizador | Pode restaurar utilizadores eliminados 
Administrador de Empresa<p>Parceiro de Suporte de Escalão 1<p>Parceiro de Suporte de Escalão 2<p>Administrador de Conta de Utilizador | Pode eliminar permanentemente os utilizadores

## <a name="how-to-restore-a-deleted-user"></a>Como restaurar um utilizador eliminado

No portal do Azure, pode restaurar um utilizador eliminado e eliminar permanentemente um utilizador eliminado.

1. No [Centro de administração do Azure AD](https://aad.portal.azure.com), selecione **utilizadores e grupos** &gt; **todos os utilizadores**. 
2. Em **mostrar**, filtrar a página para mostrar **recentemente eliminado utilizadores**. 
3. Selecione um ou mais utilizadores eliminados recentemente.
4. Selecione **restauro utilizador** ou **eliminar de forma permanente**.

## <a name="next-steps"></a>Passos Seguintes
Estes artigos fornecem informações adicionais sobre a gestão de utilizadores do Azure Active Directory.

* [Guia de introdução: Adicionar ou eliminar os utilizadores ao Azure Active Directory](add-users-azure-active-directory.md)
* [Gerir perfis de utilizador](active-directory-users-profile-azure-portal.md)
* [Adicionar utilizadores convidados a partir de outro diretório](active-directory-b2b-what-is-azure-ad-b2b.md) 
* [Atribuir um utilizador a uma função no seu Azure AD](active-directory-users-assign-role-azure-portal.md)
