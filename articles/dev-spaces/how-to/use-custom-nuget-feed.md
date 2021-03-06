---
title: Como utilizar um NuGet personalizado feed nos espaços de desenvolvimento do Azure | Documentos da Microsoft
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.subservice: azds-kubernetes
author: johnsta
ms.author: johnsta
ms.date: 05/11/2018
ms.topic: article
description: Utilize um NuGet personalizado feed para acessar e usar pacotes NuGet num espaço de desenvolvimento do Azure.
keywords: Docker, Kubernetes, Azure, AKS, Azure Container Service, contentores
manager: ghogen
ms.openlocfilehash: 21a70100fe186e176dfe8eb7c247d83a5d4705bd
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55466409"
---
#  <a name="use-a-custom-nuget-feed-in-an-azure-dev-space"></a>Utilizar um NuGet personalizado feed num espaço de desenvolvimento do Azure

Um feed do NuGet fornece uma forma conveniente para incluir origens do pacote num projeto. Espaços de desenvolvimento do Azure tem de ser capaz de aceder a este feed para que as dependências a serem instalados corretamente no contentor do Docker.

## <a name="set-up-a-nuget-feed"></a>Configurar um NuGet de feed

Para configurar um feed de NuGet:
1. Adicionar um [referência do pacote](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files) no `*.csproj` ficheiro sob o `PackageReference` nó.

   ```xml
   <ItemGroup>
       <!-- ... -->
       <PackageReference Include="Contoso.Utility.UsefulStuff" Version="3.6.0" />
       <!-- ... -->
   </ItemGroup>
   ```

2. Criar uma [NuGet.Config](https://docs.microsoft.com/nuget/reference/nuget-config-file) ficheiro na pasta do projeto.
     * Utilize o `packageSources` secção para fazer referência a sua localização do feed de NuGet. Importante: O feed do NuGet tem de ser acessível publicamente.
     * Utilize o `packageSourceCredentials` secção para configurar as credenciais de nome de utilizador e palavra-passe. 

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

3. Se estiver usando o controle de código de origem:
    - Referência `NuGet.Config` no seu `.gitignore` ficheiro, para não consolidar acidentalmente as credenciais para o seu repositório de origem.
    - Abra o `azds.yaml` de ficheiros no seu projeto e localize o `build` secção e inserir o fragmento seguinte para se certificar de que o `NuGet.Config` ficheiro será sincronizado para o Azure, para que ele usado durante o processo de compilação de imagem de contentor. (Por predefinição, espaços de desenvolvimento do Azure não sincroniza os ficheiros que correspondem `.gitignore` e `.dockerignore` regras.)

        ```yaml
        build:
        useGitIgnore: true
        ignore:
        - “!NuGet.Config”
        ```


## <a name="next-steps"></a>Passos Seguintes

Depois de concluir os passos acima, na próxima vez que executar `azds up` (ou clique em `F5` no VSCode ou o Visual Studio), espaços de desenvolvimento do Azure irá sincronizar a `NuGet.Config` ficheiro para o Azure, o que é, em seguida, é utilizado pelo `dotnet restore` para instalar o pacote dependências no contêiner.

