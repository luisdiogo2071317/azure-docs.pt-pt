---
title: 'Azure Cosmos DB: Tutorial de introdução da API SQL com .NET Core | Microsoft Docs'
description: Um tutorial que cria uma base de dados online e uma aplicação de consola em C# com o SDK de .NET Core da API SQL do Azure Cosmos DB.
services: cosmos-db
author: SnehaGunda
manager: kfile
editor: ''
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 03/12/2018
ms.author: sngun
ms.custom: devcenter
ms.openlocfilehash: ca2f855166d9a36f600889059368ebeb313e789c
ms.sourcegitcommit: 542964c196a08b83dd18efe2e0cbfb21a34558aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51636293"
---
# <a name="tutorial-build-a-net-core-app-to-manage-azure-cosmos-db-sql-api-data"></a>Tutorial: Criar uma aplicação .Net Core para gerir os dados da API SQL do Azure Cosmos DB

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [Async Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> 

Este tutorial mostra-lhe como criar uma aplicação .Net Core para criar e consultar dados da API SQL do Azure Cosmos DB. 

Este tutorial abrange as seguintes tarefas:

> [!div class="checklist"]
> * Criar e ligar a uma conta do Azure Cosmos DB
> * Configuração da sua Solução Visual Studio
> * Criação de uma base de dados online
> * Criação de uma coleção
> * Criação de documentos JSON
> * Realizar operações de CRUD nos itens, no contentor e na base de dados

Não tem tempo para criar a aplicação? Não se preocupe! A solução completa está disponível em [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started). Avance para a [secção Obter a solução completa](#GetSolution) para instruções rápidas.

Pretende compilar uma aplicação Xamarin iOS, Android ou Formulários com a API SQL e o SDK de .NET Core? Veja [Compilar aplicações móveis com o Xamarin e o Azure Cosmos DB](mobile-apps-with-xamarin.md).

## <a name="prerequisites"></a>Pré-requisitos

* Uma conta ativa do Azure. Se não tiver uma, pode inscrever-se numa [conta gratuita](https://azure.microsoft.com/free/). 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* Se ainda não tiver o Visual Studio 2017 instalado, pode transferir e utilizar o [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Se estiver a desenvolver uma aplicação da Plataforma Universal do Windows (UWP), deve utilizar o **Visual Studio 2017 com a versão 15.4** ou superior. Confirme que ativa o **desenvolvimento do Azure** durante a configuração do Visual Studio.

    * Se estiver a trabalhar no MacOS ou em Linux, pode desenvolver aplicações .NET Core a partir da linha de comandos ao instalar o [SDK de .NET Core](https://www.microsoft.com/net/core#macos) para uma plataforma à sua escolha. 

    * Se estiver a trabalhar em Windows, pode desenvolver aplicações .NET Core a partir da linha de comandos ao instalar o [SDK de .NET Core](https://www.microsoft.com/net/core#windows). 

    * Pode utilizar o seu próprio editor ou transferir o [Visual Studio Code](https://code.visualstudio.com/), que é gratuito e funciona em Windows, Linux e MacOS. 

## <a name="step-1-create-an-azure-cosmos-db-account"></a>Passo 1: Criar uma conta do Azure Cosmos DB

Vamos criar uma conta do Azure Cosmos DB. Se já tiver uma conta que pretende utilizar, pode avançar diretamente para [Configurar a sua solução do Visual Studio](#SetupVS). Se estiver a utilizar o Emulador do Azure Cosmos DB, siga os passos em [Emulador do Azure Cosmos DB](local-emulator.md) para configurar o emulador e avance para [Configurar a Solução do Visual Studio](#SetupVS).

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupVS"></a>Passo 2: Configurar a sua solução do Visual Studio

1. Abra o **Visual Studio 2017** no seu computador.

2. No menu **Ficheiro**, selecione **Novo**, e, em seguida, escolha **Projeto**.

3. Na caixa de diálogo **Novo Projeto**, selecione **Modelos** / **Visual C#** / **.NET Core**/**Aplicação de Consola (.NET Core)**, atribua ao projeto o nome **DocumentDBGettingStarted** e selecione **OK**.

   ![Captura de ecrã da janela Novo Projeto](./media/sql-api-dotnetcore-get-started/nosql-tutorial-new-project-2.png)

4. No **Explorador de Soluções**, clique com o botão direito do rato em **DocumentDBGettingStarted**.

5. No mesmo menu, selecione **Gerir Pacotes NuGet...**.

   ![Captura de ecrã do Menu com Botão do Lado Direito para o Projeto](./media/sql-api-dotnetcore-get-started/nosql-tutorial-manage-nuget-pacakges.png)

6. No separador **NuGet**, selecione **Procurar** na parte superior da janela e escreva **azure documentdb** na caixa de pesquisa.

7. Nos resultados, localize **Microsoft.Azure.DocumentDB.Core** e selecione **Instalar**.

   O ID de pacote da biblioteca é [Microsoft.Azure.DocumentDB.Core](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core). Se tiver como destino uma versão do .NET Framework (como net461) que não é suportada por este pacote.NET Core NuGet, utilize o [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB) que suporta todas as versões do .NET Framework, desde o .NET Framework 4.5.

8. Quando pedido, aceite as instalações de pacotes NuGet e o contrato de licença.

Ótimo! Agora que a configuração está concluída, vamos começar a escrever alguns códigos. Pode encontrar um projeto de código completo deste tutorial em [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started).

## <a id="Connect"></a>Passo 3: Ligar a uma conta do Azure Cosmos DB

Importe as dependências que são precisas ao adicionar o seguinte código ao início da sua aplicação do C#, no ficheiro Program.cs:

```csharp
using System;

// ADD THIS PART TO YOUR CODE
using System.Linq;
using System.Threading.Tasks;
using System.Net;
using Microsoft.Azure.Documents;
using Microsoft.Azure.Documents.Client;
using Newtonsoft.Json;
```

Agora, adicione estas duas constantes e a sua variável de *Cliente* por baixo do seu *Programa* de classe pública.

```csharp
class Program
{
    // ADD THIS PART TO YOUR CODE
    private const string EndpointUri = "<your endpoint URI>";
    private const string PrimaryKey = "<your key>";
    private DocumentClient client;
```

De seguida, vá para o [portal do Azure](https://portal.azure.com) para obter o seu URI e a chave primária. O URI e a chave primária do Azure Cosmos DB são necessários para que a sua aplicação saiba onde ligar e para que o Azure Cosmos DB confie na ligação da sua aplicação.

No portal do Azure, navegue até à sua conta do Azure Cosmos DB e, em seguida, selecione **Chaves**.

Copie o URI do portal e cole-o em `<your endpoint URI>`, no ficheiro program.cs. Em seguida, copie A CHAVE PRIMÁRIA do portal e cole-a em `<your key>`. Se estiver a utilizar o Emulador do Azure Cosmos DB, utilize `https://localhost:8081` como o ponto final e a chave de autorização definida especificamente de [How to develop using the Azure Cosmos DB Emulator](local-emulator.md) (Como programar com o Emulador do Azure Cosmos DB). Certifique-se de que remove o < and >, mas deixe as aspas no ponto final e na chave.

![Obter chaves do portal do Azure][keys]

Iremos começar a aplicação de introdução pela criação de uma nova instância do **DocumentClient**.

Sob o método **Principal**, adicione esta nova tarefa assíncrona designada **GetStartedDemo**, que irá criar a instância novo **DocumentClient**.

```csharp
static void Main(string[] args)
{
}

// ADD THIS PART TO YOUR CODE
private async Task GetStartedDemo()
{
    this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);
}
```

Adicione o seguinte código para executar a tarefa assíncrona a partir do seu método **Principal**. O método **Principal** captura as exceções e escreve-as na consola.

```csharp
static void Main(string[] args)
{
        // ADD THIS PART TO YOUR CODE
        try
        {
                Program p = new Program();
                p.GetStartedDemo().Wait();
        }
        catch (DocumentClientException de)
        {
                Exception baseException = de.GetBaseException();
                Console.WriteLine("{0} error occurred: {1}, Message: {2}", de.StatusCode, de.Message, baseException.Message);
        }
        catch (Exception e)
        {
                Exception baseException = e.GetBaseException();
                Console.WriteLine("Error: {0}, Message: {1}", e.Message, baseException.Message);
        }
        finally
        {
                Console.WriteLine("End of demo, press any key to exit.");
                Console.ReadKey();
        }
```

Selecione o botão **DocumentDBGettingStarted** para criar e executar a aplicação.

Parabéns! Ligou-se com êxito a uma conta do Azure Cosmos DB. Vejamos agora como trabalhar com recursos do Azure Cosmos DB.  

## <a name="step-4-create-a-database"></a>Passo 4: Criar uma base de dados

Antes de adicionar o código para a criação de uma base de dados, adicione um método de programa auxiliar para escrever na consola.

Copie e cole o método **WriteToConsoleAndPromptToContinue** por baixo do método **GetStartedDemo**.

```csharp
// ADD THIS PART TO YOUR CODE
private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
{
        Console.WriteLine(format, args);
        Console.WriteLine("Press any key to continue ...");
        Console.ReadKey();
}
```

Pode criar a sua [base de dados](databases-containers-items.md#azure-cosmos-databases) do Azure Cosmos DB através do método [CreateDatabaseAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) da classe **DocumentClient**. Uma base de dados é o contentor lógico do armazenamento de documentos JSON particionado em coleções.

Copie e cole o seguinte código no seu método **GetStartedDemo**, por baixo da criação de cliente. Esta ação cria uma base de dados designada *FamilyDB*.

```csharp
private async Task GetStartedDemo()
{
    this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);

    // ADD THIS PART TO YOUR CODE
    await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB_oa" });
```

Selecione o botão **DocumentDBGettingStarted** para executar a aplicação.

Parabéns! Criou uma base de dados do Azure Cosmos DB com êxito.  

## <a id="CreateColl"></a>Passo 5: Criar uma coleção

> [!WARNING]
> O **CreateDocumentCollectionAsync** cria uma nova coleção com débito reservado, o que causa repercussões sobre os preços. Para obter mais detalhes, visite a nossa [página de preços](https://azure.microsoft.com/pricing/details/cosmos-db/).

Uma coleção pode ser criada utilizando o [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) método o **DocumentClient** classe. Uma coleção é um contentor de documentos JSON e a lógica da aplicação associada JavaScript.

Copie e cole o seguinte código no seu método **GetStartedDemo**, por baixo da criação da base de dados. Este código cria uma coleção de documentos com o nome *FamilyCollection_oa*.

```csharp
    this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);

    await this.client.CreateDatabaseIfNotExistsAsync("FamilyDB_oa");

    // ADD THIS PART TO YOUR CODE
    await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("FamilyDB_oa"), new DocumentCollection { Id = "FamilyCollection_oa" });
```

Selecione o botão **DocumentDBGettingStarted** para executar a aplicação.

Parabéns! Criou uma coleção de documentos do Azure Cosmos DB com êxito.  

## <a id="CreateDoc"></a>Passo 6: Criar documentos JSON

Um documento pode ser criado utilizando o [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) método o **DocumentClient** classe. Os documentos são conteúdos JSON (arbitrários) definidos pelo utilizador. Podemos agora inserir um ou mais documentos. Se já tiver dados que pretende armazenar na sua base de dados, pode utilizar a [ferramenta de Migração de Dados](import-data.md) do Azure Cosmos DB.

Em primeiro lugar, irá criar uma classe **Família** que representa objetos armazenados no Azure Cosmos DB. Também irá criar subclasses **Principal**, **Subordinado**, **Animal de estimação** e **Endereço** utilizadas dentro da **Família**. Os documentos têm de ter uma propriedade de **Id** serializado como **id** no JSON. Crie estas classes ao adicionar as seguintes subclasses internas a seguir ao método **GetStartedDemo**.

Copie e cole as classes **Família**, **Principal**, **Subordinado**, **Animal de estimação**, e **Endereço** por baixo do método **WriteToConsoleAndPromptToContinue**.

```csharp
private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
{
    Console.WriteLine(format, args);
    Console.WriteLine("Press any key to continue ...");
    Console.ReadKey();
}

// ADD THIS PART TO YOUR CODE
public class Family
{
    [JsonProperty(PropertyName = "id")]
    public string Id { get; set; }
    public string LastName { get; set; }
    public Parent[] Parents { get; set; }
    public Child[] Children { get; set; }
    public Address Address { get; set; }
    public bool IsRegistered { get; set; }
    public override string ToString()
    {
            return JsonConvert.SerializeObject(this);
    }
}

public class Parent
{
    public string FamilyName { get; set; }
    public string FirstName { get; set; }
}

public class Child
{
    public string FamilyName { get; set; }
    public string FirstName { get; set; }
    public string Gender { get; set; }
    public int Grade { get; set; }
    public Pet[] Pets { get; set; }
}

public class Pet
{
    public string GivenName { get; set; }
}

public class Address
{
    public string State { get; set; }
    public string County { get; set; }
    public string City { get; set; }
}
```

Copie e cole o método **CreateFamilyDocumentIfNotExists** por baixo do seu método **CreateDocumentCollectionIfNotExists**.

```csharp
// ADD THIS PART TO YOUR CODE
private async Task CreateFamilyDocumentIfNotExists(string databaseName, string collectionName, Family family)
{
    try
    {
        await this.client.ReadDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, family.Id));
        this.WriteToConsoleAndPromptToContinue("Found {0}", family.Id);
    }
    catch (DocumentClientException de)
    {
        if (de.StatusCode == HttpStatusCode.NotFound)
        {
            await this.client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), family);
            this.WriteToConsoleAndPromptToContinue("Created Family {0}", family.Id);
        }
        else
        {
            throw;
        }
    }
}
```

E insira dois documentos, ou seja um para a Família Andersen e outro para a Família Wakefield.

Copie e cole o código a seguir a `// ADD THIS PART TO YOUR CODE` no método **GetStartedDemo**, por baixo da criação da coleção de documentos.

```csharp
await this.CreateDatabaseIfNotExistsAsync("FamilyDB_oa");

await this.CreateDocumentCollectionIfNotExists("FamilyDB_oa", "FamilyCollection_oa");

// ADD THIS PART TO YOUR CODE
Family andersenFamily = new Family
{
        Id = "Andersen.1",
        LastName = "Andersen",
        Parents = new Parent[]
        {
                new Parent { FirstName = "Thomas" },
                new Parent { FirstName = "Mary Kay" }
        },
        Children = new Child[]
        {
                new Child
                {
                        FirstName = "Henriette Thaulow",
                        Gender = "female",
                        Grade = 5,
                        Pets = new Pet[]
                        {
                                new Pet { GivenName = "Fluffy" }
                        }
                }
        },
        Address = new Address { State = "WA", County = "King", City = "Seattle" },
        IsRegistered = true
};

await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", andersenFamily);

Family wakefieldFamily = new Family
{
        Id = "Wakefield.7",
        LastName = "Wakefield",
        Parents = new Parent[]
        {
                new Parent { FamilyName = "Wakefield", FirstName = "Robin" },
                new Parent { FamilyName = "Miller", FirstName = "Ben" }
        },
        Children = new Child[]
        {
                new Child
                {
                        FamilyName = "Merriam",
                        FirstName = "Jesse",
                        Gender = "female",
                        Grade = 8,
                        Pets = new Pet[]
                        {
                                new Pet { GivenName = "Goofy" },
                                new Pet { GivenName = "Shadow" }
                        }
                },
                new Child
                {
                        FamilyName = "Miller",
                        FirstName = "Lisa",
                        Gender = "female",
                        Grade = 1
                }
        },
        Address = new Address { State = "NY", County = "Manhattan", City = "NY" },
        IsRegistered = false
};

await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", wakefieldFamily);
```

Selecione o botão **DocumentDBGettingStarted** para executar a aplicação.

Parabéns! Criou dois documentos do Azure Cosmos DB com êxito.  

![Relação hierárquica entre a conta, a base de dados online, a coleção](./media/sql-api-dotnetcore-get-started/nosql-tutorial-account-database.png)

## <a id="Query"></a>Passo 7: Consultar recursos do Azure Cosmos DB

O Azure Cosmos DB suporta [consultas](sql-api-sql-query.md) extensas de documentos JSON armazenados em cada coleção. O seguinte código de exemplo mostra várias consultas - utilizando ambas as sintaxes Azure Cosmos DB SQL e LINQ - que podemos procurar nos documentos que inserimos no passo anterior.

Copie e cole o método **ExecuteSimpleQuery** por baixo do seu método **CreateFamilyDocumentIfNotExists**.

```csharp
// ADD THIS PART TO YOUR CODE
private void ExecuteSimpleQuery(string databaseName, string collectionName)
{
    // Set some common query options
    FeedOptions queryOptions = new FeedOptions { MaxItemCount = -1 };

        // Here we find the Andersen family via its LastName
        IQueryable<Family> familyQuery = this.client.CreateDocumentQuery<Family>(
                UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), queryOptions)
                .Where(f => f.LastName == "Andersen");

        // The query is executed synchronously here, but can also be executed asynchronously via the IDocumentQuery<T> interface
        Console.WriteLine("Running LINQ query...");
        foreach (Family family in familyQuery)
        {
            Console.WriteLine("\tRead {0}", family);
        }

        // Now execute the same query via direct SQL
        IQueryable<Family> familyQueryInSql = this.client.CreateDocumentQuery<Family>(
                UriFactory.CreateDocumentCollectionUri(databaseName, collectionName),
                "SELECT * FROM Family WHERE Family.LastName = 'Andersen'",
                queryOptions);

        Console.WriteLine("Running direct SQL query...");
        foreach (Family family in familyQueryInSql)
        {
                Console.WriteLine("\tRead {0}", family);
        }

        Console.WriteLine("Press any key to continue ...");
        Console.ReadKey();
}
```

Copie e cole o seguinte código no seu método **GetStartedDemo**, por baixo da criação do segundo documento.

```csharp
await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", wakefieldFamily);

// ADD THIS PART TO YOUR CODE
this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");
```

Selecione o botão **DocumentDBGettingStarted** para executar a aplicação.

Parabéns! Consultou com êxito uma coleção do Azure Cosmos DB.

O seguinte diagrama ilustra como a sintaxe de consulta do Azure Cosmos DB SQL é chamada na coleção que criou, sendo a mesma lógica aplicada à consulta LINQ.

![Diagrama que ilustra o âmbito e o significado da consulta utilizada pelo tutorial NoSQL para criar uma aplicação de consola C#](./media/sql-api-dotnetcore-get-started/nosql-tutorial-collection-documents.png)

A palavra-chave [FROM](sql-api-sql-query.md#FromClause) é opcional na consulta porque as consultas do Azure Cosmos DB já estão confinadas a uma única coleção. Por conseguinte, "FROM Families f" pode ser trocada por "FROM root r" ou qualquer outra variável de nome escolher. Por predefinição, o Azure Cosmos DB infere as Famílias, raiz ou o nome da variável que escolheu, referencia a atual coleção.

## <a id="ReplaceDocument"></a>Passo 8: Substituir um documento JSON

O Azure Cosmos DB suporta a substituição de documentos JSON.  

Copie e cole o método **ReplaceFamilyDocument** por baixo do seu método **ExecuteSimpleQuery**.

```csharp
// ADD THIS PART TO YOUR CODE
private async Task ReplaceFamilyDocument(string databaseName, string collectionName, string familyName, Family updatedFamily)
{
    await this.client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, familyName), updatedFamily);
    this.WriteToConsoleAndPromptToContinue("Replaced Family {0}", familyName);
}
```

Copie e cole o seguinte código no seu método **GetStartedDemo**, por baixo da execução da consulta. Depois de substituir o documento, este irá executar a mesma consulta par ver o documento alterado.

```csharp
await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", wakefieldFamily);

this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

// ADD THIS PART TO YOUR CODE
// Update the Grade of the Andersen Family child
andersenFamily.Children[0].Grade = 6;

await this.ReplaceFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1", andersenFamily);

this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");
```

Selecione o botão **DocumentDBGettingStarted** para executar a aplicação.

Parabéns! Substituiu documentos do Azure Cosmos DB com êxito.

## <a id="DeleteDocument"></a>Passo 9: Eliminar um documento JSON

O Azure Cosmos DB suporta a eliminação de documentos JSON.  

Copie e cole o método **DeleteFamilyDocument** por baixo do seu método **ReplaceFamilyDocument**.

```csharp
// ADD THIS PART TO YOUR CODE
private async Task DeleteFamilyDocument(string databaseName, string collectionName, string documentName)
{
    await this.client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, documentName));
    Console.WriteLine("Deleted Family {0}", documentName);
}
```

Copie e cole o seguinte código no seu método **GetStartedDemo**, por baixo da segunda execução de consulta.

```csharp
await this.ReplaceFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1", andersenFamily);

this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

// ADD THIS PART TO CODE
await this.DeleteFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1");
```

Selecione o botão **DocumentDBGettingStarted** para executar a aplicação.

Parabéns! Eliminou documentos do Azure Cosmos DB com êxito.

## <a id="DeleteDatabase"></a>Passo 10: Eliminar a base de dados

Eliminar a base de dados criada irá remover a base de dados e todos os recursos subordinados (coleções, documentos, etc.).

Copie e cole o seguinte código no seu método **GetStartedDemo**, por baixo do documento a eliminar para eliminar a base de dados e todos os recursos subordinados.

```csharp
this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

await this.DeleteFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1");

// ADD THIS PART TO CODE
// Clean up/delete the database
await this.client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri("FamilyDB_oa"));
```

Selecione o botão **DocumentDBGettingStarted** para executar a aplicação.

Parabéns! Eliminou bases de dados do Azure Cosmos DB com êxito.

## <a id="Run"></a>Passo 11: Execute a sua aplicação de consola C#

Selecione o botão **DocumentDBGettingStarted** no Visual Studio para criar a aplicação no modo de depuração.

Deverá ver o resultado da sua aplicação Introdução na janela da consola. O resultado apresentará os resultados das consultas que adicionámos e deverá corresponder ao texto de exemplo abaixo.

```
Created FamilyDB_oa
Press any key to continue ...
Created FamilyCollection_oa
Press any key to continue ...
Created Family Andersen.1
Press any key to continue ...
Created Family Wakefield.7
Press any key to continue ...
Running LINQ query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Running direct SQL query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Replaced Family Andersen.1
Press any key to continue ...
Running LINQ query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Running direct SQL query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Deleted Family Andersen.1
End of demo, press any key to exit.
```

Parabéns! Concluiu este tutorial e a sua aplicação de consola C# está a funcionar!

## <a id="GetSolution"></a>Obter a solução completa do tutorial

Para criar a solução GetStarted que contém todos os exemplos deste artigo, deverá ter o seguinte:

* Uma conta ativa do Azure. Se não tiver uma, pode inscrever-se numa [conta gratuita](https://azure.microsoft.com/free/).
* Uma [conta do Azure Cosmos DB][create-sql-api-dotnet.md#create-account].
* A solução [GetStarted](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started) está disponível no GitHub.

Para restaurar as referências para a API SQL do SDK .NET do Azure Cosmos DB no Visual Studio, clique com o botão direito do rato na solução **GetStarted** no Explorador de Soluções e, em seguida, selecione **Ativar Restauro do Pacote NuGet**. Em seguida, no ficheiro Program.cs, atualize os valores EndpointUrl e AuthorizationKey, conforme descrito em [Ligar a uma conta do Azure Cosmos DB](#Connect).

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, aprendeu a criar uma aplicação .Net Core para gerir dados da API SQL do Azure Cosmos DB. Pode agora avançar para o artigo seguinte:

> [!div class="nextstepaction"]
> [Criar uma aplicação de consola Java com a conta da API SQL do Azure Cosmos DB](sql-api-java-get-started.md)

[create-sql-api-dotnet.md#create-account]: create-sql-api-dotnet.md#create-account
[keys]: media/sql-api-dotnetcore-get-started/nosql-tutorial-keys.png
