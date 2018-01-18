---
title: Restaure ou remover um utilizador recentemente eliminado permanentemente no Azure Active Directory | Microsoft Docs
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
ms.date: 01/12/2018
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: it-pro
ms.openlocfilehash: d8a1850f8635097364268abdf77394ba592f761b
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2018
---
# <a name="restore-a-deleted-user-in-azure-active-directory"></a>Restaurar um utilizador eliminado no Azure Active Directory

Este artigo contém instruções para restaurar ou eliminar permanentemente um utilizador eliminado anteriormente. Quando elimina um utilizador no Azure Active Directory (Azure AD), o utilizador eliminado é mantido durante 30 dias a contar da data de eliminação. Durante este período, é possível restaurar o utilizador e as respetivas propriedades. 


## <a name="how-to-restore-a-recently-deleted-user"></a>Como restaurar um utilizador recentemente eliminado
Quando um utilizador for eliminado recentemente, todas as informações de diretório são preservadas. Se o utilizador é restaurado, essa informação é restaurada bem.

1. No [Centro de administração do Azure AD](https://aad.portal.azure.com), selecione **utilizadores e grupos** &gt; **todos os utilizadores**. 
2. Em **mostrar**, filtrar a página para mostrar **recentemente eliminado utilizadores**. 
3. Selecione um ou mais utilizadores eliminados recentemente.
4. Selecione **restauro utilizador**.

## <a name="how-to-permanently-delete-a-recently-deleted-user"></a>Como eliminar permanentemente um utilizador recentemente eliminado

1. No [Centro de administração do Azure AD](https://aad.portal.azure.com), selecione **utilizadores e grupos** &gt; **todos os utilizadores**. 
2. Em **mostrar**, filtrar a página para mostrar **recentemente eliminado utilizadores**. 
3. Selecione um ou mais utilizadores eliminados recentemente.
4. Selecione **eliminar de forma permanente**.

## <a name="required-permissions"></a>Permissões obrigatórias
As seguintes permissões são suficientes para restaurar um utilizador.

Função  | Permissões 
--------- | ---------
Administrador de Empresa<p>Parceiro de Suporte de Escalão 1<p>Parceiro de Suporte de Escalão 2<p>Administrador de Conta de Utilizador | Pode restaurar utilizadores eliminados 
Administrador de Empresa<p>Parceiro de Suporte de Escalão 1<p>Parceiro de Suporte de Escalão 2<p>Administrador de Conta de Utilizador | Pode eliminar permanentemente os utilizadores

## <a name="next-steps"></a>Passos Seguintes
Estes artigos fornecem informações adicionais sobre a gestão de utilizadores do Azure Active Directory.

* [Guia de introdução: Adicionar ou eliminar os utilizadores ao Azure Active Directory](add-users-azure-active-directory.md)
* [Gerir perfis de utilizador](active-directory-users-profile-azure-portal.md)
* [Adicionar utilizadores convidados a partir de outro diretório](active-directory-b2b-what-is-azure-ad-b2b.md) 
* [Atribuir um utilizador a uma função no seu Azure AD](active-directory-users-assign-role-azure-portal.md)
