---
title: Impedir ataques de força bruta utilizando bloqueio inteligente do Azure AD
description: Bloqueio de inteligente do Active Directory do Azure ajuda a proteger a sua organização contra ataques de força bruta tentam adivinhar as palavras-passe
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 06/25/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: rogoya
ms.openlocfilehash: d5beb5ce6e167cd100bec2ed54dc6ea0e78ba37b
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036257"
---
# <a name="azure-active-directory-smart-lockout"></a>Bloqueio de inteligente do Active Directory do Azure

Bloqueio inteligente utiliza nuvem intelligence para bloquear mal-intencionadas que está a tentar adivinhar as de palavras-passe dos utilizadores ou utilizar métodos de força bruta obter no. Esse intelligence pode reconhecer inícios de sessão proveniente dos utilizadores válidos e tratá-los de forma diferente do que os atacantes e outras origens desconhecidas. Pode bloquear os atacantes, permitindo que os utilizadores continuam a aceder das respetivas contas e sejam produtivos bloqueio inteligente.

Bloqueio inteligente está sempre ativada para todos os clientes do Azure AD com as definições predefinidas que oferecem a combinação certa de segurança e facilidade de utilização. Personalização das definições de bloqueio inteligente, com valores específicos para a sua organização, necessita do Azure AD Basic ou superiores licenças para os seus utilizadores.

Bloqueio inteligente pode ser integrado com implementações híbridas, a sincronização de hash de palavra-passe ou a autenticação pass-through para proteger contas do Active Directory no local de que está a ser bloqueada pelos atacantes. Ao definir políticas de bloqueio inteligente adequadamente no Azure AD, ataques podem ser filtrados antes de atingirem do Active Directory no local.

Quando utilizar [autenticação pass-through](../connect/active-directory-aadconnect-pass-through-authentication.md), terá de se certificar de que:

   * O limiar de bloqueio do Azure AD é **menos** ao limiar de bloqueio de conta do Active Directory. Defina os valores de forma a que o limiar de bloqueio de conta do Active Directory, pelo menos, dois ou três vezes superior ao limiar de bloqueio do Azure AD. 
   * A duração do bloqueio do Azure AD **em segundos** é **mais** que o Active Directory repor o contador de bloqueio da conta após a duração **minutos**.

> [!IMPORTANT]
> Um administrador não é possível desbloqueie as contas na nuvem se foi bloqueados para a capacidade de bloqueio inteligente. O administrador tem de aguardar o período de bloqueio expirar.

## <a name="verify-on-premises-account-lockout-policy"></a>Certifique-se a política de bloqueio de conta no local

Utilize as instruções seguintes para verificar a sua política de bloqueio de conta de Active Directory no local:

1. Abra a ferramenta de gestão de políticas de grupo.
2. Editar a política de grupo que inclua a política de bloqueio de conta da sua organização, por exemplo, o **política de domínio predefinida**.
3. Navegue até à **configuração do computador** > **políticas** > **definições do Windows** > **definições de segurança**   >  **Políticas de conta** > **política de bloqueio de conta**.
4. Certifique-se a **limiar de bloqueio de conta** e **reposição do contador de bloqueio da conta após** valores.

![Modificar a política de conta do Active Directory do local bloqueio utilizando um objeto de política de grupo](./media/howto-password-smart-lockout/active-directory-on-premises-account-lockout-policy.png)

## <a name="manage-azure-ad-smart-lockout-values"></a>Gerir os valores de bloqueio inteligente do Azure AD

Valores de bloqueio inteligente com base nos seus requisitos organizacionais, poderão ter de ser personalizadas. Personalização das definições de bloqueio inteligente, com valores específicos para a sua organização, necessita do Azure AD Basic ou superiores licenças para os seus utilizadores.

Para verificar ou modificar os valores de bloqueio inteligente para a sua organização, utilize os seguintes passos:

1. Iniciar sessão para o [portal do Azure](https://portal.azure.com)e clique em **do Azure Active Directory**, em seguida, **métodos de autenticação**.
1. Definir o **limiar de bloqueio**, com base nos quantos inícios de sessão falhados são permitidos numa conta antes do respetivo bloqueio primeiro. A predefinição é 10.
1. Definir o **duração do bloqueio em segundos**, para o período em segundos, de cada bloqueio.

> [!NOTE]
> Se o primeiro início de sessão após a falha de um bloqueio também, a conta bloqueia novamente. Se uma conta bloqueia repetidamente, aumenta a duração do bloqueio.

![Personalizar a política de bloqueio inteligente do Azure AD no portal do Azure](./media/howto-password-smart-lockout/azure-active-directory-custom-smart-lockout-policy.png)
## <a name="next-steps"></a>Passos Seguintes

[Saiba como banir palavras-passe incorretas na sua organização utilizar o Azure AD.](howto-password-ban-bad.md)

[Configure o self-service reposição palavra-passe para permitir que os utilizadores desbloqueiem as respetivas contas.](quickstart-sspr.md)