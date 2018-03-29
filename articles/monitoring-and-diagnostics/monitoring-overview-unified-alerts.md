---
title: Explore a nova experiência de alertas no Monitor do Azure | Microsoft Docs
description: Compreender como os novos alertas simples e dimensionáveis no experiência do Azure torna a criação, ver e gerir alertas mais fácil
author: manishsm-msft
manager: kmadnani1
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ''
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2018
ms.author: mamit
ms.custom: ''
ms.openlocfilehash: 356988e8ae743d73c8e2cc7cc106cbc5b0d1a423
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
---
# <a name="the-new-alerts-experience-in-azure-monitor"></a>Experiência de novos alertas no Monitor do Azure

## <a name="overview"></a>Descrição geral
Alertas tem nova experiência. Os alertas mais antigos experiência está agora no separador alertas (clássica). A nova experiência de alertas tem as seguintes vantagens a experiência de alertas (clássica):

 - **Separação de Fired alertas e regras de alerta** - regras de alerta (a definição da condição que aciona um alerta) e que são diferenciadas desencadeado alertas (uma instância de acionadas a regra de alerta), pelo que as vistas operacionais e de configuração são separadas.
 - **A experiência de criação de unified** - todos os alertas de criação com base nas métricas, os registos e a atividade de registo através de Monitor do Azure, análise de registos, e o Application Insights está num único local. 
 - **Desencadeado de vista de alertas de análise de registos no portal do Azure** -pode agora também Consulte desencadeado alertas de análise de registos na sua subscrição. Anteriormente estes foram num portal separado. 
 - **Fluxo de trabalho melhor** - novos alertas guias de experiência de criação do utilizador ao longo do processo de configuração de uma regra de alerta, o que torna mais simples detetar as ações que o direito para receber alertas no.
 

As secções seguintes descrevem como funciona a nova experiência, mais detalhadamente.

## <a name="alert-rules-terminology"></a>Terminologia de regras de alertas
A nova experiência de alertas utiliza os seguintes conceitos para separar os objetos de regra de alerta e o alerta é desencadeado enquanto unificá a experiência de criação em diferentes tipos de alerta.

- **Recurso de destino** -um destino pode ser qualquer recurso do Azure. Recurso de destino define o âmbito e sinais disponíveis para alertas. Destinos de exemplo: uma máquina virtual, uma conta de armazenamento, um conjunto de dimensionamento de máquina virtual, uma área de trabalho de análise de registos ou um recurso do Application Insights.

- **Critérios** - critérios é a combinação de sinal e lógica aplicada no recurso de destino. Exemplos: Percentagem de CPU > 70%, tempo de resposta do servidor > 4 ms, contagem de resultados de um registo de consultar > 100 etc. 

- **Sinal** - sinalizar são emitidos pelo recurso de destino e pode ter vários tipos. **Métrica**, **registo de atividade**, **Application Insights**, e **registo** são suportados tipos de sinal.

- **Lógica** -lógica definida pelo utilizador para verificar se o sinal está dentro do esperado/valores de intervalo.  
 
- **Ação** - uma ação específica tomada quando o alerta é acionado. Por exemplo, relacionada um endereço de e-mail ou chamar um URL do webhook. Várias ações podem ocorrer quando um alerta é acionado. Estes alertas suportam os grupos de ação.  
 
- **Regra de alerta** -a condição que seria acionar o alerta. A regra de alerta captura o destino e os critérios de alerta. A regra de alerta pode ser num ativado ou desativado.
 
    > [!NOTE]
    > Isto é diferente da experiência de alertas (clássica) em que o alerta representa a regra e o desencadeou o alerta e, por conseguinte, pode ser um dos Estados de aviso, Active Directory ou desativado.
    >

## <a name="single-place-to-view-and-manage-alerts"></a>Único local para ver e gerir alertas
O objetivo da experiência de alertas está a ser o único local para ver e gerir todos os alertas do Azure. As subsecções seguintes descrevem as funções de cada ecrã individuais da nova experiência.

### <a name="alerts-overview-page"></a>Página de descrição geral de alertas
**Monitor - alertas** página Descrição geral apresenta o resumo de agregados de todos os alertas fired e configurado/ativa regras de alerta de total. Também mostra uma lista de todos os alertas fired. Alterar as subscrições ou parâmetros de filtro atualiza as agregações e os alertas desencadeado lista.

> [!NOTE]
> É desencadeadas alertas apresentadas alertas estão limitadas a métrica suportada e alertas de registo de atividade; Descrição geral do Azure do Monitor mostra a contagem de alertas é desencadeadas, incluindo as existentes na alertas mais antigos do Azure

 ![alerts-overview](./media/monitoring-overview-unified/alerts-preview-overview.png) 

### <a name="alert-rules-management"></a>Gestão de regras de alertas
**Monitor - alertas > regras** é uma única página para gerir todas as regras de alerta nas suas subscrições do Azure. Lista todas as regras de alertas (ativado ou desativado) e podem ser ordenados com base nos recursos de destino, os grupos de recursos, o nome da regra ou estado. Regras de alertas também podem ser ativado/desativado ou editá-lo a partir desta página.  

 ![regras de alertas](./media/monitoring-overview-unified/alerts-preview-rules.png)


## <a name="one-alert-authoring-experience-across-all-monitoring-sources"></a>Um alerta a experiência de criação em todas as origens de monitorização
Na nova experiência de alertas, os alertas podem ser criados de forma consistente, independentemente do serviço de monitorização ou assinalam o tipo. É desencadeado todos os alertas e detalhes relacionados estão disponíveis na página único.  
 
Criação de um alerta é uma tarefa de três passos em que o utilizador escolhe primeiro um destino para o alerta, seguido de selecionar o sinal à direita e, em seguida, especificar a lógica para ser aplicado o sinal como parte da regra de alerta. Este processo de criação simplificado já não exige que o utilizador saber a origem de monitorização ou sinais suportados antes de selecionar um recurso do Azure. A experiência de criação comuns automaticamente filtra a lista de sinais disponíveis com base no recurso de destino seleccionado e orienta a criação de lógica de alerta

Pode saber mais sobre como criar tipos de alerta seguintes [aqui](monitor-alerts-unified-usage.md).
- Alertas de Métricas
- Alertas de registo (Log Analytics)
- Alertas de registo (registos de atividade)
- Alertas de registo (Application Insights)

 

## <a name="alert-types-supported"></a>Tipos de alertas suportados


| **Tipo de sinal** | **Monitor de origem** | **Descrição** | 
|-------------|----------------|-------------|
| Métrica | Monitor do Azure | Também denominado [ **alertas de métrica de quase em Tempo Real**](monitoring-near-real-time-metric-alerts.md), estes alertas métricas suportam avaliar condições métricas com uma frequência de 1 min e permitir a métricos de várias regras. Uma lista de tipos de recurso suportados está disponível [aqui](monitoring-near-real-time-metric-alerts.md#metrics-and-dimensions-supported). Alertas de métrica mais antiga, conforme definido [aqui](monitoring-overview-alerts.md#alerts-in-different-azure-services) não são suportados na nova experiência de alertas. Pode encontrá-los em alertas (clássica)|
| Registos  | Log Analytics | Receber notificações ou executar ações automatizadas quando uma consulta de pesquisa de registo através de dados de métrica de e/ou eventos cumpra determinados critérios.|
| Registo de Atividades | Registos de Atividade | Esta categoria contém os registos de todos os criar, atualizar e eliminar ações efetuadas através de destino selecionada (grupo de recursos/recursos/subscrição). |
| Registos  | Registos do serviço de estado de funcionamento | Não é suportada na experiência de alertas.   |
| Registos  | Application Insights | Esta categoria contém registos com os detalhes de desempenho da sua aplicação. Utilizar consulta de análise, pode definir as condições para as ações a ser executadas - com base nos dados de aplicação. |
| Métrica | Application Insights | Não é suportada na experiência de alertas. Vai encontrá-los em alertas (clássica) |
| Testes de disponibilidade | Application Insights | Não é suportada na experiência de alertas. Vai encontrá-los em alertas (clássica) |


## <a name="next-steps"></a>Passos Seguintes
- [Saiba como utilizar a nova experiência de alertas para criar, ver e gerir alertas](monitor-alerts-unified-usage.md)
- [Saiba mais sobre alertas de registo na experiência de alertas](monitor-alerts-unified-log.md)
- [Saiba mais sobre alertas de métricas de experiência de alertas](monitoring-near-real-time-metric-alerts.md)
- [Saiba mais sobre alertas de registo de atividade numa experiência de alertas](monitoring-activity-log-alerts-new-experience.md)
