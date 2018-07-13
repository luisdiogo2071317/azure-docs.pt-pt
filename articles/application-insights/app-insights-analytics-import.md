---
title: Importar os seus dados para análise no Application Insights do Azure | Documentos da Microsoft
description: Importar dados estáticos para associar com telemetria de aplicações ou importar um fluxo de dados separado para consulta com a análise.
services: application-insights
keywords: Abra o esquema, a importação de dados
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 10/04/2017
ms.author: mbullwin
ms.openlocfilehash: d891cd92e70d3491ee0c7a58f1409823301b299c
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38989762"
---
# <a name="import-data-into-analytics"></a>Importar dados para análise

Importar dados de tabela em [Analytics](app-insights-analytics.md), seja para a associar com [Application Insights](app-insights-overview.md) telemetria da sua aplicação, ou, para que pode analisá-los como um fluxo separado. A análise é uma linguagem de consulta poderosa adequada para analisar fluxos de timestamped de grande volume de telemetria.

Pode importar dados para análise com o seu próprio esquema. Não tem de utilizar os esquemas padrão do Application Insights, como o pedido ou de rastreio.

Pode importar JSON ou DSV (delimitador de valores separados - vírgula, ponto e vírgula ou separador) ficheiros.

Há três situações em que a importação de análise é útil:

* **Junte-se com a telemetria da aplicação.** Por exemplo, pode importar uma tabela que mapeia os URLs do seu Web site para títulos de página mais legíveis. No Analytics, pode criar um relatório de gráfico de dashboard que mostra as dez páginas mais populares no seu Web site. Agora ele pode mostrar os títulos de página em vez dos URLs.
* **Correlacionar a telemetria da sua aplicação** com outras origens, como o tráfego de rede, dados do servidor ou CDN ficheiros de registo.
* **Aplicam-se a análise para um fluxo de dados separado.** O Application Insights Analytics é uma ferramenta poderosa, que funciona bem com fluxos de timestamped dispersa, - muito melhores do que o SQL em muitos casos. Se tiver um fluxo de tal de outra origem, pode analisá-los com a análise.

Enviar dados para a sua origem de dados é fácil. 

1. (Uma vez) Defina o esquema dos seus dados na "origem de dados".
2. (Periodicamente) Carregar os dados para o armazenamento do Azure e chamar a API REST para notificar-nos de que está a aguardar para ingestão de novos dados. Em poucos minutos, os dados estão disponíveis para consulta do Analytics.

A frequência do carregamento é definida por si e com que velocidade gostaria que os seus dados estejam disponíveis para consultas. É mais eficiente para carregar dados em blocos maiores, mas não superior a 1GB.

> [!NOTE]
> *Tem muitas origens de dados para analisar?* [*Considere a utilização* logstash *entregar os seus dados para o Application Insights.*](https://github.com/Microsoft/logstash-output-application-insights)
> 

## <a name="before-you-start"></a>Antes de começar

É necessário:

1. Um recurso do Application Insights no Microsoft Azure.

 * Se quiser analisar os seus dados em separado a partir de qualquer outra telemetria [criar um novo recurso do Application Insights](app-insights-create-new-resource.md).
 * Se estiver ingressando ou comparando-os seus dados através de telemetria a partir de uma aplicação que já está configurada com o Application Insights, em seguida, pode utilizar o recurso para essa aplicação.
 * Acesso de Contribuidor ou proprietário a esse recurso.
 
2. Armazenamento do Azure. Carregar para o armazenamento do Azure e análise obtém seus dados a partir daí. 

 * Recomendamos que crie uma conta de armazenamento dedicado para os blobs. Se os blobs são partilhados com outros processos, demora mais tempo para nossos processos ler os blobs.


## <a name="define-your-schema"></a>Na definição do esquema

Antes de pode importar dados, tem de definir uma *origem de dados,* que especifica o esquema dos seus dados.
Pode ter até 50 origens de dados num único recurso do Application Insights

1. Inicie o Assistente de origem de dados. Utilize o botão "Adicionar nova origem de dados". Em alternativa - clique no botão de configurações no canto superior direito e selecione "Origens de dados" no menu pendente.

    ![Adicionar nova origem de dados](./media/app-insights-analytics-import/add-new-data-source.png)

    Forneça um nome para a sua nova origem de dados.

2. Defina o formato dos ficheiros que irá carregar.

    Pode definir o formato manualmente ou carregar um ficheiro de exemplo.

    Se os dados estão no formato CSV, a primeira linha do exemplo pode ser cabeçalhos de coluna. Pode alterar os nomes de campos no próximo passo.

    O exemplo deve incluir, pelo menos, 10 linhas ou registos de dados.

    Nomes de coluna ou o campo devem ter nomes de alfanuméricos (sem espaços ou pontuação).

    ![Carregar um ficheiro de exemplo](./media/app-insights-analytics-import/sample-data-file.png)


3. Reveja o esquema que tem o assistente. Se ele inferida os tipos a partir de um exemplo, poderá ter de ajustar os tipos inferidos das colunas.

    ![Reveja o esquema inferido](./media/app-insights-analytics-import/data-source-review-schema.png)

 * (Opcional.) Carregar uma definição de esquema. Ver o formato abaixo.

 * Selecione um carimbo. Todos os dados no Analytics tem de ter um campo timestamp. Tem de ter tipo `datetime`, mas não tem de ter o nome "timestamp". Se os dados tiverem uma coluna que contém uma data e hora no formato ISO, escolha esta opção como a coluna timestamp. Caso contrário, escolha "como dados chegaram" e o processo de importação irá adicionar um campo timestamp.

5. Crie a origem de dados.

### <a name="schema-definition-file-format"></a>Formato de ficheiro de definição de esquema

Em vez de editar o esquema na interface do Usuário, é possível carregar a definição de esquema de um ficheiro. O formato de definição de esquema é o seguinte: 

Formato delimitado 
```
[ 
    {"location": "0", "name": "RequestName", "type": "string"}, 
    {"location": "1", "name": "timestamp", "type": "datetime"}, 
    {"location": "2", "name": "IPAddress", "type": "string"} 
] 
```

Formato JSON 
```
[ 
    {"location": "$.name", "name": "name", "type": "string"}, 
    {"location": "$.alias", "name": "alias", "type": "string"}, 
    {"location": "$.room", "name": "room", "type": "long"} 
]
```
 
Cada coluna é identificada pela localização, o nome e o tipo.

* Localização – para o ficheiro delimitado formatá-lo é a posição do valor mapeada. Para o formato JSON, é o jpath da chave mapeada.
* Nome – o nome apresentado da coluna.
* Tipo – o tipo de dados dessa coluna.
 
> [!NOTE]
> No caso de dados de exemplo foi utilizados e o formato de arquivo é delimitado, a definição de esquema tem de mapear todas as colunas e adicionar novas colunas no final.
> 
> JSON permite o mapeamento parcial dos dados, por isso a definição de esquema com um formato JSON não tem de mapear cada chave que se encontra nos dados de exemplo. Ele também pode mapear colunas que não fazem parte dos dados de exemplo. 

## <a name="import-data"></a>Importar dados

Para importar dados, carregá-lo para o armazenamento do Azure, criar uma chave de acesso para o mesmo e, em seguida, fazer uma chamada de REST API.

![Adicionar nova origem de dados](./media/app-insights-analytics-import/analytics-upload-process.png)

Pode efetuar o seguinte processo manualmente ou configurar um sistema automatizado para fazê-lo em intervalos regulares. Tem de seguir estes passos para cada bloco de dados que pretende importar.

1. Carregar os dados para [armazenamento de Blobs do Azure](../storage/blobs/storage-dotnet-how-to-use-blobs.md). 

 * Os BLOBs podem ser qualquer um tamanho até 1GB descomprimido. Blobs grandes de centenas de MB são ideais da perspectiva do desempenho.
 * Pode comprimi-los com Gzip para melhorar o tempo de carregamento e a latência para os dados estejam disponíveis para consulta. Utilize o `.gz` extensão de nome de ficheiro.
 * É melhor usar uma conta de armazenamento separada para esse fim, para evitar chamadas a partir de diferentes serviços um desempenho mais lento.
 * Quando enviar dados em alta frequência, cada alguns segundos, recomenda-se para utilizar mais do que uma conta de armazenamento, por motivos de desempenho.

 
2. [Criar uma chave de assinatura de acesso partilhado para o blob](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md). A chave deve ter um período de expiração de um dia e fornecer acesso de leitura.
3. Fazer uma chamada REST para notificar o Application Insights, que está a aguardar a dados.

 * Ponto final: `https://dc.services.visualstudio.com/v2/track`
 * Método HTTP: POST
 * Payload de:

```JSON

    {
       "data": {
            "baseType":"OpenSchemaData",
            "baseData":{
               "ver":"2",
               "blobSasUri":"<Blob URI with Shared Access Key>",
               "sourceName":"<Schema ID>",
               "sourceVersion":"1.0"
             }
       },
       "ver":1,
       "name":"Microsoft.ApplicationInsights.OpenSchema",
       "time":"<DateTime>",
       "iKey":"<instrumentation key>"
    }
```

Os marcadores de posição são:

* `Blob URI with Shared Access Key`: Recebe isso do procedimento para criar uma chave. É específico para o blob.
* `Schema ID`: O ID de esquema gerado para seu esquema definido. Os dados neste blob devem estar em conformidade com o esquema.
* `DateTime`: O tempo em que o pedido ser submetido, UTC. Aceitamos esses formatos: ISO8601 (como "2016-01-01 13:45:01"); Rfc822 ("Quarta-feira, 16 de Dezembro de 14 14:57:01 + 0000"); RFC850 ("Quarta-feira, 14-Dezembro-16 57: UTC+14:00 UTC"); RFC1123 ("Quarta-feira, 14 de Dezembro de 2016 57: UTC+14:00 + 0000").
* `Instrumentation key` do seu recurso do Application Insights.

Os dados estão disponíveis no Analytics após alguns minutos.

## <a name="error-responses"></a>Respostas de erro

* **pedido 400 inválido**: indica que o payload de pedido é inválido. Verifique:
 * Chave de instrumentação correto.
 * Valor de hora válido. Deve ser a hora em UTC.
 * Está em conformidade com o esquema JSON do evento.
* **403 Proibido**: O blob que enviou não está acessível. Certifique-se de que a chave de acesso partilhado é válida e não expirou.
* **404 não encontrado**:
 * O blob não existe.
 * O sourceId é errado.

Informações mais detalhadas estão disponíveis na mensagem de erro de resposta.


## <a name="sample-code"></a>Código de exemplo

Este código utiliza a [newtonsoft](https://www.nuget.org/packages/Newtonsoft.Json/9.0.1) pacote NuGet.

### <a name="classes"></a>Classes

```csharp
namespace IngestionClient 
{ 
    using System; 
    using Newtonsoft.Json; 

    public class AnalyticsDataSourceIngestionRequest 
    { 
        #region Members 
        private const string BaseDataRequiredVersion = "2"; 
        private const string RequestName = "Microsoft.ApplicationInsights.OpenSchema"; 
        #endregion Members 

        public AnalyticsDataSourceIngestionRequest(string ikey, string schemaId, string blobSasUri, int version = 1) 
        { 
            Ver = version; 
            IKey = ikey; 
            Data = new Data 
            { 
                BaseData = new BaseData 
                { 
                    Ver = BaseDataRequiredVersion, 
                    BlobSasUri = blobSasUri, 
                    SourceName = schemaId, 
                    SourceVersion = version.ToString() 
                } 
            }; 
        } 


        [JsonProperty("data")] 
        public Data Data { get; set; } 

        [JsonProperty("ver")] 
        public int Ver { get; set; } 

        [JsonProperty("name")] 
        public string Name { get { return RequestName; } } 

        [JsonProperty("time")] 
        public DateTime Time { get { return DateTime.UtcNow; } } 

        [JsonProperty("iKey")] 
        public string IKey { get; set; } 
    } 

    #region Internal Classes 

    public class Data 
    { 
        private const string DataBaseType = "OpenSchemaData"; 

        [JsonProperty("baseType")] 
        public string BaseType 
        { 
            get { return DataBaseType; } 
        } 

        [JsonProperty("baseData")] 
        public BaseData BaseData { get; set; } 
    } 


    public class BaseData 
    { 
        [JsonProperty("ver")] 
        public string Ver { get; set; } 

        [JsonProperty("blobSasUri")] 
        public string BlobSasUri { get; set; } 

        [JsonProperty("sourceName")] 
        public string SourceName { get; set; } 

        [JsonProperty("sourceVersion")] 
        public string SourceVersion { get; set; } 
    } 

    #endregion Internal Classes 
} 


namespace IngestionClient 
{ 
    using System; 
    using System.IO; 
    using System.Net; 
    using System.Text; 
    using System.Threading.Tasks; 
    using Newtonsoft.Json; 

    public class AnalyticsDataSourceClient 
    { 
        #region Members 
        private readonly Uri endpoint = new Uri("https://dc.services.visualstudio.com/v2/track"); 
        private const string RequestContentType = "application/json; charset=UTF-8"; 
        private const string RequestAccess = "application/json"; 
        #endregion Members 

        #region Public 

        public async Task<bool> RequestBlobIngestion(AnalyticsDataSourceIngestionRequest ingestionRequest) 
        { 
            HttpWebRequest request = WebRequest.CreateHttp(endpoint); 
            request.Method = WebRequestMethods.Http.Post; 
            request.ContentType = RequestContentType; 
            request.Accept = RequestAccess; 

            string notificationJson = Serialize(ingestionRequest); 
            byte[] notificationBytes = GetContentBytes(notificationJson); 
            request.ContentLength = notificationBytes.Length; 

            Stream requestStream = request.GetRequestStream(); 
            requestStream.Write(notificationBytes, 0, notificationBytes.Length); 
            requestStream.Close(); 

            try 
            { 
                using (var response = (HttpWebResponse)await request.GetResponseAsync())
                {
                    return response.StatusCode == HttpStatusCode.OK;
                }
            } 
            catch (WebException e) 
            { 
                HttpWebResponse httpResponse = e.Response as HttpWebResponse; 
                if (httpResponse != null) 
                { 
                    Console.WriteLine( 
                        "Ingestion request failed with status code: {0}. Error: {1}", 
                        httpResponse.StatusCode, 
                        httpResponse.StatusDescription); 
                    return false; 
                }
                throw; 
            } 
        } 
        #endregion Public 

        #region Private 
        private byte[] GetContentBytes(string content) 
        { 
            return Encoding.UTF8.GetBytes(content);
        } 


        private string Serialize(AnalyticsDataSourceIngestionRequest ingestionRequest) 
        { 
            return JsonConvert.SerializeObject(ingestionRequest); 
        } 
        #endregion Private 
    } 
} 
```

### <a name="ingest-data"></a>Ingerir dados

Utilize este código para cada blob. 

```csharp
   AnalyticsDataSourceClient client = new AnalyticsDataSourceClient(); 

   var ingestionRequest = new AnalyticsDataSourceIngestionRequest("iKey", "sourceId", "blobUrlWithSas"); 

   bool success = await client.RequestBlobIngestion(ingestionRequest);
```

## <a name="next-steps"></a>Passos Seguintes

* [Visita guiada da linguagem de consulta do Log Analytics](app-insights-analytics-tour.md)
* Se estiver a utilizar o Logstash, utilize o [Plug-in do Logstash para enviar dados para o Application Insights](https://github.com/Microsoft/logstash-output-application-insights)
