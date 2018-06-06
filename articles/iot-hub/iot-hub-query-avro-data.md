---
title: Consultar dados Avro utilizando o Azure Data Lake Analytics | Microsoft Docs
description: Utilize as propriedades de corpo de mensagem a telemetria do dispositivo de rota para armazenamento de BLOBs e consultar os dados de formato Avro escritos para armazenamento de Blobs.
services: iot-hub
documentationcenter: ''
author: ksaye
manager: obloch
ms.service: iot-hub
ms.topic: article
ms.date: 05/29/2018
ms.author: Kevin.Saye
ms.openlocfilehash: 98a30155c73a937042b4bea6568543fb5152d748
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34727974"
---
# <a name="query-avro-data-using-azure-data-lake-analytics"></a>Consultar dados Avro utilizando o Azure Data Lake Analytics

Este artigo é sobre como consultar os dados Avro de forma eficiente encaminhamento de mensagens do IoT Hub do Azure aos serviços do Azure. Seguir o anúncio de mensagem de blogue —[encaminhamento de mensagens do IoT Hub do Azure: agora com encaminhamento do corpo da mensagem], suporta o IoT Hub encaminhamento propriedades ou o corpo da mensagem. Consulte também [encaminhamento corpos de mensagem][Routing on message bodies]. 

O desafio foi que ao IoT Hub do Azure redireciona as mensagens para o blob storage, o IoT Hub escreve o conteúdo no formato Avro, que tem o corpo da mensagem e propriedades da mensagem. Tenha em atenção que o IoT Hub apenas suporta a escrita de dados ao blob storage, no formato de dados Avro e este formato não é utilizado para quaisquer outros pontos finais. Consulte [quando utilizar contentores de armazenamento do Azure][When using Azure storage containers]. Apesar do formato Avro é ótimo para e a preservação/mensagens de dados, é difícil para consultar os dados. Em comparação, o formato JSON ou CSV é muito mais fácil para consultar os dados.

Para resolver isto, pode utilizar muitos dos padrões de macrodados para transformar e dimensionamento dados para abordar as necessidades de macrodados não relacionais e formatos. Um dos padrões, um padrão "pague por consulta", é o Azure Data Lake Analytics (ADLA). É o objetivo deste artigo. Embora facilmente foi possível executar a consulta no Hadoop ou outras soluções, ADLA frequentemente melhor é adequada para esta abordagem "pagar por consulta". Não há um "extrator" para Avro em U-SQL. Consulte [Exemplo de Avro U-SQL].

## <a name="query-and-export-avro-data-to-a-csv-file"></a>Consultar e exportar dados Avro para um ficheiro CSV
A secção explica-lhe como consultar dados Avro e exportá-lo para um ficheiro CSV no armazenamento de Blobs do Azure, embora facilmente foi possível colocar os dados nos outros arquivos de dados ou repositórios.

1. Configure o IoT Hub do Azure para dados da rota para um ponto de final do Blob Storage do Azure com uma propriedade no corpo da mensagem para selecionar as mensagens.

    ![Captura de ecrã para o passo 1a][img-query-avro-data-1a]

    ![Captura de ecrã para o passo 1b][img-query-avro-data-1b]

2. Certifique-se de que o dispositivo tem a codificação, o tipo de conteúdo e os dados necessários no corpo da mensagem, tal como é referido na documentação do produto ou as propriedades. Quando visualizadas no Explorador de dispositivo (ver abaixo), pode verificar-se de que estes atributos estão definidos corretamente.

    ![Captura de ecrã para o passo 2][img-query-avro-data-2]

3. Configure um Azure Data Lake Store (ADLS) e uma instância do Azure Data Lake Analytics. Enquanto o IoT Hub do Azure não encaminhar para um Azure Data Lake Store, ADLA requer um.

    ![Captura de ecrã para o passo 3][img-query-avro-data-3]

4. ADLA, configure o armazenamento de Blobs do Azure como um armazenamento adicional, o mesmo armazenamento de BLOBs que encaminha dados para o IoT Hub do Azure.

    ![Captura de ecrã para o passo 4][img-query-avro-data-4]
 
5. Tal como explicado [Exemplo de Avro U-SQL], existem 4 DLLs que são necessários.  Carregar estes ficheiros para uma localização na sua ADLS.

    ![Captura de ecrã para o passo 5][img-query-avro-data-5] 

6. No Visual Studio, crie um projeto U-SQL
 
    ![Captura de ecrã no passo 6][img-query-avro-data-6]

7. Copiar o conteúdo do seguinte script e cole-o ficheiro criado recentemente. Modificar as 3 secções realçadas: a conta ADLA, caminhos dos DLLs associados e o caminho correto para a sua conta de armazenamento.
    
    ![Captura de ecrã para o passo 7a][img-query-avro-data-7a]

    O script U-SQL real para o resultado simple para CSV:
    
    ```sql
        DROP ASSEMBLY IF EXISTS [Avro];
        CREATE ASSEMBLY [Avro] FROM @"/Assemblies/Avro/Avro.dll";
        DROP ASSEMBLY IF EXISTS [Microsoft.Analytics.Samples.Formats];
        CREATE ASSEMBLY [Microsoft.Analytics.Samples.Formats] FROM @"/Assemblies/Avro/Microsoft.Analytics.Samples.Formats.dll";
        DROP ASSEMBLY IF EXISTS [Newtonsoft.Json];
        CREATE ASSEMBLY [Newtonsoft.Json] FROM @"/Assemblies/Avro/Newtonsoft.Json.dll";
        DROP ASSEMBLY IF EXISTS [log4net];
        CREATE ASSEMBLY [log4net] FROM @"/Assemblies/Avro/log4net.dll";

        REFERENCE ASSEMBLY [Newtonsoft.Json];
        REFERENCE ASSEMBLY [log4net];
        REFERENCE ASSEMBLY [Avro];
        REFERENCE ASSEMBLY [Microsoft.Analytics.Samples.Formats];

        // Blob container storage account filenames, with any path
        DECLARE @input_file string = @"wasb://hottubrawdata@kevinsayazstorage/kevinsayIoT/{*}/{*}/{*}/{*}/{*}/{*}";
        DECLARE @output_file string = @"/output/output.csv";

        @rs =
        EXTRACT
        EnqueuedTimeUtc string,
        Body byte[]
        FROM @input_file

        USING new Microsoft.Analytics.Samples.Formats.ApacheAvro.AvroExtractor(@"
        {
        ""type"":""record"",
        ""name"":""Message"",
        ""namespace"":""Microsoft.Azure.Devices"",
        ""fields"":[{
        ""name"":""EnqueuedTimeUtc"",
        ""type"":""string""
        },
        {
        ""name"":""Properties"",
        ""type"":{
        ""type"":""map"",
        ""values"":""string""
        }
        },
        {
        ""name"":""SystemProperties"",
        ""type"":{
        ""type"":""map"",
        ""values"":""string""
        }
        },
        {
        ""name"":""Body"",
        ""type"":[""null"",""bytes""]
        }
        ]
        }");

        @cnt =
        SELECT EnqueuedTimeUtc AS time, Encoding.UTF8.GetString(Body) AS jsonmessage
        FROM @rs;

        OUTPUT @cnt TO @output_file USING Outputters.Text(); 
    ```    

    Executar o script abaixo, ADLA demorou 5 minutos quando limitado a 10 unidades analíticas e processados 177 ficheiros, resumir a saída para um ficheiro CSV.
    
    ![Captura de ecrã para o passo 7b][img-query-avro-data-7b]

    Ver o resultado, pode ver que o conteúdo de Avro foi convertido para um ficheiro CSV. Se pretender analisar o JSON, continue para o passo 8.
    
    ![Captura de ecrã para o passo 7c][img-query-avro-data-7c]

    
8. A maioria das mensagens de IoT estão no formato JSON.  Adicionar as seguintes linhas, pode analisar a mensagem em JSON, para que possa adicionar cláusulas WHERE e saída apenas os dados necessários.

    ```sql
       @jsonify = SELECT Microsoft.Analytics.Samples.Formats.Json.JsonFunctions.JsonTuple(Encoding.UTF8.GetString(Body)) AS message FROM @rs;
    
        /*
        @cnt =
            SELECT EnqueuedTimeUtc AS time, Encoding.UTF8.GetString(Body) AS jsonmessage
            FROM @rs;
        
        OUTPUT @cnt TO @output_file USING Outputters.Text();
        */
        
        @cnt =
            SELECT message["message"] AS iotmessage,
                   message["event"] AS msgevent,
                   message["object"] AS msgobject,
                   message["status"] AS msgstatus,
                   message["host"] AS msghost
            FROM @jsonify;
            
        OUTPUT @cnt TO @output_file USING Outputters.Text();
    ```

9. Ver o resultado, agora verá colunas para cada item no comando selecionado. 
    
    ![Captura de ecrã para o passo 8][img-query-avro-data-8]

## <a name="next-steps"></a>Passos Seguintes
Neste tutorial, aprendeu a consultar dados Avro de forma eficiente encaminhamento de mensagens do IoT Hub do Azure aos serviços do Azure.

Para ver os exemplos de soluções ponto-a-ponto completas que utilizam o IoT Hub, consulte [acelerador de solução de monitorização remota do Azure IoT][lnk-iot-sa-land].

Para obter mais informações sobre como desenvolver soluções de IoT hub, consulte o [guia para programadores do IoT Hub].

Para saber mais sobre o encaminhamento de mensagens no IoT Hub, veja [enviar e receber mensagens com o IoT Hub][lnk-devguide-messaging].

<!-- Images -->
[img-query-avro-data-1a]: ./media/iot-hub-query-avro-data/query-avro-data-1a.png
[img-query-avro-data-1b]: ./media/iot-hub-query-avro-data/query-avro-data-1b.png
[img-query-avro-data-2]: ./media/iot-hub-query-avro-data/query-avro-data-2.png
[img-query-avro-data-3]: ./media/iot-hub-query-avro-data/query-avro-data-3.png
[img-query-avro-data-4]: ./media/iot-hub-query-avro-data/query-avro-data-4.png
[img-query-avro-data-5]: ./media/iot-hub-query-avro-data/query-avro-data-5.png
[img-query-avro-data-6]: ./media/iot-hub-query-avro-data/query-avro-data-6.png
[img-query-avro-data-7a]: ./media/iot-hub-query-avro-data/query-avro-data-7a.png
[img-query-avro-data-7b]: ./media/iot-hub-query-avro-data/query-avro-data-7b.png
[img-query-avro-data-7c]: ./media/iot-hub-query-avro-data/query-avro-data-7c.png
[img-query-avro-data-8]: ./media/iot-hub-query-avro-data/query-avro-data-8.png

<!-- Links -->
[encaminhamento de mensagens do IoT Hub do Azure: agora com encaminhamento do corpo da mensagem]: https://azure.microsoft.com/blog/iot-hub-message-routing-now-with-routing-on-message-body/

[Routing on message bodies]: iot-hub-devguide-query-language.md#routing-on-message-bodies
[When using Azure storage containers]:iot-hub-devguide-endpoints.md#when-using-azure-storage-containers

[Exemplo de Avro U-SQL]:https://github.com/Azure/usql/tree/master/Examples/AvroExamples

[lnk-iot-sa-land]: ../iot-accelerators/index.md
[guia para programadores do IoT Hub]: iot-hub-devguide.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
