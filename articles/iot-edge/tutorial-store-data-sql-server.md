---
title: Tutorial de armazenamento dados com o módulo SQL - Azure IoT Edge | Documentos da Microsoft
description: Saiba como armazenar dados localmente no seu dispositivo IoT Edge com um módulo do SQL Server
services: iot-edge
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 01/04/2019
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: 426e4fe05890f1669859545db3d731943a12428a
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/14/2019
ms.locfileid: "54260180"
---
# <a name="tutorial-store-data-at-the-edge-with-sql-server-databases"></a>Tutorial: Store dados na periferia com bancos de dados do SQL Server

Utilize o Azure IoT Edge e o SQL Server para armazenar e consultar dados na periferia. O Azure IoT Edge tem capacidades de armazenamento básico em cache as mensagens se um dispositivo ficar offline e, em seguida, reencaminhe-os quando a ligação for restabelecida. No entanto, pode querer capacidades de armazenamento mais avançadas, como a capacidade de consultar dados localmente. Ao incorporar bases de dados locais, os dispositivos IoT Edge podem efetuar uma computação mais complexa sem necessidade de manter uma ligação ao Hub IoT. Por exemplo, um sensor num computador carrega dados para a cloud uma vez por mês para relatórios e melhorando a um módulo de aprendizagem automática. No entanto, se um técnico de campo está a funcionar na máquina, podem aceder a alguns últimos dias de dados de sensor localmente.

Este artigo fornece instruções para implementar uma base de dados do SQL Server num dispositivo IoT Edge. As Funções do Azure, em execução no dispositivo IoT Edge, estruturam os dados recebidos e, em seguida, envia-os para a base de dados. Os passos neste artigo também podem ser aplicados a outras bases de dados que funcionam em contentores, como o MySQL ou o PostgreSQL.

Neste tutorial, ficará a saber como: 

> [!div class="checklist"]
> * Utilize o Visual Studio Code para criar uma função do Azure
> * Implementar uma base de dados do SQL Server no seu dispositivo IoT Edge
> * Utilize o Visual Studio Code para criar módulos e implementá-los no seu dispositivo IoT Edge
> * Ver os dados gerados

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

Um dispositivo Azure IoT Edge:

* Pode seguir os passos no início rápido para [dispositivos Linux](quickstart-linux.md) ou [Windows](quickstart.md) para utilizar o seu computador de desenvolvimento ou uma máquina virtual como um dispositivo Edge. 

Recursos da cloud:

* Um [Hub IoT](../iot-hub/iot-hub-create-through-portal.md) no escalão gratuito ou standard no Azure. 

Recursos de desenvolvimento:

* [Visual Studio Code](https://code.visualstudio.com/). 
* [C#para a extensão do Visual Studio Code (com tecnologia da omnisharp) para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). 
* [Ferramentas de IoT do Azure para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
* [SDK de .NET Core 2.1](https://www.microsoft.com/net/download). 
* [Docker CE](https://docs.docker.com/install/). 

## <a name="create-a-container-registry"></a>Criar um registo de contentores

Neste tutorial, vai utilizar as ferramentas de IoT do Azure para Visual Studio Code para criar um módulo e criar um **imagem de contentor** dos arquivos. Em seguida, vai enviar essa imagem para um **registo** que armazena e gere as suas imagens. Por fim, vai implementar a imagem a partir do registo para ser executada no seu dispositivo IoT Edge.  

Pode utilizar qualquer registo compatível com o Docker para armazenar as imagens de contentor. São dois populares serviços de registo de Docker [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) e [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). Este tutorial utiliza o Azure Container Registry. 

Se ainda não tiver um registo de contentor, siga estes passos para criar uma nova no Azure:

1. No [Portal do Azure](https://portal.azure.com), selecione **Criar um recurso** > **Contentores** > **Registo de Contentor**.

2. Indique os valores seguintes para criar o seu registo de contentor:

   | Campo | Valor | 
   | ----- | ----- |
   | Nome de registo | Indique um nome exclusivo. |
   | Subscrição | Selecione uma subscrição na lista pendente. |
   | Grupo de recursos | Recomendamos que utilize o mesmo grupo de recursos para todos os recursos de teste que criou durante os inícios rápidos e tutoriais do IoT Edge. Por exemplo, **IoTEdgeResources**. |
   | Localização | Escolha uma localização perto de si. |
   | Utilizador admin | Defina para **Ativar**. |
   | SKU | Selecione **Básico**. | 

5. Selecione **Criar**.

6. Depois de o registo de contentores ser criado, navegue para o mesmo e selecione **Chaves de acesso**. 

7. Copie os valores de **Servidor de início de sessão**, **Nome de utilizador** e **Palavra-passe**. Utilize estes valores mais tarde no tutorial para fornecer acesso ao registo de contentor.  

## <a name="create-a-function-project"></a>Criar um projeto de função

Para enviar dados numa base de dados, precisa de um módulo que possa estruturar corretamente os dados e, em seguida, armazená-los numa tabela. 

Os passos seguintes mostram como criar uma função do IoT Edge com o Visual Studio Code e as ferramentas de IoT do Azure.

1. Abra o Visual Studio Code.

2. Abra a paleta de comandos do VS Code ao selecionar **Ver** > **Paleta de Comandos**.

3. Na paleta de comandos, escreva e execute o comando **Azure IoT Edge: Nova solução de IoT Edge**. Na paleta de comandos, indique as seguintes informações para criar a sua solução: 

   | Campo | Valor |
   | ----- | ----- |
   | Selecionar pasta | Escolha a localização no computador de desenvolvimento na qual o VS Code vai criar os ficheiros da solução. |
   | Indicar um nome para a solução | Introduza um nome descritivo para a sua solução, como **SqlSolution**, ou aceite a predefinição. |
   | Selecionar modelo de módulo | Escolher **as funções do Azure - C#** . |
   | Indicar um nome para o módulo | Atribua o nome **sqlFunction** ao módulo. |
   | Indicar o repositório de imagens do Docker para o módulo | Os repositórios de imagens incluem o nome do seu registo de contentor e o nome da sua imagem de contentor. A imagem de contentor é pré-preenchida no passo anterior. Substitua **localhost:5000** pelo valor do servidor de início de sessão do registo de contentor do Azure Container Registry. Pode obter o servidor de início de sessão na página Overview (Descrição Geral) do registo de contentor no portal do Azure. A cadeia final se parece com \<nome do registo\>.azurecr.io/sqlFunction. |

   A janela do VS Code carrega a área de trabalho da solução do IoT Edge. 
   
4. Na sua solução de IoT Edge, abra o \.ficheiro env. 

   Sempre que cria uma nova solução de IoT Edge, VS Code pede-lhe que forneça as suas credenciais de registo no \.ficheiro env. Este ficheiro é ignorado o git, e a extensão de IoT Edge usa mais tarde para fornecer acesso de registo para o seu dispositivo IoT Edge. 

   Se não forneceu o seu registo de contentor no passo anterior, mas aceite a predefinição localhost:5000, não terá um \.ficheiro env.

5. No ficheiro .env, indique as suas credenciais de registo ao runtime do IoT Edge para que este possa aceder às imagens do módulo. Encontre as secções **CONTAINER_REGISTRY_USERNAME** e **CONTAINER_REGISTRY_PASSWORD** e insira as suas credenciais após o símbolo de igual: 

   ```env
   CONTAINER_REGISTRY_USERNAME_yourregistry=<username>
   CONTAINER_REGISTRY_PASSWORD_yourregistry=<password>
   ```

6. Guarde o ficheiro. env.

7. No Explorador do VS Code, abra **módulos** > **sqlFunction** > **sqlFunction.cs**.

8. Substitua os conteúdos do ficheiro pelo seguinte código:

   ```csharp
   using System;
   using System.Collections.Generic;
   using System.IO;
   using System.Text;
   using System.Threading.Tasks;
   using Microsoft.Azure.Devices.Client;
   using Microsoft.Azure.WebJobs;
   using Microsoft.Azure.WebJobs.Extensions.EdgeHub;
   using Microsoft.Azure.WebJobs.Host;
   using Microsoft.Extensions.Logging;
   using Newtonsoft.Json;
   using Sql = System.Data.SqlClient;

   namespace Functions.Samples
   {
       public static class sqlFunction
       {
           [FunctionName("sqlFunction")]
           public static async Task FilterMessageAndSendMessage(
               [EdgeHubTrigger("input1")] Message messageReceived,
               [EdgeHub(OutputName = "output1")] IAsyncCollector<Message> output,
               ILogger logger)
           {
               const int temperatureThreshold = 20;
               byte[] messageBytes = messageReceived.GetBytes();
               var messageString = System.Text.Encoding.UTF8.GetString(messageBytes);

               if (!string.IsNullOrEmpty(messageString))
               {
                   logger.LogInformation("Info: Received one non-empty message");
                   // Get the body of the message and deserialize it.
                   var messageBody = JsonConvert.DeserializeObject<MessageBody>(messageString);

                   //Store the data in SQL db
                   const string str = "<sql connection string>";
                   using (Sql.SqlConnection conn = new Sql.SqlConnection(str))
                   {
                       conn.Open();
                       var insertMachineTemperature = "INSERT INTO MeasurementsDB.dbo.TemperatureMeasurements VALUES (CONVERT(DATETIME2,'" + messageBody.timeCreated + "', 127), 'machine', " + messageBody.machine.temperature + ");";
                       var insertAmbientTemperature = "INSERT INTO MeasurementsDB.dbo.TemperatureMeasurements VALUES (CONVERT(DATETIME2,'" + messageBody.timeCreated + "', 127), 'ambient', " + messageBody.ambient.temperature + ");"; 
                       using (Sql.SqlCommand cmd = new Sql.SqlCommand(insertMachineTemperature + "\n" + insertAmbientTemperature, conn))
                       {
                           //Execute the command and log the # rows affected.
                           var rows = await cmd.ExecuteNonQueryAsync();
                           logger.LogInformation($"{rows} rows were updated");
                       }
                   }

                   if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
                   {
                       // Send the message to the output as the temperature value is greater than the threashold.
                       var filteredMessage = new Message(messageBytes);
                       // Copy the properties of the original message into the new Message object.
                       foreach (KeyValuePair<string, string> prop in messageReceived.Properties)
                       {filteredMessage.Properties.Add(prop.Key, prop.Value);}
                       // Add a new property to the message to indicate it is an alert.
                       filteredMessage.Properties.Add("MessageType", "Alert");
                       // Send the message.       
                       await output.AddAsync(filteredMessage);
                       logger.LogInformation("Info: Received and transferred a message with temperature above the threshold");
                   }
               }
           }
       }
       //Define the expected schema for the body of incoming messages.
       class MessageBody
       {
           public Machine machine {get; set;}
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
   }
   ```

6. Na linha 35, substitua a cadeia de caracteres **\<cadeia de ligação sql\>** com a seguinte cadeia de caracteres. O **origem de dados** propriedade referencia o nome do contentor do SQL Server, que pode ser criado com o nome **SQL** na próxima seção. 

   ```csharp
   Data Source=tcp:sql,1433;Initial Catalog=MeasurementsDB;User Id=SA;Password=Strong!Passw0rd;TrustServerCertificate=False;Connection Timeout=30;
   ```

7. Guardar a **sqlFunction.cs** ficheiro. 

8. Abra o **sqlFunction.csproj** ficheiro.

9. Encontrar o grupo de referências de pacote e adicione uma para SqlClient inclui um novo. 

   ```csproj
   <PackageReference Include="System.Data.SqlClient" Version="4.5.1"/>
   ```

10. Guardar a **sqlFunction.csproj** ficheiro.

## <a name="add-a-sql-server-container"></a>Adicionar um contentor do SQL Server

Um [Manifesto de implementação](module-composition.md) declara os módulos que o runtime do IoT Edge irá instalar no seu dispositivo IoT Edge. Que forneceu o código para tornar um módulo de função personalizado na secção anterior, mas o módulo do SQL Server já está incorporado. Apenas tem de indicar ao runtime do IoT Edge para o incluir e configurar no seu dispositivo. 

1. No explorador do Visual Studio Code, abra o ficheiro **deployment.template.json**. 

2. Encontrar o **módulos** secção. Devem existir dois módulos listados: **tempSensor**, que gera dados simulados, e o módulo **sqlFunction**.

3. Se estiver a utilizar contentores do Windows, modifique a secção **sqlFunction.settings.image**.

   ```json
   "image": "${MODULES.sqlFunction.windows-amd64}"
   ```

4. Adicione o código seguinte para declarar um terceiro módulo. Adicione uma vírgula depois da secção sqlFunction e insira:

   ```json
   "sql": {
     "version": "1.0",
     "type": "docker",
     "status": "running",
     "restartPolicy": "always",
     "env":{},
     "settings": {
       "image": "",
       "createOptions": ""
     }
   }
   ```

   ![Adicionar módulo do SQL server se manifeste](./media/tutorial-store-data-sql-server/view_json_sql.png)

5. Dependendo do tipo de contentores do Docker no seu dispositivo IoT Edge, atualize o **sql** parâmetros de módulo com o código a seguir:
   * Contentores do Windows:

      ```json
      "env": {
        "ACCEPT_EULA": {"value": "Y"},
        "SA_PASSWORD": {"value": "Strong!Passw0rd"}
      },
      "settings": {
        "image": "microsoft/mssql-server-windows-developer",
        "createOptions": {
          "HostConfig": {
            "Mounts": [{"Target": "C:\\mssql","Source": "sqlVolume","Type": "volume"}],
            "PortBindings": {
              "1433/tcp": [{"HostPort": "1401"}]
            }
          }
        }
      }
      ```

   * Contentores do Linux:

      ```json
      "env": {
        "ACCEPT_EULA": {"value": "Y"},
        "SA_PASSWORD": {"value": "Strong!Passw0rd"}
      },
      "settings": {
        "image": "mcr.microsoft.com/mssql/server:latest",
        "createOptions": {
          "HostConfig": {
            "Mounts": [{"Target": "/var/opt/mssql","Source": "sqlVolume","Type": "volume"}],
            "PortBindings": {
              "1433/tcp": [{"HostPort": "1401"}]
            }
          }
        }
      }
      ```

   >[!Tip]
   >Sempre que criar um contentor do SQL Server num ambiente de produção, deve [alterar a palavra-passe de administrador do sistema predefinida](https://docs.microsoft.com/sql/linux/quickstart-install-connect-docker#change-the-sa-password).

6. Guarde o ficheiro **deployment.template.json**.

## <a name="build-your-iot-edge-solution"></a>Criar a sua solução do IoT Edge

Nas secções anteriores, criou uma solução com um módulo e, em seguida, adicionou outra ao modelo de manifesto de implementação. Agora, tem de criar a solução, criar imagens de contentor para os módulos e enviar as imagens para o seu registo de contentor. 

1. Inicie sessão no seu registo de contentor do Visual Studio Code para que possa enviar as imagens para o seu registo. Utilize as mesmas credenciais que adicionou ao ficheiro .env. Introduza o seguinte comando no terminal integrado:

    ```csh/sh
    docker login -u <ACR username> <ACR login server>
    ```
    
    Lhe for pedida a palavra-passe. Cole a palavra-passe de linha de comandos (a sua palavra-passe é ocultada por segurança) e prima **Enter**. 

    ```csh/sh
    Password: <paste in the ACR password and press enter>
    Login Succeeded
    ```

2. No explorador do VS Code, clique com o botão direito do rato no ficheiro **deployment.template.json** e selecione **Criar e Emitir solução do IoT Edge**. 

Quando indicar ao Visual Studio Code para criar a solução, este utiliza primeiro as informações no modelo de implementação e gera um ficheiro deployment.json numa nova pasta com o nome **config**. Em seguida, executa dois comandos no terminal integrado: `docker build` e `docker push`. Estes dois comandos Compile seu código, o módulo de colocar num contentor e, em seguida, enviar o código para o registo de contentor que especificou quando a solução é inicializado. 

## <a name="deploy-the-solution-to-a-device"></a>Implementar a solução num dispositivo

Pode definir módulos num dispositivo através do Hub IoT, mas também pode aceder o seu Hub IoT e dispositivos através do Visual Studio Code. Nesta secção, pode configurar o acesso ao Hub IoT e, em seguida, utilizar o VS Code para implementar a solução no seu dispositivo IoT Edge. 

1. Na paleta de comandos VS Code, selecione **IoT Hub do Azure: Selecione o IoT Hub**.

2. Siga os avisos para iniciar sessão na conta do Azure. 

3. Na paleta de comandos, selecione a sua subscrição do Azure e, em seguida, selecione o seu Hub IoT. 

4. No explorador do VS Code, expanda a secção **Dispositivos do Hub IoT do Azure**. 

5. Clique com o botão direito do rato no dispositivo destinado à implementação e selecione **Criar Implementação para o único dispositivo**. 

   ![Criar implementação para dispositivo único](./media/tutorial-store-data-sql-server/create-deployment.png)

6. No Explorador de ficheiros, navegue para o **config** pasta dentro de sua solução e escolha **deployment.amd64**. Clique em **Selecionar manifesto de implementação do Edge**. 

Se a implementação for bem-sucedida, é apresentada uma mensagem de confirmação no resultado do VS Code. 

Atualize o estado do seu dispositivo na secção de dispositivos de Hub IoT do Azure do VS Code. Os módulos novos são apresentados e começarão a relatórios como em execução ao longo nos próximos minutos, como os contentores são instalados e iniciados. Também pode verificar se todos os módulos estão em execução no seu dispositivo. No dispositivo IoT Edge, execute o seguinte comando para ver o estado dos módulos. 

   ```cmd/sh
   iotedge list
   ```

## <a name="create-the-sql-database"></a>Criar uma base de dados SQL

Ao aplicar o manifesto de implementação ao seu dispositivo, obtém três módulos em execução. O módulo tempSensor gera dados de ambiente simulados. O módulo sqlFunction utiliza os dados e formata-os para uma base de dados. Esta secção orienta-o na configuração da base de dados do SQL Server para armazenar os dados de temperatura. 

Execute os seguintes comandos no seu dispositivo IoT Edge. Estes comandos ligar para o **sql** módulo em execução no seu dispositivo e criar uma base de dados e uma tabela para conter os dados de temperatura que está a ser enviados para o mesmo. 

1. Uma ferramenta da linha de comandos no seu dispositivo IoT Edge, ligar à base de dados. 
   * Contentor do Windows:
   
      ```cmd
      docker exec -it sql cmd
      ```
    
   * Contentor do Linux: 

      ```bash
      sudo docker exec -it sql bash
      ```

2. Abra a ferramenta de comandos SQL.
   * Contentor do Windows:

      ```cmd
      sqlcmd -S localhost -U SA -P "Strong!Passw0rd"
      ```

   * Contentor do Linux: 

      ```bash
      /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P 'Strong!Passw0rd'
      ```

3. Criar a base de dados: 

   * Contentor do Windows
      ```sql
      CREATE DATABASE MeasurementsDB
      ON
      (NAME = MeasurementsDB, FILENAME = 'C:\mssql\measurementsdb.mdf')
      GO
      ```

   * Contentor do Linux
      ```sql
      CREATE DATABASE MeasurementsDB
      ON
      (NAME = MeasurementsDB, FILENAME = '/var/opt/mssql/measurementsdb.mdf')
      GO
      ```

4. Defina a tabela.

   ```sql
   CREATE TABLE MeasurementsDB.dbo.TemperatureMeasurements (measurementTime DATETIME2, location NVARCHAR(50), temperature FLOAT)
   GO
   ```

Pode personalizar o ficheiro de docker do SQL Server para configurar automaticamente o SQL Server para ser implementado em vários dispositivos IoT Edge. Para obter mais informações, veja o [Projeto de demonstração de contentor do Microsoft SQL Server](https://github.com/twright-msft/mssql-node-docker-demo-app). 

## <a name="view-the-local-data"></a>Ver os dados locais

Assim que a tabela for criada, o módulo sqlFunction começa a armazenar os dados numa base de dados local do SQL Server 2017 no seu dispositivo IoT Edge. 

A partir da ferramenta de comandos SQL, execute o seguinte comando para ver os dados de tabela formatados: 

   ```sql
   SELECT * FROM MeasurementsDB.dbo.TemperatureMeasurements
   GO
   ```

   ![Ver conteúdos da base de dados local](./media/tutorial-store-data-sql-server/view-data.png)



## <a name="clean-up-resources"></a>Limpar recursos

Se planeia avançar para o próximo artigo recomendado, pode manter os recursos e as configurações que criou e reutilizá-los. Também pode continuar a utilizar o mesmo dispositivo IoT Edge como um dispositivo de teste. 

Caso contrário, pode eliminar as configurações locais e os recursos do Azure que criou neste artigo para evitar custos. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

[!INCLUDE [iot-edge-clean-up-local-resources](../../includes/iot-edge-clean-up-local-resources.md)]



## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, criou um módulo das Funções do Azure que contém código para filtrar dados não processados gerados pelo seu dispositivo IoT Edge. Quando estiver pronto para criar os seus próprios módulos, pode saber mais sobre como [Desenvolver Funções do Azure com o Azure IoT Edge para Visual Studio Code](how-to-develop-csharp-function.md). 

Continue para os próximos tutoriais para saber mais sobre outras formas em que o Azure IoT Edge o pode ajudar a transformar os dados em informações empresariais na periferia.

> [!div class="nextstepaction"]
> [Filtrar dados de sensores com código C#](tutorial-csharp-module.md)
