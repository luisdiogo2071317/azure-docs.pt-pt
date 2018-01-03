---
title: "Configurar a topologia de fábrica ligado | Microsoft Docs"
description: "Como configurar a topologia de uma fábrica ligada a solução pré-configurada."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/12/2017
ms.author: dobett
ms.openlocfilehash: 19e0f48ab817428a1f953c80296b2e23effe5a8a
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/02/2018
---
# <a name="configure-the-connected-factory-preconfigured-solution"></a>Configurar a solução de fábrica ligado pré-configurada

A solução de fábrica ligado pré-configurada mostra um dashboard simulado para uma empresa fictícias Contoso. Esta empresa tem quaisquer fábricas em várias localizações global global.

Este artigo utiliza Contoso como um exemplo para descrever como configurar a topologia de uma solução de fábrica ligado.

## <a name="simulated-factories-configuration"></a>Configuração de factories simulada

Cada fábrica Contoso tem linhas de produção que consiste em três estações. Cada estação é um servidor de OPC UA real com uma função específica:

* Estação de assemblagem
* Estação de teste
* Estação de empacotamento

Estes servidores de OPC UA tem nós de OPC UA e [OPC publicador](https://github.com/Azure/iot-edge-opc-publisher) envia os valores destes nós à fábrica ligada. Isto inclui:

* Estado operacional atual, tais como o consumo de energia atual.
* Informações de produção, tais como o número de produtos produzidos.

Pode utilizar o dashboard para explorar a topologia de fábrica Contoso a partir de uma vista global para baixo para uma vista de nível de estação. Permite que o dashboard de fábrica ligada:

* A visualização de OEE e KPI figuras para cada camada na topologia.
* A visualização de valores atuais de nós de OPC UA nas estações.
* A agregação de figuras OEE e KPI do nível da estação a nível global.
* A visualização de alertas e acções a executar se valores de limiares específicos do alcance.

## <a name="connected-factory-topology"></a>Topologia de fábrica ligado

A topologia de factories, linhas de produção e estações é hierárquica:

* O nível global possui nós de fábrica como subordinados.
* As fábricas de tem nós de linha de produção como subordinados.
* As linhas de produção ter nós de estação como subordinados.
* As estações (servidores OPC UA) tem nós de OPC UA como subordinados.

Cada nó na topologia tem um conjunto comum de propriedades que definem:

* Um identificador exclusivo para o nó de topologia.
* Um nome.
* Uma descrição.
* Uma imagem.
* Os subordinados do nó de topologia.
* Destino, valores mínimo e máximo para figuras OEE e KPI e as ações de alerta para executar.

## <a name="topology-configuration-file"></a>Ficheiro de configuração da topologia

Para configurar as propriedades listadas na secção anterior, a solução de fábrica ligado utiliza um ficheiro de configuração chamado [ContosoTopologyDescription.json](https://github.com/Azure/azure-iot-connected-factory/blob/master/WebApp/Contoso/Topology/ContosoTopologyDescription.json).

Pode encontrar este ficheiro no código de origem da solução no `WebApp/Contoso/Topology` pasta.

O fragmento seguinte mostra uma descrição do `ContosoTopologyDescription.json` ficheiro de configuração:

```json
{
  <global_configuration>,
  "Factories": [
    <factory_configuration>,
    "ProductionLines": [
      <production_line_configuration>,
      "Stations": [
        <station_configuration>,
        <more station_configurations>
      ],
      <more production_line_configurations>
    ]
    <more factory_configurations>
  ]
}
```

As propriedades comuns de `<global_configuration>`, `<factory_configuration>`, `<production_line_configuration>`, e `<station_configuration>` são:

* **Nome** (tipo de cadeia)

  Define um nome descritivo, o que deve ser único para o nó de topologia apresentar no dashboard.

* **Descrição** (tipo de cadeia)

  Descreve o nó de topologia mais detalhadamente.

* **Imagem** (tipo de cadeia)

  O caminho para uma imagem na solução WebApp para mostrar quando as informações sobre o nó de topologia são apresentadas no dashboard.

* **OeeOverall**, **OeePerformance**, **OeeAvailability**, **OeeQuality**, **Kpi1**, **Kpi2** (tipo `<performance_definition>`)

  Estas propriedades definir mínimo, de destino e valores garantidos de operacional figura utilizado para gerar alertas. Estas propriedades também definem as ações a executar se for detetado um alerta.

O `<factory_configuration>` e `<production_line_configuration>` itens tem uma propriedade:

* **GUID** (tipo de cadeia)

  Identifica exclusivamente o nó de topologia.

`<factory_configuration>`tem uma propriedade:

* **Localização** (tipo `<location_definition>`)

  Especifica onde está localizada a fábrica.

`<station_configuration>`tem propriedades:

* **OpcUri** (tipo de cadeia)

  Esta propriedade tem de ser definida para o URI de aplicação de UA OPC do servidor OPC UA.
  Porque tem de ser globalmente exclusivo pela especificação de OPC UA, esta propriedade é utilizada para identificar o nó de topologia de estação.

* **OpcNodes**, que são uma matriz de nós de OPC UA (tipo `<opc_node_description>`)

`<location_definition>`tem propriedades:

* **Cidade** (tipo de cadeia)

  Nome da cidade mais próxima para a localização

* **País** (tipo de cadeia)

  País da localização

* **A latitude** (tipo aspas)

  Latitude da localização

* **A longitude** (tipo aspas)

  Longitude da localização

`<performance_definition>`tem propriedades:

* **Mínimo** (tipo aspas)

  O valor de limiar mais baixo, pode aceder. Se o valor atual é abaixo deste limiar, é gerado um alerta.

* **Destino** (tipo aspas)

  Valor de destino ideal.

* **Máximo** (tipo aspas)

  O valor de limiar superior pode aceder. Se o valor atual é acima deste limiar, é gerado um alerta.

* **MinimumAlertActions** (tipo `<alert_action>`)

  Define o conjunto de ações, que podem ser executadas como resposta a um alerta mínimo.

* **MaximumAlertActions** (tipo `<alert_action>`)

  Define o conjunto de ações, que podem ser executadas como resposta a um alerta máximo.

`<alert_action`> tem propriedades:

* **Tipo** (tipo de cadeia)

  Tipo de ação de alerta. Os seguintes tipos são conhecidos:

  * **AcknowledgeAlert**: o estado do alerta deve ser alterado a confirmação.
  * **CloseAlert**: todos os alertas mais antigos do mesmo tipo já não devem ser apresentados no dashboard.
  * **CallOpcMethod**: um método de OPC UA deve ser chamado.
  * **OpenWebPage**: uma janela do browser deve ser aberta que mostra informações de contextuais adicionais.

* **Descrição** (tipo de cadeia)

  Descrição da ação apresentada no dashboard.

* **Parâmetro** (tipo de cadeia)

  Parâmetros necessários para executar a ação. O valor depende do tipo de ação.

  * **AcknowledgeAlert**: nenhum parâmetro necessário.
  * **CloseAlert**: nenhum parâmetro necessário.
  * **CallOpcMethod**: as informações de nós e os parâmetros do método OPC UA para chamar o formato "NodeId de nó principal, NodeId de método para chamar o URI do servidor OPC UA."
  * **OpenWebPage**: o URL para mostrar na janela do browser.

`<opc_node_description>`contém informações sobre nós de OPC UA uma estação (servidor OPC UA). Nós que representam sem nós de OPC UA existentes, mas que são utilizadas como armazenamento na lógica de cálculo do factory ligado também são válidas. Tem as seguintes propriedades:

* **NodeId** (tipo de cadeia)

  Endereço de UA o OPC espaço de endereço do nó (OPC UA do servidor) da estação. Sintaxe tem de ser conforme especificado na especificação OPC UA para um NodeId.

* **SymbolicName** (tipo de cadeia)

  Nome a apresentar no dashboard quando é apresentado o valor deste nó OPC UA.

* **Relevância** (matriz de cadeia de tipo)

  Indica que o cálculo de OEE ou o valor do nó de OPC UA do KPI é relevante. Cada elemento de matriz pode ser um dos seguintes valores:

  * **OeeAvailability_Running**: o valor é relevante para um cálculo de OEE disponibilidade.
  * **OeeAvailability_Fault**: o valor é relevante para um cálculo de OEE disponibilidade.
  * **OeePerformance_Ideal**: o valor é relevante para um cálculo de desempenho de OEE e é, normalmente, um valor constante.
  * **OeePerformance_Actual**: o valor é relevante para o cálculo das OEE desempenho.
  * **OeeQuality_Good**: o valor é relevante para um cálculo de qualidade de OEE.
  * **OeeQuality_Bad**: o valor é relevante para um cálculo de qualidade de OEE.
  * **Kpi1**: o valor é relevante para o cálculo das KPI1.
  * **Kpi2**: o valor é relevante para o cálculo das KPI2.

* **OpCode** (tipo de cadeia)

  Indica o modo como o valor do nó OPC UA é processado em consultas de conhecimentos aprofundados de séries de tempo e cálculos OEE/KPI. Cada consulta de informações de séries de tempo destina-se um timespan específico, o que é um parâmetro da consulta e oferece um resultado. O OpCode controla como o resultado é calculado e pode ser um dos seguintes valores:

  * **Diff**: diferença entre a última e o primeiro valor no período de tempo.
  * **Média**: a média de todos os valores timespan.
  * **Soma**: a soma de todos os valores timespan.
  * **Última**: atualmente não utilizado.
  * **Contagem**: o número de valores timespan.
  * **Máx.**: o valor garantido o período de tempo.
  * **Min**: o valor mínimo no período de tempo.
  * **Const**: o resultado é o valor especificado pela propriedade ConstValue.
  * **SubMaxMin**: a diferença entre o garantido e o valor mínimo.
  * **TimeSpan**: o período de tempo.

* **Unidades** (tipo de cadeia)

  Define uma unidade do valor para apresentação no dashboard.

* **Visível** (tipo Booleano)

  Controla se o valor deve ser apresentado no dashboard.

* **ConstValue** (tipo aspas)

  Se o **OpCode** é **Const**, em seguida, esta propriedade é o valor do nó.

* **Mínimo** (tipo aspas)

  Se o valor atual seja inferior a este valor, em seguida, é gerado um alerta mínimo.

* **Máximo** (tipo aspas)

  Se o valor atual desencadeia acima este valor, em seguida, é gerado um alerta máximo.

* **MinimumAlertActions** (tipo `<alert_action>`)

  Define o conjunto de ações, que podem ser executadas como resposta a um alerta mínimo.

* **MaximumAlertActions** (tipo `<alert_action>`)

  Define o conjunto de ações, que podem ser executadas como resposta a um alerta máximo.

Ao nível da estação, verá também **simulação** objetos. Estes objetos apenas são utilizados para configurar a simulação de fábrica ligado e não devem ser utilizados para configurar uma topologia real.

## <a name="how-the-configuration-data-is-used-at-runtime"></a>Como os dados de configuração são utilizados no tempo de execução

Todas as propriedades utilizadas no ficheiro de configuração podem ser agrupadas em categorias diferentes consoante a forma como são utilizados. Nessas categorias são:

### <a name="visual-appearance"></a>Aparência Visual

As propriedades desta categoria definem a aparência visual do dashboard fábrica ligado. Os exemplos incluem:

* Nome
* Descrição
* Imagem
* Localização
* Unidades
* Visível

### <a name="internal-topology-tree-addressing"></a>Endereçamento de árvore de topologia interna

A WebApp mantém um dicionário de dados interna que contém informações de todos os nós de topologia. As propriedades **Guid** e **OpcUri** são utilizados como chaves para aceder a este dicionário e têm de ser exclusivo.

### <a name="oeekpi-computation"></a>Cálculo OEE/KPI

Os valores OEE/KPI para a simulação de fábrica ligados são parametrizados por:

* Os valores de nó de OPC UA para ser incluídas no cálculo.
* Como é calculada a figura entre os valores de telemetria.

Fábrica ligada utiliza as fórmulas OEE como publicados pelo http://oeeindustrystandard.oeefoundation.org.

Objetos do nó de OPC UA em estações de ativar a marcação para utilização no cálculo OEE/KPI. O **relevância** propriedade indica para a figura que OEE/KPI deve ser utilizado o valor do nó de OPC UA. O **OpCode** propriedade define a forma como o valor está incluído na computação.

### <a name="alert-handling"></a>Processamento de alertas

Ligado fábrica suporta um mecanismo de simples mínimo/máximo baseadas em limiares a geração do alerta. Existem várias ações predefinidas que pode configurar em resposta a alertas. Este mecanismo de controlo das seguintes propriedades:

* Máximo
* Mínimo
* MaximumAlertActions
* MinimumAlertActions

## <a name="correlating-to-telemetry-data"></a>Correlacionar dados de telemetria

Para determinados operações, como visualizar o último valor ou a criação de consultas de conhecimentos aprofundados de séries de tempo, a WebApp necessita de um esquema de endereçamento para os dados de telemetria transmissões em. A telemetria enviada à fábrica ligada também tem de ser armazenados em estruturas de dados interno. As duas propriedades ativar estas operações são estação (servidor OPC UA) e o nível de nó de OPC UA:

* **OpcUri**

  Identifica (exclusivos) que o servidor de OPC UA a telemetria provém do. Nas mensagens transmissões em, esta propriedade é enviada como **ApplicationUri**.

* **NodeId**

  Identifica o valor do nó no servidor de OPC UA. O formato da propriedade tem de ser conforme especificado na especificação OPC UA. Nas mensagens transmissões em, esta propriedade é enviada como **NodeId**.

Verifique [isto](https://github.com/Azure/iot-edge-opc-publisher) página do GitHub para obter mais informações sobre como os dados de telemetria é ingeridos a fábrica ligada com o publicador OPC.

## <a name="example-how-kpi1-is-calculated"></a>Exemplo: Como KPI1 é calculado

A configuração no `ContosoTopologyDescription.json` ficheiro controla como figuras OEE/KPI são calculadas. O exemplo seguinte mostra como propriedades neste ficheiro controlam o cálculo de KPI1.

No ligado fábrica que kpi1 é utilizado para medir o número de com êxito fabricado produtos na última hora. Cada estação (servidor OPC UA) na simulação fábrica ligado fornece um nó de OPC UA (`NodeId: "ns=2;i=385"`), que fornece a telemetria para este KPI de computação.

A configuração deste nó OPC UA aparente ser o seguinte fragmento:

```json
{
  "NodeId": "ns=2;i=385",
  "SymbolicName": "NumberOfManufacturedProducts",
  "Relevance": [ "Kpi1", "OeeQuality_Good" ],
  "OpCode": "SubMaxMin"
},
```

Esta configuração permite consultar dos valores de telemetria deste nó utilizando Insights de séries de tempo. Obtém a consulta de informações de séries de tempo:

* O número de valores.
* O valor mínimo.
* O valor garantido.
* A média de todos os valores.
* A soma de todos os valores para todos os exclusivo **OpcUri** (**ApplicationUri**), **NodeId** pares no timespan indicado.

Uma característica do **NumberOfManufactureredProducts** o valor do nó é que apenas aumenta. Para calcular o número de produtos fabricado no timespan, ligado fábrica utiliza o **OpCode** **SubMaxMin**. O cálculo obtém o valor mínimo no início do período de tempo e o valor máximo no final de timespan.

O **OpCode** na configuração configura a lógica de cálculo para calcular o resultado da diferença do valor mínimo e máximo. Esses resultados, em seguida, são acumuladas inferior até o nível (global) de raiz e apresentadas no dashboard.

## <a name="next-steps"></a>Passos Seguintes

Sugestões de um passo seguinte é saber como [implementar um gateway no Windows ou Linux para a solução de fábrica ligado pré-configurada](iot-suite-connected-factory-gateway-deployment.md).