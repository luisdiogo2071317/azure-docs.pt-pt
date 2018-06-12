---
title: Compreender as definições de dimensionamento automático no Monitor do Azure
description: Uma divisão detalhada das definições de dimensionamento automático e como funcionam. Aplica-se às máquinas virtuais, serviços em nuvem, Web Apps
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 12/18/2017
ms.author: ancav
ms.component: autoscale
ms.openlocfilehash: 982bc43fd86a808da07833d77bde17e17789b2d6
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35265001"
---
# <a name="understand-autoscale-settings"></a>Compreender as definições de Dimensionamento Automático
Definições de dimensionamento automático ajudam Certifique-se de que possui a quantidade certa de recursos em execução para processar a carga fluctuating da sua aplicação. Pode configurar definições de dimensionamento automático para ser acionado com base nas métricas que indiquem a carga ou de desempenho ou accionadas numa hora e data agendada. Este artigo assume uma detalhadas vista de olhos anatomy de uma definição de dimensionamento automático. O artigo começa com o esquema e propriedades de uma definição e, em seguida, explica os tipos de perfil diferente que podem ser configurados. Por fim, o artigo descreve como a funcionalidade de dimensionamento automático no Azure avalia que perfil de executar em qualquer momento.

## <a name="autoscale-setting-schema"></a>Esquema de definição de dimensionamento automático
Para ilustrar o esquema de definição de dimensionamento automático, é utilizada a seguinte definição de dimensionamento automático. É importante ter em atenção que esta definição de dimensionamento automático não tem:
- Um perfil. 
- Duas regras métricas neste perfil: um para o Escalamento horizontal e outro para escala em.
  - A regra de escalamento horizontal é acionada quando métrica de CPU de percentagem média do conjunto de dimensionamento de máquina virtual for superior a 85 por cento durante os últimos 10 minutos.
  - A regra de escala é acionada quando a média do conjunto de dimensionamento de máquina virtual é inferior a 60 por cento para os últimos minutos.

> [!NOTE]
> Uma definição pode ter vários perfis. Para obter mais informações, consulte o [perfis](#autoscale-profiles) secção. Também pode ter um perfil de várias regras de escalamento horizontal e regras de dimensionamento definidas. Para ver como são avaliadas, consulte o [avaliação](#autoscale-evaluation) secção.

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
| Definição | name | O nome da definição de dimensionamento automático. |
| Definição | localização | A localização da definição de dimensionamento automático. Esta localização pode ser diferente da localização do recurso que está a ser ampliada. |
| propriedades | targetResourceUri | O ID de recurso do recurso que está a ser ampliado. Só pode ter uma definição de dimensionamento automático por recurso. |
| propriedades | Perfis | Uma definição de dimensionamento automático é composta por um ou mais perfis. Sempre que o motor de dimensionamento automático é executado, este executa um perfil. |
| perfil | name | O nome do perfil. Pode escolher qualquer nome que o ajuda a identificar o perfil. |
| perfil | Capacity.Maximum | A capacidade máxima permitida. Assegura que o dimensionamento automático, quando executar este perfil, não dimensionar o recurso acima este número. |
| perfil | Capacity.Minimum | A capacidade mínima, permitida. Assegura que o dimensionamento automático, quando executar este perfil, não dimensionar os recursos abaixo este número. |
| perfil | Capacity.default | Se existir um problema ao ler a métrica de recursos (neste caso, a CPU de "vmss1"), e a capacidade atual é inferior a predefinição, dimensionamento automático aumenta horizontalmente de forma predefinido. Isto serve para garantir a disponibilidade do recurso. Se a capacidade atual já for superior à capacidade predefinida, o dimensionamento automático não dimensionar de. |
| perfil | regras | Dimensionamento automático dimensiona automaticamente entre as mínimas e máxima capacidades, utilizando as regras no perfil. Pode ter várias regras num perfil. Normalmente, existem duas regras: um para determinar quando deve aumentar horizontalmente e determinar quando deve dimensionar de outro. |
| regra | metricTrigger | Define a condição métrica da regra. |
| metricTrigger | metricName | O nome da métrica. |
| metricTrigger |  metricResourceUri | O ID de recurso do recurso que emite a métrica. Na maioria dos casos, é o mesmo que o recurso que está a ser ampliado. Em alguns casos, pode ser diferente. Por exemplo, pode dimensionar um conjunto de dimensionamento de máquina virtual com base no número de mensagens numa fila de armazenamento. |
| metricTrigger | Intervalo de agregação | A duração de métrica de amostragem. Por exemplo, **um intervalo de agregação = "PT1M"** significa que as métricas devem ser agregadas a cada 1 minuto, utilizando o método de agregação especificado no elemento de estatística. |
| metricTrigger | estatística | O método de agregação dentro do período de um intervalo de agregação. Por exemplo, **estatística = "Médio"** e **um intervalo de agregação = "PT1M"** significa que as métricas devem ser agregadas a cada 1 minuto, efetuando a média. Esta propriedade determinam a forma como a métrica é amostragem. |
| metricTrigger | timeWindow | A quantidade de tempo para novamente procurar métricas. Por exemplo, **timeWindow = "PT10M"** significa que sempre que o dimensionamento automático é executado, consulta métricas durante os últimos 10 minutos. A janela de tempo permite métricas sejam normalizados e evita a agir os picos de transitórios. |
| metricTrigger | timeAggregation | O método de agregação utilizado para agregar as métricas de amostras. Por exemplo, **TimeAggregation = "Médio"** deve agregar as métricas de amostras, efetuando a média. No caso de anterior, tome os exemplos de 1 minuto dez e médio-los. |
| regra | scaleAction | A ação a tomar quando o metricTrigger da regra é acionada. |
| scaleAction | direção | "Aumente" para aumentar horizontalmente, ou "Diminuir" para escala em.|
| scaleAction | valor | Quantidade para aumentar ou diminuir a capacidade do recurso. |
| scaleAction | cooldown | A quantidade de tempo de espera após uma operação de escala antes de dimensionamento novamente. Por exemplo, se **cooldown = "PT10M"**, dimensionamento automático não tentar Dimensionar novamente para outro 10 minutos. O cooldown é permitir que as métricas de stabilize após a adição ou remoção de instâncias. |

## <a name="autoscale-profiles"></a>Perfis de dimensionamento automático

Existem três tipos de perfis de dimensionamento automático:

- **Perfil regular:** o perfil mais comuns. Se não precisar de aumentar os recursos com base no dia da semana, ou num dia específico, pode utilizar um perfil regular. Este perfil, em seguida, pode ser configurado com regras de métricas da sua organização ditarem quando ampliar e quando dimensionar no. Só deve ter um perfil regular definido.

    O perfil de exemplo utilizado anteriormente neste artigo é um exemplo de um perfil regular. Tenha em atenção que também é possível definir um perfil de dimensionamento para uma contagem de instâncias static para o seu recurso.

- **Corrigido perfil data:** este perfil é para casos especiais. Por exemplo, vamos supor que tem um evento importante futuras cópias de segurança no 26 de Dezembro de 2017 (PST). Pretende que as capacidades mínimas e máxima dos seus recursos sejam diferentes nesse dia, mas ainda escala nas mesmas métricas. Neste caso, deve adicionar um perfil de data fixo para lista as definições de perfis. O perfil é configurado para ser executado apenas no dia do evento. Para outro dia, o dimensionamento automático utiliza o perfil regular.

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
    
- **Perfil de periodicidade:** este tipo de perfil permite-lhe para se certificar de que este perfil é sempre utilizado num determinado dia da semana. Perfis de periodicidade tem apenas uma hora de início. São executadas até o seguinte perfil de periodicidade ou data fixo perfil está definido para iniciar. Uma definição de dimensionamento automático com o perfil de periodicidade apenas um é executado esse perfil, mesmo se existir um perfil regular definido na mesma definição. Os exemplos de dois seguintes ilustram como é utilizado este perfil:

    **Exemplo 1: Dias da semana vs fins de semana**
    
    Imaginemos que fins de semana, pretende que a capacidade máxima para ser 4. Em dias da semana, porque a esperar mais carga, pretende que a capacidade máxima para 10. Neste caso, a definição iria conter dois perfis de periodicidade para ser executada no fim de semana e outro em dias da semana.
    A definição tem o seguinte aspeto:

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

    A definição anterior mostra que cada perfil de periodicidade tem uma agenda. Esta agenda determina quando inicia o perfil de execução. O perfil é interrompida quando está na altura de executar outro perfil.

    Por exemplo, na definição de anterior, "weekdayProfile" está definido para iniciar na segunda-feira, às 12:00. Isto significa que este perfil entra em execução na segunda-feira às 12:00. Continua até sábado às 12:00, quando "weekendProfile" está agendado para iniciar a execução.

    **Exemplo 2: horas de expediente**
    
    Digamos que pretende ter um limiar métrica durante o horário comercial (09:00:00 para 5:00 PM) e outra para todas as outras vezes. A definição seria ter este aspeto:
    
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
    
    A definição anterior mostra que começa em execução na segunda-feira às 9:00 "businessHoursProfile" e continua a 17:00:00. Que é quando "nonBusinessHoursProfile" entra em execução. "nonBusinessHoursProfile" é executada até 09:00:00 Terça-feira e, em seguida, "businessHoursProfile" assume novamente. Isto repete até Sexta-feira, as 17:00:00. Nessa altura, "nonBusinessHoursProfile" é executada para segunda-feira às 9:00.
    
> [!Note]
> A interface de utilizador de dimensionamento automático no portal do Azure impõe a hora de fim para perfis de periodicidade e começa a executar o perfil predefinido de definição de dimensionamento automático entre perfis de periodicidade.
    
## <a name="autoscale-evaluation"></a>Avaliação de dimensionamento automático
Uma vez que as definições de dimensionamento automático podem ter vários perfis e cada perfil pode ter várias regras métricas, é importante compreender a forma como uma definição de dimensionamento automático é avaliada. Sempre que a tarefa de dimensionamento automático é executado, começa por escolher o perfil que se aplica. Em seguida, dimensionamento automático avalia o mínimo e os valores máximos e quaisquer regras métricas no perfil de e decide se é necessária uma ação de dimensionamento.

### <a name="which-profile-will-autoscale-pick"></a>O perfil de dimensionamento automático recolhem?

Dimensionamento automático utiliza a seguinte sequência para escolher o perfil:
1. Procura primeiro para qualquer perfil data fixo que está configurado para executar agora. Se existir, o dimensionamento automático executa-o. Se existirem vários perfis de data fixo que deveria para ser executada, o dimensionamento automático seleciona primeiro.
2. Se não existirem não existem perfis fixo data, o dimensionamento automático observa perfis de periodicidade. Se for encontrado um perfil de periodicidade, é executada.
3. Se não houver nenhum data fixa ou perfis de periodicidade, dimensionamento automático é executado o perfil regular.

### <a name="how-does-autoscale-evaluate-multiple-rules"></a>Como é que o dimensionamento automático avaliar várias regras?

Depois de dimensionamento automático determina que perfil de executar, avalia todas as regras de escalamento horizontal no perfil (são regras com **direção = "Aumentar"**).

Se uma ou mais regras de escalamento horizontal são acionadas, dimensionamento automático calcula a nova capacidade de determinado pelo **scaleAction** de cada uma dessas regras. Em seguida,-aumenta horizontalmente de forma para o número máximo desses capacidades, para garantir a disponibilidade do serviço.

Por exemplo, vamos supor que existe um dimensionamento da máquina virtual está definido com uma capacidade atual de 10. Existem duas regras de escalamento horizontal: um que aumenta a capacidade em 10 por cento e outro que aumenta a capacidade por 3 contagens. A primeira regra resultaria numa nova capacidade de 11, e a segunda regra resultaria numa capacidade de 13. Para garantir a disponibilidade do serviço, o dimensionamento automático escolhe a acção que resulta na capacidade máxima, por isso, a segunda regra é escolhida.

Se não existem regras de escalamento horizontal são acionadas, dimensionamento automático avalia todas na escala regras (regras com **direção = "Diminuir"**). Dimensionamento automático só leva uma ação de dimensionamento se todas as regras de escala são acionadas.

Dimensionamento automático calcula a nova capacidade de determinado pelo **scaleAction** de cada uma dessas regras. Em seguida, escolhe a ação de dimensionamento que resulte num máximo desses capacidades para garantir a disponibilidade do serviço.

Por exemplo, vamos supor que existe um dimensionamento da máquina virtual está definido com uma capacidade atual de 10. Existem duas regras de dimensionamento: um que diminui a capacidade em 50 por cento e outro que diminui a capacidade por 3 contagens. A primeira regra resulta de uma nova capacidade de 5 e a segunda regra resultaria numa capacidade de 7. Para garantir a disponibilidade do serviço, o dimensionamento automático escolhe a acção que resulta na capacidade máxima, por isso, a segunda regra é escolhida.

## <a name="next-steps"></a>Passos Seguintes
Saiba mais sobre o dimensionamento automático ao referir-se ao seguinte:

* [Descrição geral do dimensionamento automático](monitoring-overview-autoscale.md)
* [Azure métricas comuns para o Monitor de dimensionamento automático](insights-autoscale-common-metrics.md)
* [Melhores práticas para o dimensionamento automático de Monitor do Azure](insights-autoscale-best-practices.md)
* [Utilize ações de dimensionamento automático para enviar correio eletrónico e webhook notificações de alerta](insights-autoscale-to-webhook-email.md)
* [API de REST de dimensionamento automático](https://msdn.microsoft.com/library/dn931953.aspx)
