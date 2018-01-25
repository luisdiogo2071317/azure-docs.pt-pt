---
title: "Executar as funções do Azure com tarefas do Azure Stream Analytics | Microsoft Docs"
description: "Saiba como configurar as funções do Azure como um sink de saída para tarefas do Stream Analytics."
keywords: "dados de saída, transmissão em fluxo de dados, a função do Azure"
documentationcenter: 
services: stream-analytics
author: SnehaGunda
manager: kfile
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 12/19/2017
ms.author: sngun
ms.openlocfilehash: f2f4a8d8cda752dc6ed197b8402119f7cbcaf58f
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/24/2018
---
# <a name="run-azure-functions-with-azure-stream-analytics-jobs"></a>Executar as funções do Azure com tarefas do Azure Stream Analytics 

Pode executar as funções do Azure com o Azure Stream Analytics através da configuração de funções como um dos sinks de saída para a tarefa de Stream Analytics. As funções são uma experiência de cálculo a pedido condicionada por eventos que lhe permite implementar o código que é acionado pelos eventos que ocorrem no Azure ou serviços de terceiros. Esta capacidade de funções para responder a acionadores torna uma saída natural para tarefas do Stream Analytics.

Do Stream Analytics invoca funções através de acionadores HTTP. O adaptador de saída de funções permite aos utilizadores ligar funções Stream Analytics, para que os eventos podem ser acionados baseada em consultas do Stream Analytics. 

Este tutorial demonstra como ligar o Stream Analytics para [a Cache de Redis do Azure](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md), utilizando [das funções do Azure](../azure-functions/functions-overview.md). 

## <a name="configure-a-stream-analytics-job-to-run-a-function"></a>Configurar uma tarefa de Stream Analytics para executar uma função 

Esta secção demonstra como configurar uma tarefa de Stream Analytics para executar uma função que escreve dados para a Cache de Redis do Azure. A tarefa de Stream Analytics lê eventos a partir do Event Hubs do Azure e executa uma consulta que invoca a função. Esta função lê os dados da tarefa do Stream Analytics e escreve-o para a Cache de Redis do Azure.

![Diagrama que mostra as relações entre os serviços do Azure](./media/stream-analytics-with-azure-functions/image1.png)

Os seguintes passos são necessários para realizar esta tarefa:
* [Criar uma tarefa de Stream Analytics com os Event Hubs como entrada](#create-stream-analytics-job-with-event-hub-as-input)  
* [Criar uma instância da Cache de Redis do Azure](#create-an-azure-redis-cache)  
* [Criar uma função em funções do Azure que pode escrever dados para a Cache de Redis do Azure](#create-an-azure-function-that-can-write-data-to-the-redis-cache)    
* [Atualizar a tarefa de Stream Analytics com a função como saída](#update-the-stream-analytic-job-with-azure-function-as-output)  
* [Verifique a Cache de Redis do Azure para obter os resultados](#check-redis-cache-for-results)  

## <a name="create-a-stream-analytics-job-with-event-hubs-as-input"></a>Criar uma tarefa de Stream Analytics com os Event Hubs como entrada

Siga o [deteção de fraudes em tempo real](stream-analytics-real-time-fraud-detection.md) tutorial para criar um hub de eventos, iniciar a aplicação do gerador de eventos e criar uma tarefa de Stream Analytics. (Ignore os passos para criar a consulta e de saída. Em vez disso, consulte as secções seguintes para configurar a saída de funções.)

## <a name="create-an-azure-redis-cache-instance"></a>Criar uma instância da Cache de Redis do Azure

1. Criar uma cache na Cache de Redis do Azure, utilizando os passos descritos no [criar uma cache](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).  

2. Depois de criar a cache em **definições**, selecione **chaves de acesso**. Anote o **cadeia de ligação principal**.

   ![Cadeia de ligação de captura de ecrã da Cache de Redis Azure](./media/stream-analytics-with-azure-functions/image2.png)

## <a name="create-a-function-in-azure-functions-that-can-write-data-to-azure-redis-cache"></a>Criar uma função em funções do Azure que pode escrever dados para a Cache de Redis do Azure

1. Consulte o [criar uma aplicação de função](../azure-functions/functions-create-first-azure-function.md#create-a-function-app) secção da documentação de funções. Isto explica-lhe como criar uma aplicação de função e um [função de acionada por HTTP de mensagens em fila nas funções do Azure](../azure-functions/functions-create-first-azure-function.md#create-function), utilizando a linguagem de CSharp.  

2. Navegue para o **run.csx** função. A atualização com o seguinte código. (Certifique-se de que substitui "\<a cadeia de ligação da cache de redis aqui\>" com a cadeia de ligação principal de Cache de Redis do Azure que obteve na secção anterior.)  

   ```csharp
   using System;
   using System.Net;
   using System.Threading.Tasks;
   using StackExchange.Redis;
   using Newtonsoft.Json;
   using System.Configuration;

   public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
   {
      log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");
    
      // Get the request body
      dynamic dataArray = await req.Content.ReadAsAsync<object>();

      // Throw an HTTP Request Entity Too Large exception when the incoming batch(dataArray) is greater than 256 KB. Make sure that the size value is consistent with the value entered in the Stream Analytics portal.

      if (dataArray.ToString().Length > 262144)
      {        
         return new HttpResponseMessage(HttpStatusCode.RequestEntityTooLarge);
      }
      var connection = ConnectionMultiplexer.Connect("<your redis cache connection string goes here>");
      log.Info($"Connection string.. {connection}");
    
      // Connection refers to a property that returns a ConnectionMultiplexer
      IDatabase db = connection.GetDatabase();
      log.Info($"Created database {db}");
    
      log.Info($"Message Count {dataArray.Count}");

      // Perform cache operations using the cache object. For example, the following code block adds few integral data types to the cache
      for (var i = 0; i < dataArray.Count; i++)
      {
        string time = dataArray[i].time;
        string callingnum1 = dataArray[i].callingnum1;
        string key = time + " - " + callingnum1;
        db.StringSet(key, dataArray[i].ToString());
        log.Info($"Object put in database. Key is {key} and value is {dataArray[i].ToString()}");
       
      // Simple get of data types from the cache
      string value = db.StringGet(key);
      log.Info($"Database got: {value}");
      }

      return req.CreateResponse(HttpStatusCode.OK, "Got");
    }    

   ```

   Quando o Stream Analytics recebe a exceção "HTTP do pedido entidade demasiado grande" da função, reduz o tamanho de lotes envia para funções. Na sua função, utilize o seguinte código para verificar que o do Stream Analytics não enviar os lotes de grande dimensão. Certifique-se de que os valores de contagem e tamanho de lote máximo utilizados na função são consistentes com os valores que introduziu no portal do Stream Analytics.

   ```csharp
   if (dataArray.ToString().Length > 262144)
      {        
        return new HttpResponseMessage(HttpStatusCode.RequestEntityTooLarge);
      }
   ```

3. No editor de texto à sua escolha, crie um ficheiro JSON com o nome **project.json**. Utilize o seguinte código e guardá-lo no seu computador local. Este ficheiro contém as dependências de pacote NuGet necessárias para a função de c#.  
   
   ```json
       {
         "frameworks": {
             "net46": {
                 "dependencies": {
                     "StackExchange.Redis":"1.1.603",
                     "Newtonsoft.Json": "9.0.1"
                 }
             }
         }
     }

   ```
 
4. Volte ao portal do Azure. Do **funcionalidades da plataforma** separador, navegue até à sua função. Em **ferramentas de desenvolvimento**, selecione **Editor do serviço de aplicações**. 
 
   ![Captura de ecrã do Editor de serviço de aplicações](./media/stream-analytics-with-azure-functions/image3.png)

5. No Editor de serviço de aplicações, clique no seu diretório de raiz e carregar o **project.json** ficheiro. Depois do carregamento for bem-sucedido, atualize a página. Deverá ver um ficheiro gerado automaticamente com o nome **project.lock.json**. O ficheiro gerado automaticamente contém referências aos ficheiros. dll que são especificados no ficheiro project.json.  

   ![Captura de ecrã do Editor de serviço de aplicações](./media/stream-analytics-with-azure-functions/image4.png)

 

## <a name="update-the-stream-analytics-job-with-the-function-as-output"></a>Atualizar a tarefa de Stream Analytics com a função como saída

1. Abra a sua tarefa do Stream Analytics no portal do Azure.  

2. Navegue para a sua função e selecione **descrição geral** > **saídas** > **adicionar**. Para adicionar uma nova saída, selecione **função do Azure** para a opção de sink. O novo adaptador de saída de funções está disponível, com as seguintes propriedades:  

   |**Nome da propriedade**|**Descrição**|
   |---|---|
   |Alias de saída| Um nome amigável de utilizador que utilizar na consulta da tarefa para a referência de saída. |
   |Importar opção| Pode utilizar a função da subscrição atual, ou forneça as definições manualmente se a função estiver localizada na outra subscrição. |
   |Aplicação de Funções| Nome da sua aplicação de funções. |
   |Função| Nome da função na sua aplicação de funções (nome da sua função run.csx).|
   |Tamanho de lote máximo|Define o tamanho máximo para cada lote de saída, que é enviado para a sua função. Por predefinição, este valor é definido como 256 KB.|
   |Contagem máxima de Batch|Especifica o número máximo de eventos em cada lote que é enviado para a função. O valor predefinido é 100. Esta propriedade é opcional.|
   |Chave|Permite-lhe utilizar uma função a partir de outra subscrição. Forneça o valor da chave para aceder à sua função. Esta propriedade é opcional.|

3. Forneça um nome para o alias de saída. Neste tutorial, iremos nome **saop1** (pode utilizar qualquer nome à sua escolha). Preencha outros detalhes.  

4. Abra a sua tarefa do Stream Analytics e atualizar a consulta para o seguinte. (Certifique-se de que substitui o texto "saop1" se tem de chamar o sink de saída diferente.)  

   ```sql
    SELECT 
            System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1, 
            CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2
        INTO saop1
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
           JOIN CallStream CS2 TIMESTAMP BY CallRecTime
            ON CS1.CallingIMSI = CS2.CallingIMSI AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
        WHERE CS1.SwitchNum != CS2.SwitchNum
   ```

5. Iniciar a aplicação de telcodatagen.exe executando o seguinte comando na linha de comandos (utilize o formato `telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]`):  
   
   **telcodatagen.exe 1000 .2 2**
    
6.  Inicie a tarefa de Stream Analytics.

## <a name="check-azure-redis-cache-for-results"></a>Verifique a Cache de Redis do Azure para obter os resultados

1. Navegue até ao portal do Azure e localizar a Cache de Redis do Azure. Selecione **consola**.  

2. Utilize [comandos de cache de Redis](https://redis.io/commands) para se certificar de que os dados na cache de Redis. (O comando assume o formato Get {chave}.) Por exemplo:

   **Obter "19/12/2017 21:32:24-123414732"**

   Este comando deve imprimir o valor para o nome da chave:

   ![Captura de ecrã do Azure Redis Cache de saída](./media/stream-analytics-with-azure-functions/image5.png)

## <a name="known-issues"></a>Problemas conhecidos

No portal do Azure, quando tenta repor o tamanho de lote máximo / valor de contagem máxima de Batch para vazio (predefinição), o valor de volta a mudar para o valor introduzido anteriormente após guardar. Introduza manualmente os valores predefinidos para estes campos neste caso.

