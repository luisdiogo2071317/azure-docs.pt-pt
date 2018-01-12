---
title: "Quais são as notificações de estado de funcionamento do serviço do Azure? | Microsoft Docs"
description: "Notificações do Estado de funcionamento do serviço permitem-lhe ver mensagens de estado de funcionamento de serviço publicadas pelo Microsoft Azure."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: ancav
ms.openlocfilehash: 4a95e9882515e6a2861292829a44847e11f39063
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/10/2018
---
# <a name="view-service-health-notifications-by-using-the-azure-portal"></a>Ver as notificações de estado de funcionamento do serviço utilizando o portal do Azure

Notificações de estado de funcionamento de serviço são publicadas pelo Azure e contêm informações sobre os recursos na sua subscrição. Estas notificações são uma classe secundárias da atividade de registo de eventos e também podem ser encontradas no registo de atividade. Notificações do Estado de funcionamento do serviço podem ser informativa ou acionável, consoante a classe.

Existem várias classes de notificações do Estado de funcionamento do serviço:  

- **Ação necessária:** Azure poderá reparar que algo invulgar se acontecer na sua conta e trabalhar para resolver isto. Azure envia uma notificação, ou com detalhes sobre as ações que precisa de tomar ou como contactar o suporte ou de engenharia do Azure.  
- **Assistido recuperação:** tiver ocorrido um evento e engenheiros tem confirmado que ainda estão a experienciar impacto. Tem de engenharia do Azure trabalhar consigo diretamente para restaurar os serviços de estado de funcionamento completo.  
- **Incidente:** um evento que afeta o serviço está atualmente a afetar um ou mais recursos na sua subscrição.  
- **Manutenção:** uma atividade de manutenção planeada que poderá afetar um ou mais dos recursos na sua subscrição.  
- **Informações:** otimizações potenciais que podem ajudar a melhorar a utilização de recursos. 
- **Segurança:** urgentes relacionadas com segurança estão disponíveis informações sobre as soluções que são executadas no Azure.

Cada notificação de estado de funcionamento do serviço inclui detalhes sobre o âmbito e o impacto para os recursos. Detalhes incluem:

Nome da propriedade | Descrição
-------- | -----------
canais | Um dos seguintes valores: **Admin** ou **operação**.
correlationId | Normalmente, um GUID no formato de cadeia. Os eventos que pertencem, normalmente, para a mesma ação de partilham o mesmo correlationId.
eventDataId | O identificador exclusivo de um evento.
EventName | O título de um evento.
nível | O nível de um evento. Um dos seguintes valores: **crítico**, **erro**, **aviso** ou **informativo**.
resourceProviderName | O nome do fornecedor de recursos para o recurso afetado.
resourceType| O tipo de recurso do recurso afetado.
Subestado | Normalmente, o código de estado HTTP do resto correspondente chamar, mas também pode incluir outras cadeias que descrevem um subestado. Por exemplo: OK (código de estado de HTTP: 200), criado (código de estado HTTP: 201), aceite (código de estado de HTTP: 202), não conteúdo (código de estado de HTTP: 204), pedido incorreto (código de estado HTTP: 400), não encontrado (código de estado de HTTP: 404), conflito (código de estado HTTP: 409), interno do servidor Erro (código de estado HTTP: 500), serviço indisponível (código de estado HTTP: 503) e o tempo limite do Gateway (código de estado HTTP: 504).
eventTimestamp | Timestamp quando o evento foi gerado pelo serviço do Azure de processamento do pedido correspondente ao evento.
submissionTimestamp | Timestamp quando o evento ficou disponível para consulta.
subscriptionId | A subscrição do Azure em que este evento foi registado.
status | Cadeia que descrevem o estado da operação. Alguns valores comuns são: **iniciado**, **em curso**, **com êxito**, **falha**, **Active Directory**, e **Resolvido**.
operationName | O nome da operação.
categoria | Esta propriedade é sempre **ServiceHealth**.
resourceId | O ID de recurso do recurso afetado.
Properties.title | O título localizado para esta comunicação. Inglês é o predefinido.
Properties.Communication | Os detalhes localizados da comunicação com o markup HTML. Inglês é o predefinido.
Properties.incidentType | Um dos seguintes valores: **AssistedRecovery**, **ActionRequired**, **informações**, **incidente**,  **Manutenção**, ou **segurança**.
Properties.trackingId | O incidente a que este evento está associado. Utilize esta opção para correlacionar os eventos relacionados com um incidente.
Properties.impactedServices | Um blob JSON com caráter de escape que descreve os serviços e regiões afetadas pelo incidente. A propriedade inclui uma lista de serviços, cada um deles tem uma **ServiceName**e uma lista de regiões afetadas, cada um deles tem uma **RegionName**.
Properties.defaultLanguageTitle | A comunicação em inglês.
Properties.defaultLanguageContent | A comunicação em inglês como texto simples ou markup HTML.
Properties.Stage | Os valores possíveis para **AssistedRecovery**, **ActionRequired**, **informações**, **incidente**, e **segurança**  são **Active Directory** ou **resolvido**. Para **manutenção**, são: **Active Directory**, **planeada**, **em curso**, **cancelado**, **Reagendada**, **resolvido**, ou **concluída**.
Properties.communicationId | A comunicação com o qual este evento está associado.


## <a name="view-your-service-health-notifications-in-the-azure-portal"></a>Ver as notificações de estado de funcionamento do serviço no portal do Azure
1.  No [portal do Azure](https://portal.azure.com), selecione **Monitor**.

    ![Menu do portal captura de ecrã do Azure, com Monitor selecionado](./media/monitoring-service-notifications/home-monitor.png)

    Monitor do Azure reúne todas as suas monitorização definições e dados para uma vista consolidada. É aberto pela primeira vez para a secção **Registo de atividade**.

3.  Selecione **alertas**.

    ![Registo de captura de ecrã da atividade do Monitor, com os alertas selecionados](./media/monitoring-service-notifications/service-health-summary.png)
4. Selecione **+ Adicionar alerta de registo de atividade**e configurar um alerta para garantir que são notificados para notificações de serviço futuras. Para obter mais informações, consulte [criam alertas de registo de atividade em notificações de serviço](monitoring-activity-log-alerts-on-service-notifications.md).

## <a name="next-steps"></a>Passos Seguintes
Receber [sempre que uma notificação de estado de funcionamento do serviço de notificações de alertas](monitoring-activity-log-alerts-on-service-notifications.md) é publicada.  
Saiba mais sobre [alertas de registo de atividade](monitoring-activity-log-alerts.md).
