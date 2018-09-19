---
title: Impedir ataques de força bruta utilizando o bloqueio inteligente do Azure AD
description: Bloqueio inteligente de Active Directory do Azure ajuda a proteger a sua organização contra ataques de força bruta tentando adivinhar as palavras-passe
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/18/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: rogoya
ms.openlocfilehash: 9ea91f70a72b812803a20244bb4445b76b133b0c
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/19/2018
ms.locfileid: "46296164"
---
# <a name="azure-active-directory-smart-lockout"></a>Bloqueio inteligente de Active Directory do Azure

Bloqueio inteligente utiliza inteligência da cloud para bloquear mal-intencionadas, que estão tentando adivinhar suas senhas ou usar métodos de força bruta para entrar. Esse intelligence pode reconhecer inícios de sessão geradas por usuários válidos e tratá-los de forma diferente do que aqueles de invasores e outras origens desconhecidas. Bloqueio inteligente bloqueia os invasores, ao mesmo tempo permitindo que os utilizadores continuam a aceder às suas contas e ser produtivo.

Por predefinição, o bloqueio inteligente bloqueia a conta de tentativas de início de sessão de um minuto após 10 tentativas falhadas. Os bloqueios de conta novamente após cada subsequente início de sessão tentativa falhada, durante um minuto em primeiro e mais tempo nas tentativas subsequentes.

Bloqueio inteligente está sempre ativado para todos os clientes do Azure AD com estas definições predefinidas que oferecem a combinação certa de segurança e usabilidade. Personalização das definições de bloqueio inteligente, com valores específicos da sua organização, requer o Azure AD básico ou superior licenças para os seus utilizadores.

Bloqueio inteligente pode ser integrado em implementações híbridas, com a sincronização de hash de palavra-passe ou autenticação pass-through para proteger contas do Active Directory no local de que está a ser bloqueada pelos atacantes. Ao definir políticas de bloqueio inteligente no Azure AD adequadamente, ataques podem ser filtrados antes de atingirem o Active Directory no local.

Ao usar [autenticação pass-through](../hybrid/how-to-connect-pta.md), precisa para se certificar de que:

   * O limiar de bloqueio do Azure AD é **menos** que o limiar de bloqueio de conta do Active Directory. Defina os valores para que o limiar de bloqueio de conta do Active Directory é, pelo menos, dois ou três vezes mais do que o limiar de bloqueio do Azure AD. 
   * A duração do bloqueio do Azure AD **em segundos** é **mais** que o Active Directory repor o contador de bloqueio de conta após a duração **minutos**.

> [!IMPORTANT]
> Atualmente um administrador não pode desbloquear as contas dos utilizadores na cloud, se foi bloqueadas pela capacidade de bloqueio inteligente. O administrador tem de aguardar a duração do bloqueio a expirar.

## <a name="verify-on-premises-account-lockout-policy"></a>Certifique-se a política de bloqueio de conta no local

Utilize as instruções seguintes para verificar a sua política de bloqueio de conta de Active Directory no local:

1. Abra a ferramenta de gerenciamento de diretiva de grupo.
2. Editar a política de grupo que inclua a política de bloqueio de conta da sua organização, por exemplo, o **política de domínio predefinida**.
3. Navegue até **configuração do computador** > **políticas** > **definições do Windows** > **as definições de segurança**   >  **Políticas de conta** > **política de bloqueio de conta**.
4. Verifique se sua **limiar de bloqueio de conta** e **reposição do contador de bloqueio de conta após** valores.

![Modificar a política de bloqueio do conta no local do Active Directory usando um objeto de política de grupo](./media/howto-password-smart-lockout/active-directory-on-premises-account-lockout-policy.png)

## <a name="manage-azure-ad-smart-lockout-values"></a>Gerir valores de bloqueio inteligente do Azure AD

Valores de bloqueio inteligente com base nos requisitos organizacionais, poderão ter de ser personalizado. Personalização das definições de bloqueio inteligente, com valores específicos da sua organização, requer o Azure AD básico ou superior licenças para os seus utilizadores.

Para verificar ou modificar os valores de bloqueio inteligente para a sua organização, utilize os seguintes passos:

1. Entrar para o [portal do Azure](https://portal.azure.com)e clique em **Azure Active Directory**, em seguida, **métodos de autenticação**.
1. Definir o **limiar de bloqueio**, com base no número inícios de sessão falhados são permitidos numa conta antes do seu primeiro bloqueio. A predefinição é 10.
1. Definir o **duração do bloqueio em segundos**, para o comprimento em segundos de cada bloqueio. A predefinição é 60 segundos (um minuto).

> [!NOTE]
> Se o primeiro início de sessão após um bloqueio também falha, a conta bloqueia novamente. Se uma conta bloqueios repetidamente, aumenta a duração do bloqueio.

![Personalizar a política de bloqueio inteligente do Azure AD no portal do Azure](./media/howto-password-smart-lockout/azure-active-directory-custom-smart-lockout-policy.png)
## <a name="next-steps"></a>Passos Seguintes

[Descubra como proibir o uso de palavras-passe incorretas na sua organização utilizar o Azure AD.](howto-password-ban-bad.md)

[Configure senhas de auto-atendimento repor para permitir aos utilizadores desbloquear as suas contas.](quickstart-sspr.md)