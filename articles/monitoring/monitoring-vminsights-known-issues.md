---
title: O Azure Monitor para problemas conhecidos de VMs | Documentos da Microsoft
description: Monitor do Azure para VMs é uma solução no Azure que combina o estado de funcionamento e desempenho de monitorização do sistema de operacional de VM do Azure, bem como detetar automaticamente os componentes da aplicação e dependências com outros recursos e mapeia a comunicação entre -los. Este artigo aborda problemas conhecidos.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/18/2018
ms.author: magoedte
ms.openlocfilehash: 819c3e74355cf80c7a998abb8b02b10c9e077059
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47062773"
---
# <a name="known-issues-with-azure-monitor-for-vms"></a>Problemas conhecidos com o Azure Monitor para VMs

Os seguintes são problemas conhecidos com a funcionalidade de estado de funcionamento do Monitor do Azure para VMs:

- Com esta versão, não não possível modificar o período de tempo e a frequência dos critérios de estado de funcionamento. 
- Critérios de estado de funcionamento não podem ser desativados. 
- Após a integração, pode demorar tempo antes de dados são apresentados no Azure Monitor -> máquinas virtuais -> Estado de funcionamento ou a partir do painel de recursos do VM -> Insights
- Diagnóstico de estado de funcionamento experiência atualizações mais rapidamente do que qualquer exibição, portanto, ao alternar entre modos de exibição pode vir a ter atrasos de informações  
- Resumo de alertas fornecido no Azure Monitor de estado de funcionamento da VM é apenas para alertas acionados para os problemas de estado de funcionamento detetados com VMs do Azure monitorizado.
- O **lista de alertas** vista cuja condição do monitor de alertas de página no único mostra a VM e o Azure Monitor estiver definida para "acionada" nos últimos 30 dias.  Não são configuráveis. No entanto, depois de clicar em Resumo, uma vez a **lista de alerta** página de exibição é iniciada, pode alterar o valor do filtro de ambos os o monitor condição e o tempo de intervalo.
- No **lista de alertas** página de exibição, sugerimos que não modifica a **tipo de recurso**, **recursos**, e **serviço Monitor** filtros à medida que foram configuradas específicas para a solução (esta vista de lista mostra que alguns campos adicionais em comparação com o Azure monitor -> Vista de lista de alertas).    
- Em VMs do Linux **diagnóstico de estado de funcionamento** view tem o nome de domínio completo da VM em vez do nome VM definido pelo utilizador.
- Encerrar as VMs serão atualizados alguns dos respetivos critérios de estado de funcionamento para um estado crítico e outras pessoas para um bom estado de funcionamento com o estado de rede da VM num estado crítico.
- Não é possível alterar a gravidade do alerta de estado de funcionamento, eles só podem ser ativados ou desativados.  Além disso, algumas actualizações gravidades com base no estado de critérios de estado de funcionamento.
- Modificar qualquer definição de uma instância de critério de estado de funcionamento, levará a modificação da mesma definição de todas as instâncias de critérios de estado de funcionamento do mesmo tipo na VM. Por exemplo, se o limiar do disco livre de instância de critério de estado de funcionamento do espaço correspondente ao disco lógico é modificado c:, então este limiar será aplicada a todos os outros discos lógicos detetados e monitorizados para a mesma VM.   
- Os limites de alguns critérios de estado de funcionamento do Windows, como o estado de funcionamento de serviço de cliente de DNS não são fáceis de serem modificados, uma vez que o respetivo bom estado de funcionamento já está bloqueado para o **em execução**, **disponível** estado do serviço ou da entidade consoante o contexto.  Em vez disso, o valor é representado pelo número 4, ela será convertida para a cadeia de apresentação real numa versão futura.  
- Os limites de alguns critérios de estado de funcionamento do Linux não são fáceis de serem modificados, como o estado de funcionamento de disco lógico, como eles já estão definidos para acionar em mau estado de funcionamento.  Estas marcas indicam se algo está online/offline, ou ativada ou desativada e são representados e indicam o mesmo, mostrando o valor de 1 ou 0.
- A atualizar o filtro de grupo de recursos em qualquer grupo de recursos, ao utilizar a escala do Azure monitor -> máquinas virtuais -> Estado de funcionamento -> qualquer vista de lista com o grupo de recursos e subscrição pré-selecionados, fará com que a vista de lista para mostrar **nenhum resultado**.  Vá para o Azure Monitor -> máquinas virtuais -> separador de estado de funcionamento, selecione a subscrição pretendida e um grupo de recursos e, em seguida, navegue para a vista de lista.

## <a name="next-steps"></a>Passos Seguintes
Revisão [carregar Monitor do Azure para VMs](monitoring-vminsights-onboard.md) para compreender os requisitos e métodos para ativar a monitorização das suas máquinas virtuais.