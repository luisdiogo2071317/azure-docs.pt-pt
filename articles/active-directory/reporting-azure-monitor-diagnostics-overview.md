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
ms.openlocfilehash: 9b594360bc760fa9eec8ab9900c3aadcb10e9db6
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/25/2018
ms.locfileid: "39240113"
---
# <a name="azure-active-directory-activity-logs-in-azure-monitor-preview"></a>Registos de atividades do Azure Active Directory no Azure Monitor (pré-visualização)

Já pode encaminhar os registos de atividades do Azure AD com o Azure Monitor para a conta de armazenamento ou o Hub de Eventos. Com a pré-visualização pública dos registos do Azure Active Directory no Azure Monitor, pode:

* Arquivar os seus registos de auditoria numa conta de armazenamento do Azure, permitindo-lhe manter os dados durante muito tempo
* Transmitir em fluxo os registos de auditoria para um hub de eventos do Azure para análise, com ferramentas SIEM populares, como o Splunk e o QRadar
* Integrar os registos de auditoria nas suas soluções de registo personalizadas, ao transmiti-los em fluxo para um hub de eventos

> [!VIDEO https://www.youtube.com/embed/syT-9KNfug8]

## <a name="supported-reports"></a>Relatórios suportados

Pode encaminhar os registos de atividades de auditoria e os registos de atividades de inícios de sessão para a sua conta de armazenamento do Azure, o Hub de Eventos ou uma solução personalizada, mediante a utilização desta funcionalidade. 

* **Registos de auditoria**: o [relatório de atividade de registos de auditoria](active-directory-reporting-activity-audit-logs.md) dá-lhe acesso ao histórico de cada tarefa executada no seu inquilino.
* **Inícios de sessão**: com o [relatório de atividade de inícios de sessão](active-directory-reporting-activity-sign-ins.md), pode saber quem executou as tarefas reportadas no relatório de registos de auditoria.

> [!NOTE]
> A auditoria relacionada com B2C e os registos de atividades de inícios de sessão não são atualmente suportados.
>

## <a name="prerequisites"></a>Pré-requisitos

Para utilizar esta funcionalidade, precisa de:

* Uma subscrição do Azure. Se não tiver uma subscrição do Azure, pode [inscrever-se para obter uma avaliação gratuita](https://azure.microsoft.com/free/).
* [Licença](https://azure.microsoft.com/pricing/details/active-directory/) do Azure AD Gratuito, Básico, Premium 1 ou Premium 2 para aceder aos registos do Azure AD no portal do Azure. 

Dependendo de para onde pretende encaminhar os dados de registo de auditoria, precisa de:

* Uma conta de armazenamento do Azure, para a qual tenha as permissões *ListKeys* ativadas. Recomendamos que utilize uma conta de armazenamento geral e não uma conta de armazenamento de blobs. Para obter informações sobre os preços de armazenamento, veja a [Calculadora de preços do Armazenamento do Azure](https://azure.microsoft.com/pricing/calculator/?service=storage). 
* Um espaço de nomes dos hubs de eventos do Azure, para integrar com soluções de terceiros.

> [!NOTE]
> Tem de ser um *Administrador Global* ou *Administrador de Segurança* no inquilino do Azure AD para aceder aos registos de atividades do Azure AD.
>

## <a name="cost-considerations"></a>Considerações de custos

Se já tiver uma licença do Azure AD, precisa de uma subscrição do Azure para configurar a conta de armazenamento e o hub de eventos. A subscrição do Azure é fornecida sem custos, mas terá de pagar para utilizar os recursos do Azure, incluindo a conta de armazenamento que utilizar para arquivo e o hub de eventos que utilizar para transmissão em fluxo. A quantidade de dados e, por conseguinte, o custo incorrido, irá variar significativamente, consoante o tamanho do inquilino. 

### <a name="storage-size-for-activity-logs"></a>Tamanho de armazenamento para registos de atividades

Cada evento de registo de auditoria consome cerca de 2 KB de armazenamento de dados. Portanto, para um inquilino com 100 000 utilizadores, o que implicaria cerca de 1,5 milhões de eventos por dia, precisaria de aproximadamente 3 GB de armazenamento de dados por dia. Uma vez que ocorrem escritas em lotes com a duração aproximada de 5 minutos, é possível prever aproximadamente 9000 operações de escrita por mês. A tabela seguinte contém uma estimativa aproximada do custo, consoante o tamanho do inquilino, para uma conta de armazenamento v2 para fins gerais na região E.U.A. Oeste durante, pelo menos, um ano de retenção. Utilize a [Calculadora de preços do armazenamento do Azure](https://azure.microsoft.com/pricing/details/storage/blobs/) para criar uma estimativa mais exata do volume de dados que prevê para a sua aplicação. Cada evento de registo de auditoria consome cerca de 2 KB de armazenamento de dados. Portanto, para um inquilino com 100 000 utilizadores, o que implicaria cerca de 1,5 milhões de eventos por dia, precisaria de aproximadamente 3 GB de armazenamento de dados por dia. Uma vez que ocorrem escritas em lotes com a duração aproximada de 5 minutos, é possível prever aproximadamente 9000 operações de escrita por mês. A tabela seguinte contém uma estimativa aproximada do custo, consoante o tamanho do inquilino, para uma conta de armazenamento v2 para fins gerais na região E.U.A. Oeste durante, pelo menos, um ano de retenção. Utilize a [Calculadora de preços do armazenamento do Azure](https://azure.microsoft.com/pricing/details/storage/blobs/) para criar uma estimativa mais exata do volume de dados que prevê para a sua aplicação. 

| Categoria do registo | Número de utilizadores | Número de eventos/dia | Volume aproximado de dados por mês | Custo aproximado por mês | Custo aproximado por ano |
|--------------|-----------------|----------------------|--------------------------------------|----------------------------|---------------------------|
| Auditoria | 100 000 | 1,5 milhões | 90 GB | $1,93 | $23,12 |
| Auditoria | 1000 | 15 000 | 900 MB | $0,02 | $0,24 |
| Inícios de sessão | 1000 | 34 800 | 4GB | $0,13 | $1,56 |
| Inícios de sessão | 100 000 | 15 milhões | 1,7 TB | $35,41 | $424,92 | 


### <a name="event-hub-messages-for-activity-logs"></a>Mensagens do hub de eventos para os registos de atividades

Os eventos são colocados em lote com aproximadamente cinco minutos de intervalos e são enviados como uma única mensagem que contém todos os eventos durante esse período de tempo. Uma mensagem no hub de eventos tem um tamanho máximo de 256 KB e, se o tamanho de todas as mensagens durante o período de tempo exceder esse volume, são enviadas várias mensagens. 

Por exemplo, normalmente há cerca de 18 eventos por segundo para um inquilino de mais de 100 000 utilizadores, equivalendo a 5400 eventos a cada 5 minutos. Uma vez que os registos de auditoria são aproximadamente 2 KB por evento, isto equivale a 10,8 MB de dados; portanto, serão enviadas 43 mensagens ao hub de eventos nesse intervalo de 5 minutos. A tabela seguinte contém o custo aproximado de um hub de eventos básico nos E.U.A. Oeste, consoante o do volume de dados de eventos. Utilize a [Calculadora de preços do Hub de Eventos](https://azure.microsoft.com/pricing/details/event-hubs/) para calcular uma estimativa exata do volume de dados que prevê para a sua aplicação.

| Categoria do registo | Número de utilizadores | Número de eventos/segundo | Número de eventos por intervalo de 5 minutos | Volume por intervalo | Número de mensagens por intervalo | Número de mensagens por mês | Custo aproximado por mês |
|--------------|-----------------|-------------------------|----------------------------------------|---------------------|---------------------------------|------------------------------|----------------------------|
| Auditoria | 100 000 | 18 | 5400 | 10,8 MB | 43 | 371 520 | $10,83 |
| Auditoria | 1000 | 0.1 | 52 | 104 KB | 1 | 8640 | $10,8 |
| Inícios de sessão | 1000 | 178 | 53 400 | 106,8 MB | 418 | 3.611.520 | $11,06 |  


## <a name="frequently-asked-questions"></a>Perguntas mais frequentes

Esta secção contém as perguntas mais frequentes e os problemas conhecidos dos registos do Azure Active Directory no Azure Monitor.

**P: Por onde devo começar?** 

**R:** Comece pela [Descrição Geral](reporting-azure-monitor-diagnostics-overview.md) para ter uma ideia do que precisa para implementar esta funcionalidade. Quando estiver familiarizado com os pré-requisitos, consulte os tutoriais para ajudá-lo a configurar e encaminhar os registos para os Hubs de Eventos.

---

**P: Que registos estão incluídos?**

**R:** Tanto os registos de inícios de sessão como os registos de auditoria estão disponíveis para encaminhamento através desta funcionalidade, embora os eventos de auditoria relacionados com B2C não estejam atualmente incluídos. Leia o [Esquema de registo de auditoria](reporting-azure-monitor-diagnostics-audit-log-schema.md) e o [Esquema de registo de início de sessão](reporting-azure-monitor-diagnostics-sign-in-log-schema.md) para conhecer os tipos de registos e que registos baseados na funcionalidade são atualmente suportados. 

---

**P: Quanto tempo após uma ação os registos correspondentes serão apresentados nos Hubs de Eventos?**

**A:** Os registos devem aparecer nos Hubs de Eventos entre dois a cinco minutos após efetuar a ação. Para obter mais informações sobre os Hubs de Eventos, veja [O que são os hubs de eventos?](/azure/event-hubs/event-hubs-what-is-event-hubs.md)

---

**P: Quanto tempo após uma ação os registos correspondentes serão apresentados nas contas de armazenamento?**

**R:** Nas contas de armazenamento do Azure, a latência situa-se entre 5 e 15 minutos após efetuar a ação.

---

**P: Qual será o custo de armazenar os meus dados?**

**R:** O custo de armazenamento depende do tamanho dos seus registos, bem como do período de retenção que escolher. Para obter uma estimativa aproximada dos custos de inquilinos, consoante o volume de registos gerados, veja a [Descrição Geral](reporting-azure-monitor-diagnostics-overview.md).

---

**P: Qual será o custo de transmitir os meus dados para os Hubs de Eventos?**

**R:** O custo de transmissão em fluxo depende do número de mensagens que recebe por minuto. Leia a [Descrição Geral](reporting-azure-monitor-diagnostics-overview.md) para obter mais informações sobre como o custo é calculado, bem como para encontrar uma estimativa aproximada com base no número de mensagens. 

---

**P: Que ferramentas SIEM são atualmente suportadas?** 

**R:** Atualmente, o Azure Monitor é suportado pelo Splunk, QRadar e Sumologic. No entanto, o Splunk é a única ferramenta SIEM que é suportada para os registos do Azure Active Directory. Para obter mais informações sobre como funcionam os conectores, veja [Transmitir em fluxo dados de monitorização do Azure para um hub de eventos, para consumo por uma ferramenta externa](/azure/monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs).

---

**P: Posso aceder os dados do Hub de Eventos sem utilizar uma ferramenta SIEM externa?** 

**R:** Sim, pode utilizar a [API do Hub de Eventos](/azure/event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph) para aceder aos registos da sua aplicação personalizada. 

---


## <a name="next-steps"></a>Passos seguintes

* [Arquivar os registos de atividades numa conta de armazenamento](reporting-azure-monitor-diagnostics-azure-storage-account.md)
* [Encaminhar os registos de atividades para o Hub de Eventos](reporting-azure-monitor-diagnostics-azure-event-hub.md)
* [Leia mais sobre os Registos de Diagnóstico do Azure](/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)