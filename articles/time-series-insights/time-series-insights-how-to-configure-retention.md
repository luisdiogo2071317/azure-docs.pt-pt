---
title: Como configurar a retenção no seu ambiente de informações de séries de tempo do Azure | Microsoft Docs
description: Este artigo descreve como configurar a retenção no seu ambiente de informações de séries de tempo do Azure.
ms.service: time-series-insights
services: time-series-insights
author: anshan
ms.author: anshan
manager: kfile
ms.reviewer: jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 02/09/2018
ms.openlocfilehash: 9a236ba500af36e8fb1c8c089389ffd74b9d2abb
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34653924"
---
# <a name="configuring-retention-in-time-series-insights"></a>Configurar a retenção de informações de séries de tempo
Este artigo descreve como configurar **período de retenção de dados** e **excedido o limite de armazenamento do comportamento** nas informações de séries de tempo do Azure.

Cada ambiente de informações de séries de tempo (TSI) tem uma definição para configurar **período de retenção de dados**. Abrange o valor de 1 a 400 dias. Os dados são eliminados com base no ambiente armazenamento capacidade ou retenção duração (1-400), o que ocorrer primeiro.

Cada ambiente TSI tem uma definição de adicional **excedido o limite de armazenamento do comportamento**. Esta definição controla o comportamento de entrada e de remoção quando for atingida a capacidade máxima de um ambiente. Existem dois comportamentos para escolher entre:
- **Remover dados antigos** (predefinição)  
- **Entrada de pausa**

Para obter informações detalhadas para melhor compreender estas definições, consulte [retenção compreender no Insights de séries de tempo](time-series-insights-concepts-retention.md).  

## <a name="configure-data-retention"></a>Configurar a retenção de dados

1. Inicie sessão no [portal do Azure](https://portal.azure.com).

2. Localize o seu ambiente existente do Insights de séries de tempo. Selecione **todos os recursos** no menu no lado esquerdo do portal do Azure. Selecione o seu ambiente do Time Series Insights.

3. Sob o **definições** cabeçalho, selecione **configurar**.

4. Selecione o **período de retenção de dados** para configurar a retenção utilizando a barra do controlo de deslize ou escreva um número na caixa de texto.

5. Tenha em atenção o **capacidade** definição, uma vez que esta configuração tem impacto sobre a quantidade máxima de eventos de dados e a capacidade de armazenamento total para armazenar dados. 

6. Ativar/desativar a **excedido o limite de armazenamento do comportamento** definição. Selecione **remover dados antigos** ou **colocar em pausa entrada** comportamento.

7. Selecione **guardar** para configurar as alterações.

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações, consulte [retenção compreender no Insights de séries de tempo](time-series-insights-concepts-retention.md).
