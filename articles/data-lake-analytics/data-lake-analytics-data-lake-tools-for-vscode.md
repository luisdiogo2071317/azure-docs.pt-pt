---
title: Utilize as ferramentas do Azure Data Lake para Visual Studio Code
description: Saiba como utilizar o Azure Data Lake Tools para Visual Studio Code para criar, testar e executar scripts U-SQL.
services: data-lake-analytics
author: Jejiang
ms.author: jejiang
manager: kfile
editor: jasonwhowell
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.topic: conceptual
ms.date: 02/09/2018
ms.openlocfilehash: 79cd1a04c99891e5146ad20cfd36b8bd4fe4d893
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35261489"
---
# <a name="use-azure-data-lake-tools-for-visual-studio-code"></a>Utilize as ferramentas do Azure Data Lake para Visual Studio Code

Neste artigo, saiba como pode utilizar as ferramentas do Azure Data Lake para Visual Studio Code (VS Code) para criar, testar e executar scripts U-SQL. As informações também são abrangidas no vídeo seguinte:

<a href="https://channel9.msdn.com/Series/AzureDataLake/Azure-Data-Lake-Tools-for-VSCode?term=ADL%20Tools%20for%20VSCode"><img src="./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-video.png"></a>

## <a name="prerequisites"></a>Pré-requisitos

Ferramentas do Azure Data Lake para o VS Code suporta o Windows, Linux e MacOS.  

- [Visual Studio Code](https://www.visualstudio.com/products/code-vs.aspx)

Para MacOS e Linux:
- [.NET core SDK 2.0](https://www.microsoft.com/net/download/core)
- [Mono 5.2.x](http://www.mono-project.com/download/)

## <a name="install-azure-data-lake-tools"></a>Instalar as ferramentas do Azure Data Lake

Depois de instalar os pré-requisitos, pode instalar as ferramentas do Azure Data Lake para o VS Code.

**Para instalar as ferramentas do Azure Data Lake**

1. Abra o Visual Studio Code.
2. Selecione **extensões** no painel esquerdo. Introduza **ferramentas do Azure Data Lake** na caixa de pesquisa.
3. Selecione **instalar** junto a **ferramentas do Azure Data Lake**. 

   ![Selecione as opções para instalar as ferramentas do Data Lake](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extensions.png)

   Após alguns segundos, o **instalar** botão muda para **recarregar**.
4. Selecione **recarregar** para ativar o **ferramentas do Azure Data Lake** extensão.
5. Selecione **recarregar janela** para confirmar. Pode ver **ferramentas do Azure Data Lake** no **extensões** painel.

 
## <a name="activate-azure-data-lake-tools"></a>Ativar as ferramentas do Azure Data Lake
Criar um ficheiro de .usql ou abra um ficheiro .usql existente para ativar a extensão. 


## <a name="work-with-u-sql"></a>Trabalhar com o U-SQL

Para trabalhar com o U-SQL, tem de abrir um ficheiro de U-SQL ou uma pasta.

**Para abrir o script de exemplo**

Abra a paleta de comando (Ctrl + Shift + P) e introduza **ADL: Abra o Script de exemplo**. Abrir outra instância deste exemplo. Também pode editar, configurar e submeter um script nesta instância.

**Para abrir uma pasta para o seu projeto U-SQL**

1. A partir do Visual Studio Code, selecione o **ficheiro** e, em seguida, selecione **Abrir pasta**.
2. Especifique uma pasta e, em seguida, selecione **selecionar pasta**.
3. Selecione o **ficheiro** e, em seguida, selecione **novo**. Um ficheiro de Untitled-1 é adicionado ao projeto.
4. Introduza o seguinte código no ficheiro Untitled-1:

        @departments  = 
            SELECT * FROM 
                (VALUES
                    (31,    "Sales"),
                    (33,    "Engineering"), 
                    (34,    "Clerical"),
                    (35,    "Marketing")
                ) AS 
                      D( DepID, DepName );
         
        OUTPUT @departments
            TO "/Output/departments.csv"
        USING Outputters.Csv();

    O script cria um ficheiro de departments.csv com alguns dados incluídos na pasta /output.

5. Guarde o ficheiro como **myUSQL.usql** na pasta aberta.

**Para compilar um script U-SQL**

1. Selecione Ctrl + Shift + P para abrir a paleta de comando. 
2. Introduza **ADL: compilar o Script**. Os resultados de compilação são apresentados no **saída** janela. Pode também com o botão direito um ficheiro de script e, em seguida, selecione **ADL: compilar o Script** para compilar uma tarefa de U-SQL. O resultado da compilação é apresentado no **saída** painel.
 
**Para submeter um script U-SQL**

1. Selecione Ctrl + Shift + P para abrir a paleta de comando. 
2. Introduza **ADL: submeter a tarefa**. Pode também com o botão direito um ficheiro de script e, em seguida, selecione **ADL: Submeter tarefa**. 

Depois de submeter uma tarefa de U-SQL, os registos de submissão apresentado o **saída** janela no VS Code. A vista de tarefa é apresentado no painel direito. Se a submissão for bem sucedida, o URL de tarefa é demasiado apresentada. Pode abrir o URL de tarefa num web browser para controlar o estado da tarefa em tempo real. 

Na vista de tarefa **resumo** separador, pode ver os detalhes da tarefa. As funções principais incluem um script de submeter de novo, duplicar um script e abra no portal. Na vista de tarefa **dados** separador, pode consultar os ficheiros de entrada, ficheiros de saída e ficheiros de recursos. Os ficheiros podem ser transferidos para o computador local.

![Separador Resumo na vista de tarefa](./media/data-lake-analytics-data-lake-tools-for-vscode/job-view-summary.png)

![Separador de dados na vista de tarefa](./media/data-lake-analytics-data-lake-tools-for-vscode/job-view-data.png)

**Para definir o contexto predefinido**

Pode definir o contexto predefinido para aplicar esta definição para todos os ficheiros de script, se não tiver definido parâmetros para ficheiros individualmente.

1. Selecione Ctrl + Shift + P para abrir a paleta de comando. 
2. Introduza **ADL: definir o contexto predefinido**. Ou o editor de scripts com o botão direito e selecione **ADL: Definir contexto predefinido**.
3. Escolha a conta, a base de dados e o esquema que pretende. A definição é guardada no ficheiro de configuração xxx_settings.json.

   ![Conta, a base de dados e o esquema definido como o contexto predefinido](./media/data-lake-analytics-data-lake-tools-for-vscode/default-context-sequence.png)

**Para definir os parâmetros do script**

1. Selecione Ctrl + Shift + P para abrir a paleta de comando. 
2. Introduza **ADL: definir os parâmetros do Script**.
3. Abrir o ficheiro de xxx_settings.json com as seguintes propriedades:

   - **conta**: conta de um Azure Data Lake Analytics na sua subscrição do Azure que precisa para compilar e executar tarefas U-SQL. Tem de configurar a conta de computador antes de compilar e executar tarefas U-SQL.
   - **base de dados**: uma base de dados na sua conta. A predefinição é **mestre**.
   - **esquema**: um esquema de base de dados. A predefinição é **dbo**.
   - **optionalSettings**:
        - **prioridade**: O intervalo de prioridade é de 1 a 1000, 1, pois a prioridade mais elevada. O valor predefinido é **1000**.
        - **degreeOfParallelism**: O intervalo de paralelismo é entre 1 e 150. O valor predefinido é o paralelismo máximo permitido na sua conta do Azure Data Lake Analytics. 

   ![Conteúdo do ficheiro JSON](./media/data-lake-analytics-data-lake-tools-for-vscode/default-context-setting.png)
      
> [!NOTE] 
> Depois de guardar a configuração, a conta, base de dados e as informações de esquema são apresentados na barra de estado no canto inferior esquerdo do ficheiro .usql correspondente se não tiver um contexto predefinido que configurar.

**Para definir o Git ignorar**

1. Selecione Ctrl + Shift + P para abrir a paleta de comando. 
2. Introduza **ADL: Git conjunto ignorar**.

   - Se não tiver um **. gitignore** ficheiros na sua pasta de trabalho VS Code, um ficheiro denominado **. gitignore** é criado na pasta. Itens de quatro (**usqlCodeBehindReference**, **usqlCodeBehindGenerated**, **.cache**, **obj**) são adicionadas no ficheiro, por predefinição. Pode efetuar as atualizações mais se for necessário.
   - Se já tiver um **. gitignore** ficheiros na sua pasta de trabalho VS Code, a ferramenta adiciona quatro itens (**usqlCodeBehindReference**, **usqlCodeBehindGenerated**, **.cache**, **obj**) na sua **. gitignore** ficheiro se os quatro itens não foram incluídos no ficheiro.

   ![Itens no ficheiro. gitignore](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-gitignore.png)


## <a name="work-with-code-behind-files-c-sharp-python-and-r"></a>Trabalhar com ficheiros code-behind: C Sharp, Python e R

Ferramentas do Data Lake do Azure suporta vários códigos personalizados. Para obter instruções, consulte [desenvolver U-SQL com o Python, R e C Sharp para o Azure Data Lake Analytics no VS Code](data-lake-analytics-u-sql-develop-with-python-r-csharp-in-vscode.md).

## <a name="work-with-assemblies"></a>Trabalhar com assemblagens

Para obter informações sobre o desenvolvimento das assemblagens, consulte [assemblagens de desenvolver U-SQL para tarefas do Azure Data Lake Analytics](data-lake-analytics-u-sql-develop-assemblies.md).

Pode utilizar as ferramentas do Data Lake para registar as assemblagens de código personalizado no catálogo do Data Lake Analytics.

**Para registar uma assemblagem**

Pode registar a assemblagem através de **ADL: registar a assemblagem** ou **ADL: registar a assemblagem (avançado)** comando.

**Para registar através de ADL: o comando de registar a assemblagem**
1.  Selecione Ctrl + Shift + P para abrir a paleta de comando.
2.  Introduza **ADL: registar a assemblagem**. 
3.  Especifique o caminho da assemblagem local. 
4.  Selecione uma conta de Data Lake Analytics.
5.  Selecione uma base de dados.

O portal é aberto num browser e mostra o processo de registo de assemblagem.  

Uma forma mais conveniente para acionar o **ADL: registar a assemblagem** comando é o ficheiro. dll no Explorador de ficheiros com o botão direito. 

**Para registar através de ADL: o comando de registar a assemblagem (avançado)**
1.  Selecione Ctrl + Shift + P para abrir a paleta de comando.
2.  Introduza **ADL: registar a assemblagem (avançada)**. 
3.  Especifique o caminho da assemblagem local. 
4.  O ficheiro JSON é apresentado. Rever e editar as dependências de assemblagem e os parâmetros do recurso, se necessário. As instruções apresentadas no **saída** janela. Para prosseguir para o registo de assemblagem, guarde, por exemplo, o ficheiro JSON (Ctrl + S).

    ![Ficheiro JSON com parâmetros de recurso e dependências de assemblagem](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-register-assembly-advance.png)
    
>[!NOTE]
>- Autodetects de ferramentas do Azure Data Lake se a DLL tem quaisquer dependências de assemblagem. As dependências são apresentadas no ficheiro de JSON depois que está a ser detetados. 
>- Pode carregar os DLL recursos (por exemplo,. txt, PNG e. csv) como parte do registo de assemblagem. 

Outra forma para acionar o **ADL: registar a assemblagem (avançado)** comando é o ficheiro. dll no Explorador de ficheiros com o botão direito. 

O código de U-SQL seguinte demonstra como chamar uma assemblagem. No exemplo, o nome da assemblagem é *testar*.


        REFERENCE ASSEMBLY [test];

        @a = 
            EXTRACT 
                Iid int,
            Starts DateTime,
            Region string,
            Query string,
            DwellTime int,
            Results string,
            ClickedUrls string 
            FROM @"Sample/SearchLog.txt" 
            USING Extractors.Tsv();

        @d =
            SELECT DISTINCT Region 
            FROM @a;

        @d1 = 
            PROCESS @d
            PRODUCE 
                Region string,
            Mkt string
            USING new USQLApplication_codebehind.MyProcessor();

        OUTPUT @d1 
            TO @"Sample/SearchLogtest.txt" 
            USING Outputters.Tsv();


## <a name="use-u-sql-local-run-and-local-debug-for-windows-users"></a>Utilizar a execução local do U-SQL e depuração local para utilizadores do Windows
Execução local do U-SQL testa os seus dados locais e valida o script localmente antes do seu código é publicado para o Data Lake Analytics. Pode utilizar a funcionalidade de depuração local para concluir as seguintes tarefas antes do seu código foi submetido para o Data Lake Analytics: 
- Depurar o c# por detrás do código. 
- Percorra o código. 
- Valide o script localmente.

Para obter instruções sobre como executar local e de depuração local, consulte [execução local do U-SQL e de depuração local com o Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md).


## <a name="connect-to-azure"></a>Ligar ao Azure

Antes de poder compilar e executar scripts U-SQL no Data Lake Analytics, tem de ligar a sua conta do Azure.

<b id="sign-in-by-command">Para ligar ao Azure através da utilização de um comando</b>

1.  Selecione Ctrl + Shift + P para abrir a paleta de comando. 
2.  Introduza **ADL: início de sessão**. As informações de início de sessão é apresentado no canto inferior direito.

    ![Introduzir o comando de início de sessão](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login.png)

    ![Notificação sobre o início de sessão e autenticação](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-login-info.png)

3.  Selecione **Copiar & abrir** para abrir o [página Web de início de sessão](https://aka.ms/devicelogin). Cole o código para a caixa e, em seguida, selecione **continuar**.

    ![Página Web de início de sessão](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login-paste-code.png)  
     
4.  Siga as instruções para iniciar sessão a partir da página Web. Quando estiver ligado, o nome da sua conta do Azure é apresentado na barra de estado no canto inferior esquerdo da janela VS Code. 

> [!NOTE] 
>- Ferramentas do Data Lake automaticamente inicia a sessão da próxima vez que se não terminar.
>- Se a sua conta tiver dois fatores ativadas, recomendamos que utilize autenticação de telefone, em vez de utilizar um PIN.


Para terminar sessão, introduza o comando **ADL: fim de sessão**.

**Para ligar ao Azure a partir do Explorador do**

Expanda **AZURE DATALAKE**, selecione **iniciar sessão no Azure**e, em seguida, siga o passo 3 e o passo 4 do [para ligar ao Azure através da utilização de um comando](#sign-in-by-command).

![Seleção de "Iniciar sessão no Azure" no Explorador do](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-sign-in-from-explorer.png )  

Não é possível terminar do Explorador do. Para terminar sessão, consulte o artigo [para ligar ao Azure através da utilização de um comando](#sign-in-by-command).


## <a name="create-an-extraction-script"></a>Criar um script de extração 
Pode criar um script de extração de ficheiros. csv, .tsv e. txt, utilizando o comando **ADL: Criar Script de EXTRAIR** ou a partir do Explorador do Azure Data Lake.

**Para criar um script de extração com um comando**

1. Selecione Ctrl + Shift + P para abrir a paleta de comandos e introduza **ADL: Criar Script de EXTRAIR**.
2. Especifique o caminho completo para um ficheiro de armazenamento do Azure e selecione a tecla Enter.
3. Selecione uma conta.
4. Para um ficheiro. txt, selecione um delimitador, a extrair o ficheiro. 

![Processo de criação de um script de extração](./media/data-lake-analytics-data-lake-tools-for-vscode/create-extract-script-process.png)

O script de extração é gerado com base nas suas entradas. Para um script que não conseguiu detetar colunas, escolha uma das duas opções. Caso contrário, será gerado apenas um script.

![Resultado da criação de um script de extração](./media/data-lake-analytics-data-lake-tools-for-vscode/create-extract-script-result.png)

**Para criar um script de extração do Explorador do**

Outra forma de criar o script de extração é através do menu de contexto (atalho) no ficheiro. txt no armazenamento do Azure Data Lake Store ou Blob do Azure, .tsv ou. csv.

![Comando "Criar Script de EXTRAÇÃO" no menu de atalho](./media/data-lake-analytics-data-lake-tools-for-vscode/create-extract-script-from-context-menu.png)

## <a name="integrate-with-azure-data-lake-analytics-through-a-command"></a>Integrar com o Azure Data Lake Analytics através de um comando

Pode aceder a recursos do Azure Data Lake Analytics às contas de lista, metadados de acesso e ver tarefas de análise. 

**Para listar as contas de Azure Data Lake Analytics na sua subscrição do Azure**

1. Selecione Ctrl + Shift + P para abrir a paleta de comando.
2. Introduza **ADL: lista de contas**. As contas são apresentados no **saída** painel.

**Para aceder aos metadados do Azure Data Lake Analytics**

1.  Selecione Ctrl + Shift + P e, em seguida, introduza **ADL: lista de tabelas**.
2.  Selecione uma das contas de Data Lake Analytics.
3.  Selecione uma das bases de dados do Data Lake Analytics.
4.  Selecione uma dos esquemas. Pode ver a lista de tabelas.

**Para ver tarefas do Azure Data Lake Analytics**
1.  Abra a paleta de comando (Ctrl + Shift + P) e selecione **ADL: tarefas de mostrar**. 
2.  Selecione uma conta local ou de Data Lake Analytics. 
3.  Aguarde que a lista de tarefas a apresentar para a conta.
4.  Selecione uma tarefa da lista de tarefas. Ferramentas do Data Lake abre a vista de tarefa no painel da direita e apresenta algumas informações no resultado do VS Code.

    ![Lista de tarefas](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-show-job.png)

## <a name="integrate-with-azure-data-lake-store-through-a-command"></a>Integrar com o Azure Data Lake Store através de um comando

Pode utilizar comandos relacionados com o Azure Data Lake Store para:
 - [Procure os recursos do Azure Data Lake Store](#list-the-storage-path) 
 - [Pré-visualize o ficheiro de Azure Data Lake Store](#preview-the-storage-file) 
 - [Carregar o ficheiro diretamente para o Azure Data Lake Store no VS Code](#upload-file-or-folder)
 - [Transfira o ficheiro diretamente a partir do Azure Data Lake Store no VS Code](#download-file)

### <a name="list-the-storage-path"></a>Listar o caminho de armazenamento 

**Para listar o caminho de armazenamento através da paleta do comando**

1. O editor de scripts com o botão direito e selecione **ADL: caminho de lista**.
2. Selecione a pasta na lista ou selecione **introduza um caminho** ou **procurar a partir do caminho da raiz**. (Estamos a utilizar **introduza um caminho** como exemplo.) 
3. Selecione a sua conta do Data Lake Analytics.
4. Procurar ou introduzir o caminho da pasta de armazenamento (por exemplo, saída /).  

A paleta de comando lista as informações de caminho com base nas suas entradas.

![Resultados de caminho de armazenamento](./media/data-lake-analytics-data-lake-tools-for-vscode/list-storage-path.png)

Uma forma mais conveniente para listar o caminho relativo é efetuada através do menu de atalho.

**Para listar o caminho de armazenamento através do menu de atalho**

A cadeia de caminho com o botão direito e selecione **lista caminho**.

!["Caminho lista" no menu de atalho](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-right-click-path.png)


### <a name="preview-the-storage-file"></a>Pré-visualize o ficheiro de armazenamento

1. O editor de scripts com o botão direito e selecione **ADL: pré-visualização do ficheiro**.
2. Selecione a sua conta do Data Lake Analytics. 
3. Introduza um caminho de ficheiro de armazenamento do Azure (por exemplo, /output/SearchLog.txt). 

O ficheiro abre-se no VS Code.

![Passos e resultados de pré-visualização do ficheiro de armazenamento](./media/data-lake-analytics-data-lake-tools-for-vscode/preview-storage-file.png)

Outra forma de pré-visualize o ficheiro é através do menu de atalho no caminho completo do ficheiro ou o caminho relativo do ficheiro no editor de scripts. 

### <a name="upload-a-file-or-folder"></a>Carregar um ficheiro ou pasta

1. O editor de scripts com o botão direito e selecione **carregar o ficheiro** ou **carregar pasta**.
2. Escolha um ficheiro ou vários ficheiros, se tiver selecionado **carregar o ficheiro**, ou selecione a pasta de toda, se tiver selecionado **carregar pasta**. Em seguida, selecione **carregar**. 
3. Selecione a pasta de armazenamento na lista ou selecione **introduza um caminho** ou **procurar a partir do caminho da raiz**. (Estamos a utilizar **introduza um caminho** como exemplo.) 
4. Selecione a sua conta do Data Lake Analytics. 
5. Procurar ou introduzir o caminho da pasta de armazenamento (por exemplo, saída /). 
6. Selecione **escolha a pasta atual** para especificar o destino de carregamento.

![Passos e resultados para carregar um ficheiro ou pasta](./media/data-lake-analytics-data-lake-tools-for-vscode/upload-file.png)    

Outra forma para carregar ficheiros para o armazenamento é através do menu de atalho no caminho completo do ficheiro ou o caminho relativo do ficheiro no editor de scripts.

Pode [monitorizar o estado de carregamento](#check-storage-tasks-status).


### <a name="download-a-file"></a>Transferir um ficheiro 
Pode transferir um ficheiro ao utilizar o comando **ADL: Transferir ficheiro** ou **ADL: Transferir o ficheiro (avançado)**.

**Para transferir um ficheiro através de ADL: o comando de transferir ficheiros (avançado)**
1. O editor de scripts com o botão direito e, em seguida, selecione **transferir ficheiros (avançado)**.
2. O VS Code apresenta um ficheiro JSON. Pode introduzir caminhos de ficheiro e transferi vários ficheiros ao mesmo tempo. As instruções apresentadas no **saída** janela. Para continuar a transferir o ficheiro ou ficheiros, guarde, por exemplo, o ficheiro JSON (Ctrl + S).

    ![Ficheiro JSON com caminhos para transferências de ficheiros](./media/data-lake-analytics-data-lake-tools-for-vscode/download-multi-files.png)

O **saída** janela apresenta o estado de transferência de ficheiros.

![Janela de saída com o estado de transferência](./media/data-lake-analytics-data-lake-tools-for-vscode/download-multi-file-result.png)     

Pode [monitorizar o estado de transferência](#check-storage-tasks-status).

**Para transferir um ficheiro através de ADL: o comando de transferir ficheiros**

1. O editor de scripts com o botão direito, selecione **transferir ficheiro**e, em seguida, selecione a pasta de destino a partir de **selecionar pasta** caixa de diálogo.
2. Selecione a pasta na lista ou selecione **introduza um caminho** ou **procurar a partir do caminho da raiz**. (Estamos a utilizar **introduza um caminho** como exemplo.) 
3. Selecione a sua conta do Data Lake Analytics. 
4. Navegue para ou introduza o caminho da pasta de armazenamento (por exemplo, saída /) e, em seguida, escolha um ficheiro para transferir.

![Passos e resultados para transferir um ficheiro](./media/data-lake-analytics-data-lake-tools-for-vscode/download-file.png) 

Outra forma para transferir os ficheiros de armazenamento é através do menu de atalho no caminho completo do ficheiro ou o caminho relativo do ficheiro no editor de scripts.

Pode [monitorizar o estado de transferência](#check-storage-tasks-status).

### <a name="check-storage-tasks-status"></a>Verificar o estado de tarefas de armazenamento
O estado de carregamento e transferência é apresentada na barra de estado. Selecione a barra de estado e, em seguida, o estado é apresentado no **saída** separador.

![Na barra de estado e os detalhes de saída](./media/data-lake-analytics-data-lake-tools-for-vscode/storage-status.png)


## <a name="integrate-with-azure-data-lake-analytics-from-the-explorer"></a>Integrar com o Azure Data Lake Analytics do Explorador do

Depois de iniciar sessão, todas as subscrições para a sua conta do Azure estão listadas no painel esquerdo, em **AZURE DATALAKE**. 

![Explorador do Data Lake](./media/data-lake-analytics-data-lake-tools-for-vscode/datalake-explorer.png)

### <a name="data-lake-analytics-metadata-navigation"></a>Navegação de metadados do Data Lake Analytics

Expanda a sua subscrição do Azure. Sob o **bases de dados do U-SQL** nós, pode procurar através da base de dados do U-SQL e pastas de vista como **esquemas**, **credenciais**, **assemblagens**, **Tabelas**, e **índice**.

### <a name="data-lake-analytics-metadata-entity-management"></a>Gestão de entidades de metadados do Data Lake Analytics

Expanda **bases de dados do U-SQL**. Pode criar uma base de dados, esquema, tabela, tipo de tabela, índice ou estatística ao clicar no nó correspondente e, em seguida, selecionar **Script para criar** no menu de atalho. Na página do script foi aberto, edite o script consoante as suas necessidades. Em seguida, submeter a tarefa ao clicá-lo e selecionando **ADL: Submeter tarefa**. 

Depois de concluir a criação do item, faça duplo clique no nó e, em seguida, selecione **atualizar** para mostrar o item. Também pode eliminar o item ao clicá-lo e, em seguida, selecionar **eliminar**.

![Comando "Para criar o script" no menu de atalho no Explorador do Data Lake](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-explorer-script-create.png)

![Página de script para o novo item](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-explorer-script-create-snippet.png)

### <a name="data-lake-analytics-assembly-registration"></a>Registo de assemblagem do Data Lake Analytics

Pode registar uma assemblagem na base de dados correspondente clicando com o **assemblagens** nós e, em seguida, selecionar **registar a assemblagem**.

![Comando "Registar a assemblagem" no menu de atalho para o nó de assemblagens](./media/data-lake-analytics-data-lake-tools-for-vscode/datalake-explorer-register-assembly.png)

## <a name="integrate-with-azure-data-lake-store-from-the-explorer"></a>Integrar com o Azure Data Lake Store do Explorador do

Navegue até à **arquivo Data Lake**:

- Pode com o botão direito no nó de pasta e, em seguida, utilizar o **atualizar**, **eliminar**, **carregar**, **carregar pasta**, **cópia O caminho relativo**, e **caminho completo de cópia** comandos no menu de atalho.

   ![Comandos de menu de atalho para um nó de pasta no Explorador do Data Lake](./media/data-lake-analytics-data-lake-tools-for-vscode/storage-account-folder-menu.png)

- Pode com o botão direito no nó de ficheiro e, em seguida, utilizar o **pré-visualização**, **transferir**, **eliminar**, **criar Script de EXTRAIR** (disponível apenas para CSV TSV e os ficheiros TXT), **copiar o caminho relativo**, e **caminho completo de cópia** comandos no menu de atalho.

   ![Comandos de menu de atalho para um nó de ficheiro no Explorador do Data Lake](./media/data-lake-analytics-data-lake-tools-for-vscode/storage-account-extract.png)

## <a name="integrate-with-azure-blob-storage-from-the-explorer"></a>Integrar com o Blob storage do Azure do Explorador do

Navegue para o armazenamento de BLOBs:

- Pode com o botão direito no nó do contentor de blob e, em seguida, utilizar o **atualizar**, **eliminar o contentor de Blob**, e **carregar Blob** comandos no menu de atalho.

   ![Comandos de menu de atalho para um nó de contentor do blob no armazenamento de BLOBs](./media/data-lake-analytics-data-lake-tools-for-vscode/blob-storage-blob-container-node.png)

- Pode com o botão direito no nó de pasta e, em seguida, utilizar o **atualizar** e **carregar Blob** comandos no menu de atalho.

   ![Comandos de menu de atalho para um nó de pasta no armazenamento de BLOBs](./media/data-lake-analytics-data-lake-tools-for-vscode/blob-storage-folder-node.png)

- Pode com o botão direito no nó de ficheiro e, em seguida, utilizar o **pré-visualização/editar**, **transferir**, **eliminar**, **criar Script de EXTRAIR** (disponível apenas para ficheiros CSV, TSV e TXT), **copiar o caminho relativo**, e **caminho completo de cópia** comandos no menu de atalho.

    ![Comandos de menu de atalho para um nó de ficheiro no armazenamento de BLOBs](./media/data-lake-analytics-data-lake-tools-for-vscode/create-extract-script-from-context-menu-2.png)

## <a name="open-the-data-lake-explorer-in-the-portal"></a>Abra o Explorador do Data Lake no portal
1. Selecione Ctrl + Shift + P para abrir a paleta de comando.
2. Introduza **Abrir Explorador de armazenamento do Azure Web** ou faça duplo clique de um caminho relativo ou o caminho completo no editor de scripts e, em seguida, selecione **Abrir Explorador de armazenamento do Azure Web**.
3. Selecione uma conta de Data Lake Analytics.

Ferramentas do Data Lake abre-se o caminho de armazenamento do Azure no portal do Azure. Pode localizar o caminho e pré-visualizar o ficheiro web.

## <a name="additional-features"></a>Funcionalidades adicionais

Ferramentas do Data Lake para o VS Code suporta as seguintes funcionalidades:

-   **O IntelliSense autocomplete**: sugestões são apresentados no pop-up windows em torno de itens, como palavras-chave, os métodos e variáveis. Ícones diferentes representam os diferentes tipos de objetos:

    - Tipo de dados de scala
    - tipo de dados complexo
    - UDTs incorporadas
    - Classes e de coleção de .NET
    - Expressões de c#
    - UDFs incorporadas c#, UDOs e UDAAGs 
    - Funções de U-SQL
    - Funções de modos de janela U-SQL
 
    ![Tipos de objeto IntelliSense](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-objects.png)
 
-   **O IntelliSense autocomplete nos metadados do Data Lake Analytics**: ferramentas do Data Lake transfere as informações de metadados do Data Lake Analytics localmente. A funcionalidade de IntelliSense preenche automaticamente objetos dos metadados do Data Lake Analytics. Tais objetos incluem a base de dados, de esquema, tabela, vista, função valorizada por tabela, procedimentos e assemblagens c#.
 
    ![O IntelliSense metadados](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-metastore.png)

-   **Marcador de erro IntelliSense**: sublinhados de indicação do Data Lake Tools erros de edição para U-SQL e c#. 
-   **Destaques de sintaxe**: ferramentas do Data Lake utiliza cores para diferenciar itens como variáveis, palavras-chave, tipos de dados e funções. 

    ![Sintaxe com vários cores](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-syntax-highlights.png)

> [!NOTE]
> Recomendamos que Atualize para o Azure Data Lake Tools para Visual Studio versão 2.3.3000.4 ou posterior. As versões anteriores já não estão disponíveis para transferência e estão agora preteridas.  
   
## <a name="next-steps"></a>Passos Seguintes
- [Desenvolver U-SQL com o Python, o R e o C Sharp para o Azure Data Lake Analytics no VS Code](data-lake-analytics-u-sql-develop-with-python-r-csharp-in-vscode.md)
- [Execução local do U-SQL e de depuração local com o Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md)
- [Tutorial: Introdução ao Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md)
- [Tutorial: Desenvolver scripts U-SQL, utilizando ferramentas do Data Lake para Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
