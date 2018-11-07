---
title: Descrição aprofundada como prever o estado de funcionamento do veículo e orientar hábitos - Azure | Documentos da Microsoft
description: Utilize as capacidades do Cortana Intelligence para obter informações em tempo real e preditivo sobre o estado de funcionamento do veículo e orientar hábitos.
services: machine-learning
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: d8866fa6-aba6-40e5-b3b3-33057393c1a8
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2018
ms.author: deguhath
ms.openlocfilehash: fc4b2d0beef1c91cd937ba901974796441657441
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51234770"
---
# <a name="vehicle-telemetry-analytics-solution-playbook-deep-dive-into-the-solution"></a>Playbook de solução de análise de telemetria vehicle: Aprofunde a solução
Este ligações de menu para as secções neste manual de comunicação: 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../../includes/cap-vehicle-telemetry-playbook-selector.md)]

Este documento aprofunda em cada um dos estágios descritos na arquitetura da solução. Instruções e indicadores de personalização são incluídos. 

## <a name="data-sources"></a>Origens de dados
A solução utiliza duas origens de dados diferentes:

* Sinais de veículos simulados e o conjunto de dados de diagnóstico
* Catálogo de veículos

Computador com o vehicle telematics simulator é incluído como parte desta solução, conforme mostrado na captura de ecrã seguinte. Emite informações de diagnóstico e sinais que correspondem para o estado do veículo e o padrão de condução num determinado ponto no tempo.  O catálogo de veículos contém um conjunto de dados de referência que mapeia os números de identificação de veículos (VINs) a modelos. Nota: O conjunto de dados Vehicle Telematics Simulator solução do Visual Studio já não está disponível. 

![Vehicle telematics simulator](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig1-vehicle-telematics-simulator.png)


Este conjunto de dados formatados por JSON contém o esquema abaixo.

| Coluna | Descrição | Valores |
| --- | --- | --- |
| VIN |VIN gerado aleatoriamente |Obtidas a partir de uma lista de mestre de 10 000 VINs gerados aleatoriamente |
| Temperatura externa |A temperatura externa em que está orientando o veículo |Número gerado aleatoriamente entre 0 e 100 |
| Temperatura de motor |A temperatura do motor do veículo |Número gerado aleatoriamente entre 0 e 500 |
| Velocidade |A velocidade de mecanismo em que está orientando o veículo |Número gerado aleatoriamente entre 0 e 100 |
| Combustível |O nível de combustível do veículo |Gerados aleatoriamente o número de 0 a 100 (indica o percentual do nível de combustível) |
| EngineOil |O nível de petróleo do motor do veículo |Gerados aleatoriamente o número de 0 a 100 (indica o percentual do nível de petróleo motor) |
| Tire pressão |A pressão de tire do veículo |Gerados aleatoriamente o número de 0 a 50 (indica o percentual do nível de pressão tire) |
| Odometer |A leitura de odometer do veículo |Número gerado aleatoriamente, de 0 a 200.000 |
| Accelerator_pedal_position |A posição de pedal acelerador do veículo |Gerados aleatoriamente o número de 0 a 100 (indica o percentual do nível de accelerator) |
| Parking_brake_status |Indica se o veículo é estacionado ou não |VERDADEIRO ou FALSO |
| Headlamp_status |Indica se o headlamp é em ou não |VERDADEIRO ou FALSO |
| Brake_pedal_status |Indica se o pedal brake é pressionado ou não |VERDADEIRO ou FALSO |
| Transmission_gear_position |A posição de engrenagem de transmissão do veículo |Estados: primeiro, segundo, terceiro, quarto, quinto lugar, sixth, sétima, eighth |
| Ignition_status |Indica se o veículo está em execução ou parado |VERDADEIRO ou FALSO |
| Windshield_wiper_status |Indica se o wiper windshield está ativada ou não |VERDADEIRO ou FALSO |
| ABS |Indica se ABS participam ou não |VERDADEIRO ou FALSO |
| Carimbo de data/hora |O carimbo de data / hora quando o ponto de dados é criado |Date |
| Localidade |A localização do veículo |Quatro cidades nesta solução: Bellevue, Redmond, Sammamish, Seattle |

O conjunto de dados de referência de modelo do veículo mapeia VINs a modelos. 

| VIN | Modelo |
| --- | --- |
| FHL3O1SA4IEHB4WU1 |Sedan |
| 8J0U8XCPRGW4Z3NQE |Híbrido |
| WORG68Z2PLTNZDBI7 |Família saloon |
| JTHMYHQTEPP4WBMRN |Sedan |
| W9FTHG27LZN1YWO0Y |Híbrido |
| MHTP9N792PHK08WJM |Família saloon |
| EI4QXI2AXVQQING4I |Sedan |
| 5KKR2VB4WHQH97PF8 |Híbrido |
| W9NSZ423XZHAONYXB |Família saloon |
| 26WJSGHX4MA5ROHNL |Conversíveis |
| GHLUB6ONKMOSI7E77 |Caminhonetes |
| 9C2RHVRVLMEJDBXLP |Carro Compact |
| BRNHVMZOUJ6EOCP32 |SUV pequeno |
| VCYVW0WUZNBTM594J |Carro esporte |
| HNVCE6YFZSA5M82NY |SUV médio |
| 4R30FOR7NUOBL05GJ |Caminhonetes |
| WYNIIY42VKV6OQS1J |SUV grandes |
| 8Y5QKG27QET1RBK7I |SUV grandes |
| DF6OX2WSRA6511BVG |Coupe |
| Z2EOZWZBXAEW3E60T |Sedan |
| M4TV6IEALD5QDS3IR |Híbrido |
| VHRA1Y2TGTA84F00H |Família saloon |
| R0JAUHT1L1R3BIKI0 |Sedan |
| 9230C202Z60XX84AU |Híbrido |
| T8DNDN5UDCWL7M72H |Família saloon |
| 4WPYRUZII5YV7YA42 |Sedan |
| D1ZVY26UV2BFGHZNO |Híbrido |
| XUF99EW9OIQOMV7Q7 |Família saloon |
| 8OMCL3LGI7XNCC21U |Conversíveis |
| ……. | |

## <a name="ingestion"></a>Ingestão
Combinações de Event Hubs do Azure, Azure Stream Analytics e Azure Data Factory são utilizadas para ingerir os sinais de veículos, os eventos de diagnóstico e em tempo real e análises em lote. Todos esses componentes são criados e configurados como parte da implementação da solução. 

### <a name="real-time-analysis"></a>Análise em tempo real
Os eventos gerados por o vehicle telematics simulator são publicados para o hub de eventos utilizando o SDK do hub de eventos.  

![Dashboard do hub de eventos](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig4-vehicle-telematics-event-hub-dashboard.png) 

A tarefa do Stream Analytics ingere esses eventos do hub de eventos e processa os dados em tempo real para analisar o estado de funcionamento do veículo.

![Dados de processamento da tarefa de análise de Stream](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig5-vehicle-telematics-stream-analytics-job-processing-data.png) 


A tarefa de Stream Analytics:

* Ingere dados do hub de eventos.
* Efetua uma junção com os dados de referência para mapear o veículo VIN para o modelo correspondente. 
* Persisti-los para o armazenamento de Blobs do Azure para análise de lotes avançado. 

A seguinte consulta do Stream Analytics é utilizada para manter os dados no armazenamento de BLOBs: 

![Consulta de tarefa do Stream Analytics para ingestão de dados](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig6-vehicle-telematics-stream-analytics-job-query-for-data-ingestion.png) 


### <a name="batch-analysis"></a>Análise de batch
Um volume adicional de sinais de veículos simulados e o conjunto de dados de diagnóstico também é gerado para análise de lotes mais sofisticada. Este volume adicional é necessário para assegurar um volume de dados representativo para processamento em lotes. Para essa finalidade, PrepareSampleDataPipeline é utilizado no fluxo de trabalho do Data Factory para gerar registros um ano do conjunto de dados de diagnóstico e de sinais de veículos simulados. Para transferir a atividade de .NET personalizada da fábrica de dados solução do Visual Studio para personalizações com base nos seus requisitos, vá para o [atividade personalizada do Data Factory](https://go.microsoft.com/fwlink/?LinkId=717077) página Web. 

Este fluxo de trabalho mostra os dados de exemplo preparados para o processamento em lotes.

![Dados de exemplo preparados para o fluxo de trabalho de processamento de batch](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig7-vehicle-telematics-prepare-sample-data-for-batch-processing.png) 


O pipeline consiste numa atividade personalizada do .NET do Data Factory.

![Atividade de PrepareSampleDataPipeline](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig8-vehicle-telematics-prepare-sample-data-pipeline.png) 

Depois do pipeline é executado com êxito e o conjunto de dados RawCarEventsTable está marcada como "Pronto", registros um ano de sinais de veículos simulados e dados de diagnóstico são produzidos. Verá a seguinte pasta e ficheiro criado na sua conta de armazenamento no contentor connectedcar:

![Saída de PrepareSampleDataPipeline](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig9-vehicle-telematics-prepare-sample-data-pipeline-output.png) 

## <a name="partition-the-data-set"></a>O conjunto de dados de partição
O passo de preparação de dados, os sinais de veículos semiestruturados e não processados e o conjunto de dados de diagnóstico são particionados num formato de ano/mês. Essa divisão promove uma consulta mais eficiente e de armazenamento a longo prazo dimensionável, permitindo que a ativação pós-falha de falhas. Por exemplo, como a primeira conta do blob é preenchida, ele de falhas ao longo para a seguinte conta. 

>[!NOTE] 
>Este passo da solução só se aplica a processamento em lotes.

Entrada e saída gestão de dados:

* **Dados de saída** (PartitionedCarEventsTable etiquetada) é mantido por um longo período de tempo que o formulário básico / "rawest" de dados no data lake do cliente. 
* **Os dados de entrada** para este pipeline é descartado, normalmente, uma vez que os dados de saída têm de fidelidade total para a entrada. Ele é armazenado (particionada) melhor para uma utilização posterior.

O partição carro eventos fluxo de trabalho.

![Fluxo de trabalho de eventos de carro de partição](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig10-vehicle-telematics-partition-car-events-workflow.png)


Os dados não processados são particionados usando uma atividade do Hive do HDInsight do Azure no PartitionCarEventsPipeline, conforme mostrado na captura de ecrã seguinte. Os dados de exemplo gerados para um ano no passo de preparação de dados são particionados por ano/mês. As partições são utilizadas para gerar o sinais de veículos e dados de diagnóstico para cada mês (total de 12 partições) de um ano. 

![Atividade de PartitionCarEventsPipeline](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig11-vehicle-telematics-partition-car-events-pipeline.png)


**Script de PartitionConnectedCarEvents Hive**

Partitioncarevents.hql de script do Hive é utilizado para criação de partições. O mesmo está localizado na pasta \demo\src\connectedcar\scripts do ficheiro zip transferido. 
    
    SET hive.exec.dynamic.partition=true;
    SET hive.exec.dynamic.partition.mode = nonstrict;
    set hive.cli.print.header=true;

    DROP TABLE IF EXISTS RawCarEvents; 
    CREATE EXTERNAL TABLE RawCarEvents 
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RAWINPUT}'; 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents 
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string
    ) partitioned by (YearNo int, MonthNo int) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDOUTPUT}';

    DROP TABLE IF EXISTS Stage_RawCarEvents; 
    CREATE TABLE IF NOT EXISTS Stage_RawCarEvents 
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string,
                YearNo                             int,
                MonthNo                         int) 
    ROW FORMAT delimited fields terminated by ',' LINES TERMINATED BY '10';

    INSERT OVERWRITE TABLE Stage_RawCarEvents
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        Year(gendate),
        Month(gendate)

    FROM RawCarEvents WHERE Year(gendate) = ${hiveconf:Year} AND Month(gendate) = ${hiveconf:Month}; 

    INSERT OVERWRITE TABLE PartitionedCarEvents PARTITION(YearNo, MonthNo) 
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        YearNo,
        MonthNo
    FROM Stage_RawCarEvents WHERE YearNo = ${hiveconf:Year} AND MonthNo = ${hiveconf:Month};

Depois do pipeline é executado com êxito, verá as seguintes partições geradas na sua conta de armazenamento no contentor connectedcar:

![Saída particionada](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig12-vehicle-telematics-partitioned-output.png)

Os dados agora são otimizados, mais gerenciável e pronto para processamento adicional para obter informações avançadas do batch. 

## <a name="data-analysis"></a>Análise de dados
Nesta secção, verá como combinar o Stream Analytics, o Azure Machine Learning, o Data Factory e o HDInsight para rich análises avançadas em estado de funcionamento do veículo e hábitos de condução.

### <a name="machine-learning"></a>Aprendizagem automática
O objetivo aqui é prever os veículos que necessitam de manutenção ou lembre-se com base em determinadas estatísticas de estado de funcionamento, com base nas seguintes suposições:

* Se uma das seguintes três condições for verdadeira, os veículos necessitam de manutenção de manutenção:
  
  * A pressão de tire é baixa.
  * O nível de petróleo motor é baixo.
  * A temperatura do mecanismo é elevada.

* Se uma das seguintes condições for verdadeira, os veículos podem ter um problema de segurança e exigir Lembre-se:
  
  * A temperatura do mecanismo é alta, mas a temperatura externa é baixa.
  * A temperatura do mecanismo é baixa, mas a temperatura externa é elevada.

Com base nos requisitos anteriores, dois modelos separados detetar anomalias. Um modelo é para a deteção de manutenção do veículo e é de um modelo para a deteção de recolhimento de veículos. Em ambos os modelos, o algoritmo de análise (PCA) do componente principal interno é utilizado para deteção de anomalias. 

#### <a name="maintenance-detection-model"></a>**Modelo de deteção de manutenção**

Se um dos três indicadores – tire pressão, petróleo motor ou temperatura do motor – satisfaça sua respetiva condição, o modelo de deteção de manutenção relatórios uma anomalia. Como resultado, apenas essas três variáveis tem de considerar na criação do modelo. Na experimentação no machine learning, o **Select Columns in Dataset** módulo é utilizado para extrair essas três variáveis. Em seguida, o módulo de deteção de anomalias baseado em PCA é utilizado para criar o modelo de deteção de anomalias. 

O PCA é uma técnica estabelecida no machine learning, que pode ser aplicado a seleção de funcionalidades, a classificação e a deteção de anomalias. O PCA converte um conjunto de casos que contenham variáveis possivelmente correlacionadas num conjunto de valores chamados os principais componentes. É a idéia-chave da baseado em PCA modelagem de dados do projeto num espaço inferior-dimensional para identificar mais facilmente os recursos e anomalias.

Para cada nova entrada para o modelo de deteção, o detetor de anomalias primeiro calcula a projeção no eigenvectors. Em seguida, calcula o erro de reconstrução normalizado. Este erro normalizado é a pontuação de anomalias: quanto maior for o erro, mais anómalas a instância. 

Em que o problema de deteção de manutenção, cada registo é considerado como um ponto num espaço tridimensional definido por tire pressão, petróleo de motor e temperatura motor coordenadas. Para capturar essas anomalias, o PCA é usado para projetar os dados originais no espaço tridimensional num espaço bidimensional. Portanto, o número de parâmetro de componentes devem ser usados em PCA é definido para dois. Este parâmetro desempenha um papel importante na aplicação de deteção de anomalias baseado em PCA. Depois de usar o PCA para dados do projeto, estas anomalias são identificadas mais facilmente.

#### <a name="recall-anomaly-detection-model"></a>**Lembre-se de modelo de deteção de anomalias**

No modelo de deteção de anomalias de recolhimento, o **Select Columns in Dataset** e módulos de deteção de anomalias baseado em PCA são utilizados de forma semelhante. Especificamente, três variáveis, o motor de temperatura, fora temperatura e velocidade – são extraídos pela primeira vez ao utilizar o **Select Columns in Dataset** módulo. A variável de velocidade também está incluída, uma vez que a temperatura do motor normalmente correlaciona a velocidade. Em seguida, o módulo de deteção de anomalias baseado em PCA é utilizado para os dados do espaço tridimensional do projeto num espaço bidimensional. Os critérios de recolhimento são cumpridos. O veículo requer Lembre-se quando a temperatura de motor e a temperatura externa altamente negativamente são correlacionados. Depois do PCA é executado, o algoritmo de deteção de anomalias baseado em PCA é usado para capturar as anomalias. 

Quando preparar o modelo, os dados normais são utilizados como os dados de entrada para preparar o modelo de deteção de anomalias baseado em PCA. (Dados normais não requerem manutenção ou o recolhimento.) Na classificação experimentação, o modelo de deteção de anomalias preparado é utilizado para detetar se o veículo requer a manutenção ou o recolhimento. 

### <a name="real-time-analysis"></a>Análise em tempo real
A seguinte consulta SQL do Stream Analytics é utilizada para obter a média de todos os parâmetros de veículos importante. Esses parâmetros incluem velocidade de veículos, nível de combustível, temperatura do motor, leitura odometer, tire pressão, nível de petróleo motor e outros. As médias são utilizadas para detetar anomalias, emitir alertas e determinar as condições de estado de funcionamento geral dos veículos operados numa região específica. As médias, em seguida, são correlacionadas para dados demográficos. 

![Consulta do Stream Analytics para processamento em tempo real](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig13-vehicle-telematics-stream-analytics-query-for-real-time-processing.png)

Todas as médias são calculadas numa janela em cascata de 3 segundos. Uma janela em cascata é utilizada porque têm de intervalos de tempo não sobrepostos e contínuos. 

Para saber mais sobre as capacidades de janelas no Stream Analytics, veja [Windowing (Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835019.aspx).

#### <a name="real-time-prediction"></a>**Previsão em tempo real**

Um aplicativo é incluído como parte da solução para operacionalizar o modelo de machine learning em tempo real. O aplicativo RealTimeDashboardApp é criado e configurado como parte da implementação da solução. A aplicação:

* Escuta para uma instância do hub de eventos onde Stream Analytics publica os eventos num padrão continuamente.

    ![Consulta do Stream Analytics para publicar os dados](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig14-vehicle-telematics-stream-analytics-query-for-publishing.png) 

* Recebe eventos. Para todos os eventos que recebe esta aplicação: 
   
   * Os dados são processados através de um ponto de extremidade de solicitação-resposta classificação (RRS) de aprendizagem automática. O ponto de extremidade RRS é publicado automaticamente como parte da implementação.
   * A saída RRS é publicada um conjunto de dados do Power BI utilizando push de APIs.

Este padrão também se aplica a cenários em que pretende integrar uma aplicação de linha de negócio com o fluxo de análise em tempo real. Estes cenários incluem a alertas, notificações e de mensagens.

Nota: se os dados para a solução do Visual Studio de RealtimeDashboardApp já não estão disponíveis.

#### <a name="execute-the-real-time-dashboard-application"></a>**Executar a aplicação de dashboard em tempo real**
1. Extrair o RealtimeDashboardApp e guarde-o localmente.

    ![Pasta de RealTimeDashboardApp](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig16-vehicle-telematics-realtimedashboardapp-folder.png) 

2. Execute o aplicativo RealtimeDashboardApp.exe.

3. Introduza as credenciais válidas do Power BI e selecione **iniciar sessão**.  

    ![Janela de início de sessão da aplicação de dashboard em tempo real](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17a-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) 
    
4. Selecione **Aceitar**.

    ![Janela de-de início de sessão final do dashboard em tempo real de aplicação](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17b-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) 

>[!NOTE] 
>Se pretender remover o conjunto de dados do Power BI, execute o RealtimeDashboardApp com o parâmetro de "flushdata". 

    RealtimeDashboardApp.exe -flushdata


### <a name="batch-analysis"></a>Análise de batch
O objetivo aqui é mostrar como motores de Contoso utiliza as capacidades de computação do Azure para tirar o máximo partido dos macrodados. Estes dados revelam informações avançadas sobre padrões de condução, comportamentos de utilização e estado de funcionamento do veículo. Estas informações possibilita a:

* Melhorar a experiência do cliente e torná-lo mais barato, fornecendo informações em aumentar hábitos e comportamentos de condução fuel-efficient.
* Conheça proativamente clientes e seus padrões que regem a tomada de decisões comerciais e fornecer a melhor na classe produtos e serviços.

Nesta solução, são direcionadas as métricas seguintes:

* **Agressiva orientar o comportamento**: identifica a tendência dos modelos, localizações, motriz condições e altura do ano para obter informações sobre padrões de condução agressivos. Motores de contoso pode utilizar estas informações para as campanhas de marketing para introduzir novas funcionalidades personalizadas e de seguros com base na utilização.
* **Comportamento de condução Fuel-Efficient**: identifica a tendência dos modelos, localizações, motriz condições e altura do ano para obter informações sobre padrões de condução fuel-efficient. Motores de contoso pode utilizar estas informações para as campanhas de marketing para introduzir novas funcionalidades e relatórios proativos para controladores para econômicos e de ambiente amigável hábitos de condução.
* **Lembre-se de modelos**: identifica os modelos que exigem recolhas por operar a anomalias deteção experimentação do machine learning.

Vamos ver os detalhes de cada um destas métricas.

#### <a name="aggressive-driving-behavior-patterns"></a>**Padrões de comportamento de condução agressivo**

Os sinais de veículos particionada e dados de diagnóstico são processados em AggresiveDrivingPatternPipeline, conforme mostrado no seguinte fluxo de trabalho. Hive é utilizado para determinar os modelos, localização, veículo, condições e outros parâmetros que apresentam os padrões de condução agressivos.

![Agressiva motriz fluxo de trabalho do padrão](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig18-vehicle-telematics-aggressive-driving-pattern.png) 

***Consulta de Hive padrão motriz agressiva***

Aggresivedriving.hql de script do Hive é utilizado para analisar padrões de condição de condução agressivo. O mesmo está localizado na pasta \demo\src\connectedcar\scripts do ficheiro zip transferido. 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS CarEventsAggresive; 
    CREATE EXTERNAL TABLE CarEventsAggresive
    (
                   vin                         string, 
                model                        string,
                timestamp                    string,
                city                        string,
                speed                          string,
                transmission_gear_position    string,
                brake_pedal_status            string,
                Year                        string,
                Month                        string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:AGGRESIVEOUTPUT}';



    INSERT OVERWRITE TABLE CarEventsAggresive
    select
    vin,
    model,
    timestamp,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND brake_pedal_status = '1' AND speed >= '50'


O script usa a combinação de posição de engrenagem de transmissão, o estado de pedal brake e velocidade um veículo para detetar comportamentos de condução reckless/agressiva com base em braking padrões em alta velocidade. 

Depois do pipeline é executado com êxito, verá as seguintes partições geradas na sua conta de armazenamento no contentor connectedcar:

![Saída de AggressiveDrivingPatternPipeline](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19-vehicle-telematics-aggressive-driving-pattern-output.png) 


#### <a name="fuel-efficient-driving-behavior-patterns"></a>**Padrões de comportamento de condução Fuel-Efficient**

Os sinais de veículos particionada e dados de diagnóstico são processados em FuelEfficientDrivingPatternPipeline, conforme mostrado no seguinte fluxo de trabalho. Hive é utilizado para determinar os modelos, localização, veículo, condições e outras propriedades que apresentam fuel-efficient padrões de condução.

![Padrões de condução Fuel-Efficient](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19-vehicle-telematics-fuel-efficient-driving-pattern.png) 

***Consulta de Hive padrão motriz Fuel-Efficient***

Fuelefficientdriving.hql de script do Hive é utilizado para analisar padrões de condição de condução fuel-efficient. O mesmo está localizado na pasta \demo\src\connectedcar\scripts do ficheiro zip transferido. 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS FuelEfficientDriving; 
    CREATE EXTERNAL TABLE FuelEfficientDriving
    (
                   vin                         string, 
                model                        string,
                   city                        string,
                speed                          string,
                transmission_gear_position    string,                
                brake_pedal_status            string,            
                accelerator_pedal_position    string,                             
                Year                        string,
                Month                        string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:FUELEFFICIENTOUTPUT}';



    INSERT OVERWRITE TABLE FuelEfficientDriving
    select
    vin,
    model,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    accelerator_pedal_position,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND parking_brake_status = '0' AND brake_pedal_status = '0' AND speed <= '60' AND accelerator_pedal_position >= '50'


O script usa a combinação da posição de engrenagem de transmissão de um veículo, estado de pedal brake, a velocidade e accelerator pedal posição para detetar comportamentos de condução fuel-efficient com base na aceleração, braking e a velocidade de padrões. 

Depois do pipeline é executado com êxito, verá as seguintes partições geradas na sua conta de armazenamento no contentor connectedcar:

![Saída de FuelEfficientDrivingPatternPipeline](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig20-vehicle-telematics-fuel-efficient-driving-pattern-output.png) 

**Lembre-se de previsões do modelo**

A experimentação do machine learning é aprovisionada e publicada como um serviço da web como parte da implementação da solução. O ponto final de classificação do lote é utilizado neste fluxo de trabalho. É registado como um serviço do data factory ligado e operacionalizou ao utilizar o batch de fábrica de dados atividade de classificação.

![Ponto final do Machine learning](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig21-vehicle-telematics-machine-learning-endpoint.png) 

O serviço ligado registado é utilizado na DetectAnomalyPipeline de classificação dos dados, utilizando o modelo de deteção de anomalias. 

![Batch do Machine learning atividade de classificação no data factory](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig22-vehicle-telematics-aml-batch-scoring.png)  

Algumas etapas são executadas neste pipeline para preparação de dados, de modo a que pode ser operacionalizado com o serviço web de classificação de batch. 

![DetectAnomalyPipeline para predição de recolhimento](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig23-vehicle-telematics-pipeline-predicting-recalls.png)  

***Consulta de ramo de registo de deteção de anomalias***

Depois de concluída a classificação, uma atividade de HDInsight processa e agrega os dados de que o modelo categorizado como anomalias. O modelo utiliza uma pontuação de probabilidade de 0.60 ou superior.

    DROP TABLE IF EXISTS CarEventsAnomaly; 
    CREATE EXTERNAL TABLE CarEventsAnomaly 
    (
                vin                            string,
                model                        string,
                gendate                        string,
                outsidetemperature            string,
                enginetemperature            string,
                speed                        string,
                fuel                        string,
                engineoil                    string,
                tirepressure                string,
                odometer                    string,
                city                        string,
                accelerator_pedal_position    string,
                parking_brake_status        string,
                headlamp_status                string,
                brake_pedal_status            string,
                transmission_gear_position    string,
                ignition_status                string,
                windshield_wiper_status        string,
                abs                          string,
                maintenanceLabel            string,
                maintenanceProbability        string,
                RecallLabel                    string,
                RecallProbability            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:ANOMALYOUTPUT}';

    DROP TABLE IF EXISTS RecallModel; 
    CREATE EXTERNAL TABLE RecallModel 
    (

                vin                            string,
                model                        string,
                city                        string,
                outsidetemperature            string,
                enginetemperature            string,
                speed                        string,
                Year                        string,
                Month                        string                

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RECALLMODELOUTPUT}';

    INSERT OVERWRITE TABLE RecallModel
    select
    vin,
    model,
    city,
    outsidetemperature,
    enginetemperature,
    speed,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from CarEventsAnomaly
    where RecallLabel = '1' AND RecallProbability >= '0.60'


Depois do pipeline é executado com êxito, verá as seguintes partições geradas na sua conta de armazenamento no contentor connectedcar:

![Saída de DetectAnomalyPipeline](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig24-vehicle-telematics-detect-anamoly-pipeline-output.png) 

## <a name="publish"></a>Publicar

### <a name="real-time-analysis"></a>Análise em tempo real
Uma das consultas de tarefa do Stream Analytics publica os eventos para uma instância de hubs de eventos de saída. 

![Tarefa do Stream Analytics publicados para uma instância de hubs de eventos de saída](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig25-vehicle-telematics-stream-analytics-job-publishes-output-event-hub.png)

A seguinte consulta do Stream Analytics é utilizada para publicar para a instância de hubs de eventos de saída:

![Consulta do Stream Analytics para publicar para a instância de hubs de eventos de saída](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig26-vehicle-telematics-stream-analytics-query-publish-output-event-hub.png)

Este fluxo de eventos é consumido ao RealTimeDashboardApp que está incluído na solução. Esta aplicação utiliza o serviço machine learning de solicitação-resposta web para a classificação em tempo real. Publica os dados resultantes para um conjunto de dados do Power BI para consumo. 

### <a name="batch-analysis"></a>Análise de batch
Os resultados do batch e processamento em tempo real são publicados para tabelas de base de dados do Azure SQL para consumo. O SQL server, a base de dados e as tabelas são criadas automaticamente como parte do script de configuração. 

Os resultados do processamento de lotes são copiados para o fluxo de trabalho do data mart.

![Resultados copiados para o fluxo de trabalho do data mart de processamento em lotes](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig27-vehicle-telematics-batch-processing-results-copy-to-data-mart.png)

A tarefa do Stream Analytics é publicada para o data mart.

![Tarefa do Stream Analytics publicado no datamart](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig28-vehicle-telematics-stream-analytics-job-publishes-to-data-mart.png)

A definição do data mart é a tarefa de Stream Analytics.

![Definição de mart de dados na tarefa de Stream Analytics](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig29-vehicle-telematics-data-mart-setting-in-stream-analytics-job.png)

## <a name="consume"></a>Consumir
O Power BI fornece esta solução um dashboard avançado para dados em tempo real e visualizações de Análise Preditiva. 

O último dashboard é semelhante a este exemplo:

![Dashboard do Power BI](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig30-vehicle-telematics-powerbi-dashboard.png)

## <a name="summary"></a>Resumo
Este documento contém uma busca detalhada da solução de análise de telemetria de veículos. O padrão de arquitetura de lambda é usado para em tempo real e análise com predições e ações do batch. Este padrão aplica-se para uma ampla variedade de casos de utilização que exigem o hot path (em tempo real) e análise de caminho típico (batch). 

### <a name="references"></a>Referências

* [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)
* [Azure Data Factory](https://azure.microsoft.com/documentation/learning-paths/data-factory/)
* [SDK de Hubs de eventos do Azure para ingestão da transmissão](../../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
* [Capacidades de movimento de dados de fábrica de dados do Azure](../../data-factory/copy-activity-overview.md)
* [Atividade de .NET de fábrica de dados do Azure](../../data-factory/transform-data-using-dotnet-custom-activity.md)
* [Atividades do Azure de .NET do Data Factory usada para preparar os dados de exemplo de solução do Visual Studio](https://go.microsoft.com/fwlink/?LinkId=717077) 
