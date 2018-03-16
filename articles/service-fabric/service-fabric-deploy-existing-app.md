---
title: "Implementar um executável existente para o Azure Service Fabric | Microsoft Docs"
description: "Saiba como uma aplicação existente como convidado executável, o pacote, para que possa ser implementado para um cluster do Service Fabric."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: d799c1c6-75eb-4b8a-9f94-bf4f3dadf4c3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 07/02/2017
ms.author: mfussell;mikhegn
ms.openlocfilehash: 029a0e297469dd5845a82dbdc8fd5f898cfebccc
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="package-and-deploy-an-existing-executable-to-service-fabric"></a>O pacote e implementar um executável existente para o Service Fabric
Quando o empacotamento um executável existente como um [executável convidado](service-fabric-guest-executables-introduction.md), pode escolher a utilizar um modelo de projeto do Visual Studio ou [criar manualmente o pacote de aplicação](#manually). Com o Visual Studio, a estrutura de pacote de aplicação e ficheiros de manifesto são criados pelo modelo de projeto novo.

> [!TIP]
> A forma mais fácil para compactar um executável para um serviço existentes do Windows está a utilizar o Visual Studio e em Linux para utilizar o Yeoman
>

## <a name="use-visual-studio-to-package-and-deploy-an-existing-executable"></a>Utilizar o Visual Studio para o pacote e implementar um executável existente
O Visual Studio fornece um modelo de serviço do Service Fabric para ajudar a implementar um executável de convidado para um cluster do Service Fabric.

1. Escolha **ficheiro** > **novo projeto**e crie uma aplicação de Service Fabric.
2. Escolha **convidado executável** como modelo de serviço.
3. Clique em **procurar** para selecionar a pasta com o executável e preencha o resto dos parâmetros para criar o serviço.
   * *Comportamento de pacote de código*. Pode ser definido para copiar todo o conteúdo da pasta de para o projeto do Visual Studio, que é útil se o ficheiro executável não se altera. Se espera que o executável para alterar e pretende a capacidade de recolher novas compilações dinamicamente, pode optar por ligar para a pasta em vez disso. Pode utilizar as pastas de ligado ao criar o projeto de aplicação no Visual Studio. Esta fornece hiperligações para a localização de origem a partir do projeto, tornando a possível atualizar o convidado executável no respetivo destino de origem. Essas atualizações passam a fazer parte do pacote de aplicação na compilação.
   * *Programa* Especifica o executável que deve ser executado para iniciar o serviço.
   * *Argumentos* Especifica os argumentos que devem ser transmitidos para o executável. Pode ser uma lista de parâmetros com argumentos.
   * *WorkingFolder* Especifica o diretório de trabalho para o processo que vai ser iniciado. Pode especificar três valores:
     * `CodeBase` Especifica que o diretório de trabalho vai ser definido para o diretório de código no pacote de aplicação (`Code` directory apresentados na estrutura de ficheiros anterior).
     * `CodePackage` Especifica que o diretório de trabalho vai ser definido para a raiz do pacote de aplicação (`GuestService1Pkg` mostrada na estrutura de ficheiros anterior).
     * `Work` Especifica que os ficheiros são colocados num subdiretório chamado trabalho.
4. Dê um nome ao serviço e clique em **OK**.
5. Se o serviço precisa de um ponto final para a comunicação, agora pode adicionar o protocolo, porta e tipo para o ficheiro ServiceManifest.xml. Por exemplo: `<Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" UriScheme="http" PathSuffix="myapp/" Type="Input" />`.
6. Agora pode utilizar o pacote e publique ação contra o seu cluster local através da depuração a solução no Visual Studio. Quando estiver pronto, pode publicar a aplicação para um cluster remoto ou verifique a solução ao controlo de origem.
7. Leitura [verificar a sua aplicação em execução](#check-your-running-application) Consulte como visualizar o seu serviço executável do convidado em execução no Service Fabric Explorer.

Para um exemplo de instruções, consulte [criar a primeira aplicação executável de convidados utilizando o Visual Studio](quickstart-guest-app.md).

## <a name="use-yeoman-to-package-and-deploy-an-existing-executable-on-linux"></a>Utilizar Yeoman ao pacote e implementar um executável existente no Linux

O procedimento para criar e implementar um convidado executável no Linux é igual ao implementar uma aplicação csharp ou java.

1. Num terminal, escreva `yo azuresfguest`.
2. Dê um nome à aplicação.
3. Nome do seu serviço e forneça os detalhes, incluindo o caminho do executável e os parâmetros que têm de ser invocado com.

Yeoman cria um pacote de aplicação com a aplicação apropriada e ficheiros de manifesto juntamente com instalar e desinstalar scripts.

<a id="manually"></a>

## <a name="manually-package-and-deploy-an-existing-executable"></a>Manualmente o pacote e implementar um executável existente
O processo de empacotamento manualmente um executável de convidado baseiam-se os seguintes passos gerais:

1. Crie a estrutura de diretórios do pacote.
2. Adicione ficheiros de código e a configuração da aplicação.
3. Edite o ficheiro de manifesto do serviço.
4. Edite o ficheiro de manifesto de aplicação.

<!--
>[AZURE.NOTE] We do provide a packaging tool that allows you to create the ApplicationPackage automatically. The tool is currently in preview. You can download it from [here](http://aka.ms/servicefabricpacktool).
-->

### <a name="create-the-package-directory-structure"></a>Criar a estrutura de diretórios do pacote
Pode começar por criar a estrutura de diretórios, conforme descrito na secção anterior, "Aplicação pacote estrutura de ficheiros."

### <a name="add-the-applications-code-and-configuration-files"></a>Adicionar ficheiros de código e a configuração da aplicação
Depois de ter criado a estrutura de diretórios, pode adicionar código da aplicação e ficheiros de configuração em diretórios de configuração e de código. Também pode criar diretórios adicionais ou subdiretórios os diretórios de configuração ou de código.

Service Fabric um `xcopy` do conteúdo do diretório de raiz da aplicação, pelo que não existe nenhuma estrutura predefinida para utilizar à criação de dois diretórios superiores, o código e definições. (Pode escolher nomes diferentes se pretender. Obter mais detalhes estão na secção seguinte).

> [!NOTE]
> Certifique-se de que inclui todos os ficheiros e dependências que a aplicação necessita. Service Fabric copia o conteúdo do pacote de aplicação em todos os nós no cluster onde vai ser implementado serviços da aplicação. O pacote deve conter o código que a aplicação tem de executar. Não partem do princípio de que as dependências estão instaladas.
>
>

### <a name="edit-the-service-manifest-file"></a>Edite o ficheiro de manifesto do serviço
O passo seguinte consiste em Editar o ficheiro de manifesto do serviço para incluir as seguintes informações:

* O nome do tipo de serviço. Este é um ID de Service Fabric utiliza para identificar um serviço.
* O comando a utilizar para iniciar a aplicação (ExeHost).
* Qualquer script que tem de ser executada para configurar a aplicação (SetupEntrypoint).

Segue-se um exemplo de um `ServiceManifest.xml` ficheiro:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="NodeApp" Version="1.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceTypes>
      <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true"/>
   </ServiceTypes>
   <CodePackage Name="code" Version="1.0.0.0">
      <SetupEntryPoint>
         <ExeHost>
             <Program>scripts\launchConfig.cmd</Program>
         </ExeHost>
      </SetupEntryPoint>
      <EntryPoint>
         <ExeHost>
            <Program>node.exe</Program>
            <Arguments>bin/www</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
         </ExeHost>
      </EntryPoint>
   </CodePackage>
   <Resources>
      <Endpoints>
         <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
   </Resources>
</ServiceManifest>
```

As secções seguintes go sobre as diferentes partes do ficheiro que tem de atualizar.

#### <a name="update-servicetypes"></a>Atualizar ServiceTypes
```xml
<ServiceTypes>
  <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true" />
</ServiceTypes>
```

* Pode escolher qualquer nome que pretende para `ServiceTypeName`. O valor é utilizado o `ApplicationManifest.xml` ficheiro para identificar o serviço.
* Especifique `UseImplicitHost="true"`. Este atributo indica ao Service Fabric que o serviço baseia-se numa aplicação autónomo, pelo que todas as necessidades de Service Fabric fazer é inicie-o como um processo e monitorizar o estado de funcionamento.

#### <a name="update-codepackage"></a>Atualizar o CodePackage
O elemento de CodePackage Especifica a localização (e versão) de código do serviço.

```xml
<CodePackage Name="Code" Version="1.0.0.0">
```

O `Name` elemento é utilizado para especificar o nome do diretório no pacote de aplicação que contém o código do serviço. `CodePackage` Também tem o `version` atributo. Isto pode ser utilizado para especificar a versão do código e também potencialmente pode ser utilizado para atualizar o código do serviço, utilizando a infraestrutura de gestão do ciclo de vida de aplicação no Service Fabric.

#### <a name="optional-update-setupentrypoint"></a>Opcional: SetupEntrypoint de atualização
```xml
<SetupEntryPoint>
   <ExeHost>
       <Program>scripts\launchConfig.cmd</Program>
   </ExeHost>
</SetupEntryPoint>
```
O elemento de SetupEntryPoint é utilizado para especificar qualquer ficheiro executável ou lote que deve ser executado antes do código do serviço é iniciado. Este passo é opcional, pelo que não necessita de ser incluída, se não existir nenhuma necessária inicialização. O SetupEntryPoint é executada sempre que o serviço for reiniciado.

Não há SetupEntryPoint apenas uma, pelo que os scripts de configuração tem de ser agrupados num ficheiro batch único se a configuração da aplicação precisar de vários scripts. O SetupEntryPoint pode executar qualquer tipo de ficheiro: ficheiros executáveis, ficheiros batch e cmdlets do PowerShell. Para obter mais detalhes, consulte [configurar SetupEntryPoint](service-fabric-application-runas-security.md).

No exemplo anterior, o SetupEntryPoint executa um ficheiro batch chamado `LaunchConfig.cmd` que está localizado no `scripts` subdiretório do diretório de código (assumindo que o elemento de WorkingFolder está definido como base de código).

#### <a name="update-entrypoint"></a>Update EntryPoint
```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
  </ExeHost>
</EntryPoint>
```

O `EntryPoint` elemento no ficheiro de manifesto do serviço é utilizado para especificar como iniciar o serviço. O `ExeHost` elemento Especifica o executável (e os argumentos) que deve ser utilizada para iniciar o serviço.

* `Program` Especifica o nome do executável que deve iniciar o serviço.
* `Arguments` Especifica os argumentos que devem ser transmitidos para o executável. Pode ser uma lista de parâmetros com argumentos.
* `WorkingFolder` Especifica o diretório de trabalho para o processo que vai ser iniciado. Pode especificar três valores:
  * `CodeBase` Especifica que o diretório de trabalho vai ser definido para o diretório de código no pacote de aplicação (`Code` diretório na estrutura de ficheiros anterior).
  * `CodePackage` Especifica que o diretório de trabalho vai ser definido para a raiz do pacote de aplicação (`GuestService1Pkg` na estrutura de ficheiros anterior).
    * `Work` Especifica que os ficheiros são colocados num subdiretório chamado trabalho.

O WorkingFolder é útil para definir o diretório de trabalho correto para que podem ser utilizados caminhos relativos pelos scripts de aplicação ou inicialização.

#### <a name="update-endpoints-and-register-with-naming-service-for-communication"></a>Atualizar pontos finais e registar com o serviço de nomenclatura para comunicação
```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
</Endpoints>

```
No exemplo anterior, o `Endpoint` elemento Especifica os pontos finais de que a aplicação pode escutar. Neste exemplo, a aplicação Node.js escuta de http na porta 3000.

Além disso, pode pedir ao Service Fabric para publicar este ponto final para o serviço de nomenclatura para que outros serviços podem detetar o endereço de ponto final para este serviço. Isto permite-lhe ser capaz de comunicar entre os serviços convidados executáveis.
O endereço de ponto final publicados tem o formato `UriScheme://IPAddressOrFQDN:Port/PathSuffix`. `UriScheme` e `PathSuffix` são atributos opcionais. `IPAddressOrFQDN` é o endereço IP ou nome de domínio completamente qualificado do nó este ficheiro executável obtém colocado e esta é calculada para si.

No exemplo seguinte, assim que o serviço é implementado, no Service Fabric Explorer verá um ponto final semelhante `http://10.1.4.92:3000/myapp/` publicados para a instância de serviço. Ou se se tratar de um computador local, consulte `http://localhost:3000/myapp/`.

```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000"  UriScheme="http" PathSuffix="myapp/" Type="Input" />
</Endpoints>
```
Pode utilizar estes endereços com [proxy inverso](service-fabric-reverseproxy.md) para comunicar entre serviços.

### <a name="edit-the-application-manifest-file"></a>Edite o ficheiro de manifesto de aplicação
Assim que tiver configurado o `Servicemanifest.xml` ficheiros, terá de efetuar algumas alterações para o `ApplicationManifest.xml` ficheiro para garantir que o tipo de serviço correta e o nome são utilizadas.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="NodeAppType" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
   </ServiceManifestImport>
</ApplicationManifest>
```

#### <a name="servicemanifestimport"></a>ServiceManifestImport
No `ServiceManifestImport` elemento, pode especificar um ou mais serviços que pretende incluir na aplicação. Os serviços são referenciados com `ServiceManifestName`, que especifica o nome do diretório onde o `ServiceManifest.xml` está localizado o ficheiro.

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
</ServiceManifestImport>
```

## <a name="set-up-logging"></a>Configurar o registo
Para executáveis de convidado, é útil para conseguir ver registos de consola para saber se os scripts de configuração de aplicação e apresentar erros.
Redirecionamento de consola pode ser configurado no `ServiceManifest.xml` ficheiros utilizando o `ConsoleRedirection` elemento.

> [!WARNING]
> Nunca utilize a política de redirecionamento de consola numa aplicação que é implementada na produção porque isto pode afetar a aplicação de ativação pós-falha. *Apenas* utilizá-lo para o desenvolvimento local e fins de depuração.  
>
>

```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="5" FileMaxSizeInKb="2048"/>
  </ExeHost>
</EntryPoint>
```

`ConsoleRedirection` pode ser utilizado para redirecionar o resultado da consola (stdout e stderr) para um diretório de trabalho. Isto fornece a capacidade para verificar se não existirem erros durante a configuração ou a execução da aplicação no cluster de Service Fabric.

`FileRetentionCount` Determina quantos ficheiros são guardados no diretório de trabalho. Um valor de 5, por exemplo, significa que os ficheiros de registo para as execuções de cinco anteriores são armazenados no diretório de trabalho.

`FileMaxSizeInKb` Especifica o tamanho máximo dos ficheiros de registo.

Ficheiros de registo são guardados dos diretórios de trabalho do serviço. Para determinar onde estão localizados os ficheiros, utilize o Service Fabric Explorer para determinar o nó que o serviço está em execução, e o diretório de trabalho está a ser utilizado. Este processo é descrito neste artigo.

## <a name="deployment"></a>Implementação
O último passo consiste em [implementar a sua aplicação](service-fabric-deploy-remove-applications.md). O script do PowerShell seguinte mostra como implementar a sua aplicação para o cluster de desenvolvimento local e inicie um novo serviço de Service Fabric.

```PowerShell

Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath 'C:\Dev\MultipleApplications' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'nodeapp'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'nodeapp'

New-ServiceFabricApplication -ApplicationName 'fabric:/nodeapp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0

New-ServiceFabricService -ApplicationName 'fabric:/nodeapp' -ServiceName 'fabric:/nodeapp/nodeappservice' -ServiceTypeName 'NodeApp' -Stateless -PartitionSchemeSingleton -InstanceCount 1

```

>[!TIP]
> [Comprimir o pacote](service-fabric-package-apps.md#compress-a-package) antes de copiar para o arquivo de imagens, se o pacote é grande ou tem vários ficheiros. Leia mais [aqui](service-fabric-deploy-remove-applications.md#upload-the-application-package).
>

Um serviço do Service Fabric pode ser implementado em várias "configurações". Por exemplo, pode ser implementado como únicas ou várias instâncias, ou podem ser implementada de forma a que não há uma instância do serviço em cada nó do cluster de Service Fabric.

O `InstanceCount` parâmetro o `New-ServiceFabricService` cmdlet é utilizado para especificar quantas instâncias do serviço devem ser iniciadas no cluster de Service Fabric. Pode definir o `InstanceCount` valor, dependendo do tipo de aplicação que estiver a implementar. Os dois cenários mais comuns são:

* `InstanceCount = "1"`. Neste caso, apenas uma instância do serviço é implementada no cluster. Programador de Service Fabric determina nó que o serviço vai ser implementado em.
* `InstanceCount ="-1"`. Neste caso, uma instância do serviço é implementada em cada nó no cluster de Service Fabric. O resultado é ter um (e apenas um) instância do serviço para cada nó no cluster.

Esta é uma configuração útil para aplicações de front-end (por exemplo, um ponto final do REST), porque as aplicações de cliente tem de "ligar" para qualquer um de nós do cluster para utilizar o ponto final. Esta configuração também pode ser utilizada quando, por exemplo, todos os nós do cluster de Service Fabric estão ligados a um balanceador de carga. O tráfego de cliente, em seguida, pode ser distribuído em todo o serviço que está a ser executado em todos os nós do cluster.

## <a name="check-your-running-application"></a>Verifique a sua aplicação em execução
No Service Fabric Explorer, identifique o nó onde o serviço está em execução. Neste exemplo, é executada no Nó1:

![Nó onde o serviço está a ser executado](./media/service-fabric-deploy-existing-app/nodeappinsfx.png)

Se navegar para o nó e navegue para a aplicação, consulte as informações do nó essenciais, incluindo a localização no disco.

![Localização do disco](./media/service-fabric-deploy-existing-app/locationondisk2.png)

Se navegar para o diretório ao utilizar o Explorador de servidores, pode encontrar o diretório de trabalho e a pasta de registo do serviço, conforme mostrado na captura de ecrã seguinte: 

![Localização do registo](./media/service-fabric-deploy-existing-app/loglocation.png)

## <a name="next-steps"></a>Passos Seguintes
Neste artigo, aprendeu como um executável de convidado do pacote e implementá-la para o Service Fabric. Consulte os artigos seguintes para informações relacionadas e tarefas.

* [Exemplo de empacotamento e implementação de um executável de convidado](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started), incluindo uma hiperligação para a pré-lançamento da ferramenta de empacotamento
* [Exemplo de dois convidado executáveis (c# e nodejs) comunicar através do serviço de nomes através de REST](https://github.com/Azure-Samples/service-fabric-containers)
* [Implementar vários executáveis convidados](service-fabric-deploy-multiple-apps.md)
* [Criar a primeira aplicação de Service Fabric com o Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md)
