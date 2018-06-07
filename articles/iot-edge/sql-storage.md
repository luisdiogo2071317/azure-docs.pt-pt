---
title: Módulo de SQL de limite de IoT do Azure | Microsoft Docs
description: Armazenar dados no limite com módulos do Microsoft SQL, com as funções do Azure para formatar os dados.
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: ebertrams
ms.date: 02/21/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 9c90cc28956e4dd7730cc7ba09a173f505f056fc
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34632405"
---
# <a name="store-data-at-the-edge-with-sql-server-databases"></a>Armazenar dados no limite com bases de dados do SQL Server

Utilize dispositivos de limite de IoT do Azure para armazenar os dados que são gerados no limite. Dispositivos com ligações de internet intermitente podem manter as suas próprias bases de dados e as alterações de relatório para a nuvem apenas quando estiver ligado. Carrega a dispositivos que tenham sido programados para enviar apenas dados críticos para a nuvem, podem guardar o resto dos dados para regular em massa. Uma vez na nuvem, os dados estruturados podem ser partilhados com outros serviços do Azure, para a instância para criar um modelo de machine learning. 

Este artigo fornece instruções para implementar uma base de dados do SQL Server para um dispositivo de limite de IoT. As funções do Azure em execução no dispositivo de limite de IoT, estruturas de dados recebidos, em seguida, envia-a para a base de dados. Os passos neste artigo também podem ser aplicados a outras bases de dados que funcionam em contentores, como o MySQL ou PostgreSQL. 

## <a name="prerequisites"></a>Pré-requisitos 

Antes de iniciar as instruções neste artigo, deve concluir os tutoriais seguintes:
* Implementar o Azure IoT Edge num dispositivo simulado [Windows](tutorial-simulate-device-windows.md) ou [Linux](tutorial-simulate-device-linux.md)
* [Implementar a função do Azure como um módulo de limite de IoT](tutorial-deploy-function.md)

Os artigos seguintes não são necessários para concluir este tutorial com êxito, mas podem fornecer contexto úteis:
* [Executar a imagem do contentor de 2017 do SQL Server com o Docker](https://docs.microsoft.com/sql/linux/quickstart-install-connect-docker)
* [Utilizar o Visual Studio Code para desenvolver e implementar as funções do Azure para o limite de IoT do Azure](how-to-vscode-develop-azure-function.md)

Depois de concluir os tutoriais necessários, devem ter todos os pré-requisitos necessários prontos no seu computador: 
* Um hub IoT do Azure Active Directory.
* Um dispositivo de limite de IoT com, pelo menos, 2 GB de RAM e uma unidade de disco de 2 GB.
* [Visual Studio Code](https://code.visualstudio.com/). 
* [Extensão Azure IoT Edge para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
* [Extensão C# para Visual Studio Code (com tecnologia da OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). 
* [Docker](https://docs.docker.com/engine/installation/)
* [SDK de .NET Core 2.0](https://www.microsoft.com/net/core#windowscmd). 
* [Python 2.7](https://www.python.org/downloads/)
* [Script de controlo contorno de IoT](https://pypi.python.org/pypi/azure-iot-edge-runtime-ctl)
* Modelo de AzureIoTEdgeFunction (`dotnet new -i Microsoft.Azure.IoT.Edge.Function`)
* Um hub IoT Active Directory com, pelo menos, um dispositivo de limite de IoT.

Contentores Windows e Linux num x64 arquiteturas de processador de trabalho para este tutorial. SQL Server não suporta processadores ARM.

## <a name="deploy-a-sql-server-container"></a>Implementar um contentor do SQL Server

Nesta secção, é possível adicionar uma base de dados do MS SQL para o seu dispositivo simulado de limite de IoT. Utilizar a SQL Server 2017 docker contentor imagem, disponível como uma [Windows](https://hub.docker.com/r/microsoft/mssql-server-windows-developer/) contentor e como um [Linux](https://hub.docker.com/r/microsoft/mssql-server-linux/) contentor. 

### <a name="deploy-sql-server-2017"></a>Implemente o SQL Server de 2017

Por predefinição, o código nesta secção cria um contentor com a edição gratuita do programador do SQL Server de 2017. Se pretender executar em vez disso, as edições de produção, consulte [executar produção imagens contentor](https://docs.microsoft.com/sql/linux/sql-server-linux-configure-docker#production) para obter informações detalhadas. 

No passo 3, adicione a criar as opções para o contentor do SQL Server, que são importantes para estabelecer as variáveis de ambiente e persistant armazenamento. Configurada [variáveis de ambiente](https://docs.microsoft.com/sql/linux/sql-server-linux-configure-environment-variables) aceitar o contrato de licença de utilizador final e definir uma palavra-passe. O [persistant armazenamento](https://docs.microsoft.com/sql/linux/sql-server-linux-configure-docker#persist) é configurada utilizando [monta](https://docs.docker.com/storage/volumes/). Montar a criar o contentor de 2017 do SQL Server com um *sqlvolume* ligado para que os dados persistir mesmo é eliminar o contentor de contentor de volume. 

1. Abra o `deployment.json` ficheiro no Visual Studio Code. 
2. Substitua o **módulos** secção do ficheiro com o seguinte código: 

   ```json
   "modules": {
          "filterFunction": {
            "version": "1.0",
            "type": "docker",
            "status": "running",
            "restartPolicy": "always",
            "settings": {
              "image": "<docker registry address>/filterfunction:latest",
              "createOptions": "{}"
            }
          },
          "tempSensor": {
            "version": "1.0",
            "type": "docker",
            "status": "running",
            "restartPolicy": "always",
            "settings": {
              "image": "microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview",
              "createOptions": "{}"
            }
          },
          "sql": {
            "version": "1.0",
            "type": "docker",
            "status": "running",
            "restartPolicy": "always",
            "settings": {
              "image": "",
              "createOptions": ""
             }
          }
        }
   ```

3. Substitua o `<docker registry address>` com o endereço preenchido o tutorial concluída [implementar a função do Azure como um módulo de limite de IoT](tutorial-deploy-function.md).

   >[!NOTE]
   >O endereço do registo de contentores é o mesmo do servidor de início de sessão que copiou a partir do seu registo. Deverá estar no formato `<your container registry name>.azurecr.io`

4. Dependendo do sistema operativo que estiver a executar, Atualize as definições para o módulo SQL com o seguinte código: 

   * Windows:

      ```json
      "image": "microsoft/mssql-server-windows-developer",
      "createOptions": "{\"Env\": [\"ACCEPT_EULA=Y\",\"MSSQL_SA_PASSWORD=Strong!Passw0rd\"],\"HostConfig\": {\"Mounts\": [{\"Target\": \"C:\\\\mssql\",\"Source\": \"sqlVolume\",\"Type\": \"volume\"}],\"PortBindings\": {\"1433/tcp\": [{\"HostPort\": \"1401\"}]}}}"
      ```

   * Linux:

      ```json
      "image": "microsoft/mssql-server-linux:2017-latest",
      "createOptions": "{\"Env\": [\"ACCEPT_EULA=Y\",\"MSSQL_SA_PASSWORD=Strong!Passw0rd\"],\"HostConfig\": {\"Mounts\": [{\"Target\": \"/var/opt/mssql\",\"Source\": \"sqlVolume\",\"Type\": \"volume\"}],\"PortBindings\": {\"1433/tcp\": [{\"HostPort\": \"1401\"}]}}}"
      ```

5. Guarde o ficheiro. 
6. Na paleta de comando VS Code, selecione **Edge: criar a implementação para o dispositivo de limite**. 
7. Selecione o seu ID de dispositivo de limite de IoT.
8. Selecione o `deployment.json` ficheiro que tenha atualizado. Na janela de resultados, pode ver saídas correspondentes para a sua implementação. 
9. Para iniciar o tempo de execução de limite, selecione **Edge: Iniciar Edge** na paleta do comando.

>[!TIP]
>Sempre que criar um contentor do SQL Server num ambiente de produção, deve [alterar a palavra-passe de administrador do sistema de predefinição](https://docs.microsoft.com/sql/linux/quickstart-install-connect-docker#change-the-sa-password).

## <a name="create-the-sql-database"></a>Criar a base de dados do SQL Server

Esta secção orienta-o a configurar a base de dados do SQL Server para armazenar os dados de temperatura recebidos os sensores ligados ao dispositivo de limite de IoT. Se estiver a utilizar um dispositivo simulado, estes dados vêm o *tempSensor* contentor. 

Numa ferramenta de linha de comandos, ligar à base de dados: 

* Contentor do Windows
   ```cmd
   docker exec -it sql cmd
   ```

* Contentor de Linux
   ```bash
   docker exec -it sql bash
   ```

Abra a ferramenta do comando SQL: 

* Contentor do Windows
   ```cmd
   sqlcmd -S localhost -U SA -P 'Strong!Passw0rd'
   ```

* Contentor de Linux
   ```bash
   /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P 'Strong!Passw0rd'
   ```

Crie a base de dados: 

* Contentor do Windows
   ```sql
   CREATE DATABASE MeasurementsDB
   ON
   (NAME = MeasurementsDB, FILENAME = 'C:\mssql\measurementsdb.mdf')
   GO
   ```

* Contentor de Linux
   ```sql
   CREATE DATABASE MeasurementsDB
   ON
   (NAME = MeasurementsDB, FILENAME = '/var/opt/mssql/measurementsdb.mdf')
   GO
   ```

Defina a tabela: 

   ```sql
   CREATE TABLE MeasurementsDB.dbo.TemperatureMeasurements (measurementTime DATETIME2, location NVARCHAR(50), temperature FLOAT)
   GO
   ```

Pode personalizar o ficheiro de docker do SQL Server para configurar automaticamente o SQL Server para ser implementado em vários dispositivos de limite de IoT. Para obter mais informações, consulte o [projeto de demonstração de contentor do Microsoft SQL Server](https://github.com/twright-msft/mssql-node-docker-demo-app).

## <a name="understand-the-sql-connection"></a>Compreender a ligação do SQL Server

Outros tutoriais, utilizamos rotas para permitir contentores comunicar ao restantes isoladas umas das outras. Quando trabalha com uma base de dados do SQL Server, no entanto, uma relação próximo é necessária. 

Limite de IoT automaticamente baseia-se uma ponte (Linux) ou a rede NAT (Windows) quando é iniciado. Denomina-se a rede **azure-iot-limite**. Se alguma vez precisar de depuração esta ligação, pode ver as respetivas propriedades na linha de comandos:

* Windows

   ```cmd
   docker network inspect azure-iot-edge
   ```

* Linux

   ```bash
   sudo docker network inspect azure-iot-edge
   ```

Limite de IoT também pode resolver o DNS de um nome de contentor através do docker, pelo que não necessita de referir-se a sua base de dados do SQL Server pelo respetivo endereço IP. 

Por exemplo, eis a cadeia de ligação que utilizamos na secção seguinte: 

   ```csharp
   Data Source=tcp:sql,1433;Initial Catalog=MeasurementsDB;User Id=SA;Password=Strong!Passw0rd;TrustServerCertificate=False;Connection Timeout=30;
   ```

Pode ver que a cadeia de ligação referencia o contentor pelo respetivo nome, **sql**. Se tiver alterado o nome do módulo para outra coisa, atualize, bem como esta cadeia de ligação. Caso contrário, avançar para a secção seguinte. 

## <a name="update-your-azure-function"></a>Atualizar a função do Azure
Para enviar os dados para a base de dados, atualize a função de Azure FilterFunction que introduziu no tutorial anterior. Altere este ficheiro para que estruturas de dados recebidos pelo seu sensores em seguida, armazena-os numa tabela SQL. 

1. No Visual Studio Code, abra a pasta de FilterFunction. 
2. Substitua o ficheiro de run.csx com o seguinte código que inclui a cadeia de ligação de SQL da secção anterior: 

   ```csharp
   #r "Microsoft.Azure.Devices.Client"
   #r "Newtonsoft.Json"
   #r "System.Data.SqlClient"

   using System.IO;
   using Microsoft.Azure.Devices.Client;
   using Newtonsoft.Json;
   using Sql = System.Data.SqlClient;
   using System.Threading.Tasks;

   // Filter messages based on the temperature value in the body of the message and the temperature threshold value.
   public static async Task Run(Message messageReceived, IAsyncCollector<Message> output, TraceWriter log)
   {
       const int temperatureThreshold = 25;
       byte[] messageBytes = messageReceived.GetBytes();
       var messageString = System.Text.Encoding.UTF8.GetString(messageBytes);

       if (!string.IsNullOrEmpty(messageString))
       {
           // Get the body of the message and deserialize it
           var messageBody = JsonConvert.DeserializeObject<MessageBody>(messageString);

           //Store the data in SQL db
           const string str = "Data Source=tcp:sql,1433;Initial Catalog=MeasurementsDB;User Id=SA;Password=Strong!Passw0rd;TrustServerCertificate=False;Connection Timeout=30;";
           using (Sql.SqlConnection conn = new Sql.SqlConnection(str))
           {
           conn.Open();
           var insertMachineTemperature = "INSERT INTO MeasurementsDB.dbo.TemperatureMeasurements VALUES (CONVERT(DATETIME2,'" + messageBody.timeCreated + "', 127), 'machine', " + messageBody.machine.temperature + ");";
           var insertAmbientTemperature = "INSERT INTO MeasurementsDB.dbo.TemperatureMeasurements VALUES (CONVERT(DATETIME2,'" + messageBody.timeCreated + "', 127), 'ambient', " + messageBody.ambient.temperature + ");"; 
               using (Sql.SqlCommand cmd = new Sql.SqlCommand(insertMachineTemperature + "\n" + insertAmbientTemperature, conn))
               {
               //Execute the command and log the # rows affected.
               var rows = await cmd.ExecuteNonQueryAsync();
               log.Info($"{rows} rows were updated");
               }
           }

           if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
           {
               // Send the message to the output as the temperature value is greater than the threshold
               var filteredMessage = new Message(messageBytes);
               // Copy the properties of the original message into the new Message object
               foreach (KeyValuePair<string, string> prop in messageReceived.Properties)
               {
                   filteredMessage.Properties.Add(prop.Key, prop.Value);
               }
               // Add a new property to the message to indicate it is an alert
               filteredMessage.Properties.Add("MessageType", "Alert");
               // Send the message        
               await output.AddAsync(filteredMessage);
               log.Info("Received and transferred a message with temperature above the threshold");
           }
       }
   }

   //Define the expected schema for the body of incoming messages
   class MessageBody
   {
       public Machine machine {get;set;}
       public Ambient ambient {get; set;}
       public string timeCreated {get; set;}
   }
   class Machine
   {
      public double temperature {get; set;}
      public double pressure {get; set;}         
   }
   class Ambient
   {
      public double temperature {get; set;}
      public int humidity {get; set;}         
   }
   ```

## <a name="update-your-container-image"></a>Atualizar a imagem de contentor

Para aplicar as alterações que fez, atualizar a imagem de contentor, publicá-lo e reinicie o limite de IoT.

1. No Visual Studio Code, expanda o **Docker** pasta. 
2. Com base na plataforma que está a utilizar, expanda o **windows nano** ou **linux x64** pasta. 
3. Clique com botão direito do **Dockerfile** do ficheiro e selecione **imagem de Docker do módulo de limite de IoT criar**.
4. Navegue para o **FilterFunction** pasta do projeto e clique em **pasta Selecione como EXE_DIR**.
5. Na caixa de texto de pop-up na parte superior da janela do VS Code, introduza o nome da imagem. Por exemplo, `<your container registry address>/filterfunction:latest`. Se estiver a implementar um registo local, o nome deve ser `<localhost:5000/filterfunction:latest>`.
6. Na paleta do comando VS Code, selecione **Edge: imagem de Docker do módulo de limite de Push de IoT**. 
7. Na caixa de texto de pop-up, introduza o mesmo nome de imagem. 
8. Na paleta do comando VS Code, selecione **Edge: reiniciar Edge**.

## <a name="view-the-local-data"></a>Ver os dados locais

Depois dos contentores reiniciar, os dados recebidos dos sensores de temperatura são armazenados numa base de dados SQL Server 2017 local no seu dispositivo de limite de IoT. 

Numa ferramenta de linha de comandos, ligar à base de dados: 

* Contentor do Windows
   ```cmd
   docker exec -it sql cmd
   ```

* Contentor de Linux
   ```bash
   docker exec -it sql bash
   ```

Abra a ferramenta do comando SQL: 

* Contentor do Windows
   ```cmd
   sqlcmd -S localhost -U SA -P 'Strong!Passw0rd'
   ```

* Contentor de Linux
   ```bash
   /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P 'Strong!Passw0rd'
   ```

Ver os seus dados: 

   ```sql
   SELECT * FROM MeasurementsDB.dbo.TemperatureMeasurements
   GO
   ```

## <a name="next-steps"></a>Passos Seguintes

* Saiba como [configurar imagens do SQL Server 2017 contentor no Docker](https://docs.microsoft.com/sql/linux/sql-server-linux-configure-docker).

* Visite o [repositório do GitHub mssql docker](https://github.com/Microsoft/mssql-docker) para recursos, comentários e problemas conhecidos.
