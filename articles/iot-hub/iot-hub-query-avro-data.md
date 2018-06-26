---
title: Consultar dados Avro, utilizando o Azure Data Lake Analytics | Microsoft Docs
description: Utilize as propriedades de corpo de mensagem para encaminhar telemetria do dispositivo para o Blob storage e consultar os dados de formato Avro escrito para o Blob storage.
services: iot-hub
documentationcenter: ''
author: ksaye
manager: obloch
ms.service: iot-hub
ms.topic: article
ms.date: 05/29/2018
ms.author: Kevin.Saye
ms.openlocfilehash: 08aed809184cbb65d632e1fb6f4b9bd25747e349
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36751079"
---
# <a name="query-avro-data-by-using-azure-data-lake-analytics"></a>Consultar dados Avro, utilizando o Azure Data Lake Analytics

Este artigo aborda como consultar dados Avro eficientemente encaminhar mensagens do IoT Hub do Azure aos serviços do Azure. Como podemos anunciou o blogue [encaminhamento de mensagens do IoT Hub do Azure: agora com encaminhamento do corpo da mensagem], suporta o IoT Hub encaminhamento propriedades ou o corpo da mensagem. Para obter mais informações, consulte [encaminhamento corpos de mensagem][Routing on message bodies]. 

O desafio foi que ao IoT Hub do Azure redireciona as mensagens para o armazenamento de Blobs do Azure, o IoT Hub escreve o conteúdo no formato Avro, que tem uma propriedade de corpo de mensagem e uma propriedade de mensagem. IoT Hub suporta escrever os dados para o Blob storage apenas no formato de dados Avro e este formato não é utilizado para quaisquer outros pontos finais. Para obter mais informações, consulte [quando utilizar contentores de armazenamento do Azure][When using Azure storage containers]. Embora o formato Avro é ótimo para dados e a mensagem e a preservação, é um desafio utilizá-la para consultar dados. Em comparação, o formato JSON ou CSV é muito mais fácil para consultar os dados.

Para resolver formatos e não relacionais necessidades de macrodados e ultrapassar este desafio, pode utilizar muitos dos padrões de macrodados para transformar e dimensionamento de dados. Um dos padrões de "pagar por consulta," é o Azure Data Lake Analytics, que é o objetivo deste artigo. Embora pode facilmente executar a consulta no Hadoop ou outras soluções, o Data Lake Analytics, muitas vezes, melhor é adequado para esta abordagem "pagar por consulta". 

Não há um "extrator" para Avro em U-SQL. Para obter mais informações, consulte [Exemplo de Avro U-SQL].

## <a name="query-and-export-avro-data-to-a-csv-file"></a>Consultar e exportar dados Avro para um ficheiro CSV
Nesta secção, consultar dados Avro e exportá-lo para um ficheiro CSV no Blob storage do Azure, embora facilmente foi possível colocar os dados nos outros arquivos de dados ou repositórios.

1. Configure IoT Hub do Azure para dados da rota para um ponto final de armazenamento de Blobs do Azure através da utilização de uma propriedade no corpo da mensagem para selecionar as mensagens.

    ![A secção "Pontos finais os personalizado"][img-query-avro-data-1a]

    ![O comando de rotas][img-query-avro-data-1b]

2. Certifique-se de que o dispositivo tem codificação, tipo de conteúdo e os dados necessários no corpo da mensagem, tal como é referido na documentação do produto ou as propriedades. Quando visualiza estes atributos no Explorador de dispositivos, conforme mostrado aqui, pode verificar estão definidas corretamente.

    ![O painel de dados de Hub de eventos][img-query-avro-data-2]

3. Configure uma instância do Azure Data Lake Store e uma instância do Data Lake Analytics. IoT Hub do Azure não encaminhar para uma instância do Data Lake Store, mas uma instância do Data Lake Analytics requer um.

    ![Instâncias do Data Lake Store e do Data Lake Analytics][img-query-avro-data-3]

4. No Data Lake Analytics, configure o armazenamento de Blobs do Azure como um armazenamento adicional, o mesmo armazenamento de BLOBs que encaminha dados para o IoT Hub do Azure.

    ![O painel de "Origens de dados"][img-query-avro-data-4]
 
5. Tal como explicado [Exemplo de Avro U-SQL], terá de quatro ficheiros. DLL. Carregar estes ficheiros para uma localização na sua instância do Data Lake Store.

    ![Quatro ficheiros DLL carregados][img-query-avro-data-5] 

6. No Visual Studio, crie um projeto U-SQL.
 
    ![Criar um projeto U-SQL][img-query-avro-data-6]

7. Cole o conteúdo do seguinte script no ficheiro criado recentemente. Modificar as três secções realçadas: a conta de Data Lake Analytics, os caminhos de ficheiro DLL associados e o caminho correto para a sua conta de armazenamento.
    
    ![As três secções a modificar][img-query-avro-data-7a]

    O script U-SQL real para o resultado simple para um ficheiro CSV:
    
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

    Demorou Data Lake Analytics cinco minutos para executar o script seguinte, o que estava limitado a 10 unidades analíticas e processados 177 ficheiros. O resultado é apresentado no resultado do ficheiro CSV que é apresentado na imagem seguinte:
    
    ![Resultados de saída para um ficheiro CSV][img-query-avro-data-7b]

    ![Saída convertida para um ficheiro CSV][img-query-avro-data-7c]

    Para analisar o JSON, avance para o passo 8.
    
8. A maioria das mensagens de IoT estão no formato de ficheiro JSON. Ao adicionar as seguintes linhas, pode analisar a mensagem para um ficheiro JSON, que lhe permite adicionar cláusulas WHERE e de saída apenas os dados necessários.

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
Neste tutorial, aprendeu a consultar dados Avro eficientemente encaminhar mensagens do IoT Hub do Azure aos serviços do Azure.

Para obter exemplos de soluções ponto-a-ponto completas que utilizam o IoT Hub, consulte [acelerador de solução de monitorização remota do Azure IoT][lnk-iot-sa-land].

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
