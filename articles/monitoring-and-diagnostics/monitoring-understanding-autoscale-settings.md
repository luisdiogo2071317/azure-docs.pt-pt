---
title: "Compreender as definições de dimensionamento automático | Microsoft Docs"
description: "Uma divisão detalhada das definições de dimensionamento automático e como funcionam."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ce2930aa-fc41-4b81-b0cb-e7ea922467e1
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/18/2017
ms.author: ancav
ms.openlocfilehash: cff2be1818417a19f36da08d8c2eaa227bb945ec
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/20/2017
---
# <a name="understand-autoscale-settings"></a>Compreender as definições de dimensionamento automático
Definições de dimensionamento automático permitem-lhe para se certificar de que tem a quantidade certa de recursos em execução para processar a carga fluctuating da sua aplicação. Pode configurar as definições de dimensionamento automático para ser acionado com base nas métricas que indicam o carregamento ou de desempenho ou acionam uma hora e data agendada. Este artigo assume uma detalhadas vista de olhos anatomy de uma definição de dimensionamento automático. O artigo começa por compreender o esquema e propriedades de uma definição, em seguida, explica os tipos de perfil diferentes que podem ser configurados e, finalmente, descreve como o dimensionamento automático avalia que perfil de executar em qualquer momento.

## <a name="autoscale-setting-schema"></a>Esquema de definição de dimensionamento automático
Para ilustrar o esquema de definição de dimensionamento automático, é utilizada a seguinte definição de dimensionamento automático. É importante ter em atenção que esta definição de dimensionamento automático não tem:
- Um perfil 
- Tem duas regras métricas neste perfil; uma para Escalamento horizontal e outro para dimensionamento.
- A regra de escalamento horizontal é acionada quando métrica de percentagem de CPU média o conjunto de dimensionamento de máquina virtual for superior a 85% para o mínimo de 10 passado.
- A regra de escala é acionada quando a média do conjunto de dimensionamento de máquina virtual é inferior a 60% para os últimos minutos.

> [!NOTE]
> Uma definição pode ter vários perfis, avance para o [perfis](#autoscale-profiles) secção para saber mais.
> Um perfil também pode ter vários de escalamento horizontal regras e as regras de dimensionamento definidas, avance para o [secção avaliação](#autoscale-evaluation) para ver como são avaliadas

```JSON
{
  "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/autoscalesettings/setting1",
  "name": "setting1",
  "type": "Microsoft.Insights/autoscaleSettings",
  "location": "East US",
  "properties": {
    "enabled": true,
    "targetResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Compute/virtualMachineScaleSets/vmss1",
    "profiles": [
      {
        "name": "mainProfile",
        "capacity": {
          "minimum": "1",
          "maximum": "4",
          "default": "1"
        },
        "rules": [
          {
            "metricTrigger": {
              "metricName": "Percentage CPU",
              "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Compute/virtualMachineScaleSets/vmss1",
              "timeGrain": "PT1M",
              "statistic": "Average",
              "timeWindow": "PT10M",
              "timeAggregation": "Average",
              "operator": "GreaterThan",
              "threshold": 85
            },
            "scaleAction": {
              "direction": "Increase",
              "type": "ChangeCount",
              "value": "1",
              "cooldown": "PT5M"
            }
          },
    {
            "metricTrigger": {
              "metricName": "Percentage CPU",
              "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Compute/virtualMachineScaleSets/vmss1",
              "timeGrain": "PT1M",
              "statistic": "Average",
              "timeWindow": "PT10M",
              "timeAggregation": "Average",
              "operator": "LessThan",
              "threshold": 60
            },
            "scaleAction": {
              "direction": "Decrease",
              "type": "ChangeCount",
              "value": "1",
              "cooldown": "PT5M"
            }
          }
        ]
      }
    ]
  }
}
```

| Section | Nome do elemento | Descrição |
| --- | --- | --- |
| Definição | ID | ID de recurso. a definição de dimensionamento automático Definições de dimensionamento automático são um recurso do Azure Resource Manager. |
| Definição | nome | O nome da definição de dimensionamento automático. |
| Definição | localização | A localização da definição de dimensionamento automático. Esta localização pode ser diferente da localização do recurso que está a ser ampliada. |
| propriedades | targetResourceUri | O ID de recurso do recurso que está a ser ampliado. Só pode ter uma definição de dimensionamento automático por recurso. |
| propriedades | Perfis | Uma definição de dimensionamento automático é composta por um ou mais perfis. Sempre que o motor de dimensionamento automático é executado, este executa um perfil. |
| perfil | nome | O nome do perfil, pode escolher qualquer nome que o ajuda a identificar o perfil. |
| perfil | Capacity.Maximum | A capacidade máxima permitida. Garante que dimensionamento automático, quando executar este perfil, não se dimensiona os recursos acima este número. |
| perfil | Capacity.Minimum | A capacidade mínima, permitida. Garante que dimensionamento automático, quando executar este perfil, não se dimensiona os recursos abaixo este número. |
| perfil | Capacity.default | Se existir um problema ao ler a métrica de recursos (neste caso, a cpu de "vmss1") e a capacidade de atual é inferior a capacidade predefinida, em seguida garantir a disponibilidade do recurso, dimensionamento automático escalas Escalamento predefinido. Se a capacidade atual já for superior à capacidade predefinida, dimensionamento automático serão não escala-in. |
| perfil | regras | Dimensionamento automático dimensiona automaticamente entre as capacidades máxima e mínimas, utilizando as regras no perfil. Pode ter várias regras num perfil. O cenário básico é ter duas regras, um para determinar quando deve escalável e outra para determinar quando na escala. |
| Regra | metricTrigger | Define a condição métrica da regra. |
| metricTrigger | metricName | O nome da métrica. |
| metricTrigger |  metricResourceUri | O ID de recurso do recurso que emite a métrica. Na maioria dos casos, é o mesmo que o recurso que está a ser ampliado. Em alguns casos, pode ser diferente, por exemplo, pode dimensionar um conjunto de dimensionamento de máquina virtual com base no número de mensagens numa fila de armazenamento. |
| metricTrigger | Intervalo de agregação | A duração de métrica de amostragem. Por exemplo, um intervalo de agregação = "PT1M" significa que devem ser as métricas agregadas cada através de 1 minuto, o método de agregação especificado no "estatística." |
| metricTrigger | estatística | O método de agregação dentro do período de um intervalo de agregação. Por exemplo, estatística = "Médio" e um intervalo de agregação = "PT1M" significa que as métricas devem ser agregados a cada 1 minuto, efetuando a média. Esta propriedade determinam a forma como a métrica é amostragem. |
| metricTrigger | timeWindow | A quantidade de tempo para novamente procurar métricas. Por exemplo, timeWindow = "PT10M" significa que sempre que o dimensionamento automático é executado, consulta métricas durante os últimos 10 minutos. A janela de tempo permite que as métricas sejam normalizados e evita a agir os picos de transitórios. |
| metricTrigger | timeAggregation | O método de agregação utilizado para agregar as métricas de amostras. Por exemplo, TimeAggregation = "Médio" deve agregar as métricas de amostras, efetuando a média. No caso de acima tirar os exemplos de 1 minuto dez e médio-los. |
| Regra | scaleAction | A ação a tomar quando o metricTrigger da regra é acionada. |
| scaleAction | direção | "Aumentar" para aumentar horizontalmente, "Diminuir" para escala|
| scaleAction | valor | Quantidade para aumentar ou diminuir a capacidade do recurso |
| scaleAction | cooldown | A quantidade de tempo de espera após uma operação de escala antes de dimensionamento novamente. Por exemplo, se cooldown = "PT10M", em seguida, depois de ocorre uma operação de dimensionamento, dimensionamento automático não irá tentar Dimensionar novamente para outro 10 minutos. O cooldown é permitir que as métricas de stabilize após a adição ou remoção de instâncias. |

## <a name="autoscale-profiles"></a>Perfis de dimensionamento automático

Existem três tipos de perfis de dimensionamento automático:

1. **Perfil regular:** perfil mais comuns. Se não precisar de aumentar os recursos de forma diferente com base no dia da semana, ou num dia específico, em seguida, apenas terá de configurar um perfil regular na sua definição de dimensionamento automático. Este perfil, em seguida, pode ser configurado com regras de métricas da sua organização ditarem quando escalável e quando na escala. Só deve ter um perfil regular definido.

    O perfil de exemplo utilizado anteriormente neste artigo é um exemplo de um perfil regular. Efetue não também é possível definir um perfil de dimensionamento para uma contagem de instâncias static para o seu recurso.

2. **Corrigido perfil data:** com o perfil regular definido, vamos supor que tem um evento importante futuras cópias de segurança no 26 de Dezembro de 2017 (PST) e pretende que o valor mínimo/máximo com capacidades o recurso a ser diferente, nesse dia, mas ainda Dimensionar nas mesmas métricas . Neste caso, deve adicionar um perfil de data fixo à lista de perfis da definição. O perfil é configurado para ser executado apenas no dia do evento. Para outro dia, o perfil regular é executado.

    ``` JSON
    "profiles": [{
    "name": " regularProfile",
    "capacity": {
    ...
    },
    "rules": [{
    ...
    },
    {
    ...
    }]
    },
    {
    "name": "eventProfile",
    "capacity": {
    ...
    },
    "rules": [{
    ...
    }, {
    ...
    }],
    "fixedDate": {
        "timeZone": "Pacific Standard Time",
               "start": "2017-12-26T00:00:00",
               "end": "2017-12-26T23:59:00"
    }}
    ]
    ```
    
3. **Perfil de periodicidade:** este tipo de perfil permite-lhe para se certificar de que este perfil é sempre utilizado num determinado dia da semana. Perfis de periodicidade tem apenas uma hora de início, como resultado da execução até o seguinte perfil de periodicidade ou data fixo perfil está definido para iniciar. Um definição com apenas um perfil de periodicidade de dimensionamento automático executa esse perfil, mesmo se existir um perfil regular definido na mesma definição. Dois exemplos abaixo mostram a utilização deste perfil:

    **Exemplo 1 - vs dia da semana. Fim de semana** Digamos que no fim de semana pretende a capacidade máxima para ser 4, mas em dias da semana, uma vez que espera mais carga, pretende que a capacidade máxima para 10. Neste caso, a definição iria conter dois perfis de periodicidade para ser executada no fim de semana e outro em dias da semana.
    A definição seria ter este aspeto:

    ``` JSON
    "profiles": [
    {
    "name": "weekdayProfile",
    "capacity": {
        ...
    },
    "rules": [{
        ...
    }],
    "recurrence": {
        "frequency": "Week",
        "schedule": {
            "timeZone": "Pacific Standard Time",
            "days": [
                "Monday"
            ],
            "hours": [
                0
            ],
            "minutes": [
                0
            ]
        }
    }}
    },
    {
    "name": "weekendProfile",
    "capacity": {
        ...
    },
    "rules": [{
        ...
    }]
    "recurrence": {
        "frequency": "Week",
        "schedule": {
            "timeZone": "Pacific Standard Time",
            "days": [
                "Saturday"
            ],
            "hours": [
                0
            ],
            "minutes": [
                0
            ]
        }
    }
    }]
    ```

    Ao observar a definição anterior, irá notar que cada perfil de periodicidade tem uma agenda, esta agenda determina quando o perfil de começa a ser executada. O perfil deixa de executar quando está na altura de executar outro perfil.

    Por exemplo, na definição de anterior, "weekdayProfile" está definido para iniciar as 00, o que significa que este perfil começa na segunda-feira a executar em 12.am na segunda-feira. Continua em execução até sábado 12a.m. quando "weekendProfile" está agendado para iniciar a execução.

    **Exemplo 2 - horas de expediente** vamos noutro exemplo, talvez pretenda ter limiar métrica = 'x' durante o horário comercial, as 9 de 17: 00 e, em seguida, 17: 00 para 9 a.m. o dia seguinte, pretende que o limiar de métrico para ser 'y'.
    A definição seria ter este aspeto:
    
    ``` JSON
    "profiles": [
    {
    "name": "businessHoursProfile",
    "capacity": {
        ...
    },
    "rules": [{
        ...
    }],
    "recurrence": {
        "frequency": "Week",
        "schedule": {
            "timeZone": "Pacific Standard Time",
            "days": [
                "Monday", “Tuesday”, “Wednesday”, “Thursday”, “Friday”
            ],
            "hours": [
                9
            ],
            "minutes": [
                0
            ]
        }
    }
    },
    {
    "name": "nonBusinessHoursProfile",
    "capacity": {
        ...
    },
    "rules": [{
        ...
    }]
    "recurrence": {
        "frequency": "Week",
        "schedule": {
            "timeZone": "Pacific Standard Time",
            "days": [
                "Monday", “Tuesday”, “Wednesday”, “Thursday”, “Friday”
            ],
            "hours": [
                17
            ],
            "minutes": [
                0
            ]
        }
    }
    }]
    ```
    
    Ao observar a definição anterior, "businessHoursProfile" começa na segunda-feira a executar em 9 a.m. e mantém executar até 17: 00 uma vez que é quando "nonBusinessHoursProfile" começa a ser executada. Executa "nonBusinessHoursProfile" até 9 a.m. Terça-feira e, em seguida, assume "businessHoursProfile". Isto é repetido até Sexta-feira 17: 00, nesse momento "nonBusinessHoursProfile" executa para segunda-feira 9 a.m. uma vez que "businessHoursProfile" não inicia a execução até segunda-feira 9 a.m.
    
> [!Note]
> O dimensionamento automático UX no portal do Azure impõe a hora de fim para perfis de periodicidade e começa a executar o perfil predefinido de definição de dimensionamento automático entre perfis de periodicidade.
    
## <a name="autoscale-evaluation"></a>Avaliação de dimensionamento automático
Dado que dimensionamento automático definições podem ter vários perfis de dimensionamento automático e cada perfil pode ter várias regras métricas é importante compreender como uma definição de dimensionamento automático é avaliada. Sempre que a tarefa de dimensionamento automático é executado começa ao escolher o perfil que se aplicam, depois de escolher o perfil de dimensionamento automático avalia o mínimo e máximo de valores e quaisquer regras no perfil de métricas e decidir se é necessária uma ação de dimensionamento.

### <a name="which-profile-will-autoscale-pick"></a>O perfil de dimensionamento automático recolhem?
- Dimensionamento automático procura primeiro qualquer corrigido perfil data que está configurado para executar agora, se existir, dimensionamento automático executa-lo. Se existirem vários perfis de data fixo que deveria para executar, dimensionamento automático selecionará o primeiro.
- Se não existirem não existem perfis de data fixo, dimensionamento automático observa perfis de periodicidade, se encontrar, em seguida, ser executada-lo.
- Se existirem não fixa ou perfis de periodicidade, em seguida, dimensionamento automático, execute o perfil regular.

### <a name="how-does-autoscale-evaluate-multiple-rules"></a>Como é que o dimensionamento automático avaliar várias regras?

Depois de dimensionamento automático determina que perfil deve para executar, começa por avaliar todas as regras de escalamento horizontal no perfil de (regras com direção = "Aumentar").
- Se uma ou mais regras de escalamento horizontal são acionadas, o dimensionamento automático calcula a nova capacidade de determinado pelo scaleAction de cada uma dessas regras. Em seguida,-escalas escalamento para o número máximo desses capacidades para garantir a disponibilidade do serviço.
- Por exemplo: se houver um conjunto de dimensionamento de máquina virtual com uma capacidade de 10 atual e existem duas regras de escalamento horizontal; um que aumenta a capacidade de 10% e outro que aumenta a capacidade em 3. A primeira regra resultaria numa nova capacidade de 11, e a segunda regra resultaria numa capacidade de 13. Para garantir a disponibilidade do serviço de dimensionamento automático escolhe a acção que resulta na capacidade máxima, por isso, a segunda regra é escolhida.

Se não existem regras de escalamento horizontal são acionadas, dimensionamento automático avalia todas na escala regras (regras com direção = "Diminuir"). Dimensionamento automático só leva uma ação de dimensionamento se todas as regras de escala são acionadas.
- Dimensionamento automático calcula a nova capacidade de determinado pelo scaleAction de cada uma dessas regras. Em seguida, escolhe a ação de dimensionamento que resulte num máximo desses capacidades para garantir a disponibilidade do serviço.
- Por exemplo: se houver um conjunto de dimensionamento de máquina virtual com uma capacidade de 10 atual e existem regras de-de escala dois; um que diminui a capacidade de 50% e outro que diminui a capacidade por 3. A primeira regra resulta de uma nova capacidade de 5 e a segunda regra resultaria numa capacidade de 7. Para garantir a disponibilidade do serviço de dimensionamento automático escolhe a acção que resulta na capacidade máxima, por isso, a segunda regra é escolhida.

## <a name="next-steps"></a>Próximos Passos
Para saber mais sobre o dimensionamento automático, consulte os seguintes recursos:

* [Descrição geral do dimensionamento automático](monitoring-overview-autoscale.md)
* [Azure métricas comuns para o Monitor de dimensionamento automático](insights-autoscale-common-metrics.md)
* [Melhores práticas para o dimensionamento automático de Monitor do Azure](insights-autoscale-best-practices.md)
* [Utilize ações de dimensionamento automático para enviar correio eletrónico e webhook notificações de alerta](insights-autoscale-to-webhook-email.md)
* [API de REST de dimensionamento automático](https://msdn.microsoft.com/library/dn931953.aspx)
