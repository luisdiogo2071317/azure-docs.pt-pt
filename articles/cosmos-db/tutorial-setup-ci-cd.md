---
title: Configurar o pipeline CI/CD com a tarefa de compilação do emulador do Azure Cosmos DB
description: Tutorial sobre como configurar um fluxo de trabalho de compilação e versão no Visual Studio Team Services (VSTS) com a tarefa de compilação do emulador do Cosmos DB
services: cosmos-db
keywords: Emulador do Azure Cosmos DB
author: deborahc
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: tutorial
ms.date: 8/28/2018
ms.author: dech
ms.openlocfilehash: 37bb43435c34f14145b3642aa12c5cb0f16d780c
ms.sourcegitcommit: e2348a7a40dc352677ae0d7e4096540b47704374
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/05/2018
ms.locfileid: "43783879"
---
# <a name="set-up-a-cicd-pipeline-with-the-azure-cosmos-db-emulator-build-task-in-visual-studio-team-services"></a>Configurar um pipeline de CI/CD com a tarefa de compilação do emulador do Azure Cosmos DB no Visual Studio Team Services

O emulador do Azure Cosmos DB fornece um ambiente local que emula o serviço do Azure Cosmos DB para fins de desenvolvimento. O emulador permite-lhe programar e testar a sua aplicação localmente, sem criar uma subscrição do Azure ou incorrer em custos. 

A tarefa de compilação do emulador do Azure Cosmos DB para o Visual Studio Team Services (VSTS) permite-lhe fazer o mesmo num ambiente de CI. Com a tarefa de compilação, pode testar o emulador como parte dos seus fluxos de trabalho de compilação e versão. A tarefa acelera um contentor Docker com o emulador a ser executado e fornece um ponto final que pode ser utilizado pelo resto da definição da compilação. Pode criar e iniciar todas as instâncias do emulador que precisar, com cada uma a ser executada num contentor separado. 

Este artigo demonstra como configurar um pipeline de CI no VSTS para uma aplicação ASP.NET que utiliza a tarefa de compilação do emulador do Azure Cosmos DB para executar testes. 

## <a name="install-the-emulator-build-task"></a>Instalar a tarefa de compilação do emulador

Para utilizar a tarefa de compilação, tem de a instalar primeiro na sua organização do VSTS. Procure a extensão **Emulador do Azure Cosmos DB** no [Microsoft Azure Marketplace](https://marketplace.visualstudio.com/items?itemName=azure-cosmosdb.emulator-public-preview) e clique em **Get it free** (Obter gratuitamente).

![Procure e instale a tarefa de compilação do Emulador do Azure Cosmos DB no Microsoft Azure Marketplace do VSTS](./media/tutorial-setup-ci-cd/addExtension_1.png)

Em seguida, selecione a organização em que pretende instalar a extensão. 

> [!NOTE]
> Para instalar uma extensão numa organização do VSTS, tem de ser proprietário de uma conta ou administrador de uma coleção de projetos. Se não tiver permissões mas for membro de uma conta, terá a opção de pedir extensões. [Saiba mais.](https://docs.microsoft.com/vsts/marketplace/faq-extensions?view=vsts#install-request-assign-and-access-extensions) 

![Selecione uma organização do VSTS para instalar uma extensão](./media/tutorial-setup-ci-cd/addExtension_2.png)

## <a name="create-a-build-definition"></a>Criar uma definição de compilação

Agora que a extensão já foi instalada, tem de adicioná-la a uma [definição de compilação.](https://docs.microsoft.com/vsts/pipelines/get-started-designer?view=vsts&tabs=new-nav) Pode modificar uma definição de compilação existente ou criar uma nova. Se já tem uma definição de compilação existente, pode avançar para [Adicionar a tarefa de compilação do Emulador à definição de compilação](#addEmulatorBuildTaskToBuildDefinition).

Para criar uma nova definição de compilação, navegue até ao separador **Build and Release** (Compilação e Versão) no VSTS. Selecione **+New** (+Novo).

![Criar uma nova definição de compilação](./media/tutorial-setup-ci-cd/CreateNewBuildDef_1.png) Selecione o projeto de equipa, o repositório e o ramo pretendidos para ativar as compilações. 

![Selecione o projeto de equipa, o repositório e o ramo para a definição de compilação ](./media/tutorial-setup-ci-cd/CreateNewBuildDef_2.png)

Por fim, selecione o modelo desejado para a definição de compilação. Neste tutorial, vamos selecionar o modelo **ASP.NET**. 

![Selecionar o modelo de definição de compilação pretendido ](./media/tutorial-setup-ci-cd/CreateNewBuildDef_3.png)

Agora tem uma definição de compilação que pode configurar para utilizar a tarefa de compilação do emulador do Azure Cosmos DB que se assemelha à que é apresentada abaixo. 

![Modelo ASP.NET de definição de compilação](./media/tutorial-setup-ci-cd/CreateNewBuildDef_4.png)

## <a name="addEmulatorBuildTaskToBuildDefinition"></a>Adicionar a tarefa a uma definição de compilação

Para adicionar a tarefa de compilação do emulador, procure **cosmos** na caixa de pesquisa e selecione **Add** (Adicionar). A tarefa de compilação irá iniciar um contentor com uma instância do emulador do Cosmos DB já a ser executada, pelo que a tarefa tem de ser colocada antes de outras tarefas que esperem a execução do emulador.

![Adicionar a tarefa de compilação do Emulador à definição de compilação](./media/tutorial-setup-ci-cd/addExtension_3.png) Neste tutorial, a tarefa é adicionada ao início da Fase 1 para garantir que o emulador está disponível antes da execução dos testes.
A definição de compilação completa tem o seguinte aspeto. 

![Modelo ASP.NET de definição de compilação](./media/tutorial-setup-ci-cd/CreateNewBuildDef_5.png)

## <a name="configure-tests-to-use-the-emulator"></a>Configurar testes para utilizar o emulador
Agora serão configurados os testes para utilizar o emulador. A tarefa de compilação do emulador exporta uma variável de ambiente ("CosmosDbEmulator.Endpoint") à qual todas as tarefas no pipeline de compilação podem emitir pedidos. 

Neste tutorial, é utilizada a [tarefa do Visual Studio Test](https://github.com/Microsoft/vsts-tasks/blob/master/Tasks/VsTestV2/README.md) para executar testes de unidades configurados através de um ficheiro **.runsettings**. Para saber mais sobre a configuração de testes de unidades, aceda à [documentação](https://docs.microsoft.com/visualstudio/test/configure-unit-tests-by-using-a-dot-runsettings-file?view=vs-2017).

Abaixo encontra-se o exemplo de um ficheiro **.runsettings** que define parâmetros a ser passados para os testes de unidades de uma aplicação. Tenha em atenção que a variável `authKey` utilizada é a [chave já conhecida](https://docs.microsoft.com/azure/cosmos-db/local-emulator#authenticating-requests) do emulador. A `authKey` é a chave esperada pela tarefa de compilação do emulador e deve ser definida no seu ficheiro **.runsettings**.

```csharp
<RunSettings>
    <TestRunParameters>
    <Parameter name="endpoint" value="https://localhost:8081" />
    <Parameter name="authKey" value="C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==" />
    <Parameter name="database" value="ToDoListTest" />
    <Parameter name="collection" value="ItemsTest" />
  </TestRunParameters>
</RunSettings>
```
Estes parâmetros `TestRunParameters` são mencionados através de uma propriedade `TestContext` no projeto de teste da aplicação. Eis um exemplo de um teste que é executado no Cosmos DB. 

```csharp
namespace todo.Tests
{
    [TestClass]
    public class TodoUnitTests
    {
        public TestContext TestContext { get; set; }

        [TestInitialize()]
        public void Initialize()
        {
            string endpoint = TestContext.Properties["endpoint"].ToString();
            string authKey = TestContext.Properties["authKey"].ToString();
            Console.WriteLine("Using endpoint: ", endpoint);
            DocumentDBRepository<Item>.Initialize(endpoint, authKey);
        }
        [TestMethod]
        public async Task TestCreateItemsAsync()
        {
            var item = new Item
            {
                Id = "1",
                Name = "testName",
                Description = "testDescription",
                Completed = false,
                Category = "testCategory"
            };

            // Create the item
            await DocumentDBRepository<Item>.CreateItemAsync(item);
            // Query for the item
            var returnedItem = await DocumentDBRepository<Item>.GetItemAsync(item.Id, item.Category);
            // Verify the item returned is correct.
            Assert.AreEqual(item.Id, returnedItem.Id);
            Assert.AreEqual(item.Category, returnedItem.Category);
        }

        [TestCleanup()]
        public void Cleanup()
        {
            DocumentDBRepository<Item>.Teardown();
        }
    }
}
```

Navegue até Execution Options (Opções de Execução) na tarefa do Visual Studio Test. Na opção **Settings file** (ficheiro de Definições), especifique que os testes são configurados com o ficheiro **.runsettings**. Na opção **Override test run parameters** (Substituir parâmetros de execução de testes), adicione ` -endpoint $(CosmosDbEmulator.Endpoint)`. Este procedimento irá configurar a tarefa Test para se referir ao ponto final da tarefa de compilação do emulador, em vez do ponto final definido no ficheiro **.runsettings**.  

![Substituir uma variável de ponto final pelo ponto final da tarefa de compilação do Emulador](./media/tutorial-setup-ci-cd/addExtension_5.png)

## <a name="run-the-build"></a>Executar a compilação
Agora, guarde a compilação e coloque-a em fila. 

![Guardar e executar a compilação](./media/tutorial-setup-ci-cd/runBuild_1.png)

Assim que a compilação iniciar, verá que a tarefa do emulador do Cosmos DB irá começar a puxar para baixo a imagem Docker com o emulador instalado. 

![Guardar e executar a compilação](./media/tutorial-setup-ci-cd/runBuild_4.png)

Após a conclusão da compilação, poderá observar os seus testes a passarem, com todos a serem executados no emulador do Cosmos DB a partir da tarefa de compilação.

![Guardar e executar a compilação](./media/tutorial-setup-ci-cd/buildComplete_1.png)

## <a name="next-steps"></a>Passos seguintes

Para saber mais sobre a utilização do emulador para desenvolvimento e testes locais, veja [Utilizar o Emulador do Azure Cosmos DB para desenvolvimento e teste locais](https://docs.microsoft.com/azure/cosmos-db/local-emulator).

Para exportar certificados SSL do emulador, veja [Exportar os certificados de Emulador do Azure Cosmos DB para utilizar com o Java, o Python e o Node.js](https://docs.microsoft.com/azure/cosmos-db/local-emulator-export-ssl-certificates)
