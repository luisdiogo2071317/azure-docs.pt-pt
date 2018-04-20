---
title: Como trabalhar com o SDK Node.js do servidor de back-end para Mobile Apps | Microsoft Docs
description: Saiba como trabalhar com o SDK Node.js do servidor de back-end para Mobile Apps do Azure App Service.
services: app-service\mobile
documentationcenter: ''
author: elamalani
manager: elamalani
editor: ''
ms.assetid: e7d97d3b-356e-4fb3-ba88-38ecbda5ea50
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: node
ms.topic: article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: 335186deccaa82b9a8d262d62dd8ce5d620446b6
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/20/2018
---
# <a name="how-to-use-the-mobile-apps-nodejs-sdk"></a>Como utilizar o SDK do Mobile Apps Node.js
[!INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

Este artigo fornece informações detalhadas e exemplos mostram como trabalhar com um Node.js back-end na funcionalidade Mobile Apps do App Service do Azure.

## <a name="Introduction"></a>Introdução
As Mobile Apps fornece a capacidade de adicionar um acesso de dados com otimização de mobile Web API a uma aplicação web. O SDK de aplicações móveis é fornecido para aplicações web do ASP.NET e Node.js. O SDK fornece as seguintes operações:

* Operações de tabela (ler, inserir, atualizar, eliminar) para acesso a dados
* Operações de API de personalizado

Ambas as operações de fornecem para autenticação através de todos os fornecedores de identidade que permite o App Service do Azure. Estes fornecedores incluem fornecedores de identidade de redes sociais, como o Facebook, Twitter, Google e Microsoft, bem como a Azure Active Directory para a identidade empresarial.

Pode encontrar exemplos para cada caso de utilização no [diretório de exemplos em GitHub].

## <a name="supported-platforms"></a>Plataformas suportadas
O SDK do Mobile Apps Node.js suporta a versão atual do LTS do nó e mais tarde. Atualmente, a versão mais recente do LTS é v4.5.0 de nó. Outras versões de nó poderão funcionar mas não são suportadas.

O SDK do Mobile Apps Node.js suporta dois controladores de base de dados: 

* O controlador de nó mssql suporta SQL Database do Azure e instâncias do SQL Server locais.  
* O controlador de sqlite3 suporta bases de dados do SQLite numa única instância apenas.

### <a name="howto-cmdline-basicapp"></a>Criar um básico Node.js de back-end utilizando a linha de comandos
Cada back-end do Node.js de aplicações móveis é iniciada como uma aplicação ExpressJS. ExpressJS é a arquitetura de serviço de web mais popular, disponível para Node.js. Pode criar um basic [Express] aplicação da seguinte forma:

1. Numa janela do PowerShell ou comando, crie um diretório para o seu projeto:

        mkdir basicapp
2. Executar `npm init` ao inicializar a estrutura do pacote:

        cd basicapp
        npm init

   O `npm init` comando pede-lhe um conjunto de perguntas para inicializar o projeto. Ver o resultado de exemplo:

   ![A saída de init npm][0]
3. Instalar o `express` e `azure-mobile-apps` bibliotecas do repositório npm:

        npm install --save express azure-mobile-apps
4. Crie um ficheiro app.js para implementar o servidor móvel básico:

        var express = require('express'),
            azureMobileApps = require('azure-mobile-apps');

        var app = express(),
            mobile = azureMobileApps();

        // Define a TodoItem table.
        mobile.tables.add('TodoItem');

        // Add the Mobile API so it is accessible as a Web API.
        app.use(mobile);

        // Start listening on HTTP.
        app.listen(process.env.PORT || 3000);

Esta aplicação cria uma API Web do mobile-otimizada com um único ponto final (`/tables/TodoItem`) que fornece acesso não autenticado para um arquivo de dados do SQL Server subjacente utilizando um esquema dinâmico. É adequado para os biblioteca de cliente inícios rápidos a seguir:

* [Guia de introdução do cliente do Android]
* [Início rápido de cliente do Apache Cordova]
* [Guia de introdução do cliente do iOS]
* [Início rápido do cliente de loja Windows]
* [Início rápido de cliente do xamarin. IOS]
* [Início rápido de cliente do xamarin. Android]
* [Início rápido de cliente do xamarin. Forms]

Pode encontrar o código para esta aplicação básica no [basicapp exemplo no GitHub].

### <a name="howto-vs2015-basicapp"></a>Criar um back-end de Node.js utilizando o Visual Studio 2015
Visual Studio 2015 requer uma extensão para desenvolver aplicações Node.js no IDE. Para começar, instale o [Node.js ferramentas 1.1 para o Visual Studio]. Quando concluir a instalação, crie uma aplicação de 4. x rápida:

1. Abra o **novo projeto** caixa de diálogo (de **ficheiro** > **novo** > **projeto**).
2. Expanda **modelos** > **JavaScript** > **Node.js**.
3. Selecione **aplicação básicas do Azure Node.js Express 4**.
4. Preencha o nome do projeto. Selecione **OK**.

   ![Visual Studio 2015 novo projeto][1]
5. Clique com botão direito do **npm** nó e selecione **instalar novos pacotes de npm**.
6. Poderá ter de atualizar o catálogo de npm, depois de criar a sua primeira aplicação Node.js. Selecione **atualizar** se necessário.
7. Introduza **-mobile apps do azure** na caixa de pesquisa. Selecione o **-mobile apps do azure-2.0.0** do pacote e, em seguida, selecione **Instalar pacote**.

   ![Instalar novos pacotes de npm][2]
8. Selecione **fechar**.
9. Abra o ficheiro app.js para adicionar suporte para o SDK de aplicações móveis. Na linha at 6 na parte inferior da biblioteca `require` declarações, adicione o seguinte código:

        var bodyParser = require('body-parser');
        var azureMobileApps = require('azure-mobile-apps');

   Na linha aproximadamente 27 após o outro `app.use` declarações, adicione o seguinte código:

        app.use('/users', users);

        // Mobile Apps initialization
        var mobile = azureMobileApps();
        mobile.tables.add('TodoItem');
        app.use(mobile);

   Guarde o ficheiro.
10. Quer executar a aplicação localmente (a API é fornecida no http://localhost:3000) ou publicar no Azure.

### <a name="create-node-backend-portal"></a>Criar um back-end de Node.js utilizando o portal do Azure
Pode criar um Mobile Apps de back-end à direita no [portal do Azure]. Pode concluir os passos seguintes ou criar um cliente e servidor em conjunto, seguindo o [criar uma aplicação móvel](app-service-mobile-ios-get-started.md) tutorial. O tutorial contém uma versão simplificada destas instruções e é melhor para projetos de prova de conceito.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

Volta a **começar** painel, em **criar uma API de tabela**, escolha **Node.js** como linguagem de back-end.
Selecione a caixa de **reconheço que esta ação substituirá todo o conteúdo do site**e, em seguida, selecione **criar tabela TodoItem**.

### <a name="download-quickstart"></a>Transferir o projeto de código de início rápido de back-end do Node.js utilizando o Git
Quando cria um back-end de aplicações móveis do Node.js utilizando o portal **início rápido** painel, um projeto de Node.js é criada por si e implementada para o seu site. No portal, pode adicionar tabelas e das APIs e editar ficheiros de código de back-end Node.js. Também pode utilizar várias ferramentas de implementação para transferir o projeto de back-end, para que possam adicionar ou modificar as tabelas e das APIs e, em seguida, voltar a publicar o projeto. Para obter mais informações, consulte o [guia de implementação do App Service do Azure]. 

O procedimento seguinte utiliza um repositório de Git para transferir o código de projeto de início rápido:

1. Instale o Git, se ainda não o tiver feito. Os passos necessários para instalar o Git variam entre sistemas operativos. Para as distribuições de específicas do sistema operativo e as diretrizes de instalação, consulte [instalar o Git](http://git-scm.com/book/en/Getting-Started-Installing-Git).
2. Consulte [preparar o seu repositório](../app-service/app-service-deploy-local-git.md#prepare-your-repository) para ativar o repositório de Git para o seu site de back-end. Anote o nome de utilizador de implementação e a palavra-passe.
3. No painel para as suas aplicações móveis back-end, anote o **URL de clone de Git** definição.
4. Executar o `git clone` comando utilizando o URL do clone de Git. Introduza a palavra-passe quando necessário, como no exemplo seguinte:

        $ git clone https://username@todolist.scm.azurewebsites.net:443/todolist.git
5. Procure o diretório local (`/todolist` no exemplo anterior) e tenha em atenção que os ficheiros de projeto foram transferidos. Localize o ficheiro de todoitem.json no `/tables` diretório. Este ficheiro define as permissões na tabela. Também localize o ficheiro de todoitem.js no mesmo diretório. Define os scripts de operação de CRUD da tabela.
6. Depois de efetuar alterações aos ficheiros de projeto, executados os seguintes comandos para adicionar, confirmar e, em seguida, carregue as alterações para o site:

        $ git commit -m "updated the table script"
        $ git push origin master

   Quando adicionar novos ficheiros ao projeto, primeiro tem de executar o `git add .` comando.

O site sejam publicados novamente sempre que um novo conjunto de consolidações é enviado para o site.

### <a name="howto-publish-to-azure"></a>Publicar o seu back-end do Node.js no Azure
O Microsoft Azure oferece vários mecanismos para publicar o Node.js de aplicações móveis back-end para o serviço do Azure. Estes mecanismos incluem as ferramentas de implementação integradas no Visual Studio, as ferramentas de linha de comandos e opções de implementação contínua, com base no controlo de origem. Para obter mais informações, consulte o [guia de implementação do App Service do Azure].

App Service do Azure tem conselhos específico para aplicações de Node.js que deve consultar antes de publicar o back-end:

* Como [especifique a versão de nó]
* Como [utilizar módulos do nó]

### <a name="howto-enable-homepage"></a>Ativar uma página inicial para a sua aplicação
Muitas aplicações são uma combinação de web apps e móveis. Pode utilizar o framework ExpressJS combinar as duas facetas. Por vezes, no entanto, pode querer implementar apenas uma interface móvel. É útil para fornecer uma página inicial para se certificar de que o serviço de aplicações está operacional e em execução. Pode fornecer a sua própria home page ou ativar uma página inicial temporária. Para ativar uma página inicial temporária, utilize o seguinte código ao instanciar Mobile Apps:

    var mobile = azureMobileApps({ homePage: true });

Se apenas pretender que esta opção disponível quando desenvolver localmente, pode adicionar esta definição no ficheiro azureMobile.js.

## <a name="TableOperations"></a>Operações da tabela
O SDK de servidor de Node.js-mobile apps do azure fornece mecanismos para expor as tabelas de dados armazenadas na base de dados do Azure SQL Server como uma API Web. Fornece cinco operações:

| Operação | Descrição |
| --- | --- |
| GET /tables/*tablename* |Obter todos os registos na tabela. |
| GET /tables/*tablename*/:id |Obter um registo específico na tabela. |
| POST /tables/*tablename* |Crie um registo na tabela. |
| PATCH /tables/*tablename*/:id |Atualize um registo na tabela. |
| Eliminar /tables/*tablename*/:id |Elimine um registo na tabela. |

Esta API Web suporta [OData] e expande o esquema de tabela para suportar [sincronização de dados offline].

### <a name="howto-dynamicschema"></a>Definir tabelas utilizando um esquema dinâmico
Antes de poder utilizar uma tabela, tem de defini-lo. Pode definir tabelas utilizando um esquema estático (onde define as colunas no esquema) ou dinamicamente (em que o SDK controla o esquema com base nos pedidos de entrada). Além disso, pode controlar aspetos específicos da Web API adicionando código JavaScript para a definição.

Como melhor prática, deve definir cada tabela num ficheiro JavaScript no `tables` diretório e, em seguida, utilize o `tables.import()` método para importar as tabelas. Expandir o exemplo de aplicação básico, deverá ajustar o ficheiro app.js:

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Define the database schema that is exposed.
    mobile.tables.import('./tables');

    // Provide initialization of any tables that are statically defined.
    mobile.tables.initialize().then(function () {
        // Add the Mobile API so it is accessible as a Web API.
        app.use(mobile);

        // Start listening on HTTP.
        app.listen(process.env.PORT || 3000);
    });

Definir a tabela. / tables/TodoItem.js:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Additional configuration for the table goes here.

    module.exports = table;

As tabelas utilizam um esquema dinâmico por predefinição. Para desativar o esquema dinâmico globalmente, defina o `MS_DynamicSchema` definição de aplicação para false no portal do Azure.

Pode encontrar um exemplo completo no [todo de exemplo no GitHub].

### <a name="howto-staticschema"></a>Definir tabelas utilizando um esquema estático
Pode definir explicitamente as colunas a expor o através da API Web. O SDK de Node.js-mobile apps do azure adiciona automaticamente quaisquer colunas adicionais necessárias para a sincronização de dados offline para a lista que fornecer. Por exemplo, as aplicações de cliente de início rápido necessitam de uma tabela com duas colunas: `text` (uma cadeia) e `complete` (um valor boleano).  
A tabela pode ser definida no ficheiro de JavaScript de definição de tabela (localizado no `tables` diretório) da seguinte forma:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table.
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off the dynamic schema.
    table.dynamicSchema = false;

    module.exports = table;

Se definir tabelas estaticamente, também tem de chamar o `tables.initialize()` método para criar o esquema de base de dados no arranque. O `tables.initialize()` método devolve um [promessa] para que o serviço web não serve pedidos antes de inicializar a base de dados.

### <a name="howto-sqlexpress-setup"></a>Utilizar o SQL Server Express como um arquivo de dados do desenvolvimento no seu computador local
O SDK do Mobile Apps Node.js fornece três opções para servir os dados a Box:

* Utilize o **memória** controladores para fornecer um arquivo de exemplo não persistente.
* Utilize o **mssql** controladores para fornecer um arquivo de dados do SQL Server Express para o desenvolvimento.
* Utilize o **mssql** controladores para fornecer um arquivo de dados SQL Database do Azure de produção.

O SDK do Mobile Apps Node.js utiliza o [mssql Node.js pacote] estabelecer e utilizar uma ligação ao SQL Server Express e a base de dados do SQL Server. Este pacote requer que ative as ligações de TCP a instância do SQL Server Express.

> [!TIP]
> O controlador de memória não fornece um conjunto completo de instalações para fins de teste. Se pretender testar a sua back-end localmente, recomendamos a utilização de um arquivo de dados do SQL Server Express e o controlador de mssql.
>
>

1. Transfira e instale [Microsoft SQL Server 2014 Express]. Certifique-se de que instala o SQL Server 2014 Express com a edição de ferramentas. A menos que explicitamente necessitar de suporte de 64 bits, a versão de 32 bits consome menos memória quando em execução.
2. Execute o Gestor de configuração do SQL Server 2014:

   a. Expanda o **configuração de rede do SQL Server** nó no menu de árvore.

   b. Selecione **protocolos para o SQLEXPRESS**.

   c. Clique com botão direito **TCP/IP** e selecione **ativar**. Selecione **OK** na caixa de diálogo de pop-up.

   d. Clique com botão direito **TCP/IP** e selecione **propriedades**.

   e. Selecione o **endereços IP** separador.

   f. Localizar o **IPAll** nós. No **a porta TCP** campo, introduza **1433**.

      ![Configurar o SQL Server Express para TCP/IP][3]

   g. Selecione **OK**. Selecione **OK** na caixa de diálogo de pop-up.

   h. Selecione **do SQL Server Services** no menu de árvore.

   i. Clique com botão direito **SQL Server (SQLEXPRESS)** e selecione **reiniciar**.

   j. Feche o Gestor de configuração do SQL Server 2014.
3. Executar o SQL Server 2014 Management Studio e ligue à sua instância do SQL Server Express local:

   1. A instância no Object Explorer com o botão direito e selecione **propriedades**.
   2. Selecione o **segurança** página.
   3. Certifique-se de que **modo do SQL Server e a autenticação do Windows** está selecionada.
   4. Selecione **OK**.

      ![Configurar a autenticação do SQL Server Express][4]
   5. Expanda **segurança** > **inícios de sessão** no Object Explorer.
   6. Clique com botão direito **inícios de sessão** e selecione **novo início de sessão**.
   7. Introduza um nome de início de sessão. Selecione **Autenticação do SQL Server**. Introduza uma palavra-passe e, em seguida, introduza a mesma palavra-passe no **Confirmar palavra-passe**. A palavra-passe tem de cumprir requisitos de complexidade do Windows.
   8. Selecione **OK**.

      ![Adicionar um novo utilizador do SQL Server Express][5]
   9. O novo início de sessão com o botão direito e selecione **propriedades**.
   10. Selecione o **funções de servidor** página.
   11. Selecione a caixa de verificação a **dbcreator** função de servidor.
   12. Selecione **OK**.
   13. Feche o SQL Server 2015 Management Studio.

Lembre-se de que registe o nome de utilizador e palavra-passe que selecionou. Poderá ser necessário atribuir funções de servidor adicionais ou permissões, dependendo dos requisitos de base de dados.

Leituras de aplicação Node.js a `SQLCONNSTR_MS_TableConnectionString` variável de ambiente para a cadeia de ligação para esta base de dados. Pode definir esta variável no seu ambiente. Por exemplo, pode utilizar o PowerShell para definir esta variável de ambiente:

    $env:SQLCONNSTR_MS_TableConnectionString = "Server=127.0.0.1; Database=mytestdatabase; User Id=azuremobile; Password=T3stPa55word;"

Acesso a base de dados através de uma ligação de TCP/IP. Forneça um nome de utilizador e palavra-passe para a ligação.

### <a name="howto-config-localdev"></a>Configurar o seu projeto de desenvolvimento local
As Mobile Apps lê um ficheiro de JavaScript chamado *azureMobile.js* do sistema de ficheiros local. Utilize este ficheiro para configurar o SDK de aplicações móveis na produção. Em alternativa, utilize **as definições de aplicação** no [portal do Azure]. 

O ficheiro azureMobile.js deve exportar de um objeto de configuração. As definições mais comuns são:

* Definições de base de dados
* Definições de registo de diagnóstico
* Definições CORS da alternativas

Este exemplo de ficheiro azureMobile.js implementa as definições de base de dados anterior:

    module.exports = {
        cors: {
            origins: [ 'localhost' ]
        },
        data: {
            provider: 'mssql',
            server: '127.0.0.1',
            database: 'mytestdatabase',
            user: 'azuremobile',
            password: 'T3stPa55word'
        },
        logging: {
            level: 'verbose'
        }
    };

Recomendamos que adicione azureMobile.js ao seu ficheiro. gitignore (ou outro controlo de código fonte ignorar ficheiro) para impedir que as palavras-passe a ser armazenados na nuvem. Configure sempre as definições de produção no **as definições de aplicação** dentro de [portal do Azure].

### <a name="howto-appsettings"></a>Configurar definições da aplicação para a sua aplicação móvel
A maioria das definições no ficheiro azureMobile.js têm uma definição de aplicação equivalente [portal do Azure]. Utilize a lista seguinte para configurar a sua aplicação no **as definições de aplicação**:

| Definição de aplicação | definição de azureMobile.js | Descrição | Valores válidos |
|:--- |:--- |:--- |:--- |
| **MS_MobileAppName** |nome |Nome da aplicação |string |
| **MS_MobileLoggingLevel** |Logging.level |Nível de registo mínimo de mensagens em fila para iniciar sessão |erro, aviso, informações, verbosa, depuração, silly |
| **MS_DebugMode** |depurar |Ativa ou desativa o modo de depuração |TRUE, false |
| **MS_TableSchema** |data.schema |Nome de esquema predefinido para tabelas SQL |cadeia (predefinição: dbo) |
| **MS_DynamicSchema** |data.dynamicSchema |Ativa ou desativa o modo de depuração |TRUE, false |
| **MS_DisableVersionHeader** |versão (defina a indefinido) |Desativa o cabeçalho X-ZUMO--versão do servidor |TRUE, false |
| **MS_SkipVersionCheck** |skipversioncheck |Desativa a verificação da versão do cliente de API |TRUE, false |

Para definir uma definição de aplicação:

1. Inicie sessão no [portal do Azure].
2. Selecione **todos os recursos** ou **serviços aplicacionais**e, em seguida, selecione o nome da sua aplicação móvel.
3. O **definições** painel abre-se por predefinição. Se não, selecionar **definições**.
4. No **geral** menu, selecione **definições da aplicação**.
5. Desloque-se para o **as definições de aplicação** secção.
6. Se a aplicação definir já existir, selecione o valor da definição de aplicação para editar o valor.
   Se a definição de aplicação não existe, introduza a definição de aplicação no **chave** caixa e o valor de **valor** caixa.
8. Selecione **Guardar**.

Alterar a maioria das definições de aplicação requer um reinício do serviço.

### <a name="howto-use-sqlazure"></a>Armazenar a base de dados de SQL de utilização como os dados de produção
<!--- ALTERNATE INCLUDE - we can't use ../includes/app-service-mobile-dotnet-backend-create-new-service.md - slightly different semantics -->

Utilizar a SQL Database do Azure como um arquivo de dados é idêntico em todos os tipos de aplicações do App Service do Azure. Se não o fez, já, siga estes passos para criar um back-end de Mobile Apps:

1. Inicie sessão no [portal do Azure].
2. No canto superior esquerdo da janela, selecione o **+ novo** botão > **Web + móvel** > **aplicação móvel**e, em seguida, forneça um nome para o seu Mobile Apps back-end.
3. No **grupo de recursos** box, introduza o mesmo nome que a sua aplicação.
4. A predefinição é selecionado o plano de serviço de aplicações. Se pretender alterar o seu plano de serviço de aplicações:

   a. Selecione **plano do App Service** > **+ criar novos**. 
   
   b. Forneça um nome do novo plano de serviço de aplicações e selecione uma localização adequada. 
   
   c. Selecione um escalão de preço adequado para o serviço. Selecione **ver todos os** para ver mais preços opções, tais como **livres** e **partilhados**. 
   
   d. Clique em de **selecione** botão. 
   
   e. Volta a **plano do App Service** painel, selecione **OK**.
5. Selecione **Criar**. 

Aprovisionamento de aplicações móveis de um back-end pode demorar alguns minutos. Depois de efetuar uma cópia de Mobile Apps final é aprovisionado, o portal abre o **definições** painel para o back-end do Mobile Apps.

Pode optar por ligar uma base de dados existente do SQL Server ao seu back-end do Mobile Apps ou criar uma nova base de dados do SQL Server. Nesta secção, iremos criar uma base de dados do SQL Server.

> [!NOTE]
> Se já tiver uma base de dados na mesma localização como Mobile Apps back-end, em vez disso, pode selecionar **utilizar uma base de dados existente** e, em seguida, selecione a base de dados. Não recomendamos a utilização de uma base de dados numa localização diferente devido a latências.
>
>

1. No novo Mobile Apps back-end, selecione **definições** > **aplicação móvel** > **dados** > **+ adicionar**.
2. No **adicionar ligação de dados** painel, selecione **base de dados SQL - configurar as definições necessárias** > **criar uma nova base de dados**. Introduza o nome da nova base de dados a **nome** caixa.
3. Selecione **servidor**. No **novo servidor** painel, introduza um nome de servidor exclusivo no **nome do servidor** caixa e forneça um servidor adequado, início de sessão de administrador e a palavra-passe. Certifique-se de que **permitir serviços do azure ao servidor de acesso** está selecionada. Selecione **OK**.

   ![Criar uma SQL Database do Azure][6]
4. No **nova base de dados** painel, selecione **OK**.
5. Volta a **adicionar ligação de dados** painel, selecione **cadeia de ligação**e introduza o início de sessão e palavra-passe que forneceu quando criou a base de dados. Se utilizar uma base de dados existente, forneça as credenciais de início de sessão para essa base de dados. Selecione **OK**.
6. Volta a **adicionar ligação de dados** painel novamente, selecione **OK** para criar a base de dados.

<!--- END OF ALTERNATE INCLUDE -->

A criação da base de dados pode demorar alguns minutos. Utilize o **notificações** área para monitorizar o progresso da implementação. Não avança até que a base de dados é implementado com êxito. Depois da base de dados é implementado, é criada uma cadeia de ligação para a instância de base de dados do SQL Server nas definições de aplicação de back-end do Mobile Apps. Pode ver esta definição de aplicação na **definições** > **definições da aplicação** > **cadeias de ligação**.

### <a name="howto-tables-auth"></a>Exigir autenticação para aceder a tabelas
Se pretender utilizar a autenticação do serviço de aplicações com o `tables` ponto final, tem de configurar a autenticação do serviço de aplicações no [portal do Azure] primeiro. Para obter mais informações, consulte o guia de configuração para o fornecedor de identidade que pretende utilizar:

* [Configurar a autenticação do Azure Active Directory]
* [Configurar a autenticação do Facebook]
* [Configurar a autenticação do Google]
* [Configurar a autenticação da Microsoft]
* [Configurar a autenticação do Twitter]

Cada tabela tem uma propriedade de acesso que pode utilizar para controlar o acesso à tabela. O exemplo seguinte mostra uma tabela estaticamente definida com a autenticação necessária.

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table.
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off the dynamic schema.
    table.dynamicSchema = false;

    // Require authentication to access the table.
    table.access = 'authenticated';

    module.exports = table;

A propriedade de acesso, pode efetuar um dos três valores:

* *anónimo* indica que a aplicação de cliente tem permissão para ler os dados sem autenticação.
* *autenticado* indica que a aplicação cliente terá de enviar um token de autenticação válido com o pedido.
* *desativado* indica que esta tabela está atualmente desativada.

Se a propriedade de acesso não está definida, é permitido o acesso não autenticado.

### <a name="howto-tables-getidentity"></a>Utilizar afirmações de autenticação com as tabelas
Pode configurar várias afirmações que são pedidas quando configurar a autenticação. Estas afirmações não estão normalmente disponíveis através do `context.user` objeto. No entanto, pode obtê-los utilizando o `context.user.getIdentity()` método. O `getIdentity()` método devolve uma promessa que é resolvido para um objeto. O objeto é codificado pelo método de autenticação (`facebook`, `google`, `twitter`, `microsoftaccount`, ou `aad`).

Por exemplo, se configurar a autenticação da conta Microsoft e os endereços de e-mail de afirmação de pedido, pode adicionar o endereço de e-mail para o registo com o controlador da tabela seguinte:

    var azureMobileApps = require('azure-mobile-apps');

    // Create a new table definition.
    var table = azureMobileApps.table();

    table.columns = {
        "emailAddress": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;
    table.access = 'authenticated';

    /**
    * Limit the context query to those records with the authenticated user email address
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function queryContextForEmail(context) {
        return context.user.getIdentity().then((data) => {
            context.query.where({ emailAddress: data.microsoftaccount.claims.emailaddress });
            return context.execute();
        });
    }

    /**
    * Adds the email address from the claims to the context item - used for
    * insert operations
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function addEmailToContext(context) {
        return context.user.getIdentity().then((data) => {
            context.item.emailAddress = data.microsoftaccount.claims.emailaddress;
            return context.execute();
        });
    }

    // Configure specific code when the client does a request.
    // READ: only return records that belong to the authenticated user.
    table.read(queryContextForEmail);

    // CREATE: add or overwrite the userId based on the authenticated user.
    table.insert(addEmailToContext);

    // UPDATE: only allow updating of records that belong to the authenticated user.
    table.update(queryContextForEmail);

    // DELETE: only allow deletion of records that belong to the authenticated user.
    table.delete(queryContextForEmail);

    module.exports = table;

Para ver quais as afirmações que estão disponíveis, utilize um web browser para ver o `/.auth/me` ponto final do seu site.

### <a name="howto-tables-disabled"></a>Desativar o acesso às operações de tabela específica
Para além de apresentação na tabela, a propriedade de acesso pode ser utilizada para controlar operações individuais. Existem quatro operações:

* `read` é a operação obter RESTful na tabela.
* `insert` é a operação RESTful POST na tabela.
* `update` é a operação de aplicar o PATCH RESTful na tabela.
* `delete` é a operação eliminar RESTful na tabela.

Por exemplo, poderá pretender fornecer uma tabela só de leitura não autenticada:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Read-only table. Only allow READ operations.
    table.read.access = 'anonymous';
    table.insert.access = 'disabled';
    table.update.access = 'disabled';
    table.delete.access = 'disabled';

    module.exports = table;

### <a name="howto-tables-query"></a>Ajustar a consulta que é utilizada com operações de tabela
Um requisito para operações de tabela comum consiste em fornecer uma vista restrita dos dados. Por exemplo, pode fornecer uma tabela que está marcada com o ID de utilizador autenticado que só pode ler ou atualizar os seus próprios registos. A definição da tabela seguinte fornece esta funcionalidade:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define a static schema for the table.
    table.columns = {
        "userId": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;

    // Require authentication for this table.
    table.access = 'authenticated';

    // Ensure that only records for the authenticated user are retrieved.
    table.read(function (context) {
        context.query.where({ userId: context.user.id });
        return context.execute();
    });

    // When adding records, add or overwrite the userId with the authenticated user.
    table.insert(function (context) {
        context.item.userId = context.user.id;
        return context.execute();
    });

    module.exports = table;

As operações que normalmente executar uma consulta de tem uma propriedade de consulta que pode ajustar utilizando um `where` cláusula. A propriedade de consulta é um [QueryJS] pode processar o objeto que é utilizado para converter uma consulta de OData para algo que os dados de back-end. Para cenários de igualdade simples (como a anterior), pode utilizar um mapa. Também pode adicionar cláusulas específicas do SQL Server:

    context.query.where('myfield eq ?', 'value');

### <a name="howto-tables-softdelete"></a>Configurar a eliminação de forma recuperável numa tabela
Eliminação de forma recuperável, na verdade, não a eliminar os registos. Em vez disso, marca-los como eliminados na base de dados ao definir a coluna eliminada como true. O SDK de aplicações móveis remove automaticamente registos eliminados de forma recuperável resultados, a menos que o SDK do cliente de Mobile utiliza `IncludeDeleted()`. Para configurar uma tabela para eliminação de forma recuperável, defina o `softDelete` propriedade no ficheiro de definição de tabela:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table.
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off the dynamic schema.
    table.dynamicSchema = false;

    // Turn on soft delete.
    table.softDelete = true;

    // Require authentication to access the table.
    table.access = 'authenticated';

    module.exports = table;

Deve estabelecer um mecanismo para eliminar registos: uma aplicação de cliente, um WebJob, uma função do Azure ou uma API personalizada.

### <a name="howto-tables-seeding"></a>A base de dados com dados de seed
Quando estiver a criar uma nova aplicação, poderá querer efetuar o seeding uma tabela com dados. Pode fazê-dentro do ficheiro de JavaScript de definição de tabela da seguinte forma:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table.
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };
    table.seed = [
        { text: 'Example 1', complete: false },
        { text: 'Example 2', complete: true }
    ];

    // Turn off the dynamic schema.
    table.dynamicSchema = false;

    // Require authentication to access the table.
    table.access = 'authenticated';

    module.exports = table;

O seeding dos dados ocorre apenas quando utilizou o SDK de aplicações móveis para criar a tabela. Se a tabela já existir na base de dados, não existem dados é inseridos na tabela. Se o esquema dinâmico estiver ativado, o esquema é inferido a partir dos dados seeded.

Recomendamos que pode chama explicitamente o `tables.initialize()` método para criar a tabela quando inicia o serviço em execução.

### <a name="Swagger"></a>Ativar o suporte de Swagger
As Mobile Apps vem com incorporado [Swagger] suportar. Para ativar o suporte do Swagger, instale primeiro o IU do swagger como uma dependência:

    npm install --save swagger-ui

Em seguida, pode ativar o suporte de Swagger no construtor Mobile Apps:

    var mobile = azureMobileApps({ swagger: true });

É provavelmente apenas pretende ativar o suporte de Swagger nas edições de desenvolvimento. Pode fazê-lo utilizando o `NODE_ENV` definição de aplicação:

    var mobile = azureMobileApps({ swagger: process.env.NODE_ENV !== 'production' });

O `swagger` ponto final está localizado em http://*yoursite*.azurewebsites.net/swagger. Pode aceder a IU do Swagger através de `/swagger/ui` ponto final. Se optar por exigir a autenticação na sua aplicação completa, Swagger produz um erro. Para obter os melhores resultados, optar por permitir pedidos não autenticados nas definições de autenticação/autorização do serviço de aplicações do Azure e, em seguida, controlar autenticação utilizando o `table.access` propriedade.

Também pode adicionar a opção de Swagger azureMobile.js no ficheiro, se pretender apenas suporte de Swagger para desenvolver localmente.

## <a name="a-namepushpush-notifications"></a><a name="push">Notificações Push
As Mobile Apps é integrado com Notification Hubs do Azure pode enviar notificações push direcionadas para milhões de dispositivos em todas as plataformas principais. Ao utilizar os Notification Hubs, pode enviar notificações push para dispositivos iOS, Android e Windows dispositivos. Para saber mais sobre tudo o que pode fazer com os Notification Hubs, consulte o [descrição geral dos Notification Hubs](../notification-hubs/notification-hubs-push-notification-overview.md).

### </a><a name="send-push"></a>Enviar notificações push
O código seguinte mostra como utilizar o `push` objeto para enviar uma notificação push de difusão para dispositivos iOS:

    // Create an APNS payload.
    var payload = '{"aps": {"alert": "This is an APNS payload."}}';

    // Only do the push if configured.
    if (context.push) {
        // Send a push notification by using APNS.
        context.push.apns.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

Ao criar um registo de push de modelo do cliente, em vez disso, pode enviar uma mensagem de push de modelo para dispositivos em todas as plataformas suportadas. O código seguinte mostra como enviar uma notificação de modelo:

    // Define the template payload.
    var payload = '{"messageParam": "This is a template payload."}';

    // Only do the push if configured.
    if (context.push) {
        // Send a template notification.
        context.push.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }


### <a name="push-user"></a>Enviar notificações push para um utilizador autenticado utilizando etiquetas
Quando um utilizador autenticado regista para notificações push, uma tag de ID de utilizador é adicionada automaticamente para o registo. Ao utilizar esta etiqueta, pode enviar notificações push para todos os dispositivos registados por um utilizador específico. O código seguinte obtém o SID do utilizador que está a efetuar o pedido e envia uma notificação push de modelo para cada registo de dispositivos para que o utilizador:

    // Only do the push if configured.
    if (context.push) {
        // Send a notification to the current user.
        context.push.send(context.user.id, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

Quando estiver a registar para notificações push a partir de um cliente autenticado, certifique-se de que a autenticação é concluída antes de tentar registo.

## <a name="CustomAPI"></a> APIs personalizadas
### <a name="howto-customapi-basic"></a>Definir uma API personalizada
Para além da API de acesso de dados através de `/tables` ponto final, Mobile Apps pode fornecer cobertura de API personalizada. Personalizada APIs são definidos de forma semelhante para as definições de tabela o acesso e as mesmas instalações, incluindo a autenticação.

Se pretender utilizar a autenticação do serviço de aplicação com uma API personalizada, tem de configurar autenticação do serviço de aplicações no [portal do Azure] primeiro. Para obter mais informações, consulte o guia de configuração para o fornecedor de identidade que pretende utilizar:

* [Configurar a autenticação do Azure Active Directory]
* [Configurar a autenticação do Facebook]
* [Configurar a autenticação do Google]
* [Configurar a autenticação da Microsoft]
* [Configurar a autenticação do Twitter]

APIs personalizadas definidas muito da mesma forma como a API de tabelas:

1. Criar um `api` diretório.
2. Criar um ficheiro de JavaScript de definição de API no `api` diretório.
3. Utilize o método de importação para importar o `api` diretório.

Eis o protótipo definição da API de amostra de aplicação básico que utilizámos anteriormente:

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Import the custom API.
    mobile.api.import('./api');

    // Add the Mobile API so it is accessible as a Web API.
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

Vejamos um exemplo de API que devolve a data de servidor utilizando o `Date.now()` método. Segue-se o ficheiro api/date.js:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };

    module.exports = api;

Cada parâmetro é um dos verbos padrão RESTful: obter, POST, PATCH ou eliminar. O método é uma norma [ExpressJS middleware] função que envia a saída obrigatória.

### <a name="howto-customapi-auth"></a>Exigir autenticação para aceder a uma API personalizada
O SDK de aplicações móveis implementa autenticação da mesma forma para ambos os `tables` ponto final e APIs personalizadas. Para adicionar autenticação à API desenvolvida na secção anterior, adicione um `access` propriedade:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };
    // All methods must be authenticated.
    api.access = 'authenticated';

    module.exports = api;

Também pode especificar a autenticação em operações específicas:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        }
    };
    // The GET methods must be authenticated.
    api.get.access = 'authenticated';

    module.exports = api;

O mesmo token que é utilizado para o `tables` ponto final tem de ser utilizada para APIs personalizadas que exija autenticação.

### <a name="howto-customapi-auth"></a>Processar os carregamentos de ficheiros grandes
Utiliza o SDK de aplicações móveis a [middleware de corpo parser](https://github.com/expressjs/body-parser) para aceitar e descodificar o conteúdo do corpo na sua submissão. Pode pré-configurar o parser de corpo para aceitar carregamentos de ficheiros maiores:

    var express = require('express'),
        bodyParser = require('body-parser'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Set up large body content handling.
    app.use(bodyParser.json({ limit: '50mb' }));
    app.use(bodyParser.urlencoded({ limit: '50mb', extended: true }));

    // Import the custom API.
    mobile.api.import('./api');

    // Add the Mobile API so it is accessible as a Web API.
    app.use(mobile);

    // Start listening on HTTP.
    app.listen(process.env.PORT || 3000);

O ficheiro é codificado antes da transmissão de base-64. Esta codificação aumenta o tamanho do carregamento real (e o tamanho que tem em conta).

### <a name="howto-customapi-sql"></a>Executar instruções SQL personalizadas
O SDK de aplicações móveis permite o acesso ao contexto de todo, através do objeto pedido. Pode executar facilmente instruções parametrizadas do SQL Server para o fornecedor de dados definido:

    var api = {
        get: function (request, response, next) {
            // Check for parameters. If not there, pass on to a later API call.
            if (typeof request.params.completed === 'undefined')
                return next();

            // Define the query. Anything that the mssql
            // driver can handle is allowed.
            var query = {
                sql: 'UPDATE TodoItem SET complete=@completed',
                parameters: [{
                    completed: request.params.completed
                }]
            };

            // Execute the query. The context for Mobile Apps is available through
            // request.azureMobile. The data object contains the configured data provider.
            request.azureMobile.data.execute(query)
            .then(function (results) {
                response.json(results);
            });
        }
    };

    api.get.access = 'authenticated';
    module.exports = api;

## <a name="Debugging"></a>Depuração, tabelas simples e fácil APIs
### <a name="howto-diagnostic-logs"></a>Depurar, diagnosticar e resolver problemas de aplicações móveis
App Service do Azure fornece várias depuração e resolução de problemas de técnicas para as aplicações Node.js.
Para começar a utilizar na sua back-end do Node.js Mobile Apps de resolução de problemas, consulte os artigos seguintes:

* [Monitorização do serviço de aplicações do Azure]
* [Ativar o registo de diagnóstico no App Service do Azure]
* [Resolver problemas do App Service do Azure no Visual Studio]

Aplicações node.js têm acesso a uma vasta gama de ferramentas de registo de diagnóstico. Internamente, utiliza o SDK do Mobile Apps Node.js [Winston] para o registo de diagnóstico. Registo é ativado automaticamente ao ativar a depuração conjunto ou de modo a `MS_DebugMode` definição de aplicação como verdadeiro no [portal do Azure]. Registos gerados aparecem nos registos de diagnóstico no [portal do Azure].

### <a name="in-portal-editing"></a><a name="work-easy-tables"></a>Trabalhar com tabelas fácil no portal do Azure
Pode utilizar tabelas fácil para criar e trabalhar com o direito de tabelas no portal. Pode carregar o conjunto de dados para tabelas fácil num formato CSV. Tenha em atenção que não é possível utilizar nomes de propriedade (no seu conjunto de dados CSV) que estão em conflito com nomes de propriedade de sistema de back-end das Mobile Apps. Os nomes de propriedade de sistema são:
* createdAt
* updatedAt
* eliminado
* versão

Pode editar mesmo operações da tabela, utilizando o Editor de serviço de aplicações. Quando seleciona **tabelas fácil** nas definições do site de back-end, pode adicionar, modificar ou eliminar uma tabela. Também pode ver os dados da tabela.

![Trabalhar com tabelas fácil](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-tables.png)

Os comandos seguintes estão disponíveis na barra de comandos para uma tabela:

* **Alterar permissões**: modificar a permissão de leitura, inserir, atualizar e eliminar operações na tabela.
 As opções são permitir o acesso anónimo, para exigir autenticação ou desativar a todos os acessos à operação.
* **Editar o script**: O ficheiro de script para a tabela é aberto no Editor de serviço de aplicações.
* **Gerir o esquema**: Adicionar ou eliminar colunas ou alterar o índice de tabela.
* **Tabela limpar**: truncar uma tabela existente, eliminando todas as linhas de dados, mas deixar o esquema inalterado.
* **Eliminar linhas**: eliminar linhas individuais dos dados.
* **Ver registos de transmissão em fluxo**: ligar ao serviço de registo de transmissão em fluxo para o seu site.

### <a name="work-easy-apis"></a>Trabalhar com APIs fácil no portal do Azure
Pode utilizar APIs simples para criar e trabalhar com o direito APIs personalizado no portal. Pode editar os scripts de API utilizando o Editor de serviço de aplicações.

Quando seleciona **APIs fácil** nas definições do site de back-end, pode adicionar, modificar ou eliminar um ponto final de API personalizado.

![Trabalhar com APIs fácil](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-apis.png)

No portal, pode alterar as permissões de acesso de uma ação de HTTP, edite o ficheiro de script de API no App Service Editor ou consulte os registos de transmissão em fluxo.

### <a name="online-editor"></a>Editar o código no Editor de serviço de aplicações
Ao utilizar o portal do Azure, pode editar os ficheiros de script de back-end do Node.js no App Service Editor sem ter de transferir o projeto para o seu computador local. Para editar ficheiros de script no online editor:

1. No painel para as suas aplicações móveis back-end, selecione **todas as definições** > ou **tabelas fácil** ou **APIs fácil**. Selecione uma tabela ou a API e, em seguida, selecione **Editar script**. No Editor de serviço de aplicação para abrir o ficheiro de script.

   ![Editor do Serviço de Aplicação](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-visual-studio-editor.png)
2. Efetue as alterações para o ficheiro de código no online editor. As alterações são guardadas automaticamente à medida que escreve.

<!-- Images -->
[0]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/npm-init.png
[1]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-new-project.png
[2]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-install-npm.png
[3]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-config.png
[4]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-authconfig.png
[5]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-newuser-1.png
[6]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/dotnet-backend-create-db.png

<!-- URLs -->
[Guia de introdução do cliente do Android]: app-service-mobile-android-get-started.md
[Guia de introdução do cliente do Apache Cordova]: app-service-mobile-cordova-get-started.md
[Guia de introdução do cliente do iOS]: app-service-mobile-ios-get-started.md
[Guia de introdução do cliente do xamarin. IOS]: app-service-mobile-xamarin-ios-get-started.md
[Guia de introdução do cliente do xamarin. Android]: app-service-mobile-xamarin-android-get-started.md
[Guia de introdução do cliente do xamarin. Forms]: app-service-mobile-xamarin-forms-get-started.md
[Guia de introdução do cliente de loja do Windows]: app-service-mobile-windows-store-dotnet-get-started.md
[sincronização de dados offline]: app-service-mobile-offline-data-sync.md
[Configurar a autenticação do Azure Active Directory]: ../app-service/app-service-mobile-how-to-configure-active-directory-authentication.md
[Configurar a autenticação do Facebook]: ../app-service/app-service-mobile-how-to-configure-facebook-authentication.md
[Configurar a autenticação do Google]: ../app-service/app-service-mobile-how-to-configure-google-authentication.md
[Configurar a autenticação da Microsoft]: ../app-service/app-service-mobile-how-to-configure-microsoft-authentication.md
[Configurar a autenticação do Twitter]: ../app-service/app-service-mobile-how-to-configure-twitter-authentication.md
[guia de implementação do App Service do Azure]: ../app-service/app-service-deploy-local-git.md
[Monitorização do serviço de aplicações do Azure]: ../app-service/web-sites-monitor.md
[Ativar o registo de diagnóstico no App Service do Azure]: ../app-service/web-sites-enable-diagnostic-log.md
[Resolver problemas do App Service do Azure no Visual Studio]: ../app-service/web-sites-dotnet-troubleshoot-visual-studio.md
[especifique a versão de nó]: ../nodejs-specify-node-version-azure-apps.md
[utilizar módulos do nó]: ../nodejs-use-node-modules-azure-apps.md
[Create a new Azure App Service]: ../app-service/
[azure-mobile-apps]: https://www.npmjs.com/package/azure-mobile-apps
[Express]: http://expressjs.com/
[Swagger]: http://swagger.io/

[portal do Azure]: https://portal.azure.com/
[OData]: http://www.odata.org
[Promessa]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp exemplo no GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[todo de exemplo no GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[diretório de exemplos em GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js ferramentas 1.1 para o Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js pacote]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS middleware]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
