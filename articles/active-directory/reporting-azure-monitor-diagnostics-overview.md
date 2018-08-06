---
title: Registos de atividades do Azure Active Directory no Azure Monitor (pré-visualização) | Microsoft Docs
description: Descrição geral dos registos de atividades do Azure Active Directory no Azure Monitor (pré-visualização)
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 07/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 0d88aef46761e8c7d8217f04befec88816587c03
ms.sourcegitcommit: 99a6a439886568c7ff65b9f73245d96a80a26d68
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/31/2018
ms.locfileid: "39358281"
---
# <a name="azure-ad-activity-logs-in-azure-monitor-preview"></a>Registos de atividades do Azure AD no Azure Monitor (pré-visualização)

Agora, pode encaminhar os registos de atividades do Azure Active Directory (Azure AD) para a sua conta de armazenamento ou o seu hub de eventos com o Azure Monitor. Com a pré-visualização pública dos registos do Azure Active Directory no Azure Monitor, pode:

* Arquivar os seus registos de auditoria numa conta de armazenamento do Azure, o que lhe permite manter os dados durante muito tempo.
* Transmitir em fluxo os registos de auditoria para um hub de eventos do Azure para análise com ferramentas populares de Gestão de Informações e Eventos de Segurança (SIEM), como o Splunk e o QRadar.
* Integrar os registos de auditoria nas suas soluções de registo personalizadas, ao transmiti-los em fluxo para um hub de eventos.

> [!VIDEO https://www.youtube.com/embed/syT-9KNfug8]

## <a name="supported-reports"></a>Relatórios suportados

Pode utilizar esta funcionalidade para encaminhar os registos de atividades de auditoria e os registos de atividades de inícios de sessão para a sua conta de armazenamento do Azure, o seu hub de eventos ou uma solução personalizada. 

* **Registos de auditoria**: o [relatório de atividades de registos de auditoria](active-directory-reporting-activity-audit-logs.md) dá-lhe acesso ao histórico de cada tarefa que é executada no seu inquilino.
* **Registos de inícios de sessão**: com o [relatório de atividades de inícios de sessão](active-directory-reporting-activity-sign-ins.md), pode saber quem executou as tarefas reportadas no relatório de registos de auditoria.

> [!NOTE]
> A auditoria relacionada com B2C e os registos de atividades de inícios de sessão não são suportados atualmente.
>

## <a name="prerequisites"></a>Pré-requisitos

Para utilizar esta funcionalidade, precisa de:

* Uma subscrição do Azure. Se não tiver uma subscrição do Azure, pode [inscrever-se para obter uma avaliação gratuita](https://azure.microsoft.com/free/).
* [Licença](https://azure.microsoft.com/pricing/details/active-directory/) do Azure AD Gratuito, Básico, Premium 1 ou Premium 2 para aceder aos registos do Azure AD no portal do Azure. 

Dependendo do local para onde pretende encaminhar os dados de registo de auditoria, precisa de um dos seguintes:

* Uma conta de armazenamento do Azure, para a qual tenha permissões *ListKeys*. Recomendamos que utilize uma conta de armazenamento para fins gerais e não uma conta de armazenamento de Blobs. Para obter informações sobre os preços de armazenamento, veja a [Calculadora de preços do Armazenamento do Azure](https://azure.microsoft.com/pricing/calculator/?service=storage). 
* Um espaço de nomes dos Hubs de Eventos do Azure, para integrar com soluções de terceiros.

> [!NOTE]
> Para aceder aos registos de atividades, tem de ser *administrador global* ou *administrador de segurança* no inquilino do Azure AD.
>

## <a name="cost-considerations"></a>Considerações de custos

Se já tiver uma licença do Azure AD, precisa de uma subscrição do Azure para configurar a conta de armazenamento e o hub de eventos. A subscrição do Azure é fornecida sem custos, mas terá de pagar para utilizar os recursos do Azure, incluindo a conta de armazenamento que utilizar para arquivo e o hub de eventos que utilizar para transmissão em fluxo. A quantidade de dados e, por conseguinte, o custo incorrido, pode variar significativamente consoante o tamanho do inquilino. 

### <a name="storage-size-for-activity-logs"></a>Tamanho de armazenamento para registos de atividades

Cada evento de registo de auditoria consome cerca de 2 KB de armazenamento de dados. Para um inquilino com 100 000 utilizadores, o que implicaria cerca de 1,5 milhões de eventos por dia, precisaria de aproximadamente 3 GB de armazenamento de dados por dia. Uma vez que ocorrem escritas em lotes com a duração aproximada de cinco minutos, é possível prever aproximadamente 9000 operações de escrita por mês. 

A tabela seguinte contém uma estimativa do custo, dependendo do tamanho do inquilino, de uma conta de armazenamento para fins gerais v2 nos E.U.A. Oeste durante, pelo menos, um ano de retenção. Para criar uma estimativa mais exata do volume de dados que prevê para a sua aplicação, utilize a [calculadora de preços do armazenamento do Azure](https://azure.microsoft.com/pricing/details/storage/blobs/). 

| Categoria do registo | Número de utilizadores | Eventos por dia | Volume de dados por mês (est.) | Custo por mês (est.) | Custo por ano (est.) |
|--------------|-----------------|----------------------|--------------------------------------|----------------------------|---------------------------|
| Auditoria | 100 000 | 1,5&nbsp;milhões | 90 GB | $1,93 | $23,12 |
| Auditoria | 1,000 | 15 000 | 900 MB | $0,02 | $0,24 |
| Inícios de sessão | 1,000 | 34 800 | 4GB | $0,13 | $1,56 |
| Inícios de sessão | 100 000 | 15&nbsp;milhões | 1,7 TB | $35,41 | $424,92 | 


### <a name="event-hub-messages-for-activity-logs"></a>Mensagens do hub de eventos para os registos de atividades

Os eventos são colocados em lote com aproximadamente cinco minutos de intervalo e são enviados como uma única mensagem que contém todos os eventos durante esse período de tempo. Uma mensagem no hub de eventos tem um tamanho máximo de 256 KB e, se o tamanho total de todas as mensagens durante o período de tempo exceder esse volume, são enviadas várias mensagens. 

Por exemplo, para um inquilino grande com mais de 100 000 utilizadores, ocorrem normalmente cerca de 18 eventos por segundo, uma taxa que equivale a 5400 eventos de cinco em cinco minutos. Uma vez que os registos de auditoria têm perto de 2k por evento, equivale a 10,8 MB de dados. Consequentemente, são enviadas 43 mensagens para o hub de eventos nesse intervalo de cinco minutos. 

A tabela seguinte contém os custos estimados por mês para um hub de eventos básico nos E.U.A. Oeste, consoante o volume de dados de eventos. Para calcular uma estimativa exata do volume de dados que prevê para a sua aplicação, utilize a [calculadora de preços do Hub de Eventos](https://azure.microsoft.com/pricing/details/event-hubs/).

| Categoria do registo | Número de utilizadores | Eventos por segundo | Eventos por intervalo de cinco minutos | Volume por intervalo | Mensagens por intervalo | Mensagens por mês | Custo por mês (est.) |
|--------------|-----------------|-------------------------|----------------------------------------|---------------------|---------------------------------|------------------------------|----------------------------|
| Auditoria | 100 000 | 18 | 5400 | 10,8 MB | 43 | 371 520 | $10,83 |
| Auditoria | 1,000 | 0.1 | 52 | 104 KB | 1 | 8640 | 10,80 $ |
| Inícios de sessão | 1,000 | 178 | 53 400 | 106,8&nbsp;MB | 418 | 3.611.520 | $11,06 |  


## <a name="frequently-asked-questions"></a>Perguntas mais frequentes

Esta secção responde às perguntas mais frequentes e inclui discussões sobre problemas conhecidos dos registos do Azure AD no Azure Monitor.

**P: Por onde devo começar?** 

**R**: este artigo aborda o que precisa para implementar esta funcionalidade. Quando estiver satisfeito com os pré-requisitos, aceda aos tutoriais que o podem ajudar a configurar e encaminhar os registos para um hub de eventos.

---

**P: Que registos estão incluídos?**

**R:** Tanto os registos de inícios de sessão, como os registos de auditoria, estão disponíveis para encaminhamento através desta funcionalidade, embora os eventos de auditoria relacionados com B2C não estejam atualmente incluídos. Para conhecer os tipos de registos e que registos baseados na funcionalidade são atualmente suportados, veja [Audit log schema](reporting-azure-monitor-diagnostics-audit-log-schema.md) (Esquema de registos de auditoria) e [Sign-in log schema](reporting-azure-monitor-diagnostics-sign-in-log-schema.md) (Esquema de registo de inícios de sessão). 

---

**P: Quanto tempo após uma ação é que os registos correspondentes são apresentados nos hubs de eventos?**

**R**: os registos devem aparecer no hub de eventos entre dois a cinco minutos após a ação ter sido realizada. Para obter mais informações sobre os Hubs de Eventos, veja [O que são os Hubs de Eventos do Azure?](../event-hubs/event-hubs-about.md)

---

**P: Quanto tempo após uma ação é que os registos correspondentes são apresentados nas contas de armazenamento?**

**R**: Relativamente às contas de armazenamento do Azure, a latência situa-se entre 5 e 15 minutos após a ação ter sido realizada.

---

**P: Qual será o custo de armazenar os meus dados?**

**R**: Os custos de armazenamento dependem do tamanho dos seus registos e do período de retenção que escolher. Para obter uma lista dos custos estimados para os inquilinos, que dependerão do volume de registos gerados, aceda à secção [Tamanho do armazenamento para os registos de atividades](https://review.docs.microsoft.com/en-us/azure/active-directory/reporting-azure-monitor-diagnostics-overview?branch=pr-en-us-47660#storage-size-for-activity-logs).

---

**P: Qual será o custo de transmitir os meus dados para os hubs de eventos?**

**R**: Os custos da transmissão em fluxo dependem do número de mensagens que recebe por minuto. Este artigo mostra como é que os custos são calculados e apresenta uma lista das estimativas de custos, que têm por base o número de mensagens. 

---

**P: Que ferramentas SIEM são atualmente suportadas?** 

**R:** Atualmente, o Azure Monitor é suportado pelo Splunk, o QRadar e o Sumo Logic. No entanto, o Splunk é a única ferramenta SIEM que é suportada para os registos do Azure AD. Para obter mais informações sobre como funcionam os conectores, veja [Stream Azure monitoring data to an event hub for consumption by an external tool](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md) (Transmitir em fluxo dados de monitorização do Azure para um hub de eventos, para consumo por uma ferramenta externa).

---

**P: Posso aceder aos dados de um hub de eventos sem utilizar uma ferramenta SIEM externa?** 

**R**: Sim. Pode utilizar a [API dos Hub de Eventos](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md) para aceder aos registos da sua aplicação personalizada. 

---


## <a name="next-steps"></a>Passos seguintes

* [Arquivar registos de atividades numa conta de armazenamento](reporting-azure-monitor-diagnostics-azure-storage-account.md)
* [Encaminhar registos de atividades para um hub de eventos](reporting-azure-monitor-diagnostics-azure-event-hub.md)
* [Leia mais sobre os Registos de Diagnóstico do Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)
