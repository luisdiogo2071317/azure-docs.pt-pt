---
title: Tutorial - atualizar um aplicativo de malha do Azure Service Fabric | Documentos da Microsoft
description: Saiba como atualizar uma aplicação do Service Fabric com o Visual Studio
services: service-fabric-mesh
documentationcenter: .net
author: tylerMSFT
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: azure-cli
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/18/2018
ms.author: twhitney
ms.custom: mvc, devcenter
ms.openlocfilehash: c23646bca6109d27e57b2f928363e65c83c634eb
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47031157"
---
# <a name="tutorial-learn-how-to-upgrade-a-service-fabric-application-using-visual-studio"></a>Tutorial: Saiba como atualizar uma aplicação do Service Fabric com o Visual Studio

Este tutorial é a parte quatro de uma série e mostra-lhe como atualizar uma aplicação do Azure Service Fabric em malha diretamente a partir do Visual Studio. A atualização irá incluir uma atualização de código e uma configuração de atualização. Verá que os passos para atualizar e publicar a partir de dentro do Visual Studio são os mesmos.

Neste tutorial, ficará a saber como:
> [!div class="checklist"]
> * Atualizar um serviço de malha do Service Fabric com o Visual Studio

Nesta série de tutoriais, ficará a saber como:
> [!div class="checklist"]
> * [Criar uma malha de recursos de infraestrutura do serviço aplicação no Visual Studio](service-fabric-mesh-tutorial-create-dotnetcore.md)
> * [Depurar uma aplicação de malha de recursos de infraestrutura do serviço de mensagens em fila em execução no cluster de desenvolvimento local](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md)
> * [Implementar uma malha de recursos de infraestrutura do serviço aplicação](service-fabric-mesh-tutorial-deploy-service-fabric-mesh-app.md)
> * Atualizar uma malha de recursos de infraestrutura do serviço aplicação
> * [Limpar os recursos de malha do Service Fabric](service-fabric-mesh-tutorial-cleanup-resources.md)

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar este tutorial:

* Se ainda não implementou a aplicação de tarefas pendentes, siga as instruções em [publicar uma aplicação de web Service Fabric em malha](service-fabric-mesh-tutorial-deploy-service-fabric-mesh-app.md).

## <a name="upgrade-a-service-fabric-mesh-service-by-using-visual-studio"></a>Atualizar um serviço de malha do Service Fabric com o Visual Studio

Este artigo mostra como atualizar independentemente um microsserviço dentro de um aplicativo.  Neste exemplo, modificaremos o `WebFrontEnd` serviço para apresentar uma categoria de tarefa. Em seguida, podemos irá atualizar o serviço implementado.

## <a name="modify-the-config"></a>Modificar a configuração

As atualizações podem ser devido a alterações de código, alterações de configuração ou ambos.  Para introduzir uma alteração de configuração, abra a `WebFrontEnd` do projeto `service.yaml` ficheiro (que está sob a **recursos de serviço** nó).

Na `resources:` secção, alterar `cpu:` de 0,5 a 1,0, na expectativa de que o front-end da web será utilizado intensamente. Deve agora ser semelhante isto:

```xml
              ...
              resources:
                requests:
                  cpu: 1.0
                  memoryInGB: 1
              ...
```

## <a name="modify-the-model"></a>Modificar o modelo

Para introduzir uma alteração de código, adicione uma `Category` propriedade para o `ToDoItem` classe no `ToDoItem.cs` ficheiro.

```csharp
public class ToDoItem
{
    public string Category { get; set; }
    ...
}
```

Em seguida, atualize o `Load()` método, no mesmo ficheiro, para definir a categoria para uma cadeia de caracteres padrão:

```csharp
public static ToDoItem Load(string description, int index, bool completed)
{
    ToDoItem newItem = new ToDoItem(description)
    {
        Index = index,
        Completed = completed, 
        Category = "none"    // <-- add this line
    };

    return newItem;
}
```

## <a name="modify-the-service"></a>Modificar o serviço

O `WebFrontEnd` projeto é uma aplicação ASP.NET Core com uma página da web que mostra itens de lista de tarefas pendentes. Na `WebFrontEnd` projeto aberto `Index.cshtml` e adicione as seguintes duas linhas, indicadas abaixo, para apresentar a categoria da tarefa:

```HTML
<div>
    <table class="table-bordered">
        <thead>
            <tr>
                <th>Description</th>
                <th>Category</th>           @*add this line*@
                <th>Done?</th>
            </tr>
        </thead>
        <tbody>
            @foreach (var item in Model.Items)
            {
                <tr>
                    <td>@item.Description</td>
                    <td>@item.Category</td>           @*add this line*@
                    <td>@item.Completed</td>
                </tr>
            }
        </tbody>
    </table>
</div>
```

Crie e execute a aplicação para verificar que vê uma nova coluna de categoria na página da web que lista as tarefas.

## <a name="upgrade-the-app-from-visual-studio"></a>Atualizar a aplicação a partir do Visual Studio

Independentemente se estiver fazendo uma atualização de código, ou uma atualização de configuração (neste caso estamos fazendo ambos), para atualizar a sua aplicação de malha de recursos de infraestrutura do serviço de mensagens em fila no contexto do Azure num **todolistapp** no Visual Studio e selecione **publicar ...**

Em seguida, verá a caixa de diálogo **Publish Service Fabric Application** (Publicar Aplicação do Service Fabric).

![Caixa de diálogo de publicação do Service Fabric Mesh do Visual Studio](./media/service-fabric-mesh-tutorial-deploy-dotnetcore/visual-studio-publish-dialog.png)

Selecione a conta e a subscrição do Azure. Certifique-se de que **localização** está definido para a localização que utilizou quando publicado originalmente a aplicação de tarefas pendentes para o Azure. Este artigo utilizado **E.U.A. Leste**.

Certifique-se de que **grupo de recursos** está definido para o grupo de recursos que utilizou quando publicado originalmente a aplicação de tarefas pendentes para o Azure.

Certifique-se de que **Azure Container Registry** está definido como o nome do registo de contentor do azure que criou quando publicado originalmente a aplicação de tarefas pendentes para o Azure.

Na caixa de diálogo Publicar, prima a **publicar** botão para atualizar a aplicação de tarefas no Azure.

Pode monitorizar o progresso da atualização, selecionando o **ferramentas do Service Fabric** painel no Visual Studio **saída** janela. Assim que a atualização estiver concluída, o **ferramentas do Service Fabric** saída irá apresentar o endereço IP e porta do seu aplicativo na forma de um URL.

```json
Packaging Application...
Building Images...
Web1 -> C:\Code\ServiceFabricMeshApp\ToDoService\bin\Any CPU\Release\netcoreapp2.0\ToDoService.dll
Uploading the images to Azure Container Registy...
Deploying application to remote endpoint...
The application was deployed successfully and it can be accessed at http://10.000.38.000:20000.
```

Abra um browser e navegue para o URL para ver o site em execução no Azure. Agora, deverá ver uma página da web que contém uma coluna de categoria.

## <a name="next-steps"></a>Passos Seguintes

Nesta parte do tutorial, ficou a saber como:
> [!div class="checklist"]
> * Como atualizar uma aplicação de serviço Bagunça de recursos de infraestrutura com o Visual Studio

Avance para o tutorial seguinte:
> [!div class="nextstepaction"]
> [Limpar os recursos de malha do Service Fabric](service-fabric-mesh-tutorial-cleanup-resources.md)