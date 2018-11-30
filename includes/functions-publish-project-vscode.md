

## <a name="publish-the-project-to-azure"></a>Publicar o projeto no Azure

O Visual Studio Code permite-lhe publicar o projeto de funções diretamente no Azure. No processo, vai criar uma aplicação de funções e recursos relacionados na sua subscrição do Azure. A aplicação de funções proporciona um contexto de execução para as suas funções. O projeto é empacotado e implementado na nova aplicação de funções na sua subscrição do Azure. 

Este artigo pressupõe que está a criar uma nova aplicação de funções. A publicação de uma aplicação de funções existente substitui o conteúdo dessa aplicação no Azure.

1. Na área **Azure: Funções**, selecione o ícone Implementar na Aplicação de Funções.

    ![Definições da Aplicação de funções](./media/functions-publish-project-vscode/function-app-publish-project.png)

1. Escolha a pasta do projeto, que é a sua área de trabalho atual.

1. Se tiver mais de uma subscrição, escolha a que pretende alojar a sua aplicação de funções e, em seguida, escolha **+ Criar Nova Aplicação de Funções**.

1. Escreva um nome globalmente exclusivo que identifique a sua aplicação de funções e prima Enter. Os carateres válidos para um nome de aplicação de funções são `a-z`, `0-9` e `-`.

1. Escolha **+ Criar Novo Grupo de Recursos**, escreva um nome de grupo de recursos, como `myResourceGroup`, e prima enter. Também pode utilizar um grupo de recursos existente.

1. Escolher **+ criar nova conta de armazenamento**, tipo de um nome globalmente exclusivo da nova conta de armazenamento utilizado pela sua aplicação de funções e prima Enter. Os nomes das contas do Storage devem ter entre 3 e 24 carateres de comprimento e apenas podem conter números e letras minúsculas. Também pode utilizar uma conta já existente.

1. Escolha uma localização numa [região](https://azure.microsoft.com/regions/) perto de si ou de outros serviços aos quais as suas funções acedem.

    A criação da aplicação de funções é iniciada depois de escolher a sua localização. Depois de criar a aplicação de funções, é apresentada uma notificação e o pacote de implementação é aplicado.

1. Selecione **Ver Resultados** nas notificações, para ver os resultados da criação e da implementação, incluindo os recursos do Azure que criou.

    ![Resultado de criação da aplicação de funções](./media/functions-publish-project-vscode/function-create-notifications.png)

1. Anote o URL da nova aplicação de funções no Azure. Utilize isto para testar a sua função depois de o projeto ser publicado no Azure.

    ![Resultado de criação da aplicação de funções](./media/functions-publish-project-vscode/function-create-output.png)

1. Novamente na área **Azure: Funções**, verá a nova aplicação de funções apresentada na sua subscrição. Ao expandir este nó, verá as funções na aplicação de funções, bem como as definições da aplicação e proxies de funções.

    ![Definições da Aplicação de funções](./media/functions-publish-project-vscode/function-app-project-settings.png)

    No seu nó da aplicação de funções, escreva Ctrl e clique (com o botão direito do rato) para optar por fazer várias tarefas de gestão e configuração relativamente à aplicação de funções no Azure. Também pode optar por ver a aplicação de funções no portal do Azure.
