---
title: Autenticação multifator (MFA) e o PIM - Azure | Documentos da Microsoft
description: Saiba como o Azure AD Privileged Identity Management (PIM) valida a autenticação multifator (MFA).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 08/31/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: c26050eb8844368554726d0e642b7b7384649763
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55195617"
---
# <a name="multi-factor-authentication-mfa-and-pim"></a>Autenticação multifator (MFA) e o PIM

Recomendamos que exigem autenticação multifator (MFA) para todos os seus administradores. Isso reduz o risco de um ataque devido a uma palavra-passe comprometida.

Pode exigir que os utilizadores concluam uma submissão da MFA no início de sessão. Também pode exigir que os utilizadores concluam um desafio MFA quando ativam uma função no Azure AD Privileged Identity Management (PIM). Dessa forma, se o utilizador não foi concluída uma submissão da MFA quando eles conectado, eles serão solicitados a fazê-lo pelo PIM.

> [!IMPORTANT]
> Certo, agora, o MFA do Azure só funciona com trabalho contas profissionais ou escolares, não as contas Microsoft (normalmente, uma conta pessoal que serve para iniciar sessão nos serviços Microsoft, como o Skype, Xbox, Outlook.com, etc.). Por este motivo, qualquer pessoa que usar uma conta Microsoft não pode ser um administrador elegível porque estes não podem utilizar o MFA para ativar as suas funções. Se esses usuários precisam continuar a gerir cargas de trabalho com uma conta Microsoft, elevá-las para os administradores permanentes por agora.

## <a name="how-pim-validates-mfa"></a>Como o PIM valida a MFA

Existem duas opções para validar a MFA quando um utilizador ativa a uma função.

A opção mais simples é contar com MFA do Azure para utilizadores que estão a ativar uma função com privilégios. Para tal, verifique primeiro que esses utilizadores são licenciados, se necessário e tenham registado para MFA do Azure. Para obter mais informações sobre como implementar o MFA do Azure, consulte [implementar com base na cloud do Azure multi-factor Authentication](../authentication/howto-mfa-getstarted.md). Também é recomendável, mas não obrigatório, que configura o Azure AD para impor a MFA para estes utilizadores quando iniciam sessão. Isto acontece porque as verificações MFA serão feitas pelo PIM em si.

Em alternativa, se os utilizadores autenticam no local pode ter o seu fornecedor de identidade a ser responsável pela MFA. Por exemplo, se tiver configurado para exigir a autenticação baseada em smart card antes de aceder ao Azure AD, dos serviços de Federação do AD [proteger recursos da cloud com o Azure multi-factor Authentication e o AD FS](../authentication/howto-mfa-adfs.md) inclui instruções Para configurar o AD FS para enviar afirmações com o Azure AD. Quando um utilizador tenta ativar uma função, o PIM aceitará que MFA já foi validado para o utilizador depois de receber as declarações adequadas.

## <a name="next-steps"></a>Passos Seguintes

- [Configurar definições de função de diretório do Azure AD no PIM](pim-how-to-change-default-settings.md)
- [Configurar as definições de função de recursos do Azure no PIM](pim-resource-roles-configure-role-settings.md)
