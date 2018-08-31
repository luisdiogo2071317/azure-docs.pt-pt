---
title: Consultar dados Avro com o Azure Data Lake Analytics | Documentos da Microsoft
description: Utilize as propriedades do corpo de mensagem para encaminhar a telemetria do dispositivo para o armazenamento de BLOBs e consultar os dados de formato Avro que são escritos no armazenamento de Blobs.
author: ash2017
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: asrastog
ms.openlocfilehash: a17df39c55b5c02c83e3f0b74a91d7109ddb4d3d
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/29/2018
ms.locfileid: "43188949"
---
# <a name="query-avro-data-by-using-azure-data-lake-analytics"></a>Consultar dados Avro com o Azure Data Lake Analytics

Este artigo discute como consultar dados Avro para encaminhar eficientemente mensagens do IoT Hub do Azure aos serviços do Azure. À medida, anunciámos a mensagem de blogue [encaminhamento de mensagens do IoT Hub do Azure: agora com encaminhamento do corpo da mensagem], suporta o IoT Hub encaminhamento nas propriedades ou o corpo da mensagem. Para obter mais informações, consulte [roteamento de corpos de mensagem][Routing on message bodies]. 

O desafio foi que quando o IoT Hub do Azure redireciona as mensagens para o armazenamento de Blobs do Azure, o IoT Hub grava o conteúdo no formato Avro, que tem uma propriedade de corpo de mensagem e uma propriedade da mensagem. IoT Hub suporta escrever os dados no armazenamento de BLOBs apenas no formato de dados Avro, e esse formato não é utilizado para outros pontos de extremidade. Para obter mais informações, consulte [ao utilizar contentores de armazenamento do Azure][When using Azure storage containers]. Embora o formato Avro é ótimo para preservação de dados e a mensagem, é um desafio para utilizá-lo para consultar dados. Em comparação, o formato JSON ou CSV é muito mais fácil para consultar dados.

Para resolver necessidades de grandes volumes de dados não relacionais e formatos e superar esse desafio, pode utilizar muitos dos padrões de grandes volumes de dados para transformar e dados de dimensionamento. Um dos padrões, "pagar por consulta," é o Azure Data Lake Analytics, que é o foco deste artigo. Embora seja possível executar a consulta facilmente no Hadoop ou outras soluções, o Data Lake Analytics é costuma ser mais bem adequado para essa abordagem de "pagar por consulta". 

Há um "extrator" para Avro em U-SQL. Para obter mais informações, consulte [exemplo de U-SQL Avro].

## <a name="query-and-export-avro-data-to-a-csv-file"></a>Consultar e exportar dados Avro para um ficheiro CSV
Nesta secção, consultar dados Avro e exportá-lo para um ficheiro CSV no armazenamento de Blobs do Azure, embora pode colocar facilmente os dados em outros armazenamentos de dados ou de repositórios.

1. Configure o IoT Hub do Azure, para encaminhar os dados para um ponto de final de armazenamento de Blobs do Azure, usando uma propriedade no corpo da mensagem para selecionar as mensagens.

    ![A secção "Pontos finais personalizados"][img-query-avro-data-1a]

    ![O comando de rotas][img-query-avro-data-1b]

2. Certifique-se de que o dispositivo tem a codificação, tipo de conteúdo e os dados necessários no corpo da mensagem, conforme referenciado na documentação do produto ou as propriedades. Quando exibir esses atributos no Device Explorer, conforme mostrado aqui, pode verificar se estão definidos corretamente.

    ![O painel de dados do Hub de eventos][img-query-avro-data-2]

3. Configure uma instância do Azure Data Lake Store e uma instância do Data Lake Analytics. O IoT Hub do Azure não é encaminhado para uma instância do Data Lake Store, mas uma instância do Data Lake Analytics requer uma.

    ![Instâncias do Data Lake Store e do Data Lake Analytics][img-query-avro-data-3]

4. No Data Lake Analytics, configure o armazenamento de Blobs do Azure como um armazenamento adicional, o mesmo armazenamento de BLOBs que encaminha dados para o IoT Hub do Azure.

    ![O painel "Origens de dados"][img-query-avro-data-4]
 
5. Conforme discutido [Exemplo de U-SQL Avro], terá quatro arquivos DLL. Carregar estes ficheiros para uma localização na sua instância do Data Lake Store.

    ![Quatro arquivos DLL carregados][img-query-avro-data-5] 

6. No Visual Studio, crie um projeto de U-SQL.
 
    ![Criar um projeto U-SQL][img-query-avro-data-6]

7. Cole o conteúdo do seguinte script no ficheiro recentemente criado. Modificar as três secções realçadas: conta do Data Lake Analytics, os caminhos de ficheiro DLL associados e o caminho correto para a sua conta de armazenamento.
    
    ![As três seções para ser modificado][img-query-avro-data-7a]

    O script de U-SQL real para saída simple para um ficheiro CSV:
    
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

    Demorou do Data Lake Analytics para executar o script seguinte, o que estava limitado a 10 unidades de análise e processados 177 ficheiros de cinco minutos. O resultado é mostrado no resultado do ficheiro CSV que é apresentado na imagem seguinte:
    
    ![Resultados da saída para o ficheiro CSV][img-query-avro-data-7b]

    ![Convertido em arquivo CSV de saída][img-query-avro-data-7c]

    Para analisar o JSON, avance para o passo 8.
    
8. A maioria das mensagens de IoT estão no formato de ficheiro JSON. Ao adicionar as seguintes linhas, pode analisar a mensagem para um ficheiro JSON, que permite adicionar cláusulas WHERE e apenas os dados necessários de saída.

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

    A saída apresenta uma coluna para cada item no `SELECT` comando. 
    
    ![Mostrar uma coluna para cada item de saída][img-query-avro-data-8]

## <a name="next-steps"></a>Passos Seguintes
Neste tutorial, aprendeu a consultar dados Avro para encaminhar eficientemente mensagens do IoT Hub do Azure aos serviços do Azure.

Para obter exemplos de soluções ponto a ponto completas que utilizam o IoT Hub, veja [solution accelerator do monitoramento remoto do Azure IoT][lnk-iot-sa-land].

Para saber mais sobre o desenvolvimento de soluções com o IoT Hub, veja a [guia para programadores do IoT Hub].

Para saber mais sobre o roteamento de mensagens do IoT Hub, veja [enviar e receber mensagens com o IoT Hub][lnk-devguide-messaging].

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

[Exemplo de U-SQL Avro]:https://github.com/Azure/usql/tree/master/Examples/AvroExamples

[lnk-iot-sa-land]: ../iot-accelerators/index.yml
[guia para programadores do IoT Hub]: iot-hub-devguide.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
