---
title: Compreender a retenção de dados no seu ambiente de informações de séries de tempo do Azure | Microsoft Docs
description: Este artigo descreve as duas definições que controlam a retenção de dados no seu ambiente de informações de séries de tempo do Azure.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: kfile
ms.reviewer: jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 02/09/2018
ms.openlocfilehash: c8fad0d626fc0b9754f7f04c8000f48976eb766b
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36293100"
---
# <a name="understand-data-retention-in-time-series-insights"></a>Compreender a retenção de dados no Insights de séries de tempo
Este artigo descreve as duas definições que afetam a retenção de dados no seu ambiente de informações de séries de tempo (TSI).

Cada ambiente TSI tem uma definição que controla **período de retenção de dados**. Abrange o valor de 1 e 400 dias. Os dados são eliminados com base no ambiente armazenamento capacidade ou retenção duração (1-400), o que ocorrer primeiro.

Cada ambiente TSI tem uma definição de adicional **excedido o limite de armazenamento do comportamento**. Esta definição controla o comportamento de entrada e de remoção quando for atingida a capacidade máxima de um ambiente. Existem dois comportamentos para escolher entre:
- **Remover dados antigos** (predefinição)  
- **Entrada de pausa**

> [!NOTE]
> Por predefinição, quando criar um novo ambiente, a retenção está configurada para **remover dados antigos**. Esta definição pode ser ativada, conforme necessário após a hora de criação no portal do Azure, no **configurar** página do ambiente TSI.

Para informações sobre a mudança comportamentos de retenção, reveja [configurar a retenção de informações de séries de tempo](time-series-insights-how-to-configure-retention.md).

Compare o comportamento de retenção de dados:

## <a name="purge-old-data"></a>Remover dados antigos
- Este comportamento é o comportamento predefinido para ambientes de TSI e exhibits mesmos ambientes TSI comportamento tem baixos apresentados uma vez que o se iniciadas na pré-visualização pública.  
- Este comportamento é preferencial quando os utilizadores querem sempre ver os respetivos *dados mais recentes* no respetivo ambiente TSI. 
- Este comportamento *elimina* dos dados de uma vez o ambiente de limites (período de retenção, tamanho ou contagem, o que ocorrer primeiro) são atingidos. Retenção está definida como 30 dias por predefinição. 
- Os dados de transmissões em mais antigos são removidos primeiro (abordagem FIFO).

### <a name="example-1"></a>Exemplo 1:
Considere um ambiente de exemplo com o comportamento de retenção **continuar a entrada e remover dados antigos**: neste exemplo, **período de retenção de dados** está definida para dias 400. **Capacidade** está definido como uma unidade S1, que contém 30 GB da capacidade total.   Vamos assumir que a entrada dados acumulam-se para 500 MB cada dia em média. Este ambiente só pode reter 60 dias, mas dos dados fornecidos a taxa de dados de entrada, uma vez que for atingida a capacidade máxima em 60 dias. Os dados de entrada acumula como: 500 MB cada dia x 60 dias = 30 GB. 

Neste exemplo, no dia 61st, o ambiente mostra os dados mais recente, mas elimina os dados mais antigos, com mais de 60 dias. A remoção faz com que o espaço para os novos dados de transmissão em fluxo, para que os novos dados podem continuar a ser explorou. 

Se o utilizador pretende manter os dados do tempo, que podem aumentar o tamanho do ambiente, adicionando unidades adicionais ou podem enviar menos dados.  

### <a name="example-2"></a>Exemplo 2:
Considere um ambiente configurado também o comportamento de retenção **continuar a entrada e remover dados antigos**. Neste exemplo, **período de retenção de dados** está definido para um valor inferior de 180 dias. **Capacidade** está definido como uma unidade S1, que contém 30 GB da capacidade total. Para armazenar dados para os completos de 180 dias, a entrada de diária não pode exceder 0.166 GB (166 MB) por dia.  

Sempre que a taxa de entrada de diário neste ambiente excede de 0.166 GB por dia, os dados não podem ser armazenados durante 180 dias, uma vez que alguns dados obtém removidos. Considere este mesmo ambiente durante um período de tempo ocupado. Assumir que a taxa de entrada do ambiente pode aumentar para um GB 0.189 média por dia. Em que período de tempo ocupado, cerca de 158 dias de dados são mantidos (30GB/0.189 = 158.73 dias de retenção). Este tempo é menor que o intervalo de tempo de retenção de dados pretendido.

## <a name="pause-ingress"></a>Entrada de pausa
- Este comportamento é concebido para garantir que os dados não são removidos se os limites de tamanho e a contagem são atingidos antes do respetivo período de retenção.  
- Este comportamento fornece mais tempo para que os utilizadores aumenta a capacidade do seu ambiente antes dos dados são removidos devido a ser, de período de retenção
- Este comportamento ajuda a proteger contra perda de dados, mas cria uma oportunidade para a perda de dados mais recentes, se a entrada está em pausa para além do período de retenção da sua origem de evento.
- No entanto, quando for atingida a capacidade máxima de um ambiente, o ambiente interrompe a entrada de dados enquanto ocorrem ações adicionais: 
   - Aumentar a capacidade máxima do ambiente. Para obter mais informações, consulte [como dimensionar o ambiente de tempo série Insights](time-series-insights-how-to-scale-your-environment.md) para adicionar mais unidades de escala.
   - O período de retenção de dados seja atingido e os dados são removidos, colocar, por conseguinte, o ambiente abaixo a capacidade máxima.

### <a name="example-3"></a>Exemplo 3:
Considere um ambiente com o comportamento de retenção configurado para **colocar em pausa entrada**. Neste exemplo, o **período de retenção de dados** está configurado para 60 dias. **Capacidade** está definido para 3 unidades de S1. Assumir que este ambiente tem de entrada de 2 GB de dados por dia. Neste ambiente, entrada está em pausa assim que a capacidade máxima for atingida. Nessa altura, o ambiente mostra o mesmo conjunto de dados até que retoma de entrada ou até 'continue a entrada' está ativada (o que pretende remover dados antigos para disponibilizar espaço para novos dados). 

Quando a entrada retoma:
- Fluxos de dados na ordem que foi recebido pela origem de evento
- Os eventos são indexados com base no respetivo timestamp, a menos que tenha excedido políticas de retenção na sua origem de evento. Para obter mais informações sobre a configuração de retenção de origem do evento, [FAQ de Hubs de eventos](../event-hubs/event-hubs-faq.md)

> [!IMPORTANT]
> Deve definir alertas de aviso para ajudar a evitar a entrada que está a ser colocada em pausa. Perda de dados é possível uma vez que o período de retenção predefinido é 1 dia para origens de eventos do Azure. Por conseguinte, depois de entrada está em pausa, provavelmente, perderá os dados mais recentes, a menos que é necessária nenhuma ação adicional. Tem de aumentar a capacidade ou mudar o comportamento para **remover dados antigos** para evitar a perda de dados possível.

Os Event Hubs afetados, considere ajustar o **mensagem retenção** propriedade para minimizar a perda de dados quando ocorre a entrada de colocar em pausa no Insights de séries de tempo.

![Retenção de mensagem de hub de eventos.](media/time-series-insights-contepts-retention/event-hub-retention.png)

Se não existem propriedades são configuradas na origem de evento (timeStampPropertyName), será assumida TSI a timestamp da chegada ao hub de eventos, como o eixo x. Se timeStampPropertyName estiver configurado para ser algo diferentes, o ambiente de procura o timeStampPropertyName configurado no pacote de dados quando os eventos são analisados. 

Se precisar de dimensionar o seu ambiente para acomodar capacidade adicional ou para aumentar o período de retenção, consulte [como dimensionar o ambiente de tempo série Insights](time-series-insights-how-to-scale-your-environment.md) para obter mais informações.  

## <a name="next-steps"></a>Passos Seguintes
Para informações sobre o comportamento de retenção da mudança, reveja [configurar a retenção de informações de séries de tempo](time-series-insights-how-to-configure-retention.md).
