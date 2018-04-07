---
title: Criar uma solução de IoT utilizando o Azure Stream Analytics
description: Tutorial de introdução para a solução de IoT de análise de fluxo de um cenário de tollbooth
services: stream-analytics
author: jasonwhowell
ms.author: jasonh
manager: kfile
ms.reviewer: jasonh, sngun
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/21/2018
ms.openlocfilehash: 80e287d09fdc5ab7157b9ee46bc830fd2db4d501
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="build-an-iot-solution-by-using-stream-analytics"></a>Criar uma solução de IoT utilizando o Stream Analytics

## <a name="introduction"></a>Introdução
Nesta solução, irá aprender a utilizar o Azure Stream Analytics para obter informações em tempo real a partir dos seus dados. Os programadores podem facilmente combinar fluxos de dados, tais como os fluxos de clique, os registos e eventos gerados pelo dispositivo, com registos históricos ou dados de referência derivar informações empresariais. Como um serviço de cálculo de fluxo em tempo real, completamente gerido que está alojado no Microsoft Azure, Azure Stream Analytics fornece resiliência incorporada, latência baixa e escalabilidade para obter a cópia de segurança e em execução em minutos.

Depois de concluir esta solução, é capaz de:

* Familiarize-se com o portal do Azure Stream Analytics.
* Configurar e implementar uma tarefa de transmissão em fluxo.
* Articule reais problemas e resolvê-los utilizando o idioma de consulta do Stream Analytics.
* Desenvolva a transmissão em fluxo soluções para os seus clientes utilizando o Stream Analytics com confiança.
* Utilize a monitorização e a experiência de registo para resolver problemas.

## <a name="prerequisites"></a>Pré-requisitos
Terá dos seguintes pré-requisitos para concluir esta solução:
* Um [subscrição do Azure](https://azure.microsoft.com/pricing/free-trial/)

## <a name="scenario-introduction-hello-toll"></a>Introdução de cenário: "Olá, utilização!"
Uma estação de utilização é um phenomenon comuns. Encontre-los em muitas expressways, pontes e túneis por todo o mundo. Cada estação utilização tem vários booths de utilização. Em booths manuais, parar a pagar a utilização a uma attendant. Em booths automatizadas, um sensor sobre cada booth analisa um cartão RFID affixed para windshield do seu vehicle como passar o booth de utilização. É fácil visualizar passage de veículos através destes estações de utilização como um fluxo de eventos durante o qual podem ser efetuadas operações interessantes.

![Imagem de automóveis em booths de utilização](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image1.jpg)

## <a name="incoming-data"></a>Dados de entrada
Esta solução funciona com dois fluxos de dados. Sensores instalados na entrada como e saída das estações de utilização produzem o fluxo primeiro. O fluxo segundo é um conjunto de dados de pesquisa estático com dados de registo vehicle.

### <a name="entry-data-stream"></a>Fluxo de dados de entrada
O fluxo de dados de entrada contém informações sobre carros à medida que entra utilização estações. Os eventos de dados de saída estão em direto, transmissão em fluxo para uma fila de Hub de eventos a partir de uma aplicação Web incluídos na aplicação de exemplo.

| TollID | EntryTime | LicensePlate | Estado | Certifique- | Modelo | VehicleType | VehicleWeight | Utilização | Etiqueta |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 |2014-09-10 12:01:00.000 |JNB 7001 |NY |Honda |CRV |1 |0 |7 | |
| 1 |2014-09-10 12:02:00.000 |YXZ 1001 |NY |Toyota |Camry |1 |0 |4 |123456789 |
| 3 |2014-09-10 12:02:00.000 |ABC 1004 |CT |Ford |Taurus |1 |0 |5 |456789123 |
| 2 |2014-09-10 12:03:00.000 |XYZ 1003 |CT |Toyota |Corolla |1 |0 |4 | |
| 1 |2014-09-10 12:03:00.000 |BNJ 1007 |NY |Honda |CRV |1 |0 |5 |789123456 |
| 2 |2014-09-10 12:05:00.000 |CDE 1007 |NJ |Toyota |4x4 |1 |0 |6 |321987654 |

Eis uma breve descrição das colunas:

| Coluna | Descrição |
| --- | --- |
| TollID |O ID de booth de utilização que identifica exclusivamente um booth de utilização |
| EntryTime |A data e hora de entrada de vehicle para booth a utilização em UTC |
| LicensePlate |O número de licença plate o veículo |
| Estado |Um Estado nos Estados Unidos |
| Certifique- |Fabricante do automóvel |
| Modelo |O número de modelo do automóvel |
| VehicleType |O 1 para veículos passenger ou 2 para veículos comercial |
| WeightType |Peso vehicle em toneladas; 0 para veículos passenger |
| Utilização |O valor de utilização no EUR |
| Etiqueta |A Tag de i no automóvel que automatiza o pagamento; em branco onde o pagamento foi feito manualmente |

### <a name="exit-data-stream"></a>Fluxo de dados de saída
O fluxo de dados de saída contém informações sobre carros deixa a estação de utilização. Os eventos de dados de saída estão em direto, transmissão em fluxo para uma fila de Hub de eventos a partir de uma aplicação Web incluídos na aplicação de exemplo.

| **TollId** | **ExitTime** | **LicensePlate** |
| --- | --- | --- |
| 1 |2014-09-10T12:03:00.0000000Z |JNB 7001 |
| 1 |2014-09-10T12:03:00.0000000Z |YXZ 1001 |
| 3 |2014-09-10T12:04:00.0000000Z |ABC 1004 |
| 2 |2014-09-10T12:07:00.0000000Z |XYZ 1003 |
| 1 |2014-09-10T12:08:00.0000000Z |BNJ 1007 |
| 2 |2014-09-10T12:07:00.0000000Z |CDE 1007 |

Eis uma breve descrição das colunas:

| Coluna | Descrição |
| --- | --- |
| TollID |O ID de booth de utilização que identifica exclusivamente um booth de utilização |
| ExitTime |A data e hora de saída do veículo de booth de utilização em UTC |
| LicensePlate |O número de licença plate o veículo |

### <a name="commercial-vehicle-registration-data"></a>Dados de registo vehicle comercial
A solução utiliza um instantâneo estático de uma base de dados de registo vehicle comercial. Estes dados são guardados como um ficheiro JSON para o armazenamento de Blobs do Azure, incluído na amostra.

| LicensePlate | RegistrationId | Fora do prazo |
| --- | --- | --- |
| SVT 6023 |285429838 |1 |
| XLZ 3463 |362715656 |0 |
| BAC 1005 |876133137 |1 |
| RIV 8632 |992711956 |0 |
| SNY 7188 |592133890 |0 |
| ELH 9896 |678427724 |1 |

Eis uma breve descrição das colunas:

| Coluna | Descrição |
| --- | --- |
| LicensePlate |O número de licença plate o veículo |
| RegistrationId |ID de registo o veículo |
| Fora do prazo |O estado do registo do veículo: 0 se o registo de vehicle está ativo, 1 se o registo está expirado |

## <a name="set-up-the-environment-for-azure-stream-analytics"></a>Configurar o ambiente para o Azure Stream Analytics
Para concluir esta solução, precisa de uma subscrição do Microsoft Azure. Se não tiver uma conta do Azure, pode [pedir uma versão de avaliação gratuita](http://azure.microsoft.com/pricing/free-trial/).

Lembre-se de que siga os passos na secção "Limpar a sua conta do Azure" no final deste artigo, para que pode fazer a melhor utilização da sua crédito do Azure.

## <a name="deploy-the-sample"></a>Implementar o exemplo 
Existem vários recursos que podem facilmente ser implementados num grupo de recursos, juntamente com alguns cliques. A definição de solução está alojada no repositório do github em [ https://github.com/Azure/azure-stream-analytics/tree/master/Samples/TollApp ](https://github.com/Azure/azure-stream-analytics/tree/master/Samples/TollApp).

### <a name="deploy-the-tollapp-template-in-the-azure-portal"></a>Implementar o modelo de TollApp no portal do Azure
1. Para implementar o ambiente de TollApp no Azure, utilize esta hiperligação para [implementar o modelo do Azure de TollApp](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-stream-analytics%2Fmaster%2FSamples%2FTollApp%2FVSProjects%2FTollAppDeployment%2Fazuredeploy.json).

2. Se lhe for solicitado, inicie sessão no portal do Azure.

3. Selecione a subscrição na qual os vários recursos são cobrados.

4. Especifique um novo grupo de recursos, com um nome exclusivo, por exemplo `MyTollBooth`. 

5. Selecione uma localização do Azure.

6. Especifique um **intervalo** como um número de segundos. Este valor é utilizado na aplicação de web de exemplo, para a frequência de enviar dados para o Hub de eventos. 

7. **Verifique** para aceitar os termos e condições.

8. Selecione **afixar ao dashboard** para que possa localizar facilmente os recursos mais tarde.

9. Selecione **Compra** para implementar o modelo de exemplo.

10. Após alguns instantes, é apresentada uma notificação confirmar o **implementação concluída com êxito**.

### <a name="review-the-azure-stream-analytics-tollapp-resources"></a>Reveja os recursos do Azure Stream Analytics TollApp
1. Iniciar sessão no portal do Azure

2. Localize o grupo de recursos com o nome na secção anterior.

3. Certifique-se de que os seguintes recursos estão listados no grupo de recursos:
   - Conta do uma Cosmos base de dados
   - Uma tarefa do Azure Stream Analytics
   - Uma conta de armazenamento do Azure
   - Um Hub de eventos do Azure
   - Duas aplicações Web

## <a name="examine-the-sample-tollapp-job"></a>Examine a tarefa de TollApp de exemplo 
1. A partir do grupo de recursos na secção anterior, selecione a tarefa de transmissão em fluxo do Stream Analytics, começando com o nome **tollapp** (o nome contém carateres aleatórios exclusividade).

2. No **descrição geral** página da tarefa, tenha em atenção o **consulta** caixa para ver a sintaxe de consulta.

   ```sql
   SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*) AS Count
   INTO CosmosDB
   FROM EntryStream TIMESTAMP BY EntryTime
   GROUP BY TUMBLINGWINDOW(minute, 3), TollId
   ```

   Para paraphrase a intenção da consulta, vamos supor que o que precisa contabilizar o número de veículos que introduza uma booth de utilização. Porque um booth de utilização de autoestrada tem um fluxo contínuo de veículos introduzir, são os eventos de entrada como são análogos a uma sequência que nunca interrompe. A quantificar o fluxo, tem de definir um "período de tempo" para medir a ativação pós-falha. Vamos refinar além disso, a pergunta "veículos quantos introduzir um booth de utilização a cada três minutos?" Isto é frequentemente denominado a contagem de em cascata.

   Como pode ver, o Azure Stream Analytics utiliza uma linguagem de consulta que é como o SQL e adiciona alguns extensões para especificar os aspetos relacionados com a hora da consulta.  Para obter mais detalhes, leia sobre [tempo gestão](https://msdn.microsoft.com/library/azure/mt582045.aspx) e [modos de janela](https://msdn.microsoft.com/library/azure/dn835019.aspx) construções utilizadas na consulta.

3. Examine as entradas da tarefa de exemplo TollApp. Apenas a entrada de EntryStream é utilizada na consulta atual.
   - **EntryStream** entrada é uma ligação do Hub de eventos que dados que representam sempre que um carro introduz um tollbooth na autoestrada em fila. Uma aplicação web que faz parte de exemplo está a criar os eventos e os dados colocados em fila neste Hub de eventos. Tenha em atenção que esta entrada é consultada na cláusula FROM da consulta de transmissão em fluxo.
   - **ExitStream** entrada é uma ligação do Hub de eventos que dados que representam sempre que um carro sai um tollbooth na autoestrada em fila. Esta entrada de transmissão em fluxo é utilizada na variações posteriores da sintaxe de consulta.
   - **Registo** entrada é uma ligação de armazenamento de Blobs do Azure, que aponta para um ficheiro estático registration.json, utilizado para pesquisas com conforme necessário. Esta entrada de dados de referência é utilizada na variações posteriores da sintaxe de consulta.

4. Examine as saídas da tarefa de exemplo TollApp.
   - **BD do cosmos** o resultado é uma coleção de base de dados do Cosmos que recebe os eventos de sink de saída. Tenha em atenção que este resultado é utilizado na cláusula da consulta de transmissão em fluxo INTO.

## <a name="start-the-tollapp-streaming-job"></a>Iniciar a tarefa de transmissão em fluxo TollApp
Siga estes passos para iniciar a tarefa de transmissão em fluxo:

1. No **descrição geral** página da tarefa, selecione de **iniciar**.

2. No **iniciar tarefa** painel, selecione **agora**.

3. Após alguns instantes, assim que a tarefa está em execução, no **descrição geral** página da tarefa de transmissão em fluxo, ver o **monitorização** gráfico. O gráfico deve mostrar vários eventos de entrada milhares e dezenas de eventos de saída.

## <a name="review-the-cosmosdb-output-data"></a>Reveja os dados de saída CosmosDB
1. Localize o grupo de recursos que contém os recursos de TollApp.

2. Selecione a conta de base de dados do Cosmos do Azure com o padrão de nome **tollapp<random>-cosmos**.

3. Selecione o **Explorador de dados** cabeçalho para abrir a página do Explorador de dados.

4. Expanda o **tollAppDatabase** > **tollAppCollection** > **documentos**.

5. Na lista de ids, vários documentos são apresentados depois do resultado está disponível.

6. Selecione cada id para rever o documento JSON. Tenha em atenção cada tollid, windowend hora e o número de carros que janela.

7. Após um minutos três adicionais, outro conjunto de quatro documentos que está disponível um documento por tollid. 


## <a name="report-total-time-for-each-car"></a>Tempo total do relatório para cada carro
O tempo médio que necessita de um carro passar a utilização ajuda-o para avaliar a eficiência do processo e a experiência do cliente.

Para determinar o total de tempo, associe o fluxo de EntryTime com o fluxo de ExitTime. Associe os dois fluxos de entrada nas iguais correspondentes TollId e LicencePlate colunas. O **associar** operador requer que especifique leeway temporal que descreve a diferença de tempo aceitável, entre os eventos associados. Utilize o **DATEDIFF** função para especificar que os eventos devem ser mais do que 15 minutos entre si. Também se aplicam a **DATEDIFF** função para sair e as horas de entrada para calcular o real de tempo que um carro passa na estação de utilização. Tenha em atenção a diferença de utilização de **DATEDIFF** quando é utilizado num **SELECIONE** instrução em vez de um **associar** condição.

```sql
SELECT EntryStream.TollId, EntryStream.EntryTime, ExitStream.ExitTime, EntryStream.LicensePlate, DATEDIFF (minute, EntryStream.EntryTime, ExitStream.ExitTime) AS DurationInMinutes
INTO CosmosDB
FROM EntryStream TIMESTAMP BY EntryTime
JOIN ExitStream TIMESTAMP BY ExitTime
ON (EntryStream.TollId= ExitStream.TollId AND EntryStream.LicensePlate = ExitStream.LicensePlate)
AND DATEDIFF (minute, EntryStream, ExitStream ) BETWEEN 0 AND 15
```

### <a name="to-update-the-tollapp-streaming-job-query-syntax"></a>Para atualizar o TollApp sintaxe de consulta da tarefa de transmissão em fluxo:

1. No **descrição geral** página da tarefa, selecione de **parar**.

2. Aguarde um momento para a notificação que a tarefa foi parada.

3. Sob o cabeçalho de TOPOLOGIA de tarefa, selecione **< > consulta**

4. Cole a consulta SQL de transmissão em fluxo ajustada.

5. Selecione **guardar** para guardar a consulta. Confirmar **Sim** para guardar as alterações.

6. No **descrição geral** página da tarefa, selecione de **iniciar**.

7. No **iniciar tarefa** painel, selecione **agora**.

### <a name="review-the-total-time-in-the-output"></a>Reveja o tempo total no resultado
Repita os passos na secção anterior para rever os dados de saída CosmosDB da tarefa de transmissão em fluxo. Reveja os documentos JSON mais recentes. 

Por exemplo, este documento mostra um carro de exemplo com um determinado plate de licença, a hora entrytime e de saída e o campo de durationinminutes calculada DATEDIFF que mostra a duração de booth utilização como dois minutos: 
```JSON
{
    "tollid": 4,
    "entrytime": "2018-04-05T06:51:39.0491173Z",
    "exittime": "2018-04-05T06:53:09.0491173Z",
    "licenseplate": "JVR 9425",
    "durationinminutes": 2,
    "id": "ff52eb25-d580-7566-2879-1f52bba6601e",
    "_rid": "+8E4AI1DZgBjAAAAAAAAAA==",
    "_self": "dbs/+8E4AA==/colls/+8E4AI1DZgA=/docs/+8E4AI1DZgBjAAAAAAAAAA==/",
    "_etag": "\"ad02f6b8-0000-0000-0000-5ac5c8330000\"",
    "_attachments": "attachments/",
    "_ts": 1522911283
}
```

## <a name="report-vehicles-with-expired-registration"></a>Relatório veículos com registo expirado
O Azure Stream Analytics pode utilizar instantâneos estáticos de dados de referência para juntar com fluxos de dados temporal. Para demonstrar esta capacidade, utilize a pergunta de exemplo seguinte. A entrada de registo é um ficheiro de json do blob estático apresenta uma lista de Expirações das etiquetas de licença. Ao aderir no plate de licença, os dados de referência é comparado com cada vehicle passar através de utilização. 

Se um vehicle comercial estiver registado com a empresa de utilização, pode passar através de booth utilização sem ser parada para inspecção. Utilize a tabela de pesquisa de registo para identificar todos os veículos comercial que tenham expirado registos.

```sql
SELECT EntryStream.EntryTime, EntryStream.LicensePlate, EntryStream.TollId, Registration.RegistrationId
INTO CosmosDB
FROM EntryStream TIMESTAMP BY EntryTime
JOIN Registration
ON EntryStream.LicensePlate = Registration.LicensePlate
WHERE Registration.Expired = '1'
```

1. Repita os passos na secção anterior para atualizar o TollApp sintaxe de consulta da tarefa de transmissão em fluxo.

2. Repita os passos na secção anterior para rever os dados de saída CosmosDB da tarefa de transmissão em fluxo. 

Exemplo de saída:
```json
    {
        "entrytime": "2018-04-05T08:01:28.0252168Z",
        "licenseplate": "GMT 3221",
        "tollid": 1,
        "registrationid": "763220582",
        "id": "47db0535-9716-4eb2-db58-de7886966cbf",
        "_rid": "y+F8AJ9QWACSAQAAAAAAAA==",
        "_self": "dbs/y+F8AA==/colls/y+F8AJ9QWAA=/docs/y+F8AJ9QWACSAQAAAAAAAA==/",
        "_etag": "\"88007d8d-0000-0000-0000-5ac5d7e20000\"",
        "_attachments": "attachments/",
        "_ts": 1522915298
    }
```

## <a name="scale-out-the-job"></a>Aumentar horizontalmente a tarefa
O Azure Stream Analytics foi concebido aprovisionadas o dimensionamento, para que pode processar grandes volumes de dados. A consulta do Azure Stream Analytics pode utilizar um **PARTITION BY** cláusula para informar o sistema que este passo aumenta horizontalmente de forma. **PartitionId** é uma coluna especial que adiciona o sistema para corresponder ao ID de partição da entrada (hub de eventos).

Para ampliar a consulta para partições, edite a sintaxe de consulta para o seguinte código:
```sql
SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*)AS Count
INTO CosmosDB
FROM EntryStream 
TIMESTAMP BY EntryTime 
PARTITION BY PartitionId
GROUP BY TUMBLINGWINDOW(minute,3), TollId, PartitionId
```

Para aumentar verticalmente a tarefa de transmissão em fluxo mais unidades de transmissão em fluxo:

1. **Parar** a tarefa atual. 

2. Atualize a sintaxe de consulta no **< > consulta** página e guardar as alterações.

3. Sob o cabeçalho de configurar na tarefa de transmissão em fluxo, selecione **escala**.
   
4. Faça deslizar a **unidades de transmissão em fluxo** controlo de deslize de 1 a 6. Unidades de transmissão em fluxo definem a quantidade de capacidade de computação que pode receber a tarefa. Selecione **Guardar**.

5. **Iniciar** a tarefa de transmissão em fluxo para demonstrar a escala adicional. O Azure Stream Analytics distribui trabalho mais recursos de computação e alcançar a melhorar o débito, o trabalho de criação de partições através de recursos através da coluna designada na cláusula PARTITION BY. 

## <a name="monitor-the-job"></a>Monitorizar a tarefa
O **MONITOR** área contém estatísticas sobre a tarefa em execução. Configuração da hora primeiro é necessário para utilizar a conta do storage na mesma região (nome de utilização, como o resto deste documento).   

![Captura de ecrã do monitor](media/stream-analytics-build-an-iot-solution-using-stream-analytics/monitoring.png)

Pode aceder ao **registos de atividade** partir do dashboard tarefa **definições** área bem.

## <a name="clean-up-the-tollapp-resources"></a>Limpar os recursos de TollApp
1. Pare a tarefa de Stream Analytics no portal do Azure.

2. Localize o grupo de recursos contém oito recursos relacionados com o modelo de TollApp.

3. Selecione **Eliminar grupo de recursos**. Escreva o nome do grupo de recursos para confirmar a eliminação.

## <a name="conclusion"></a>Conclusão
Esta solução introduzidos o serviço Azure Stream Analytics. -Demonstrado como configurar as entradas e saídas da tarefa de Stream Analytics. Utilizando o cenário de dados de utilização, a solução explicado tipos comuns dos problemas que surgir no espaço de dados em movimento e a forma como pode ser resolvidos com consultas de SQL Server como simples no Azure Stream Analytics. A solução descrito construções de extensão SQL para trabalhar com dados temporais. -Mostrou como associar fluxos de dados, como enriqueça o fluxo de dados com dados de referência estáticos e como aumentar horizontalmente uma consulta para alcançar o débito mais elevado.

Embora esta solução fornece uma introdução boa, não estão concluída por qualquer meio. Pode encontrar mais padrões de consulta utilizando a linguagem SAQL em [consultar exemplos de padrões de utilização comuns do Stream Analytics](stream-analytics-stream-analytics-query-patterns.md).
