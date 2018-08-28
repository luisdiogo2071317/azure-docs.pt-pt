---
title: Tutorial - criar, depurar e implementar uma aplicação Web com vários serviços no Service Fabric Mesh | Microsoft Docs
description: Neste tutorial, vai criar uma aplicação do Azure Service Fabric Mesh com vários serviços, que consiste num site ASP.NET Core que comunica com um serviço Web de back-end, depurá-lo localmente e publicá-lo no Azure.
services: service-fabric-mesh
documentationcenter: .net
author: TylerMSFT
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/17/2018
ms.author: twhitney
ms.custom: mvc, devcenter
ms.openlocfilehash: 59ff3434e7b984f4530ad4f8b03b27991d3a9c1c
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/17/2018
ms.locfileid: "41920896"
---
# <a name="tutorial-create-debug-and-deploy-a-multi-service-web-application-to-service-fabric-mesh"></a>Tutorial: criar, depurar e implementar uma aplicação Web com vários serviços no Service Fabric Mesh

Este tutorial é a primeira parte de uma série. Ficará a saber como criar uma aplicação do Azure Service Fabric Mesh com um serviço de front-end Web ASP.NET e um serviço de back-end de API Web ASP.NET Core. Em seguida, vai depurar a aplicação no seu cluster de desenvolvimento local e publicá-la no Azure. Quando tiver terminado, terá uma aplicação de tarefas simples que demonstra uma chamada serviço a serviço numa aplicação do Service Fabric Mesh em execução no Azure Service Fabric Mesh.

Se não quiser criar manualmente a aplicação de tarefas, pode [transferir o código de origem](https://github.com/azure-samples/service-fabric-mesh) da aplicação concluída e avançar diretamente para o [Depurar a aplicação localmente](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md).

Na primeira parte da série, saiba como:

> [!div class="checklist"]
> * Criar uma aplicação do Service Fabric Mesh que consiste num front-end Web ASP.NET.
> * Criar um modelo para representar os itens a fazer.
> * Criar um serviço de back-end e obter dados do mesmo.
> * Adicionar um controlador e o DataContext como parte desse padrão Model View Controller para o serviço de back-end.
> * Criar uma página Web para apresentar os itens a fazer.
> * Criar variáveis de ambiente que identificam o serviço de back-end

Nesta série de tutoriais, ficará a saber como:
> [!div class="checklist"]
> * Criar uma aplicação do Service Fabric Mesh
> * [Depurar a aplicação localmente](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md)
> * [Publicar a aplicação no Azure](service-fabric-mesh-tutorial-deploy-service-fabric-mesh-app.md)

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar este tutorial:

* Se não tiver uma subscrição do Azure, pode [criar uma conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

* Certifique-se de que [configurou o ambiente de desenvolvimento](service-fabric-mesh-howto-setup-developer-environment-sdk.md), que inclui a instalação do runtime do Service Fabric, o SDK, o Docker e o Visual Studio 2017.

* A aplicação para este tutorial tem de ser, por agora, criada com o idioma inglês.

## <a name="create-a-service-fabric-mesh-project"></a>Criar um projeto do Service Fabric Mesh

Execute o Visual Studio e selecione **Ficheiro** > **Novo** > **Projeto...**

Na caixa de diálogo **Novo Projeto**, na parte superior da caixa **Pesquisar**, escreva `mesh`. Selecione o modelo **Aplicação do Service Fabric Mesh**. (Se não vir o modelo, certifique-se de que instalou o SDK do Mesh e a pré-visualização das ferramentas do VS, conforme descrito em [configurar o ambiente de desenvolvimento](service-fabric-mesh-howto-setup-developer-environment-sdk.md).  

Na caixa **Nome**, escreva `todolistapp` e, na caixa **Localização**, defina o caminho da pasta onde pretende armazenar os ficheiros para o projeto.

Certifique-se de que **Criar diretório para a solução** está selecionado e clique em **OK** para criar o projeto do Service Fabric Mesh.

![Caixa de diálogo de novo projeto do Service Fabric Mesh do Visual Studio](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-new-project.png)

Em seguida, verá a caixa de diálogo **Novo Serviço do Service Fabric**.

### <a name="create-the-web-front-end-service"></a>Criar o serviço de front-end da Web

Na caixa de diálogo **Novo Serviço do Service Fabric**, selecione o tipo de projeto **ASP.NET Core** e certifique-se de que o **SO do Contentor** está definido como **Windows**.

Defina o **Nome do Serviço** como **WebFrontEnd**. Prima **OK** para criar o serviço ASP.NET Core.

![Caixa de diálogo de novo projeto do Service Fabric Mesh do Visual Studio](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-new-service-fabric-service.png)

Em seguida, verá a caixa de diálogo **Nova Aplicação Web do ASP.NET Core**. Na caixa de diálogo **Nova Aplicação Web do ASP.NET Core**, selecione **Aplicação Web** e, em seguida, clique em **OK**.

![Nova aplicação ASP.NET Core do Visual Studio](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-new-aspnetcore-app.png)

Tem agora uma aplicação do Service Fabric Mesh. Em seguida, crie o modelo para as informações das tarefas.

## <a name="create-the-to-do-items-model"></a>Criar o modelo de itens a fazer

Para simplificar, os itens a fazer são armazenados numa lista na memória. Crie uma biblioteca de classes para os itens a fazer e uma lista para armazená-los. No Visual Studio, que tem atualmente o **todolistapp** carregado, selecione **Ficheiro** > **Adicionar** > **Novo Projeto**.

Na caixa de diálogo **Novo Projeto**, na parte superior da caixa **Pesquisar**, escreva `C# .net core class`. Selecione o modelo **Biblioteca de Classes (.NET Core)**.

Na caixa **Nome**, escreva `Model`. Clique em **OK** para criar a biblioteca de classes.

No Explorador de Soluções, em **Modelo**, clique com o botão direito do rato em **Class1.cs** e escolha **Mudar o nome**. Mude o nome da classe para **ToDoItem.cs**. Se aparecer um aviso a perguntar se quer mudar o nome de todas as referências, clique em **Sim**.

Substitua os conteúdos do `class ToDoItem` vazio por:

```csharp
public class ToDoItem
{
    public string Description { get; set; }
    public int Index { get; set; }
    public bool Completed { get; set; }

    public ToDoItem(string description)
    {
        Description = description;
        Index = 0;
    }

    public static ToDoItem Load(string description, int index, bool completed)
    {
        ToDoItem newItem = new ToDoItem(description)
        {
            Index = index,
            Completed = completed
        };

        return newItem;
    }
}
```

Esta classe representa os itens a fazer individuais.

No Visual Studio, clique com o botão direito do rato na biblioteca de classes **Modelo** e selecione **Adicionar** > **Classe...** , de modo a criar uma lista para conter os itens a fazer. A caixa de diálogo **Adicionar Novo Item** será apresentada. Defina o **Nome** como `ToDoList.cs` e clique em **Adicionar**.

Em **ToDoList.cs**, substitua o `class ToDoList` vazio por:

```csharp
public class ToDoList
{
    private List<ToDoItem> _items;

    public string Name { get; set; }
    public IEnumerable<ToDoItem> Items { get => _items; }

    public ToDoList(string name)
    {
        Name = name;
        _items = new List<ToDoItem>();
    }

    public ToDoItem Add(string description)
    {
        var item = new ToDoItem(description);
        _items.Add(item);
        item.Index = _items.IndexOf(item);
        return item;
    }
    public void Add(ToDoItem item)
    {
        _items.Add(item);
        item.Index = _items.Count - 1;
    }

    public ToDoItem RemoveAt(int index)
    {
        if (index >= 0 && index < _items.Count)
        {
            var result = _items[index];
            _items.RemoveAt(index);

            // Reorder items
            for (int i = index; i < _items.Count; i++)
            {
                _items[i].Index = i;
            }

            return result;
        }
        else
        {
            throw new IndexOutOfRangeException();
        }
    }
}
```

Em seguida, crie o serviço do Service Fabric que controlará os itens a fazer.

## <a name="create-the-back-end-service"></a>Criar o serviço de back-end

Na janela **Explorador de Soluções** do Visual Studio, clique com o botão direito do rato em **todolistapp** e clique em **Adicionar** > **Novo Serviço do Service Fabric...**

A caixa de diálogo **Novo Serviço do Service Fabric** é apresentada. Selecione o tipo de projeto **ASP.NET Core** e certifique-se de que o **SO do Contentor** está definido como **Windows**.

Defina o **Nome do Serviço** como **ToDoService**. Clique em **OK** para criar o serviço ASP.NET Core. Em seguida, a caixa de diálogo **Nova Aplicação Web do ASP.NET Core** será apresentada. Nessa caixa de diálogo, selecione **API** e, em seguida **OK**, e um projeto para o serviço é adicionado à solução.

![Nova aplicação ASP.NET Core do Visual Studio](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-new-webapi.png)

Uma vez que o serviço de back-end não fornece qualquer IU, desative a inicialização do browser quando o serviço é iniciado. No **Explorador de Soluções**, clique com o botão direito do rato em **ToDoService** e selecione **Propriedades**. Na janela de propriedades que é apresentada, selecione o separador **Depurar** no lado esquerdo e desmarque **Iniciar browser**. Prima **Ctrl+S** para guardar a alteração.

Como este serviço mantém as informações de tarefas pendentes, adicione uma referência para a biblioteca de classes Modelo. No Explorador de Soluções, clique com o botão direito do rato em **ToDoService** e, em seguida, selecione **Adicionar** > **Referência...**. A caixa de diálogo **Gestor de Referências** será apresentada.

No **Gestor de Referências**, selecione a caixa de verificação para **Modelo** e clique em **OK**.

### <a name="add-a-data-context"></a>Adicionar um contexto de dados

Em seguida, crie um contexto de dados que coordena a disponibilização dos dados do modelo de dados.

Para adicionar a classe de contexto de dados, no Explorador de Soluções, clique com o botão direito do rato em **ToDoService** e, em seguida **Adicionar** > **Classe**.
Na caixa de diálogo **Adicionar Novo Item** que é apresentada, certifique-se de que **Classe** está selecionado e defina o **Nome** para `DataContext` e clique em **Adicionar**.

Em **DataContext.cs**, substitua os conteúdos do `class DataContext` vazio por:

```csharp
public static class DataContext
{
    public static Model.ToDoList ToDoList { get; } = new Model.ToDoList("Azure learning List");

    static DataContext()
    {
        ToDoList = new Model.ToDoList("Main List");

        // Seed to-do list

        ToDoList.Add(Model.ToDoItem.Load("Learn about microservices", 0, true));
        ToDoList.Add(Model.ToDoItem.Load("Learn about Service Fabric", 1, true));
        ToDoList.Add(Model.ToDoItem.Load("Learn about Service Fabric Mesh", 2, false));
    }
}
```

Este contexto de dados mínimo preenche alguns itens a fazer e fornece acesso aos mesmos.

### <a name="add-a-controller"></a>Adicionar um controlador

Um controlador predefinido, que processa os pedidos HTPP e cria a resposta HTTP, foi apresentado pelo modelo quando o projeto **ToDoService** foi criado. No **Explorador de Soluções**, em **ToDoService**, abra a pasta **Controladores** para ver o ficheiro **ValuesController.cs**. 

Clique com o botão direito do rato em **ValuesController.cs** e, em seguida, em **Mudar o nome**. Mude o nome do ficheiro para `ToDoController.cs`. Se aparecer um aviso para mudar o nome de todas as referências, clique em **Sim**.

Abra o ficheiro **ToDoController.cs** e substitua os conteúdos de `class ToDoController` por:

```csharp
[Route("api/[controller]")]
public class ToDoController : Controller
{
    // GET api/todo
    [HttpGet]
    public IEnumerable<Model.ToDoItem> Get()
    {
        return DataContext.ToDoList.Items;
    }

    // GET api/todo/5
    [HttpGet("{index}")]
    public Model.ToDoItem Get(int index)
    {
        return DataContext.ToDoList.Items.ElementAt(index);
    }

    //// POST api/values
    //[HttpPost]
    //public void Post([FromBody]string value)
    //{
    //}

    //// PUT api/values/5
    //[HttpPut("{id}")]
    //public void Put(int id, [FromBody]string value)
    //{
    //}

    // DELETE api/values/5
    [HttpDelete("{index}")]
    public void Delete(int index)
    {
    }
}
```

Este tutorial não aborda a adição, eliminação, etc., para manter o foco na comunicação com outro serviço.

## <a name="create-the-web-page-that-displays-to-do-items"></a>Criar a página Web que apresenta os itens a fazer

Com o serviço de back-end implementado, codifique o site que irá apresentar os itens a fazer que fornece. Os seguintes passos ocorrem dentro do projeto **WebFrontEnd**.

A página Web que apresenta os itens a fazer necessita de acesso à classe e à lista **ToDoItem**.
No **Explorador de Soluções**, adicione uma referência para o projeto Modelo, ao clicar com o botão direito do rato em **WebFrontEnd** e selecionar **Adicionar** > **Referência...** A caixa de diálogo **Gestor de Referências** será apresentada.

No **Gestor de Referências**, clique na caixa de verificação para **Modelo** e clique em **OK**.

No **Explorador de Soluções**, abra a página Index.cshtml, ao navegar até **WebFrontEnd** > **Páginas** > **Index.cshtml**. Abra o **Index.cshtml**.

Substitua o conteúdo da totalidade do ficheiro pelo seguinte HTML, que define uma tabela simples para apresentar os itens a fazer:

```HTML
@page
@model IndexModel
@{
    ViewData["Title"] = "Home page";
}

<div>
    <table class="table-bordered">
        <thead>
            <tr>
                <th>Description</th>
                <th>Done?</th>
            </tr>
        </thead>
        <tbody>
            @foreach (var item in Model.Items)
            {
                <tr>
                    <td>@item.Description</td>
                    <td>@item.Completed</td>
                </tr>
            }
        </tbody>
    </table>
</div>
```

Abra o código da página Índice no **Explorador de Soluções**, ao abrir o **Index.cshtml** e, em seguida, abra o **Index.cshtml.cs**.
Na parte superior do **Index.cshtml.cs**, adicionar `using System.Net.Http;`

Substitua os conteúdos de `public class IndexModel` por:

```csharp
public class IndexModel : PageModel
{
    public Model.ToDoItem[] Items = new Model.ToDoItem[] { };

    public void OnGet()
    {
        HttpClient client = new HttpClient();

        using (HttpResponseMessage response = client.GetAsync(backendUrl).GetAwaiter().GetResult())
        {
            if (response.StatusCode == System.Net.HttpStatusCode.OK)
            {
                Items = Newtonsoft.Json.JsonConvert.DeserializeObject<Model.ToDoItem[]>(response.Content.ReadAsStringAsync().Result);
            }
        }
    }

    private static string backendDNSName = $"{Environment.GetEnvironmentVariable("ToDoServiceName")}";
    private static Uri backendUrl = new Uri($"http://{backendDNSName}:{Environment.GetEnvironmentVariable("ApiHostPort")}/api/todo");
}
```

### <a name="create-environment-variables"></a>Criar variáveis de ambiente

O URL para o serviço de back-end é necessário para a comunicação com o serviço. Para efeitos deste tutorial, o seguinte excerto de código (que é definido acima como parte do IndexModel) lê as variáveis de ambiente para compor o URL:

```csharp
private static string backendDNSName = $"{Environment.GetEnvironmentVariable("ToDoServiceName")}";
private static Uri backendUrl = new Uri($"http://{backendDNSName}:{Environment.GetEnvironmentVariable("ApiHostPort")}/api/todo");
```

O URL é composto pelo nome e porta do serviço. Todas estas informações estão no ficheiro service.yaml, que se encontra no projeto **ToDoService**.

Navegue no **Explorador de Soluções** para o projeto **ToDoService** e abra **Recursos do Serviço** > **service.yaml**.

![Figura 1 – o ficheiro service.yaml de ToDoService](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-serviceyaml-port.png)

* O nome do serviço, `ToDoService`, pode ser encontrado em `services:` depois de `name:` Ver (1) na figura acima.
* A porta, `20008`, pode ser encontrada em `endpoints:` depois de `port:` Ver (2) na figura acima. O número de porta do seu projeto pode ser diferente.

Em seguida, as variáveis de ambiente que representam o nome do serviço e o número de porta serão definidas no projeto WebFrontEnd para permitir-lhe chamar o serviço de back-end.

Na **Explorador de Soluções**, navegue até **WebFrontEnd** > **Recursos do Serviço** > **service.yaml** para definir as variáveis que especificam o endereço do serviço de back-end.

No ficheiro service.yaml, adicione as seguintes variáveis em `environmentVariables`. O espaçamento é importante, por isso alinhe as variáveis que adicionar com as outras variáveis em `environmentVariables:`

> [!IMPORTANT]
> Devem ser utilizados espaços, e não separadores, para avançar as variáveis no ficheiro service.yaml; caso contrário, não será compilado. O Visual Studio pode inserir separadores, à medida que cria as variáveis de ambiente. Substitua todos os separadores por espaços. Embora vá ver erros na saída de depuração da **compilação**, a aplicação será iniciada na mesma. No entanto, não funcionará até que converta os separadores em espaços. Para garantir que não existem separadores no ficheiro service.yaml, pode tornar o espaço em branco visível no editor do Visual Studio com **Editar**  > **Avançado**  > **Ver Espaço em Branco**.

O ficheiro **service.yaml** do projeto **WebFrontEnd** deve ser semelhante ao seguinte, embora o seu valor `ApiHostPort` possa ser diferente:

![Service.yaml no projeto WebFrontEnd](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-serviceyaml-envvars.png)

Está agora pronto para criar e implementar a imagem da aplicação do Service Fabric Mesh, juntamente com o serviço Web de back-end, no seu cluster local.

## <a name="next-steps"></a>Passos seguintes

Nesta parte do tutorial, ficou a saber como:

> [!div class="checklist"]
> * Criar uma aplicação do Service Fabric Mesh que consiste num front-end Web ASP.NET.
> * Criar um modelo para representar os itens a fazer.
> * Criar um serviço de back-end e obter dados do mesmo.
> * Adicionar um controlador e o DataContext como parte desse padrão Model View Controller para o serviço de back-end.
> * Criar uma página Web para apresentar os itens a fazer.
> * Criar variáveis de ambiente que identificam o serviço de back-end

Avance para o tutorial seguinte:
> [!div class="nextstepaction"]
> [Depurar uma aplicação do Service Fabric Mesh em execução localmente](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md)
