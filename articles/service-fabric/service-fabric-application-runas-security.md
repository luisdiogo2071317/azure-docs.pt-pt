---
title: Executar um serviço de Azure Service Fabric em sistema e as contas de segurança local | Microsoft Docs
description: Saiba como executar uma aplicação de Service Fabric em sistema e as contas de segurança local.  Crie princípios de segurança e aplicar a política de executar como para executar em segurança os serviços.
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: ''
ms.assetid: 4242a1eb-a237-459b-afbf-1e06cfa72732
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/29/2018
ms.author: mfussell
ms.openlocfilehash: 33ca23834f35e631c6943ec22a88f4fe3dc853e1
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="run-a-service-as-a-local-user-account-or-local-system-account"></a>Executar um serviço como uma conta de utilizador local ou como conta de sistema local
Ao utilizar o Service Fabric do Azure, pode proteger aplicações em execução no cluster em contas de utilizador diferente. Por predefinição, as aplicações de Service Fabric executam sob a conta que executa o processo de Fabric.exe em. Serviço de recursos de infraestrutura também fornece a capacidade para executar aplicações numa conta de utilizador ou sistema local. Tipos de conta de sistema local suportados são **Utilizador_local**, **NetworkService**, **LocalService**, e **LocalSystem**.  Se estiver a executar o Service Fabric num cluster de autónomo do Windows, pode executar um serviço em [contas de domínio do Active Directory](service-fabric-run-service-as-ad-user-or-group.md) ou [contas de serviço geridas de grupo](service-fabric-run-service-as-gmsa.md).

No manifesto da aplicação, é possível definir as contas de utilizador necessárias para executar os serviços ou proteger recursos no **principais** secção. Também pode definir e criar grupos de utilizadores para que um ou mais utilizadores podem ser geridos em conjunto. Isto é útil quando existem vários utilizadores para pontos de entrada de serviço diferente e que necessitam de privilégios comuns que estão disponíveis ao nível do grupo.  Os utilizadores, em seguida, são referenciados numa política RunAs, que é aplicada a um serviço específico ou de todos os serviços na aplicação. 

Por predefinição, é aplicada a política de RunAs para o ponto de entrada principal.  Também pode aplicar uma política de RunAs a para o ponto de entrada de configuração, se precisar de [executar determinadas operações de configuração de privilégio elevado com uma conta do sistema](service-fabric-run-script-at-service-startup.md), ou ambos os principal e dos pontos de entrada de configuração.  

> [!NOTE] 
> Se aplicar uma política de RunAs para um serviço e o manifesto do serviço declara recursos de ponto final com o protocolo HTTP, tem de especificar um **SecurityAccessPolicy**.  Para obter mais informações, consulte [atribuir uma política de acesso de segurança para pontos finais HTTP e HTTPS](service-fabric-assign-policy-to-endpoint.md). 
>

## <a name="run-a-service-as-a-local-user"></a>Executar um serviço como um utilizador local
Pode criar um utilizador local que pode ser utilizado para ajudar a proteger um serviço na aplicação. Quando um **Utilizador_local** tipo de conta é especificada na secção de principais de manifesto da aplicação, Service Fabric cria contas de utilizador local em máquinas em que a aplicação é implementada. Por predefinição, estas contas não têm os mesmos nomes que as especificadas no manifesto da aplicação (por exemplo, *Customer3* no seguinte exemplo de manifesto de aplicação). Em vez disso, são geradas dinamicamente e ter palavras-passe aleatórias.

No **RunAsPolicy** secção para um **ServiceManifestImport**, especifique a conta de utilizador do **principais** secção para executar o pacote do código de serviço.  O exemplo seguinte mostra como criar um utilizador local e aplicar uma política de RunAs para o ponto de entrada principal:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application7Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Parameters>
    <Parameter Name="Web1_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Web1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <RunAsPolicy CodePackageRef="Code" UserRef="Customer3" EntryPointType="Main" />
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>    
    <Service Name="Web1" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="Web1Type" InstanceCount="[Web1_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
  <Principals>
    <Users>
      <User Name="Customer3" />
    </Users>
  </Principals>
</ApplicationManifest>
```

## <a name="create-a-local-user-group"></a>Criar um grupo de utilizadores local
Pode criar grupos de utilizadores e adicionar um ou mais utilizadores ao grupo. Isto é útil se existirem vários utilizadores para pontos de entrada de serviço diferente e que precisam para tem determinados privilégios comuns que estão disponíveis ao nível do grupo. O exemplo de manifesto de aplicação seguinte mostra um grupo local com o nome *LocalAdminGroup* que tem privilégios de administrador. Dois utilizadores, *Customer1* e *Customer2*, são efetuados os membros deste grupo local. No **ServiceManifestImport** secção uma RunAs política é aplicada ao executar o *Stateful1Pkg* pacote do código como *Customer2*.  Outra RunAs política é aplicada ao executar o *Web1Pkg* pacote do código como *Customer1*.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application7Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Parameters>
    <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="3" />
    <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
    <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
    <Parameter Name="Web1_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateful1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <RunAsPolicy CodePackageRef="Code" UserRef="Customer2" EntryPointType="Main"/>
    </Policies>
  </ServiceManifestImport>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Web1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" EntryPointType="Main"/>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <Service Name="Stateful1" ServicePackageActivationMode="ExclusiveProcess">
      <StatefulService ServiceTypeName="Stateful1Type" TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]" MinReplicaSetSize="[Stateful1_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[Stateful1_PartitionCount]" LowKey="-9223372036854775808" HighKey="9223372036854775807" />
      </StatefulService>
    </Service>
    <Service Name="Web1" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="Web1Type" InstanceCount="[Web1_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
  <Principals>
    <Groups>
      <Group Name="LocalAdminGroup">
        <Membership>
          <SystemGroup Name="Administrators" />
        </Membership>
      </Group>
    </Groups>
    <Users>
      <User Name="Customer1">
        <MemberOf>
          <Group NameRef="LocalAdminGroup" />
        </MemberOf>
      </User>
      <User Name="Customer2">
        <MemberOf>
          <Group NameRef="LocalAdminGroup" />
        </MemberOf>
      </User>
    </Users>
  </Principals>
</ApplicationManifest>
```

## <a name="apply-a-default-policy-to-all-service-code-packages"></a>Aplicar uma política predefinida para todos os pacotes de código do serviço
Utilizar o **DefaultRunAsPolicy** secção para especificar uma conta de utilizador predefinido para todo o código de pacotes que não tem um específico **RunAsPolicy** definido. Se a maioria dos pacotes de código que são especificados no manifesto de serviço utilizado por uma aplicação precisa de ser executado sob o mesmo utilizador, a aplicação apenas pode definir uma política de RunAs predefinida com essa conta de utilizador. O exemplo seguinte especifica que, se um pacote do código não tem um **RunAsPolicy** especificado, o pacote do código deve ser executado o **MyDefaultAccount** especificada na secção de principais de utilizador.  Tipos de conta suportada são Utilizador_local, NetworkService, LocalSystem e LocalService.  Se utilizar um serviço ou utilizador local, especifique também o nome da conta e palavra-passe.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application7Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Parameters>
    <Parameter Name="Web1_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Web1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    
  </ServiceManifestImport>
  <DefaultServices>    
    <Service Name="Web1" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="Web1Type" InstanceCount="[Web1_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
  <Principals>
    <Users>
      <User Name="MyDefaultAccount" AccountType="NetworkService" />      
    </Users>
  </Principals>
  <Policies>
    <DefaultRunAsPolicy UserRef="MyDefaultAccount" />
  </Policies>
</ApplicationManifest>
```

## <a name="debug-a-code-package-locally-using-console-redirection"></a>Um pacote do código localmente utilizando o redirecionamento de consola de depuração
Ocasionalmente, é útil para fins de depuração ver o resultado da consola de um serviço em execução. Pode definir uma política de redirecionamento de consola no ponto de entrada no manifesto de serviço, que escreve a saída para um ficheiro. O resultado de ficheiro é escrito na pasta de aplicação chamado **registo** no nó de cluster onde a aplicação é implementada e executar. 

> [!WARNING]
> Nunca utilize a política de redirecionamento de consola numa aplicação que é implementada na produção porque isto pode afetar a aplicação de ativação pós-falha. *Apenas* utilizá-lo para o desenvolvimento local e fins de depuração.  
> 
> 

O seguinte serviço manifesto ativar o redirecionamento de consola com um valor de FileRetentionCount do exemplo mostra:

```xml
<CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ExeHost>
        <Program>VotingWeb.exe</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
        <ConsoleRedirection FileRetentionCount="10"/>
      </ExeHost>
    </EntryPoint>
</CodePackage>

```

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Passos Seguintes
* [Compreender o modelo de aplicação](service-fabric-application-model.md)
* [Especificar recursos num manifesto de serviço](service-fabric-service-manifest-resources.md)
* [Implementar uma aplicação](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
