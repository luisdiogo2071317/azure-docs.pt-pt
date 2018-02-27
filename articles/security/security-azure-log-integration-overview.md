---
title: Integrar os registos de recursos do Azure para os sistemas SIEM | Microsoft Docs
description: "Saiba mais sobre a integração de registos do Azure, as suas capacidades principais e como funciona."
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TerryLanfear
ms.assetid: 9c1346e1-baf8-4975-b2f2-42ae05b2dc0a
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/15/2018
ms.author: TomSh
ms.custom: azlog
ms.openlocfilehash: e427e2f7dafb6db9bc5c15d841fbbf83a02fc0e1
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
---
# <a name="introduction-to-microsoft-azure-log-integration"></a>Introdução à integração de registo do Microsoft Azure

Integração de registos do Azure permite-lhe integrar registos não processados a partir dos seus recursos do Azure com os sistemas Security Information and Event Management (SIEM) no local no caso de um conector para [Azure Monitor](../monitoring-and-diagnostics/monitoring-get-started.md) ainda não está disponível a partir do o fornecedor do SIEM.

O método preferencial para integrar os registos do Azure é através do conector de Monitor do Azure do seu fornecedor SIEM e seguir estes [instruções](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md). No entanto, se o fornecedor do SIEM não fornecer um conector para monitorizar o Azure, poderá utilizar a integração de registo do Azure como uma solução temporária (se o SIEM for suportado pelo Azure registo integração) até esse um conector está disponível.

>[!IMPORTANT]
>Se tiver o interesse primário recolher registos de máquina virtual, a maioria dos fornecedores SIEM incluir esta na sua solução. Utilizar o SIEM conector do fornecedor deve ser sempre a alternativa preferencial.

Integração de registos do Azure recolhe eventos do Windows a partir de registos do Visualizador de eventos do Windows, [registos de atividade do Azure](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), [alertas do Centro de segurança do Azure](../security-center/security-center-intro.md), e [registos de diagnóstico do Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) a partir dos recursos do Azure. Esta integração ajuda a sua solução SIEM fornecer um dashboard unificado para todos os seus recursos, no local ou na nuvem, para que pode agregar, correlacionar, analisar e alerta para eventos de segurança.

>[!NOTE]
Neste momento, as nuvens suportadas apenas são comercial do Azure e Azure Government. Não são suportadas outras nuvens.

![Integração de registos do Azure][1]

## <a name="what-logs-can-i-integrate"></a>Os registos que pode a integrar

Azure produz um vasto conjunto registo para cada serviço do Azure. Estes registos representam três tipos de registos:

* **Registos de controlo/gestão** proporcionam visibilidade a [do Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) operações criar, ATUALIZAR e eliminar. Os registos de atividade do Azure é um exemplo deste tipo de registo.
* **Dados plane registos** dar visibilidade sobre os eventos gerados como parte da utilização de um recurso do Azure. Um exemplo deste tipo de registo é o Visualizador de eventos de Windows **sistema**, **segurança**, e **aplicação** canais na máquina virtual do Windows. Outro exemplo é configurada por meio do Monitor do Azure de registo de diagnóstico
* **Processar eventos** fornecer eventos analisados e informações de alerta processados em seu nome. Um exemplo deste tipo de evento é Azure Center alertas de segurança, em que Centro de segurança do Azure tem processado e analisados a sua subscrição para fornecer alertas relevantes para a sua postura de segurança atual.

Integração de registo do Azure suporta ArcSight, QRadar e Splunk. Em todas as circunstâncias, verifique junto do fabricante do SIEM para avaliar se tiverem um conector nativo. Não deve utilizar a integração de registo do Azure quando conectores nativos estão disponíveis.

Se não existem outras opções estão disponíveis, pode ser considerada a integração de registo do Azure. A tabela seguinte inclui a nossa recomendações.

|**SIEM** | **Cliente já a utilizar integrador de registo** | **Cliente investigar as opções de integração do SIEM**|
|---------|--------------------------|-------------------------------------------|
|**SPLUNK** | Começar a migrar para o [suplemento de monitor do Azure para Splunk](https://splunkbase.splunk.com/app/3534/) | Utilize [conector SPLUNK](https://splunkbase.splunk.com/app/3534/) |
|**IBM QRADAR** | Migrar para ou a começar a utilizar o conector de QRadar documentado no fim do http://aka.ms/azmoneventhub | Utilizar o conector de QRadar documentado no fim do http://aka.ms/azmoneventhub  |
|**ARCSIGHT** | Continuar a utilizar o integrador de registo, até um conector está disponível, em seguida, migrar para a solução com base no conector.  | Considere Log Analytics do Azure como alternativa. Efetue não carregar para a integração de registo do Azure, a menos que está disposto a seguir o processo de migração quando o conector fica disponível. |

>[!NOTE]
>Enquanto a integração de registo do Azure é uma solução livre, existem custos de armazenamento do Azure resultante do armazenamento de informações do ficheiro de registo.

Se necessitar de assistência pode abrir um [pedido de suporte](../azure-supportability/how-to-create-azure-support-request.md). Para tal, selecione **integração de registo** como o serviço para o qual está a pedir suporte.

## <a name="next-steps"></a>Passos Seguintes

Neste documento, foram introduzidas para integração de registos do Azure. Para saber mais sobre a integração de registos do Azure e os tipos de registos suportados, consulte o seguinte:

* [Introdução à integração de registos do Azure](security-azure-log-integration-get-started.md) - este tutorial orienta-o através da instalação de integração de registos do Azure e integrar os registos do armazenamento do Azure WAD, registos de atividade do Azure, alertas do Centro de segurança do Azure e o Azure Active Directory registos de auditoria.
* [Registos do Azure integração perguntas mais frequentes (FAQ) do sobre](security-azure-log-integration-faq.md) -FAQ este respondem a dúvidas sobre a integração de registos do Azure.
* [Azure de fluxo monitorização dados para um hub de eventos para consumo por uma ferramenta externa](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md)

<!--Image references-->
[1]: ./media/security-azure-log-integration-overview/azure-log-integration.png
