---
title: Serializar dados no Hadoop - Microsoft Avro Library - Azure
description: Saiba como serializar e desserializar dados no Hadoop no HDInsight usando o Microsoft Avro Library para manter a memória, uma base de dados ou ficheiro.
keywords: avro, hadoop avro
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: jasonh
ms.custom: hdiseo17may2017
ms.openlocfilehash: ca7b79e02f63875b95f84b8d1e92a2ed2180db24
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43043050"
---
# <a name="serialize-data-in-hadoop-with-the-microsoft-avro-library"></a>Serializar dados no Hadoop com o Microsoft Avro Library

>[!NOTE]
>O SDK do Avro já não é suportado pela Microsoft. A biblioteca é suportada de Comunidade de código aberto. As origens para a biblioteca estão localizadas num [Github](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).

Este tópico mostra como utilizar o [Microsoft Avro Library](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro) para serializar objetos e outras estruturas de dados em fluxos para persistam na memória, uma base de dados ou um ficheiro. Ele também mostra como anular a serialização-los para recuperar os objetos originais.

[!INCLUDE [windows-only](../../../includes/hdinsight-windows-only.md)]

## <a name="apache-avro"></a>Apache Avro
O <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro Library</a> implementa o sistema de serialização de dados do Apache Avro para o ambiente de Microsoft.NET. Apache Avro fornece um formato de intercâmbio de dados binários compact para serialização. Ele usa <a href="http://www.json.org" target="_blank">JSON</a> para definir um esquema de linguagem desconhecida que assegura a interoperabilidade da linguagem. Dados serializados numa linguagem podem ser lidos noutra. Atualmente são suportados C, C++, c#, Java, PHP, Python e Ruby. Obter informações detalhadas sobre o formato podem ser encontradas no <a href="http://avro.apache.org/docs/current/spec.html" target="_blank">especificação do Apache Avro</a>. 

>[!NOTE]
>A Microsoft Avro Library não suporta a parte de (RPCs chamadas) de procedimento remoto dessa especificação.
>

A representação serializada de um objeto no sistema Avro consiste em duas partes: esquema e o valor real. O esquema Avro descreve o modelo de dados independente de idioma dos dados serializados com JSON. É apresentada a lado a lado com uma representação binária de dados. Ter o esquema em separado da representação binária permite que cada objeto a ser escrito com nenhuma sobrecargas por valor, tornando rápida de serialização e a representação pequeno.

## <a name="the-hadoop-scenario"></a>O cenário de Hadoop
O formato de serialização do Apache Avro é amplamente utilizado no Azure HDInsight e outros ambientes de Apache Hadoop. Avro fornece uma forma conveniente para representar estruturas de dados complexas dentro de uma tarefa de MapReduce do Hadoop. O formato dos ficheiros do Avro (arquivo de contêiner de objeto do Avro) foi projetado para suportar o modelo de programação de MapReduce distribuído. O principal recurso que permite a distribuição é que os ficheiros estão "divisíveis", no sentido de que um pode procurar qualquer ponto num ficheiro e começar a ler a partir de um bloco específico.

## <a name="serialization-in-avro-library"></a>Serialização na biblioteca do Avro
A biblioteca .NET para Avro suporta duas formas de serializar objetos:

* **reflexão** -esquema o JSON para os tipos é automaticamente criado a partir os dados de atributos de contrato dos tipos .NET ser serializada.
* **registo genérico** -esquema JSON de um esteja explicitamente especificado num registo representado pela [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) classe quando não existem tipos .NET estão presentes para descrever o esquema para os dados de ser serializada.

Quando o esquema de dados é conhecido para o escritor e o leitor da transmissão em fluxo, os dados podem ser enviados sem seu esquema. Em casos quando um arquivo de contêiner de objeto do Avro é utilizado, o esquema é armazenado no arquivo. Outros parâmetros, como o codec usados de compressão de dados, podem ser especificados. Estes cenários são descritos mais detalhadamente e ilustrados nos exemplos de código a seguir:

## <a name="install-avro-library"></a>Instalar a biblioteca do Avro
É necessário o seguinte antes de instalar a biblioteca:

* <a href="http://www.microsoft.com/download/details.aspx?id=17851" target="_blank">Microsoft .NET Framework 4</a>
* <a href="http://james.newtonking.com/json" target="_blank">Newtonsoft Json.NET</a> (6.0.4 ou posterior)

Tenha em atenção que a dependência de Newtonsoft.Json.dll é transferida automaticamente com a instalação da Microsoft Avro Library. O procedimento é fornecido na seção a seguir:

A Microsoft Avro Library é distribuído como um pacote NuGet que pode ser instalado a partir do Visual Studio via o seguinte procedimento:

1. Selecione o **Project** separador -> **gerir pacotes NuGet...**
2. Procure "Microsoft.Hadoop.Avro" na **Pesquisa Online** caixa.
3. Clique nas **instale** junto a **biblioteca do Microsoft Azure HDInsight Avro**.

Tenha em atenção que o Newtonsoft.Json.dll (> = 6.0.4) dependência também é transferida automaticamente com a Microsoft Avro Library.

O código-fonte Microsoft Avro Library está disponível em [Github](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).

## <a name="compile-schemas-using-avro-library"></a>Compilar esquemas com a biblioteca do Avro
A Microsoft Avro Library contém um utilitário de geração de código que permite a criação de c# tipos automaticamente com base no esquema JSON definido anteriormente. O utilitário de geração de código não é distribuído como um executável binário, mas pode ser facilmente construído com o seguinte procedimento:

1. Transfira o ficheiro. zip com a versão mais recente do código-fonte do SDK do HDInsight <a href="http://hadoopsdk.codeplex.com/SourceControl/latest#" target="_blank">Microsoft .NET SDK para Hadoop</a>. (Clique a **baixe** ícone, não a **Downloads** separador.)
2. Extraia o SDK do HDInsight para um diretório no computador com o .NET Framework 4 instalado e ligado à Internet para transferir pacotes de NuGet necessárias de dependência. Veja a seguir, partimos do princípio de que o código-fonte é extraído para C:\SDK.
3. Aceda à pasta C:\SDK\src\Microsoft.Hadoop.Avro.Tools e execute o Build. bat. (O ficheiro chamadas MSBuild partir a distribuição de 32 bits do .NET Framework. Se gostaria de utilizar a versão de 64 bits, editar Build. bat, seguindo os comentários dentro do arquivo.) Certifique-se de que a compilação seja bem-sucedida. (Em alguns sistemas, MSBuild pode produzir avisos. Esses avisos não afetam o utilitário, desde que não há nenhum erro de compilação.)
4. O utilitário compilado está localizado em C:\SDK\Bin\Unsigned\Release\Microsoft.Hadoop.Avro.Tools.

Para se familiarizar com a sintaxe da linha de comandos, execute o seguinte comando a partir da pasta onde está localizado o utilitário de geração de código: `Microsoft.Hadoop.Avro.Tools help /c:codegen`

Para testar o utilitário, pode gerar classes c# a partir do ficheiro de esquema JSON de exemplo fornecido com o código-fonte. Execute o seguinte comando:

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:

Isso deve para produzir dois c# ficheiros no diretório atual: SensorData.cs e Location.cs.

Para compreender a lógica que está a utilizar o utilitário de geração de código ao converter o esquema JSON para tipos do c#, veja o ficheiro que generationverification.Feature localizado na C:\SDK\src\Microsoft.Hadoop.Avro.Tools\Doc.

Espaços de nomes são extraídos do esquema JSON, usando a lógica descrita no arquivo mencionado no parágrafo anterior. Espaços de nomes extraídos do esquema têm precedência sobre tudo o que é fornecido com o parâmetro /n na linha de comandos de utilitário. Se quiser substituir os espaços de nomes contidos no esquema, utilize o parâmetro de /nf. Por exemplo, para alterar todos os espaços de nomes do SampleJSONSchema.avsc para my.own.nspace, execute o seguinte comando:

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:. /nf:my.own.nspace

## <a name="about-the-samples"></a>Sobre os exemplos
Exemplos de seis fornecidos neste tópico mostram diferentes cenários suportados pelo Microsoft Avro Library. A Microsoft Avro Library foi concebido para funcionar com qualquer fluxo. Nestes exemplos, os dados são manipulados por meio de fluxos de memória em vez de fluxos de arquivos ou bancos de dados para simplicidade e consistência. A abordagem obtida no ambiente de produção depende dos requisitos de cenário exato, origem de dados e volume, as restrições de desempenho e outros fatores.

Os dois primeiros exemplos mostram como serializar e desserializar dados em buffers de fluxo de memória com reflexão e genéricos registos. O esquema nesses dois casos pressupõe-se para ser compartilhado entre os leitores e gravadores fora de banda.

Os exemplos de terceiro e o quarto mostram como serializar e desserializar dados utilizando os ficheiros do Avro objeto contentor. Quando dados são armazenados num arquivo de contêiner Avro, seu esquema é sempre armazenada com o mesmo, porque o esquema tem de ser partilhado para desserialização.

O exemplo que contém os quatro primeiros exemplos pode ser transferido a partir da <a href="http://code.msdn.microsoft.com/Serialize-data-with-the-86055923" target="_blank">exemplos de código do Azure</a> site.

O exemplo quinto mostra como utilizar um codec de compressão personalizadas para ficheiros do Avro objeto contentor. Um exemplo que contém o código para este exemplo pode ser transferido a partir da <a href="http://code.msdn.microsoft.com/Serialize-data-with-the-67159111" target="_blank">exemplos de código do Azure</a> site.

O sexto exemplo mostra como usar a serialização do Avro para carregar dados para o armazenamento de Blobs do Azure e, em seguida, analise-o através do Hive num cluster do HDInsight (Hadoop). Pode ser transferido a partir da <a href="https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3" target="_blank">exemplos de código do Azure</a> site.

Seguem-se ligações para exemplos de seis discutidas no tópico:

* <a href="#Scenario1">**Serialização com reflexão** </a> -esquema o JSON para tipos de ser serializada é automaticamente criado a partir de dados os atributos do contrato.
* <a href="#Scenario2">**Serialização com registo genérico** </a> -esquema o JSON é especificada explicitamente num registo de quando nenhum tipo de .NET está disponível para reflexão.
* <a href="#Scenario3">**Serialização com ficheiros de contentor de objeto com reflexão** </a> -esquema JSON do é automaticamente criou e partilhou juntamente com os dados serializados por meio de um arquivo de contêiner de objeto do Avro.
* <a href="#Scenario4">**Serialização com ficheiros de contentor de objeto com registo genérico** </a> -esquema JSON do é explicitamente especificado antes da serialização e partilhado em conjunto com os dados por meio de um arquivo de contêiner de objeto do Avro.
* <a href="#Scenario5">**Serialização com ficheiros de contentor de objeto com um codec de compressão personalizado** </a> -o exemplo mostra como criar um arquivo de contêiner de objeto do Avro com uma implementação personalizada do .NET do codec de compressão Deflate dados.
* <a href="#Scenario6">**Usando o Avro para carregar dados para o serviço do Microsoft Azure HDInsight** </a> -o exemplo ilustra como a serialização do Avro interage com o serviço HDInsight. Uma subscrição do Azure Active Directory e o acesso a um cluster do HDInsight do Azure são necessários para executar este exemplo.

## <a name="Scenario1"></a>Exemplo 1: Serialização com reflexão
O esquema JSON para os tipos pode ser automaticamente criado por Microsoft Avro Library por meio de reflexão dos dados de atributos de contrato de objetos c# ser serializada. A Microsoft Avro Library cria um [ **IAvroSeralizer<T>**  ](http://msdn.microsoft.com/library/dn627341.aspx) para identificar os campos de ser serializada.

Neste exemplo, objetos (um **SensorData** classe com um membro **localização** struct) são serializadas para um fluxo de memória, e esse fluxo é por sua vez anular a serialização. O resultado é, em seguida, em comparação com a inicial de instâncias para confirmar que o **SensorData** objeto recuperado é idêntico à original.

O esquema neste exemplo é assumido sejam partilhados entre os leitores e gravadores, para que o formato de contêiner de objeto do Avro não é necessário. Para obter um exemplo de como serializar e desserializar dados em buffers de memória usando reflexão com o formato de contêiner de objeto, quando o esquema tem de ser partilhado com os dados, consulte <a href="#Scenario3">serialização com ficheiros de contentor de objeto com reflexão</a>.

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serialize and deserialize sample data set represented as an object using reflection.
            //No explicit schema definition is required - schema of serialized objects is automatically built.
            public void SerializeDeserializeObjectUsingReflection()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION\n");
                Console.WriteLine("Serializing Sample Data Set...");

                //Create a new AvroSerializer instance and specify a custom serialization strategy AvroDataContractResolver
                //for serializing only properties attributed with DataContract/DateMember
                var avroSerializer = AvroSerializer.Create<SensorData>();

                //Create a memory stream buffer
                using (var buffer = new MemoryStream())
                {
                    //Create a data set by using sample class and struct
                    var expected = new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } };

                    //Serialize the data to the specified stream
                    avroSerializer.Serialize(buffer, expected);


                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Deserialize data from the stream and cast it to the same type used for serialization
                    var actual = avroSerializer.Deserialize(buffer);

                    Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                    //Finally, verify that deserialized data matches the original one
                    bool isEqual = this.Equal(expected, actual);

                    Console.WriteLine("Result of Data Set Identity Comparison is {0}", isEqual);

                }
            }

            //
            //Helper methods
            //

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }



            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample Class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization to memory using reflection
                Sample.SerializeDeserializeObjectUsingReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION
    //
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


## <a name="sample-2-serialization-with-a-generic-record"></a>Exemplo 2: Serialização com um registo genérico
Um esquema JSON pode ser especificado explicitamente num registo genérico quando não é possível utilizar reflexão, porque os dados não podem ser representados por meio de classes do .NET com um contrato de dados. Esse método é mais lento do que usando a reflexão. Nesses casos, o esquema para os dados também pode ser dinâmico, ou seja, não é conhecido em tempo de compilação. Dados representados como arquivos de valores separados por vírgulas (CSV), cuja esquema é desconhecida até que ela é transformada para o formato Avro em tempo de execução são um exemplo desse tipo de cenário dinâmico.

Este exemplo mostra como criar e utilizar um [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) especificar explicitamente um esquema JSON, como preenchê-lo com os dados e, em seguida, como serializar e desserializá-lo. O resultado é, em seguida, em comparação com a inicial de instâncias para confirmar que o registo recuperado é idêntico à original.

O esquema neste exemplo é assumido sejam partilhados entre os leitores e gravadores, para que o formato de contêiner de objeto do Avro não é necessário. Para obter um exemplo de como serializar e desserializar dados em buffers de memória através da utilização de um registo genérico com o formato de contêiner de objeto, quando o esquema deve ser incluído com os dados serializados, consulte o <a href="#Scenario4">serialização usando arquivos de contentor de objeto com registo genérico</a> exemplo.

    namespace Microsoft.Hadoop.Avro.Sample
    {
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using System.Runtime.Serialization;
    using Microsoft.Hadoop.Avro.Container;
    using Microsoft.Hadoop.Avro.Schema;
    using Microsoft.Hadoop.Avro;

    //This class contains all methods demonstrating
    //the usage of Microsoft Avro Library
    public class AvroSample
    {

        //Serialize and deserialize sample data set by using a generic record.
        //A generic record is a special class with the schema explicitly defined in JSON.
        //All serialized data should be mapped to the fields of the generic record,
        //which in turn is then serialized.
        public void SerializeDeserializeObjectUsingGenericRecords()
        {
            Console.WriteLine("SERIALIZATION USING GENERIC RECORD\n");
            Console.WriteLine("Defining the Schema and creating Sample Data Set...");

            //Define the schema in JSON
            const string Schema = @"{
                                ""type"":""record"",
                                ""name"":""Microsoft.Hadoop.Avro.Specifications.SensorData"",
                                ""fields"":
                                    [
                                        {
                                            ""name"":""Location"",
                                            ""type"":
                                                {
                                                    ""type"":""record"",
                                                    ""name"":""Microsoft.Hadoop.Avro.Specifications.Location"",
                                                    ""fields"":
                                                        [
                                                            { ""name"":""Floor"", ""type"":""int"" },
                                                            { ""name"":""Room"", ""type"":""int"" }
                                                        ]
                                                }
                                        },
                                        { ""name"":""Value"", ""type"":""bytes"" }
                                    ]
                            }";

            //Create a generic serializer based on the schema
            var serializer = AvroSerializer.CreateGeneric(Schema);
            var rootSchema = serializer.WriterSchema as RecordSchema;

            //Create a memory stream buffer
            using (var stream = new MemoryStream())
            {
                //Create a generic record to represent the data
                dynamic location = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location.Floor = 1;
                location.Room = 243;

                dynamic expected = new AvroRecord(serializer.WriterSchema);
                expected.Location = location;
                expected.Value = new byte[] { 1, 2, 3, 4, 5 };

                Console.WriteLine("Serializing Sample Data Set...");

                //Serialize the data
                serializer.Serialize(stream, expected);

                stream.Seek(0, SeekOrigin.Begin);

                Console.WriteLine("Deserializing Sample Data Set...");

                //Deserialize the data into a generic record
                dynamic actual = serializer.Deserialize(stream);

                Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                //Finally, verify the results
                bool isEqual = expected.Location.Floor.Equals(actual.Location.Floor);
                isEqual = isEqual && expected.Location.Room.Equals(actual.Location.Room);
                isEqual = isEqual && ((byte[])expected.Value).SequenceEqual((byte[])actual.Value);
                Console.WriteLine("Result of Data Set Identity Comparison is {0}", isEqual);
            }
        }

        static void Main()
        {

            string sectionDivider = "---------------------------------------- ";

            //Create an instance of AvroSample class and invoke methods
            //illustrating different serializing approaches
            AvroSample Sample = new AvroSample();

            //Serialization to memory using generic record
            Sample.SerializeDeserializeObjectUsingGenericRecords();

            Console.WriteLine(sectionDivider);
            Console.WriteLine("Press any key to exit.");
            Console.Read();
        }
    }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING GENERIC RECORD
    //
    // Defining the Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


## <a name="sample-3-serialization-using-object-container-files-and-serialization-with-reflection"></a>Exemplo 3: Serialização utilizar ficheiros de contentor de objeto e serialização com reflexão
Este exemplo é semelhante para o cenário a <a href="#Scenario1"> primeiro exemplo</a>, em que o esquema implicitamente é especificado com reflexão. A diferença é que, aqui, o esquema não é assumido como conhecido para o leitor que anula a serialização. O **SensorData** objetos ser serializada e seu esquema implicitamente especificada estão armazenados num arquivo de contentor de objeto do Avro representado pela [ **AvroContainer** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) classe.

Os dados são serializados neste exemplo, com [ **SequentialWriter<SensorData>**  ](http://msdn.microsoft.com/library/dn627340.aspx) e serialização anulada com [ **SequentialReader<SensorData>**  ](http://msdn.microsoft.com/library/dn627340.aspx). O resultado é comparado com as instâncias iniciais para se certificar de identidade.

Os dados no ficheiro de contentor de objeto são comprimidos via a predefinição [ **Deflate** ] [ deflate-100] codec de compressão do .NET Framework 4. Consulte o <a href="#Scenario5"> exemplo quinto</a> neste tópico para saber como utilizar uma versão mais recente e superior a [ **Deflate** ] [ deflate-110] codec de compressão disponível no .NET Framework 4.5.

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes the sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with the Deflate codec.
            public void SerializeDeserializeUsingObjectContainersReflection()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION AND AVRO OBJECT CONTAINER FILES\n");

                //Path for Avro object container file
                string path = "AvroSampleReflectionDeflate.avro";

                //Create a data set by using sample class and struct
                var testData = new List<SensorData>
                        {
                            new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } },
                            new SensorData { Value = new byte[] { 6, 7, 8, 9 }, Position = new Location { Room = 244, Floor = 1 } }
                        };

                //Serializing and saving data to file.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Data is compressed using the Deflate codec.
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize the data to stream by using the sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream to file
                    Console.WriteLine("Saving serialized data to file...");
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing data.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which deserializes all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true)))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches the original one
                        Console.WriteLine("Comparing Initial and Deserialized Data Sets...");
                        int count = 1;
                        var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = serialized, actual = deserialized });
                        foreach (var pair in pairs)
                        {
                            bool isEqual = this.Equal(pair.expected, pair.actual);
                            Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual);
                            count++;
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }

            //
            //Helper methods
            //

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading a file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using reflection to Avro object container file
                Sample.SerializeDeserializeUsingObjectContainersReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION AND AVRO OBJECT CONTAINER FILES
    //
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


## <a name="sample-4-serialization-using-object-container-files-and-serialization-with-generic-record"></a>Exemplo 4: Serialização utilizar ficheiros de contentor de objeto e serialização com registo genérico
Este exemplo é semelhante para o cenário a <a href="#Scenario2"> segundo exemplo</a>, em que o esquema é especificado explicitamente com JSON. A diferença é que, aqui, o esquema não é assumido como conhecido para o leitor que anula a serialização.

O conjunto de dados de teste é recolhido para uma lista de [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) objetos através de um esquema JSON explicitamente definido e, em seguida, armazenados num arquivo de contentor de objeto representado pelos [  **AvroContainer** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) classe. Este arquivo de contêiner cria um gravador que é utilizado para serializar os dados, descomprimidos, como um fluxo de memória que depois é salvo num arquivo. O [ **Codec.Null** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.codec.null.aspx) utilizado para criar o leitor de parâmetro especifica a que estes dados não sejam compactados.

Os dados, em seguida, são lidas a partir do ficheiro e desserializados numa coleção de objetos. Esta coleção é comparada com a lista inicial de registos de Avro para confirmar se são idênticos.

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro.Schema;
        using Microsoft.Hadoop.Avro;

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes a sample data set by using a generic record and Avro object container files.
            //Serialized data is not compressed.
            public void SerializeDeserializeUsingObjectContainersGenericRecord()
            {
                Console.WriteLine("SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES\n");

                //Path for Avro object container file
                string path = "AvroSampleGenericRecordNullCodec.avro";

                Console.WriteLine("Defining the Schema and creating Sample Data Set...");

                //Define the schema in JSON
                const string Schema = @"{
                                ""type"":""record"",
                                ""name"":""Microsoft.Hadoop.Avro.Specifications.SensorData"",
                                ""fields"":
                                    [
                                        {
                                            ""name"":""Location"",
                                            ""type"":
                                                {
                                                    ""type"":""record"",
                                                    ""name"":""Microsoft.Hadoop.Avro.Specifications.Location"",
                                                    ""fields"":
                                                        [
                                                            { ""name"":""Floor"", ""type"":""int"" },
                                                            { ""name"":""Room"", ""type"":""int"" }
                                                        ]
                                                }
                                        },
                                        { ""name"":""Value"", ""type"":""bytes"" }
                                    ]
                            }";

                //Create a generic serializer based on the schema
                var serializer = AvroSerializer.CreateGeneric(Schema);
                var rootSchema = serializer.WriterSchema as RecordSchema;

                //Create a generic record to represent the data
                var testData = new List<AvroRecord>();

                dynamic expected1 = new AvroRecord(rootSchema);
                dynamic location1 = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location1.Floor = 1;
                location1.Room = 243;
                expected1.Location = location1;
                expected1.Value = new byte[] { 1, 2, 3, 4, 5 };
                testData.Add(expected1);

                dynamic expected2 = new AvroRecord(rootSchema);
                dynamic location2 = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location2.Floor = 1;
                location2.Room = 244;
                expected2.Location = location2;
                expected2.Value = new byte[] { 6, 7, 8, 9 };
                testData.Add(expected2);

                //Serializing and saving data to file.
                //Create a MemoryStream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Data is not compressed (Null compression codec).
                    using (var writer = AvroContainer.CreateGenericWriter(Schema, buffer, Codec.Null))
                    {
                        using (var streamWriter = new SequentialWriter<object>(writer, 24))
                        {
                            // Serialize the data to stream by using the sequential writer
                            testData.ForEach(streamWriter.Write);
                        }
                    }

                    Console.WriteLine("Saving serialized data to file...");

                    //Save stream to file
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing the data.
                //Create a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which deserializes all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    using (var reader = AvroContainer.CreateGenericReader(buffer))
                    {
                        using (var streamReader = new SequentialReader<object>(reader))
                        {
                            var results = streamReader.Objects;

                            Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                            //Finally, verify the results
                            var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = (dynamic)serialized, actual = (dynamic)deserialized });
                            int count = 1;
                            foreach (var pair in pairs)
                            {
                                bool isEqual = pair.expected.Location.Floor.Equals(pair.actual.Location.Floor);
                                isEqual = isEqual && pair.expected.Location.Room.Equals(pair.actual.Location.Room);
                                isEqual = isEqual && ((byte[])pair.expected.Value).SequenceEqual((byte[])pair.actual.Value);
                                Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual.ToString());
                                count++;
                            }
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }

            //
            //Helper methods
            //

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading a file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using the given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of the AvroSample class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using generic record to Avro object container file
                Sample.SerializeDeserializeUsingObjectContainersGenericRecord();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES
    //
    // Defining the Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.




## <a name="sample-5-serialization-using-object-container-files-with-a-custom-compression-codec"></a>Exemplo 5: Serialização utilizar ficheiros de contentor de objeto com um codec de compressão personalizado
O exemplo quinto mostra como utilizar um codec de compressão personalizadas para ficheiros do Avro objeto contentor. Um exemplo que contém o código para este exemplo pode ser transferido a partir da [exemplos de código do Azure](http://code.msdn.microsoft.com/Serialize-data-with-the-67159111) site.

O [especificação do Avro](http://avro.apache.org/docs/current/spec.html#Required+Codecs) permite a utilização de um codec de compressão opcional (além **nulo** e **Deflate** predefinições). Neste exemplo não é implementar um novo codec como Snappy (mencionado como um codec opcional suportado no [especificação do Avro](http://avro.apache.org/docs/current/spec.html#snappy)). Ele mostra como usar o .NET Framework 4.5 de implementação do [ **Deflate** ] [ deflate-110] codec, que fornece um melhor algoritmo de compactação com base no [zlib ](http://zlib.net/) biblioteca de compressão que a versão do .NET Framework 4 padrão.

    //
    // This code needs to be compiled with the parameter Target Framework set as ".NET Framework 4.5"
    // to ensure the desired implementation of the Deflate compression algorithm is used.
    // Ensure your C# project is set up accordingly.
    //

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.IO;
        using System.IO.Compression;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        #region Defining objects for serialization
        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }
        #endregion

        #region Defining custom codec based on .NET Framework V.4.5 Deflate
        //Avro.NET codec class contains two methods,
        //GetCompressedStreamOver(Stream uncompressed) and GetDecompressedStreamOver(Stream compressed),
        //which are the key ones for data compression.
        //To enable a custom codec, one needs to implement these methods for the required codec.

        #region Defining Compression and Decompression Streams
        //DeflateStream (class from System.IO.Compression namespace that implements Deflate algorithm)
        //cannot be directly used for Avro because it does not support vital operations like Seek.
        //Thus one needs to implement two classes inherited from stream
        //(one for compressed and one for decompressed stream)
        //that use Deflate compression and implement all required features.
        internal sealed class CompressionStreamDeflate45 : Stream
        {
            private readonly Stream buffer;
            private DeflateStream compressionStream;

            public CompressionStreamDeflate45(Stream buffer)
            {
                Debug.Assert(buffer != null, "Buffer is not allowed to be null.");

                this.compressionStream = new DeflateStream(buffer, CompressionLevel.Fastest, true);
                this.buffer = buffer;
            }

            public override bool CanRead
            {
                get { return this.buffer.CanRead; }
            }

            public override bool CanSeek
            {
                get { return true; }
            }

            public override bool CanWrite
            {
                get { return this.buffer.CanWrite; }
            }

            public override void Flush()
            {
                this.compressionStream.Close();
            }

            public override long Length
            {
                get { return this.buffer.Length; }
            }

            public override long Position
            {
                get
                {
                    return this.buffer.Position;
                }

                set
                {
                    this.buffer.Position = value;
                }
            }

            public override int Read(byte[] buffer, int offset, int count)
            {
                return this.buffer.Read(buffer, offset, count);
            }

            public override long Seek(long offset, SeekOrigin origin)
            {
                return this.buffer.Seek(offset, origin);
            }

            public override void SetLength(long value)
            {
                throw new NotSupportedException();
            }

            public override void Write(byte[] buffer, int offset, int count)
            {
                this.compressionStream.Write(buffer, offset, count);
            }

            protected override void Dispose(bool disposed)
            {
                base.Dispose(disposed);

                if (disposed)
                {
                    this.compressionStream.Dispose();
                    this.compressionStream = null;
                }
            }
        }

        internal sealed class DecompressionStreamDeflate45 : Stream
        {
            private readonly DeflateStream decompressed;

            public DecompressionStreamDeflate45(Stream compressed)
            {
                this.decompressed = new DeflateStream(compressed, CompressionMode.Decompress, true);
            }

            public override bool CanRead
            {
                get { return true; }
            }

            public override bool CanSeek
            {
                get { return true; }
            }

            public override bool CanWrite
            {
                get { return false; }
            }

            public override void Flush()
            {
                this.decompressed.Close();
            }

            public override long Length
            {
                get { return this.decompressed.Length; }
            }

            public override long Position
            {
                get
                {
                    return this.decompressed.Position;
                }

                set
                {
                    throw new NotSupportedException();
                }
            }

            public override int Read(byte[] buffer, int offset, int count)
            {
                return this.decompressed.Read(buffer, offset, count);
            }

            public override long Seek(long offset, SeekOrigin origin)
            {
                throw new NotSupportedException();
            }

            public override void SetLength(long value)
            {
                throw new NotSupportedException();
            }

            public override void Write(byte[] buffer, int offset, int count)
            {
                throw new NotSupportedException();
            }

            protected override void Dispose(bool disposing)
            {
                base.Dispose(disposing);

                if (disposing)
                {
                    this.decompressed.Dispose();
                }
            }
        }
        #endregion

        #region Define Codec
        //Define the actual codec class containing the required methods for manipulating streams:
        //GetCompressedStreamOver(Stream uncompressed) and GetDecompressedStreamOver(Stream compressed).
        //Codec class uses classes for compressed and decompressed streams defined above.
        internal sealed class DeflateCodec45 : Codec
        {

            //We merely use different IMPLEMENTATIONS of Deflate, so CodecName remains "deflate"
            public static readonly string CodecName = "deflate";

            public DeflateCodec45()
                : base(CodecName)
            {
            }

            public override Stream GetCompressedStreamOver(Stream decompressed)
            {
                if (decompressed == null)
                {
                    throw new ArgumentNullException("decompressed");
                }

                return new CompressionStreamDeflate45(decompressed);
            }

            public override Stream GetDecompressedStreamOver(Stream compressed)
            {
                if (compressed == null)
                {
                    throw new ArgumentNullException("compressed");
                }

                return new DecompressionStreamDeflate45(compressed);
            }
        }
        #endregion

        #region Define modified Codec Factory
        //Define modified codec factory to be used in the reader.
        //It catches the attempt to use "Deflate" and provide  a custom codec.
        //For all other cases, it relies on the base class (CodecFactory).
        internal sealed class CodecFactoryDeflate45 : CodecFactory
        {

            public override Codec Create(string codecName)
            {
                if (codecName == DeflateCodec45.CodecName)
                    return new DeflateCodec45();
                else
                    return base.Create(codecName);
            }
        }
        #endregion

        #endregion

        #region Sample Class with demonstration methods
        //This class contains methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with the custom compression codec (Deflate of .NET Framework 4.5).
            //
            //This sample uses memory stream for all operations related to serialization, deserialization and
            //object container manipulation, though file stream could be easily used.
            public void SerializeDeserializeUsingObjectContainersReflectionCustomCodec()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION, AVRO OBJECT CONTAINER FILES AND CUSTOM CODEC\n");

                //Path for Avro object container file
                string path = "AvroSampleReflectionDeflate45.avro";

                //Create a data set by using sample class and struct
                var testData = new List<SensorData>
                        {
                            new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } },
                            new SensorData { Value = new byte[] { 6, 7, 8, 9 }, Position = new Location { Room = 244, Floor = 1 } }
                        };

                //Serializing and saving data to file.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Here the custom codec is introduced. For convenience, the next commented code line shows how to use built-in Deflate.
                    //Note that because the sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, new DeflateCodec45()))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize the data to stream using the sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream to file
                    Console.WriteLine("Saving serialized data to file...");
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing data.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Because of SequentialReader<T> constructor signature, an AvroSerializerSettings instance is required
                    //when codec factory is explicitly specified.
                    //You may comment the line below if you want to use built-in Deflate (see next comment).
                    AvroSerializerSettings settings = new AvroSerializerSettings();

                    //Create a SequentialReader instance for type SensorData, which deserializes all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    //Here the custom codec factory is introduced.
                    //For convenience, the next commented code line shows how to use built-in Deflate
                    //(no explicit Codec Factory parameter is required in this case).
                    //Note that because the sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var reader = new SequentialReader<SensorData>(AvroContainer.CreateReader<SensorData>(buffer, true)))
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true, settings, new CodecFactoryDeflate45())))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches the original one
                        Console.WriteLine("Comparing Initial and Deserialized Data Sets...");
                        bool isEqual;
                        int count = 1;
                        var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = serialized, actual = deserialized });
                        foreach (var pair in pairs)
                        {
                            isEqual = this.Equal(pair.expected, pair.actual);
                            Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual.ToString());
                            count++;
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }
        #endregion

            #region Helper Methods

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }
            #endregion

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample Class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using reflection to Avro object container file using custom codec
                Sample.SerializeDeserializeUsingObjectContainersReflectionCustomCodec();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION, AVRO OBJECT CONTAINER FILES AND CUSTOM CODEC
    //
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    //For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.

## <a name="sample-6-using-avro-to-upload-data-for-the-microsoft-azure-hdinsight-service"></a>Exemplo 6: Utilização Avro para carregar dados para o serviço do Microsoft Azure HDInsight
O exemplo de sexto ilustra algumas técnicas de programação relacionadas com a interação com o serviço Azure HDInsight. Um exemplo que contém o código para este exemplo pode ser transferido a partir da [exemplos de código do Azure](https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3) site.

O exemplo faz as seguintes tarefas:

* Liga-se a um cluster de serviço existente do HDInsight.
* Serializa vários ficheiros CSV e carrega o resultado para o armazenamento de Blobs do Azure. (Os arquivos CSV são distribuídos em conjunto com o exemplo e representam uma extração de dados históricos de AMEX Stock distribuídos pela [Infochimps](http://www.infochimps.com/) para o período de 1970 2010. O exemplo lê os dados de ficheiro CSV, converte os registos para instâncias do **Stock** de classe e, em seguida, serializa-los usando reflexão. Definição de tipo de stock é criada a partir de um esquema JSON por meio do utilitário de geração de código do Microsoft Avro Library.
* Cria uma nova tabela externa, chamada **Stocks** no Hive e links para os dados carregado no passo anterior.
* Executa uma consulta através do Hive através do **Stocks** tabela.

Além disso, o exemplo executa um procedimento de limpeza antes e depois de efetuar operações principais. Durante a limpeza, todos os dados de Blobs do Azure relacionados e pastas são removidos e a tabela de Hive é removida. Também é possível invocar o procedimento de limpeza a partir da linha de comando de exemplo.

O exemplo tem os seguintes pré-requisitos:

* Uma subscrição do Microsoft Azure Active Directory e o seu ID de subscrição.
* Um certificado de gestão para a subscrição com a chave privada correspondente. O certificado deve ser instalado no armazenamento privado do utilizador atual na máquina utilizada para executar o exemplo.
* Um cluster do HDInsight Active Directory.
* Uma conta de armazenamento do Azure, o cluster do HDInsight é vinculado ao pré-requisito anterior, juntamente com a chave de acesso primária ou secundária correspondente.

Todas as informações de pré-requisitos devem ser inseridas para o ficheiro de configuração de exemplo antes do exemplo é executado. Existem duas maneiras possíveis de fazê-lo:

* Edite o ficheiro App. config no diretório de raiz de exemplo e, em seguida, criar o exemplo
* Primeiro criar o exemplo e, em seguida, edite AvroHDISample.exe.config no diretório build

Em ambos os casos, todas as edições devem ser feitas **<appSettings>** da secção definições. Siga os comentários no arquivo.
O exemplo é executado a partir da linha de comando executando o seguinte comando (caso contrário, onde o ficheiro. zip com o exemplo pressupunha-se a ser extraído para C:\AvroHDISample; se utilizar o caminho do ficheiro relevantes):

    AvroHDISample run C:\AvroHDISample\Data

Para limpar o cluster, execute o seguinte comando:

    AvroHDISample clean

[deflate-100]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.100).aspx
[deflate-110]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.110).aspx
