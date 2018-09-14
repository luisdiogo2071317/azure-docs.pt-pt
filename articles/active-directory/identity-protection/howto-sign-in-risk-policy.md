---
title: Como configurar a política de risco de início de sessão no Azure Active Directory Identity Protection | Documentos da Microsoft
description: Saiba como configurar a política de risco de início de sessão do Azure AD Identity Protection.
services: active-directory
keywords: proteção de identidade do Azure Active Directory, descoberta de aplicações na cloud, gestão de aplicações, a segurança, a risco, a nível de risco, a vulnerabilidade, a política de segurança
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.component: conditional-access
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/13/2018
ms.author: markvi
ms.reviewer: raluthra
ms.openlocfilehash: 7350cbd3e8aed6098f24d0edaa5807d241890287
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/14/2018
ms.locfileid: "45581476"
---
# <a name="how-to-configure-the-sign-in-risk-policy"></a>Como: Configurar a política de risco de início de sessão

O Azure Active Directory Deteta [tipos de eventos de risco](../reports-monitoring/concept-risk-events.md#risk-event-types) no offline e em tempo real. Cada evento de risco que foi detetado um início de sessão de um utilizador contribui para um conceito lógico chamado o início de sessão arriscado. Um risco início de sessão é um indicador de uma tentativa de início de sessão não poderão ter sido executada pelo proprietário legítimo de uma conta de utilizador.


## <a name="sign-in-risk-level"></a>Nível de risco do início de sessão

Um nível de risco de início de sessão é uma indicação (elevada, média ou baixa) da probabilidade de que uma tentativa de início de sessão não foi efetuada pelo proprietário legítimo de uma conta de utilizador.

## <a name="mitigating-sign-in-risk-events"></a>Mitigar eventos de risco de início de sessão

Uma atenuação é uma ação para limitar a capacidade de um atacante explore uma identidade comprometida ou dispositivo sem restaurar o dispositivo ou a identidade para um estado seguro. Uma atenuação não resolve eventos de risco de início de sessão anteriores associados com a identidade ou dispositivo.

Para mitigar o risco de inícios de sessão automaticamente, pode configurar políticas de segurança de risco de início de sessão. Utilizar estas políticas, considere o nível de risco do utilizador ou o início de sessão para inícios de sessão de risco de bloquear ou exigir que o utilizador para efetuar a autenticação multifator. Estas ações podem impedir que um invasor explorando uma identidade roubada provocar danos e podem lhe oferecer algum tempo para proteger a identidade.

## <a name="sign-in-risk-security-policy"></a>Política de segurança de risco de início de sessão
Uma política de risco de início de sessão é uma política de acesso condicional que avalia o risco para um início de sessão-in específico e aplica-se mitigações com base em regras e condições predefinidas.

![Política de início de sessão de risco](./media/howto-sign-in-risk-policy/1014.png "política de risco de início de sessão")

O Azure AD Identity Protection ajuda a gerir a redução de inícios de sessão de risco ao permitir-lhe:

* Defina os utilizadores e grupos que a política aplica-se a:

    ![Política de início de sessão de risco](./media/howto-sign-in-risk-policy/1015.png "política de risco de início de sessão")
* Defina o risco de início de sessão ao nível limiar (baixa, média ou alta) que aciona a política:

    ![Política de início de sessão de risco](./media/howto-sign-in-risk-policy/1016.png "política de risco de início de sessão")
* Defina os controlos a serem impostas quando a política aciona:  

    ![Política de início de sessão de risco](./media/howto-sign-in-risk-policy/1017.png "política de risco de início de sessão")
* Alterne o estado da política:

    ![Registo na MFA](./media/howto-sign-in-risk-policy/403.png "registo na MFA")
* Rever e avaliar o impacto de uma alteração antes de ativar ele:

    ![Política de início de sessão de risco](./media/howto-sign-in-risk-policy/1018.png "política de risco de início de sessão")

## <a name="what-you-need-to-know"></a>O que precisa saber
Pode configurar uma política de segurança de risco de início de sessão para exigir autenticação multifator:

![Política de início de sessão de risco](./media/howto-sign-in-risk-policy/1017.png "política de risco de início de sessão")

No entanto, por motivos de segurança, esta definição só funciona para os utilizadores que já tenham sido registados para a autenticação multifator. Se a condição para exigir autenticação multifator for satisfeita para um utilizador que ainda não está registado na autenticação multifator, o utilizador está bloqueado.

Como melhor prática, se quiser exigir autenticação multifator para risco inícios de sessão, deve:

1. Ativar a [política de registo de autenticação multifator](#multi-factor-authentication-registration-policy) para os utilizadores afetados.
2. Requer que os utilizadores afetados para início de sessão numa sessão não risco para efetuar um registo na MFA

Concluir estes passos garante que a autenticação multifator é necessária um risco para início de sessão.

## <a name="best-practices"></a>Melhores práticas
Escolher uma **elevada** limiar reduz o número de vezes que uma política é acionada e minimiza o impacto para os utilizadores.  

No entanto, exclui **baixa** e **médio** inícios de sessão sinalizados para risco da política, que não pode bloquear um invasor a exploração de uma identidade comprometida.

Ao definir a política

* Excluir os usuários que não o fizer / não é possível ter a autenticação multifator
* Excluir os usuários em localidades onde o ativar a política não é prático (por exemplo, sem acesso ao suporte técnico)
* Excluir utilizadores que têm propensão para gerar muitos falsos positivos (desenvolvedores, analistas de segurança)
* Utilize um **elevada** limiar durante o política inicial com, ou se deve minimizar desafios vistos pelos utilizadores finais.
* Utilize um **baixa** limiar se a sua organização precisar de maior segurança. Selecionar uma **baixa** limiar introduz utilizador adicional início de sessão desafios, mas uma maior segurança.

A predefinição recomendada na maioria das organizações é configurar uma regra para um **médio** limiar para encontrar o equilíbrio entre segurança e usabilidade.

A política de risco de início de sessão é:

* Aplicadas a todo o tráfego de navegador e inícios de sessão que utilizam autenticação moderna.
* Não é aplicada a aplicações através de protocolos de segurança mais antigos, desativando o ponto de extremidade do WS-Trust no IDP federado, por exemplo, o AD FS.

O **eventos de risco** página na consola do Identity Protection apresenta uma lista de todos os eventos:

* Esta política foi aplicada a
* Pode rever a atividade e determinar se a ação foi apropriada ou não

Para uma descrição geral da experiência do usuário relacionadas, consulte:

* [Recuperação de início de sessão arriscada](flows.md#risky-sign-in-recovery)
* [Risco início de sessão bloqueado](flows.md#risky-sign-in-blocked)  
* [Experiências de início de sessão com o Azure AD Identity Protection](flows.md)  

**Para abrir a caixa de diálogo de configuração relacionados**:

- Sobre o **do Azure AD Identity Protection** painel, na **configurar** secção, clique em **política de risco de início de sessão**.

    ![Política de risco do utilizador](./media/howto-sign-in-risk-policy/1014.png "política de risco do utilizador")




## <a name="next-steps"></a>Passos Seguintes

Para obter uma visão geral do Azure AD Identity Protection, consulte a [descrição geral do Azure AD Identity Protection](overview.md).
