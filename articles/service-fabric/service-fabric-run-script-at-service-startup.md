---
title: Executar um script quando inicia um serviço de Azure Service Fabric | Microsoft Docs
description: Saiba como configurar uma política para um ponto de entrada de configuração de serviço do Service Fabric e executar um script no início do serviço tempo.
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/21/2018
ms.author: mfussell
ms.openlocfilehash: 3fe22d8bb52fa5f45ce5f1cdc7b860d1ce295a71
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
ms.locfileid: "34210499"
---
# <a name="run-a-service-startup-script-as-a-local-user-or-system-account"></a>Executar um script de arranque do serviço como uma conta de utilizador local ou conta de sistema
Antes de inicia o executável do serviço de Service Fabric poderá ser necessário executar algumas trabalho de configuração ou a configuração.  Por exemplo, a configurar variáveis de ambiente. Pode especificar um script seja executado antes do executável do serviço é iniciado no manifesto de serviço para o serviço. Ao configurar uma política de RunAs para o ponto de entrada de configuração de serviço, pode alterar a conta é executado o programa de configuração executável em.  Um ponto de entrada de configuração individual permite-lhe executar a configuração de alta-privilged durante um curto período de tempo para que o executável do anfitrião do serviço não tem de ser executado com privilégios elevados para períodos de tempo prolongados.

O ponto de entrada de configuração (**SetupEntryPoint** no [manifesto serviço](service-fabric-application-and-service-manifests.md)) é um ponto de entrada com privilégios que, por predefinição, é executada com as mesmas credenciais de Service Fabric (normalmente o  *NetworkService* conta) antes de qualquer outro ponto de entrada. O executável que é especificado por **EntryPoint** é, geralmente, o anfitrião do serviço de execução longa. O **EntryPoint** ficheiro executável é executado o **SetupEntryPoint** executável sai com sucesso. O processo de resultante é monitorizado e reiniciado e começa novamente com **SetupEntryPoint** se alguma vez termina ou falhas. 

## <a name="configure-the-service-setup-entry-point"></a>Configurar o ponto de entrada da configuração do serviço
Segue-se um exemplo de manifesto do serviço simples para um serviço sem monitorização de estado que especifica um script de configuração *MySetup.bat* no serviço **SetupEntryPoint**.  **Argumentos** é utilizado para transmitir argumentos ao script quando é executada.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="MyStatelessServicePkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest.</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyStatelessServiceType" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
        <Arguments>MyValue</Arguments>
        <WorkingFolder>Work</WorkingFolder>        
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyStatelessService.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpoint" />
    </Endpoints>
  </Resources>
</ServiceManifest>
```
## <a name="configure-the-policy-for-a-service-setup-entry-point"></a>Configure a política para um ponto de entrada de configuração de serviço
Por predefinição, o executável de ponto de entrada de configuração de serviço é executado sob as mesmas credenciais de Service Fabric (normalmente o *NetworkService* conta).  No manifesto da aplicação, pode alterar as permissões de segurança para executar o script de arranque com uma conta de sistema local ou uma conta de administrador.

### <a name="configure-the-policy-by-using-a-local-system-account"></a>Configurar a política utilizando uma conta de sistema local
O exemplo de manifesto de aplicação seguinte mostra como configurar o ponto de entrada de configuração de serviço para ser executado com uma conta de utilizador de administrador (SetupAdminUser).

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Parameters>
    <Parameter Name="MyStatelessService_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="MyStatelessServicePkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <RunAsPolicy CodePackageRef="Code" UserRef="SetupAdminUser" EntryPointType="Setup" />
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <Service Name="MyStatelessService" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="MyStatelessServiceType" InstanceCount="[MyStatelessService_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
  <Principals>
    <Users>
      <User Name="SetupAdminUser">
        <MemberOf>
          <SystemGroup Name="Administrators" />
        </MemberOf>
      </User>
    </Users>
  </Principals>
</ApplicationManifest>
```

Em primeiro lugar, crie um **principais** secção com um nome de utilizador, tais como SetupAdminUser. A conta de utilizador SetupAdminUser é um membro do grupo de administradores de sistema.

Em seguida, sob o **ServiceManifestImport** secção, configure uma política para aplicar este principal para **SetupEntryPoint**. Esta política indica ao Service Fabric que, à **MySetup.bat** é executar o ficheiro deve ser executado como SetupAdminUser (com privilégios de administrador). Uma vez que tiver *não* aplicada uma política para o ponto de entrada principal, o código no **MyServiceHost.exe** é executado o sistema **NetworkService** conta. Esta é a conta predefinida que todos os pontos de entrada de serviço são executados como.

### <a name="configure-the-policy-by-using-local-system-accounts"></a>Configurar a política através da utilização de contas do sistema local
Muitas vezes, é preferível executar o script de arranque com uma conta de sistema local em vez de uma conta de administrador. Com a política de RunAs como um membro do grupo Administradores, normalmente, não funciona corretamente porque os computadores que têm o controlo de acesso utilizador (UAC) ativada por predefinição. Nestes casos, a recomendação é run a SetupEntryPoint como LocalSystem, em vez de um utilizador local adicionado ao grupo de administradores. O exemplo seguinte mostra a definição SetupEntryPoint para ser executado como LocalSystem:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Parameters>
    <Parameter Name="MyStatelessService_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="MyStatelessServicePkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupLocalSystem" EntryPointType="Setup" />
      </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <Service Name="MyStatelessService" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="MyStatelessServiceType" InstanceCount="[MyStatelessService_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
  <Principals>
      <Users>
         <User Name="SetupLocalSystem" AccountType="LocalSystem" />
      </Users>
   </Principals>
</ApplicationManifest>
```

> [!NOTE]
> Para os clusters do Linux, para executar um serviço ou a configuração entrada apontar como **raiz**, pode especificar o **AccountType** como **LocalSystem**.

## <a name="run-a-script-from-the-setup-entry-point"></a>Executar um script a partir do ponto de entrada de configuração
Agora, adicione um script de arranque para o projeto para ser executado com privilégios de administrador. 

No Visual Studio, clique com o botão direito no projeto de serviço e adicione um novo ficheiro chamado *MySetup.bat*.

Em seguida, certifique-se de que o *MySetup.bat* ficheiros está incluído no pacote de serviço. Por predefinição, não é. Selecione o ficheiro, faça duplo clique para obter o menu de contexto e escolha **propriedades**. Na caixa de diálogo de propriedades, certifique-se de que **copiar para o diretório de saída** está definido como **copiar se for mais recente**. Veja a captura de ecrã abaixo.

![Visual Studio CopyToOutput para o ficheiro de batch SetupEntryPoint][image1]

Agora editar o *MySetup.bat* de ficheiros e adicionar os seguintes comandos definir uma variável de ambiente de sistema e um ficheiro de texto de saída:

```
REM Set a system environment variable. This requires administrator privilege
setx -m TestVariable %*
echo System TestVariable set to > out.txt
echo %TestVariable% >> out.txt

REM To delete this system variable us
REM REG delete "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /v TestVariable /f
```

Em seguida, criar e implementar a solução para um cluster de desenvolvimento local. Depois do serviço foi iniciado, conforme mostrado no [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md), pode ver que o ficheiro MySetup.bat foi concluída com êxito um duas formas. Abra uma linha de comandos do PowerShell e escreva:

```
PS C:\ [Environment]::GetEnvironmentVariable("TestVariable","Machine")
MyValue
```

Em seguida, tome nota do nome do nó onde o serviço foi implementado e iniciado no [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md). Por exemplo, o nó 2. Em seguida, navegue para a pasta de trabalho de instância de aplicação para localizar o ficheiro de out.txt que mostra o valor de **TestVariable**. Por exemplo, se este serviço foi implementado para o nó 2, em seguida, pode ir para este caminho para o **MyApplicationType**:

```
C:\SfDevCluster\Data\_App\Node.2\MyApplicationType_App\work\out.txt
```

## <a name="run-powershell-commands-from-a-setup-entry-point"></a>Execute comandos do PowerShell a partir de um ponto de entrada de configuração
Para executar o PowerShell do **SetupEntryPoint** ponto, pode executar **PowerShell.exe** num ficheiro batch que aponta para um ficheiro do PowerShell. Primeiro, adicione um ficheiro de PowerShell para o projeto de serviço – por exemplo, **MySetup.ps1**. Não se esqueça de definir o *copiar se for mais recente* propriedade para que o ficheiro também está incluído no pacote de serviço. O exemplo seguinte mostra um ficheiro de batch de exemplo que inicia um ficheiro de PowerShell chamado MySetup.ps1, que define uma variável de ambiente de sistema chamada **TestVariable**.

MySetup.bat para iniciar um ficheiro de PowerShell:

```
powershell.exe -ExecutionPolicy Bypass -Command ".\MySetup.ps1"
```

No ficheiro de PowerShell, adicione o seguinte para definir uma variável de ambiente de sistema:

```
[Environment]::SetEnvironmentVariable("TestVariable", "MyValue", "Machine")
[Environment]::GetEnvironmentVariable("TestVariable","Machine") > out.txt
```

> [!NOTE]
> Por predefinição, quando executa o ficheiro batch, analisa a pasta de aplicação denominada **trabalhar** para ficheiros. Neste caso, quando é executada MySetup.bat, queremos esta opção para encontrar o ficheiro de MySetup.ps1 na mesma pasta, o que é a aplicação **pacote do código** pasta. Para alterar esta pasta, defina a pasta de trabalho:
> 
> 

```xml
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    </ExeHost>
</SetupEntryPoint>
```

## <a name="debug-a-startup-script-locally-using-console-redirection"></a>Um script de arranque localmente utilizando o redirecionamento de consola de depuração
Ocasionalmente, é útil para fins para ver o resultado da execução de um script de configuração consola de depuração. Pode definir uma política de redirecionamento de consola no ponto de entrada de configuração no manifesto de serviço, que escreve a saída para um ficheiro. O resultado de ficheiro é escrito na pasta de aplicação chamado **registo** no nó de cluster onde a aplicação é implementada e executar. 

> [!WARNING]
> Nunca utilize a política de redirecionamento de consola numa aplicação que é implementada na produção porque isto pode afetar a aplicação de ativação pós-falha. *Apenas* utilizá-lo para o desenvolvimento local e fins de depuração.  
> 
> 

O exemplo de manifesto do serviço seguinte mostra a definição o redirecionamento de consola com um valor de FileRetentionCount:

```xml
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="10"/>
    </ExeHost>
</SetupEntryPoint>
```

Se alterar agora o ficheiro de MySetup.ps1 para escrever um **eco** comando, este irá escrever no ficheiro de saída para fins de depuração:

```
Echo "Test console redirection which writes to the application log folder on the node that the application is deployed to"
```

> [!WARNING]
> Depois de depurar o script, imediatamente remova esta política de redirecionamento da consola.



<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Passos Seguintes
* [Saiba mais sobre a aplicação e segurança do serviço](service-fabric-application-and-service-security.md)
* [Compreender o modelo de aplicação](service-fabric-application-model.md)
* [Especificar recursos num manifesto de serviço](service-fabric-service-manifest-resources.md)
* [Implementar uma aplicação](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
