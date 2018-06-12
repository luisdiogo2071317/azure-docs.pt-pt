---
title: Criar alertas com limiares dinâmicos no Monitor do Azure
description: Criar alertas com limiares dinâmico de machine learning
author: antonfrMSFT
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/27/2018
ms.author: mbullwin
ms.reviewer: antonfr
ms.component: alerts
ms.openlocfilehash: 01f924e0b3a2976a3f537cb5acac842eeeaccb4b
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35263318"
---
# <a name="alerts-with-dynamic-thresholds-in-azure-monitor-limited-public-preview"></a>Alertas com limiares dinâmicas no Monitor do Azure (limitado pré-visualização pública)

Os alertas com limiares dinâmicos são uma melhoria Azure métrica alertas no Monitor do Azure, tirar partido das capacidades avançadas de Machine Learning (ML) para saber o comportamento de históricos dos métricas calcular automaticamente linhas de base e utilizá-los como limiares de alertas.

Os benefícios da utilização de limiares de dinâmicos são:

- Guarde o hassle associado a definir um limite de rigid predefinido, como o monitor automaticamente aprende o desempenho da métrica histórico e aplica-se de algoritmos de ML para determinar os limiares de alertas.
- Estes podem identificar sazonais comportamento e o alerta apenas em desvios o comportamento de sazonais esperado. Métricas alertas com limiares dinâmicos não irão acionar se o serviço está inativo regularmente na fim de semana e, em seguida, spikes cada segunda-feira. Atualmente, suportado: sazonalidade horária, diária e semanal.
- Continuamente aprende o desempenho de métrico e é adaptável a métricas alterações.

Alertas dinâmicas baseadas em limiares estão disponíveis para todos os monitor do Azure com base no origens métricas listadas neste [artigo](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts#what-resources-can-i-create-near-real-time-metric-alerts-for).

## <a name="sign-up-to-access-the-preview"></a>Inscreva-se para aceder a pré-visualização

A efetuar esta capacidade para uma giratórias [Inscreva-se a pré-visualização](http://aka.ms/DynamicThresholdMetricAlerts). Como sempre, gostaríamos de ouvir os seus comentários, mantenha-a disponível em [azurealertsfeedback@microsoft.com](mailto:azurealertsfeedback@microsoft.com)

## <a name="how-to-configure-alerts-with-dynamic-thresholds"></a>Como configurar alertas com limiares dinâmicos

Alertas com limiares dinâmicas podem ser configuradas através de alertas no Monitor do Azure

![Pré-visualização de alertas](./media/monitoring-alerts-dynamic-thresholds/0001.png)

## <a name="creating-an-alert-rule-with-dynamic-thresholds"></a>Criar uma regra de alerta com limiares dinâmicos

1. A partir do painel de alertas no Monitor, selecione o **nova regra de alerta** botão para criar um novo alerta no Azure.

   ![Nova Regra de Alerta](./media/monitoring-alerts-dynamic-thresholds/002.png)

2. A secção de regra de criação é apresentada com três partes consiste em: _definir a condição de alerta_, _definir os detalhes do alerta_, e _grupo de ação de definir_. Primeiro começar com o _definir a condição de alerta_ secção utilização o **selecionar destino** ligação para especificar o destino, selecionando um recurso. Depois de um recurso apropriado é escolhido, clique no botão efectuado.

   ![Selecione o destino](./media/monitoring-alerts-dynamic-thresholds/0003.png)

3. Em seguida utilize o **adicionar critérios** botão para ver uma lista de opções de sinal disponíveis para o recurso e a lista de sinal escolha apropriadas **métrica** opção. (Por exemplo, percentagem de CPU.)

   ![Adicionar critérios](./media/monitoring-alerts-dynamic-thresholds/004.png)

4. No ecrã Configurar Sinal lógica, na secção de lógica de alerta tem a opção para mudar a condição para um tipo de dinâmico, que irá gerar automaticamente os limiares dinâmicos (linhas vermelhos) juntamente com a métrica (azul linha).

   ![Dinâmica](./media/monitoring-alerts-dynamic-thresholds/005.png)

5. Os limiares de apresentação do gráfico são calculados com base nos últimos sete dias de dados históricos, assim que for criado um alerta, os limiares dinâmicos vão adquirir dados históricos adicionais que está disponíveis em continuamente ficará a saber com base em novos dados para se certificar a limiares mais exatos.

6. Definições de lógica de alerta adicionais:
   - Condição - pode escolher o alerta ser acionado dos três seguintes condições:
       - Superior ao limiar superior ou inferior ao limiar mais baixo (predefinição)
       - Superior ao limiar superior
       - Inferior ao limiar inferior.
   - Agregação de tempo: média (predefinição), sum, min, max.
   - Sensibilidade de alerta:
       - Elevado – mais alertas, como o alerta será acionado no desvio mais pequeno.
       - MED-menos confidencial à alta, menos alertas que com alta sensibilidade (predefinição)
       - Baixo – o limiar menos confidencial.

    ![Definições de lógica de alertas](./media/monitoring-alerts-dynamic-thresholds/00007.png)

7. Avaliada com base em:
    -  Que duração de tempo, o alerta deve procurar a condição especificada, seleccionando o **período**.

    ![Avaliada com base em](./media/monitoring-alerts-dynamic-thresholds/007.png)

   > [!NOTE]
   > Período valores suportados: 5 minutos, 10 minutos, 30 minutos e 1 hora.

   Para reduzir o ruído de alertas gerado pelo picos transitórios, recomendamos que utilize as definições "Número de violações para acionar o alerta". Esta funcionalidade permite-lhe obter um alerta apenas se o limiar foi violado X vezes consecutivas ou Y vezes sem períodos de Z último. Por exemplo:

    Para acionar um alerta quando o problema é contínuo para 15 minutos, 3 vezes consecutivas num determinado período de 5 minutos, utilize as seguintes definições:

   ![Avaliada com base em](./media/monitoring-alerts-dynamic-thresholds/0008.png)

    Para acionar um alerta quando ocorreu uma violação de um limiar dinâmico em 15 minutos fora últimos 30 minutos, com um período de 5 minutos, utilize as seguintes definições:

   ![Avaliada com base em](./media/monitoring-alerts-dynamic-thresholds/0009.png)

8. Os utilizadores podem tem actualmente alertas com os critérios de limiar dinâmico como um único critério.

   ![Criar regra](./media/monitoring-alerts-dynamic-thresholds/010.png)

## <a name="q--a"></a>P&R

- P: se a métrica lentamente alterações ao longo do tempo, esta irá acionar um alerta com limiares dinâmicos?

- R: provavelmente não. Limiares de dinâmicos são ideais para detetar desvios significativos em vez de evolução lentamente de problemas.

- P: pode configurar limiares dinâmicos através de uma API?

- R: estamos a trabalhar no mesmo.
