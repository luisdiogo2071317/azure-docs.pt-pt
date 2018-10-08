---
title: Armazenar dados com o módulo SQL do Azure IoT Edge | Microsoft Docs
description: Saiba como armazenar dados localmente no seu dispositivo IoT Edge com um módulo do SQL Server
services: iot-edge
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 09/21/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 0cfcc01d50937f80a5c621c95ccec0901da270f3
ms.sourcegitcommit: 42405ab963df3101ee2a9b26e54240ffa689f140
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/28/2018
ms.locfileid: "47423277"
---
# <a name="tutorial-store-data-at-the-edge-with-sql-server-databases"></a>Tutorial: Armazenar dados na periferia com bases de dados do SQL Server

Utilize o Azure IoT Edge e o SQL Server para armazenar e consultar dados na periferia. O Azure IoT Edge tem funcionalidades de armazenamento básicas incorporadas que colocam as mensagens em cache se um dispositivo ficar offline e em seguida, reencaminha-as quando a ligação é restabelecida. No entanto, pode querer capacidades de armazenamento mais avançadas, como a capacidade de consultar dados localmente. Ao incorporar bases de dados locais, os dispositivos IoT Edge podem efetuar uma computação mais complexa sem necessidade de manter uma ligação ao Hub IoT. Por exemplo, um técnico de campo pode visualizar os últimos dias de dados de sensor numa máquina localmente, apesar desses dados apenas serem carregados para a cloud uma vez por mês para ajudar a melhorar um modelo de machine learning.

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

* Um [Hub IoT](../iot-hub/iot-hub-create-through-portal.md) no escalão gratuito no Azure. 

Recursos de desenvolvimento:

* [Visual Studio Code](https://code.visualstudio.com/). 
* [Extensão C# para o Visual Studio Code (com tecnologia da OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) para o Visual Studio Code. 
* [Extensão Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) para o Visual Studio Code. 
* [SDK de .NET Core 2.1](https://www.microsoft.com/net/download). 
* [Docker CE](https://docs.docker.com/install/). 

## <a name="create-a-container-registry"></a>Criar um registo de contentores
Neste tutorial, vai utilizar a extensão Azure IoT Edge para VS Code para criar um módulo e criar uma **imagem de contentor** a partir dos ficheiros. Em seguida, vai enviar essa imagem para um **registo** que armazena e gere as suas imagens. Por fim, vai implementar a imagem a partir do registo para ser executada no seu dispositivo IoT Edge.  

Pode utilizar qualquer registo compatível com o Docker neste tutorial. O [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) e o [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags) são dois populares serviços de registo do Docker que estão disponíveis na cloud. Este tutorial utiliza o Azure Container Registry. 

1. No [Portal do Azure](https://portal.azure.com), selecione **Criar um recurso** > **Contentores** > **Registo de Contentor**.

    ![Criar registo de contentor](./media/tutorial-deploy-function/create-container-registry.png)

2. Atribua um nome ao registo e selecione uma subscrição.
3. Para o grupo de recursos, é recomendado que utilize o mesmo nome do grupo de recursos que contém o seu Hub IoT. Ao manter todos os recursos no mesmo grupo, pode geri-los em conjunto. Por exemplo, eliminar o grupo de recursos utilizado para teste elimina todos os recursos de teste que se encontram no grupo. 
4. Defina o SKU como **Básico** e **Utilizador administrador** como **Ativar**. 
5. Clique em **Criar**.
6. Assim que o registo de contentores for criado, navegue para o mesmo e selecione **Chaves de acesso**. 
7. Copie os valores de **Servidor de início de sessão**, **Nome de utilizador** e **Palavra-passe**. Vai utilizá-los mais à frente no tutorial. 

## <a name="create-a-function-project"></a>Criar um projeto de função

Para enviar dados numa base de dados, precisa de um módulo que possa estruturar corretamente os dados e, em seguida, armazená-los numa tabela. 

Os passos seguintes mostram-lhe como criar uma função do IoT Edge com o Visual Studio Code e a extensão Azure IoT Edge.

1. Abra o Visual Studio Code.
2. Abra o terminal integrado do VS Code ao selecionar **Ver** > **Terminal**.
3. Abra a paleta de comandos do VS Code ao selecionar **Ver** > **Paleta de Comandos**.
4. Na paleta de comandos, escreva e execute o comando **Azure: Sign in** e siga as instruções para iniciar sessão na sua conta do Azure. Se já iniciou sessão, pode ignorar este passo.
3. Na paleta de comandos, escreva e execute o comando **Azure IoT Edge: Nova solução do IoT Edge**. Na paleta de comandos, indique as seguintes informações para criar a sua solução: 
   1. Selecione a pasta onde quer criar a solução. 
   2. Indique um nome para a sua solução ou aceite a predefinição **EdgeSolution**.
   3. Selecione **Funções do Azure - C#** como modelo de módulo. 
   4. Atribua o nome **sqlFunction** ao módulo. 
   5. Especifique o Azure Container Registry que criou na secção anterior como repositório de imagens para o primeiro módulo. Substitua **localhost:5000** pelo valor do servidor de início de sessão que copiou. A cadeia final é semelhante a **\<nome do registo\>.azurecr.io/sqlFunction**.

4. A janela do VS Code carrega a área de trabalho da solução do IoT Edge. Existe uma pasta **modules**, uma pasta **.vscode** e um ficheiro de modelo do manifesto de implementação. Abra **modules** > **sqlFunction** > **EdgeHubTrigger Csharp** > **run.csx**.

5. Substitua os conteúdos do ficheiro pelo seguinte código:

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

6. Na linha 24, substitua a cadeia **\<sql connection string\>** pela cadeia seguinte. A propriedade **Origem de Dados** referencia o nome do contentor do SQL Server, que irá criar com o nome **SQL** na secção seguinte. 

   ```C#
   Data Source=tcp:sql,1433;Initial Catalog=MeasurementsDB;User Id=SA;Password=Strong!Passw0rd;TrustServerCertificate=False;Connection Timeout=30;
   ```

7. Guarde o ficheiro **run.csx**. 

## <a name="add-a-sql-server-container"></a>Adicionar um contentor do SQL Server

Um [Manifesto de implementação](module-composition.md) declara os módulos que o runtime do IoT Edge irá instalar no seu dispositivo IoT Edge. Adicionou o código para criar um módulo de Função personalizado na secção anterior, mas o módulo do SQL Server já está incorporado. Apenas tem de indicar ao runtime do IoT Edge para o incluir e configurar no seu dispositivo. 

1. No explorador do Visual Studio Code, abra o ficheiro **deployment.template.json**. 
2. Localize a secção **moduleContent.$edgeAgent.properties.desired.modules**. Devem existir dois módulos listados: **tempSensor**, que gera dados simulados, e o módulo **sqlFunction**.
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
       "settings": {
           "image": "",
           "createOptions": ""
       }
   }
   ```

   Eis um exemplo, se houver uma confusão com a adição de um elemento JSON. ![Adicionar um contentor do sql server](./media/tutorial-store-data-sql-server/view_json_sql.png)

5. Consoante o tipo de contentores do Docker no dispositivo IoT Edge, atualize os parâmetros **sql.settings** com o seguinte código:

   * Contentores do Windows:

      ```json
      "image": "microsoft/mssql-server-windows-developer",
      "createOptions": "{\"Env\": [\"ACCEPT_EULA=Y\",\"SA_PASSWORD=Strong!Passw0rd\"],\"HostConfig\": {\"Mounts\": [{\"Target\": \"C:\\\\mssql\",\"Source\": \"sqlVolume\",\"Type\": \"volume\"}],\"PortBindings\": {\"1433/tcp\": [{\"HostPort\": \"1401\"}]}}}"
      ```

   * Contentores do Linux:

      ```json
      "image": "microsoft/mssql-server-linux:2017-latest",
      "createOptions": "{\"Env\": [\"ACCEPT_EULA=Y\",\"MSSQL_SA_PASSWORD=Strong!Passw0rd\"],\"HostConfig\": {\"Mounts\": [{\"Target\": \"/var/opt/mssql\",\"Source\": \"sqlVolume\",\"Type\": \"volume\"}],\"PortBindings\": {\"1433/tcp\": [{\"HostPort\": \"1401\"}]}}}"
      ```

   >[!Tip]
   >Sempre que criar um contentor do SQL Server num ambiente de produção, deve [alterar a palavra-passe de administrador do sistema predefinida](https://docs.microsoft.com/sql/linux/quickstart-install-connect-docker#change-the-sa-password).

6. Guarde o ficheiro **deployment.template.json**.

## <a name="build-your-iot-edge-solution"></a>Criar a sua solução do IoT Edge

Nas secções anteriores, criou uma solução com um módulo e, em seguida, adicionou outra ao modelo de manifesto de implementação. Agora, tem de criar a solução, criar imagens de contentor para os módulos e enviar as imagens para o seu registo de contentor. 

1. No ficheiro .env, indique as suas credenciais de registo ao runtime do IoT Edge para que este possa aceder às imagens do módulo. Encontre as secções **CONTAINER_REGISTRY_USERNAME** e **CONTAINER_REGISTRY_PASSWORD** e insira as suas credenciais após o símbolo de igual: 

   ```env
   CONTAINER_REGISTRY_USERNAME_yourContainerReg=<username>
   CONTAINER_REGISTRY_PASSWORD_yourContainerReg=<password>
   ```

2. Guarde o ficheiro. env.
3. Inicie sessão no seu registo de contentor do Visual Studio Code para que possa enviar as imagens para o seu registo. Utilize as mesmas credenciais que adicionou ao ficheiro .env. Introduza o seguinte comando no terminal integrado:

    ```csh/sh
    docker login -u <ACR username> <ACR login server>
    ```
    Ser-lhe-á solicitada a palavra-passe. Cole a palavra-passe na linha de comandos e prima **Enter**.

    ```csh/sh
    Password: <paste in the ACR password and press enter>
    Login Succeeded
    ```

4. No explorador do VS Code, clique com o botão direito do rato no ficheiro **deployment.template.json** e selecione **Criar e Emitir solução do IoT Edge**. 

## <a name="deploy-the-solution-to-a-device"></a>Implementar a solução num dispositivo

Pode definir módulos num dispositivo através do Hub IoT, mas também pode aceder o seu Hub IoT e dispositivos através do Visual Studio Code. Nesta secção, pode configurar o acesso ao Hub IoT e, em seguida, utilizar o VS Code para implementar a solução no seu dispositivo IoT Edge. 

1. Na paleta de comandos do VS Code, selecione **Azure IoT Hub: Select IoT Hub**.
2. Siga os avisos para iniciar sessão na conta do Azure. 
3. Na paleta de comandos, selecione a sua subscrição do Azure e, em seguida, selecione o seu Hub IoT. 
4. No explorador do VS Code, expanda a secção **Dispositivos do Hub IoT do Azure**. 
5. Clique com o botão direito do rato no dispositivo destinado à implementação e selecione **Criar Implementação para o único dispositivo**. 
6. No explorador de ficheiros, navegue para a pasta **config** na solução e selecione **deployment.json**. Clique em **Selecionar manifesto de implementação do Edge**. 

Se a implementação for efetuada com êxito, é apresentada uma mensagem de confirmação no resultado do VS Code. Também pode verificar se todos os módulos estão em execução no seu dispositivo. 

No dispositivo IoT Edge, execute o seguinte comando para ver o estado dos módulos. Esta operação poderá demorar alguns minutos.

   ```PowerShell
   iotedge list
   ```

## <a name="create-the-sql-database"></a>Criar uma base de dados SQL

Ao aplicar o manifesto de implementação ao seu dispositivo, obtém três módulos em execução. O módulo tempSensor gera dados de ambiente simulados. O módulo sqlFunction utiliza os dados e formata-os para uma base de dados. 

Esta secção orienta-o na configuração da base de dados do SQL Server para armazenar os dados de temperatura. 

1. Numa linha de comandos, ligue à base de dados. 
   * Contentor do Windows:
   
      ```cmd
      docker exec -it sql cmd
      ```
    
   * Contentor do Linux: 

      ```bash
      docker exec -it sql bash
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

   ![Ver dados locais](./media/tutorial-store-data-sql-server/view-data.png)



## <a name="clean-up-resources"></a>Limpar recursos

Se planeia avançar para o próximo artigo recomendado, pode manter os recursos e as configurações que criou e reutilizá-los. Também pode continuar a utilizar o mesmo dispositivo IoT Edge como um dispositivo de teste. 

Caso contrário, pode eliminar as configurações locais e os recursos do Azure que criou neste artigo para evitar custos. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

[!INCLUDE [iot-edge-clean-up-local-resources](../../includes/iot-edge-clean-up-local-resources.md)]



## <a name="next-steps"></a>Passos seguintes

Neste tutorial, criou um módulo das Funções do Azure que contém código para filtrar dados não processados gerados pelo seu dispositivo IoT Edge. Quando estiver pronto para criar os seus próprios módulos, pode saber mais sobre como [Desenvolver Funções do Azure com o Azure IoT Edge para Visual Studio Code](how-to-develop-csharp-function.md). 

Continue para os próximos tutoriais para saber mais sobre outras formas em que o Azure IoT Edge o pode ajudar a transformar os dados em informações empresariais na periferia.

> [!div class="nextstepaction"]
> [Localizar médias com um período flutuante no Azure Stream Analytics](tutorial-deploy-stream-analytics.md)
