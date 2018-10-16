---
title: Processamento de dados e funções definidas pelo utilizador duplos Digital do Azure | Documentos da Microsoft
description: Descrição geral de processamento de dados, matchers e funções definidas pelo utilizador duplos Digital do Azure
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/08/2018
ms.author: alinast
ms.openlocfilehash: b7561848ffd0158e22e97530774112dcee2a9864
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49324183"
---
# <a name="data-processing-and-user-defined-functions"></a>Processamento de dados e funções definidas pelo utilizador

Duplos Digital do Azure oferece capacidades de computação avançada. Os desenvolvedores podem definir e executar funções personalizadas em mensagens de telemetria de entrada para enviar eventos para pontos de extremidade previamente definidos.

## <a name="data-processing-flow"></a>Fluxo de processamento de dados

Assim que os dispositivos enviam dados telemétricos para Digital Twins, os desenvolvedores capaz de processar dados em quatro fases: _validar_, _corresponde ao_, _computação_, e _expedição_:

![Fluxo de processamento de dados digitais duplos][1]

1. O _validar_ fase transforma a mensagem de telemetria de entrada para um normalmente compreendida [ `data transfer object` ](https://en.wikipedia.org/wiki/Data_transfer_object) formato. Esta fase também executa a validação de dispositivos e sensores.
1. O _corresponde ao_ fase localiza a função de (ões) do adequadas definidas pelo utilizador para ser executado. Matchers predefinidos irão encontrar que a função de definidas pelo utilizador (ões) com base em informações de espaço de mensagem de telemetria de entrada, sensores e dispositivos.
1. O _computação_ fase é executado o definidas pelo utilizador de função ((de ões), correspondentes na fase anterior. Estes função (ões) pode ler e atualizar valores computados em nós de gráfico geográficos e pode emitir notificações personalizadas.
1. O _expedição_ fase encaminha qualquer notificações personalizadas da fase de computação para pontos finais definidos no gráfico.

## <a name="data-processing-objects"></a>Objetos de processamento de dados

Processamento de dados no duplos Digital do Azure consiste em definir três objetos: _matchers_, _funções definidas pelo utilizador_, e _atribuições de funções_:

![Fluxo de processamento de dados digitais duplos][2]

### <a name="matchers"></a>Matchers

_Matchers_ definir um conjunto de condições que avaliar quais ações irão ocorrer com base na telemetria de sensores de entrada. Estas condições para determinar a correspondência podem incluir as propriedades do sensor, o dispositivo de principal do sensor e o espaço de principal do sensor. As condições são expressas como comparações contra um [caminho JSON](http://jsonpath.com/) conforme descrito no exemplo abaixo:

- Todos os sensores do tipo de dados `Temperature`.
- Ter `01` na respetiva porta.
- Que pertencem a dispositivos com a chave da propriedade expandida `Manufacturer` definido para o valor `GoodCorp`.
- Que pertencem aos espaços do tipo `Venue`.
- Quais são os descendentes do pai `SpaceId` `DE8F06CA-1138-4AD7-89F4-F782CC6F69FD`.

```JSON
{
  "SpaceId": "DE8F06CA-1138-4AD7-89F4-F782CC6F69FD",
  "Name": "My custom matcher",
  "Description": "All sensors of datatype Temperature with 01 in their port that belong to devices with the extended property key Manufacturer set to the value GoodCorp and that belong to spaces of type Venue that are somewhere below space Id DE8F06CA-1138-4AD7-89F4-F782CC6F69FD",
  "Conditions": [
    {
      "target": "Sensor",
      "path": "$.dataType",
      "value": "\"Temperature\"",
      "comparison": "Equals"
    },
    {
      "target": "Sensor",
      "path": "$.port",
      "value": "01",
      "comparison": "Contains"
    },
    {
      "target": "SensorDevice",
      "path": "$.properties[?(@.name == 'Manufacturer')].value",
      "value": "\"GoodCorp\"",
      "comparison": "Equals"
    },
    {
      "target": "SensorSpace",
      "path": "$.type",
      "value": "\"Venue\"",
      "comparison": "Equals"
    }
  ]
}
```

> [!IMPORTANT]
> - Caminhos JSON diferenciam maiúsculas de minúsculas.
> - O payload JSON é o mesmo que o payload devolvido por:
>   - `/sensors/{id}?includes=properties,types` para o sensor.
>   - `/devices/{id}?includes=properties,types,sensors,sensorsproperties,sensorstypes` para o dispositivo de principal do sensor.
>   - `/spaces/{id}?includes=properties,types,location,timezone` para o espaço de principal do sensor.
> - As comparações diferenciam maiúsculas de minúsculas.

### <a name="user-defined-functions"></a>Funções definidas pelo utilizador

R _função definida pelo utilizador_, ou _UDF_, é uma função personalizada que é executado dentro de um ambiente isolado em duplos Digital do Azure. UDFs têm acesso a ambos os a mensagem de telemetria não processados do sensor conforme foi recebido, bem como para o serviço graph e o dispatcher espacial. Depois da UDF está registrada no gráfico, tem de ser criada na ferramenta de correspondência (detalhada acima) para especificar quando executar a UDF. Quando duplos Digital recebe telemetria nova de um determinado sensor, a UDF correspondente pode calcular uma média móvel das último alguns as leituras dos sensores, por exemplo.

UDFs podem ser escritos em JavaScript e permitir que os desenvolvedores executem fragmentos personalizados de código em relação a mensagens de telemetria de sensores. Também há métodos auxiliares para interagir com o gráfico no ambiente de execução definido pelo utilizador. Com uma UDF, os programadores podem:

- Defina o diretamente para o objeto de sensor em gráfico de leitura de sensor.
- Execute uma ação baseada nas leituras de sensores diferentes dentro de um espaço no gráfico.
- Crie uma notificação quando forem cumpridas determinadas condições para um entrada de leitura de sensor.
- Anexe os metadados de gráfico para o sensor ler antes de enviar uma notificação.

Consulte a [como funções de User User-Defined](how-to-user-defined-functions.md) para obter mais detalhes.

### <a name="role-assignment"></a>Atribuição de função

Ações de uma UDF estão sujeitos a controlo de acesso baseado em funções de Digital duplos para proteger os dados no serviço. Atribuições de funções Certifique-se de que uma determinada UDF tem as permissões adequadas para interagir com o graph espacial. Por exemplo, uma UDF pode tentar criar, ler, atualizar ou eliminar dados de gráficos num determinado espaço. Nível de uma UDF de acesso é verificado durante a UDF pede-lhe o gráfico de dados ou tenta uma ação. Para obter mais informações, consulte [controlo de acesso baseado em funções](security-create-manage-role-assignments.md).

É possível que na ferramenta de correspondência acionar uma UDF com não existem atribuições de função. Neste caso, a UDF falharia ler todos os dados do gráfico.

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre como eventos de roteamento e mensagens de telemetria para outros serviços do Azure, leia [mensagens de eventos de roteamento e](concepts-events-routing.md).

Para saber mais sobre como criar matchers, funções definidas pelo utilizador e as atribuições de funções, leia [guia para utilizar as funções definidas pelo utilizador](how-to-user-defined-functions.md).

<!-- Images -->
[1]: media/concepts/digital-twins-data-processing-flow.png
[2]: media/concepts/digital-twins-user-defined-functions.png
