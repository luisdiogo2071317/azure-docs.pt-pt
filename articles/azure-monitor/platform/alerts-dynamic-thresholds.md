---
title: Criar alertas com limiares dinâmicos no Azure Monitor
description: Criar alertas com limiares de dinâmica de machine learning
author: yanivlavi
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 11/29/2018
ms.author: Yaniv.Lavi
ms.reviewer: mbullwin
ms.openlocfilehash: b9b56145c13cb83b1686004ce215b1960caced14
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/10/2018
ms.locfileid: "53183000"
---
# <a name="alerts-with-dynamic-thresholds-in-azure-monitor-limited-private-preview"></a>Alertas com limiares dinâmicos no Azure Monitor (pré-visualização privada limitado)

Alertas com limiares dinâmicos são um melhoramento a alertas de métrica do Azure no Azure Monitor, que tiram partido de capacidades avançadas de Machine Learning (ML) para obter informações sobre comportamento de históricos dos métricas para calcular automaticamente as linhas de base e utilizá-los como limiares de alertas.

Os benefícios da utilização de limiares dinâmicos são:

- Guarde as complicações associadas à configuração um limite rígido predefinido, como o monitor automaticamente aprende o desempenho histórico da métrica e aplica-se de algoritmos de ML para determinar os limiares de alertas.
- Eles podem identificar comportamento sazonal e alerta apenas sobre os desvios do comportamento esperado de sazonal. Alertas de métricas com limiares dinâmicos não irão acionar se o seu serviço está inativo regularmente aos fins de semana e, em seguida, picos sempre de segunda a. Atualmente suportados: sazonalidade por hora, diária e semanal.
- Aprende o desempenho de métrica e é adaptável a métrica alterações continuamente.

Alertas baseados no limiar dinâmicas estão disponíveis para todos os do Azure monitor com base em métricas origens demonstradas neste [artigo](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts#what-resources-can-i-create-near-real-time-metric-alerts-for).

## <a name="sign-up-to-access-the-preview"></a>Inscreva-se para acessar a pré-visualização

Para tirar esta capacidade para uma volta [Inscreva-se na pré-visualização](https://aka.ms/DynamicThresholdMetricAlerts). Como sempre, gostaríamos muito de ouvir os seus comentários, mantenha-a disponível em [azurealertsfeedback@microsoft.com](mailto:azurealertsfeedback@microsoft.com)

## <a name="how-to-configure-alerts-with-dynamic-thresholds"></a>Como configurar alertas com limiares dinâmicos

Alertas com limiares dinâmicos podem ser configurados através de alertas no Azure Monitor

![Pré-visualização de alertas](media/alerts-dynamic-thresholds/0001.png)

## <a name="creating-an-alert-rule-with-dynamic-thresholds"></a>Criar uma regra de alerta com limiares dinâmicos

1. No painel de alertas em Monitor, selecione o **nova regra de alerta** botão para criar um novo alerta no Azure.

   ![Nova Regra de Alerta](media/alerts-dynamic-thresholds/002.png)

2. A secção de regra de criar é mostrada com três partes consiste em: _Definir a condição do alerta_, _definir detalhes do alerta_, e _Definir grupo de ação_. Em primeiro lugar para começar, o _definir a condição de alerta_ secção utilizam a **selecionar destino** link para especificar o destino, ao selecionar um recurso. Depois de um recurso apropriado for escolhido, clique no botão concluído.

   ![Selecionar o Destino](media/alerts-dynamic-thresholds/0003.png)

3. Em seguida utilize o **adicionar critérios** botão para ver uma lista de opções de sinal disponíveis para o recurso e a partir da lista de sinal escolha apropriadas **métrica** opção. (Por exemplo, percentagem de CPU.)

   ![Adicionar critérios](media/alerts-dynamic-thresholds/004.png)

4. No ecrã de lógica de sinal de configurar, na seção de lógica de alerta tem a opção para mudar a condição a um tipo de dinâmico, que irá gerar automaticamente os limiares dinâmicos (linhas vermelhas) juntamente com a métrica (linha azul).

   ![Dinâmica](media/alerts-dynamic-thresholds/005.png)

5. Os limiares que aparece no gráfico são calculados com base nos últimos sete dias de dados históricos, assim que for criado um alerta, os limiares dinâmicos vão adquirir dados históricos adicionais que estão disponíveis e aprenderá continuamente com base nos dados de novo para tornar o limiares mais precisas.

6. Definições de lógica de alerta adicional:
   - Condição - pode escolher o alerta das três condições a seguir:
       - Superior ao limiar superior ou inferior ao limiar mais baixo (predefinição)
       - Superior ao limiar superior
       - Inferior ao limiar mais baixo.
   - Agregação de tempo: Média (predefinição), sum, min, max.
   - Sensibilidade de alerta:
       - Alta – mais alertas, como o alerta será acionado no desvio mais pequeno.
       - MED – menos sensíveis que alta, menos alertas que com alta sensibilidade (predefinição)
       - Baixa – o limiar de menos confidencial.

    ![Definições de lógica de alerta](media/alerts-dynamic-thresholds/00007.png)

7. Avaliada com base em:
    -  O período de tempo, o alerta deve procurar a condição especificada por escolher o **período**.

    ![Avaliada com base em](media/alerts-dynamic-thresholds/007.png)

   > [!NOTE]
   > Valores suportados: cinco minutos, 10 minutos, 30 minutos e 1 hora.

   Para reduzir o ruído de alertas gerado pelo picos transitórios, recomendamos que utilize as definições de "Número de violações para acionar o alerta". Esta funcionalidade permite que recebe um alerta apenas se o limiar foi violado X vezes consecutivas ou tempos de Y fora últimos períodos de Z. Por exemplo:

    Para acionar um alerta quando o problema é contínuo para 15 minutos, a 3 vezes consecutivas num determinado período de 5 minutos, utilize as seguintes definições:

   ![Avaliada com base em](media/alerts-dynamic-thresholds/0008.png)

    Para acionar um alerta quando ocorreu uma violação de um limiar dinâmico em 15 minutos sem os últimos 30 minutos com o período de 5 minutos, utilize as seguintes definições:

   ![Avaliada com base em](media/alerts-dynamic-thresholds/0009.png)

8. Atualmente os usuários podem ter alertas com os critérios de limiar dinâmico como um critério exclusivo.

   ![Criar regra](media/alerts-dynamic-thresholds/010.png)

## <a name="q--a"></a>P&R

- P: Se a métrica lentamente mudar ao longo do tempo, isto irá acionar um alerta com limiares dinâmicos?

- R: Provavelmente não. Limiares dinâmicos são bons para detetar desvios significativos, em vez de lentamente evoluindo problemas.

- P: Pode configurar limiares dinâmicos através de uma API?

- R: Estamos a trabalhar no mesmo.
