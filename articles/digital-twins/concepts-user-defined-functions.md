---
title: Processamento de dados e funções definidas pelo utilizador duplos Digital do Azure | Documentos da Microsoft
description: Descrição geral de processamento de dados, matchers e funções definidas pelo utilizador duplos Digital do Azure
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: alinast
ms.openlocfilehash: 2703778cd2eab582a9e7311aaf2024f100261889
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51624527"
---
# <a name="data-processing-and-user-defined-functions"></a>Processamento de dados e funções definidas pelo utilizador

Duplos Digital do Azure oferece capacidades de computação avançada. Os desenvolvedores podem definir e executar funções personalizadas em mensagens de telemetria de entrada para enviar eventos para pontos finais predefinidos.

## <a name="data-processing-flow"></a>Fluxo de processamento de dados

Depois dos dispositivos enviam dados telemétricos para Twins Digital do Azure, os desenvolvedores podem processar os dados em quatro fases: validar, corresponder, computação e expedir.

![Fluxo de processamento de dados digitais duplos do Azure][1]

1. A fase de validar transforma a mensagem de telemetria de entrada para um normalmente compreendida [objeto de transferência de dados](https://en.wikipedia.org/wiki/Data_transfer_object) formato. Esta fase também executa a validação de dispositivos e sensores.
1. A fase de correspondência localiza as funções adequadas do definidas pelo utilizador (UDFs) para executar. Matchers predefinidos encontrar UDFs com base em informações de espaço de mensagem de telemetria de entrada, sensores e dispositivos.
1. A fase de computação é executado a UDFs correspondida na fase anterior. Estas funções podem ler e atualizar valores computados geográficos em nós de gráfico e pode emitir notificações personalizadas.
1. A fase de expedição encaminha qualquer notificações personalizadas da fase de computação para pontos finais definidos no gráfico.

## <a name="data-processing-objects"></a>Objetos de processamento de dados

Processamento de dados no duplos Digital do Azure consiste em definir três objetos: matchers, funções definidas pelo utilizador e as atribuições de funções.

![Objetos de processamento de dados digitais duplos do Azure][2]

### <a name="matchers"></a>Matchers

Matchers definir um conjunto de condições que avaliar quais ações com base na telemetria de sensores de entrada. Condições para determinar a correspondência podem incluir as propriedades do sensor, o dispositivo de principal do sensor e o espaço de principal do sensor. As condições são expressas como comparações contra um [caminho JSON](http://jsonpath.com/) conforme descrito neste exemplo:

- Todos os sensores do tipo de dados **temperatura**
- Ter `01` na respetiva porta
- Que pertencem a dispositivos com a chave da propriedade expandida **fabricante** definido para o valor `"GoodCorp"`
- Que pertencem aos espaços de tipo `"Venue"`
- Quais são os descendentes de principal **SpaceId** `DE8F06CA-1138-4AD7-89F4-F782CC6F69FD`

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
> - Caminhos JSON são maiúsculas de minúsculas.
> - O payload JSON é o mesmo que o payload devolvido pelo:
>   - `/sensors/{id}?includes=properties,types` para o sensor.
>   - `/devices/{id}?includes=properties,types,sensors,sensorsproperties,sensorstypes` para o dispositivo de principal do sensor.
>   - `/spaces/{id}?includes=properties,types,location,timezone` para o espaço de principal do sensor.
> - As comparações diferenciam maiúsculas de minúsculas.

### <a name="user-defined-functions"></a>Funções definidas pelo utilizador

Uma função definida pelo usuário é uma função personalizada que é executado dentro de um ambiente isolado em duplos Digital do Azure. UDFs têm acesso para a mensagem de telemetria não processados do sensor, conforme foi recebido. UDFs também tem acesso para o serviço graph e o dispatcher espacial. Depois da UDF está registrada no gráfico, tem de ser criada na ferramenta de correspondência (detalhada acima) para especificar quando executar a UDF. Quando duplos Digital do Azure recebe telemetria nova de um determinado sensor, a UDF correspondente pode calcular uma média móvel das último alguns as leituras dos sensores, por exemplo.

UDFs podem ser escritos em JavaScript. Os desenvolvedores podem executar fragmentos personalizados de código em relação a mensagens de telemetria de sensores. Métodos auxiliares interagirem com o gráfico no ambiente de execução definido pelo utilizador. Com uma UDF, os programadores podem:

- Defina o diretamente para o objeto de sensor em gráfico de leitura de sensor.
- Execute uma ação baseada nas leituras de sensores diferentes dentro de um espaço no gráfico.
- Crie uma notificação quando forem cumpridas determinadas condições para um entrada de leitura de sensor.
- Anexe os metadados de gráfico para o sensor ler antes de enviar uma notificação.

Para obter mais informações, consulte [como utilizar as funções definidas pelo utilizador](how-to-user-defined-functions.md).

### <a name="role-assignment"></a>Atribuição de função

Ações de uma UDF estão sujeitos a o controlo de acesso baseado em funções de duplos Digital do Azure para proteger os dados no serviço. Atribuições de funções Certifique-se de que uma determinada UDF tem as permissões adequadas para interagir com o graph espacial. Por exemplo, uma UDF pode tentar criar, ler, atualizar ou eliminar dados de gráficos num determinado espaço. Nível de uma UDF de acesso é verificado durante a UDF pede-lhe o gráfico de dados ou tenta uma ação. Para obter mais informações, consulte [controlo de acesso baseado em funções](security-create-manage-role-assignments.md).

É possível que na ferramenta de correspondência acionar uma UDF com não existem atribuições de função. Neste caso, a UDF não consegue ler todos os dados do gráfico.

## <a name="next-steps"></a>Passos Seguintes

* Para saber mais sobre como rotear mensagens de telemetria e de eventos para outros serviços do Azure, leia [encaminhar eventos e mensagens](concepts-events-routing.md).

* Para saber mais sobre como criar matchers, funções definidas pelo utilizador e as atribuições de funções, leia [guia para utilizar as funções definidas pelo utilizador](how-to-user-defined-functions.md).

<!-- Images -->
[1]: media/concepts/digital-twins-data-processing-flow.png
[2]: media/concepts/digital-twins-user-defined-functions.png
