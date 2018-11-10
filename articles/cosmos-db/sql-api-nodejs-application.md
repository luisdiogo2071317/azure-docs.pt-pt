---
title: Criar uma aplicação Web Node.js com o JavaScript SDK para gerir os dados da API SQL do Azure Cosmos DB | Microsoft Docs
description: Este tutorial do Node.js explica como utilizar o Microsoft Azure Cosmos DB para armazenar e aceder a dados a partir de uma aplicação Web Node.js Express alojada em sites do Azure.
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 09/24/2018
ms.author: sngun
ms.openlocfilehash: 0c99b7d1ef774e20a49564db269555bab95789a3
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/01/2018
ms.locfileid: "50741794"
---
# <a name="tutorial-build-a-nodejs-web-app-using-javascript-sdk-to-manage-azure-cosmos-db-sql-api-data"></a>Tutorial: Criar uma aplicação Web Node.js com o JavaScript SDK para gerir os dados da API SQL do Azure Cosmos DB

> [!div class="op_single_selector"]
> * [.NET](sql-api-dotnet-application.md)
> * [Java](sql-api-java-application.md)
> * [Node.js](sql-api-nodejs-application.md)
> * [Python](sql-api-python-application.md)
> * [Xamarin](mobile-apps-with-xamarin.md)
> 

Este tutorial do Node.js mostra-lhe como armazenar e aceder a dados com a conta da API SQL do Azure Cosmos DB a partir de uma aplicação Node.js Express alojada em sites do Azure. Neste tutorial, vai criar uma aplicação baseada na Web (aplicação Todo), que lhe permite criar, obter e concluir tarefas. As tarefas são armazenadas como documentos JSON no Azure Cosmos DB. 

Este tutorial demonstra como criar uma conta da API SQL do Azure Cosmos DB com o portal do Azure. Depois, vai criar e executar uma aplicação Web criada com base no SDK Node.js para criar a base de dados e o contentor, e adicionar itens a este último. Este tutorial utiliza a versão 2.0 do JavaScript SDK.

Também pode obter o exemplo completo no [GitHub][GitHub] e ver o ficheiro [Leia-me](https://github.com/Azure-Samples/documentdb-node-todo-app/blob/master/README.md) para obter instruções sobre como executar a aplicação.

Este tutorial abrange as seguintes tarefas:

> [!div class="checklist"]
> * Criar uma conta do Azure Cosmos DB
> * Criar uma nova aplicação Node.js
> * Ligar a aplicação ao Azure Cosmos DB
> * Executar e implementar a aplicação no Azure

## <a name="_Toc395783176"></a>Pré-requisitos

Antes de seguir as instruções deste artigo, certifique-se de que tem os seguintes recursos:

* Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar. 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [Node.js][Node.js] versão 6.10 ou superior.
* [Express generator](http://www.expressjs.com/starter/generator.html) (pode instalar o Express através de `npm install express-generator -g`)
* Instale o [Git][Git] na estação de trabalho local.

## <a name="_Toc395637761"></a>Passo 1: criar uma conta do Azure Cosmos DB
Comecemos por criar uma conta do Azure Cosmos DB. Se já tiver uma conta ou se estiver a utilizar o Emulador do Azure Cosmos DB para este tutorial, pode avançar para o [Passo 2: Criar uma aplicação Node.js](#_Toc395783178).

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [cosmos-db-keys](../../includes/cosmos-db-keys.md)]

## <a name="_Toc395783178"></a>Passo 2: criar uma nova aplicação do Node.js
Agora, vamos aprender a criar um projeto básico Olá, Mundo Node.js com a arquitetura [Express](http://expressjs.com/).

1. Abra o seu terminal favorito, como a linha de comandos Node.js.

1. Navegue para o diretório no qual pretende armazenar a nova aplicação.

1. Utilizar o Express generator para gerar uma nova aplicação designada **todo**.

   ```bash
   express todo
   ```

1. Abra o diretório **todo** e instale as dependências.

   ```bash
   cd todo
   npm install
   ```

1. Execute a nova aplicação.

   ```bash
   npm start
   ```

1. Pode ver a sua aplicação nova ao navegar para [http://localhost:3000](http://localhost:3000).
   
   ![Saiba Node.js - Captura de ecrã da aplicação Olá, Mundo numa janela do browser](./media/sql-api-nodejs-application/cosmos-db-node-js-express.png)

 Prima CTRL+C na janela do terminal para parar a aplicação e clique em **Y** para terminar a tarefa de lote.

## <a name="_Toc395783179"></a>Passo 3: Instalar os módulos necessários

O ficheiro **package.json** é um dos ficheiros criados na raiz do projeto. Este ficheiro contém uma lista de módulos adicionais que são necessários para a sua aplicação Node.js. Quando implementar esta aplicação no Azure, este ficheiro servirá para determinar quais os módulos que devem estar instalados no Azure para suportar a sua aplicação. Instale mais dois pacotes para este tutorial.

1. Abra o terminal e instale o módulo **async** através do npm.

   ```bash
   npm install async --save
   ```

2. Instale o módulo **@azure/cosmos** através do npm. 

   ```bash
   npm install @azure/cosmos
   ```

## <a name="_Toc395783180"></a>Passo 4: Ligar a aplicação Node.js ao Azure Cosmos DB
Agora que já concluiu a configuração inicial, vai escrever o código de que a aplicação Todo precisa para comunicar com o Azure Cosmos DB.

### <a name="create-the-model"></a>Criar o modelo
1. Na raiz do diretório do projeto, crie um diretório novo designado **modelos**.  

2. No diretório **modelos**, crie um novo ficheiro designado **taskDao.js**. Este ficheiro contém o código necessário para criar a base de dados e o contentor e define os métodos para ler, atualizar, criar e localizar tarefas no Azure Cosmos DB. 

3. Copie o seguinte código para o ficheiro **taskDao.js**

   ```nodejs
   // @ts-check
   const CosmosClient = require("@azure/cosmos").CosmosClient;
   const debug = require("debug")("todo:taskDao");
   class TaskDao {
     /**
      * Manages reading, adding, and updating Tasks in Cosmos DB
      * @param {CosmosClient} cosmosClient
      * @param {string} databaseId
      * @param {string} containerId
      */
     constructor(cosmosClient, databaseId, containerId) {
       this.client = cosmosClient;
       this.databaseId = databaseId;
       this.collectionId = containerId;

       this.database = null;
       this.container = null;
     }

     async init() {
       debug("Setting up the database...");
       const dbResponse = await this.client.databases.createIfNotExists({
         id: this.databaseId
       });
       this.database = dbResponse.database;
       debug("Setting up the database...done!");
       debug("Setting up the container...");
       const coResponse = await this.database.containers.createIfNotExists({
         id: this.collectionId
       });
       this.container = coResponse.container;
       debug("Setting up the container...done!");
     }

     async find(querySpec) {
       debug("Querying for items from the database");
       if (!this.container) {
         throw new Error("Collection is not initialized.");
       }
       const { result: results } = await this.container.items
        .query(querySpec)
        .toArray();
      return results;
    }

    async addItem(item) {
      debug("Adding an item to the database");
      item.date = Date.now();
      item.completed = false;
      const { body: doc } = await this.container.items.create(item);
      return doc;
    }

    async updateItem(itemId) {
      debug("Update an item in the database");
      const doc = await this.getItem(itemId);
      doc.completed = true;

      const { body: replaced } = await this.container.item(itemId).replace(doc);
      return replaced;
    }

    async getItem(itemId) {
      debug("Getting an item from the database");
      const { body } = await this.container.item(itemId).read();
      return body;
    }
  }

   module.exports = TaskDao;
   ```
4. Guarde e feche o ficheiro **taskDao.js**.  

### <a name="create-the-controller"></a>Criar o controlador

1. No diretório **rotas** do seu projeto, crie um novo ficheiro designado **tasklist.js**.  

2. Adicione o seguinte código ao **tasklist.js**. Este código carrega os módulos CosmosClient e async, que são utilizados por **tasklist.js**. Este código também define a classe **TaskList**, que é transmitida como uma instância do objeto **TaskDao** que definimos anteriormente:
   
   ```nodejs
   const TaskDao = require("../models/TaskDao");

   class TaskList {
     /**
      * Handles the various APIs for displaying and managing tasks
      * @param {TaskDao} taskDao
     */
    constructor(taskDao) {
    this.taskDao = taskDao;
    }
    async showTasks(req, res) {
      const querySpec = {
        query: "SELECT * FROM root r WHERE r.completed=@completed",
        parameters: [
          {
            name: "@completed",
            value: false
          }
        ]
      };

      const items = await this.taskDao.find(querySpec);
      res.render("index", {
        title: "My ToDo List ",
        tasks: items
      });
    }

    async addTask(req, res) {
      const item = req.body;

      await this.taskDao.addItem(item);
      res.redirect("/");
    }

    async completeTask(req, res) {
      const completedTasks = Object.keys(req.body);
      const tasks = [];

      completedTasks.forEach(task => {
        tasks.push(this.taskDao.updateItem(task));
      });

      await Promise.all(tasks);

      res.redirect("/");
    }
  }

  module.exports = TaskList;
   ```

3. Guarde e feche o ficheiro **tasklist.js**.

### <a name="add-configjs"></a>Adicionar config.js

1. Na raiz do diretório do projeto, crie um ficheiro novo designado **config.js**. 

2. Adicione o seguinte código ao ficheiro **config.js**. Este código define os parâmetros e os valores da configuração necessários para a nossa aplicação.
   
   ```nodejs
   const config = {};

   config.host = process.env.HOST || "[the endpoint URI of your Azure Cosmos DB account]";
   config.authKey =
     process.env.AUTH_KEY || "[the PRIMARY KEY value of your Azure Cosmos DB account";
   config.databaseId = "ToDoList";
   config.containerId = "Items";

   if (config.host.includes("https://localhost:")) {
     console.log("Local environment detected");
     console.log("WARNING: Disabled checking of self-signed certs. Do not have this code in production.");
     process.env.NODE_TLS_REJECT_UNAUTHORIZED = "0";
     console.log(`Go to http://localhost:${process.env.PORT || '3000'} to try the sample.`);
   }

   module.exports = config;
   ```

3. No ficheiro **config.js**, atualize os valores de HOST e AUTH_KEY com os valores encontrados na página Chaves da sua conta do Azure Cosmos DB no [portal do Microsoft Azure](https://portal.azure.com). 

4. Guarde e feche o ficheiro **config.js**.

### <a name="modify-appjs"></a>Modificar app.js

1. No diretório do projeto, abra o ficheiro **app.js**. Este ficheiro foi criado anteriormente, aquando da criação da aplicação Web Express.  

2. Adicione o seguinte código ao ficheiro **app.js**. Este código define o ficheiro de configuração a utilizar e carrega os valores para algumas variáveis que utilizará nas próximas secções. 
   
   ```nodejs
   const CosmosClient = require("@azure/cosmos").CosmosClient;
   const config = require("./config");
   const TaskList = require("./routes/tasklist");
   const TaskDao = require("./models/taskDao");

   const express = require("express");
   const path = require("path");
   const logger = require("morgan");
   const cookieParser = require("cookie-parser");
   const bodyParser = require("body-parser");

   const app = express();

   // view engine setup
   app.set("views", path.join(__dirname, "views"));
   app.set("view engine", "jade");

   // uncomment after placing your favicon in /public
   //app.use(favicon(path.join(__dirname, 'public', 'favicon.ico')));
   app.use(logger("dev"));
   app.use(bodyParser.json());
   app.use(bodyParser.urlencoded({ extended: false }));
   app.use(cookieParser());
   app.use(express.static(path.join(__dirname, "public")));

   //Todo App:
   const cosmosClient = new CosmosClient({
     endpoint: config.host,
     auth: {
       masterKey: config.authKey
     }
   });
   const taskDao = new TaskDao(cosmosClient, config.databaseId, config.containerId);
   const taskList = new TaskList(taskDao);
   taskDao
     .init(err => {
       console.error(err);
     })
     .catch(err => {
       console.error(err);
       console.error("Shutting down because there was an error settinig up the database.");
       process.exit(1);
     });

   app.get("/", (req, res, next) => taskList.showTasks(req, res).catch(next));
   app.post("/addtask", (req, res, next) => taskList.addTask(req, res).catch(next));
   app.post("/completetask", (req, res, next) => taskList.completeTask(req, res).catch(next));
   app.set("view engine", "jade");

   // catch 404 and forward to error handler
   app.use(function(req, res, next) {
     const err = new Error("Not Found");
     err.status = 404;
     next(err);
   });

   // error handler
   app.use(function(err, req, res, next) {
     // set locals, only providing error in development
     res.locals.message = err.message;
     res.locals.error = req.app.get("env") === "development" ? err : {};

     // render the error page
     res.status(err.status || 500);
     res.render("error");
   });

   module.exports = app;
   ```

3. Por fim, guarde e feche o ficheiro **app.js**.

## <a name="_Toc395783181"></a>Passo 5: criar uma interface de utilizador

Agora, vamos criar a interface de utilizador para que um utilizador possa interagir com a aplicação. A aplicação Express que criámos nas secções anteriores utiliza o **Jade** como motor de vista. Para obter mais informações sobre o Jade, veja [Linguagem Jade](http://jade-lang.com/).

1. O ficheiro **layout.jade** no diretório **vistas** é utilizado como um modelo global para outros ficheiros **.jade**. Neste passo, irá modificá-lo para utilizar o [Twitter Bootstrap](https://github.com/twbs/bootstrap), que é um conjunto de ferramentas utilizado para criar um site.  

2. Abra o ficheiro **layout.jade** encontrado na pasta **views** e substitua o conteúdo pelo seguinte código:

   ```html
   doctype html
   html
     head
       title= title
       link(rel='stylesheet', href='//ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css')
       link(rel='stylesheet', href='/stylesheets/style.css')
     body
       nav.navbar.navbar-inverse.navbar-fixed-top
         div.navbar-header
           a.navbar-brand(href='#') My Tasks
       block content
       script(src='//ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js')
       script(src='//ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js')
   ```

    Este código indica eficazmente ao motor **Jade** para compor HTML para a nossa aplicação e cria um **bloco** denominado **conteúdo** onde podemos fornecer o modelo para as nossas páginas de conteúdo. Guarde e feche o ficheiro **layout.jade**.

3. Agora, abra o ficheiro **index.jade**, a vista que será utilizada pela nossa aplicação, e substitua o conteúdo do ficheiro pelo seguinte código:

   ```html
   extends layout
   block content
        h1 #{title}
        br
        
        form(action="/completetask", method="post")
         table.table.table-striped.table-bordered
            tr
              td Name
              td Category
              td Date
              td Complete
            if (typeof tasks === "undefined")
              tr
                td
            else
              each task in tasks
                tr
                  td #{task.name}
                  td #{task.category}
                  - var date  = new Date(task.date);
                  - var day   = date.getDate();
                  - var month = date.getMonth() + 1;
                  - var year  = date.getFullYear();
                  td #{month + "/" + day + "/" + year}
                  td
                   if(task.completed) 
                    input(type="checkbox", name="#{task.id}", value="#{!task.completed}", checked=task.completed)
                   else
                    input(type="checkbox", name="#{task.id}", value="#{!task.completed}", checked=task.completed)
          button.btn.btn-primary(type="submit") Update tasks
        hr
        form.well(action="/addtask", method="post")
          label Item Name:
          input(name="name", type="textbox")
          label Item Category:
          input(name="category", type="textbox")
          br
          button.btn(type="submit") Add item
   ```

Este código expande o modelo e fornece o conteúdo para o marcador de posição **conteúdo** que vimos anteriormente no ficheiro **layout.jade**. Neste esquema, criámos dois formulários HTML.

O primeiro formulário contém uma tabela para os dados e um botão que nos permite atualizar os itens mediante a publicação no método **/completeTask** do controlador.
    
O segundo formulário contém dois campos de entrada e um botão que permite criar um novo item ao publicar no método **/addtask** do controlador. É tudo o que precisamos para que a aplicação funcione.

## <a name="_Toc395783181"></a>Passo 6: executar a sua aplicação localmente

1. Para testar a aplicação no computador local, execute `npm start` no terminal para iniciar a aplicação e, em seguida, atualize a página do browser [http://localhost:3000](http://localhost:3000). A página deve ser agora apresentada conforme mostrado na captura de ecrã seguinte:
   
    ![Captura de ecrã da aplicação MyTodo List numa janela do browser](./media/sql-api-nodejs-application/cosmos-db-node-js-localhost.png)

    > [!TIP]
    > Se receber um erro sobre o avanço no ficheiro layout.jade ou no ficheiro index.jade, certifique-se de que as primeiras duas linhas em ambos os ficheiros é justificada à esquerda, sem espaços. Se existirem espaços antes das primeiras duas linhas, remova-os, guarde ambos os ficheiros e, em seguida, atualize a janela do browser. 

2. Utilize os campos Item, Nome do Item e Categoria para introduzir uma nova tarefa e, em seguida, selecione **Adicionar Item**. Cria um documento no Azure Cosmos DB com essas propriedades. 

3. A página deverá ser atualizada para mostrar o item criado recentemente na ToDo List.
   
    ![Captura de ecrã da aplicação com um novo item na ToDo List](./media/sql-api-nodejs-application/cosmos-db-node-js-added-task.png)

4. Para concluir uma tarefa, selecione a caixa de verificação na coluna Concluir e, em seguida, selecione **Atualizar tarefas**. Este procedimento atualiza o documento já criado e retira-o da vista.

5. Para parar a aplicação, prima CTRL+C na janela do terminal e, em seguida, selecione **Y** para terminar a tarefa de lote.

## <a name="_Toc395783182"></a>Passo 7: Implementar a aplicação em sites do Azure

1. Se ainda não o fez, ative um repositório git para o seu site do Azure. Pode encontrar instruções sobre como ativar um repositório git no tópico [Implementação de Git Local no Serviço de Aplicações do Azure](../app-service/app-service-deploy-local-git.md).

2. Adicione o seu site do Azure como um git remoto.
   
   ```bash
   git remote add azure https://username@your-azure-website.scm.azurewebsites.net:443/your-azure-website.git
   ```

3. Implemente a aplicação ao enviá-la para a ligação remota.
   
   ```bash
   git push azure master
   ```

4. Em alguns segundos, a aplicação Web é publicada e iniciada num browser.

Se quiser transferir ou consultar a aplicação de referência completa deste tutorial, pode transferi-la a partir do [GitHub][GitHub].

## <a name="_Toc395637775"></a>Passos seguintes

Neste tutorial, aprendeu a criar uma aplicação Web Node.js com o JavaScript SDK para gerir os dados da API SQL do Azure Cosmos DB. Pode agora avançar para o artigo seguinte:

> [!div class="nextstepaction"]
> [Criar aplicações móveis com o Xamarin e o Azure Cosmos DB](mobile-apps-with-xamarin.md)


[Node.js]: http://nodejs.org/
[Git]: http://git-scm.com/
[GitHub]: https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-todo-app

