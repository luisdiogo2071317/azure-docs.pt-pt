---
title: Como configurar alertas de segurança | Microsoft Docs
description: Saiba como configurar alertas de segurança para a extensão do Azure Privileged Identity Management.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.component: protection
ms.date: 06/06/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 9b1779ed409a01356615561b1eb6fea0235529f4
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35233925"
---
# <a name="how-to-configure-security-alerts-in-azure-ad-privileged-identity-management"></a>Como configurar alertas de segurança no Azure AD Privileged Identity Management
## <a name="security-alerts"></a>Alertas de segurança
Azure Privileged Identity Management (PIM) gera alertas quando existe atividade suspeita ou insegura no seu ambiente. Quando um alerta é acionado, aparece no dashboard do PIM. Selecione o alerta para ver um relatório que lista os utilizadores ou funções que acionou o alerta.

![Alertas de segurança de dashboard do PIM - captura de ecrã][1]

| Alerta | Gravidade | Acionador | Recomendação |
| --- | --- | --- | --- |
| **Funções estão a ser atribuídas fora do PIM** |Elevado |Um utilizador permanentemente foi atribuído uma função com privilégios, fora da interface do PIM. |Reveja os utilizadores na lista e anule a atribuição privilegiado-los de funções atribuídas fora do PIM. |
| **Funções estão a ser ativadas com demasiada frequência** |Médio |Ocorreram demasiadas reactivations da mesma função dentro do tempo permitido nas definições. |Contacte o utilizador para ver por que motivo tem ativado a função tantas vezes. Talvez o limite de tempo é demasiado pequeno para-los para concluir as suas tarefas ou talvez estiver a utilizar scripts para ativar automaticamente uma função. Certifique-se que a duração de ativação para a respetiva função está definida tempo suficiente para os mesmos realize as suas tarefas. |
| **Funções não necessitam de autenticação multifator para a ativação** |Médio |Existem funções sem MFA ativada nas definições. |Iremos exigir a MFA para as funções com privilégios mais elevados, mas vivamente encorajar ativar a MFA para a ativação de todas as funções. |
| **Os utilizadores não estiverem a utilizar as respetivas funções com privilégios** |Baixa |Existem administradores elegíveis que ainda não tiver ativado as respetivas funções recentemente. |Inicie uma revisão do acesso para determinar os utilizadores que não precisam de acesso já. |
| **Existem demasiados administradores globais** |Baixa |Existem mais globais administradores que recomendado. |Se tiver um número elevado de administradores globais, é provável que os utilizadores estão a obter mais permissões do que precisam. Mover os utilizadores a funções com menos privilégios, ou fazer algumas delas elegível para a função em vez de permanentemente atribuído. |

### <a name="severity"></a>Gravidade
* **Elevada**: necessita de uma ação imediata devido a uma violação de política. 
* **Média**: não requerem ação imediata, mas sinalizar uma potencial violação de política.
* **Baixa**: não requerem ação imediata, mas sugere uma alteração de política preferrable.

## <a name="configure-security-alert-settings"></a>Configurar definições de alerta de segurança
Pode personalizar alguns dos alertas de segurança no PIM para trabalhar com o seu ambiente e os objetivos de segurança. Siga estes passos para alcançar o painel de definições:

1. Iniciar sessão para o [portal do Azure](https://portal.azure.com/) e selecione o **do Azure AD Privileged Identity Management** mosaico do dashboard.
2. Selecione **geridos funções com privilégios** > **definições** > **alertas definições**.
   
    ![Navegue para as definições de alertas de segurança][2]

### <a name="roles-are-being-activated-too-frequently-alert"></a>Alerta de "Funções estão a ser ativadas com demasiada frequência"
Este alerta é acionado se um utilizador ativa a mesma função com privilégios várias vezes num período de tempo especificado. Pode configurar o período de tempo e o número de ativações.

* **Período de tempo de renovação de ativação**: Especifique em dias, horas, minutos e segundo o período de tempo que pretende utilizar para controlar renovações suspeitas.
* **Número de renovação de ativação**: Especifique o número de ativações, de 2 para 100, o que considerar worthy de alerta, durante o período de tempo que escolheu. Pode alterar esta definição ao mover o controlo de deslize ou escrevendo um número na caixa de texto.

### <a name="there-are-too-many-global-administrators-alert"></a>Alerta de "Existem demasiados administradores globais"
PIM este alerta é acionado se forem cumpridos dois critérios diferentes e pode configurar ambos os parâmetros. Em primeiro lugar, tem de atingir um determinado limiar de administradores globais. Segundo, percentagem das suas atribuições de função total tem de ser administradores globais. Caso cumpra apenas uma destas medidas, o alerta não aparecer.  

* **Número mínimo de administradores globais**: Especifique o número de administradores globais, de 2 para 100, o que considerar uma quantidade insegura.
* **Percentagem de administradores globais**: Especifique a percentagem de administradores que são administradores globais, de % de 0 a 100%, que é segura no seu ambiente.

### <a name="administrators-arent-using-their-privileged-roles-alert"></a>Alerta de "Administradores não estiverem a utilizar as respetivas funções com privilégios"
Este alerta é acionado se um utilizador passa um determinado período de tempo sem uma função de a ativar.

* **Número de dias**: Especifique o número de dias, de 0 a 100, que um utilizador pode aceder sem ativar uma função.

## <a name="next-steps"></a>Passos Seguintes
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-configure-security-alerts/PIM_security_dash.png
[2]: ./media/active-directory-privileged-identity-management-how-to-configure-security-alerts/PIM_security_settings.png
