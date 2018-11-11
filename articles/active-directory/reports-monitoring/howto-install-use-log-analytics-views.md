---
title: Como instalar e utilizar as vistas de Log Analytics do Azure Active Directory (pré-visualização) | Documentos da Microsoft
description: Saiba como instalar e utilizar as vistas do Log Analytics do Azure Active Directory (pré-visualização)
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 2290de3c-2858-4da0-b4ca-a00107702e26
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 10/01/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 11cc6a69b8072fca0639da5e517a39c22645710e
ms.sourcegitcommit: d372d75558fc7be78b1a4b42b4245f40f213018c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/09/2018
ms.locfileid: "51300511"
---
# <a name="install-and-use-the-log-analytics-views-for-azure-active-directory"></a>Instalar e utilizar as vistas do Log Analytics do Azure Active Directory

Os modos de exibição do Azure Active Directory do Log Analytics ajuda a analisar e registos de atividades de pesquisa do Azure AD no inquilino do Azure AD. Atividades do Azure AD registos incluem:

* Registos de auditoria: O [relatório de atividade de registos de auditoria](concept-audit-logs.md) permite-lhe aceder ao histórico de cada tarefa que é executada no seu inquilino.
* Registos de início de sessão: com o [relatório de atividade de início de sessão](concept-sign-ins.md), pode determinar quem realizou as tarefas que são enviadas nos registos de auditoria.

## <a name="prerequisites"></a>Pré-requisitos

Para utilizar as vistas do Log Analytics, tem de:

* Uma área de trabalho do Log Analytics na sua subscrição do Azure. Saiba como [criar uma área de trabalho do Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-quick-create-workspace).
* Em primeiro lugar, conclua os passos para [registos de atividade de rota do Azure AD para a área de trabalho do Log Analytics](howto-integrate-activity-logs-with-log-analytics.md).
* Transferir as vistas a partir do [repositório do GitHub](https://aka.ms/AADLogAnalyticsviews) para o computador local.

## <a name="install-the-log-analytics-views"></a>Instalar os modos de exibição do Log Analytics

1. Navegue até à sua área de trabalho do Log Analytics. Para tal, primeiro navegue para o [portal do Azure](https://portal.azure.com) e selecione **todos os serviços**. Tipo **do Log Analytics** na caixa de texto e selecione **do Log Analytics**. Selecione a área de trabalho encaminhado os registos de atividades, como parte dos pré-requisitos.
2. Selecione **estruturador de vistas**, selecione **importar** e, em seguida, selecione **Escolher ficheiro** para importar as exibições do computador local.
3. Selecione as vistas que transferiu a partir de pré-requisitos e selecione **guardar** para guardar a importação. Fazê-lo o **eventos de aprovisionamento da conta do Azure AD** vista e o **eventos de inícios de sessão** vista.

## <a name="use-the-views"></a>Utilizar as vistas

1. Navegue até à sua área de trabalho do Log Analytics. Para tal, primeiro navegue para o [portal do Azure](https://portal.azure.com) e selecione **todos os serviços**. Tipo **do Log Analytics** na caixa de texto e selecione **do Log Analytics**. Selecione a área de trabalho encaminhado os registos de atividades, como parte dos pré-requisitos.

2. Quando estiver na área de trabalho, selecione **resumo de área de trabalho**. Deverá ver as seguintes três vistas:

    * **Eventos de aprovisionamento de conta de AD do Azure**: esta vista mostra relatórios relacionados com a auditoria de atividade de aprovisionamento, como o número de novos utilizadores aprovisionado e falhas de aprovisionamento, o número de utilizadores atualizado e atualize a falhas e o número de utilizadores falhas de desaprovisionadas e correspondentes.    
    * **Eventos de inícios de sessão**: esta vista mostra os relatórios mais relevantes relacionada com a monitorização da atividade de entrada, tais como inícios de sessão por aplicação, utilizador, dispositivo, bem como uma vista de resumo de controlo o número de inícios de sessão ao longo do tempo.

3. Selecione uma destas vistas para avançar para os relatórios individuais. Também pode definir alertas em qualquer um dos parâmetros do relatório. Por exemplo, vamos definir um alerta para sempre que houver um erro de início de sessão. Para tal, primeiro selecione a **inícios de sessão de eventos** visualizar, selecione **erros de início de sessão ao longo do tempo** relatório e, em seguida, selecione **análise** para abrir a página de detalhes, com a consulta real por trás do relatório. 

    ![Detalhes](./media/howto-install-use-log-analytics-views/details.png)


4. Selecione **definir alerta**e, em seguida, selecione **pesquisa de registos personalizado o sempre que é &lt;lógica indefinida&gt;**  sob o **critérios de alerta** secção. Como queremos sempre que existe um erro de início de sessão de alertas, defina o **limiar** a lógica de alerta predefinido para **1** e, em seguida, selecione **feito**. 

    ![Configurar lógica de sinal](./media/howto-install-use-log-analytics-views/configure-signal-logic.png)

5. Introduza um nome e descrição do alerta e definir a gravidade **aviso**.

    ![Criar regra](./media/howto-install-use-log-analytics-views/create-rule.png)

6. Selecione o grupo de ação do alerta. Em geral, isso pode ser qualquer um de uma equipa que pretende notificar por e-mail ou mensagem de texto, ou pode ser uma tarefa automatizada através de webhooks, runbooks, funções, aplicações lógicas ou soluções ITSM externas. Saiba como [criar e gerir grupos de ação no portal do Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups).

7. Selecione **criar regra de alerta** para criar o alerta. Agora será alertado sempre que houver um erro de início de sessão.

## <a name="next-steps"></a>Passos Seguintes

* [Como analisar a atividade de registos no Log Analytics](howto-analyze-activity-logs-log-analytics.md)
* [Introdução ao Log Analytics no portal do Azure](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-analytics-portal)
