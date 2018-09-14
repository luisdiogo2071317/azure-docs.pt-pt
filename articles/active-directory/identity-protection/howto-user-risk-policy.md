---
title: Como configurar a política de risco do utilizador no Azure Active Directory Identity Protection | Documentos da Microsoft
description: Saiba como configurar a política de risco de utilizador do Azure AD Identity Protection.
services: active-directory
keywords: proteção de identidade do Azure Active Directory, descoberta de aplicações na cloud, gestão de aplicações, a segurança, a risco, a nível de risco, a vulnerabilidade, a política de segurança
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.component: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/08/2017
ms.author: markvi
ms.reviewer: raluthra
ms.openlocfilehash: bcfab9ab95e41b723cb8a8e49d7390a2894d5219
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/14/2018
ms.locfileid: "45581371"
---
# <a name="how-to-configure-the-user-risk-policy"></a>Como: Configurar a política de risco do utilizador

Todas as ativas [eventos de risco](../reports-monitoring/concept-risk-events.md) que foram detetadas pelo Azure Active Directory para um utilizador contribuir para um conceito lógico chamado risco de utilizador. Um utilizador sinalizado para risco é um indicador de uma conta de utilizador que pode ter sido comprometida.

![Utilizadores marcados em risco](./media/howto-user-risk-policy/1200.png)


## <a name="user-risk-level"></a>O utilizador tem um nível de risco elevado

Um nível de risco do utilizador é uma indicação (elevada, média ou baixa) da probabilidade de que a identidade do utilizador foi comprometida. Esta é calculada com base sobre os eventos de risco de utilizador que estão associados a identidade do utilizador.

O estado de um evento de risco está **Active Directory** ou **Closed**. Apenas os eventos que são de risco **Active Directory** contribuir para o cálculo de nível de risco de utilizador.

O nível de risco do utilizador é calculado usando as seguintes entradas:

* Eventos de risco de Active Directory que afetam o utilizador
* Nível de risco desses eventos
* Se as ações de remediação foram executadas

![Riscos de usuário](./media/howto-user-risk-policy/1031.png "riscos de utilizador")

Pode utilizar os níveis de risco do utilizador para criar políticas de acesso condicional a bloquear utilizadores de risco de iniciar sessão ou forçá-los em segurança alterar a palavra-passe.

## <a name="closing-risk-events-manually"></a>Fechar eventos de risco manualmente

Na maioria dos casos, realizará ações de remediação, como uma palavra-passe segura repor fechar automaticamente os eventos de risco. No entanto, isso pode não sempre ser possível.  
Isto é, por exemplo, o caso, quando:

* Um utilizador com eventos de risco de Active Directory foi eliminado
* Uma investigação revela que tem sido realizar um evento de risco comunicados pelo usuário legítimo

Como os eventos de risco que são **Active Directory** contribuir para o cálculo de risco do utilizador, poderá ter de reduza manualmente um nível de risco fechando os eventos de risco manualmente.  
Durante a investigação, pode optar por colocar uma destas ações para alterar o estado de um evento de risco:

![Ações](./media/howto-user-risk-policy/34.png "ações")

* **Resolver** - se depois de investigar um evento de risco, que realizou uma ação de remediação adequadas fora de proteção de identidade e considera que deve ser considerado o evento de risco fechado, marque o evento como resolvido. Resolver eventos irão definir o estado do evento de risco para fechado e o evento de risco já não irá contribuir para o risco de utilizador.
* **Marcar como falso-positivos** -em alguns casos, pode investigar um evento de risco e detetar que incorretamente foi sinalizado como um risco. Pode ajudar a reduzir o número de tais ocorrências, marcando o evento de risco como falso positivo. Isso ajudará os algoritmos de machine learning para melhorar a classificação de eventos semelhantes no futuro. É o estado de eventos de Falso-positivos **fechado** e já não irão contribuir para o risco de utilizador.
* **Ignorar** – se não tomar qualquer ação de remediação, mas o evento de risco a serem removidos da lista Active Directory, pode marcar um evento de risco ignorar e o estado do evento será fechado. Eventos ignorados não contribuem para o risco de utilizador. Esta opção deve ser usada apenas em circunstâncias invulgares.
* **Reativar** -eventos de risco que foram fechados manualmente (escolhendo **resolver**, **falso positivo**, ou **ignorar**) pode ser reativado, definindo o evento novamente o estado **Active Directory**. Eventos de risco reativado contribuem para o cálculo de nível de risco de utilizador. Eventos de risco fechados através de remediação (como repor uma palavra-passe segura) não podem ser reativados.

**Para abrir a caixa de diálogo de configuração relacionados**:

1. Sobre o **do Azure AD Identity Protection** painel, em **investigar**, clique em **eventos de risco**.

    ![Reposição de palavra-passe manual](./media/howto-user-risk-policy/1002.png "de reposição de palavra-passe Manual")
2. Na **eventos de risco** , clique num risco.

    ![Reposição de palavra-passe manual](./media/howto-user-risk-policy/1003.png "de reposição de palavra-passe Manual")
3. No painel de risco, com o botão direito um utilizador.

    ![Reposição de palavra-passe manual](./media/howto-user-risk-policy/1004.png "de reposição de palavra-passe Manual")

## <a name="closing-all-risk-events-for-a-user-manually"></a>Fechar todos os eventos de risco de um utilizador manualmente
Em vez de fechar manualmente individualmente eventos de risco de um utilizador de segurança, do Azure Active Directory Identity Protection também fornece um método para fechar todos os eventos para um utilizador com um só clique.

![Ações](./media/howto-user-risk-policy/2222.png "ações")

Quando clica em **dispensar todos os eventos**, todos os eventos são fechados e o utilizador afectado já não está em risco.

## <a name="remediating-user-risk-events"></a>Resolver eventos de risco do utilizador

Uma atualização é uma ação para proteger uma identidade ou um dispositivo que foi anteriormente suspeita ou conhecido por ser comprometido. Uma ação de remediação restaura a identidade ou um dispositivo para um estado seguro e resolve anterior eventos de risco associados com a identidade ou dispositivo.

Para remediar eventos de risco do utilizador, pode:

* Executar uma palavra-passe segura repor manualmente a remediar eventos de risco do utilizador
* Configurar uma política de segurança de risco do utilizador para mitigar ou remediar automaticamente eventos de risco do utilizador
* Recriar a imagem do dispositivo infetado  

### <a name="manual-secure-password-reset"></a>Reposição de palavra-passe segura manual
Uma reposição de palavra-passe segura é uma remediação em vigor a partir de muitos eventos de risco e quando executada, automaticamente fecha estes eventos de risco e recalcula o nível de risco do utilizador. Pode utilizar o dashboard do Identity Protection para iniciar uma palavra-passe reposta para um utilizador de risco.

A caixa de diálogo relacionada fornece dois métodos diferentes para repor uma palavra-passe:

**Repor palavra-passe** - selecione **exigir que o utilizador para repor a palavra-passe** para permitir ao utilizador para a Self-recuperação se o usuário tiver registrado para multi-factor authentication. Durante próximo início de sessão o usuário, o utilizador será necessária para resolver um desafio de autenticação multifator com êxito e, em seguida, forçado a alterar a palavra-passe. Esta opção não está disponível se a conta de utilizador não estiver registado multi-factor authentication.

**Palavra-passe temporária** - selecione **gerar uma palavra-passe temporária** imediatamente invalidar a palavra-passe existente e criar uma nova palavra-passe temporária ao utilizador. Envie a nova palavra-passe temporária para um endereço de correio eletrónico alternativo para o utilizador ou para o Gestor do utilizador. Uma vez que a palavra-passe é temporária, será pedido ao utilizador para alterar a palavra-passe após o início de sessão.

![Política](./media/howto-user-risk-policy/1005.png "política")

**Para abrir a caixa de diálogo de configuração relacionados**:

1. Sobre o **do Azure AD Identity Protection** painel, clique em **utilizadores sinalizados para risco**.

    ![Reposição de palavra-passe manual](./media/howto-user-risk-policy/1006.png "de reposição de palavra-passe Manual")
2. Na lista de utilizadores, selecione um utilizador com eventos de risco de pelo menos um.

    ![Reposição de palavra-passe manual](./media/howto-user-risk-policy/1007.png "de reposição de palavra-passe Manual")
3. No painel do utilizador, clique em **Repor palavra-passe**.

    ![Reposição de palavra-passe manual](./media/howto-user-risk-policy/1008.png "de reposição de palavra-passe Manual")

## <a name="user-risk-security-policy"></a>Política de segurança de risco do utilizador
Uma política de segurança de risco do utilizador é uma política de acesso condicional que avalia o nível de risco para um utilizador específico e aplica-se ações de remediação e atenuação com base em regras e condições predefinidas.

![Política de risco do utilizador](./media/howto-user-risk-policy/1009.png "política de risco do utilizador")

O Azure AD Identity Protection ajuda a gerir a mitigação e remediação de utilizadores sinalizados para risco ao permitir-lhe:

* Defina os utilizadores e grupos que a política aplica-se a:

    ![Política de risco do utilizador](./media/howto-user-risk-policy/1010.png "política de risco do utilizador")
* Defina o utilizador risco nível limiar (baixa, média ou alta) que aciona a política:

    ![Política de risco do utilizador](./media/howto-user-risk-policy/1011.png "política de risco do utilizador")
* Defina os controlos a serem impostas quando a política aciona:

    ![Política de risco do utilizador](./media/howto-user-risk-policy/1012.png "política de risco do utilizador")
* Alterne o estado da política:

    ![Política de risco do utilizador](./media/howto-user-risk-policy/403.png "registo na MFA")
* Rever e avaliar o impacto de uma alteração antes de ativar ele:

    ![Política de risco do utilizador](./media/howto-user-risk-policy/1013.png "política de risco do utilizador")

Escolher uma **elevada** limiar reduz o número de vezes que uma política é acionada e minimiza o impacto para os utilizadores.
No entanto, exclui **baixa** e **médio** utilizadores sinalizados para risco da política, que pode não proteger identidades ou dispositivos que foram anteriormente suspeita ou conhecidos por ser comprometido.

Ao definir a política

* Excluir utilizadores que têm propensão para gerar muitos falsos positivos (desenvolvedores, analistas de segurança)
* Excluir os usuários em localidades onde o ativar a política não é prático (por exemplo, sem acesso ao suporte técnico)
* Utilize um **elevada** limiar durante o política inicial com, ou se deve minimizar desafios vistos pelos utilizadores finais.
* Utilize um **baixa** limiar se a sua organização precisar de maior segurança. Selecionar uma **baixa** limiar introduz utilizador adicional início de sessão desafios, mas uma maior segurança.

A predefinição recomendada na maioria das organizações é configurar uma regra para um **médio** limiar para encontrar o equilíbrio entre segurança e usabilidade.

Para uma descrição geral da experiência do usuário relacionadas, consulte:

* [Comprometido de fluxo de recuperação de conta](flows.md#compromised-account-recovery).  
* [Comprometido fluxo conta bloqueada](flows.md#compromised-account-blocked).  

**Para abrir a caixa de diálogo de configuração relacionados**:

- Sobre o **Azure AD Identity Protection** painel, na **configurar** secção, clique em **política de risco do utilizador**.

    ![Política de risco do utilizador](./media/howto-user-risk-policy/1009.png "política de risco do utilizador")

## <a name="mitigating-user-risk-events"></a>Mitigar eventos de risco do utilizador
Os administradores podem definir uma política de segurança de risco do utilizador para impedir que os utilizadores após o início de sessão dependendo do nível de risco.

Bloquear um início de sessão:

* Impede que a geração de novos eventos de risco de utilizador para o utilizador afectado
* Permite aos administradores manualmente remediar os eventos de risco que afetam a identidade do utilizador e restaurá-lo para um estado seguro


## <a name="next-steps"></a>Passos Seguintes

Para obter uma visão geral do Azure AD Identity Protection, consulte a [descrição geral do Azure AD Identity Protection](overview.md).
