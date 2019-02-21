---
title: O Azure Application Insights deteção inteligente – futuras alterações aos destinatários de notificação predefinido | Documentos da Microsoft
description: Monitorizar rastreios de aplicações com o Azure Application Insights para padrões invulgares na telemetria de rastreio.
services: application-insights
author: harelbr
manager: carmonm
ms.service: application-insights
ms.topic: conceptual
ms.reviewer: mbullwin
ms.date: 02/12/2019
ms.author: harelbr
ms.openlocfilehash: 4bcbed82a78caff62a9459ecb44c6513f367f6b7
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/20/2019
ms.locfileid: "56457994"
---
# <a name="smart-detection-e-mail-notification-change"></a>Alterar a notificação de email de deteção inteligente

Com base nos comentários dos clientes, no dia 1 de Abril de 2019, estamos a alterar as funções predefinidas que recebem notificações por e-mail a partir de deteção inteligente.

## <a name="what-is-changing"></a>O que está a mudar?

Atualmente, são enviadas notificações de e-mail de deteção inteligente por predefinição, para o _proprietário da subscrição_, _contribuinte da subscrição_, e _leitor de subscrição_ funções. Estas funções incluem muitas vezes, os utilizadores que não estejam ativamente envolvidos na monitorização, que faz com que muitos destes utilizadores para receber notificações desnecessariamente. Para melhorar esta experiência, estamos a efetuar uma alteração para que as notificações por correio eletrónico só vão para o [monitorização leitor](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-reader) e [Contribuidor de monitorização](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor) funções por predefinição.

## <a name="scope-of-this-change"></a>Âmbito desta alteração

Esta alteração irá afetar todas as regras de deteção inteligente, excluindo os seguintes:

* Regras de deteção inteligentes marcadas como pré-visualização. Estas regras de deteção inteligente não é suportada notificações por e-mail hoje mesmo.

* Regra de anomalias de falha. Esta regra irá iniciar o direcionamento as novas funções de predefinição depois ele migrou de um alerta de clássico para a plataforma de alertas unificada (estão disponíveis mais informações [aqui](https://docs.microsoft.com/azure/azure-monitor/platform/monitoring-classic-retirement).)

## <a name="how-to-prepare-for-this-change"></a>Como se preparar para esta alteração?

Para garantir que sejam enviadas notificações de e-mail de deteção inteligente para os utilizadores relevantes, esses utilizadores tem de ser atribuídos para o [leitor de monitorização](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-reader) e [Contribuidor de monitorização](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor) funções. Deve ser feita a atribuição ao nível de subscrição (que afetam todos os recursos do Application Insights na subscrição), ou ao nível do recurso do Application Insights (que afetam apenas esse recurso específico).

Para atribuir utilizadores às funções de leitor de monitorização ou Contribuidor de monitorização através do portal do Azure, siga os passos descritos no [adicionar uma atribuição de função](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal#add-a-role-assignment) artigo. Certifique-se de selecionar o _monitorização leitor_ ou _Contribuidor de monitorização_ que a função ao qual os utilizadores são atribuídos.

> [!NOTE]
> Destinatários específicos de notificações de deteção inteligente, configuradas utilizando o _destinatários de e-mail adicionais_ opção nas definições da regra, não será afetada por esta alteração. Esses destinatários irão continuar a receber as notificações por e-mail.

Se tiver dúvidas ou preocupações sobre esta alteração, não hesite em [contacte-nos](mailto:smart-alert-feedback@microsoft.com).

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre a deteção inteligente:

- [Anomalias de falha](../../azure-monitor/app/proactive-failure-diagnostics.md)
- [Vazamentos de memória](../../azure-monitor/app/proactive-potential-memory-leak.md)
- [Anomalias de desempenho](../../azure-monitor/app/proactive-performance-diagnostics.md)