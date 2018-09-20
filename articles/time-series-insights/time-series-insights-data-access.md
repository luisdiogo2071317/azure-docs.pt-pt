---
title: Configurar a segurança para aceder e gerir o Azure Time Series Insights | Documentos da Microsoft
description: Este artigo descreve como configurar permissões e segurança que o acesso de gestão políticas e acesso a dados políticas para proteger o Azure Time Series Insights.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 11/15/2017
ms.openlocfilehash: 7cb5dc5b170103f98d56abc920f36dd85f855961
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/19/2018
ms.locfileid: "46364824"
---
# <a name="grant-data-access-to-a-time-series-insights-environment-using-azure-portal"></a>Conceder acesso a dados a um ambiente do Time Series Insights com o portal do Azure

Os ambientes do Time Series Insights têm dois tipos de políticas de acesso independentes:

* Políticas de acesso de gestão
* Políticas de acesso a dados

Ambas as políticas concedem aos principais (utilizadores e aplicações) do Azure Active Directory várias permissões num determinado ambiente. Principais (utilizadores e aplicações) têm de pertencer ao active directory (conhecido como o inquilino do Azure) associado à subscrição que contém o ambiente.

As políticas de acesso de gestão concedem permissões relacionadas com a configuração do ambiente, como a:
*   Criação e eliminação do ambiente, origens de eventos, conjuntos de dados de referência, e
*   Gestão das políticas de acesso a dados.

As políticas de acesso a dados concedem permissões para emitir consultas de dados, manipular dados de referência no ambiente e partilhar consultas guardadas e perspetivas associadas ao ambiente.

Ambos os tipos de políticas permitem uma clara separação entre o acesso à gestão do ambiente e o acesso aos dados dentro do ambiente. Por exemplo, é possível configurar um ambiente de forma a que o proprietário/criador do ambiente é removido do acesso a dados. Além disso, os utilizadores e serviços que têm permissão para ler dados a partir do ambiente podem ser concedidos sem acesso à configuração do ambiente.

[!INCLUDE [iot-tsi-data-access](../../includes/iot-tsi-data-access.md)]

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais [como adicionar uma origem de evento do Hub de eventos para o seu ambiente do Azure Time Series Insights](time-series-insights-how-to-add-an-event-source-eventhub.md).
* [Enviar eventos](time-series-insights-send-events.md) para a origem do evento.
* Ver o seu ambiente no [Explorador do Time Series Insights](https://insights.timeseries.azure.com).
