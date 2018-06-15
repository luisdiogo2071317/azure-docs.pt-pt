---
title: Solução de gestão no Azure melhores práticas | Microsoft Docs
description: ''
services: operations-management-suite
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 1915e204-ba7e-431b-9718-9eb6b4213ad8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2017
ms.author: bwren
ms.openlocfilehash: 0bd5e19e00dbae1d0ece27d0498a1f599dba05b7
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/08/2018
ms.locfileid: "33887840"
---
# <a name="best-practices-for-creating-management-solutions-in-azure-preview"></a>Melhores práticas para criar soluções de gestão no Azure (pré-visualização)
> [!NOTE]
> Esta documentação é de preliminar para criar soluções de gestão no Azure que estão atualmente em pré-visualização. Nenhum esquema descrita abaixo está sujeita a alterações.  

Este artigo apresenta as melhores práticas para [criar um ficheiro de solução de gestão](monitoring-solutions-solution-file.md) no Azure.  Estas informações serão atualizadas conforme as melhores práticas adicionais são identificadas.

## <a name="data-sources"></a>Origens de dados
- Origens de dados podem ser [configurado com um modelo do Resource Manager](../log-analytics/log-analytics-template-workspace-configuration.md), mas não devem ser incluídos no ficheiro de solução.  O motivo é que configurar origens de dados não é atualmente idempotent, o que significa que a solução foi substituir a configuração existente na área de trabalho do utilizador.<br><br>Por exemplo, a solução pode necessitar de eventos de aviso e erro de registo de eventos de aplicações.  Se especificar esta como uma origem de dados na sua solução, o risco de remover os eventos de informações se o utilizador tinha esta configuração na respetiva área de trabalho.  Se incluídos todos os eventos, poderá ser a recolher eventos excessivos de informações na área de trabalho do utilizador.

- Se a sua solução requer que dados de uma das origens de dados padrão, em seguida, deve definir este como pré-requisito.  Estado na documentação de que o cliente tem de configurar a origem de dados por si próprios.  
- Adicionar um [a verificação de fluxo de dados](../log-analytics/log-analytics-view-designer-tiles.md) mensagem para todas as vistas na sua solução ao instruir o utilizador em origens de dados que têm de ser configurados para os dados necessários para serem recolhidos.  Esta mensagem é apresentada no mosaico da vista quando os dados necessários não foi encontrados.


## <a name="runbooks"></a>Runbooks
- Adicionar um [automatização](../automation/automation-schedules.md) para cada runbook na sua solução de que necessita para ser executada com base numa agenda.
- Incluir o [IngestionAPI módulo](https://www.powershellgallery.com/packages/OMSIngestionAPI/1.5) na sua solução para ser utilizada pelos runbooks escrita de dados para o repositório de análise de registos.  Configurar a solução para [referência](monitoring-solutions-solution-file.md#solution-resource) este recurso, de modo que permanece se a solução é removida.  Isto permite que várias soluções partilhar o módulo.
- Utilize [as variáveis da automatização](../automation/automation-schedules.md) para fornecer valores para a solução que os utilizadores ser aconselhável alterar mais tarde.  Mesmo que a solução é configurada para conter a variável, ainda pode ser alterado de valor.

## <a name="views"></a>Vistas
- Todas as soluções devem incluir uma vista única que é apresentada no portal do utilizador.  A vista pode conter vários [partes de visualização](../log-analytics/log-analytics-view-designer-parts.md) para ilustrar diferentes conjuntos de dados.
- Adicionar um [a verificação de fluxo de dados](../log-analytics/log-analytics-view-designer-tiles.md) mensagem para todas as vistas na sua solução ao instruir o utilizador em origens de dados que têm de ser configurados para os dados necessários para serem recolhidos.
- Configurar a solução para [conter](monitoring-solutions-solution-file.md#solution-resource) a vista, para que as TI do removidos se a solução é removida.

## <a name="alerts"></a>Alertas
- Defina a lista de destinatários como um parâmetro no ficheiro de solução para que o utilizador pode defini-las ao efetuar a instalação a solução.
- Configurar a solução para [referência](monitoring-solutions-solution-file.md#solution-resource) regras de alerta para esse utilizador possa alterar a respetiva configuração.  Podem optar por efetuar alterações, tais como modificar a lista de destinatários, alterar o limiar do alerta ou desativar a regra de alerta. 


## <a name="next-steps"></a>Passos Seguintes
* Percorrer o processo básico de [estruturação e criação de uma solução de gestão](monitoring-solutions-creating.md).
* Saiba como [criar um ficheiro de solução](monitoring-solutions-solution-file.md).
* [Adicionar alertas e pesquisas guardadas](monitoring-solutions-resources-searches-alerts.md) à sua solução de gestão.
* [Adicionar vistas](monitoring-solutions-resources-views.md) à sua solução de gestão.
* [Adicionar runbooks de automatização e outros recursos](monitoring-solutions-resources-automation.md) à sua solução de gestão.

