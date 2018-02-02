---
title: "Adicione as métricas expandidas para máquinas virtuais do Azure | Microsoft Docs"
description: "Este artigo ajuda-o a ativar e configurar as métricas de diagnóstico expandida para as suas VMs do Azure."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 01/30/2018
ms.topic: article
ms.service: cost-management
manager: carmonm
ms.custom: 
ms.openlocfilehash: 91797aaab1dca96e78643f57776eb16d336e894b
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/01/2018
---
# <a name="add-extended-metrics-for-azure-virtual-machines"></a>Adicione as métricas expandidas para máquinas virtuais do Azure

Custo de gestão utiliza dados do Azure de métricos de suas VMs do Azure para mostrar que informações detalhadas sobre os seus recursos. Os dados métricos, também denominados contadores de desempenho, são utilizados pela gestão de custos para gerar relatórios. No entanto, custo de gestão não automaticamente recolher todos os dados do Azure de métricos de VMs de convidado — tem de ativar a coleção de métrica. Este artigo ajuda-o a ativar e configurar as métricas de diagnóstico adicionais para as suas VMs do Azure.

Depois de ativar a coleção de métrica, pode:

- Sabe quando as suas VMs são atingir os seus memória, disco e limites de CPU.
- Detete tendências de utilização e anomalias.
- Controla os custos ao dimensionamento de acordo com a utilização.
- Obter o custo Efetivo recomendações de Otimização da gestão de custo de dimensionamento.

Por exemplo, pode querer monitorizar a % de CPU e memória % das suas VMs do Azure. As métricas de VM do Azure correspondem ao _[anfitrião] percentagem de CPU_ e _percentagem de memória [convidada]_.

## <a name="verify-that-metrics-are-enabled-on-vms"></a>Certifique-se de que as métricas estão ativadas em VMs

1. Inicie sessão no portal do Azure em http://portal.azure.com.
2. Em **máquinas virtuais**, selecione uma VM e, em **monitorização**, selecione **métricas**. É apresentada uma lista de métricas disponíveis.
3. Selecione algumas métricas e um gráfico apresenta dados para os mesmos.  
    ![Métrica de exemplo – da CPU de percentagem de anfitrião](./media/azure-vm-extended-metrics/metric01.png)

No exemplo anterior, um conjunto limitado de métricas padrão estão disponíveis para os anfitriões, mas não são métricas de memória. Métricas de memória são parte das métricas expandidas. Tem de efetuar alguns passos adicionais para ativar as métricas expandidas. As seguintes informações orienta-o ativá-las.

## <a name="enable-extended-metrics-in-the-azure-portal"></a>Ativar as métricas expandidas no portal do Azure

Métricas padrão são métricas de computador do anfitrião. O _[anfitrião] percentagem de CPU_ métrica é um exemplo. Também existem básicas métricas para VMs de convidado e está a também denominados métricas expandidas. Exemplos de métricas expandidas _percentagem de memória [convidada]_ e _[convidado] memória disponível_.

Ativar métricas expandidas é simples. Para cada VM, ative a monitorização de ao nível do convidado. Quando ativar a monitorização de ao nível do convidado, o agente de diagnóstico do Azure está instalado na VM. O seguinte processo é o mesmo para as VMs clássicas e regulares e o mesmo para o Windows e VMs com Linux.

### <a name="enable-guest-level-monitoring-on-existing-vms"></a>Ativar a monitorização de ao nível do convidado em VMs existentes

1. No **máquinas virtuais**, ver a lista das suas VMs e, em seguida, selecione uma VM.
2. Em **monitorização**, selecione **métricas**.
3. Clique em **definições de diagnóstico**.
4. Na página de definições de diagnóstico, clique em **ativar a monitorização de ao nível do convidado**. VMs com Linux necessitam de uma conta de armazenamento existente. Se não escolher uma conta de armazenamento para uma VM do Windows, em seguida, é criada uma por si.  
    ![Ativar a monitorização de nível do convidado](./media/azure-vm-extended-metrics/enable-guest-monitoring.png)
5. Após alguns minutos, o agente de diagnóstico do Azure está instalado na VM. Atualize a página e a lista de métricas disponíveis é atualizada com a métrica de convidado.  
    ![Métricas expandidas](./media/azure-vm-extended-metrics/extended-metrics.png)

### <a name="enable-guest-level-monitoring-on-new-vms"></a>Ativar a monitorização de ao nível do convidado em novas VMs

Quando criar novas VMs, certifique-se de que seleciona **diagnósticos de SO convidado**. VMs com Linux necessitam de uma conta de armazenamento existente. Se não escolher uma conta de armazenamento para uma VM do Windows, em seguida, é criada uma por si.

![Ative os diagnósticos de SO convidado](./media/azure-vm-extended-metrics/new-enable-diag.png)

## <a name="resource-manager-credentials"></a>Credenciais do Gestor de recursos

Depois de ativar as métricas expandidas, certifique-se de que o custo de gestão tem acesso à sua [credenciais do Gestor de recursos](activate-subs-accounts.md). As suas credenciais são necessárias para a gestão de custos recolher e apresentar dados de desempenho para as suas VMs. São também utilizados para criar as recomendações de otimização de custos. Custo de gestão tem, pelo menos, três dias de dados de desempenho de uma instância para determinar se é uma candidata para uma recomendação downsizing.

## <a name="enable-vm-metrics-with-a-script"></a>Ativar as métricas VM com um script

Pode ativar as métricas VM com scripts do PowerShell do Azure. Quando tiver várias VMs que pretende ativar as métricas no, pode utilizar um script para automatizar o processo. São scripts de exemplo no GitHub em [Azure ativar diagnósticos](https://github.com/Cloudyn/azure-enable-diagnostics).

## <a name="view-azure-performance-metrics"></a>Veja as métricas de desempenho do Azure

Para ver métricas de desempenho nas instâncias do Azure no portal do Cloudyn, navegue até à **ativos** > **computação** > **Explorador de instância**. Na lista de instâncias de VM, expanda uma instância e, em seguida, expanda um recurso para ver os detalhes.

![Explorador de instância](./media/azure-vm-extended-metrics/instance-explorer.png)

## <a name="next-steps"></a>Passos Seguintes

- Se já ainda não ativou o acesso à API do Azure Resource Manager para as contas, avance para [subscrições do Azure ativar e contas](activate-subs-accounts.md)
