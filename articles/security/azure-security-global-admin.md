---
title: Ativar a MFA para todos os administradores do Azure
description: Documentação de orientação para ativar o administrador global
ms.service: security
author: barclayn
manager: barbkess
editor: TomSh
ms.topic: article
ms.date: 03/20/2018
ms.author: barclayn
ms.openlocfilehash: 2dcd7f42d86000dd3b642c10f6d3db0b0d0fcb03
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/12/2019
ms.locfileid: "56116342"
---
# <a name="enforce-multi-factor-authentication-mfa-for-subscription-administrators"></a>Impor autenticação multifator (MFA) para os administradores da subscrição

Quando cria seus administradores, incluindo sua conta de administrador global, é essencial que use métodos de autenticação muito forte.

Pode realizar diárias de administração de atribuir funções de administrador específico — por exemplo, o administrador do Exchange ou o administrador de palavras-passe — a contas de utilizador da equipe de TI conforme necessário.
Além disso, permitindo [Azure multi-factor Authentication (MFA)](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) nos seus administradores adiciona uma segunda camada de segurança aos inícios de sessão de utilizador e transações. MFA do Azure também ajuda a IT reduzir a probabilidade de que uma credencial comprometida terão acesso aos dados da organização.

Por exemplo: impor o MFA do Azure para os seus utilizadores e configurá-lo para utilizar uma chamada telefónica ou mensagem de texto como verificação. Se as credenciais do usuário forem comprometidas, o invasor não será capaz de aceder a qualquer recurso uma vez que ele não terá acesso para o telefone do utilizador. As organizações que não adicione camadas adicionais de proteção de identidade são mais suscetíveis de ataque de roubo de credenciais, que pode levar ao comprometimento de dados.

Uma alternativa para as organizações que desejam manter o autenticação completa controle no local é usar [servidor do Azure multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server), também chamado de "MFA no local". Ao utilizar este método, continuará a ser capaz de impor autenticação multifator, mantendo o MFA server no local.

Para verificar quem na sua organização tem privilégios administrativos, que pode verificar com o seguinte comando do Microsoft Azure AD V2 PowerShell:

```azurepowershell-interactive
Get-AzureADDirectoryRole | Where { $_.DisplayName -eq "Company Administrator" } | Get-AzureADDirectoryRoleMember | Ft DisplayName
```

## <a name="enabling-mfa"></a>Ativação da MFA

Rever como [MFA](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next) funciona antes de continuar.

Desde que os utilizadores tenham licenças que incluam a Multi-Factor Authentication do Azure, não é necessário fazer nada para ativar a MFA do Azure. Pode requerer a verificação de dois passos para cada utilizador. As licenças que permitem a MFA do Azure são:

- Multi-Factor Authentication do Azure
- Azure Active Directory Premium
- Enterprise Mobility + Security

## <a name="turn-on-two-step-verification-for-users"></a>Ativar a verificação de dois passos para os utilizadores

Utilize um dos procedimentos listados na [como requerer a verificação de dois passos](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-user-states) para um utilizador ou grupo começar a utilizar o MFA do Azure. Pode optar por impor a verificação de dois passos para todos os inícios de sessão ou pode criar políticas de acesso condicional para exigir a verificação apenas quando lhe interessa.

