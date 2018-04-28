---
title: Ativar a MFA para todos os administradores do Azure
description: Orientações para ativar o administrador global
ms.service: security
author: barclayn
manager: mbaldwin
editor: TomSh
ms.topic: article
ms.date: 03/20/2018
ms.author: barclayn
ms.openlocfilehash: a7a08c54fe0c59e1e100e1c46e7a640da0692077
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="enforce-multi-factor-authentication-mfa-for-subscription-administrators"></a>Impor autenticação multifator (MFA) para os administradores da subscrição

Quando criar os administradores, incluindo a sua conta de administrador global, é essencial que utilize métodos de autenticação muito forte.

Pode realizar tarefas diárias de administração através da atribuição de funções de administrador específico — por exemplo, o administrador do Exchange ou o administrador de palavras-passe — contas de utilizador de equipa de TI, conforme necessário.
Além disso, permitindo [Azure multi-factor Authentication (MFA)](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) para os administradores adiciona uma segunda camada de segurança aos inícios de sessão de utilizador e de transações. Também ajuda-o MFA do Azure IT reduzem a probabilidade de que uma credencial comprometida terão acesso aos dados da organização.

Por exemplo: impor o MFA do Azure para os seus utilizadores e configurá-lo para utilizar uma chamada telefónica ou mensagem de texto como verificação. Se as credenciais do utilizador ficam comprometidas, o atacante será capaz de aceder a qualquer recurso uma vez que ele não terá acesso para o telefone do utilizador. As organizações que não adicione camadas adicionais de proteção de identidade sejam mais suscetíveis de ataque de roubo de credenciais, que pode levar ao comprometimento de dados.

Uma alternativa para as organizações que pretende manter a autenticação completa controlo no local é utilizar [do servidor multi-factor Authentication Azure](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server), também designado por "MFA no local". Através deste método, irá ainda ser capaz de impôr a autenticação multifator, mantendo o MFA server no local.

Para verificar que na sua organização tem privilégios administrativos, que pode verificar, utilizando o seguinte comando do PowerShell V2 do Microsoft Azure AD:

```azurepowershell-interactive
Get-AzureADDirectoryRole | Where { $_.DisplayName -eq "Company Administrator" } | Get-AzureADDirectoryRoleMember | Ft DisplayName
```

## <a name="enabling-mfa"></a>Ative a MFA

Reveja como [MFA](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next) funciona antes de continuar.

Desde que os utilizadores tenham licenças que incluam a Multi-Factor Authentication do Azure, não é necessário fazer nada para ativar a MFA do Azure. Pode requerer a verificação de dois passos para cada utilizador. As licenças que permitem a MFA do Azure são:

- Multi-Factor Authentication do Azure
- Azure Active Directory Premium
- Enterprise Mobility + Security

## <a name="turn-on-two-step-verification-for-users"></a>Ativar a verificação de dois passos para os utilizadores

Utilize um dos procedimentos listados na [como requerer a verificação de dois passos](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-user-states) para um utilizador ou grupo começar a utilizar o MFA do Azure. Pode optar por impor a verificação de dois passos para todos os inícios de sessão ou pode criar políticas de acesso condicional para exigir a verificação apenas quando lhe interessa.

