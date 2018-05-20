---
title: Como utilizar um NuGet personalizado feed nos espaços de programador do Azure | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: johnsta
ms.author: johnsta
ms.date: 05/11/2018
ms.topic: article
description: Utilize um NuGet personalizado feed aceder e utilizar pacotes NuGet num espaço do programador do Azure.
keywords: Docker, Kubernetes, Azure, AKS, serviço de contentor do Azure, contentores
manager: ghogen
ms.openlocfilehash: 3badd15bcfd09c97b43744a20c5df05f4ff57e84
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
#  <a name="use-a-custom-nuget-feed-in-an-azure-dev-space"></a>Utilize um NuGet personalizado do feed num espaço do programador do Azure

Um feed do NuGet fornece uma maneira conveniente para incluir origens de pacote num projeto. Os espaços de programador do Azure tem de ser capazes de aceder este feed por ordem para as dependências para estar corretamente instalada no contentor do Docker.

## <a name="set-up-a-nuget-feed"></a>Configurar um NuGet feed

Para configurar um NuGet feed:
1. Adicionar um [referência do pacote](https://docs.microsoft.com/en-us/nuget/consume-packages/package-references-in-project-files) no `*.csproj` ficheiro sob o `PackageReference` nó.

   ```xml
   <ItemGroup>
       <!-- ... -->
       <PackageReference Include="Contoso.Utility.UsefulStuff" Version="3.6.0" />
       <!-- ... -->
   </ItemGroup>
   ```

2. Criar um [NuGet.Config](https://docs.microsoft.com/en-us/nuget/reference/nuget-config-file) ficheiro na pasta do projeto.
     * Utilize o `packageSources` secção para referenciar o NuGet localização do feed. Importante: O feed NuGet tem de estar acessível publicamente.
     * Utilize o `packageSourceCredentials` secção Configurar as credenciais de nome de utilizador e palavra-passe. 

   ```xml
   <packageSources>
       <add key="Contoso" value="https://contoso.com/packages/" />
   </packageSources>

   <packageSourceCredentials>
       <Contoso>
           <add key="Username" value="user@contoso.com" />
           <add key="ClearTextPassword" value="33f!!lloppa" />
       </Contoso>
   </packageSourceCredentials>
   ```

3. Se estiver a utilizar o controlo de código fonte:
    - Referência `NuGet.Config` no seu `.gitignore` para acidentalmente não consolidar as credenciais para o seu repositório de origem do ficheiro.
    - Abra o `azds.yaml` de ficheiros no seu projeto e localize o `build` secção e inserir o fragmento seguinte para se certificar de que o `NuGet.Config` ficheiro será sincronizado para o Azure para que utilizado durante o processo de compilação da imagem de contentor. (Por predefinição, os espaços de programador do Azure não efetua a sincronização ficheiros que correspondam à `.gitignore` e `.dockerignore` regras.)

        ```yaml
        build:
        useGitIgnore: true
        ignore:
        - “!NuGet.Config”
        ```


## <a name="next-steps"></a>Passos Seguintes

Depois de concluir os passos acima, da próxima vez executa `azds up` (ou clique em `F5` no VSCode ou Visual Studio), irá sincronizar os espaços de programador do Azure a `NuGet.Config` ficheiro para o Azure, que, em seguida, é utilizado pelo `dotnet restore` para instalar pacote dependências no contentor.

