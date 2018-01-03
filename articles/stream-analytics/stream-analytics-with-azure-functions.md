---
title: "Executar as funções do Azure com tarefas do Azure Stream Analytics | Microsoft Docs"
description: "Saiba como configurar a função do Azure como um sink de saída para as tarefas do Stream Analytics."
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
ms.openlocfilehash: adc147fc9f47e78cc0a2fcaf53f064bcfa5eee2c
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/21/2017
---
# <a name="run-azure-functions-with-azure-stream-analytics-jobs"></a>Executar as funções do Azure com tarefas do Azure Stream Analytics 
 
> [!IMPORTANT]
> Esta funcionalidade está em pré-visualização.

Pode executar as funções do Azure com o Azure Stream Analytics através da configuração das funções do Azure como um dos sinks de saída para a tarefa de Stream Analytics. A função do Azure é um evento suscitada pelo departamento, a experiência de cálculo a pedido que lhe permite implementar o código que é acionado pelos eventos que ocorrem no Azure ou serviços de terceiros. Esta capacidade de função do Azure para responder a acionadores torna uma natural saída à tarefa do Azure Stream Analytics.

O Azure Stream Analytics invoca a função do Azure através de acionadores HTTP. O adaptador de saída da função do Azure permite aos utilizadores ligar as funções do Azure Stream Analytics, de modo a que os eventos podem ser acionados baseada em consultas de Stream Analytics. 

Este tutorial demonstra como ligar o Azure Stream Analytics para [a Cache de Redis do Azure](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md) utilizando [das funções do Azure](../azure-functions/functions-overview.md). 

## <a name="configure-stream-analytics-job-to-run-an-azure-function"></a>Configurar a tarefa do Stream Analytics para executar uma função do Azure 

Esta secção demonstra como configurar uma tarefa de Stream Analytics para executar uma função do Azure que escreve dados para a Cache de Redis do Azure. A tarefa de Stream Analytics lê eventos do Hub de eventos, executa uma consulta que invoca a função do Azure. Esta função do Azure lê os dados da tarefa do Stream Analytics e escreve-o à Cache de Redis do Azure.

![Gráfico para ilustrar o tutorial](./media/stream-analytics-with-azure-functions/image1.png)

Os seguintes passos são necessários para realizar esta tarefa:
* [Crie tarefa do Stream Analytics com o Hub de eventos como entrada.](#create-stream-analytics-job-with-event-hub-as-input)  
* [Crie uma Cache de Redis do Azure.](#create-an-azure-redis-cache)  
* [Crie uma função do Azure que pode escrever dados para a Cache de Redis.](#create-an-azure-function-that-can-write-data-to-the-redis-cache)    
* [Atualize a tarefa de Stream Analytics com a função do Azure como saída.](#update-the-stream-analytic-job-with-azure-function-as-output)  
* [Verifique a Cache de Redis para obter os resultados.](#check-redis-cache-for-results)  

## <a name="create-stream-analytics-job-with-event-hub-as-input"></a>Criar tarefa do Stream Analytics com o Hub de eventos como entrada

Siga o [deteção de fraudes em tempo real](stream-analytics-real-time-fraud-detection.md) tutorial para criar um hub de eventos, iniciar a aplicação do gerador de eventos e criar um Azure Stream Analytics (ignorar os passos para criar a consulta e de saída, em vez disso, irá configurar uma As funções do Azure saída na secção seguinte.)

## <a name="create-an-azure-redis-cache"></a>Criar uma Cache de Redis do Azure

1. Criar uma Cache de Redis do Azure, utilizando os passos descritos no [criar uma cache](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache) secção do artigo de Cache de Redis.  

2. Depois de criar a Cache de Redis, navegue para a cache criada > **chaves de acesso** > e anote o **cadeia de ligação principal**.

   ![Cadeia de ligação da Cache de redis](./media/stream-analytics-with-azure-functions/image2.png)

## <a name="create-an-azure-function-that-can-write-data-to-the-redis-cache"></a>Criar uma função do Azure que pode escrever dados para a Cache de Redis

1. Utilize o [criar uma aplicação de função](../azure-functions/functions-create-first-azure-function.md#create-a-function-app) secção de documentação de funções do Azure para criar uma aplicação de função do Azure e um [função de acionada por HTTP de Azure](../azure-functions/functions-create-first-azure-function.md#create-function) (aka Webhook) utilizando **CSharp** idioma.  

2. Navegue para o **run.csx** funcionar e atualizá-lo com o seguinte código (Certifique-se de que substitui o "\<a cadeia de ligação da cache de redis aqui\>" texto com a cadeia de ligação primária da Cache de Redis que obteve na secção anterior):  

   ```c#
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

   Quando o Azure Stream Analytics recebe 413 exceção (Http do pedido entidade demasiado grande) da função do Azure, reduz o tamanho de lotes envia para as funções do Azure. Na sua função do Azure, utilize o seguinte código para verificar que o Azure Stream Analytics não enviar os lotes de grande dimensão. Certifique-se de que os valores de contagem e tamanho de lote máximo utilizados na função são consistentes com os valores que introduziu no portal do Stream Analytics.

   ```c#
   if (dataArray.ToString().Length > 262144)
      {        
        return new HttpResponseMessage(HttpStatusCode.RequestEntityTooLarge);
      }
   ```

3. No editor de texto à sua escolha, crie um ficheiro JSON com o nome **project.json** com o seguinte código e guarde-o no seu computador local. Este ficheiro contém as dependências de pacote NuGet necessárias para a função de c#.  
   
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
 
4. Volte ao portal do Azure > navegue para a função do Azure > do **funcionalidades da plataforma** separador > clique em **Editor do serviço de aplicações** que está localizado em **ferramentas de desenvolvimento**. 
 
   ![Ecrã de editor do serviço de aplicação](./media/stream-analytics-with-azure-functions/image3.png)

5. No Editor de serviço de aplicações com o botão direito clique no seu diretório de raiz e carregar o **project.json** ficheiro. Depois do carregamento for bem-sucedido, atualize a página, deverá ver um ficheiro gerado automaticamente com o nome **project.lock.json**.  O ficheiro gerado automaticamente contém referências para os dll, que são especificados no ficheiro Project.json.  

   ![Carregar ficheiro project.json](./media/stream-analytics-with-azure-functions/image4.png)

 

## <a name="update-the-stream-analytic-job-with-azure-function-as-output"></a>Atualizar a tarefa de Stream Analytics com a função do Azure como saída

1. Abra a sua tarefa do Azure Stream Analytics no portal do Azure.  

2. Navegue para a função do Azure > **descrição geral** > **saídas** > **adicionar** uma saída novo > selecione **afunçãodoAzure** para a opção de Sink. O novo adaptador de saída da função do Azure está disponível com as seguintes propriedades:  

   |**Nome da propriedade**|**Descrição**|
   |---|---|
   |Alias de saída| Um nome amigável de utilizador que utilizar na consulta da tarefa para a referência de saída. |
   |Opção de importar| Pode utilizar a função do azure da subscrição atual ou forneça as definições manualmente se a função estiver localizada na outra subscrição. |
   |Aplicação de Funções| Nome da sua aplicação de função do Azure. |
   |Função| Nome da função na sua aplicação de função (nome da sua função run.csx).|
   |Tamanho de lote máximo|Esta propriedade é utilizada para definir o tamanho máximo para cada lote de saída, que é enviado para a função do Azure. Por predefinição, este valor é definido como 256 KB.|
   |Contagem máxima de Batch|Esta propriedade permite-lhe especificar o número máximo de eventos em cada lote que é enviado para a função do Azure. O valor de contagem de lote máximo predefinido é 100. Esta propriedade é opcional.|
   |Chave|Esta propriedade permite-lhe utilizar uma função do Azure a partir de outra subscrição. Forneça o valor da chave para aceder à sua função. Esta propriedade é opcional.|

3. Forneça um nome para o alias de saída. Neste tutorial, iremos nome **saop1** (pode utilizar qualquer outro nome à sua escolha) e outros detalhes de preencher.  

4. Abra a sua tarefa do Stream Analytics e atualizar a consulta seguinte (Certifique-se de que substitui o texto "saop1" se tem de chamar o sink de saída diferente):  

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

5. Iniciar a aplicação de telcodatagen.exe executando o seguinte comando na linha de comandos (o comando assume o formato - `telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]`)  
   
   **telcodatagen.exe 1000.2 2**
    
6.  Inicie a tarefa do Stream Analytics.

## <a name="check-redis-cache-for-results"></a>Verifique a Cache de Redis para obter os resultados

1. Navegue para o portal do Azure e localize a Cache de Redis > selecione **consola**.  

2. Utilize [comandos de cache de Redis](https://redis.io/commands) para se certificar de que os dados na cache de Redis. (O comando recebe Get formato {chave}). Por exemplo:

   **Obter "19/12/2017 21:32:24-123414732"**

   Este comando deve imprimir o valor para o nome da chave:

   ![Saída da Cache de redis](./media/stream-analytics-with-azure-functions/image5.png)

## <a name="known-issues"></a>Problemas conhecidos

* No portal do Azure, quando tenta repor o tamanho de lote máximo / valor de contagem máxima de Batch para empty(default), volta a mudar para o valor introduzido anteriormente após guardar. Deliberadamente introduza neste caso, os valores predefinidos para estes campos.

## <a name="next-steps"></a>Passos Seguintes
