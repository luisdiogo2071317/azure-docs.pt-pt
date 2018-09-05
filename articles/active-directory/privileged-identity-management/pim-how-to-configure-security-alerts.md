---
title: Configurar alertas de segurança para funções de diretório do Azure AD no PIM | Documentos da Microsoft
description: Saiba como configurar alertas de segurança para funções de diretório do Azure AD no Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 06/06/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 4e1cb47989011f179c54061bd29ae55b4ff86d80
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/04/2018
ms.locfileid: "43669416"
---
# <a name="configure-security-alerts-for-azure-ad-directory-roles-in-pim"></a>Configurar alertas de segurança para funções de diretório do Azure AD no PIM
## <a name="security-alerts"></a>Alertas de segurança
Azure Privileged Identity Management (PIM) gera alertas quando houver atividade suspeita ou não segura no seu ambiente. Quando for acionado um alerta, este aparece no dashboard do PIM. Selecione o alerta para ver um relatório que lista os utilizadores ou funções que acionou o alerta.

![Alertas de segurança de dashboard do PIM - captura de ecrã](./media/pim-how-to-configure-security-alerts/PIM_security_dash.png)

| Alerta | Gravidade | Acionador | Recomendação |
| --- | --- | --- | --- |
| **Funções estão a ser atribuídas fora do PIM** |Elevado |Um utilizador permanentemente foi atribuído uma função com privilégios, fora da interface do PIM. |Reveja os utilizadores na lista e anular a atribuição privilegiado-los a partir de funções atribuídas fora do PIM. |
| **Funções estão a ser ativadas com demasiada frequência** |Médio |Ocorreram demasiados reativações da mesma função no tempo permitido nas definições. |Entre em contato com o utilizador veja por que eles tenham ativado a função muitas vezes. Talvez o limite de tempo é demasiado curto para-los para concluir suas tarefas, ou talvez eles que estiver a utilizar scripts para ativar automaticamente uma função. Certifique-se que a duração de ativação para a respetiva função é definida tempo suficiente para os mesmos executar suas tarefas. |
| **Funções não exigem autenticação multifator para a ativação** |Médio |Existem funções sem MFA ativada nas definições. |Podemos exigir a MFA para as funções com privilégios mais elevados, mas recomendo enfaticamente usar a ativar a MFA para ativação de todas as funções. |
| **Os utilizadores não estiverem a utilizar as suas funções com privilégios** |Baixa |Existem os administradores elegíveis que ainda não tiver ativado as suas funções recentemente. |Inicie uma revisão de acesso para determinar os utilizadores que não precisam mais o acesso. |
| **Existem muitos administradores globais** |Baixa |Existem mais globais administradores que recomendado. |Se tiver um elevado número de administradores globais, é provável que os utilizadores estão a obter permissões mais do que o necessário. Mover os utilizadores a funções com menos privilégios, ou tornar alguns deles elegível para a função em vez de permanentemente atribuída. |

### <a name="severity"></a>Gravidade
* **Alta**: requer uma ação imediata devido a uma violação de política. 
* **Médio**: não requerer ação imediata, mas sinaliza uma potencial violação de política.
* **Baixa**: não requerer ação imediata, mas sugere uma alteração de política preferrable.

## <a name="configure-security-alert-settings"></a>Configurar definições de alerta de segurança
Pode personalizar algumas dos alertas de segurança no PIM para trabalhar com o seu ambiente e os objetivos de segurança. Siga estes passos para alcançar o painel de definições:

1. Entrar para o [portal do Azure](https://portal.azure.com/) e selecione o **Azure AD Privileged Identity Management** mosaico no dashboard.
2. Selecione **geridos funções com privilégios** > **definições** > **definições de alertas**.
   
    ![Navegar para as definições de alertas de segurança](./media/pim-how-to-configure-security-alerts/PIM_security_settings.png)

### <a name="roles-are-being-activated-too-frequently-alert"></a>Alerta de "Funções estão a ser ativadas com demasiada frequência"
Este alerta for acionado, se um utilizador ativa a mesma função com privilégios a várias vezes dentro de um período de tempo especificado. Pode configurar o período de tempo e o número de ativações.

* **Período de tempo de renovação de ativação**: Especifique em dias, horas, minutos e, segundo o período de tempo que pretende utilizar para controlar as renovações suspeitas.
* **Número de renovações de ativação**: Especifique o número de ativações, de 2 para 100, o que considerar digna de alerta, durante o período de tempo que escolheu. Pode alterar esta definição ao mover o controlo de deslize ou digitando um número na caixa de texto.

### <a name="there-are-too-many-global-administrators-alert"></a>Alerta de "Existem muitos administradores globais"
PIM aciona este alerta, se dois critérios diferentes são cumpridos, e pode configurar os dois. Em primeiro lugar, precisa atingir um certo limite de administradores globais. Em segundo lugar, um determinado percentual das atribuições de função total tem de ser administradores globais. Se cumprir apenas um dessas medidas, o alerta não aparecer.  

* **Número mínimo de administradores globais**: Especifique o número de administradores globais, de 2 para 100, que considere uma quantidade de não segura.
* **Percentagem de administradores globais**: Especifique a percentagem de administradores que são administradores globais, de % de 0 a 100%, que é inseguro no seu ambiente.

### <a name="administrators-arent-using-their-privileged-roles-alert"></a>Alerta de "Administradores não estiverem a utilizar as suas funções com privilégios"
Este alerta for acionado, se um utilizador der um determinado período de tempo sem uma função de ativação.

* **Número de dias**: Especifique o número de dias, de 0 a 100, o que um utilizador pode aceder sem ativar uma função.

## <a name="next-steps"></a>Passos Seguintes

- [Configurar definições de função de diretório do Azure AD no PIM](pim-how-to-change-default-settings.md)
